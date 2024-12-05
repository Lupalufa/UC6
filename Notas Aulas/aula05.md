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


create table if not exists usuario(
  id serial primary key,
  nome varchar(100) not null,
  email varchar(100) not null unique,
  data_cadastro timestamp default current_timestamp
);

create table if not exists editora(
  id serial primary key,
  nome varchar(50) not null unique
);

create table if not exists genero(
  id serial primary key,
  nome varchar(50) not null unique
);

create table if not exists livro(
  id serial primary key,
  titulo varchar(50) not null,
  quantidade_disponivel int not null check (quantidade_disponivel > 0),
  idEditora int not null,
  idGenero int not null,
  constraint fk_livros_editora foreign key(idEditora) references editora(id) on delete cascade,
  constraint fk_livros_genero foreign key(idGenero) references genero(id) on delete cascade
);

create table if not exists emprestimo(
  id_usuario int not null,
  id_livro int not null,
  data_emprestimo timestamp default current_timestamp,
  data_devolucao timestamp not null,
  constraint fk_emprestimo_usuario foreign key (id_usuario) references usuario(id) on delete cascade,
  constraint fk_emprestimo_livros foreign key (id_livro) references livro(id) on delete cascade
);

-- Adicionar um campo para armazenar o telefone dos usuarios

-- alter table usuario add telefone char(11);
alter table usuario add column telefone char(11) default 'nenhum';

-- Altere o tamanho do campo titulo na tabela livros para comportar até 200 caracteres
alter table livro alter column titulo type varchar(200);

-- Remova o campo data_cadastro da tabela usuarios, pois ele não será mais utilizado.
alter table usuario drop data_cadastro;

-- Garanta que o mesmo título de livro não possa ser registrado na mesma editora.
alter table livro add constraint uq_livros_titulo_editora unique(titulo, idEditora);

alter table emprestimo add constraint chk_data_devolucao check (data_devolucao >= data_emprestimo);

insert into usuario(id, nome, email)
values(1, 'Valtemir', 'valtemir@senac.com'), (2, 'Joel', 'Joel@senac.com');

select * from usuario;
select * from genero;
select * from editora;

insert into editora(id, nome)
values (1,'Senac'), (2,'Sesc'), (3,'Mundo'),(4,'Leitura');
select * from editora;
insert into genero(id, nome)
values (1,'Terror'),(2,'Drama'),(3,'Romance'),(4,'Ação');
select * from genero;

insert into livro(titulo,quantidade_disponivel,idEditora,idGenero)
values('Bela e a Fera',50,1,2), ('FNAF The silver eyes',100,4,1),('FNAF the fourth closet', 74,4,1),('FNAF Into the pit',38,4,1),('FNAF the twisted one', 90,4,1);
select * from livro;

insert into emprestimo(id_usuario,id_livro,data_devolucao)
values(1, 2, '2024-12-09');
select * from emprestimo


