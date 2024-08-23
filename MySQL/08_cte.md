# CTE

https://dev.mysql.com/doc/refman/8.4/en/with.html

```sql
WITH
  cte1 AS (SELECT a, b FROM table1),
  cte2 AS (SELECT c, d FROM table2)
SELECT b, d FROM cte1 JOIN cte2
WHERE cte1.a = cte2.c;
```

```sql
WITH FruitFactoryUserCTE AS (
    SELECT 
        f.id AS fruit_id, 
        f.name AS fruit_name, 
        fa.id AS factory_id, 
        fa.name AS factory_name, 
        u.id AS user_id, 
        u.name AS user_name
    FROM 
        fruit f
    JOIN 
        factory fa ON f.factory_id = fa.id
    JOIN 
        user u ON fa.user_id = u.id
)
SELECT *
FROM FruitFactoryUserCTE;
```
