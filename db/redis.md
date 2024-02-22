# Redis

### 1a. When is Redis CP (consistent and partition tolerant)
Redis on single server is CP (consistent and parttion tolerance)

### 1b. Does redis provide linearzable consistency ? Why ?
Yes. Its single threaded.

### 1c. How many partitions Redis support ?
* 16K slots

### 1d. Does redis support automatic rebalancing ?
No. Support manual rebalancing for adding or removing nodes

### 1e. What are the benefits of using redis ?
* **Higher throughput at lower latency** – Redis shards data across nodes in the cluster.
* **Cost-effective** – Usage of Lua script reduces unnecessary data transfer overheads.
* **Better at handling spiky traffic** rate limiting workloads at scale.

### 1f. What are the challenges of using redis ?
* Any long-running Lua script can cause other Redis commands to be blocked until the script completes execution.
> Thus, it’s optimal for the Lua script to execute in under 5 milliseconds

> Additionally, the current time is passed as an argument to the script to account for potential variations in time when the script is executed on a node’s replica, which could be caused by clock drift.

### 1g. What is the default number of channels supported by redis pubsub
Support 10k channels by default

### 1h. What kind of delivery policy redis support (At-least-once | At-most-once | Exactly-once)
At-most-once

### 1i. How does redis client know if a key is has reached its TTl ?
* TTL based trigger. Key based
* TTL expire events are published using pubsub

## 1j. Describe Redis cluster management process of fetching data ?
* Initial connection
* When client connects to a cluster its unaware of the topology of the cluster
* So connect to any node
* Cluster nodes command
* Client can request all information about all cluster
* ASK command
* If a node do not have information of the key, it would reply back with a ASK IP and port for the node which would have the key

## What protocol Redis uses for communication between nodes in a cluster
Gossip protocol

