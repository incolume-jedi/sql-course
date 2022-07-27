# SQL DELETE

[indice do curso](../README.md#curso-de-sql---structured-query-language)

---
[O que é o comando SQL DELETE?](#)
[Qual a sintaxe do comando SQL DELETE?](#)
[5 exemplos de uso do comando SQL DELETE](#)
[Boas práticas ao usar o comando SQL DELETE](#)

## O que é o comando SQL DELETE?

O comando SQL DELETE é usado para deletar os dados de uma ou mais linhas da tabela. É importante ressaltar que esse comando não exclui estruturas do banco, apenas os dados armazenados nele.

Ou seja, mesmo que o DELETE seja usado para apagar todos os registros de uma tabela, a sua estrutura ainda vai permanecer intacta no banco de dados. Portanto, ela passará a ser apenas uma tabela vazia.

# Qual a sintaxe do comando SQL DELETE?

A sintaxe básica usada para o comando DELETE é:
```sql
 DELETE FROM nome_da_tabela
 WHERE condição;
 ```

Observe que após a expressão FROM indicamos o nome da tabela que vamos excluir os dados. Já na cláusula WHERE definimos a condição que será usada como filtro para encontrar exatamente o registro que será excluído.

Perceba também que nele não há a necessidade de indicar as colunas da tabela que terão seus dados excluídos. Isso porque o comando deleta as informações contidas em todos os campos da linha indicada pela cláusula WHERE.

## 5 exemplos de uso do comando SQL DELETE

Agora que já foi esclarecido para que serve o comando SQL DELETE, vamos fazer algumas demonstrações práticas de uso dessa expressão. Para isso, nos exemplos abaixo usaremos como base de dados a tabela “funcionarios” mostrada a seguir.

|id	|nome	|setor	|cargo	|salario|
|---|-----|------|------|---------------|
|1	|Alexandre Matos	|TI	|Desenvolvedor Web	|6400|
|2	|Fabrice Nunes	|TI	|Analista de Dados	|7530|
|3	|Marília Delgado	|RH	|Gerente de Recursos Humanos	|6400|
|4	|Alberto Rodrigues	|Financeiro	|Analista Financeiro	|5870|
|5	|Flávia Souza	|RH	|Analista de Desenvolvimento Humano	|4500|
|6	|Roberto Moraes	|TI	|Analista de Sistemas	|5870|
|7	|Emília Ferraz	|TI	|Desenvolvedor Web	|6400|
|8	|Vitor Carvalho	|Financeiro	|Analista Fiscal	|3980|
|9	|Juliana Martins	|TI	|Desenvolvedor Web	|6400|
|10|	Luana Montenegro	|RH	|Coordenador de Recrutamento e Seleção	|5600|
|Tabela 1. Tabela funcionarios|

### 1. Usando DELETE com a cláusula WHERE

Para nosso primeiro exemplo, vamos mostrar a forma mais simples de uso do comando DELETE. Nesse caso, queremos excluir o registro com id = 3 na tabela “funcionarios”. O código que atende a esse requisito então seria:

```sql
 DELETE FROM funcionarios
 WHERE id = 3;
 ```

Após a execução do comando, nossa tabela estaria assim:

|id	|nome	|setor	|cargo	|salário|
|---|-----|------|------|---------------|
|1	|Alexandre Matos	|TI	|Desenvolvedor Web	|6400|
|2	|Fabrice Nunes	|TI	|Analista de Dados	|7530|
|4	|Alberto Rodrigues	|Financeiro	|Analista Financeiro	|5870|
|5	|Flávia Souza	|RH	|Analista de Desenvolvimento Humano	|4500|
|6	|Roberto Moraes	|TI	|Analista de Sistemas	|5870|
|7	|Emília Ferraz	|TI	|Desenvolvedor Web	|6400|
|8	|Vitor Carvalho	|Financeiro	|Analista Fiscal	|3980|
|9	|Juliana Martins	|TI	|Desenvolvedor Web	|6400|
|10	|Luana Montenegro	|RH	|Coordenador de Recrutamento e Seleção	|5600|
|Tabela 2. Demonstração do exemplo 1|

### 2. Usando DELETE para excluir várias linhas de uma tabela

No exemplo anterior mostramos como excluir uma linha específica da tabela. No entanto, há casos em que precisamos deletar vários dados de uma única vez e apagar linha por linha não seria eficiente.

Considerando novamente a tabela “funcionarios”, se quiséssemos excluir todos os registros em que a coluna “setor” tem valor igual a “RH”, poderíamos usar o seguinte comando:

```sql
 DELETE FROM funcionarios
 WHERE setor = 'RH';
 ```

Agora, observe o resultado que seria obtido após o código acima ser executado:

|id	|nome	|setor	|cargo	|salário|
|-------|-----|-------|-----|--------|
|1	|Alexandre Matos	|TI	|Desenvolvedor Web	|6400|
|2	|Fabrice Nunes	|TI	|Analista de Dados	|7530|
|4	|Alberto Rodrigues	|Financeiro	|Analista Financeiro	|5870|
|6	|Roberto Moraes	|TI	|Analista de Sistemas	|5870|
|7	|Emília Ferraz	|TI	|Desenvolvedor Web	|6400|
|8	|Vitor Carvalho	|Financeiro	|Analista Fiscal	|3980|
|9	|Juliana Martins	|TI	|Desenvolvedor Web	|6400|
|Tabela 3. Demonstração do exemplo 2|

### 3. Usando DELETE com múltiplas condições

O comando SQL DELETE também permite definir mais de uma condição como filtro na cláusula WHERE usando os operadores lógicos, como AND e OR.

Nesse caso, imagine uma situação em que é necessário excluir os dados das pessoas com salário igual a 6400. Entretanto, queremos excluir apenas daqueles que estão no setor de TI e há funcionários com o mesmo salário em outro setor.

Para cumprir tal propósito, o código que poderíamos usar é:

```sql
 DELETE FROM funcionarios
 WHERE salario = 6400
 AND setor = 'TI';
```

No fim da execução, teríamos o resultado a seguir:

|id|	nome	|setor	|cargo	|salário|
|--|------|-----------|--------|-----------|
|2	|Fabrice Nunes	|TI	|Analista de Dados	|7530|
|3	|Marília Delgado	|RH|	Gerente de Recursos Humanos	|6400|
|4	|Alberto Rodrigues	|Financeiro	|Analista Financeiro	|5870|
|5	|Flávia Souza	|RH	|Analista de Desenvolvimento Humano	|4500|
|6	|Roberto Moraes	|TI	|Analista de Sistemas	|5870|
|8	|Vitor Carvalho	|Financeiro	|Analista Fiscal	|3980|
|10|	Luana Montenegro	|RH	|Coordenador de Recrutamento e Seleção	|5600|
|Tabela 4. Demonstração do exemplo 3|

Observe que ainda há uma pessoa com salário igual a 6400 na tabela. Afinal, como o seu setor não é o de TI e, portanto, não cumpria os dois requisitos especificados na cláusula WHERE, seus dados não foram apagados.

### 4. Usando DELETE com o operador LIKE

Agora, para este exemplo, pense em um cenário fictício em que é necessário excluir os dados de todas as pessoas que têm a função de “Analista” no cargo. Porém, há vários cargos em diferentes setores em que esse termo aparece. Então, como eles poderiam ser excluídos de forma eficiente?

Uma solução para essa questão seria utilizar o operador LIKE como filtro na cláusula WHERE. De forma resumida, esse operador é responsável por encontrar padrões dentro de diferentes strings. Assim, o código ficaria da seguinte forma:
```sql
 DELETE FROM funcionarios
 WHERE cargo LIKE 'Analista%';
```

E como resultado de sua execução teríamos a tabela de dados:

|id|	nome	|setor	|cargo	|salário|
|--|------|----------|-------|----------|
|1	|Alexandre Matos	|TI	Desenvolvedor Web	|6400|
|3	|Marília Delgado	|RH	Gerente de Recursos Humanos	|6400|
|7	|Emília Ferraz	|TI	Desenvolvedor Web	|6400|
|9	|Juliana Martins	|TI	Desenvolvedor Web	|6400|
|10|	Luana Montenegro	|RH	Coordenador de Recrutamento e Seleção	|5600|
|Tabela 5. Demonstração do exemplo 4|


### 5. Usando DELETE com subquery

Neste último exemplo, vamos mostrar uma forma um pouco mais complexa de uso do comando DELETE. No caso, usaremos uma subquery dentro do comando principal para buscar os dados que precisamos. Para isso, usaremos como base as duas tabelas mostradas a seguir.

|id|	nome|	preco|
|---|---|---|
|1	|X Burguer|	25.90|
|2	|X Picanha|	38.90|
|3	|Cachorro Quente|	20.90|
|4	|Bauru|	22.90|
|Tabela 6. Tabela sanduiches|

|idPedido	|idCliente	|nomeSanduiche	|quantidade	|data|
|---------|----------|--------------|-----------|-------------|
|1	|10|	X Picanha|	        1|2021-07-28|
|2	|15|	X Burguer|	        2|2021-07-06|
|3	|10|	Cachorro Quente	|   2|2021-07-28|
|4	|8	|X Picanha	|           3|2021-07-19|
|5	|12|	Bauru	|           2|2021-05-12|
|6	|11|	X Burguer|	        4|2021-06-09|
|7	|15|	X Picanha|	        1|2021-07-06|
|Tabela 7. Tabela pedidos|

Agora imagine que precisamos deletar da tabela “sanduiches” o sanduíche que não recebeu nenhum pedido no último mês. Para isso, seria necessário verificar na tabela “pedidos” as datas registradas em cada pedido, avaliando se elas são mais recentes do que um mês.

Uma forma de resolver esse problema seria com o comando SQL abaixo:
```sql
 DELETE FROM sanduiches s
 WHERE nome NOT IN (
 SELECT nomeSanduiche
 FROM pedidos
 WHERE  data >= CURRENT_DATE - INTERVAL 1 MONTH);
```

## Boas práticas ao usar o comando SQL DELETE

Ao utilizar o comando DELETE é importante ter atenção à aplicação da cláusula WHERE.
 Apesar de não ser obrigatória, caso ela não seja utilizada todos os registros da tabela serão deletados.
 Se esse não for o objetivo, isso certamente vai gerar vários problemas para o banco.

Além disso, é essencial pensar em uma maneira de definir as condições para o filtro de forma que não atrapalhem o desempenho do banco.
 Também é válido considerar o uso do DELETE dentro de blocos TRY/CATCH para fazer o tratamento de erros.

Por fim, se for necessário excluir todas as linhas da tabela, é interessante considerar o uso do comando TRUNCATE TABLE no lugar do DELETE.

Como foi possível notar ao longo do artigo, o comando SQL DELETE pode ser usado de várias formas diferentes, sendo fundamental entender como aplicá-lo corretamente para evitar erros e inconsistência de dados no banco. Apesar disso, se trata de um comando extremamente útil para quem trabalha diretamente com essa linguagem.

---

[Topo](#sql-delete)
