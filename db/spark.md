# Spark
Upgraded version of map reduce.

### 1a. Why page rank doesnt work well with map reduce
* Mutiple steps
* No iteration in mapreduce (can perform doing n map reduce and is slow)

### 1b. What is Spark reading of a file means
* Execute on each chunk of the file stored in HDFS like.

### 1c. What does the code do
Build a lineage graph. Lineage graph is a sequence of processing states.

### 1d. What does `collect()` do in spark
* What does collect do
* Takes a lineage graph
* compile and create java byte codes
* send to each worker and then
* collect fetch all the data from workers

### 1e. What does `distinct()` do in spark
Shuffle and sort and then split across all workers

### 1f. What does `groupByKey` do in spark
Shuffle and group based on key. Spark already knows that each worker has all key in on worker so it would use that. Result: u1: [u2, u5, u6]

### 1g. Calling `collect` reruns from the start so how to optimize this
Use `cache()` which persist the data in-memory

### 1h. How does spark work
* Input data is pre-partitined into HDFS (64MB)
* Might run the computation on the same machine or another worker.
* Spark streams the data from node to node and apply these functions.
* **read**: Read these chunk data
* **map**: Local operation in the same worker

### 1i. Advantage over map reduce
* Read from the memory

### 1j. What is narrow transformation
Occur local worker. `read`, `map`

### 1k. What is wide transformation
Across all worker (similar to reduce), `distinct`
Example: From narrow to wide, the data is hased per key to a given reducer, So that each reducer gets a chunk of the data. Expensive as TB of data across network. 

### 1l. WHat is optimization it runs
* Inspect Lineage graph and run all narrow in one worker and run sequential calls
* Data is already shuffled for a wide transformation. The next wide shuffle is command then do not need to reshuffle again.

### 1m. Failed worker in wide command
* lineage narrow1 --> narrow2 --> wide1
* As spark move through a set of narrow transformation as it doesnt persist
* If one wide transformation worker fails then all narrow transformation have to run to get the data for the wide transformation worker. We want to do relative less work. that is why spark allows to do checkpoint. After a set of narrow transformation then persist it in HDFS.
* Their is always be a failure hence the model of immutable and reconstructible

### 1n. What are the benefits of spark
* using `cache()` persist the data for future iteration in-memory
* Immutable
* Make the data flow explicit and optimization using lineage graph
* Performant - leaving the data in-memory rather then storing it in HDFS (these RDDs)

### 1o. How does spark implement streaming
* Using micro batches

### 1p. What is RDD
* Immutable data created after a given transfrmation
* RDDs themselves in Spark are now somewhat
deprecated: Spark has recently moved towards something called "DataFrames" (which implements a more column-oriented representation while maintaining
the good ideas from RDDs.)

### 1q. How do applications figure out the location of an RDD?
The application names RDDs with variable names in Scala. Each RDD
has location information associated with it, in its metadata (see
Table 3). The scheduler uses all this information to colocate
computations with the data. An RDD may be generated by multiple nodes
but it is for different partitions of the RDD.

### 1r. How to do faster operation in spark scala ?
Using `.par` which does parallel processing using multiple threads. This is not particular to Spark. Example
```python
data_list = []
par_date_list = date_list.par
map(len, par_date_list)
```

### 1s. How does join work in spark ?
* Spark provides three types of joins (inner, outer, semi and anti)
* Its a wide transformation
* Key on which the join is performed is hashed and the key available in both datasets are send to the same node to be joined together.

### 1t. What are the issues with join operation in spark ?
* Skew where one key dominates leading to one node getting large data set then other

### 1u. How to optimize join in spark ?
* **Broadcast join**: if one dataset is smaller and the other is larger then the smaller one can be broadcasted to every node (given smaller can fit in-memory)
* **Sort Merge join**: If the dataset is sorted on the join key, then rather then shuffling it could be merged
* **Partition pruining**: If Spark knows the partitioner of the datasets, it can avoid shuffling all partitions. For example, if both datasets are hash-partitioned with the same partitioner, Spark can just join the corresponding partitions

### 1v. What are transformations and actions in spark ?
**Actions** are operations that trigger the execution of the transformation pipeline built by Spark. They result in data being brought back to the driver program or saved to storage. Examples include count, collect, save, etc.
**Transformations** in Spark are lazy, meaning they are not executed immediately. Instead, Spark builds up a plan of these transformations, forming a Directed Acyclic Graph (DAG) of operations.

## Refferences
* [MIT FAQ](http://nil.csail.mit.edu/6.824/2020/papers/spark-faq.txt)