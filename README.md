# Primeiro projeto lógico de banco de dados

## 1. Descrição do projeto

Este desafio de projeto consiste na criação de um projeto lógico de banco de dados para um cenário de e-commerce, com o objetivo de gerenciar clientes, produtos, pedidos, pagamentos, estoque, fornecedores e vendedores. Durante a modelagem, foi realizada a implementação de chaves primárias, chaves estrangeiras e constraints presente no cenário modelado. O projeto contempla o Script SQL para a criação do banco de dados relacional. Posteriormente, por meio de Scripts, os dados devem ser persistidos dados para a realização de testes, utilizando queries simples e complexas. As queries criadas devem possuir as seguintes cláusulas.

- Recuperação de informações simples com Select Statment
- Filtros com WHERE Statement
- Expressões para gerar atributos derivados
- Definir ordenação dos dados com Order By
- aplicação de condições de filtro aos grupos Having Statement
- Criação de junções entre tabelas para fornecer uma perspectivas mais complexas dos dados

## 2. Modelo Entidade Relacional
- Imagem Modelo Entidade Relacional e-commerce
![Diagrama de Relacionamento](https://github.com/FredericoSander/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Imagens/E-commerce.png)

## 3. Tabelas do Banco de Dados

### 3.1 **Tabela `clients`** (Clientes)

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


**Exemplo de dados:**
```sql
insert into clients (Pname, minit, lname, CPF, street, num, district, city, state, country)
    values
    ('Maria','M','Neves',12345678901,'Rua Santa Rita',29,'Jacuí','João Monlevade','MG','Brasil'),
    ('Antônio','A','Souza',45678912345,'Rua Juazeiro',100,'Satelite','João Monlevade','MG','Brasil'),
    ('Joaquim','J','Cruz',78912345678,'Rua Parauna',78,'Castelo','João Monlevade','MG','Brasil'),
    ('José','J','Silva',78456952135,'Rua Jequitibá',62,'Santa Barbara','João Monlevade','MG','Brasil');

```

---

### 3.2 **Tabela `product`** (Produtos)

A tabela `product` armazena informações sobre os produtos disponíveis para venda no sistema, como nome, categoria, avaliação e tamanho.

**Estrutura:**
- **Idproduct** (PK) - Identificador único do produto (auto-increment).
- **Pname** - Nome do produto.
- **Classification_Kids** - Indica se o produto é voltado para crianças.
- **Category** - Categoria do produto (enum: 'Eletronic', 'Clothes', 'Toys', 'Furniture', 'Books').
- **Avaliation** - Avaliação média do produto.
- **size** - Tamanho do produto.

**Exemplo de dados:**

```sql
insert into product (Pname, classification_Kids, Category, Avaliation, size)
    values
    ('CelularA03', false, 'Eletronic', 4, null),
    ('Nintendo', true, 'Toys', 3, null),
    ('NoteBook', false, 'Eletronic', 4, null),
    ('Camiseta', false, 'Clothes', 4, null),
    ('A Indomada', false, 'Books', 4, null);
```
---

### 3.3 **Tabela `payments`** (Pagamentos)

A tabela `payments` armazena as informações sobre os pagamentos dos clientes.

**Estrutura:**
- **idclient** (FK) - Relaciona ao cliente.
- **idpayment** (PK) - Identificador único de pagamento.
- **TypePayment** - Tipo de pagamento (enum: 'Cartão', 'Dois cartões').
- **limiteAvailable** - Limite disponível para o pagamento.

**Chave Primária Composta:**
- `idclient`, `idpayment`.

---

### 3.4 **Tabela `orders`** (Pedidos)

A tabela `orders` registra os pedidos feitos pelos clientes, incluindo status, descrição, valores de envio e pagamento.

- **IdOrder** (PK) - Identificador único do pedido (auto-increment).
- **IdOrderClient** (FK) - Relaciona o pedido ao cliente.
- **orderStatus** - Status do pedido (enum: 'Cancelado', 'Confirmado', 'Em processamento').
- **orderDescription** - Descrição do pedido.
- **sendValue** - Valor do envio.
- **paymentCash** - Indica se o pagamento foi feito em dinheiro.

**Relacionamento:**
- Relaciona-se com `clients(idClients)`.

**Exemplo de dados:**

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

### 3.5 **Tabela `productStorage`** (Estoque de Produtos)

A tabela `productStorage` mantém informações sobre a quantidade de cada produto armazenado em diferentes locais.

**Estrutura:**
- **idProductStorage** (PK) - Identificador único do estoque (auto-increment).
- **storageLocation** - Localização do armazenamento do produto.
- **quantity** - Quantidade disponível do produto.

**Exemplo de dados:**

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
---

### 3.6 **Tabela `supplier`** (Fornecedores)

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

**Exemplo de dados:**

```sql
insert into supplier (storageLocation, corporateName, SocialName, CNPJ, contact, street, num, District, city, state, country)
values
    ('São Paulo', 'Limeira LTDA', null, 31999991234, 31898888141, 'Jacuí', 15, 'Jacuí', 'São Paulo', 'SP', 'Brasil'),
    ('Curitiba', 'Laranja LTDA', null, 222222222222222, 11111111111, 'Jequitibá', 23, 'palmeiras', 'Curitiba', 'SC', 'Brasil'),
    ('Rio de Janeiro', 'Jurubeba LTDA', null, 111111111111111, 31999991234, 'California', 45, 'Leblon', 'Rio de Janeiro', 'RJ', 'Brasil');
```
---

### 3.7 **Tabela `seller`** (Vendedores)

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

**Exemplo de dados:**

```sql
insert into seller(CorporateName, SocialName, CNPJ, CPF, contact, RegistrationDate, street, num, District, city, state, country)
values
    ('Limeira LTDA', 'Limeira LTDA', 123456789000152, null, 31999981234, '2020-01-23', 'Jacuí', 15, 'Jacuí', 'São Paulo', 'SP', 'Brasil'),
    ('Laranja LTDA', 'Laranja LTDA', 456789123000174, null, 15985214576, '2022-04-23', 'Jequitibá', 23, 'palmeiras', 'Curitiba', 'SC', 'Brasil'),
    ('João Antunes', 'João Antunes', null, 85214796345, 31999991234, '2015-05-23', 'California', 45, 'Leblon', 'Rio de Janeiro', 'RJ', 'Brasil');
```
---

### 3.8 **Tabela `productSeller`** (Produtos do Vendedor)

A tabela `productSeller` registra os produtos que estão sendo vendidos por cada vendedor, junto com a quantidade de cada produto disponível.

**Estrutura:**
- **IdPSeller** (FK) - Identificador do vendedor.
- **IdPproduct** (FK) - Identificador do produto.
- **prodquantity** - Quantidade do produto disponível no vendedor.

**Chave Primária Composta:**
- `IdPSeller`, `IdPproduct`.

**Exemplo de dados:**

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
---

### 3.9 **Tabela `productOrder`** (Produtos do Pedido)

A tabela `productOrder` relaciona os produtos aos pedidos.

**Estrutura:**
- **IdPOproduct** (FK) - Identificador do produto.
- **IdPOorder** (FK) - Identificador do pedido.
- **poQuantity** - Quantidade do produto no pedido.
- **poStatus** - Status do produto no pedido (enum: 'Disponível', 'Sem estoque').

**Chave Primária Composta:**
- `IdPOproduct`, `IdPOorder`.

**Exemplo de dados:**

```sql
insert into productOrder(idPOproduct, idPOorder, poQuantity, poStatus) 
values
    (1, 15, 1, null),
    (2, 15, 1, null),
    (3, 16, 1, null);
```
---

### 3.10 **Tabela `storageLocation`** (Localização de Armazenamento de Produtos)

A tabela `storageLocation` mantém a relação entre os produtos e seus respectivos locais de armazenamento.

**Estrutura:**
- **idLproduct** (FK) - Identificador do produto.
- **idLstorage** (FK) - Identificador do local de armazenamento.
- **location** - Localização do armazenamento.

**Chave Primária Composta:**
- `idLproduct`, `idLstorage`.

**Exemplo de dados:**

```sql
insert into storageLocation (idLproduct,idLstorage,location) values
			(1,1,'RJ'),
            (2,3,'SP');
```
---

### 3.11 **Tabela `productSupplier`** (Fornecimento de Produtos)

A tabela `productSupplier` mantém a relação entre os produtos e seus respectivos fornecedores.

**Estrutura:**
- **idPsSupplier** (FK) - Identificador do fornecedor.
- **idPsProduct** (FK) - Identificador do produto.
- **quantity** - Quantidade do produto fornecido.

**Chave Primária Composta:**
- `idPsSupplier`, `idPsProduct`.

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
---

## 4. Relacionamentos entre as Tabelas

- **`clients`** e **`orders`**: Relacionamento de um para muitos, onde um cliente pode ter vários pedidos.
- **`clients`** e **`payments`**: Relacionamento de um para muitos, onde um cliente pode ter vários pagamentos.
- **`orders`** e **`productOrder`**: Relacionamento de um para muitos, onde um pedido pode conter vários produtos.
- **`product`** e **`productOrder`**: Relacionamento de um para muitos, onde um produto pode aparecer em vários pedidos.
- **`supplier`** e **`productSupplier`**: Relacionamento de um para muitos, onde um fornecedor pode fornecer vários produtos.
- **`seller`** e **`productSeller`**: Relacionamento de um para muitos, onde um vendedor pode vender vários produtos.
- **`product`** e **`storageLocation`**: Relacionamento de um para muitos, onde um produto pode ser armazenado em vários locais de estoque.
- **`product`** e **`productStorage`**: Relacionamento de um para um, onde cada produto tem um único estoque associado.

---

## 5. Consultas simples para validação do banco de dados

### 5.1 Quantos clientes existem na base de dados?

```sql
select count(*) from clients;
```

### 5.2 Quantos produtos existem na base de dados?

```sql
select count(*) from product;
```

### 5.3 Quantos pedidos estão cadastrados?

```sql
select count(*) from orders;
```

### 5.4  Quantos pedidos foram realizados por cada cliente?

```sql
select c.idClients, Pname, count(*) as Number_of_orders 
from clients c
inner join orders o on c.idclients = o.idOrderClient
inner join productOrder p on p.idPOorder = o.idOrder
group by idClients;
```

## 6. Considerações Finais

Este banco de dados foi projetado para gerenciar as operações de um e-commerce, possibilitando a administração de clientes, fornecedores, vendedores, produtos, pedidos, pagamentos e estoque. O modelo foi estruturado para otimizar as consultas e facilitar a integração com outras partes do sistema.

## Autor

- [Frederico S N Cota](https://github.com/Sanderfn)