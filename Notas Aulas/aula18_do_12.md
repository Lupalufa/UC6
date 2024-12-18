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
	data_adesao date check (date_part('year', age(current_date, data_adesao)) >= 18)
);

create table if not exists emprestimo(
	id serial primary key,
	livro_emprestimo int,
	aluguel_usuario int,
	data_emprestimo date not null,
	data_devolucao date,
	constraint fk_livro_emprestimo foreign key (livro_emprestimo) references livro(id) on delete cascade,
	constraint fk_usuario_emprestimo foreign key (aluguel_usuario) references usuario(id) on delete cascade
	constraint chk_data_devolucao check (data_devolucao >= data_emprestimo)
);

-- Alter tables e suas modificações

-- alter table livro add constraint lmt_ano_publicado check (ano_publicado >= 1500 and ano_publicado <= date_part('year', age(current_date,ano_publicado)))

