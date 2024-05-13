# Primeiro projeto lógico de banco de dados

## Descrição do projeto

<p align="justify"> Este desafio de projeto consiste na criação de um projeto lógico de banco de dados para um cenário de e-commerce, com o objetivo de gerenciar clientes, produtos, pedidos, pagamentos, estoque, fornecedores e vendedores. Durante a modelagem, foi realizada a implementação de chaves primárias, chaves estrangeiras e constraints presente no cenário modelado. O projeto contempla o Script SQL para a criação do banco de dados relacional. Posteriormente, por meio de Scripts, os dados devem ser persistidos dados para a realização de testes, utilizando queries simples e complexas. As queries criadas devem possuir as seguintes cláusulas.

- Recuperação de informações simples com Select Statment
- Filtros com WHERE Statement
- Expressões para gerar atributos derivados
- Definir ordenação dos dados com Order By
- aplicação de condições de filtro aos grupos Having Statement
- Criação de junções entre tabelas para fornecer uma perspectivas mais complexas dos dados

## Modelo Entidade Relacional
- Imagem Modelo Entidade Relacional e-commerce
<div aling="center">
 <img src="https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/E-commerce.png">
</div>

## [Schema e-commerce](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Schema-e-commerce.sql)
<p align="justify"> O arquivo Schema-e-commerce possui o conjunto de instruções SQL para a criação das tabelas do banco de dados destinado ao armazenamento das informações dos clientes, produtos, pedidos, fornecedores e vendedores, de maneira estruturada para suportar as operações do negócio.<p/> 

|Instrução| Descrição da instrução|
|---------|------------------|
|Create database ecommerce:|Cria o banco de dados chamado e-commerce.|
|use ecommerce| Define o banco de dados e-commerce como o banco de dados atual, permitindo a criação das tabelas para armazenamento dos dados.|
|create table product|Cria a tabela product conformes as definições do tipo de dados a ser armazenado e do tamanho do campo. Nesta tabela são inseridas informações dos clientes como Nome, sobrenome, endereço entre outras que se fizerem necessarias.|
|create table product|Cria a tabela product que será responsável por armazenar informações dos produtos disponíveis para venda, incluindo nome, categoria e avaliação.|
|create table payments|Cria a tabela payment para armazenar as informações sobre os pagamentos associados a cada cliente.|
|create table orders|Cria a tabela orders para armazenar as informações sobre os pedidos feitos pelos clientes, incluindo status do pedido e descrição.|
|create table productStorage|Cria a tabela productStorage para armazenar as informações sobre o estoque de produtos, incluindo localização e quantidade disponível.|
|create table supplier|Cria a tabela supplier para armazenar as informações sobre os fornecedores dos produtos, incluindo CNPJ e endereço.|
|create table seller|Cria a tabela seller para armazenar informações, como CPF e endereço dos vendedores.|
|create table productSeller|Cria a tabela productSeller que tem por finalidade relacionar produtos aos vendedores, especificando a quantidade disponível.|
|create table productOrder|Cria a tabela  productOrder que tem por finalidade relacionar produtos aos pedidos, especificando a quantidade e o status.|
|create table storageLocation|Cria a tabela storageLocation que tem por finalidade relacionar produtos às localizações de armazenamento no estoque.|
|create table productSupplier|Cria a tabela productSupplier que tem por finalidade relacionar produtos aos fornecedores, especificando a quantidade fornecida.|

## [Insertion](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Insertion.sql)
<p align="justify">
O arquivo Insertion fornece um conjunto de instruções SQL para realizar a persistencia de dados nas tabelas do banco de dados e-commerce, possibilitando a realização de consultas para verificar os dados persistidos. Após a persistencia, é possível testar e verificar o funcionamento do banco de dados e das relações entre as tabelas criadas pelo script Schema e-commerce.<p/>
    
|Instrução| Descrição da instrução|
|---------|------------------|
|insert into clients| Esta instrução insere na tabela clients informações como nome, CPF e endereço.|
|insert into product| Esta instrução insere na tabela product informações como nome, categoria e avaliação dos produtos.|
|insert into orders|  Esta instrução insere na tabela de orders informações como cliente associado aos pedido, status do pedido e descrição.|
|insert into productOrder| Esta instrução insere na tabela productOrder registros associando produtos aos pedidos, especificando a quantidade e o status.|
|insert into productStorage| Esta instrução insere na tabela productStorage registros relacionados ao armazenamento de produtos, especificando a localização e a quantidade disponível.|
|insert into storageLocation| Esta instrução insere na tabela storageLocation registros relacionando produtos a locais de armazenamento específicos.|
|insert into productSupplier| Esta instrução insere na tabela productSupplier registros associando produtos aos fornecedores, especificando a quantidade fornecida.|
|insert into seller| Esta instrução insere na tabela seller registros dos vendedores com informações como CPF e endereço.|
|insert into productSeller| Esta instrução insere na tabela productSeller registros associando produtos aos vendedores, especificando a quantidade disponível.|

## [Search](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Search.sql)
<p align="justify"> 
O arquivo Search fornece um conjunto de instruções SQL que realiza a recuperação dos dados persistidos no banco de dados.</p>

|Instrução| Descrição da instrução|
|---------|------------------|
|select count(*) from Clients|Esta consulta retorna o número total de clientes na tabela "Clients".|
|select count(*) from product|Esta consulta retorna o número total de produtos na tabela "product".|
|select count(*) from orders|Esta consulta retorna o número total de pedidos na tabela "orders"|
|select concat(Pname,' ',Lname) as Client, idOrder as Request, orderStatus as Status from clients c, orders o where c.idclients = idOrderClient|Esta consulta une as tabelas "clients" e "orders" e retorna o nome completo do cliente (unificando o nome e sobrenome), o número do pedido e o status do pedido.|
|select * from clients c, orders o where c.idclients = idOrderClient group by idOrder|Esta consulta retorna uma relação onde a identificação do cliente é igual à identificação do pedido do cliente. Ela une as tabelas "clients" e "orders" e agrupa os resultados pelo ID do pedido.|
|select c.idClients, Pname, count(*) as Number_of_orders from clients c inner join orders o on c.idclients = o.idOrderClient inner join productOrder p on p.idPOorder = o.idOrder group by idClients|Esta consulta retorna o número de pedidos realizados por cada cliente. Ela une as tabelas "clients", "orders" e "productOrder", contabiliza o número de pedidos para cada cliente e agrupa os resultados pelo ID do cliente.|
