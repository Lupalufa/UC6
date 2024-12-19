begin;
rollback;

create table if not exists autor(
	id serial primary key,
	nome varchar(60) not null,
	data_nascimento date check (date_part('year', age(current_date, data_nascimento)) >= 18)
);

create table if not exists livro(
	id serial primary key,
	titulo varchar(70) not null,
	id_autor int,
	ano_publicado smallint check (ano_publicado >= 1500 and ano_publicado <= extract(year from current_date)),
	constraint fk_autor_livro foreign key (id_autor) references autor(id) on delete cascade
);

create table if not exists usuario(
	id serial primary key,
	nome varchar(70) not null,
	endereco_email varchar(100) unique ,
	data_adesao date default (current_date)
);

create table if not exists emprestimo(
	id serial primary key,
	id_livro int,
	id_usuario int,
	data_emprestimo date not null,
	data_devolucao date not null,
	constraint fk_livro_emprestimo foreign key (id_livro) references livro(id) on delete cascade,
	constraint fk_usuario_emprestimo foreign key (id_usuario) references usuario(id) on delete cascade,
	constraint chk_data_devolucao check (data_devolucao >= data_emprestimo),
	constraint uq_livro_emprestimo unique (id_livro, data_emprestimo)
);

-- Alter tables e suas modificações

-- alter table livro add constraint lmt_ano_publicado check (ano_publicado >= 1500 and ano_publicado <= date_part('year', age(current_date,ano_publicado)))

insert into autor(nome,data_nascimento)
values('Valtemir', '2006-01-10'),('Beethoven','1839-07-12'),('William Shakespeare','1616-07-20'),('Miguel de Cervantes','1547-09-29'),
('Paulo Coelho','1872-02-21');

insert into livro(titulo,id_autor,ano_publicado)
values('As tranças do rei careca',1,2000),('A volta dos que não foram',2,1924),('O enterro do vivo',3,1600),('O pequeno principe',4,1740),('Lucas o intruso no formigueiro',5,2003);

insert into usuario(nome,endereco_email)
values('Pedro Lucas','pedrolucas@gmail.com'),('João Pedro','joaopedro@gmail.com'),('Lucas','lucas@gmail.com'),('Gabriel','gabriel@gmail.com'),('Ana Flavia','anaflavia@gmail.com');

insert into emprestimo(id_livro,id_usuario,data_emprestimo,data_devolucao)
values(1,1,'2024-12-10','2024-12-15'),(2,2,'2024-08-10','2024-08-18'),(3,3,'2024-09-28','2024-10-04'),(4,4,'2024-05-19','2024-05-25'),(5,5,'2024-06-07','2024-06-19');

-- Questão 1
select livro.titulo, autor.nome from autor join livro on livro.id = autor.id;

-- Questão 2
select nome,endereco_email from usuario; 

-- Questão 3
select usuario.nome, livro.titulo, emprestimo.data_emprestimo, emprestimo.data_devolucao from emprestimo join usuario 
on id_usuario = usuario.id join livro on id_livro = livro.id;

-- Questão 4
select livro.titulo, emprestimo.data_emprestimo from emprestimo join livro on id_livro = livro.id where data_devolucao = null;

-- Questão 5
select usuario.nome, livro.titulo, livro.id_autor, autor.nome from emprestimo join livro on id_livro = livro.id 
join usuario on id_usuario = usuario.id join autor on id_autor = autor.id where autor.nome = 'Beethoven'; 


select * from autor;
select * from livro;
select * from usuario;