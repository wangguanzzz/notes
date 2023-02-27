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

## RS
primary node, secondary node, arbitor
* when start mongod, need to add --replSet <rs_name>
* when RS is all up (3 nodes)
rs.initiate({
    "_id": <rs_name>,
    "version": 1,
    "members":[
        {“_id": 0, "host": "localhost:27017"},
        {“_id": 1, "host": "localhost:27018"},
        {“_id": 2, "host": "localhost:27019"}
    ]

})