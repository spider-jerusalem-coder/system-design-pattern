# Zookeeper
* cordination service
* runs on zab === raft
* clients talk to zookeeper and and zab layer work with replica with logs

## Linearizability
### 1a. Does adding more servers makes it faster for a linearizable system ?
No its inversely proportioanl as we add more nodes, the leader has to contact all the replica nodes. As leader is the bottleneck. All request go through the leader.

### 1b. For a linearizable system can reads happen from replica ?
* As its quorom based and the leader need to find the majorityn(stale data)
* Replica node might not have recieved the commit command
> Linearizable system  forbids that

### Zookeeper
## 2a. Does zk allow read request from replica node ?
* yes as it doesnt guarantee linearizibility

### 2b. What does zk guarantee ?
* linearizibility writes
* FIFO client order (writes/read client specified order)
  * If client writes to the leader and immediately tries to read from a replica
  * It has to stall until all data is committed
* Client read doesnt not go back in time (zxid). Doesnt matter which replica it connects to. Its similar to snapshot isolation

### 2c. Give an example of client writing to zookeeper ?
```
* delete('ready')
* write f1 (f1, f2, ready are filenames)
* writr f2
* create('ready')
```

### 2d. Give an example of client reading from zookeeper ?
```
* exists('ready')
* read f1
* read f2
```

### 2e. How does zookeeper handle session where data read is stale in it ?
```
* create('ready')
* -- read f1
* --- some time passed
* delete('ready')
* write f1
* write f2
* read f2
```

The client would get a stale f1 and a new f2
> To counter this zk has watches notfying the client whenever a write operation occur for the 'ready' file.

> zk also will notify the client if the replica node crashes and the client has to refetch all the data as the watcher is not replicated to another node.

### 2f. What are use cases of zookeeper
* Test-and-Set Service (lock)
* Configuration
* Who is the current master

### 2g. What is the API structure of Zookeeper
* Folder heirarchy example (folder1/x, folder2/y, folder3/a, ...)
* folder1, folder2 are called znodes. Every znodes have a version.
* When requesting data API is /folder1/x
* flag: seq = When a file is created, a monotoic increasing sequence number is appended to the file

### 2h. What is ephemeral znodes ?
The znodes are deleted if the client is not alive. Hence the client has to send heartbeat to keep the znodes active.

### 2i. What are the RPC's provided by zookeeper ?
* `create(path, data, flag=regular | ephemeral | seq)`
  * exclusive create client would know if they created the file
* `delete(path, znode_version)`
* `exist(path, watch: bool)`
  * If anything changes about the path the client is notified
* `get(path, watch)`
 * If anything changes about the data the client is notified
* `set_data(path, data, znode_version)`

## Zookeeper application
### 3a. Why not use a KV store for maintaining key and value and counter ?
File with a key and value. Need to increment the count

Using KV store, x = get(k), put(k, value+1)
> issue is its not atomic

### 3b. How does zookeeper handle writing into the latest version of a value?
Using Zookeeper we have an issue as get would return stale data. To counter this issue
```python
 while True:
   x, version = get("f")
   if set_dat(f, x_1, version) # if the latest version is version, if not then go back to the loop
    break
```
### 3c. How does zookeeper handle writing into the latest version of a value - optimized?

#### Lock
Acquire
```python
   if create("f",, epehem=T):
     return  # if the file is created  then return implying lock is acquired
   if exists("f", watch=T)
      # wait for notification # wait for the lock to be released when file is deleted
      # their is if coz if the file doesnt exist it would not go in the if
    goto the top
```
> Issue leads to thundering herd effect where in each loop only one client gets lock (O(n^2))
> Non scalable lock as number of clients increases

### 3d. Implement scalable lock which compensate for herd problem
```python
create_file("f", seq=T, ephem=T) # system provided a sequence number
list_files("f*")
# if no lower number then what was provided then return (lock acquired)
# its setting up the order in which the lock would be given
if exists("f", next lower number file, watch):
  wait
goto list_files
```
> This avoids her problem as each client is only waiting for the number before my sequence number
> Example a client has sequence 500 and is waiting for 499 and in the next check if 499 client died then it will wait for 488
> Client 500 will get a chance when all clients prior has acquired lock implying no files exist.

### 3e. Give example of using Zookeeper
* Store parition metadata
* Node registry
* Client lookuo

#### Store partition metadata
```
/database_cluster_name/partition1/nodeA
/database_cluster_name/partition1/nodeB

Contents of nodeA
{ ip: "", size: 32GB, status: "active"
```

#### Node registry 
```
/services/payment_service/instance1
{"address": "192.168.1.10", "port": 8080}
```
> Ephemeral znodes are special because they automatically get deleted when the session that created them ends (e.g., if the service instance crashes or loses its connection to ZooKeeper).

#### Client Lookup
```
/database_cluster_name/partition1/nodeA
{ node: "nodeA", status: "active"
```


## References
* [MIT FAQ](http://nil.csail.mit.edu/6.824/2020/papers/zookeeper-faq.txt)
