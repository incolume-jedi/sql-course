# SQL GROUP BY

[Indice do curso](../README.md#curso-de-sql---structured-query-language)

---
O uso do comando SQL GROUP BY é importante quando queremos selecionar diversos registros em uma tabela e agrupá-los por um ou mais campos. No SQL, também podemos realizar uma ação específica, como descobrir a quantidades de registros selecionados pela instrução. Um exemplo de uso é para selecionarmos a quantidade vendas realizadas por uma pessoa vendedora em uma tabela de vendas.

Entretanto, é preciso atenção ao utilizar esse recurso, pois a complexidade da instrução SQL pode comprometer a performance da aplicação, principalmente quando utilizado com mais de uma tabela. Em contrapartida, sua utilização de forma correta proporciona benefícios, como mais agilidade para a realização de cálculos.

Para demonstrar como esse recurso funciona, preparamos este conteúdo com os seguintes tópicos:

- [O que é o comando SQL GROUP BY?](#o-que-é-o-comando-sql-group-by)
- [Qual a sintaxe do comando SQL GROUP BY?](#qual-a-sintaxe-do-comando-sql-group-by)
- [6 exemplos de uso do comando SQL GROUP BY](#seis-exemplos-de-uso-do-comando-sql-group-by)
- [Quais as diferenças entre o comando GROUP BY e o GROUP BY ALL?](#quais-as-diferenças-entre-o-comando-group-by-e-o-group-by-all)
- [Boas práticas ao usar o comando SQL GROUP BY](#boas-práticas-ao-usar-o-comando-sql-group-by)

## O que é o comando SQL GROUP BY?

O comando SQL GROUP BY é utilizado em conjunto com a cláusula SELECT para agrupar registros semelhantes em uma tabela. A seleção é feita de acordo os critérios definidos na cláusula WHERE, caso ela seja utilizada, e conforme o campo indicado para o agrupamento no comando GROUP BY. Sobre o resultado obtido, podemos aplicar diversas funções, entre elas:

- somatória dos valores de uma coluna: SUM();
- quantidade de registros que atenda a um determinado critério: COUNT();
- cálculo da média ponderada: AVG();
- identificar os menores valores: MIN();
- identificar os maiores valores: MAX().

## Qual a sintaxe do comando SQL GROUP BY?

A sintaxe do comando SQL GROUP BY é:

```sql
SELECT nome_coluna(s)
 FROM nome_tabela
WHERE condição
GROUP BY nome_coluna (s)
ORDER BY nome_coluna (s);
```

Em que:

- nome_coluna(s): indica os nomes das colunas que serão apresentadas no resultado da instrução SQL. Vale ressaltar que podemos utilizar funções em conjunto com as colunas. Veremos mais sobre isso no próximo tópico;
- nome_tabela: indica o nome da tabela;
- condição: representa o critério de seleção para recuperar os registros;
- nome_coluna(s): indica o campo que será agrupado.

É importante dizer que a cláusula ORDER BY é opcional e é utilizada para classificar o resultado da busca em ordem crescente ou decrescente de acordo com os campos indicados.

## Seis exemplos de uso do comando SQL GROUP BY

Nada melhor que conferir exemplos práticos para entender como o comando GROUP BY funciona.
 Para isso, vamos utilizar uma pequena base de dados (criada no banco de dados MySQL) chamada “Comissão” que contém duas tabelas: Vendas e Vendedores.
  Confira o script para a criação do banco:

```sql
CREATE DATABASE IF NOT EXISTS Comissao;
USE Comissao;
CREATE TABLE IF NOT EXISTS Vendedores(
    vendedor_id INT AUTO_INCREMENT PRIMARY KEY,
    nome_vendedor VARCHAR(255) NOT NULL,
);

CREATE TABLE IF NOT EXISTS Vendas(
    venda_id INT AUTO_INCREMENT PRIMARY KEY,
    vendedor_id INT,
    valor_venda decimail(10, 2),
    data_venda date,
    estado varchar(45),
    FOREIGN KEY (vendedor_id) REFERENCES Vendedores(vendedor_id)
);

INSERT INTO Vendedores(nome_vendedor)
VALUES
    ("João"),
    ("Maria"),
    ("Sandra"),
    ("Pedro"),
    ("Marcos");

INSERT INTO Vendas
    (vendedor_id, valor_venda, data_venda, estado)
VALUES
    (1, 5000.00, '20210521', 'MG'),
    (1, 3000.00, '20210522', 'MG'),
    (1, 2000.00, '20210621', 'SP'),
    (2, 1500.00, '20210521', 'SP'),
    (2, 2200.00, '20210522', 'SP'),
    (2, 500.00, '20210621', 'MG'),
    (3, 4500.00, '20210521', 'RJ'),
    (3, 3700.00, '20210522', 'RJ'),
    (4, 4300.00, '20210521', 'BA'),
```

### 1. Usando o SQL GROUP BY com JOIN

A cláusula JOIN é utilizada quando queremos selecionar a combinação de campos originados de duas ou mais tabelas. Basicamente, ela é dividida em INNER JOIN (que considera o relacionamento entre as tabelas) e OUTER JOIN (que recupera os registros mesmo que não existam uma correspondência entre todos os registros das tabelas). O OUTER JOIN ainda se divide em:

- LEFT JOIN: retorna todos os valores da tabela à esquerda unidos à direita, mesmo que existam registros que não tenham nenhum relacionamento com a tabela à direita;
- RIGHT JOIN: retorna os valores da tabela à direita unidos à esquerda, mesmo que tenham registros sem relacionamento com a tabela à esquerda;
- FULL OUTER JOIN: retorna informações das duas tabelas, mesmo quando não houver relação entre elas. Vale ressaltar que essa opção não está disponível no MySQL.

No código a seguir, vamos exibir o nome da pessoa vendedora e a quantidade de registros de vendas para cada uma. Para isso, precisamos recuperar o campo nome_vendedor, que faz parte da tabela vendedores.

Como queremos listar todas as pessoas vendedoras, vamos utilizar o comando LEFT JOIN. Veja a instrução SQL:

```sql
SELECT a.nome_vendedor, count(b.venda_id) as nr_vendas
FROM vendedores AS a
LEFT JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / nr_vendas
João / 3
Maria / 3
Sandra / 2
Pedro / 1
Marcos / 0
```

Perceba que, entre os valores retornados, temos o vendedor Marcos que não tem nenhuma venda relacionada na tabela Vendas.
 Ele faz parte do resultado porque utilizamos a cláusula LEFT JOIN, que considera todos os registros da tabela à esquerda.

### 2. Agrupando com apenas uma coluna

Ao utilizarmos o comando GROUP BY, podemos agrupar por uma ou mais colunas. No código acima, por exemplo, agrupamos as vendas realizadas pelo id do vendedor. Portanto, utilizamos apenas uma coluna.

#### Agrupando com duas ou mais colunas

Também podemos utilizar mais colunas para agrupar o resultado. Veja o código de exemplo a seguir:

```sql
SELECT a.nome_vendedor, count(b.venda_id) as nr_vendas, b.estado
FROM vendedores AS a
JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY b.vendedor_id, b.estado

Resultado:
nome_vendedor / nr_vendas / estado
João / 2 / MG
João / 1 / SP
Maria / 2 / SP
Maria / 1 / MG
Sandra / 2 / RJ
Pedro / 1 / BA
```

No exemplo acima, além de agruparmos as vendas realizadas por cada pessoa vendedora, também utilizamos a coluna “estado” da tabela “vendas”.
 O objetivo da instrução SQL é contar quantas vendas foram realizadas e agrupá-las por pessoa vendedora e por estado.
  Por isso, temos a duplicação dos vendedores que realizaram vendas em diferentes estados.

Outra observação importante é que, como queremos os resultados também por estados, queremos apenas o retorno de registros que existam correspondências na tabela vendas.
 Por isso, utilizamos a cláusula JOIN (também poderia ser escrita como INNER JOIN), pois ela retorna os registros com correspondência na tabela vendas.

### 3. Agrupando com o uso de expressões

O comando GROUP BY também pode ser utilizado com expressões. No exemplo a seguir, vamos agrupar os registros por id de vendedor e por ano. Entretanto, como temos apenas a data da venda, precisamos usar uma expressão para determinar essa condição. Veja a instrução SQL:

```sql
SELECT a.nome_vendedor, count(b.venda_id) as nr_vendas, YEAR(b.data_venda) as ano
FROM vendedores AS a
JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY b.vendedor_id, YEAR(b.data_venda)

Resultado:
nome_vendedor / nr_vendas / ano
João / 2 / 2021
João / 1 / 2020
Maria /2 / 2021
Maria / 1 / 2020
Sandra / 2 / 2021
Pedro / 1 / 2021
```

Perceba que utilizamos a função YEAR() para recuperar apenas o valor correspondente ao ano no campo data_venda. Ela também foi utilizada na cláusula SELECT para exibir o resultado nesse formato.

### 4. Usando GROUP BY com INNER JOIN

Como mencionamos, a cláusula INNER JOIN retorna os valores que contêm correspondência em duas ou mais tabelas.
 No nosso primeiro exemplo, utilizamos a cláusula LEFT JOIN e ela retornou uma pessoa vendedora que não tinha realizado nenhuma venda.
  Vamos realizar a mesma instrução, agora com a cláusula INNER JOIN. Veja a diferença no resultado:

```sql
SELECT a.nome_vendedor, count(b.venda_id) as nr_vendas
FROM vendedores AS a
INNER JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / nr_vendas
João / 3
Maria / 3
Sandra / 2
Pedro / 1
```

Perceba que somente as pessoas vendedoras com vendas realizadas foram listadas nesse comando SQL.

### 5. Usando o GROUP BY com COUNT

A função COUNT() retorna a quantidade de registros de acordo com o critério de seleção. Como já fizemos vários exemplos utilizando essa função, vamos demonstrar como utilizar a função SUM(), que realiza a somatória dos valores de um determinado campo, conforme o critério da instrução SQL. Veja o exemplo:

```sql
SELECT a.nome_vendedor, SUM(b.valor_venda) as total_vendas
FROM vendedores AS a
INNER JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / total_vendas
João / 10000.00
Maria / 4200.00
Sandra / 8200.00
Pedro / 4300.00
```

### 6. Usando o GROUP BY com AVG, MIN e MAX

O SQL oferece diversas outras funções que podem ser utilizadas com a cláusula GROUP BY para fornecer o resultado conforme a função aplicada.

#### AVG

A função AVG() retorna a média ponderada entre os registros selecionados pela instrução SQL. No exemplo a seguir, vamos determinar o valor médio que cada pessoa vendedora realiza. Na prática, ele soma todas as vendas e divide pelo número de vendas realizadas. Confira a instrução SQL:

```sql
SELECT a.nome_vendedor, AVG(b.valor_venda) as media_vendas
FROM vendedores AS a
INNER JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / media_vendas
João / 3333.333333
Maria / 1400.000000
Sandra / 4100.000000
Pedro / 4300.000000
```

#### MIN()

A função MIN() retorna o valore mínimo de acordo com o critério de seleção. Portanto, vamos recuperar na nossa tabela qual o menor valor de venda agrupado por pessoa vendedora. Veja o código abaixo:

```sql
SELECT a.nome_vendedor, min(b.valor_venda) as menor_venda
FROM vendedores AS a
INNER JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / menor_venda
João / 2000.00
Maria / 500.00
Sandra / 3700.00
Pedro / 4300.00
```

#### MAX()

A função MAX() retorna o maior valor conforme o critério de seleção. Dessa forma, vamos recuperar o valor da maior venda agrupada por pessoa vendedora. Veja o resultado:

```sql
SELECT a.nome_vendedor, max(b.valor_venda) as maior_venda
FROM vendedores AS a
INNER JOIN vendas AS b
ON a.vendedor_id = b.vendedor_id
GROUP BY a.vendedor_id

Resultado:
nome_vendedor / maior_venda
João / 5000.00
Maria / 2200.00
Sandra / 4500.00
Pedro / 4300.00
```

## Quais as diferenças entre o comando GROUP BY e o GROUP BY ALL?

O comando GROUP BY agrupa todos os registros existentes que atendam ao critério de seleção especificado na instrução SQL. No banco de dados SQL Server existe a cláusula GROUP BY ALL. Na prática, ela possibilita a criação de grupos com resultados iguais a zero.

Imagine que queremos listar todas as pessoas vendedoras que realizaram vendas em 2021 e agrupá-las por estado e quantidade de vendas efetuadas. Veja a instrução SQL para rodar no SQL Server:

```sql
SELECT b.nome_vendedor, COUNT(a.venda_id) AS nr_vendas, a.estado
FROM [comissao].[dbo].[vendas] as a
LEFT JOIN [comissao].[dbo].[Vendedores] AS b ON a.vendedor_id = b.vendedor_id
WHERE YEAR(data_venda) = 2021
GROUP BY all a.estado, b.nome_vendedor

Resultado:
nome_vendedor/ nr_vendas/ estado
João / 1 / MG
João / 1 / SP
Maria / 0 / MG
Maria / 2 / SP
Pedro / 1 / BA
Sandra / 2 / RJ
```

Perceba que, no terceiro registro, temos a vendedora Maria, que não realizou nenhuma venda para o estado de MG no ano de 2021. Por isso, o retorno da coluna nr_vendas é igual a zero. Se realizássemos esse mesmo comando apenas com a cláusula GROUP BY, o resultado seria:

```bash
nome_vendedor/ nr_vendas/ estado
João / 1 / MG
João / 1 / SP
Maria / 2 / SP
Pedro / 1 / BA
Sandra / 2 / RJ
```

Vale ressaltar que a cláusula GROUP BY ALL não será inserida em versões futuras dos bancos SQL Server e Azure. Atualmente, ela está presente para oferecer compatibilidade com versões anteriores desses bancos.

## Boas práticas ao usar o comando SQL GROUP BY

A cláusula GROUP BY facilita a vida das pessoas programadoras, pois permite o agrupamento de diversos registros de acordo com o que desejamos. Entretanto, é preciso atenção, especialmente ao realizar o agrupamento com outras tabelas, pois se a instrução ficar muito complexa, pode impactar na performance da aplicação.

Além disso, é importante sempre realizar testes em cada instrução para garantir que o resultado seja realmente o desejado, caso contrário, pode comprometer as funcionalidades da aplicação.

O comando SQL GROUP BY é um recurso da linguagem SQL para agrupar registros conforme critérios definidos na instrução SQL. Sua utilização proporciona diversos benefícios, como mais agilidade na realização de cálculos, pois além de agrupar, podemos utilizar diversas funções disponíveis no SQL, como a COUNT(), AVG(), MIN(), MAX(), entre outras.


---
[Topo](#sql-group-by)
