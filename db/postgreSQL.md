# PostgreSQL

### 1. Name a middleware in postgreSQL for connection pooling
* PGBool (routing and loadbalancing)
* PBBouncer

### 2. How much QPS does one postgreSQL can handle
* Single node for 100K / sec transactions
* Replication or distributed db with SQL for > 100K / sec transaction

### 3. How to add index without locking the table / rows ?
* Locks the table for all writes or updates
* To prevent locking the table use `CONCURRENTLY`
```
CREATE INDEX CONCURRENTLY sales_quantity_index ON sales_table (quantity)
```

### 4. How to add new FK constraint in PostgreSQL ?
* Lock the table when adding a new constraint to check for validity of the constraint
* Below query will not check or lock the writes or updates on the table
```
ALTER TABLE distributors ADD CONSTRAINT distfk FOREIGN KEY (address) REFERENCES addresses (address) NOT VALID;
```

### 5. How to make postgresql a history table
* [implementing-system-versioned-tables-in-postgres](https://hypirion.com/musings/implementing-system-versioned-tables-in-postgres)
* Using stored procedure calls

#### Adding NOT NULL constraint
```
ALTER TABLE table_name ALTER COLUMN column_name SET NOT NULL;
```
* This will scan the whole table
* Instead add constraint with no check
* Validate the constraint
* Add a checke

### 5. What are the types of lock in PostgreSQL ?
* Table lock: `ACCESS EXCLUSIVE LOCK`
* Row lock: `SHARE UPDATE EXCLUSIVE`

### 6. How to convert it to be a distributed database
using Citus

### 7. What need to be used with postgresql for CDC
* Debzeium

### 6. What are the different Isolation levels ?
| Isolation level | Dirty Reads | Non-repeatable reads | Phantom reads | Serializable Anamolies | 
|-----------------|-------------|----------------------|----------------|-----------------------|
| Read uncommitted | Allowed but not in postgres | Possible | Possible | Possible |
| Read committed | Not possible | Possible | Possible | Possible |
| Repeatable reads | Not Possible |  Not Possible | Allowed but not in postgres | Possible | 
| Serializable | Not Possible  | Not Possible | Not Possible  | Not Possible  |

> As you can see, Postgres “repeatable reads” is equivalent to the SQL92 “serializable”. In fact, the current “repeatable reads” behavior was the behavior of “serializable” in Postgres 9.1 and earlier. This level is also called Snapshot Isolation (SI) or even “serializable” in other databases.

### Dirty Reads
Not supported

### Non-Repeatable read / Read Committed
* The READ COMMITTED SELECT FOR UPDATE approach is essentially pessimistic locking: get the lock first, then do your work.
> Read committed is the default isolation level in postgres 
* If same item is read multiple times in the same transaction the value would differ
* Detrimental if a value is read in the application and then updated
* To counter it use SELECT FOR UPDATE and lock the transaction. SELECT…. FOR UPDATE locks the rows that it selects. 

```
BEGIN;
	SELECT job FROM job_queue FOR UPDATE SKIP LOCKED LIMIT 1
    -- do stuff for the job
    DELETE job from job_queue
COMMIT
```
> Here, you pick a job from the queue and lock it for the duration of the transaction. When you are done performing the job, you delete it from the queue and commit.

### Read Uncommitted 
Can be used as append only data. Enables faster write.

### Repeatable read / 
> basically snapshot isolation


### Serializable
* The SERIALIZABLE retry-loop approach is essentially optimistic locking with retry.
* It avoids the overhead of getting the row lock beforehand, at the
cost of having to redo the whole transaction if there is a conflict
* Uses predicate locking to prevent phantom reads
* Rollback on Conflict: When a conflict is detected – for example, if two transactions are attempting to modify data in a way that would not be possible in a serial (one-after-the-other) order – PostgreSQL will resolve this conflict by aborting and rolling back one of the transactions. The aborted transaction typically receives a serialization failure error, indicating that it should be retried.

## Vaccuming
* Old versions of rows can be reclaimed (by VACUUM) once they are no longer visible to any running transaction.
* Dead tuples due to MVCC in case of updates, delete operations (memory fragmentation as disk is allocated per page)

## Tradeoffs
### Benefits
* CHanges in DB with smaller lock contention

### Challenges
* Requires auto vaccuming
* UPgrading is more complex as compared with MySQL
* Write amplification due to indexing and MVCC and its replication using WAL

### Overcome
* Tuning autovacuum
* 

## References
* [Postgresql from infra](https://www.shayon.dev/post/2022/17/why-i-enjoy-postgresql-infrastructure-engineers-perspective/)
* [Hermitage](https://github.com/ept/hermitage)
* [Postgresql Isolation level](https://www.thenile.dev/blog/transaction-isolation-postgres)
* [Scaling Postgresql](https://onesignal.com/blog/lessons-learned-from-5-years-of-scaling-postgresql/)

