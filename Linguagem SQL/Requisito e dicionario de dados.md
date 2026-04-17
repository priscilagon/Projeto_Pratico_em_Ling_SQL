

## **Projeto de Banco de Dados – Sistema de Vacinação**

### **1. Requisitos Funcionais**

1. **Cadastrar paciente**
    - Permitir cadastro de paciente com nome, CPF, data de nascimento, telefone, status (ativo/inativo) e observações.
2. **Cadastrar vacina**
    - Cadastrar tipos de vacina (nome, descrição, fabricante, duração da imunidade em meses) em um catálogo.
3. **Registrar aplicação de vacina**
    - Registrar aplicação informando: paciente, vacina, clínica, data de aplicação, lote, dose (1ª, 2ª, 3ª, etc.) e responsável (profissional).
4. **Consultar histórico de vacinação do paciente**
    - Exibir lista de vacinas aplicadas a um paciente, com data, dose, clínica e responsável.
5. **Gerenciar clínicas**
    - Cadastrar, editar e listar clínicas (nome, endereço, responsável) utilizadas no sistema.
6. **Emitir relatórios básicos**
    - Gerar relatórios de:
        - vacinas aplicadas por período;
        - total de doses por tipo de vacina;
        - pacientes por clínica.

***

### **2. Requisitos Não Funcionais**

1. **Disponibilidade**
    - O sistema deve estar disponível em horário de atendimento de clínicas (pelo menos 8h por dia, 5 dias por semana).
2. **Segurança de dados**
    - CPF e dados de pacientes devem ser armazenados com controle de acesso.
    - Telas e relatórios não devem expor dados sensíveis sem autenticação.
3. **Performance**
    - Consulta de histórico de vacinação de um paciente deve retornar resultados em até 2 segundos, mesmo com até 100.000 registros.
4. **Integridade dos dados**
    - Não é permitido:
        - cadastrar dois pacientes com o mesmo CPF;
        - registrar aplicação sem paciente, vacina ou clínica válidos;
        - apagar dados de paciente com histórico sem auditoria.
5. **Auditoria e rastreabilidade**
    - O sistema deve registrar quem realizou cadastro/alteração de dados críticos (se possível, com módulo mínimo de log de auditoria).
6. **Compatibilidade**
    - Banco de dados em **MySQL 5.7 ou superior**, com suporte a servidor Linux/Windows.
7. **Usabilidade**
    - Principais funcionalidades (paciente, aplicação e histórico) devem ser acessíveis em até 3 telas.

***

### **3. Dicionário de Dados – Tabelas**

#### **Tabela: pacientes**

```markdown
| Campo               | Tipo                     | Null?      | PK/FK                               | Descrição                                        | Observação                                     |
|---------------------|--------------------------|------------|--------------------------------------|--------------------------------------------------|-----------------------------------------------|
| id_paciente         | INT AUTO_INCREMENT       | NOT NULL   | PK                                   | Identificador único do paciente.                 | Chave primária, autoincremento.              |
| nome                | VARCHAR(100)             | NOT NULL   | –                                    | Nome completo do paciente.                       | Não pode ser vazio.                          |
| cpf                 | VARCHAR(14)              | NOT NULL   | UNIQUE                               | CPF do paciente.                                | Único, sem repetição.                        |
| data_nascimento     | DATE                     | NULL       | –                                    | Data de nascimento.                              | Opcional, mas recomendado.                   |
| telefone            | VARCHAR(20)              | NULL       | –                                    | Telefone de contato.                             | Opcional.                                    |
| status              | ENUM('ativo', 'inativo') | NOT NULL   | –                                    | Situação do cadastro.                            | Padrão 'ativo'.                              |
| comentario          | TEXT                     | NULL       | –                                    | Observações sobre o paciente.                    | Campo livre.                                 |
```


#### **Tabela: vacinas**

```markdown
| Campo                          | Tipo             | Null?      | PK/FK                            | Descrição                                           | Observação                                    |
|--------------------------------|------------------|------------|----------------------------------|-----------------------------------------------------|----------------------------------------------|
| id_vacina                      | INT AUTO_INCREMENT | NOT NULL   | PK                               | Identificador único da vacina.                      | Chave primária, autoincremento.             |
| nome                           | VARCHAR(50)      | NOT NULL   | UNIQUE                           | Nome da vacina (ex.: "COVID-19", "Gripe").          | Único, sem repetição.                        |
| descricao                      | TEXT             | NULL       | –                                | Descrição da vacina.                                | Texto livre.                                 |
| fabricante                     | VARCHAR(100)     | NULL       | –                                | Nome do fabricante.                                 | Opcional.                                    |
| duracao_imunidade_meses        | INT              | NOT NULL   | –                                | Duração esperada da imunidade, em meses.            | Padrão 60 (5 anos).                          |
```


#### **Tabela: clinicas**

```markdown
| Campo               | Tipo             | Null?      | PK/FK                            | Descrição                                       | Observação                                       |
|---------------------|------------------|------------|----------------------------------|-------------------------------------------------|-------------------------------------------------|
| id_clinica          | INT AUTO_INCREMENT | NOT NULL   | PK                               | Identificador único da clínica.                 | Chave primária, autoincremento.              |
| nome                | VARCHAR(100)     | NOT NULL   | UNIQUE                           | Nome da clínica/posto.                          | Único, sem repetição.                          |
| endereco            | TEXT             | NULL       | –                                | Endereço completo da unidade.                   | Campo texto longo.                             |
| responsavel         | VARCHAR(100)     | NULL       | –                                | Nome do responsável/gestor da clínica.          | Opcional.                                      |
```


#### **Tabela: aplicacoes_vacina**

```markdown
| Campo               | Tipo             | Null?      | PK/FK                                                           | Descrição                                                       | Observação                                                      |
|---------------------|------------------|------------|------------------------------------------------------------------|------------------------------------------------------------------|-----------------------------------------------------------------|
| id_aplicacao        | INT AUTO_INCREMENT | NOT NULL   | PK                                                               | Identificador único da aplicação.                               | Chave primária, autoincremento.                              |
| id_paciente         | INT              | NOT NULL   | FK → pacientes(id_paciente)                                      | Referência ao paciente.                                         | Não pode ser NULL.                                            |
| id_vacina           | INT              | NOT NULL   | FK → vacinas(id_vacina)                                          | Referência ao tipo de vacina.                                   | Não pode ser NULL.                                            |
| id_clinica          | INT              | NOT NULL   | FK → clinicas(id_clinica)                                        | Referência à clínica onde foi aplicada.                         | Não pode ser NULL.                                            |
| data_aplicacao      | DATE             | NOT NULL   | –                                                                | Data em que a vacina foi aplicada.                              | Validada pela aplicação.                                      |
| lote                | VARCHAR(20)      | NULL       | –                                                                | Lote da vacina.                                                 | Opcional.                                                     |
| dose                | INT              | NOT NULL   | –                                                                | Número da dose (ex.: 1, 2, 3).                                  | Padrão 1.                                                     |
| responsavel         | VARCHAR(100)     | NULL       | –                                                                | Profissional que aplicou a vacina.                              | Pode ser omitido.                                             |
| observacao          | TEXT             | NULL       | –                                                                | Observações adicionais da aplicação.                            | Campo livre.                                                  |
```


***

Com esse Markdown, você pode:

- copiar para um arquivo `.md` e converter em **PDF** (usando `pandoc` ou gerador online);
- usar como **anexo de projeto de BD**, entregável a alunos ou para avaliação final;
- aproveitar como modelo de dicionário de dados para outros sistemas (restaurante, locadora, escola, etc.).

Se quiser, posso montar uma **versão compacta só com o dicionário de dados em tabela SQL comentado** (sem Markdown, pronto para colar no MySQL Workbench como documentação). Quer que eu faça isso também?

