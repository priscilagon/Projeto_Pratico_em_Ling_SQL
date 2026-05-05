# Projeto de Sistema de Gerenciamento de Banco de Dados II - Prática

Documento de organização dos temas de projetos integradores do componente **Projeto de Gerenciamento de Banco de Dados**, alinhado ao plano de ensino que prevê modelagem de dados, construção do banco em MySQL, uso de CRUD, procedures, triggers, integração com Python, dicionário de dados e apresentação final.

O plano também estabelece que a apresentação final deve contemplar contextualização do sistema, DER, demonstração de operações CRUD, execução de procedure ou trigger e relatório em Python, além de documentação técnica e dicionário de dados.


LINK: [https://docs.google.com/spreadsheets/d/1CoC2VqG8fB9aeopMoSdMeHH2X1LWzoBSXJmD5MWgbVI/edit?usp=sharing]
---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório: 06/05/2026 termino da aula.**
- **Tema escolhido:** Tema 1
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 01)

1. Ana Carolina Bomfim Bandeira
2. Breno Giovanni Batalha Frota
3. Marley Gabriel Alves Lemos
4. Wesley Neves Batista

---

## Tema 1

### Sistema de Gerenciamento: Locadora de Carros

Este projeto propõe o desenvolvimento de um SGBD para uma locadora de carros, permitindo cadastrar clientes, categorias de veículos, compras de veículos para a frota e processos de locação, com controle de disponibilidade, prazos, valores e multas.

O sistema pode trabalhar com entidades como **Cliente, Compra, Locação, Categoria e Veículo**, permitindo ao aluno modelar regras de negócio reais, realizar operações CRUD, aplicar integridade referencial e desenvolver consultas gerenciais sobre faturamento, veículos locados, clientes mais ativos e multas por atraso.

### Sugestões para desenvolvimento

- Modelar o DER com foco em cliente, veículo, categoria e locação.
- Criar tabelas com chaves primárias, estrangeiras e restrições de integridade.
- Desenvolver consultas SQL para faturamento e controle de locações.
- Implementar procedure para cálculo de multa por atraso e trigger para atualizar o status do veículo.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 2
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 04)

1. Antônio Augusto Teixeira de Souza
2. Chester da Silva Amorim
3. Gabriela Pinto da Mota
4. Jose Roberto Garcia Souza 

---

## Tema 2

### Sistema de Gerenciamento: Escola de Ensino Fundamental

Este tema consiste na construção de um banco de dados para uma escola, organizando informações acadêmicas e pedagógicas sobre alunos, turmas, professores, matérias e notas.

Com entidades como **Aluno, Turma, Professor, Matéria e Nota**, o projeto possibilita trabalhar relacionamentos entre matrícula, ensino e avaliação, além de permitir consultas como médias por aluno, desempenho por turma, professores por disciplina e histórico escolar.

### Sugestões para desenvolvimento

- Modelar entidades acadêmicas e seus relacionamentos pedagógicos.
- Criar regras de negócio para matrícula, lançamento de notas e vínculo entre professor e disciplina.
- Elaborar consultas para desempenho por turma e rendimento escolar.
- Implementar procedure para cálculo de média final e trigger para atualização de situação do aluno.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 3
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 05)

1. Elano Barros Serrão
2. Izhac Nylton Silva Santos
3. Victor Emanuel Souza de Oliveira

---

## Tema 3

### Sistema de Gerenciamento: Estoque de Loja de Informática

Este projeto tem como finalidade desenvolver um SGBD para controle de estoque e movimentação comercial de uma loja de informática, registrando produtos, fornecedores, vendas e saldo de estoque.

As entidades **Produto, Fornecedor, Venda e Estoque** permitem modelar entradas e saídas, reposição, cadastro de itens, valor unitário, quantidades disponíveis e relatórios como produtos mais vendidos, itens com baixo estoque e fornecedores mais utilizados.

### Sugestões para desenvolvimento

- Criar o cadastro de produtos e fornecedores com controle de estoque.
- Modelar processos de entrada e saída de mercadorias.
- Desenvolver consultas sobre itens com baixo estoque e vendas realizadas.
- Implementar procedure para atualização automática de saldo e trigger para alertas de estoque mínimo.

---
## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 4
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 02)

1. Dayana Araujo
2. Isaque Bezerra Reis
3. Ygor Menezes e Silva

---

## Tema 4

### Sistema de Gerenciamento: Clínica Médica

Neste tema, os alunos desenvolvem um sistema de banco de dados voltado à rotina de uma clínica médica, com controle de pacientes, profissionais, consultas, prontuários e medicamentos.

As entidades **Paciente, Médico, Consulta, Prontuário e Medicamento** favorecem a construção de um projeto mais robusto, com foco em sigilo, histórico clínico, agendamento e relatórios de atendimentos, além de consultas avançadas por especialidade, frequência de consultas e prescrição médica.

### Sugestões para desenvolvimento

- Organizar os dados de atendimento clínico e histórico do paciente.
- Estruturar o banco para cadastro de médicos, consultas e medicamentos.
- Criar consultas sobre agendamentos, atendimentos e prontuários.
- Desenvolver procedure para agendamento e trigger para registrar atualizações de prontuário.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 5
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 06)

1. Ana Kelley Carril de Paula
2. Caio Felipe Freitas de Jesus
3. Sarah da Siva Siqueira

---

## Tema 5

### Sistema de Gerenciamento: Biblioteca Digital

O projeto da biblioteca digital busca estruturar um banco de dados para controle do acervo e dos empréstimos realizados em uma biblioteca escolar.

Com entidades como **Livro, Usuário, Empréstimo e Multa**, o sistema permite registrar entradas e saídas de livros, calcular atrasos, controlar disponibilidade e produzir relatórios sobre livros mais emprestados, usuários inadimplentes e histórico de empréstimos.

### Sugestões para desenvolvimento

- Modelar o acervo bibliográfico e o cadastro de usuários.
- Criar a estrutura de empréstimos e devoluções.
- Elaborar consultas sobre multas, atrasos e livros mais utilizados.
- Implementar procedure para cálculo de multa e trigger para bloqueio de novos empréstimos em caso de atraso.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 6
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 08)

1. Joquebede Barboza Santos
2. Sophia Roque Martins
3. Suellen Tavares dos Santos
4. Suely Nascimento Costa

---

## Tema 6

### Sistema de Gerenciamento: Restaurante/Pizzaria

Este projeto propõe o desenvolvimento de um SGBD para registrar pedidos, mesas, itens consumidos e atendimento em uma pizzaria ou restaurante.

As entidades **Mesa, Item, Pedido e Garçom** permitem simular um sistema comercial com fluxo de atendimento, consumo por mesa, fechamento de conta e consultas de negócio, como itens mais vendidos, pedidos por período e desempenho de atendimento por garçom.

### Sugestões para desenvolvimento

- Criar tabelas para controle de pedidos, itens e atendimento por mesa.
- Relacionar garçom, pedido e produtos consumidos.
- Desenvolver consultas para produtos mais vendidos e pedidos do dia.
- Implementar procedure para fechamento de conta e trigger para mudança automática do status do pedido.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 7
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 07)

1. Camilly Pires de Oliveira
2. Natanael dos Santos Borges
3. Osvaldo Oliveira de Souza

---

## Tema 7

### Sistema de Gerenciamento: Oficina Mecânica

Neste tema, o aluno desenvolve um banco de dados para gerenciar os processos de entrada de veículos, atendimento ao cliente, serviços executados, peças utilizadas e ordens de serviço.

As entidades **Veículo, Cliente, Serviço, Peça e Ordem** possibilitam trabalhar uma modelagem rica em relacionamentos, além de consultas sobre serviços mais realizados, clientes frequentes, uso de peças e custos de manutenção por veículo.

### Sugestões para desenvolvimento

- Modelar atendimento, ordem de serviço e controle de peças.
- Relacionar cliente, veículo e serviços executados.
- Produzir consultas sobre custos, serviços e consumo de peças.
- Implementar procedure para gerar orçamento e trigger para atualizar estoque de peças utilizadas.

---

## Orientações gerais do grupo

- **Turma:** Técnico de Nível Médio em Informática 2025.1
- **Professor(a):** Priscila Gonçalves
- **Data de entrega:** 11/05/2026 - Relatório Técnico: Impresso e encadernação ou grampeado.
- **Entrega de versões do relatório, toda sexta-feira ao termino da aula.**
- **Tema escolhido:** Tema 8
- **Líder do grupo:** 

### Lista de nomes dos alunos (Grupo 03)

1. Aldenora Santos de Oliveira
2. Bryam Richard Rocha do Nascimento
3. Ivson Padilha Freire

---

## Tema 8

### Sistema de Gerenciamento: Vacinação

Este projeto propõe a construção de um SGBD para apoiar campanhas de vacinação em postos de saúde, controlando cidadãos, vacinas, doses aplicadas, lotes, agendamentos, reações adversas e relatórios epidemiológicos.

Trata-se de um tema com forte relevância social e boa complexidade técnica, pois permite explorar controle de estoque por lote, cálculo de próximas doses, acompanhamento de cobertura vacinal e consultas analíticas por faixa etária, bairro, posto de saúde e tipo de vacina.

### Sugestões para desenvolvimento

- Estruturar tabelas para cidadãos, vacinas, lotes, doses e postos de saúde.
- Controlar validade de lotes e histórico de aplicação de doses.
- Criar consultas sobre cobertura vacinal, agendamentos e reações adversas.
- Implementar procedure para cálculo da próxima dose e trigger para impedir uso de lote vencido.

---

## Critérios de apresentação do projeto

- Apresentar a contextualização do sistema escolhido.
- Exibir o DER e explicar as entidades, relacionamentos e regras de negócio.
- Demonstrar na apresentação pelo menos três operações CRUD.
- Executar uma procedure ou trigger funcionando no banco.
- Mostrar integração com Python por meio de consulta, menu ou relatório.
- Entregar dicionário de dados e relatório final das atividades.

---

## Estrutura mínima esperada no projeto

- Tema do sistema.
- Requisitos funcionais e não funcionais.
- Modelagem conceitual, lógica e física.
- Script DDL e operações CRUD.
- Consultas SQL básicas e avançadas.
- Procedure, trigger e integração Python.
- Dicionário de dados.
- Relatório técnico final, das práticas no laboratório de informática.

## Verificar documentação para elaborar o Relatório Técnico
