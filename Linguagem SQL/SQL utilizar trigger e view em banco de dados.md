# Como Utilizar trigger e view em banco de dados no sistema de vinação

## VIEW: Resumo de Vacinação

Uma VIEW é uma **tabela virtual** que junta dados de várias tabelas. Ela simplifica consultas e **atualiza automaticamente** quando os dados mudam.

```sql
USE vacinacao_db;

-- VIEW 1: Resumo Completo de Aplicações
CREATE OR REPLACE VIEW vw_resumo_vacinacao AS
SELECT 
    p.nome AS paciente,
    p.cpf,
    pr.nome AS profissional,
    cp.nome_cargo,
    v.nome_vacina,
    a.dose_numero,
    a.data_aplicacao,
    l.numero_lote,
    l.data_validade
FROM Aplicacao_Vacina a
JOIN Paciente p ON a.paciente_id = p.id_paciente
JOIN Profissional pr ON a.profissional_aplicador = pr.id_profissional
JOIN Cargo_Profissional cp ON pr.id_cargo = cp.id_cargo
JOIN Lote_Vacina l ON a.lote_numero = l.numero_lote
JOIN Vacina v ON l.id_vacina = v.id_vacina
ORDER BY a.data_aplicacao DESC;

-- Como usar a VIEW (igual uma tabela normal):
SELECT * FROM vw_resumo_vacinacao LIMIT 10;

-- Agregação DIRETO na VIEW:
SELECT 
    profissional,
    nome_cargo,
    COUNT(*) AS total_aplicacoes,
    AVG(dose_numero) AS dose_media
FROM vw_resumo_vacinacao 
WHERE data_aplicacao >= '2026-05-01'
GROUP BY profissional
ORDER BY total_aplicacoes DESC;
```


## VIEW 2: Cobertura Vacinal

```sql
-- VIEW 2: Cobertura por Vacina
CREATE OR REPLACE VIEW vw_cobertura_vacinal AS
SELECT 
    v.nome_vacina,
    COUNT(DISTINCT a.paciente_id) AS pacientes_vacinados,
    COUNT(a.id_aplicacao) AS total_doses,
    ROUND(100.0 * COUNT(DISTINCT a.paciente_id) / 
          (SELECT COUNT(*) FROM Paciente), 2) AS cobertura_percentual
FROM Aplicacao_Vacina a
JOIN Lote_Vacina l ON a.lote_numero = l.numero_lote
JOIN Vacina v ON l.id_vacina = v.id_vacina
GROUP BY v.id_vacina, v.nome_vacina;

-- Uso:
SELECT * FROM vw_cobertura_vacinal ORDER BY pacientes_vacinados DESC;
```


## TRIGGER: Controle Automático de Estoque

**Trigger** executa **automaticamente** quando você insere/atualiza uma aplicação de vacina, atualizando o estoque do lote.

```sql
DELIMITER //

-- TRIGGER 1: Reduz Estoque ao Aplicar Vacina
CREATE TRIGGER trg_atualiza_estoque_vacina
AFTER INSERT ON Aplicacao_Vacina
FOR EACH ROW
BEGIN
    -- Reduz 1 unidade do lote
    UPDATE Lote_Vacina 
    SET quantidade_disponivel = quantidade_disponivel - 1
    WHERE numero_lote = NEW.lote_numero;
    
    -- Marca lote como esgotado se estoque = 0
    IF (SELECT quantidade_disponivel FROM Lote_Vacina 
        WHERE numero_lote = NEW.lote_numero) = 0 THEN
        UPDATE Lote_Vacina 
        SET status = 'esgotado' 
        WHERE numero_lote = NEW.lote_numero;
    END IF;
END//

-- TRIGGER 2: Impede Uso de Lote Vencido
CREATE TRIGGER trg_impede_lote_vencido
BEFORE INSERT ON Aplicacao_Vacina
FOR EACH ROW
BEGIN
    DECLARE v_data_validade DATE;
    SELECT data_validade INTO v_data_validade
    FROM Lote_Vacina WHERE numero_lote = NEW.lote_numero;
    
    IF v_data_validade < CURDATE() THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'ERRO: Lote vencido! Não é possível aplicar vacina vencida.';
    END IF;
END//

-- TRIGGER 3: Auditoria (Registra TODAS as aplicações)
CREATE TABLE auditoria_vacinacao (
    id_auditoria INT AUTO_INCREMENT PRIMARY KEY,
    acao VARCHAR(10),
    id_aplicacao INT,
    profissional INT,
    paciente INT,
    data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TRIGGER trg_auditoria_aplicacao
AFTER INSERT ON Aplicacao_Vacina
FOR EACH ROW
BEGIN
    INSERT INTO auditoria_vacinacao (acao, id_aplicacao, profissional, paciente)
    VALUES ('INSERT', NEW.id_aplicacao, NEW.profissional_aplicador, NEW.paciente_id);
END//

DELIMITER ;
```


## Demonstração Completa: VIEW + TRIGGER Juntos

**Teste prático** para mostrar aos alunos como funciona:

```sql
-- 1. Verificar estoque inicial
SELECT numero_lote, quantidade_disponivel FROM Lote_Vacina;

-- 2. Aplicar vacina (TRIGGER executa AUTOMATICAMENTE)
INSERT INTO Aplicacao_Vacina (
    data_aplicacao, dose_numero, via_aplicacao, 
    paciente_id, profissional_aplicador, lote_numero
) VALUES (
    '2026-05-08', 1, 'Intramuscular', 
    1, 1, 'LT2026001'
);

-- 3. Verificar: Estoque REDUZIU sozinho!
SELECT numero_lote, quantidade_disponivel FROM Lote_Vacina 
WHERE numero_lote = 'LT2026001';

-- 4. Consultar na VIEW (dados atualizados automaticamente)
SELECT * FROM vw_resumo_vacinacao ORDER BY data_aplicacao DESC LIMIT 3;

-- 5. Ver auditoria (registrada pelo trigger)
SELECT * FROM auditoria_vacinacao ORDER BY data_hora DESC;
```


## VIEW Avançada: Ranking Profissionais

```sql
CREATE OR REPLACE VIEW vw_ranking_profissionais AS
SELECT 
    profissional,
    nome_cargo,
    total_aplicacoes,
    DENSE_RANK() OVER (ORDER BY total_aplicacoes DESC) AS ranking,
    ROUND(100.0 * total_aplicacoes / SUM(total_aplicacoes) OVER(), 2) AS percentual
FROM (
    SELECT 
        pr.nome AS profissional,
        cp.nome_cargo,
        COUNT(*) AS total_aplicacoes
    FROM vw_resumo_vacinacao v
    JOIN Profissional pr ON v.profissional = pr.nome
    JOIN Cargo_Profissional cp ON pr.id_cargo = cp.id_cargo
    GROUP BY pr.id_profissional
) ranking;

-- Uso:
SELECT * FROM vw_ranking_profissionais WHERE ranking <= 3;
```
