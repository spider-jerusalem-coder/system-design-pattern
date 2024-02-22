# Streaming

### 1. What are the problems in big data processing ?
#### Pure offline jobs
Query on OLAP (data warehouse)
#### Pure online jobs
Realtime dashboard, surge pricing
#### Dual footprint jobs:
* Backfill: Offline reprocessing fixes to fix the issues with realtime processing
* Offline experimentation and testing: Before taking the job online
* Online and Offline feature generation: for ML
* Bootstrapping state: Before starting the online jobs 

### 2. What are the two architectures for Backfill ?
1. Lambda architecture
> Combination of offline and online jobs (map reduce similar to map reduce and streaming for online) 
2. Kappa architecture
> Leave the data in Kafka for longer and rewind back and replay

### 3. What are challenges in lambda architecture ?
1. Different APIs for batch and stream (code need to be synced)
> Compensate by unified API based on mode
2. Jobs in streaming when executed in batched job would require more resources
> Compensate it with mini batch and then have to take care of windowing issue

### 4. What are challenges in Kappa architecture ?
* Disk is limited
* Not designed to be data warehouse (no index)
> Store data in HDFS and load from HDFS into Kafka before replaying but order is not maintained.

### 5. What is Kappa+ ?
#### a. Stateless job
* No windowing. Memory not a concern
* Data order do not matter
* Example: transform, filter, projection
#### b. Aggregations (low-medium memory)
* Retain only value
* Order is important but solvable without strict total order
* Example: sum, avg, max, min, reduce
#### c. Windowed retention (High memory)
* Holds the data until window expires
* Joins, distinct count, pattern analysis within window 
#### d. Global retention
* Sorting enture input data. Joins without windowing
* No realtime jobs
* Pure offline jobs

### 6. What is the processing model for Kappa+
* Older partitins first
* Concurrent read from the same partition
* Emit watermark at the end of a partition

### 7. How to implement reading from one partition at a time ?
#### File selector
1. Source emits files in the zookeeper
2. Create znode for the given partition
3. Bulk create child znodes for each files
4. Emit watermark at the end of the partition

#### File reader (reads concurrently N)
1. Deserialize the files
2. Inform zookeeper on completing file to delete the childznode of the file. Delete the znode if the partition is empty

## References
* (Uber Kappa+ presentation)[https://www.youtube.com/watch?v=4qSlsYogALo]
