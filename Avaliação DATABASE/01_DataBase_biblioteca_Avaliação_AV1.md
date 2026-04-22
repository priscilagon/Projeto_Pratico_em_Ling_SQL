# Avaliação AV1 22/04/2026
## Script Completo do Banco (Execute no MySQL Workbench)

```sql
-- 1. Criar e usar banco
CREATE DATABASE IF NOT EXISTS biblioteca_db CHARACTER SET utf8mb4;
USE biblioteca;

-- 2. Tabela Autores
CREATE TABLE autores (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    nacionalidade VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 3. Tabela Livros
CREATE TABLE livros (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    ano_publicacao INT NOT NULL,
    autor_id INT NOT NULL,
    FOREIGN KEY (autor_id) REFERENCES autores(id) ON DELETE RESTRICT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 4. Inserir Autores (4 registros de dados)
INSERT INTO autores (nome, nacionalidade) VALUES
('J.K. Rowling', 'Britânica'),
('Machado de Assis', 'Brasileira'),
('George Orwell', 'Britânica'),
('Clarice Lispector', 'Brasileira');

-- 5. Inserir Livros (8 registros de dados)
INSERT INTO livros (titulo, ano_publicacao, autor_id) VALUES
('Harry Potter e a Pedra Filosofal', 1997, 1),
('Harry Potter e o Prisioneiro de Azkaban', 1999, 1),
('Harry Potter e o Cálice de Fogo', 2000, 1),
('Dom Casmurro', 1899, 2),
('Memórias Póstumas de Brás Cubas', 1881, 2),
('1984', 1949, 3),
('A Hora da Estrela', 1977, 4),
('A Paixão Segundo G.H.', 1964, 4);

-- 6. Verificação final
SELECT COUNT(*) as total_autores FROM autores;
SELECT COUNT(*) as total_livros FROM livros; 
SHOW TABLES;
```