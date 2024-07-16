# ALTER TABLE

* `InnoDB` storage engine
* `ALTER` typically uses an SHARED lock
* Which allows concurrent reads but not writes
* Specify the `ALGORITHM=INPLACE` option, MySQL will try to avoid copying the table and will use a less restrictive lock

```sql
ALTER TABLE fruit
DROP PRIMARY KEY;
```

```sql
ALTER TABLE fruit
ADD COLUMN id INT UNSIGNED AUTO_INCREMENT FIRST,
ADD PRIMARY KEY (id);
```

```sql
ALTER TABLE fruit 
ADD COLUMN updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP, 
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
```

```sql
ALTER TABLE fruit ADD COLUMN factory VARCHAR(255) AFTER `name`;
```

```sql
ALTER TABLE fruit ADD COLUMN factory VARCHAR(255) NOT NULL DEFAULT '';
```

```sql
ALTER TABLE fruit ADD COLUMN factory VARCHAR(255),
ALGORITHM=INPLACE, LOCK=NONE;
```

```sql
ALTER TABLE fruit 
CONVERT TO CHARACTER SET utf8mb4 
COLLATE utf8mb4_0900_ai_ci;
```

```sql
ALTER TABLE fruit 
CONVERT TO CHARACTER SET utf8mb4 
COLLATE utf8mb4_bin;
```
