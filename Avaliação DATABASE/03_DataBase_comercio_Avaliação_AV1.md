# Avaliação AV1 22/04/2026
## Script Completo do Banco (Execute no MySQL Workbench)

```sql
-- 1. Criar banco de dados
CREATE DATABASE IF NOT EXISTS comercio_db CHARACTER SET utf8mb4;
USE comercio_db;

-- 2. Tabela Departamento
CREATE TABLE departamentos (
    id_departamento INT AUTO_INCREMENT PRIMARY KEY,
    nome_departamento VARCHAR(100) NOT NULL,
    localizacao VARCHAR(100) NOT NULL
);

-- 3. Tabela Funcionários
CREATE TABLE funcionarios (
    id_funcionario INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(100) NOT NULL,
    data_contratacao DATE NOT NULL,
    id_departamento INT NOT NULL,
    FOREIGN KEY (id_departamento) REFERENCES departamentos(id_departamento)
);

-- 4. Tabela Salários
CREATE TABLE salarios (
    id_salario INT AUTO_INCREMENT PRIMARY KEY,
    id_funcionario INT NOT NULL,
    valor_salario DECIMAL(10,2) NOT NULL,
    data_vigencia DATE NOT NULL,
    FOREIGN KEY (id_funcionario) REFERENCES funcionarios(id_funcionario)
);

-- 5. Tabela Vendas
CREATE TABLE vendas (
    id_venda INT AUTO_INCREMENT PRIMARY KEY,
    id_funcionario INT NOT NULL,
    valor_venda DECIMAL(10,2) NOT NULL,
    data_venda DATE NOT NULL,
    cliente VARCHAR(100) NOT NULL,
    FOREIGN KEY (id_funcionario) REFERENCES funcionarios(id_funcionario)
);

-- 6. Inserir DEPARTAMENTOS
INSERT INTO departamentos (nome_departamento, localizacao) VALUES
('Vendas', 'Loja Principal'),
('TI', 'Térreo'),
('RH', '2º Andar'),
('Financeiro', '3º Andar');

-- 7. FUNCIONÁRIOS
INSERT INTO funcionarios (nome, cargo, data_contratacao, id_departamento) VALUES
('Ana Pereira', 'Vendedora', '2024-01-15', 1),
('Bruno Santos', 'Gerente Vendas', '2023-06-10', 1),
('Carla Lima', 'Desenvolvedor', '2024-03-20', 2),
('Diego Costa', 'Analista Financeiro', '2023-09-05', 4);

-- 8. Inserir SALÁRIOS
INSERT INTO salarios (id_funcionario, valor_salario, data_vigencia) VALUES
(1, 2800.00, '2025-01-01'),  -- Ana
(2, 4500.00, '2025-01-01'),  -- Bruno
(3, 3800.00, '2025-01-01'),  -- Carla
(4, 3200.00, '2025-01-01');  -- Diego

-- 9. Inserir VENDAS
INSERT INTO vendas (id_funcionario, valor_venda, data_venda, cliente) VALUES
(1, 1250.50, '2026-04-01', 'João Silva'),
(1, 890.00, '2026-04-05', 'Maria Santos'),
(2, 2450.75, '2026-04-10', 'Empresa XYZ'),
(3, 670.30, '2026-04-12', 'Pedro Almeida');

-- 10. Verificação final
SELECT 'DEPARTAMENTOS' as Tabela, COUNT(*) as Registros FROM departamentos
UNION ALL
SELECT 'FUNCIONARIOS', COUNT(*) FROM funcionarios
UNION ALL
SELECT 'SALARIOS', COUNT(*) FROM salarios
UNION ALL
SELECT 'VENDAS', COUNT(*) FROM vendas;
```
