# Pessimistic Locking

```sql
START TRANSACTION;
SELECT id, name, price FROM fruit WHERE name = 'apple' FOR UPDATE;
DO SLEEP(10);
COMMIT;
```

```sql
UPDATE fruit SET price = 100 WHERE name = 'banana';
```

## ROW locking granularity

* InnoDB's Next-key locking
    * It is a combination of row locks on the index records and gap locks on the spaces between index records
    * This mechanism is crucial for preventing phantom reads, a scenario where new records matching the query criteria are inserted into the dataset by another transaction after the initial read

```sql
SELECT * FROM fruit WHERE id = 1 FOR UPDATE;
```

* InnoDB, REPEATABLE READ isolation level
* Given that `id` is a primary key
* InnoDB uses row-level locking for this query
* This will lock only the row with `id` = 1 because `id` has a unique index
* This is because MySQL can precisely identify and lock the specific row due to the unique index on the primary key.

```sql
SELECT * FROM fruit WHERE name = 'apple' FOR UPDATE;
```

* InnoDB, REPEATABLE READ isolation level
* Given that `name` does not have any index
* Row Lock: the rows that match the condition are locked
* Gap Lock: it may also lock the gaps between rows to prevent the insertion of new rows that match the query conditions
* This broad application of locks, including gap locks, can lead to situations where concurrent `SELECT ... FOR UPDATE` queries on the same table, even with different WHERE conditions, block each other. This is because the gap locks acquired by the first query can overlap with the row or gap locks needed by the second query.


```sql
START TRANSACTION;
SELECT * FROM fruit WHERE name = 'apple' FOR UPDATE;
UPDATE fruit SET name = 'APPLE' WHERE name = 'apple';
COMMIT;
```

```sql
START TRANSACTION;
SELECT * FROM fruit WHERE name = 'apple' FOR UPDATE;
UPDATE fruit SET status = 'completed' WHERE name = 'apple';
COMMIT;
```

> First Thread (T1):
> Executes `SELECT * FROM fruit WHERE name = 'apple' FOR UPDATE;`
> This locks the rows with name = 'apple' for update, preventing other transactions from reading (with FOR UPDATE or LOCK IN SHARE MODE), updating, or deleting these rows.
> Then executes `UPDATE fruit SET name='APPLE' WHERE name = 'apple';`
> This updates the locked rows to have name = 'APPLE'. Since T1 has the lock, this operation proceeds without delay.
>
> Second Thread (T2):
> Executes `SELECT * FROM fruit WHERE name = 'apple' FOR UPDATE;`
> Since T1 has already locked the rows with name = 'apple' and updated their name to 'APPLE', T2 will wait for T1 to release its locks if T1 hasn't committed yet. Once T1 commits, T2's `SELECT ... FOR UPDATE` will execute.
> However, because T1 changed the name from 'apple' to 'APPLE', T2's SELECT condition (WHERE name = 'apple') will not find any rows (assuming all 'apple' entries were updated by T1 and there are no new 'apple' entries added before T2's SELECT executes).

### row-level locks

```
[mysqld]
innodb_locking_mode = 1
```

## TRANSACTION ISOLATION LEVEL

* READ COMMITTED
* REPEATABLE READ
* SERIALIZABLE

```sql
SELECT @@transaction_isolation;
```

### READ COMMITTED (default in many databases)

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

* Lock only the specific rows and avoid gap locking
* But also uses next-key locks which means it locks the index range scanned
* Does not prevent all forms of conflicts
* Does not prevent non-repeatable reads or phantom reads

> Non-repeatable reads occur when a transaction reads the same row twice and gets different results each time
> because another transaction has updated the row in the meantime.

### REPEATABLE READ (default in MySQL)

```sql
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

* In certain circumstances, the `SELECT ... FOR UPDATE statement` can lock more than just the rows it retrieves
* MySQL acquires exclusive next-key locks for the rows it retrieves
* The next-key lock is a combination of a record lock on the index record and a gap lock on the gap before the index record
* This means that not only the selected row is locked, but also the gap before this row in the index order

> In this case, if the 'apple' row comes before the 'banana' row in the index order,
> the `SELECT ... FOR UPDATE` statement on 'apple' will also lock the gap before 'banana',
> preventing any new rows from being inserted in this gap or any modification to the 'banana' row until the lock is released.

### SERIALIZABLE

```sql
SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

* The highest isolation level
* Guaranteed to be serial in that it appears to the user as though transactions were executed one after another
* Prevents both non-repeatable reads and phantom reads
