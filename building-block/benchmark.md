# Benchmark
* **Simple insert**: inserts a new row into a table.
* **Single row update**: updates a single row in a table. In a concurrent environment this leads to a lot of waiting due to concurrent transactions waiting to be able to modify the same row. In case of “repeatable read” and “serializable” isolation levels, it needs to handle serialization failure (i.e. when a transaction cannot be committed because another transaction already committed their changes). Those cases are retried until they succeed.
* **Random row update**: similar to the single row update test but instead updates a random row. This doesn’t constantly put a lock on the same row.
* **Select for update single row**: runs a select for update query to put a lock on a single row.
* **Select for update skip locked**: runs a select for update skip locked query to select the first unlocked row.
* **Select for update with foreign key**: all transactions run select for update queries on table e which has a foreign key to table d. The outcome of this is that the table d gets locked, too.
* **Select single row**: runs a simple select to get 1 row.

## References
* [Benchmark postgres](https://lchsk.com/benchmarking-concurrent-operations-in-postgresql)
