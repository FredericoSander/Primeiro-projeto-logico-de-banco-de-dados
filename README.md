# Primeiro projeto lógico de banco de dados

## Índice 

* [Primeiro projeto lógico de banco de dados](#primeiro-projeto-lógico-de-banco-de-dados)
* [Índice](#índice)
* [Descrição do Projeto](#descrição-do-projeto)
* [Modelo Entidade Relacionamento](#modelo-entidade-relacionamento)
* [Tabelas do Banco de Dados](#tabelas-do-banco-de-dados)
* [Relacionamentos entre Tabelas](#relacionamentos-entre-tabelas)
* [Consultas SQL](consultas-sql)
* [Técnicas e tecnologias utilizadas](#técnicas-e-tecnologias-utilizadas)
* [Status do Projeto](#status-do-projeto)
* [Acesse o Projeto](#acesse-o-projeto)
* [Conclusão](#conclusão)

## Descrição do Projeto

Este desafio de projeto consiste na criação de um projeto lógico de banco de dados para um cenário de e-commerce, com o objetivo de gerenciar clientes, produtos, pedidos, pagamentos, estoque, fornecedores e vendedores. Durante a modelagem, foi realizada a implementação de chaves primárias, chaves estrangeiras e constraints presente no cenário modelado. O projeto contempla o Script SQL para a criação do banco de dados relacional. Posteriormente, por meio de Scripts, os dados devem ser persistidos dados para a realização de testes, utilizando queries simples e complexas. As queries criadas devem possuir as seguintes cláusulas.

- Recuperação de informações simples com Select Statment.
- Filtros com WHERE Statement.
- Expressões para gerar atributos derivados.
- Definir ordenação dos dados com Order By.
- aplicação de condições de filtro aos grupos Having Statement.
- Criação de junções entre tabelas para fornecer uma perspectivas mais complexas dos dados.

## Modelo Entidade Relacionamento
- Imagem Modelo Entidade Relacional e-commerce
![Diagrama de Relacionamento](https://github.com/FredericoSander/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Imagens/E-commerce.png)

## Tabelas do Banco de Dados

### **Tabela `clients`** (Clientes)

A tabela `clients` armazena as informações dos clientes do sistema, como nome, CPF, endereço e dados de localização.

**Estrutura:**
- **idClients** (PK) - Identificador único do cliente (auto-increment).
- **Pname** - Primeiro nome do cliente.
- **Minit** - Inicial do nome do meio do cliente.
- **Lname** - Sobrenome do cliente.
- **CPF** - CPF do cliente (campo único e não nulo).
- **Street** - Rua do endereço do cliente.
- **Num** - Número do endereço do cliente.
- **District** - Bairro do cliente.
- **City** - Cidade do cliente.
- **State** - Estado do cliente.
- **Country** - País do cliente.

**Restrição:**
- `unique_cpf_client`: CPF único.

**Comando SQL para criação da tabela**
```sql
create table clients(
	idClients int auto_increment primary key,
    Pname varchar(10),
    Minit char(3),
    Lname varchar(20),
    CPF char(11) not null,
    Street varchar(45),
    Num int not null,
    District varchar(45),
    City varchar(15),
    State Varchar(10),
    Country Varchar(10),
    constraint unique_cpf_client unique (CPF)
    );
```

**Exemplo de dados a serem persistidos:**
```sql
insert into clients (Pname, minit, lname, CPF, street, num, district, city, state, country)
    values
    ('Maria','M','Neves',12345678901,'Rua Santa Rita',29,'Jacuí','João Monlevade','MG','Brasil'),
    ('Antônio','A','Souza',45678912345,'Rua Juazeiro',100,'Satelite','João Monlevade','MG','Brasil'),
    ('Joaquim','J','Cruz',78912345678,'Rua Parauna',78,'Castelo','João Monlevade','MG','Brasil'),
    ('José','J','Silva',78456952135,'Rua Jequitibá',62,'Santa Barbara','João Monlevade','MG','Brasil');

```


### **Tabela `product`** (Produtos)

A tabela `product` armazena informações sobre os produtos disponíveis para venda no sistema, como nome, categoria, avaliação e tamanho.

**Estrutura:**
- **Idproduct** (PK) - Identificador único do produto (auto-increment).
- **Pname** - Nome do produto.
- **Classification_Kids** - Indica se o produto é voltado para crianças.
- **Category** - Categoria do produto (enum: 'Eletronic', 'Clothes', 'Toys', 'Furniture', 'Books').
- **Avaliation** - Avaliação média do produto.
- **size** - Tamanho do produto.

**Comando SQL para criação da tabela**
```sql
create table product(
	Idproduct int auto_increment primary key,
    Pname varchar(10) not null,
    Classification_Kids bool default false,
    Category enum('Eletronic','Clothes','Toys','Furniture','Books') not null,
	Avaliation float default 0,
	size varchar(10)
);

```

**Exemplo de dados a serem persistidos:**

```sql
insert into product (Pname, classification_Kids, Category, Avaliation, size)
    values
    ('CelularA03', false, 'Eletronic', 4, null),
    ('Nintendo', true, 'Toys', 3, null),
    ('NoteBook', false, 'Eletronic', 4, null),
    ('Camiseta', false, 'Clothes', 4, null),
    ('A Indomada', false, 'Books', 4, null);
```


### **Tabela `payments`** (Pagamentos)

A tabela `payments` armazena as informações sobre os pagamentos dos clientes.

**Estrutura:**
- **idclient** (FK) - Relaciona ao cliente.
- **idpayment** (PK) - Identificador único de pagamento.
- **TypePayment** - Tipo de pagamento (enum: 'Cartão', 'Dois cartões').
- **limiteAvailable** - Limite disponível para o pagamento.

**Chave Primária Composta:**
- `idclient`, `idpayment`.

**Comando SQL para criação da tabela**
```sql
 create table payments(
	idclient int,
    idpayment int,
    TypePayment enum('Cartão','Dois cartões'),
    limiteAvailable float,
    primary key (idclient, idpayment)
  );
```

### **Tabela `orders`** (Pedidos)

A tabela `orders` registra os pedidos feitos pelos clientes, incluindo status, descrição, valores de envio e pagamento.

- **IdOrder** (PK) - Identificador único do pedido (auto-increment).
- **IdOrderClient** (FK) - Relaciona o pedido ao cliente.
- **orderStatus** - Status do pedido (enum: 'Cancelado', 'Confirmado', 'Em processamento').
- **orderDescription** - Descrição do pedido.
- **sendValue** - Valor do envio.
- **paymentCash** - Indica se o pagamento foi feito em dinheiro.

**Relacionamento:**
- Relaciona-se com `clients(idClients)`.

**Comando SQL para criação da tabela**
```sql
 create table orders(
	IdOrder int auto_increment primary key,
    IdOrderClient int,
    orderStatus enum('Cancelado','Confirmado','Em processamento') default 'Em processamento',
    orderDescription varchar(255),
    sendValue float default 10,
    paymentCash bool default false,
    constraint fk_order_client foreign key (IdOrderClient) references clients(idClients)
	on update cascade
 );
```

**Exemplo de dados a serem persistidos:**
```sql
insert into orders (idOrderClient, orderStatus, orderDescription, sendValue, paymentCash) 
values
    (1, default, 'compra via aplicativo', null, 1),
    (2, default, 'compra via aplicativo', 50, 1),
    (3, 'Confirmado', null, null, 1),
    (4, default, 'compra via site', 150, 0),
    (1, 'Confirmado', 'compra via aplicativo', 100, 0),
    (2, 'Confirmado', 'compra via aplicativo', 50, 1),
    (3, 'Confirmado', null, null, 1),
    (3, default, 'compra via site', 500, 0);
```

### **Tabela `productStorage`** (Estoque de Produtos)

A tabela `productStorage` mantém informações sobre a quantidade de cada produto armazenado em diferentes locais.

**Estrutura:**
- **idProductStorage** (PK) - Identificador único do estoque (auto-increment).
- **storageLocation** - Localização do armazenamento do produto.
- **quantity** - Quantidade disponível do produto.

**Comando SQL para criação da tabela**
```sql
   create table productStorage(
	idProductStorage int auto_increment primary key,
    storageLocation varchar (255),
    quantity int default 0
);
```

**Exemplo de dados a serem persistidos:**
```sql
insert into productStorage (storageLocation, quantity)
values
    ('São Paulo', 1000),
    ('Rio de Janeiro', 200),
    ('Belo Horizonte', 500),
    ('Curitiba', 100),
    ('Curitiba', 550),
    ('São Paulo', 20);
```

### **Tabela `supplier`** (Fornecedores)

A tabela `supplier` armazena as informações dos fornecedores dos produtos, como nome, CNPJ, contato e endereço.

**Estrutura:**
- **IdSupplier** (PK) - Identificador único do fornecedor (auto-increment).
- **storageLocation** - Localização do fornecedor.
- **corporateName** - Razão social do fornecedor.
- **SocialName** - Nome fantasia do fornecedor.
- **CNPJ** - CNPJ do fornecedor (campo único e não nulo).
- **contact** - Contato telefônico do fornecedor.
- **Street** - Rua do endereço do fornecedor.
- **Num** - Número do endereço do fornecedor.
- **District** - Bairro do fornecedor.
- **City** - Cidade do fornecedor.
- **State** - Estado do fornecedor.
- **Country** - País do fornecedor.

**Restrição:**
- `unique_supplier`: CNPJ único.

**Comando SQL para criação da tabela**
```sql
create table supplier(
	IdSupplier int auto_increment primary key,
    storageLocation varchar (255),
    corporateName varchar (255) not null,
    SocialName varchar(255),
    CNPJ char(15) not null,
    contact varchar (11) not null,    
	Street varchar(45)not null,
    Num int not null,
    District varchar(45)not null,
    City varchar(15)not null,
    State Varchar(10)not null,
    Country Varchar(10)not null,
    constraint unique_supplier unique (CNPJ)
);

```

**Exemplo de dados a serem persistidos:**
```sql
insert into supplier (storageLocation, corporateName, SocialName, CNPJ, contact, street, num, District, city, state, country)
values
    ('São Paulo', 'Limeira LTDA', null, 31999991234, 31898888141, 'Jacuí', 15, 'Jacuí', 'São Paulo', 'SP', 'Brasil'),
    ('Curitiba', 'Laranja LTDA', null, 222222222222222, 11111111111, 'Jequitibá', 23, 'palmeiras', 'Curitiba', 'SC', 'Brasil'),
    ('Rio de Janeiro', 'Jurubeba LTDA', null, 111111111111111, 31999991234, 'California', 45, 'Leblon', 'Rio de Janeiro', 'RJ', 'Brasil');
```


### **Tabela `seller`** (Vendedores)

A tabela `seller` armazena informações sobre os vendedores, incluindo nome, CPF, CNPJ, e dados de contato.

**Estrutura:**
- **IdSeller** (PK) - Identificador único do vendedor (auto-increment).
- **CorporateName** - Razão social do vendedor.
- **SocialName** - Nome fantasia do vendedor.
- **CNPJ** - CNPJ do vendedor.
- **CPF** - CPF do vendedor.
- **contact** - Contato telefônico do vendedor.
- **RegistrationDate** - Data de registro do vendedor.
- **Street** - Rua do endereço do vendedor.
- **Num** - Número do endereço do vendedor.
- **District** - Bairro do vendedor.
- **City** - Cidade do vendedor.
- **State** - Estado do vendedor.
- **Country** - País do vendedor.

**Restrições:**
- `unique_CNPJ_supplier`: CNPJ único.
- `unique_CPF_supplier`: CPF único.

**Comando SQL para criação da tabela**
```sql
create table seller(
	IdSeller int auto_increment primary key,
    CorporateName varchar (255),
    SocialName varchar(255) not null,
    CNPJ char(15),
    CPF char(11),
    contact varchar (11), 
    RegistrationDate date,
	Street varchar(45),
    Num int not null,
    District varchar(45),
    City varchar(15),
    State Varchar(10),
    Country Varchar(10),
    constraint unique_CNPJ_supplier unique (CNPJ),
    constraint unique_CPF_supplier unique (CPF)
);
```

**Exemplo de dados a serem persistidos:**
```sql
insert into seller(CorporateName, SocialName, CNPJ, CPF, contact, RegistrationDate, street, num, District, city, state, country)
values
    ('Limeira LTDA', 'Limeira LTDA', 123456789000152, null, 31999981234, '2020-01-23', 'Jacuí', 15, 'Jacuí', 'São Paulo', 'SP', 'Brasil'),
    ('Laranja LTDA', 'Laranja LTDA', 456789123000174, null, 15985214576, '2022-04-23', 'Jequitibá', 23, 'palmeiras', 'Curitiba', 'SC', 'Brasil'),
    ('João Antunes', 'João Antunes', null, 85214796345, 31999991234, '2015-05-23', 'California', 45, 'Leblon', 'Rio de Janeiro', 'RJ', 'Brasil');
```

### **Tabela `productSeller`** (Produtos do Vendedor)

A tabela `productSeller` registra os produtos que estão sendo vendidos por cada vendedor, junto com a quantidade de cada produto disponível.

**Estrutura:**
- **IdPSeller** (FK) - Identificador do vendedor.
- **IdPproduct** (FK) - Identificador do produto.
- **prodquantity** - Quantidade do produto disponível no vendedor.

**Chave Primária Composta:**
- `IdPSeller`, `IdPproduct`.

**Comando SQL para criação da tabela**
```sql
create table productSeller(
	IdPSeller int,
    IdPproduct int,
    prodquantity int default 1,
    primary key(IdPSeller, IdPproduct),
    constraint fk_product_seller foreign key (IdPSeller) references Seller(idSeller),
    constraint fk_product_product foreign key (IdPproduct) references product(Idproduct)
);
```

**Exemplo de dados a serem persistidos:**
```sql
insert into productSeller(idPSeller, idPproduct, quantity)
values
    (1, 1, 30),
    (1, 3, 50),
    (2, 1, 30),
    (2, 2, 50),
    (3, 1, 50),
    (3, 2, 100);
```


### **Tabela `productOrder`** (Produtos do Pedido)

A tabela `productOrder` relaciona os produtos aos pedidos.

**Estrutura:**
- **IdPOproduct** (FK) - Identificador do produto.
- **IdPOorder** (FK) - Identificador do pedido.
- **poQuantity** - Quantidade do produto no pedido.
- **poStatus** - Status do produto no pedido (enum: 'Disponível', 'Sem estoque').

**Chave Primária Composta:**
- `IdPOproduct`, `IdPOorder`.

**Comando SQL para criação da tabela**
```sql
create table productOrder(
	IdPOproduct int,
    IdPOorder int,
    poQuantity int default 1,
    poStatus enum('Disponível','Sem estoque') default 'Disponível',
    primary key (IdPOproduct,IdPOorder),
    constraint fk_productorder_seller foreign key (IdPOproduct) references product(Idproduct),
	constraint fk_productorder_product foreign key (IdPOorder) references orders(IdOrder)
);
```

**Exemplo de dados a serem persistidos:**
```sql
insert into productOrder(idPOproduct, idPOorder, poQuantity, poStatus) 
values
    (1, 15, 1, null),
    (2, 15, 1, null),
    (3, 16, 1, null);
```

### **Tabela `storageLocation`** (Localização de Armazenamento de Produtos)

A tabela `storageLocation` mantém a relação entre os produtos e seus respectivos locais de armazenamento.

**Estrutura:**
- **idLproduct** (FK) - Identificador do produto.
- **idLstorage** (FK) - Identificador do local de armazenamento.
- **location** - Localização do armazenamento.

**Chave Primária Composta:**
- `idLproduct`, `idLstorage`.

**Comando SQL para criação da tabela**
```sql
create table storageLocation(
	idLproduct int,
    idLstorage int,
    location varchar (255) not null,
    primary Key (idLproduct,idLstorage),
	constraint fk_storage_location_product foreign key (idLproduct) references product(Idproduct),
	constraint fk_storage_location_storage foreign key (idLstorage) references productStorage(idProductStorage)
);
```

**Exemplo de dados a serem persistidos:**
```sql
insert into storageLocation (idLproduct,idLstorage,location) values
			(1,1,'RJ'),
            (2,3,'SP');
```


### **Tabela `productSupplier`** (Fornecimento de Produtos)

A tabela `productSupplier` mantém a relação entre os produtos e seus respectivos fornecedores.

**Estrutura:**
- **idPsSupplier** (FK) - Identificador do fornecedor.
- **idPsProduct** (FK) - Identificador do produto.
- **quantity** - Quantidade do produto fornecido.

**Chave Primária Composta:**
- `idPsSupplier`, `idPsProduct`.

**Comando SQL para criação da tabela**
```sql
create table productSupplier(
	idPsSupplier int,
    idPsProduct int,
    quantity int not null,
    primary key(idPsSupplier,idPsProduct),
    constraint fk_product_supplier_supplier foreign key (idPsSupplier) references supplier(idSupplier),
    constraint fk_product_supplier_product foreign key (idPsProduct) references product(idProduct)
 );
```

**Exemplo de dados:**

```sql
insert into productSupplier(idPsSupplier, idPsProduct, quantity) 
values
    (1, 1, 500),
    (1, 2, 400),
    (2, 4, 633),
    (3, 3, 5),
    (2, 5, 10);
```

## Relacionamentos entre Tabelas

- **`clients`** e **`orders`**: Relacionamento de um para muitos, onde um cliente pode ter vários pedidos.
- **`clients`** e **`payments`**: Relacionamento de um para muitos, onde um cliente pode ter vários pagamentos.
- **`orders`** e **`productOrder`**: Relacionamento de um para muitos, onde um pedido pode conter vários produtos.
- **`product`** e **`productOrder`**: Relacionamento de um para muitos, onde um produto pode aparecer em vários pedidos.
- **`supplier`** e **`productSupplier`**: Relacionamento de um para muitos, onde um fornecedor pode fornecer vários produtos.
- **`seller`** e **`productSeller`**: Relacionamento de um para muitos, onde um vendedor pode vender vários produtos.
- **`product`** e **`storageLocation`**: Relacionamento de um para muitos, onde um produto pode ser armazenado em vários locais de estoque.
- **`product`** e **`productStorage`**: Relacionamento de um para um, onde cada produto tem um único estoque associado.

---

## Consultas SQL

### Quantos clientes existem na base de dados?

```sql
select count(*) from clients;
```

### Quantos produtos existem na base de dados?

```sql
select count(*) from product;
```

### Quantos pedidos estão cadastrados?

```sql
select count(*) from orders;
```

### Quantos pedidos foram realizados por cada cliente?

```sql
select c.idClients, Pname, count(*) as Number_of_orders 
from clients c
inner join orders o on c.idclients = o.idOrderClient
inner join productOrder p on p.idPOorder = o.idOrder
group by idClients;
```

## Conclusão

Este banco de dados foi projetado para gerenciar as operações de um e-commerce, possibilitando a administração de clientes, fornecedores, vendedores, produtos, pedidos, pagamentos e estoque. O modelo foi estruturado para otimizar as consultas e facilitar a integração com outras partes do sistema.

## Técnicas e tecnologias utilizadas

- ``SQL``
- ``MySQL Workbench 8.0``
- ``Modelagem de dados``
- ``Análise de dados``
- ``Git``
- ``GitHub``

## Status do Projeto

![Status do Projeto](http://img.shields.io/static/v1?label=STATUS&message=Concluído&color=GREEN&style=for-the-badge)

## Acesse o Projeto

Você pode acessar o projeto clicando [aqui](https://github.com/FredericoSander/Primeiro-projeto-logico-de-banco-de-dados/tree/main/Arquivos%20SQL)

## Autor

| [<img loading="lazy" src="https://avatars.githubusercontent.com/u/136928502?s=96&v=4" width=115><br><sub>Frederico Sander</sub>](https://github.com/FredericoSander)
| :---: | 