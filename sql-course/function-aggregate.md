# Funções de Agregação

[Indice do Curso](../README.md#curso-de-sql---structured-query-language)

---

Os programadores precisam conhecer esses recursos, pois eles ajudam que auxiliam no desenvolvimento de suas aplicações.
Entretanto, pode existir dúvidas sobre qual delas utilizar ou como aplicar essas funções com cláusulas como a GROUP BY, ORDER BY e HAVING.
Por isso, preparamos este conteúdo com os seguintes tópicos:

- [O que são funções de agregações do SQL?](#o-que-são-funções-de-agregações-do-sql)
- [O que é a função SQL COUNT?](#o-que-é-a-função-sql-count)
- [O que é a função SQL AVG?](#o-que-é-a-função-sql-avg)
- [O que é a função SQL SUM?](#o-que-é-a-função-sql-sum)
- [O que é a função SQL MAX?](#o-que-é-a-função-sql-max)
- [O que é a função SQL MIN?](#o-que-é-a-função-sql-min)

## O que são funções de agregações do SQL?

As funções agregadas são aquelas que realizam um determinado cálculo em um grupo de registros de uma tabela.
Elas são muito úteis, pois são utilizadas para várias finalidades pelas pessoas programadoras ao desenvolver aplicações, como para determinar o número de registros, calcular a soma de valores de um grupo de registros e muito mais.

### O que é a função SQL COUNT?

A função SQL COUNT é utilizada para determinar o valor total de registros que atenda a uma condição específica.
Se nenhuma condição for definida na instrução SQL, o resultado será com base em todos os registros da tabela.

### Qual a sintaxe da função SQL COUNT?

A sintaxe da função SQL COUNT é:

```SQL
SELECT COUNT(nome_coluna)
FROM nome_tabela
WHERE condição
```

### Exemplo de uso da função SQL COUNT

Vamos realizar alguns exemplos práticos para demonstrar como esse recurso funciona.
Para isso, criaremos uma pequena base de dados chamada “Escola”, que contém uma tabela chamada “Pessoa” para armazenar algumas informações sobre as pessoas estudantes.
Veja o script para a criação da base no banco de dados MySQL:

```SQL
CREATE DATABASE IF NOT EXISTS Escola;

USE Escola;

CREATE TABLE IF NOT EXISTS Pessoa(
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    idade INT,
    cidade VARCHAR(50),
    nota1 decimal(10, 2),
    nota2 decimal(10, 2),
    valor_mensalidade decimal(10, 2),
    valor_desconto decimal(10, 2)
);

INSERT INTO Pessoa VALUES
   (1, 'João', 23, 'São Paulo', 8.2, 7.0, 289.90, 50.00),
   (2, 'Ana', 24, null, 8.2, 7.0, 289.90, 50.00),
   (3, 'Pedro', null, null, 8.2, 7.0, 289.90, 50.00),
   (4, 'Filipe', 26, 'Brasília', 8.2, 7.0, 289.90, 50.00),
   (5, 'Tiago', 21, 'Brasília', 8.2, 7.0, 289.90, 50.00),
   (6, 'José', 31, 'Blumenau', 8.2, 7.0, 289.90, 50.00),
   (7, 'Carlos', 22, 'Blumenau', 8.2, 7.0, 289.90, 50.00),
   (8, 'Jonas', 31, 'Belo Horizonte', 8.2, 7.0, 289.90, 50.00),
   (9, 'André', 30, 'Belo Horizonte', 8.2, 7.0, 289.90, 50.00),
   (10, 'Poliana', 25, 'Goiânia', 8.2, 7.0, 289.90, 50.00),
   (11, 'Carla', 18, 'Aracajú', 9.4, 9.0, null, null),

```

Após criarmos o banco de dados, vamos contar o total de registros na tabela “Pessoa”. Veja a instrução SQL:

```SQL
SELECT COUNT(*) AS total_registros FROM pessoa

Resultado:
total_registros
8
```

Também podemos indicar um campo específico. Veja o exemplo a seguir:

```sql
SELECT COUNT(cidade) AS total_cidade FROM pessoa

Resultado:
total_cidade
6
```

Na seleção acima, selecionamos o total de registros em que o campo cidade está preenchido. É importante dizer que a função COUNT() considera apenas valores válidos, ou seja, não conta os valores nulos. Além disso, ela não faz distinção se o conteúdo do campo é repetido ou não. Por isso, a quantidade de cidades é de 6, apesar de existirem registros com cidades de mesmo nome.

O comando SQL COUNT também pode ser utilizado com outras cláusulas. Confira algumas delas a seguir.

### Usando SQL COUNT com GROUP BY

Podemos utilizar a função COUNT() para descobrir a quantidade de registros que pertença a um determinado grupo de dados. Veja a instrução SQL a seguir:

```sql
SELECT
   cidade,
   COUNT(*) AS total_por_cidade
FROM pessoa
GROUP BY cidade

Resultado:
Cidade / total_por_cidade
Null / 2
Belo Horizonte / 1
Campinas / 2
Rio de Janeiro / 1
São Paulo / 2
```

Perceba que temos duas cidades com o conteúdo igual a NULL entre os resultados listados.
Isso acontece porque a cláusula GROUP BY considera os valores nulos como válidos para o agrupamento.
 Se quisermos excluir esses valores, teríamos que adicionar um critério de seleção. Veja a instrução abaixo:

```sql
SELECT cidade, COUNT(*) AS total_por_cidade
FROM pessoa
WHERE cidade IS NOT NULL
GROUP BY cidade

Resultado:
cidade / total_por_cidade
Belo Horizonte / 1
Campinas / 2
Rio de Janeiro / 1
São Paulo / 2
```

### Usando SQL COUNT com ORDER BY

A cláusula ORDER BY é utilizada para ordenar o resultado de acordo com um critério específico. Ela também pode ser aplicada com a função COUNT(). Confira a instrução SQL abaixo:

```sql
SELECT
   cidade,
   count(*) AS total_por_cidade
FROM pessoa
WHERE cidade IS NOT NULL
GROUP BY cidade
ORDER BY count(*)

Resultado:
cidade / total_por_cidade
Belo Horizonte / 1
Rio de Janeiro / 1
São Paulo / 2
Campinas / 2
```

No código acima, perceba que também utilizamos a cláusula GROUP BY e excluímos os registros com o campo “cidade” igual a “null”. Isso foi necessário para definirmos o grupo e realizarmos a ordenação com base nessa seleção de dados.

### Usando SQL COUNT com HAVING

A cláusula HAVING é utilizada para adicionarmos uma condição na seleção de dados que é feita com o uso de funções agregadas, como a COUNT(), pois a cláusula WHERE não pode ser utilizada com funções desse tipo. Confira a instrução SQL abaixo:

```sql
SELECT
   cidade,
   count(*) AS total_por_cidade
FROM pessoa
GROUP BY cidade
HAVING COUNT(*) > 1

Resultado:
cidade / total_por_cidade
Null / 2
Campinas / 2
São Paulo / 2
```

Perceba que temos os valores nulos listados como resultado. Isso porque utilizamos o caractere “*” na função COUNT() na cláusula HAVING.
 Para eliminar esses dados, podemos adicionar a cláusula WHERE ou indicar o campo cidade na função COUNT().
  Veja a seguir:

```sql
SELECT
   cidade,
   count(*) AS total_por_cidade
FROM pessoa
GROUP BY cidade
HAVING COUNT(cidade) > 1

Resultado:
cidade / total_por_cidade
Campinas / 2
São Paulo / 2
```

### Boas práticas ao usar a função SQL COUNT

A função COUNT() é muito útil para diversos cálculos de agrupamentos de registros. Entretanto, é preciso atenção ao utilizá-la, especialmente com campos que contenham valores nulos, pois se essa condição não for bem observada, o resultado do comando SQL pode não ser o desejado pela aplicação.

Também é preciso atenção com os campos que tenham valores duplicados, pois a função COUNT() considera os conteúdos redundantes. Portanto, para eliminar essa condição é preciso adicionar essa validação no critério de seleção ou utilizar a cláusula DISTINCT.

## O que é a função SQL AVG?

A função SQL AVG() é utilizada para retornar o valor médio de um grupo de registros selecionados com a cláusula SELECT. Ela só pode ser aplicada em campos que contenham valores numéricos.

### Qual a sintaxe da função SQL AVG?

A sintaxe da função AVG() é:

```sql
SELECT AVG( [ALL/DISTINCT] nome_coluna ou expressão)
FROM nome_tabela
WHERE condição
```

A função AVG() pode ser utilizada com a seleção de um campo ou por meio de uma expressão. Ela retornará a soma de todos os números selecionados dividido pela quantidade de registros. Por isso, a cláusula ALL é considerada como padrão de seleção. Se quisermos eliminar os valores repetidos, devemos utilizar a cláusula DISTINCT.

### Exemplo de uso da função SQL AVG

Confira alguns exemplos de como utilizar a função AVG().

```sql
SELECT
   cidade,
   AVG(nota1) AS nota_media
FROM pessoa
GROUP BY cidade

Resultado:
cidade / nota_media
NULL/ 8.500000
Belo Horizonte / 6.200000
Campinas / 9.000000
Rio de Janeiro / 8.000000
São Paulo / 8.100000
```

Como mencionamos, a função AVG() ignora os campos com valores nulos.
 Por isso, no resultado apresentado, o valor calculado da média para a cidade de Campinas é igual a 9.0 apesar de termos dois registros para essa cidade.

Entretanto, temos uma cidade listada com o valor nulo e com o cálculo da média realizado para ela. Nesse caso, a média foi calculada, pois não há valores nulos para o campo “nota1” e sim, para o campo “cidade”, que é utilizado pela cláusula GROUP BY — que considera valores nulos para realizar agrupamentos. Portanto, se quisermos excluir os valores nulos, devemos adicionar essa condição na instrução SQL.

A função AVG() também pode ser aplicada com uma expressão. Veja o exemplo a seguir:

```sql
SELECT
   cidade,
   AVG((nota1 + nota2)/2) AS notas_media
FROM pessoa
GROUP BY cidade

Resultado:
cidade / notas_media
Null / 7.7500000000
Belo Horizonte / 7.8500000000
Campinas / 8.5000000000
Rio de Janeiro / 8.6000000000
São Paulo / 8.0500000000
```

Perceba que, como utilizamos uma expressão, pudemos inserir outros campos do mesmo registro para realizar um cálculo da média de notas e só então, calcular o valor médio agrupado por cidade.

### Boas práticas ao usar a função SQL AVG

Reforçamos que é preciso atenção em relação aos valores nulos existentes na tabela e como os comandos e funções SQL funcionam com essa condição. Isso porque o resultado de cada instrução pode mudar completamente quando esses valores são ou não considerados em um comando!

## O que é a função SQL SUM?

A função SQL SUM() é utilizada para retornar a somatória de um conjunto de registros de uma tabela.

### Qual a sintaxe da função SQL SUM?

A sintaxe da função SUM() é:

```sql
SELECT SUM ([ALL / DISTINCT] nome_campo ou expressão)
FROM nome_tabela
WHERE condição
```

### Exemplo de uso da função SQL SUM

No exemplo a seguir, somaremos os valores correspondentes às mensalidades agrupados por cidade. Veja a instrução SQL:

```sql
SELECT
   cidade,
   SUM(valor_mensalidade) AS total_mensalidade
FROM pessoa
GROUP BY cidade

Resultado:
cidade / total_mensalidade
Null / 250.00
Belo Horizonte / 375.50
Campinas / Null
Rio de Janeiro / 250.00
São Paulo / 477.90
```

Perceba que a função SUM() não considera os valores nulos ao fazer a somatória. Além disso, perceba o que acontece na cidade de Campinas no nosso exemplo: se todos os campos tiverem com valores iguais a nulo, o resultado será igual a nulo.

A função SUM() também pode ser utilizada com dois ou mais campos do mesmo registro. Entretanto, se eles não fizerem parte de uma expressão, devem ser chamados de forma individual. Veja o exemplo:

```sql
SELECT
   cidade,
   SUM(valor_mensalidade) AS total_mensalidade,
   SUM(valor_desconto) AS total_desconto
FROM pessoa
GROUP BY cidade

Resultado:
cidade / total_mensalidade / total_desconto
Null / 250.00 / 30.00
Belo Horizonte / 375.50 / 30.00
Campinas / Null / Null
Rio de Janeiro / 250.00 / 42.00
São Paulo / 477.90 / 108.00
```

### Boas práticas ao usar a função SQL SUM

A função SUM() somente deve ser utilizada com campos que contenham valores numéricos.
 Assim como as recomendações feitas para as funções anteriores, é preciso atenção com os campos que contenham valores nulos.


## O que é a função SQL MAX?

A função SQL MAX retorna o valor máximo de um determinado campo de uma tabela de acordo com o critério de seleção estabelecido.

### Qual a sintaxe da função SQL MAX?

A sintaxe da função MAX() é:

```sql
SELECT MAX(nome_coluna)
FROM nome_tabela
WHERE condição
```

### Exemplo de uso da função SQL MAX

Se nenhuma condição for especificada, a função MAX() retorna o maior valor de um determinado campo comparando todos os dados da tabela. Veja o exemplo a seguir:

```sql
SELECT MAX(idade) AS idade_maxima FROM pessoa;

Resultado:
idade_maxima
35
```

Também podemos utilizar essa função com o agrupamento de registros. Veja a instrução SQL a seguir:

```sql
SELECT
   cidade,
   MAX(idade) AS idade_maxima
FROM pessoa
WHERE CIDADE IS NOT NULL
GROUP BY cidade

Resultado:
cidade / idade-maxima
Belo Horizonte / 35
Campinas / 30
Rio de Janeiro / Null
São Paulo / 25
```

### Boas práticas ao usar a função SQL MAX

A função MAX() ignora valores nulos. Além disso, ela retorna nulo caso não exista valores a serem considerados. Ela pode ser utilizada com campos de caracteres. Nesse caso, ela faz a classificação por colunas de caracteres. Portanto, o resultado será semelhante à organização alfabética.


## O que é a função SQL MIN?

A função MIN() retorna o menor valor de um campo, de acordo com o critério de seleção estabelecido.

### Qual a sintaxe da função SQL MIN?

A sintaxe da função MIN() é:

```sql
SELECT MIN(nome_coluna)
FROM nome_tabela
WHERE condição
```

### Exemplo de uso da função SQL MIN

A função MIX() retorna o menor valor de um determinado campo. Se nenhuma condição for definida, todos os dados da tabela serão comparados. Veja o exemplo a seguir:

```sql
SELECT MIN(idade) AS idade_minima FROM pessoa;

Resultado:
idade_minima
18
```

Também podemos utilizar essa função com o agrupamento de registros. Veja o comando SQL a seguir:

```sql
SELECT
   cidade,
   MIN(idade) AS idade_minima
FROM pessoa
WHERE CIDADE IS NOT NULL
GROUP BY cidade

Resultado:
cidade / idade-minima
Belo Horizonte / 35
Campinas / 28
Rio de Janeiro / Null
São Paulo / 23
```

### Boas práticas ao usar a função SQL MIN

A função MIN() é semelhante à MAX(), porém, com a classificação ao contrário. Portanto, as recomendações para as duas são as mesmas. Vale ressaltar que é sempre importante realizar testes para conferir se as instruções SQL retornam os valores esperados.

Os campos com valores nulos geralmente causam muitos erros em aplicações. Por isso, é preciso atenção em relação ao que a função ou cláusula SQL faz e como elas lidam com valores nulos.

As funções de agregações, como a SQL COUNT e as outras que vimos ao longo do texto são um poderoso recurso da linguagem SQL para realizarmos cálculos de dados agrupados por um determinado critério. Elas proporcionam mais agilidade no desenvolvimento de aplicações. Por isso, as pessoas programadoras precisam entender como aproveitá-las ao máximo!

---
[Topo](#funções-de-agregação)
