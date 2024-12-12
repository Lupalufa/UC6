-- Criação da tabale Cliente
	create table if not exists cliente(
	id serial primary key,
	nome varchar(100) not null,
	email varchar(100) not null unique,
	telefone varchar(20) not null unique,
	data_cadastro date default(current_date) not null
	);


-- Criação da tabela Serviços

	create table if not exists servico(
	id serial primary key,
	nome varchar(100) not null,
	descricao text not null,
	preco decimal(10,2) not null,
	tipo_servico varchar(50) not null
	);

	alter table servico add constraint verificar_preco check (preco > 0);
	alter table servico add constraint verificar_tipo_servico check (tipo_servico in ('Consultoria', 'Suporte','Manutenção'))

	-- Alter tables de Serviço

	create table if not exists tecnico(
	id serial primary key,
	nome varchar(100) not null,
	especialidade varchar(50) not null,
	data_contratacao date default(current_date) not null
	);

	alter table tecnico add constraint especialidade check (especialidade in ('Consultoria TI', 'Suporte Técnico', 'Manutenção'));

	-- Criação da tabela Chamados

	create table if not exists chamado(
	id serial primary key,
	cliente_id int,
	servico_id int,
	tecnico_id int,
	data_chamado date default(current_date) not null,
	status varchar(20) not null,
	descricao text not null,
	constraint fk_cliente foreign key (cliente_id) references cliente(id) on delete cascade,
	constraint fk_servico foreign key (servico_id) references servico(id) on delete cascade,
	constraint fk_tecnico foreign key (tecnico_id) references tecnico(id) on delete cascade
	);

	alter table chamado add constraint verificar_status check (status in ('Pendente', 'Em Andamento', 'Finalizado'));

	-- Criação do Alter table de chamados

	-- Criação da tabela Pagamento

	create table if not exists pagamento(
	id serial primary key,
	cliente_id int,
	chamado_id int,
	valor_pago decimal(10,2) not null,
	data_pagamento date default(current_date) not null,
	forma_pagamento varchar(50) not null,
	constraint fk_cliente_pagamento foreign key (cliente_id) references cliente(id) on delete cascade,
	constraint fk_chamado_pagamento foreign key (chamado_id) references chamado(id) on delete cascade
	);

	alter table pagamento add constraint vf_valor_pago check (valor_pago > 0);

	-- Criação da tabale a

	-- Fazendo Insert de Informações

	insert into cliente(nome,email,telefone,data_cadastro)
	values('João Silva','joao@email.com','(11) 98765-4321','2023-01-15');
	insert into cliente(nome,email,telefone,data_cadastro)
	values('Maria Oliveira','maria@email.com','(21) 99654-3210','2023-02-20');
	insert into cliente(nome,email,telefone,data_cadastro)
	values('Pedro Souza','pedro@email.com','(31) 99765-1234','2023-03-10');
	insert into cliente(nome,email,telefone,data_cadastro)
	values('Ana Costa','ana@email.com','(41) 98888-9999','2023-04-25');
	insert into cliente(nome,email,telefone,data_cadastro)
	values('Lucas Almeida','lucas@email.com','(61) 99123-4567','2023-05-30');

	insert into servico(nome,descricao,preco,tipo_servico)
	values('Consltoria em TI','Consultoria especilizada em infraestrutura de TI',500.00,'Consultoria'),
	('Manutenção de Equipamentos','Manutenção preventiva e corretiva de equipamentos',300.00,'Manutenção'),
	('Suporte Técnico','Suporte remoto e presencial para empresas',200.00,'Suporte');

	insert into tecnico(nome,especialidade,data_contratacao)
	values('Carlos Oliveira','Consultoria TI','2022-10-10'),
	('Fernanda Souza', 'Manutenção','2021-06-15'),
	('Suporte Técnico','Suporte Técnico','2020-08-20');
	
	insert into chamado(cliente_id,tecnico_id,servico_id,data_chamado,status,descricao)
	values(1,1,1,'2023-06-01','Pendente','Reparo no cabeamento de rede'),
	(2,2,2,'2023-07-10','Em Andamento','Manutenção no Servidor'),
	(3,3,3,'2023-08-05','Finalizado','Configuração de software empresarial'),
	(4,1,2,'2023-09-20','Pendente','Manutenção de Computador'),
	(5,2,1,'2023-10-15','Finalizado','Suporte para software');

	insert into pagamento(cliente_id,chamado_id,valor_pago,data_pagamento,forma_pagamento)
	values(1,1,350.00,'2023-06-15','Pix'),
	(2,2,450.00,'2023-07-15','Pix'),
	(3,3,600.00,'2023-08-10','Pix'),
	(4,4,300.00,'2023-09-25','Pix');

	-- Selecionando os dados

	-- Questão 4
	select nome,email from cliente;

	-- Questão 5
	select nome,preco from servico;

	-- Questão 6
	select nome,especialidade from tecnico;

	-- Questão 7
	select descricao,status from chamado;

	-- Questão 8
	select valor_pago,data_pagamento from pagamento;

	-- Questão 9
	select cliente.nome,chamado.descricao from cliente join chamado on cliente.id = chamado.cliente_id where chamado.status = 'Em Andamento';	

	-- Questão 10
	select nome, especialidade from tecnico where especialidade = 'Manutenção';

	-- Questão 11
	select cliente.nome,pagamento.valor_pago from cliente join pagamento on cliente.id = pagamento.cliente_id;

	-- Questão 12
	select cliente.nome,servico.tipo_servico,servico.descricao from cliente join servico on cliente.id = servico.id;

	-- Questão 13
	select tecnico.nome,servico.preco,servico.nome from tecnico join servico on tecnico.id = servico.id where preco > 400;

	-- Questão 14
	select preco
	from servico;
	update servico
	set preco = 350.00
	where tipo_servico = 'Manutenção' and preco < 350.00;

	-- Questão 15
	select * from tecnico 
	where id not in (select distinct tecnico_id from chamado);

	select * from servico;
	select * from tecnico;
	select * from pagamento;
	