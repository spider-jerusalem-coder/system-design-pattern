## RDB

#### Benefits
* ACID
* Complex queries and reporting (adhoc)
* Structural data model
* **Referential integrity**
 
#### Challenges
* **Vertical scaling limitation**
  * Supports vertical scaling better than horzontal scaling
* **Limited Horizontal Scaling**
  * While some modern RDBMSs support horizontal scaling (sharding), it's often more complex and less efficient than with NoSQL databases.
* **Struggle to support partition tolerance**
  * Default configuration support CA
* Fixed schema
* Not best for unstructured data
* Performance overhead
  * Join is expensive for large data set
  * ACID compliance is expensive and can reduce availability
* Performance overhead with partitioning
  * Relational features that include table joins, transactions, and referential integrity require steep performance penalties in sharded deployments.


#### When to use
* Data is structured and requires joins to fetch
* Data is centralized in one region and can be replicated async across regions
* Can use large, high-end hardware
* Workload is thousands of transaction per second
