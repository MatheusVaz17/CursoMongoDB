### HyperLinks

- [Conceitos](#conceitos)
- [Variáveis utilizadas nos comandos](#variáveis-utilizadas-nos-comandos)
- [Comandos para iniciar o serviço do mongo](#comandos-para-iniciar-o-serviço-do-mongo)
- [Comandos no Mongo Shell](#comandos-no-mongo-shell)
- [Relacionamentos](#relacionamentos)
- [Queries](#queries)
  - [O que são queries?](#o-que-são-queries)
  - [Operadores de comparação](#operadores-de-comparação)
  - [Combinação de operadores](#combinação-de-operadores)
  - [Sort](#sort)
  - [Paginação](#paginação)
  - [Embeded documents](#embeded-documents)
- [Updates](#updates)
  - [Adicionar campo em documento ou alterar campo existente](#adicionar-campo-em-documento-ou-alterar-campo-existente)
  - [Updates em arrays](#updates-em-arrays)
- [Indexes](#indexes)
  - [O que são índices de banco de dados](#o-que-são-índices-de-banco-de-dados)
  - [Comandos para índices](#comandos-para-índices)
- [Aggregation](#aggregation)
  - [Conceito](#conceito)
  - [Join combate de pokemons](#join-combate-de-pokemons)
  - [Escolher quais campos retornar](#escolher-quais-campos-retornar)
  - [Condições para retorno dos dados](#condições-para-retorno-dos-dados)

## Conceitos

 **Banco de dados não relacional** 
>  Um banco de dados não relacional, também conhecido como NoSQL, é um sistema de armazenamento de dados que difere 
>  dos bancos de dados relacionais tradicionais, como SQL, em sua estrutura e organização. Ele é projetado para lidar 
>  com grandes volumes de dados, oferecendo flexibilidade na modelagem e escalabilidade horizontal. Em vez de usar tabelas 
>  com esquemas rígidos, ele pode usar modelos de dados como documentos, gráficos, colunas ou chave-valor, permitindo que os dados 
>  sejam armazenados de forma mais flexível e distribuída. Isso é especialmente útil em cenários onde a estrutura dos dados é variável 
>  ou imprevisível, como em aplicativos web ou Big Data.

**Coleção (Collection)**
>  Uma coleção em um banco de dados não relacional é semelhante a uma tabela em um banco de dados relacional, mas armazena documentos 
>  em vez de registros. Ela organiza os dados de forma flexível, permitindo diferentes tipos de documentos em uma única coleção. 
>  As coleções são frequentemente usadas em bancos de dados NoSQL orientados a documentos, como o MongoDB, e podem ser consultadas e
>  manipuladas usando operações específicas do banco de dados. Cada documento em uma coleção pode ter uma estrutura diferente, oferecendo 
>  versatilidade na modelagem de dados.

**Documento**
>  Documentos em bancos de dados não relacionais são estruturas de dados flexíveis e sem esquema fixo, usadas para armazenar informações. 
>  Eles são análogos a linhas em bancos de dados relacionais, mas têm uma estrutura mais livre, geralmente usando formatos como JSON ou BSON. 
>  Cada documento contém dados relacionados e pode variar em conteúdo e tamanho dentro da mesma coleção. Essa flexibilidade permite uma 
>  modelagem de dados mais dinâmica e escalável, comumente encontrada em sistemas NoSQL, como MongoDB e Couchbase. Os documentos são 
>  manipulados e consultados usando operações específicas do banco de dados para criar, atualizar, recuperar e excluir informações.

**Tipos de dados BSON**

[Documentação dos tipos de dados suportados no BSON](https://www.mongodb.com/docs/manual/reference/bson-types/)

## Variáveis utilizadas nos comandos

Essas "variáveis" são palavras que serão alteradas de acordo com o comando que será realizado

| Variável | Descrição |
| -------- | --------- |
| ${path}  | Caminho de uma pasta |
| ${file}  | Nome de um arquivo |
| ${db}    | Nome do banco |
| ${collection} | Nome da collection |
| ${document} | BSON do documento |
| ${field} | Campo do documento |
| ${value} | Valor do documento |

## Comandos para iniciar o serviço do mongo

Inicia o serviço do MongoDB:
```console
mongod --dbpath ${path}
```

Inicia o serviço do MongoDB em background para o terminal não precise ficar aberto:
```console
mongod --dbpath ${path} --service
```

Inicia o serviço do MongoDB em background e define uma pasta para guardar o arquivo de logs:
```console
mongod --dbpath ${path} --service --logpath ${path}/${file}
```

Se conecta ao host do banco:
```console
mongosh
```

## Comandos no Mongo Shell

Seleciona em qual banco de dados será realizado as ações:
```console
> use ${db}
```

Exibe todos os bancos ativos:
```console
> show dbs
```

Mostra qual banco está selecionado no momento:
Exibe todos os bancos ativos:
```console
> db
```

Exibe todas as *collections* ativas no banco selecionado:
```console
> show collections
```

Insere um documento no banco:
```console
> db.${collection}.insert()
```
Ou
```console
> db.${collection}.insertOne()
```

Retorna os dados existentes na collection:
```console
> db.${collection}.find()
```
> Dentro comando find pode ser passado campos, valores, filtros e [projections](https://www.mongodb.com/docs/manual/reference/operator/projection/positional/) de quais documentos queremos buscar,
> no exemplo abaixo o banco retornará todos os documentos que possuem o campo "a" com o valor 123
> 
> [Link para documentação das querys no MongoDB](https://www.mongodb.com/docs/manual/tutorial/query-documents/)
```console
> db.${collection}.find({"a": 123})
```

Retorna apenas um registro:
> Comando semelhante ao find() porém retorna o primeiro registro encontrado
```console
> db.${collection}.findOne()
```

Formata o retorno dos dados existentes:
```console
> db.${collection}.find().pretty()
```

Retorna em ISODate o valor do timestamp do primeiro registro inserido:
```console
> db.${collection}.findOne()._id.getTimestamp()
```

Insere um ou vários documentos:
```console
> db.${collection}.insertMany([${document}, ${document}...])
```

> Também pode ser passado um campo ordered para que a aplicação continue a cadastrar os documentos caso ocorra algum erro,
> por padrão o campo ordered é *true*, caso seja *false* ele continua cadastrando no banco 
> os registros que estão corretos mesmo se algum deles não for cadastrado
```console
> db.${collection}.insertMany([${document}, ${document}...], {ordered: false})
```

Altera o valor de um documento existente:
```console
> db.${collection}.replaceOne({ ${field}:${value} }, { ${field}:${value} })
```

Altera o valor de qualquer documento correspondente ao filtro:
```console
> db.${collection}.updateMany()
```

Altera o valor do primeiro documento encontrado correspondente ao filtro:
```console
> db.${collection}.updateOne()
```

Deleta o primeiro registro encontrado:
```console
> db.${collection}.deleteOne()
```

Deleta um ou vários registros encontrados:
```console
> db.${collection}.deleteMany()
```

## Relacionamentos

**Embedded documents:**
> Em MongoDB, documentos incorporados (Embedded documents) referem-se à capacidade de incluir documentos BSON dentro de outros documentos BSON.
> Isso permite modelar dados de forma hierárquica e estruturada, simplificando consultas e operações de leitura e gravação. A principal
> vantagem é reduzir a necessidade de consultas complexas e junções, melhorando o desempenho e simplificando a lógica de consulta do 
> aplicativo. Além disso, documentos incorporados podem ser mais eficientes em termos de armazenamento e acesso em comparação com estruturas de
> dados normalizadas.

Exemplo de relacionamento "um para um" de um sistema de faculdade com embedded documents:

Cllection Aluno
```JSON
{
    "_id": 1,
    "cpf": 123456798,
    "nome": "Joãozinho",
    "cidade": "São Paulo"
}
```
Cllection Carteirinha
```JSON
{
    "_id": 8,
    "turma": "220A",
    "ra": 1245,
}
```

> Em bancos relacionais a normalização para o relacionamento desses dados seria criar uma coluna que ligue os dados da carteirinha com o aluno, por exemplo criar uma coluna "carteirinha_id" na tabela de aluno como 
> chave estrangeira para a tabela de carteirinha, porém para buscar as informações de carteirinha do aluno teria que ser realizado um JOIN.
> Em bancos não relacionais não é preciso criar um atributo para realizar o relacionamento, pois o uso de JOINS em consultas de banco resulta em uma perca de performance.
> Nesse caso é utilizado *Embedded documents* para que a consulta retorne as informações da carteirinha do aluno dessa maneira:
```JSON
{
    "_id": 1,
    "cpf": 123456798,
    "nome": "Joãozinho",
    "cidade": "São Paulo",
    "carteirinha": {
        "_id": 8,
        "turma": "220A",
        "ra": 1245,
    }
}
```

**Relacionamento "muitos para muitos":**

> Quando existe uma relação de "muitos para muitos", no banco relacional esse tipo de relação pode ser consultado através de uma tabela auxiliar que realiza a ligação entre as duas tabelas, porém no banco não 
> relacional ao invés de criar uma *collection* para relacionar essas informações, pode ser adicionado um array dentro das *collections* que precisam do relacionamento, por exemplo:

Collection Fornecedores
```JSON
{
    "_id": "f04",
    "cnpj": "165486984",
    "nome": "Fornecedor Legal",
    "cep": "1098465",
    "produto_ids": ["p16", "p21"]
}
 
{
    "_id": "f07",
    "cnpj": "98498151",
    "nome": "Fornecedor Maneiro",
    "cep": "198498",
    "produto_ids": ["p21", "p47"]
}
```

Collection Produtos
```JSON
{
    "_id": "p16",
    "descricao": "Panela",
    "preco": 45.50,
    "fornecedor_ids": ["f04"]
}
 
{
    "_id": "p21",
    "descricao": "Prato",
    "preco": 14,
    "fornecedor_ids": ["f04", "f07"]
} 
 
{
    "_id": "p47",
    "descricao": "Faqueiro",
    "preco": 127.46,
    "fornecedor_ids": ["f07"]
}
```

Se o fornecedor de algum dos produtos quiser alterar o preço do produto, pode retornar essas informações de preço do produto no relacionamento:

Collection Fornecedores
```JSON
{
    "_id": "f04",
    "cnpj": "165486984",
    "nome": "Fornecedor Legal",
    "cep": "1098465",
    "produtos": [
        {
            "_id": "p16",
            "preco": 46.50
        },
        {
            "_id": "p21",
            "preco": 12
        }
    ]
}
 
{
    "_id": "f07",
    "cnpj": "98498151",
    "nome": "Fornecedor Maneiro",
    "cep": "198498",
    "produtos": [
        {
            "_id": "p21",
            "preco": 16
        },
        {
            "_id": "p47",
            "preco": 127.46
        }
    ]
}
```

Collection Produtos:
```JSON
{
    "_id": "p16",
    "descricao": "Panela",
    "fornecedores": [
        {
            "_id": "f04",
            "preco": 46.50
        }
    ]
}
 
{
    "_id": "p21",
    "descricao": "Prato",
    "fornecedores": [
        {
            "_id": "f04",
            "preco": 12
        },
        {
            "_id": "f07",
            "preco": 16
        }
    ]
} 
 
{
    "_id": "p47",
    "descricao": "Faqueiro",
    "fornecedores": [
        {
            "_id": "f07",
            "preco": 127.46
        }
    ]
}
```

## Queries

### O que são queries?

> As queries de banco de dados são comandos ou instruções que você usa para interagir com um banco de dados. Elas permitem que você faça operações como buscar, inserir, atualizar ou excluir dados em um banco de dados. 
> As queries são escritas em uma linguagem específica, no caso do mongoDB é utilizada a linguagem MQL (MongoDB Query Language), e são essenciais para recuperar informações específicas ou manipular dados de acordo com as necessidades do usuário ou da aplicação.

**Exemplos de queries com Pokemon**

Busca simples no banco para retornar o primeiro registro encontrado:
```console
> db.pokemon.findOne()
```

Busca simples no banco retornando apenas campos específicos do documento, como nome e força de ataque:
```console
> db.pokemon.findOne({}, { name: true, attack: true, _id: false })
```

### Operadores de comparação

[Documentação dos operadores de comparação](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/)

| Nome do operador | Descrição da comparação |
| -------- | --------- |
| $eq  | Igual |
| $gt  | Maior |
| $gte | Maior ou igual |
| $in | No array |
| $lt | Menor |
| $lte | Menor ou igual |
| $ne | Diferente |
| $nin | Fora do array |

**Exemplos:**

Retornar pokemons com ataque **maior ou igual** a 85:
```console
> db.pokemon.find({attack: {$gte: 85} }, { name: true, attack: true, _id: false })
```

Retornar pokemons com ataque **menor ou igual** a 85:
```console
> db.pokemon.find({attack: {$lte: 85} }, { name: true, attack: true, _id: false })
```

Retornar pokemons que **tenham** os tipos fogo ou pedra:
```console
> db.pokemon.find({types: { $in: ["Fire", "Rock"] }}, {name: 1, types: 1, _id: 0})
```

Retornar pokemons que **não tenham** os tipos fogo ou pedra:
```console
> db.pokemon.find({types: { $nin: ["Fire", "Rock"] }}, {name: 1, types: 1, _id: 0})
```

### Combinação de operadores

Retornar pokemons que tenham a defesa **maior que** 60 e **menor ou igual a** 72:
```console
> db.pokemon.find({defense: { $gt: 60, $lte: 72 } }, {_id: 0, name: 1, defense: 1})
```

Retornar pokemons que tenham o hp **maior ou igual a** 80 e defesa **maior ou igual a** 80 **ou** o ataque **maior ou igual a** 80 e velocidade **maior ou igual a** 80:
```console
> db.pokemon.find({$or: [{hp: {$gte: 80}, defense: {$gte: 80}}, {attack: {$gte: 80}, speed: {$gte: 80}}]})
```

### Sort
> O sort() em MongoDB é um método que permite ordenar os documentos em uma coleção com base em um ou mais campos. 
> Você pode especificar se deseja ordenar em ordem crescente (ascendente) ou decrescente (descendente) para cada campo. Por exemplo:

Retornar pokemons ordenados de forma crescente pelo hp:
```console
> db.pokemon.find().sort({hp: 1})
```

Retornar pokemons ordenados de forma crescente pelo hp, defesa decrescente e ataque crescente:
```console
> db.pokemon.find().sort({hp: 1, defense: -1, attack: 1})
```

> No exemplo acima, a defesa e o ataque são critérios de desempate no caso do hp ser igual, caso haja duas linhas onde o hp seja igual, o sistema irá ordenar pela defesa de forma decrescente, caso o hp e a defesa sejam iguais, o sistema irá ordenar pelo ataque de forma crescente

### Paginação

> Para realizar a paginação das informações retornadas, no MongoDB é utilizado o limit() e skip().
> O limit() limita o número de documentos retornados, e o skip() pula os primeiros n documentos da coleção e retorna os documentos restantes, por exemplo se houver skip(5) e limit(5), será retornado os 5 documentos cadastrados após os primeiros 5 documentos

Retornar os pokemons do documento 6 ao 10 do tipo fogo, ordenados pelo ataque:
```console
> db.pokemon.find({ types: "Fire" }).sort({ attack: -1 }).skip(10).limit(5)
```

### Embeded documents

```JSON
{
  "_id": 102,
  "types": [
    "Ghost",
    "Poison"
  ],
  "name": "Gengar",
  "legendary": false,
  "battle_points": {
    "hp": 60,
    "attack": 65,
    "defense": 60,
    "speed": 110
  },
  "generation": 1
}
```

> Nessa estrutura de documento, para realizar queries buscando pelas informações contidas dentro de **battle_points**, pode-se utilizar dot notation para isso, por exemplo:

Retornar os pokemons que possuem hp maior ou igual a 60:
```console
> db.pokemon.find({ "battle_points.hp": { $gte: 40 } })
```

Nas consultas por embeded documents, deve-se utilizar aspas e dot notation para especificar os campos da consulta

## Updates

[Documentação dos updates](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Adicionar campo em documento ou alterar campo existente

Adicionar campo em documento existente, por exemplo, adicionar o campo startsWithO em pokemons que começam com a letra "O":

> Neste exemplo está sendo utilizado o updateMany que irá atualizar todos os registros que derem match  
> com a query do update, se utilizar o updateOne irá atualizar apenas o primeiro registro encontrado na query

```console
> db.pokemon.updateMany({name: /^O/}, { $set: {startsWithO: true} })
```

Alterar campo valor de campo existente, por exemplo, alterar todos os nomes dos pokemons que começam com a letra "O" para "Começa com O":

```console
> db.pokemon.updateMany({name: /^O/}, { $set: {name: "Começa com O} })
```

### Updates em arrays

[Documentação dos updates em arrays](https://www.mongodb.com/docs/manual/reference/operator/update-array/)

## Indexes

### O que são índices de banco de dados

> Índices de banco de dados são estruturas que ajudam a otimizar a recuperação de dados em um banco de dados. 
> Eles funcionam como listas ordenadas que armazenam referências para os registros de uma tabela, permitindo que o sistema de banco de dados localize rapidamente os registros relevantes durante consultas. 
> Essencialmente, os índices aceleram a pesquisa de dados, melhorando o desempenho das consultas e reduzindo o tempo necessário para recuperar informações específicas.

Ao realizar uma consulta no banco, quando não há indexação é verificado documento por documento para saber se o documento verificado corresponde com a query, se na collection onde é realizado a query houver muitos registros, isso pode causar lentidão no tempo de resposta da query.

### Comandos para índices

| Comando | Descrição |
| ------- | --------- |
| createIndex | Cria índices para a collection |
| getIndexes | Busca os índices existentes no banco |
| hint | Sugere qual índice vai ser utilizado |
| dropIndex | Deleta um índice do banco |

## Aggregation

### Conceito

> Em bancos de dados NoSQL, a agregação (ou "aggregation") refere-se ao processo de combinar e processar dados de várias fontes ou documentos em um formato útil para análise.
> Isso pode envolver operações como filtragem, agrupamento, soma, média e outras funções para obter informações específicas dos dados armazenados em um banco de dados NoSQL.


### Join combate de pokemons

> Aggregation é semelhante aos Joins de banco relacional, é utilizado para combinar os dados de documentos, imagine que você possui uma collection ***combats*** que guarda o combate entre
> dois pokemons, porém guarda apenas o id dos dois pokemons e o id do pokemon vencedor, por exemplo:

```JSON
{
    First_pokemon: 702,
    Second_pokemon: 701,
    Winner: 701
}
```

Note que o First_pokemon e Second_pokemon se referem ao id dos pokemons da collection ***pokemon***, podemos retornar as informações do primeiro pokemon utilizando o $lookup:

```JSON
db.combats.aggregate([
    {
		$lookup: {
			from: "pokemon",
			localField: "First_pokemon",
			foreignField: "_id",
			as: "pokemon1"
		}
	}
])
```
db.$collection.aggregate:

| Parâmetro | Descrição |
| ------- | --------- |
| from | Collection que contém os dados a serem consultados |
| localField | Campo utilizado da collection atual para a consulta |
| foreignField | Campo da collection do from para a combinação dos dados |
| as | Nome do campo que retornará a combinação dos dados |

Resultado da pesquisa de aggregation:

```JSON
{
    First_pokemon: 702,
    Second_pokemon: 701,
    Winner: 701,
    pokemon1: [
        {
        _id: 702,
        types: [
            'Grass',
            'Fighting'
        ],
        name: 'Virizion',
        legendary: true,
        hp: 91,
        attack: 90,
        defense: 72,
        speed: 108,
        generation: 5
        }
    ]
}
```

Também é possível buscar os dados do segundo pokemon:

```JSON
db.combats.aggregate([
    {
		$lookup: {
			from: "pokemon",
			localField: "First_pokemon",
			foreignField: "_id",
			as: "pokemon1"
		}
	},
    {
		$lookup: {
			from: "pokemon",
			localField: "Second_pokemon",
			foreignField: "_id",
			as: "pokemon2"
		}
	}
])
```

Resultado:

```JSON
{
    First_pokemon: 702,
    Second_pokemon: 701,
    Winner: 701,
    pokemon1: [
        {
        _id: 702,
        types: [
            'Grass',
            'Fighting'
        ],
        name: 'Virizion',
        legendary: true,
        hp: 91,
        attack: 90,
        defense: 72,
        speed: 108,
        generation: 5
        }
    ],
    pokemon2: [
        {
        _id: 701,
        types: [
            'Rock',
            'Fighting'
        ],
        name: 'Terrakion',
        legendary: true,
        hp: 91,
        attack: 129,
        defense: 90,
        speed: 108,
        generation: 5
        }
    ]
}
```

### Escolher quais campos retornar

É possível utilizar $project para definir quais informações quer que sejam retornadas, para que não retorne todas as informações, por exemplo, retornar apenas as informações do primeiro pokemon:

```JSON
db.combats.aggregate([
    {
		$lookup: {
			from: "pokemon",
			localField: "First_pokemon",
			foreignField: "_id",
			as: "pokemon1"
		}
	},
    {
		$lookup: {
			from: "pokemon",
			localField: "Second_pokemon",
			foreignField: "_id",
			as: "pokemon2"
		}
	},
    {
        $project: {
            pokemon1: {
                $arrayElemAt: ["$pokemon1", 0]
            }
        }
    }
])
```

### Condições para retorno dos dados

É possível utilizar $cond para definir uma condição para o retorno dos dados, nesse caso podemos utilizar o cond juntamente com o project para retornar apenas o pokemon vencedor:

```JSON
db.combats.aggregate([
    {
		$lookup: {
			from: "pokemon",
			localField: "First_pokemon",
			foreignField: "_id",
			as: "pokemon1"
		}
	},
    {
		$lookup: {
			from: "pokemon",
			localField: "Second_pokemon",
			foreignField: "_id",
			as: "pokemon2"
		}
	},
    {
        $project: {
            _id: 0,
            first_pokemon: {$arrayElemAt: ["$pokemon1.name", 0]},
            second_pokemon: {$arrayElemAt: ["$pokemon2.name", 0]},
            winner: {
                $cond: {
                    if: { $eq: ["$Winner", "$pokemon1._id"] },
                    then: {$arrayElemAt: ["$pokemon1.name", 0]},
                    else: {$arrayElemAt: ["$pokemon2.name", 0]},
                }
            }
        }
    }
])
```
$cond:

| Parâmetro | Descrição |
| ------- | --------- |
| if | Condição |
| then | Retorno caso a condição for verdadeira |
| else | Retorno caso a condição for falsa |

No caso acima, para retornar o pokemon vencedor, é realizado a condição de comparar se o campo Winner é igual ao campo _id do pokemon1, se for igual, é retornado o nome do primeiro pokemon, senão é retornado o nome do segundo pokemon.