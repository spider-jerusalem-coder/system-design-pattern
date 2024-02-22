## Aurora

## Generic database
### 1a. What is transaction
Sequence of operation which can be clubed together as atomic. All or nothing.

### 1b. How is transaction written to disk
Its maintained in-memory and written to WAL logs and then only written to disk.  Accumulates

### 1c. What is stored in the WAL for the transaction
ALso store the previous value in the transaction which is used for undo/revert

### 1d. How is transaction which is not flushed to the disk  recovered after commit ?
* The recovery reads from the WAL which has commit and has not being written to disk

## EBS
* EC2 --> EBS volumen --> EBS volumne --> ...
* EC2 holds the data in-memory and when page writes happen the data sent to the EBS volumne
* Request from EC2 to EBS volumne over network

### 1a. If an EC2 dies what is the recovery plan
A new EC2 is spin up and the EBS volumnes with attached disk is then attached to the new EC2

### 1b. What replication is used by EBS
Chain replication

### 1c. How is it better than running on EC2
Its fault tolerant

### 1d. Challenges regarding it
* Only one ec2 instance can attach EBS volumne (network based storage system).
* Sending large data over network.
* EBS are all stored in one DC by AZ so if a dc fails then not fault tolerant.

### 1e. What is the upgraded version of EBS
Aurora

## RDS
* One EC2 instance and EBS volumnes in one availability zone
* Whenever a page write is sent to the EBS volumne in one DC, its also sent to another DC
* DC1 (EC2 --> EBS1 -> ...) and DC1 (EC2) --> DC2 (EC2 --> EBS1 --> ..)
* The write is finished whem data is synced in a different availability zones

### 2a. What is the challenge
* Large data size in KB would have high network bandwidth cost
* Page sizes are large as page are batched with all the data.
* Storage nodes concept instead of EBS.
* When a new log arrives, each page has a log
* Old version of page and sequence of logs per page
* If cient request for updated copy pf thr page then log is applied

## Aurora
* **WAL**: Data sent over is only log entries
* **Quorum scheme**: Uses quorum to ignore the slowest nodes
* **More replicas across multiple DC**

### 3a. What is the goal of Aurora
* Write with one DC(AZ) is down
* Read with one DC (AZ) + one node in another DC
* Transient slow nodes
* Fast re-replication (as failures are connected and its a race against time)

### 3b. What is Quorum replication
* `R + W = N + 1` (write nodes must overlap with read nodes)
* Example R = W = 2 and N = 3
* Using version number to decide the latest value
* Three DC, each with two nodes
* N = 6, R = 3, N = 4 (implying one DC is down and still can write)

### 3c. How is read performed in Aurora
* Client first fetch from the in-memory buffer in EC2. If the page is not latest version or the page is not present then fetch from the storage node
* The storage node then applies the logs and update the latest page in the buffer cache
* Their is no quorum read from application

### 3d. When do we do quorum read in Aurora
In case of fault recovery, Find the first missing log entry, then db sends request to discard all entries after this log entry one. These are the uncommitted transactions

### 3e: How does it regain from crash
Have protection group to horizontal scale. Exame PG1: S1, S2, S3 | PG2: S22, S33, S44. Each storage node has a segment of data similar to partition in a node. When a node is recoverying, all the segments are read from each portection group in parallel.

### 3f. How is read scalable
Database node is also replicated and each replicated database node would cache data. Aurora also sends the WAL to the replicated database node.

### 3g. How is write performed
Writes occurs through one database node

## References
* [MIT FAQ](http://nil.csail.mit.edu/6.824/2020/papers/aurora-faq.txt)

