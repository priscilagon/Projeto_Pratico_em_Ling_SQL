# Avaliação AV1 22/04/2026
## Script Completo do Banco (Execute no MySQL Workbench)

```sql
-- 1. Criar banco de dados
CREATE DATABASE hospital_db;
USE hospital_db;

-- 2. Tabela Usuario
CREATE TABLE Usuario (
    id_usuario INT PRIMARY KEY AUTO_INCREMENT,
    nome_usuario VARCHAR(100) NOT NULL,
    login VARCHAR(50) NOT NULL UNIQUE,
    senha VARCHAR(255) NOT NULL,
    perfil VARCHAR(50) NOT NULL,
    status VARCHAR(20) DEFAULT 'ativo'
);

-- 3. Tabela Paciente
CREATE TABLE Paciente (
    id_paciente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) NOT NULL,
    cpf VARCHAR(14) UNIQUE,
    cns VARCHAR(20) UNIQUE,
    data_nascimento DATE NOT NULL,
    sexo CHAR(1),
    telefone VARCHAR(20),
    endereco VARCHAR(150)
);

-- 4. Tabela Cargo_Profissional
CREATE TABLE Cargo_Profissional (
    id_cargo INT PRIMARY KEY AUTO_INCREMENT,
    nome_cargo VARCHAR(60) NOT NULL UNIQUE
);

-- 5. Tabela Profissional
CREATE TABLE Profissional (
    id_profissional INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) NOT NULL,
    cpf VARCHAR(14) NOT NULL UNIQUE,
    cns VARCHAR(20),
    registro_conselho VARCHAR(30),
    id_cargo INT NOT NULL,
    id_usuario INT UNIQUE,
    FOREIGN KEY (id_cargo) REFERENCES Cargo_Profissional(id_cargo),
    FOREIGN KEY (id_usuario) REFERENCES Usuario(id_usuario)
);

-- 6. Tabela Medicamento
CREATE TABLE Medicamento (
    id_medicamento INT PRIMARY KEY AUTO_INCREMENT,
    nome_medicamento VARCHAR(100) NOT NULL,
    fabricante VARCHAR(100),
    descricao VARCHAR(200)
);

-- 7. Tabela Lote Medicamento
CREATE TABLE Lote_Medicamento (
    id_lote_medicamento INT PRIMARY KEY AUTO_INCREMENT,
    numero_lote VARCHAR(50) NOT NULL UNIQUE,
    data_validade DATE NOT NULL,
    quantidade_disponivel INT NOT NULL,
    id_medicamento INT NOT NULL,
    FOREIGN KEY (id_medicamento) REFERENCES Medicamento(id_medicamento)
);
 -- 8. Tabela Aplicacao Medicamento
CREATE TABLE Aplicacao_Medicamento (
    id_aplicacao INT PRIMARY KEY AUTO_INCREMENT,
    data_aplicacao DATE NOT NULL,
    dose VARCHAR(30) NOT NULL,
    via_aplicacao VARCHAR(50),
    local_aplicacao VARCHAR(50),
    observacao VARCHAR(255),
    id_paciente INT NOT NULL,
    id_profissional INT NOT NULL,
    id_lote_medicamento INT NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES Paciente(id_paciente),
    FOREIGN KEY (id_profissional) REFERENCES Profissional(id_profissional),
    FOREIGN KEY (id_lote_medicamento) REFERENCES Lote_Medicamento(id_lote_medicamento)
);
```