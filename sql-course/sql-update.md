# SQL UPDATE

[indice do curso](../README.md#curso-de-sql---structured-query-language)

---
No SQL, o comando SQL UPDATE tem uma das funções mais importantes da linguagem: manter os dados da base atualizados.

No entanto, por manipular diretamente as informações salvas no banco, usar esse comando pode ser mais complicado. Afinal, se houver um pequeno erro na lógica do comando, podemos acabar inserindo dados errados em nossa tabela.

Por isso, é preciso aprender com cuidado a formular um UPDATE de forma correta, tendo atenção especial sobre a forma que filtramos os dados que queremos atualizar.  Para ajudar nessa questão, neste post vamos explicar:

[O que é o comando SQL UPDATE?](#)
[Qual a sintaxe do comando SQL UPDATE?](#)
[Como atualizar colunas text, ntext e image?](#)
[Como atualizar tipos de dados de valores grandes?](#)
[Exemplo de uso do comando SQL UPDATE](#)
[Boas práticas com o comando SQL UPDATE](#)

## O que é o comando SQL UPDATE?

O comando UPDATE é o comando SQL responsável por atualizar os dados já armazenados em uma tabela do banco. Ele pode ser usado tanto para atualizar um único registro quanto para alterar múltiplas informações de uma vez.

Além disso, ele faz parte do subconjunto SQL denominado como DML (Data Manipulation Language), que agrupa os comandos utilizados para manipular as informações registradas na base de dados.

## Qual a sintaxe do comando SQL UPDATE?

A sintaxe do comando UPDATE é bastante simples, observe:

```sql
 UPDATE nome_da_tabela
 SET nome_da_coluna1 = valor_da_coluna1, nome_da_coluna2 = valor_da_coluna2
 WHERE condição;
 ```

A princípio, pode parecer um pouco confuso, mas vamos explicar detalhadamente para quê servem cada uma dessas instruções presentes no comando.

Primeiro, é preciso entender o argumento SET. Ele é o responsável por especificar as colunas da tabela que queremos alterar, assim como os novos valores que vamos inserir nas colunas indicadas. É importante lembrar que, ao listar o nome das colunas, devemos sempre separá-los com vírgula.

Após o argumento SET, usamos a cláusula WHERE. Essa cláusula é muito importante para que os dados sejam atualizados sem nenhum erro, pois é com ela que vamos indicar exatamente a linha da tabela que queremos alterar.

Além deles, também podemos utilizar os operadores OR e AND junto com a cláusula WHERE para combinar várias condições e realizar uma filtragem mais apurada na tabela. Nesse caso, a sintaxe do comando seria, por exemplo:

```sql
UPDATE nome_da_tabela
SET nome_da_coluna1 = valor_da_coluna1, nome_da_coluna2 = valor_da_coluna2
WHERE condição AND condição;
```

## Como atualizar colunas text, ntext e image?

No Transact-SQL (uma extensão da linguagem SQL implementada pela Microsoft e que é utilizada no SQL Server) existem os tipos de dados text, ntext e image.
 Para atualizar dados desse tipo, é preciso uma operação um pouco diferente.

Primeiro, precisamos inicializar a coluna e atribuir um ponteiro de texto a ela.
 Em seguida, é necessário alocar ao menos uma página (que é a unidade fundamental de armazenamento de dados no SQL Server) para guardar os dados.
  Já para alterar grandes blocos de dados ntext, text e image, devemos usar os comandos UPDATETEXT e WRITETEXT no lugar do comando UPDATE convencional.

No entanto, é importante ressaltar que esse tipos de dados estão obsoletos, portanto é recomendado evitar usá-los e substituí-los pelos tipos varchar (max), nvarchar (max), e varbinary (max).

## Como atualizar tipos de dados de valores grandes?

Também no SQL Server usamos a cláusula .WRITE (expression , @Offset , @Length) para fazer uma atualização completa ou parcial para tipos de dados de valores grandes, como varchar (max), nvarchar (max), e varbinary (max).

A sintaxe básica nesse caso seria:

```sql
 UPDATE nome_da_tabela
 SET nome_da_coluna.WRITE (expression, @offset, @length)
 WHERE condição;
 ```

## Exemplo de uso do comando SQL UPDATE

Agora que já explicamos como o comando UPDATE funciona, vamos analisar alguns exemplos práticos de código para fixar o conhecimento.

### 1. Atualizando uma linha da tabela

Para o nosso primeiro exemplo, considere a tabela “estudantes” que mostramos logo abaixo:

|id| nome	          |matrícula	|curso|
|---|----------------|----------|-----------|
|1	|Marcos Antônio	 | 21045	|Programação|
|2	|Maria Gabriela	  |19856	|Técnico em Informática|
|3	|Eduardo Mendes	  |23634	|Banco de Dados|
|4	|Clarisse Nunes	  |22579	|Programa|
|5	|Pedro Alves	  |20025	|Banco de Dados|
|6	|Joaquim Ferreira	|18154	|Técnico em Informática|
|7	|Marcos Antônio	  |18649	|Banco de Dados|

Imagine que precisamos atualizar o curso da aluna Clarisse Nunes para “Programação”, pois ele foi cadastrado errado e consta na tabela como “Programa”. Para isso, usamos o seguinte comando:

```sql
 UPDATE estudantes
 SET curso = 'Programação'
 WHERE id = 4;
 ```

Observe que aqui especificamos o nome da tabela que será alterada, no caso “estudantes”. Depois indicamos a coluna “curso” para receber novas informações e, em seguida, definimos o novo valor. Por fim, usando a cláusula WHERE atualizamos apenas uma linha da coluna, pois usamos como filtro a instrução “id = 4”.

Depois da atualização, nossa tabela vai estar assim:

|id|	nome	         |matrícula|	curso|
|---|----------------|--------|------------------|
|1	|Marcos Antônio	 |21045	  |  Programação|
|2	|Maria Gabriela	 |19856	  |  Técnico em Informática|
|3	|Eduardo Mendes	 |23634	  |  Banco de Dados|
|4	|Clarisse Nunes	 |22579	  |  Programação|
|5	|Pedro Alves	    | 20025	 |   Banco de Dados|
|6	|Joaquim Ferreira| 18154	 |   Técnico em Informática|
|7	|Marcos Antônio	 |18649	  |  Banco de Dados|

### 2. Atualizando várias linhas da tabela

Agora pense em uma situação em que precisamos alterar o nome do curso “Programação” para “Programação Orientada a Objetos”. No entanto, existem várias linhas da tabela em que esse dado foi cadastrado e seria muito trabalhoso atualizar linha por linha, certo?

Então, na cláusula WHERE dessa vez iremos utilizar um filtro que nos permitirá mudar mais de uma linha com um único comando. Observe:

```sql
 UPDATE estudantes
 SET curso = 'Programação Orientada a Objetos'
 WHERE curso = 'Programação';
 ```

Após rodar o comando, nossa tabela agora terá os seguintes dados:

|id	|nome	|matrícula	   |curso      |
|---|-----  |----------    |-----------|
|1	|Marcos Antônio	 |21045	|Programação Orientada a Objetos|
|2	|Maria Gabriela	 |19856	|Técnico em Informática|
|3	|Eduardo Mendes	 |23634	|Banco de Dados|
|4	|Clarisse Nunes	 |22579	|Programação Orientada a Objetos|
|5	|Pedro Alves     |20025	|Banco de Dados|
|6	|Joaquim Ferreira|	18154|	Técnico em Informática|
|7	|Marcos Antônio	 |18649	|Banco de Dados|

### 3. Combinando condições com AND e OR

Como você deve ter reparado, em nossa tabela existem dois estudantes cadastrados com o nome “Marcos Antônio”. Então, imagine que queremos atualizar o nome de um deles, mais precisamente do estudantes que está no curso “Banco de Dados”. Para isso, poderíamos usar o operador AND na cláusula WHERE para fazer a filtragem na tabela. Veja:
```sql
 UPDATE estudantes
 SET nome = 'Marcos Antônio Souza'
 WHERE nome = 'Marcos Antônio' AND curso = 'Banco de Dados';
 ```

Agora nossa tabela está assim:

|id|	nome	               | matrícula|	curso|
|--|---------------------|----------|---------------|
|1	|Marcos Antônio	      |  21045	  |  Programação Orientada a Objetos|
|2	|Maria Gabriela	      |  19856	  |  Técnico em Informática|
|3	|Eduardo Mendes	      |  23634	  |  Banco de Dados|
|4	|Clarisse Nunes	      |  22579	  |  Programação Orientada a Objetos|
|5	|Pedro Alves	         |   20025  | 	Banco de Dados|
|6	|Joaquim Ferreira	    |18154	    |Técnico em Informática|
|7	|Marcos Antônio Souza	|18649	    |Banco de Dados|

Pensando em outro exemplo fictício, vamos atualizar agora o nome do estudante “Eduardo Mendes”, mas dessa vez faremos a filtragem da busca usando o id ou a matrícula.
 Nesse caso, o comando SQL ficaria assim:

```sql
 UPDATE estudantes
 SET nome = 'Eduardo Mendes Ferreira'
 WHERE matricula = 23634 OR id = 3;
 ```

Como resultado, temos a seguinte tabela de dados:

|id|	nome	               | matrícula|	curso|
|--|---------------------|---------|--------------|
|1	|Marcos Antônio	      |  21045	 |   Programação Orientada a Objetos|
|2	|Maria Gabriela	      |  19856	 |   Técnico em Informática|
|3	|Eduardo Mendes Ferreira|	23634 |  	Banco de Dados|
|4	|Clarisse Nunes	      |  22579	 |   Programação Orientada a Objetos|
|5	|Pedro Alves	         |   20025 |  	Banco de Dados|
|6	|Joaquim Ferreira	    |18154   	|Técnico em Informática|
|7	|Marcos Antônio Souza	|18649   	|Banco de Dados|

## Boas práticas com o comando SQL UPDATE

O principal cuidado que precisamos tomar ao utilizar o comando UPDATE é na especificação da cláusula WHERE. Apesar desta cláusula ser opcional e poder ser omitida no código, é recomendado sempre utilizá-la, a não ser em casos específicos em que você realmente precise atualizar todas as linhas da coluna de uma tabela.

Pense no último exemplo em que atualizamos o nome do estudante “Eduardo Mendes” para “Eduardo Mendes Ferreira”. Se naquele comando não tivéssemos usado a cláusula WHERE, agora nossa tabela estaria assim:

|id|	nome	|matrícula	|curso|
|--|------------------------|----|--|
|1	|Eduardo Mendes Ferreira	|21045	|Programação Orientada a Objetos|
|2	|Eduardo Mendes Ferreira	|19856	|Técnico em Informática|
|3	|Eduardo Mendes Ferreira	|23634	|Banco de Dados|
|4	|Eduardo Mendes Ferreira	|22579	|Programação Orientada a Objetos|
|5	|Eduardo Mendes Ferreira	|20025	|Banco de Dados|
|6	|Eduardo Mendes Ferreira	|18154	|Técnico em Informática|
|7	|Eduardo Mendes Ferreira	|18649	|Banco de Dados|

Certamente, esse não é um resultado desejado, pois o nome de todos os estudantes da tabela foi perdido.
Imagine isso acontecendo em um banco de dados gigantesco com milhares de dados armazenados? Seria problemático, não é mesmo?

Por isso, sempre que utilizar o comando UPDATE, é importante pensar cuidadosamente em qual será o filtro usado na cláusula WHERE para evitar inserir erros no banco.

Como foi possível notar, o comando SQL UPDATE é essencial para manter os dados cadastrados no banco atualizados e sem erros.
Afinal, ele nos permite alterar informações que foram cadastradas de forma errada ou simplesmente modificá-las para dados mais apropriados.
 No entanto, é preciso ter cautela ao usar esse comando, para evitar perder dados por modificações indevidas.

---

[Topo](#sql-update)
