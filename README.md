# PostgreSQL

## Comandos PostgreSQL

Acessar banco de dados

```sql
psql nome_do_banco
```
Sair do banco de dados

```sql
\q
```
Acessar banco de dados de forma remota

```sql
psql -h localhost -p 5432 -U meu_usuario -d meu_banco
```

Criar usuario quando tiver acessando o banco de dados

# Base-de-dados

## AWS RDS PostgreSQL

O comando para conectar um banco de dados chamado loja_chocode em uma instância de banco de dados PostgreSQL chamada chocode usando credenciais fictícias, com o usuario postgres.

```shell
psql --host=chocode.c6c8mwvfdgv0.us-west-2.rds.amazonaws.com --port=5432 --username=postgres --password --dbname=loja_chocode
```
Em seguida vai pedir para digitar o password.


## Criando as tabelas

```sql
CREATE TABLE cliente (
	id serial PRIMARY KEY,
	name VARCHAR ( 50 ) NOT NULL,
	email VARCHAR ( 255 ) UNIQUE NOT NULL
);

CREATE TABLE produto (
	id serial PRIMARY KEY,
	nome VARCHAR ( 50 ) NOT NULL,
	descricao TEXT NOT NULL,
	quantidade INTEGER NOT NULL,
	data_cadastro DATE,
	preco DECIMAL NOT NULL
);

CREATE TABLE pedido (
	id serial PRIMARY KEY,
	id_cliente INTEGER,
	data_pedido DATE,
	FOREIGN KEY(id_cliente) REFERENCES cliente(id)
);


CREATE TABLE pedido_produto (
	id serial PRIMARY KEY,
	id_pedido INTEGER,
	id_produto INTEGER,
	quantidade INTEGER,
	preco DECIMAL,
	FOREIGN KEY (id_pedido) REFERENCES pedido(id),
	FOREIGN KEY (id_produto) REFERENCES produto(id)
);
```

## Inserindo os valores

```sql
INSERT INTO cliente(name, email)
	VALUES('Danieli', 'Danieli@email.com'), ('Danilo', 'danilo@email.com'), ('Claudia', 'claudia@email.com'), ('Rafaella', 'rafa@email.com');

INSERT INTO produto(nome, descricao, quantidade, data_cadastro, preco)
	VALUES('mesa', 'mesa em MDF (120x70cm)', 14, '2022-05-04', 588.54), ('mouse', 'mouse gamer, cor: preto e vermelho', 24, '2022-05-04', 58.54), ('teclado', 'teclado gamer, cor: preto e azul, com leds', 57, '2022-05-04', 168.45);

INSERT INTO pedido(id_cliente, data_pedido)
	VALUES('2', '2022-05-05'), ('1', '2022-05-06'), ('4', '2022-05-05'), ('2', '2022-05-07'), ('3', '2022-05-05');

INSERT INTO pedido_produto(id_pedido, id_produto, quantidade, preco)
	VALUES(1, 2, 1, 58.54), (1, 3, 1, 168.45), (2, 1, 1, 588.54), (2, 3, 1, 168.45), (3, 1, 1, 588.54), (3, 2, 1, 58.54), (3, 3, 1, 168.45), (4, 2, 1, 58.54), (5, 1, 1, 588.54), (5, 3, 1, 168.45);
```

## Construindo um SELECT com INNER JOIN e ORDER BY.

```sql
SELECT c.name AS cliente, pe.id AS id_do_pedido, p.nome, pp.quantidade AS quantidade_item, pp.preco AS preco_do_item, (pp.quantidade * pp.preco) AS total_item
	FROM cliente c
	INNER JOIN pedido pe ON c.id = pe.id_cliente
	INNER JOIN pedido_produto pp ON pe.id = pp.id_pedido
	INNER JOIN produto p ON pp.id_produto = p.id
	ORDER BY id_do_pedido;
```

