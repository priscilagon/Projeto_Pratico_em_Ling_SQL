# Consultas SQL avançadas com JOIN para relatórios por faixa etária

Aqui estão **consultas SQL avançadas com múltiplos JOINs** para relatórios por **faixa etária** no sistema de vacinação. Elas usam `CASE WHEN` para categorizar idades, `GROUP BY` para agregação e funções de janela para ranking, seguindo técnicas de relatórios complexos.

## Q1. Cobertura Vacinal por Faixa Etária e Sexo (Múltiplos JOINs).

```sql
SELECT 
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 1 THEN '0-11 meses'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 2 THEN '1 ano'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 THEN '2-4 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 12 THEN '5-11 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 18 THEN '12-17 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 60 THEN '18-59 anos'
        ELSE '60+ anos'
    END as faixa_etaria,
    p.sexo,
    COUNT(DISTINCT p.id_paciente) as pacientes_vacinados,
    COUNT(a.id_aplicacao) as total_doses,
    ROUND((COUNT(a.id_aplicacao) / COUNT(DISTINCT p.id_paciente)), 1) as doses_por_paciente
FROM Paciente p
INNER JOIN Aplicacao_Vacina a ON p.id_paciente = a.id_paciente
INNER JOIN Profissional pr ON a.id_profissional = pr.id_profissional
INNER JOIN Lote_Vacina l ON a.id_lote = l.id_lote
INNER JOIN Vacina v ON l.id_vacina = v.id_vacina
WHERE a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
GROUP BY faixa_etaria, p.sexo
ORDER BY 
    CASE faixa_etaria
        WHEN '0-11 meses' THEN 1
        WHEN '1 ano' THEN 2
        WHEN '2-4 anos' THEN 3
        WHEN '5-11 anos' THEN 4
        WHEN '12-17 anos' THEN 5
        WHEN '18-59 anos' THEN 6
        ELSE 7
    END, p.sexo;
```


## Q2. Ranking de Profissionais por Faixa Etária (Função de Janela).

```sql
SELECT 
    pr.nome as profissional,
    c.nome_cargo,
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 THEN '0-4 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 12 THEN '5-11 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 60 THEN '12-59 anos'
        ELSE '60+ anos'
    END as faixa_etaria,
    COUNT(a.id_aplicacao) as total_doses,
    RANK() OVER (PARTITION BY faixa_etaria ORDER BY COUNT(a.id_aplicacao) DESC) as ranking_profissional
FROM Profissional pr
INNER JOIN Cargo_Profissional c ON pr.id_cargo = c.id_cargo
INNER JOIN Aplicacao_Vacina a ON pr.id_profissional = a.id_profissional
INNER JOIN Paciente p ON a.id_paciente = p.id_paciente
WHERE a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
GROUP BY pr.id_profissional, pr.nome, c.nome_cargo, faixa_etaria
HAVING total_doses > 0
ORDER BY faixa_etaria, ranking_profissional;
```


## Q3. Taxa de Cobertura por Vacina e Faixa Etária (Subconsulta).

```sql
SELECT 
    v.nome_vacina,
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 THEN 'Crianças (0-4)'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 12 THEN 'Crianças (5-11)'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 60 THEN 'Adultos (12-59)'
        ELSE 'Idosos (60+)'
    END as faixa_etaria,
    COUNT(a.id_aplicacao) as doses_aplicadas,
    COUNT(DISTINCT p.id_paciente) as pacientes_vacinados,
    ROUND(
        (COUNT(a.id_aplicacao) * 100.0 / 
         (SELECT COUNT(*) FROM Paciente p2 
          WHERE TIMESTAMPDIFF(YEAR, p2.data_nascimento, CURDATE()) 
                BETWEEN 
                CASE faixa_etaria 
                    WHEN 'Crianças (0-4)' THEN 0 WHEN 'Crianças (5-11)' THEN 5 
                    WHEN 'Adultos (12-59)' THEN 12 ELSE 60 
                END 
                AND 
                CASE faixa_etaria 
                    WHEN 'Crianças (0-4)' THEN 4 WHEN 'Crianças (5-11)' THEN 11 
                    WHEN 'Adultos (12-59)' THEN 59 ELSE 999 
                END)
        ), 2
    ) as taxa_cobertura_percent
FROM Aplicacao_Vacina a
INNER JOIN Paciente p ON a.id_paciente = p.id_paciente
INNER JOIN Lote_Vacina l ON a.id_lote = l.id_lote
INNER JOIN Vacina v ON l.id_vacina = v.id_vacina
WHERE a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY v.id_vacina, v.nome_vacina, faixa_etaria
ORDER BY v.nome_vacina, faixa_etaria;
```


## Q4. Comparativo Mensal por Faixa Etária (PIVOT com CTE).

```sql
WITH vacinacao_mensal AS (
    SELECT 
        CASE 
            WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 THEN '0-4 anos'
            WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 12 THEN '5-11 anos'
            ELSE '12+ anos'
        END as faixa_etaria,
        DATE_FORMAT(a.data_aplicacao, '%Y-%m') as mes_ano,
        COUNT(*) as doses
    FROM Aplicacao_Vacina a
    INNER JOIN Paciente p ON a.id_paciente = p.id_paciente
    WHERE a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
    GROUP BY faixa_etaria, mes_ano
)
SELECT 
    faixa_etaria,
    SUM(CASE WHEN mes_ano = DATE_FORMAT(DATE_SUB(CURDATE(), INTERVAL 1 MONTH), '%Y-%m') THEN doses ELSE 0 END) as mes_anterior,
    SUM(CASE WHEN mes_ano = DATE_FORMAT(CURDATE(), '%Y-%m') THEN doses ELSE 0 END) as mes_atual,
    ROUND(
        (SUM(CASE WHEN mes_ano = DATE_FORMAT(CURDATE(), '%Y-%m') THEN doses ELSE 0 END) * 100.0 /
         NULLIF(SUM(CASE WHEN mes_ano = DATE_FORMAT(DATE_SUB(CURDATE(), INTERVAL 1 MONTH), '%Y-%m') THEN doses ELSE 0 END), 0)),
        1
    ) as variacao_percent
FROM vacinacao_mensal
GROUP BY faixa_etaria
ORDER BY 
    CASE faixa_etaria
        WHEN '0-4 anos' THEN 1
        WHEN '5-11 anos' THEN 2
        ELSE 3
    END;
```


## Q5. Pacientes Atrasados por Faixa Etária e Vacina (LEFT JOIN Completo).

```sql
SELECT 
    CASE 
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 THEN '0-4 anos'
        WHEN TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 12 THEN '5-11 anos'
        ELSE '12+ anos'
    END as faixa_etaria,
    v.nome_vacina,
    COUNT(*) as pacientes_pendentes,
    GROUP_CONCAT(DISTINCT p.nome ORDER BY p.nome SEPARATOR ', ') as nomes_pacientes
FROM Paciente p
CROSS JOIN Vacina v  -- Todas as combinações possíveis
LEFT JOIN (
    SELECT DISTINCT id_paciente, id_vacina 
    FROM Aplicacao_Vacina a
    INNER JOIN Lote_Vacina l ON a.id_lote = l.id_lote
    WHERE a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
) vacinados ON p.id_paciente = vacinados.id_paciente AND v.id_vacina = vacinados.id_vacina
WHERE vacinados.id_paciente IS NULL
    AND TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) BETWEEN 0 AND 11  -- Foco em crianças
GROUP BY faixa_etaria, v.nome_vacina
HAVING pacientes_pendentes > 0
ORDER BY faixa_etaria, pacientes_pendentes DESC;
```


## Q6. Dashboard Executivo - KPIs por Faixa Etária (Subconsultas Aninhadas).

```sql
SELECT 
    '0-4 anos' as faixa_etaria,
    (SELECT COUNT(*) FROM Paciente WHERE TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE()) < 5) as total_pacientes,
    (SELECT COUNT(DISTINCT id_paciente) FROM Aplicacao_Vacina a 
     INNER JOIN Paciente p ON a.id_paciente = p.id_paciente 
     WHERE TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 
       AND a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)) as vacinados_ano,
    ROUND(
        (SELECT COUNT(DISTINCT id_paciente) FROM Aplicacao_Vacina a 
         INNER JOIN Paciente p ON a.id_paciente = p.id_paciente 
         WHERE TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) < 5 
           AND a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)) * 100.0 /
        NULLIF((SELECT COUNT(*) FROM Paciente WHERE TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE()) < 5), 0),
        1
    ) as cobertura_percent

UNION ALL

SELECT 
    '5-11 anos' as faixa_etaria,
    (SELECT COUNT(*) FROM Paciente WHERE TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE()) BETWEEN 5 AND 11) as total_pacientes,
    (SELECT COUNT(DISTINCT id_paciente) FROM Aplicacao_Vacina a 
     INNER JOIN Paciente p ON a.id_paciente = p.id_paciente 
     WHERE TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) BETWEEN 5 AND 11 
       AND a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)) as vacinados_ano,
    ROUND(
        (SELECT COUNT(DISTINCT id_paciente) FROM Aplicacao_Vacina a 
         INNER JOIN Paciente p ON a.id_paciente = p.id_paciente 
         WHERE TIMESTAMPDIFF(YEAR, p.data_nascimento, CURDATE()) BETWEEN 5 AND 11 
           AND a.data_aplicacao >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)) * 100.0 /
        NULLIF((SELECT COUNT(*) FROM Paciente WHERE TIMESTAMPDIFF(YEAR, data_nascimento, CURDATE()) BETWEEN 5 AND 11), 0),
        1
    ) as cobertura_percent;
```


## Pontos Técnicos Avançados Usados

- **Múltiplos INNER JOINs** para relacionar 5+ tabelas sem perda de dados.
- **CASE WHEN aninhado** para categorização dinâmica de faixas etárias.
- **Funções de janela (RANK())** para ranking dentro de partições.
- **Subconsultas correlacionadas** para cálculos de taxa de cobertura.
- **CTE (WITH)** para organizar consultas complexas com PIVOT simulado.
- **CROSS JOIN + LEFT JOIN** para matriz completa de cobertura.
- **UNION ALL** para KPIs executivos em formato de tabela única.
