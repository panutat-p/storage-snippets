# InnoDB Storage Engine

```sql
SHOW VARIABLES LIKE 'default_storage_engine';
SHOW VARIABLES LIKE 'character_set_database';
SHOW VARIABLES LIKE 'collation_database';
```

## Character size

>  MySQL
> ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin
> 
> Max row size = 16KB

```sql
SELECT (CHAR_LENGTH(id) + CHAR_LENGTH(name)) as row_size
FROM person;
```

## Storage

* InnoDB storage engine
* utf8mb4 character set

| Type       | Size |
| ---------- | ---- |
| INT        | 4B   |
| BINARY(16) | 16B  |
| CHAR(27)   | 108B |
| CHAR(36)   | 144B |

## Index

* MySQL automatically creates an index on the child table, which is the table that contains the foreign key
* No need to explicity create an index when adding a foreign key

## Secondary Index

* Secondary indexes will also consume more space
* This is because secondary indexes use the primary key as a pointer to the actual row, meaning they need to be stored with the index
* This can lead to a significant increase in storage requirements for your database depending on how many indexes are created on tables using UUIDs as the primary key
