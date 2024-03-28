# Primeiro projeto lógico de banco de dados

Este projeto cria um esquema de banco de dados para um cenário de E-commerce. No arquivo [Schema-e-commerce](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Schema-e-commerce.sql) é fornecido é um conjunto de instruções SQL que cria as tabelas necessárias para um banco de dados destinado a um cenário de E-commerce onde são armazenadas e relacionadas as informações sobre clientes, produtos, pedidos, fornecedores e vendedores, de maneira estruturada para suportar as operações do negócio. No arquivo [Insertion](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Insertion.sql) é fornecido um conjunto de instruções SQL que realiza a inserção de dados em várias tabelas de um banco de dados relacionais e executa consultas para verificar os dados inseridos. Isso permite testar e verificar o funcionamento do banco de dados e das relações entre as tabelas criandas no arquivos [Schema-e-commerce](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Schema-e-commerce.sql). No arquivo [Search](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Search.sql) é fornecido um conjunto de instruções SQL que realiza uma série de consultas SQL para realizar várias operações em um banco de dados.

## Descrição da funcionalidade

### [Schema e-commerce](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Schema-e-commerce.sql)
    
    1. Criação do banco de dados:
      - Create database ecommerce: Está instrução cria o banco de dados chamado "ecommerce".
      - use ecommerce: Esta instrução define o banco de dados "ecommerce" como o banco de dados atual, permitindo a criação das tabelas para armazenamento dos dados.
    2. Criação das tabelas: 
      - create table clients: Esta instrução cria a tabela clientes conformes as definições do tipo de dados a ser armazenado e do tamanho do campo. Nesta tabela são inseridas informações dos clientes como Nome, sobrenome, endereço entre outras que se fizerem necessarias.
      - create product: Esta instrução cria a tabela produtos que será responsável por armazenar informações dos produtos disponíveis para venda, incluindo nome, categoria e avaliação.
      - create payments: Esta instrução cria a tabela pagamentos para armazenar as informações sobre os pagamentos associados a cada cliente. 
      - create orders: Esta instrução cria a tabela ordens para armazenar as informações sobre os pedidos feitos pelos clientes, incluindo status do pedido e descrição.
      - create productStorage: Esta instrução cria a tabela produtosEstoque para armazenar as informações sobre o estoque de produtos, incluindo localização e quantidade disponível.
      - create supplier: Esta instrução cria a tabela fornrcedores para armazenar as informações sobre os fornecedores dos produtos, incluindo CNPJ e endereço.
      - create seller: Esta instrução cria a tabela vendedores para armazenar informações, como CPF e endereço dos vendedores.
      - create productSeller: Esta instrução cria a tabela produtoVendedor que tem por finalidade relacionar produtos aos vendedores, especificando a quantidade disponível.
      - create productOrder: Esta instrução cria a tabela  produtoPedidos que tem por finalidade relacionar produtos aos pedidos, especificando a quantidade e o status.
      - create storageLocation: Esta instrução cria a tabela lojasLocalização que tem por finalidade relacionar produtos às localizações de armazenamento no estoque.
      - create productSupplier: Esta instrução cria a tabela produtoSuprimento que tem por finalidade relacionar produtos aos fornecedores, especificando a quantidade fornecida.
    3. Restrições e chaves Estrangeiras: Exite ainda restrições como "constraint unique_cpf_client unique (CPF)" que cria uma restrição que impede o cadastramento de um CPF em duplicidade. Existe ainda outras restrições e chaves estrangeiras para garantir a integridade dos dados, como chaves primárias, chaves únicas e chaves estrangeiras que estabelecem relações entre as tabelas.

    Em resumo, o código do arquivo Schema-e-commerce cria um esquema de banco de dados para um cenário de e-commerce, onde informações sobre clientes, produtos, pedidos, fornecedores e vendedores são armazenadas e relacionadas de maneira estruturada para suportar as operações do negócio.
   
### [Insertion](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Insertion.sql)

    1. Inserção de dados nas tabelas criada.
      - insert into clients: Esta instrução insere na tabela clientes informações como nome, CPF e endereço.
      - insert into product: Esta instrução insere na tabela produtos informações como nome, categoria e avaliação dos produtos.
      - insert into orders:  Esta instrução insere na tabela de pedidos informações como cliente associado aos pedido, status do pedido e descrição.
      - insert into productOrder: Esta instrução insere na tabela produtosPedidos registros associando produtos aos pedidos, especificando a quantidade e o status.
      - insert into productStorage: Esta instrução insere na tabela produtosEstoque registros relacionados ao armazenamento de produtos, especificando a localização e a quantidade disponível.
      - insert into storageLocation: Esta instrução insere na tabela lojasLocalização registros relacionando produtos a locais de armazenamento específicos.
      - insert into productSupplier: Esta instrução insere na tabela produtoSuprimento registros associando produtos aos fornecedores, especificando a quantidade fornecida.
      - insert into seller: Esta instrução insere na tabela vendedor registros dos vendedores com informações como CPF e endereço.
      - insert into productSeller: Esta instrução insere na tabela produtoVendedor registros associando produtos aos vendedores, especificando a quantidade disponível.

### [Search](https://github.com/Sanderfn/Primeiro-projeto-logico-de-banco-de-dados/blob/main/Search.sql)

    1. Criação das consultas no banco de dados:
      - select count(*) from Clients: Esta consulta retorna o número total de clientes na tabela "Clients".
      - select count(*) from product;: Esta consulta retorna o número total de produtos na tabela "product".
      - select count(*) from orders;: Esta consulta retorna o número total de pedidos na tabela "orders".
      - select concat(Pname,' ',Lname) as Client, idOrder as Request, orderStatus as Status from clients c, orders o where c.idclients = idOrderClient;: Esta consulta une as tabelas "clients" e "orders" e retorna o nome completo do cliente (unificando o nome e sobrenome), o número do pedido e o status do pedido.
      - select * from clients c, orders o where c.idclients = idOrderClient group by idOrder;: Esta consulta retorna uma relação onde a identificação do cliente é igual à identificação do pedido do cliente. Ela une as tabelas "clients" e "orders" e agrupa os resultados pelo ID do pedido.
      - select c.idClients, Pname, count(*) as Number_of_orders from clients c inner join orders o on c.idclients = o.idOrderClient inner join productOrder p on p.idPOorder = o.idOrder group by idClients;: Esta consulta retorna o número de pedidos realizados por cada cliente. Ela une as tabelas "clients", "orders" e "productOrder", contabiliza o número de pedidos para cada cliente e agrupa os resultados pelo ID do cliente.
      

   
