## 13
cluster -> nodes(ES instances)

document : json format

index -> a set of documents

## 14
check cluster health
```elasticsearch
GET /_cluster/health
_cluster #API
health #COMMAND
```

```
#_cat is user readable API
GET /_cat/nodes

GET /_cat/nodes?v
# ?v filter, there is the verbose option

GET /_cat/indices?v
```

## 16 concept of shard
sharding -> divide indices into smaller pieces, kind of an independent index (lucene index), shard of a index could scattered on multiple nodes.

split & shrink API for shards

## 17 shard replication

replication is configured at the index level

primary <-> replica shards, which is stored on the different nodes from primary

* snapshosts ; at index or cluster level



check shards
```
GET /_cat/shards?v
```
**auto_expand_replicas** change the number of replica according to node number

## 21 create and delete index
example, create index
```
PUT /pages
```
delete index
```
DELETE /pages
```
create index with specify the shard and replica number
```
PUT /products 
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}
```
## 22 store a document in index
example 
```
POST /products/_doc
{
  "name": "coffee",
  "price": 64,
  "in_stock":10
}
```
return _id is the identity

we can also explicitly provide the ip ( option: action.auto_create_index), this is the best practice
```
PUT /products/_doc/100
{
  "name": "toaster",
  "price": 102,
  "in_stock":2
}
```
## 23 retrieve document by ID
```
GET /products/_doc/100
```
return is the found key and _source key

## 24 update document
udpate existing key
```
POST /products/_update/100
{
  "doc": {
    "in_stock": 100
  }
}

```
add new key
```
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}

```
**document is in fact immutable, see _version, _seq_number**

##25 scripted updates
example: directly reduce in stock by 1
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}

```
example: use parameter
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock = params.quantity",
    "params": {
      "quantity": 400
    }
  }
}
```
special usage
ctx.op = 'noop' /'delete'

## 26 upsert ( if existed ,update, if now , create)
```
POST /products/_update/101
{
  "script": {
    "source": "ctx._source.in_stock ++"
  },
  "upsert": {
    "name": "blender",
    "price": 399,
    "in_stock": 50
  }
}
```
## delete
```
DELETE /products/_update/101
```
## query
search
```
GET /products/_search
{
  "query": {
    "match_all": {}
  }
}
```
update with query
```
POST /products/_update_by_query
{
  "script": {
    "source": "ctx._source.in_stock--"
  },
  "query":{
    "match_all": {}
  }
}
```
delete with query
```
POST /products/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

## batch processing
NDJSON specification

note: index (if exist, then udpate), create (if exist , then error)
example
```
POST /_bulk
{"index": { "_index": "products", "_id": 200}}
{"name": "machine1", "price":99, "in_stock": 5}
{"create": { "_index": "products", "_id": 201}}
{"name": "machine2", "price":199, "in_stock": 15}

```

can use change API to omit the _index
```
POST /products/_bulk
{"index": {  "_id": 200}}
{"name": "machine1", "price":99, "in_stock": 5}
{"create": {  "_id": 201}}
{"name": "machine2", "price":199, "in_stock": 15}

```
**for NGJSON the last line also must have \n **

## 4.0 analysis
**_source object is not used when searching documents**
Document => Analyzer => Storage
Analyzer has
* character filters (0->N)
  eg. remove html tags html_strip: 
```html
<h1>hello world</h1>

hello world
```
* Tokenizer (must 1) ,
tokenizer records also the string offset
```
#input
"I REALLY like beer“
#output
["I", "REALY", "like", "beer"]
```

* Token filters ( 0->N)
lowercaes filter
```
#input
["I", "REALY", "like", "beer"]
#output
["i","really","like","beer"]
```
## 4.1 analyzer API
```console
POST /_analyze
{
  "text": "Chris is a good guy! :-)",
  "analyzer": "standard"
}
```
customizing the char_filter, tokenizer, and filter
```console
POST /_analyze
{
  "text": "Chris is a good guy! :-)",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}
```
## 4.2 inverted index
mapping between terms and which documents contain them
an inverted index is created for each "text" "field"
name and description below has two inverted index
```
{ 
  name: "xxx"
  description: "xxx" 
}
```
numeric, date and geospatial fields are stored has BKD trees.
## 4.3 mapping
mapping defines the structure of documents (like DB schema)
## 4.4 data types
1. object : json object, can be nested, all document indexed is object,
we use "properties" field for object rather than "type"
object stored internally is flatterned json

array index: field is flatterned , array fields are duplicated as array

2. nested : useful indexing array of objects; must use nested query;
nested object are stored as hidden documents

3. keyword: used for fields you want to match for exact values; for sorting , filtering, and aggregations
( full text searches, use text instead); 

unlike text, keyword are analyzed with the keyword analyzer(do no-op)

```console
POST /_analyze
{
  "text": "Chris is a good guy! :-)",
  "analyzer": "keyword"
}
```

4. coercion
coercion will force the index field to be converted into the expected type;

notice the _source object will use the original value, 
coercion is enabled by default
```console
PUT /coercion_test/_doc/1
{
  "price": 7.4
}
PUT /coercion_test/_doc/1
{
  "price": "7.4"
}
PUT /coercion_test/_doc/1
{
  "price": "7.4m"
}
```

5. array, array is not a data type, it is by default supported,
for string, it is stored as concatenated with " "
```console
POST /_analyze
{
  "text" : ["Strings are simple", "merge together"]
  "analyzer": "standard"
}
```

## 4.9 create explicit mapping
```console
# create reviews index with mapping
PUT /reviews
{
  "mappings":{
    "properties": {
      "rating": { "type": "float"},
      "content": {"type": "text"},
      "product_id": {"type": "integer"},
      "author": {
        "properties": {
          "first_name": {"type": "text"},
          "last_name" : {"type": "text"},
          "email: {"type": "keyword"}
        }
      }
    }
  }
}
```
## 5.0 retrieve mapping
```console
GET /reviews/_mapping
GET /reviews/_mapping/field/author.email
```
## 5.1 use dot convention for 4.9 example
```console
# create reviews index with mapping
PUT /reviews
{
  "mappings":{
    "properties": {
      "rating": { "type": "float"},
      "content": {"type": "text"},
      "product_id": {"type": "integer"},
      "author.first_name": {"type": "text"},
      "author.last_name" : {"type": "text"},
      "author.email: {"type": "keyword"}
      }
    }
  }
}
```

## 5.1 add a new filed in mapping
```
PUT /reviews/_mapping
{
  "properties" : {
    "created_at" : {
      "type": "date"
    }
  }
}
```
## 5.2 dates in elasticsearch
elasticsearch suuport 
* date without time
* date with time (ISO 8601)  e.g **2015-04-15T13:07:41Z** z is the time zone
* milliseconds since the epoch (long)  (this is the internal stored format)

## 5.3 missing fileds 
**all fields in ES are optional**

integrety is handled in applicaiton level

## 5.4 mapping parameters
1. format : used to customizing date (use the default format whenever posssible)
2. object nested fields
3. coerce. used to enable or disable coercion of values, it can be field or index level
4. doc_values, (antoher data structure from inverted index term-> doc)  doc-> terms; if it is used, both is maintained; it can be disable by false, when no aggregation, sorting, or scrpiting needed.
5. norm. used for relevance scoring, rank, it can be disabled, if no ranking needed
6. index. disables indexing for a field, values are still stored in _source, it still can be used for aggregation
7. null_value, replace null value to a default value
8. copy_to parameter, copy multiple fields values into a "group field" , eg first_name, last_name => full name

## 5.5 update existing mappings
for example. product_id include letters
from long to keyword is not allowed , as the field is already indexed

## 5.4 reindexing documents with the Reindex API
```
POST /_reindex
{
  "source" : {
    "index": "source_index"
    "query"(optional): {
    }
    "_source" : ["content" , "a", "b" , ...]
  },
  "dest" : {
    "index": "destination_index"post 
  },
  "script (optional): {
    "source": """
      ..
    """
  }
}
```
* in order to change the data type in the source, need to add the source segment,
* it is ok to add query to only reindex partial documents
* source filter , that remove the not needed fileds
* using script can rename the property name

## 5.5 defining the field alias
sometimes it is not worth to reindex just for rename the field

## 5.6 multi-field mappings
in below way , the property indredient will be handled both as text (analyzer- inverted index), and keyword (index)
```
PUT /multi_field_test
{
  "mappings": {
    "properties" : {
      "ingredients" : {
        "type": "text"
        "fields": {
          "keyword" : {
            "type": "keyword"
          }
        }
      },
    }
  }
}
```

## index template

index template is defined for every day or month indcies.
if the setting or mapping is availabe in configuration of indcies, they will merge and index setting will override.

retrive or delete the index template
```
get /_template/access-logs
```

## ECS ( Elastic Common Schema)
apache log is called apache2.access.url; nginx is called nginx.access.url;
ECS means that common fields are named as the same thing, eg @timestamp

## dynamic mapping
