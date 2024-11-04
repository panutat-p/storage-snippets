# Transaction

https://dev.mysql.com/doc/refman/8.4/en/commit.html

## Auto commit

* By default, MySQL runs with `autocommit` mode enabled
* Using `START TRANSACTION` will disable `autocommit` explicitly

```sql
START TRANSACTION;
SELECT name FROM fruit;
DO SLEEP(10);
COMMIT;
```

## Transaction Isolation Level

```sql
SELECT @@transaction_isolation;
```

```sql
SELECT @@global.transaction_isolation;
```

* `READ COMMITTED`: Lower isolation, allows non-repeatable reads and phantom reads, avoids gap locks.
* `REPEATABLE READ`: Higher isolation, prevents non-repeatable reads, allows phantom reads.
* `SERIALIZABLE`: Highest isolation, prevents dirty reads, non-repeatable reads, and phantom reads, but can significantly reduce concurrency.

### READ COMMITTED

* Isolation Level: Lower
* Each query within a transaction sees only committed data from other transactions.
* Does not see uncommitted changes (no dirty reads).
* Allows non-repeatable reads: if a row is read twice in the same transaction, it might see different data if another transaction modifies and commits the row in between.
* Allows phantom reads: new rows that match the query condition can appear if another transaction inserts them after the initial read.
* Suitable for applications where consistency is important but strict repeatability is not required, and where avoiding gap locks is beneficial.

### REPEATABLE READ

* Isolation Level: Higher
* Ensures that if a transaction reads a row, it will see the same data if it reads that row again, even if other transactions modify and commit the data (prevents non-repeatable reads).
* Prevents dirty reads.
* Prevents non-repeatable reads.
* Allows phantom reads: new rows that match the query condition can appear if another transaction inserts them after the initial read.
* Suitable for applications where consistency and repeatability are important, but where some level of concurrency is still desired.

### SERIALIZABLE

* Isolation Level: Highest
* Ensures complete isolation from other transactions.
* Prevents dirty reads, non-repeatable reads, and phantom reads.
* Transactions are executed in a way that they appear to be serialized, one after the other.
* Uses range locks to prevent other transactions from inserting, updating, or deleting rows that would affect the result set.
* Suitable for applications where the highest level of consistency is required, and where performance and concurrency can be sacrificed for strict isolation.
