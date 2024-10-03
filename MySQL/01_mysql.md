# MySQL

## Database

```sql
SHOW DATABASES;
```

```sql
CREATE DATABASE `poc` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
```

```sql
USE poc;
```

```sql
SHOW TABLES IN poc;
```

```sql
SHOW TABLES IN poc;
```

```sql
SHOW VARIABLES LIKE 'foreign_key_checks';
```

## Tasks

```sql
SHOW FULL PROCESSLIST;
```

```sql
KILL process_id;
```

https://emmer.dev/blog/finding-long-running-queries-in-mysql

```sql
SELECT
  *,
  concat('KILL ', processlist_id, ';') AS kill_command,
  concat('CALL mysql.rds_kill(', processlist_id, ');') AS rds_kill_command
FROM
  performance_schema.threads
WHERE
  type = 'FOREGROUND'
  AND processlist_command != 'Sleep'
  and processlist_command != 'Daemon'
  AND processlist_id != connection_id()
ORDER BY
  processlist_time DESC;
```
