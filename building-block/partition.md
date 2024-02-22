## Partition

Different ways to partition
* Horizontal partition
* Vertical partition
* Range based
* Hash based

### Decision partition key
* partition key based on query pattern

## Decision to use local index (sort key)

### Decision rebalancing strategy
* Automatic by a co-ordinating node
* Manual by an adimistrator
  * Add more virtual node for the non-hot key partition
  * Add more nodes (node provsisioning)
* Salting the partition key

### Decision to use secondary index or global index
* Based on access pattern
#### Challenges
* Write overhead
* Storage overhead
* Fault tolerance of the index

### Tradeoff
#### Benefits
* Horizontal scale with commodity server (CPU as well as disk)
* Backups are more simple and in chunks
* Partial availability

#### Challenges
* Existence of a hot spot or skew due to query pattern or incorrect partition key
* Availability and consistency during rebalancing
* Scatter Gather


