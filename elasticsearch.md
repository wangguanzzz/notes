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
