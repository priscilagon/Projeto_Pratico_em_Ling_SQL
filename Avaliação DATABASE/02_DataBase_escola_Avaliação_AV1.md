# Avaliação AV1 22/04/2026
## Script Completo do Banco (Execute no MySQL Workbench)

```sql
-- 1. Criar e usar banco
CREATE DATABASE IF NOT EXISTS escola_db;
USE escola_db;

-- 2. Tabela Professores
CREATE TABLE professores (
    id_professor INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    especialidade VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- 3. Tabela Turmas
CREATE TABLE turmas (
    id_turma INT AUTO_INCREMENT PRIMARY KEY,
    nome_turma VARCHAR(50) NOT NULL,
    turno VARCHAR(20) NOT NULL,
    sala VARCHAR(20) NOT NULL
);

-- 4. Tabela Alunos
CREATE TABLE alunos (
    id_aluno INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    data_nascimento DATE NOT NULL,
    email VARCHAR(100) NOT NULL,
    id_turma INT,
    FOREIGN KEY (id_turma) REFERENCES turmas(id_turma)
);

-- 5. Tabela Disciplina
CREATE TABLE disciplinas (
    id_disciplina INT AUTO_INCREMENT PRIMARY KEY,
    nome_disciplina VARCHAR(100) NOT NULL,
    carga_horaria INT NOT NULL,
    id_professor INT,
    FOREIGN KEY (id_professor) REFERENCES professores(id_professor)
);
ss
-- 6. Tabela Matriculas
CREATE TABLE matriculas (
    id_matricula INT AUTO_INCREMENT PRIMARY KEY,
    id_aluno INT NOT NULL,
    id_disciplina INT NOT NULL,
    nota_final DECIMAL(4,2),
    frequencia DECIMAL(5,2),
    FOREIGN KEY (id_aluno) REFERENCES alunos(id_aluno),
    FOREIGN KEY (id_disciplina) REFERENCES disciplinas(id_disciplina)
);

-- 7. Inserir professores
INSERT INTO professores (nome, especialidade, email) VALUES
('Carlos Silva', 'Banco de Dados', 'carlos@escola.com'),
('Marina Souza', 'Programação', 'marina@escola.com'),
('João Lima', 'Redes', 'joao@escola.com'),
('Fernanda Alves', 'Sistemas Operacionais', 'fernanda@escola.com');

-- 8. Inserir turmas
INSERT INTO turmas (nome_turma, turno, sala) VALUES
('1INFO-A', 'Manhã', 'Lab 01'),
('1INFO-B', 'Tarde', 'Lab 02'),
('2INFO-A', 'Manhã', 'Sala 05'),
('2INFO-B', 'Noite', 'Sala 08');

-- 9. Inserir alunos
INSERT INTO alunos (nome, data_nascimento, email, id_turma) VALUES
('Ana Beatriz', '2008-03-12', 'ana@aluno.com', 1),
('Bruno Costa', '2007-09-21', 'bruno@aluno.com', 2),
('Camila Rocha', '2008-01-30', 'camila@aluno.com', 1),
('Diego Martins', '2007-11-14', 'diego@aluno.com', 3);

-- 10. Inserir disciplinas
INSERT INTO disciplinas (nome_disciplina, carga_horaria, id_professor) VALUES
('Introdução ao SQL', 60, 1),
('Lógica de Programação', 80, 2),
('Fundamentos de Redes', 60, 3),
('Linux Básico', 40, 4);

-- 11. Inserir matriculas
INSERT INTO matriculas (id_aluno, id_disciplina, nota_final, frequencia) VALUES
(1, 1, 8.5, 92.0),
(2, 2, 7.8, 88.5),
(3, 1, 9.0, 95.0),
(4, 3, 6.9, 80.0);
```