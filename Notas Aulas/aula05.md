SQL - (Structured Query Language)

\dt (Descrição de todas as relações)
\d (Descrição de uma tabela)

Definições de atributos

01 - not null

atributo não pode receber valores nulos
*tabela cliente
*atributo nome
Nome varchar(100) not null

02 - unique
o atributo tem valores unicos na tabela não sera possível 
introduzir um valor na tabela com o valor igual
*tabela cliente 
*atributo CPF
cpf varchar(14) not full unique

03 - primary key

indica a chave primária da tabela
*tabela VENDEDOR
PK: id
id INT primary key

04 - foreign key
permite fazer a referencia entre tabelas
*tabela PEDIDO
*atributo id_vendedor
id_vendedor INT REFERENCES vendedor (id)

05 - check 

garantir uma regra ded negocio do sistema 
*tabela cliente
*atributo salário
salario REAL CHECK (salario > 0)



-- Criando um Banco de dados (Database)

-- CREATE DATABASE aula0312


-- Criando uma tabela
-- CREATE TABLE IF NOT EXISTS PRODUTO(
--   ID INT PRIMARY KEY,
--   COD VARCHAR(4) NOT NULL UNIQUE,
--   NOME VARCHAR(100) NOT NULL,
--   PRECO REAL CHECK(PRECO > 0)
-- );

-- -- \dt;
-- \d PRODUTO;

-- -- Deletando uma DataBase e uma tabela
-- -- DROP DATABASE aula0312;
-- DROP TABLE PRODUTO;

-- CREATE TABLE IF NOT EXISTS PROFESSOR(
--   id int primary key,
--   nome varchar(100) not null,
--   cpf char(11) unique not null
-- );

-- CREATE TABLE IF NOT EXISTS TURMA(
--   id int,
--   numero INT UNIQUE,
--   duracao_dias int,
--   id_professor int,
--   primary key (id, numero),
--   constraint fk_prof foreign key(id_professor) references PROFESSOR(id)
-- );

-- \dt
-- \d TURMA
-- \d PROFESSOR

CREATE TABLE IF NOT EXISTS FORNECEDOR(
  id int primary key,
  nome varchar(100) not null,
  cidade varchar(100)
);

CREATE TABLE IF NOT EXISTS PECA(
  id int primary key,
  nome varchar(100) not null,
  descricao varchar(300)
);

CREATE TABLE IF NOT EXISTS VENDA(
  idFornecedor int,
  idPeca int,
  data date,
  quantidade int,
  foreign key(idFornecedor) references FORNECEDOR(id),
  foreign key(idPeca) references PECA(id)
);

alter table FORNECEDOR add column telefone varchar(13);
alter table PECA add column peca varchar(11);
alter table FORNECEDOR drop column cidade;

\dt
\d FORNECEDOR
\d PECA
\d VENDA