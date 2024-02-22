# Cassandra
* Provides availability and partition tolerance (AP)

## 1a. How is it implemented internally ?
LSM tree (append only)

## 1b. How is request routung performed ?
* Client is cluster topology aware
* For the first time client connects to a seed node and discovers the layout.

## Conflict resolution
* Uses timestamps based (Last write wins)

## Benefits
* Leaderless architecture
* No single point of failure

## Challenges
* Garbage collection (halting operations)
* Large memory footprint
* Slow startup
* Just-in-time warmup and complex configuration
* LWW based conflict resolution

## Analytics
Has integegration with map reduce and spark (HDFS)

## Issues
### Update and delete happening concurrently
* Cassandra update are insert
* So if the id doesnt exist it would insert a new row with rest of the values as NULL and only updated value

## Partition size
* technical limit 2 GB
* recommended limit: 100 MB

## Use Case
* Time series data
  * Ad data is fetch per column and rarely for the entire row
  * Data compression
 
## References
* [Versioning](https://cassandra.apache.org/doc/latest/cassandra/architecture/dynamo.html#data-versioning)
