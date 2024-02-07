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