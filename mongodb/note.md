## type of nosql database
key value store, wide-column stores(hbase,cassendra), graph based(neo4j), document-oriented (mongodb)
## difference between mongodb & RDBMS
table <-> collection
row <-> docuemnt
column <-> field
joins <-> embedded documents, linking

## CAP theorem
consistency, availability, partitionn tolerance : three choose two
C: all clients see the same data at the same time, request recieve the most recent write
A: any client making a request for data gets a response
P: a communication break within a distributed system, the system continue to operate
CA: RDBMS
CP: MongoDB, HBase, Redis
AP: CouchDB, Cassandra, DynamoDB

## start db comand
chriswang@chriswang-RedmiBook-Pro-14S:~/Downloads/mongodb/mongodb-linux-x86_64-ubuntu2204-6.0.4/bin$ ./mongod --dbpath /home/chriswang/mongodb/data/ --logpath /home/chriswang/mongodb/log/mongodb.log --fork


# mongdb concept
database - database
collection - table
view - view
capped collections - fixed-size collection, used as a buffer or queue

## mongoDB database tools
binary import/export: mongodump, mongorestore, bsondump
data import/export: mongoimport, mongoexport
diagnostic tools: mongostat, mongotop
gridfs tools: mongofiles

## aggregation
1. $match stage
```
db.articles.aggregate({"$match": { "author": "dave" } })

```
2. group stage
```
raw data:
{"_id": 1, "item": "abc", "price": NumberDecimal("10"), "quantity： 10}

db.sales.aggregate([
    {
        "$group": {"_id": "$item", "totalSaleAmount": {"$sum": {"$multiply": ["$price", "$quality" ] }}}
    },
    {
        "$match": { "totalSaleAmount": {"$gte": 100} }
    }
])
```
3. project stage
```
# only keep title and author fields
db.books.aggregate(
    { "$project": {"_id": 0 ,"title": 1, "isbn" : {"publisher": { "$substr": ["$ibsn", 0, 3]}  , "lastName":"$author.last"}}}
)
```
4. collstats stage
latencyStats - reports latency stats
storageStats - storage stats
count - totoal number of documents
queryExecStats - total number of query

```
db.lease.aggregate(
    [
        {"$collStats": { "latencyStats":  {"histograms": true} } }
    ]
)

db.lease.aggreate(
    [
        {
            "$collStats": {"storageStats": {"scale": 1024*1024, "count": {} }}
        }
    ]
)
db.lease.aggregate(
    [{"$collStats": {"queryExecStats": {} } }]
)
```
5. indexStats stage
```
db.lease.aggregate(
    [{"$indexStats": {} }]
)
```

## RS
primary node, secondary node, arbitor
* when start mongod, need to add --replSet <rs_name>
* when RS is all up (3 nodes)
```
rs.initiate({
    "_id": <rs_name>,
    "version": 1,
    "members":[
        {“_id": 0, "host": "localhost:27017"},
        {“_id": 1, "host": "localhost:27018"},
        {“_id": 2, "host": "localhost:27019"}
    ]

})
```

## write concern
```
db.products.insert({"item“： ”something"}, {"writeconcern": {"w":"majority","wtimeout": 100}})
```

## read preference
```
cursor.readPref(mode, tagSet, hedgeOptions)
Mongo.readPref(mode, tagSet(array of pref), hedgeOptions)

db.products.find().readPref("secondary",[{"datacenter": "B"}])

```
