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
* character filters
* Tokenizer
* Token filters
