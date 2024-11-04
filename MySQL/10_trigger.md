# Event Sourcing

* `log_xxx` table provides a complete history of events
* Referential integrity is not needed
* Data consistency is not needed

## Schemaless log

```sql
CREATE TABLE fruit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(20,2),
    tags JSON,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

```sql
CREATE TABLE log_fruit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    source_id INT NOT NULL,
    activity VARCHAR(255) NOT NULL,
    data JSON NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP NOT NULL
);
```

## MySQL triggers

```sql
CREATE TABLE fruit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(20,2),
    tags JSON,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

```sql
CREATE TABLE log_fruit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    source_id INT,
    action VARCHAR(255),
    data JSON,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

```sql
CREATE TRIGGER fruit_after_insert AFTER INSERT ON fruit
FOR EACH ROW
BEGIN
    INSERT INTO log_fruit(source_id, action, data)
    VALUES (NEW.id, 'INSERT', JSON_OBJECT('id', NEW.id, 'name', NEW.name, 'price', NEW.price, 'tags', NEW.tags, 'updated_at', NEW.updated_at, 'created_at', NEW.created_at));
END;

CREATE TRIGGER fruit_after_update AFTER UPDATE ON fruit
FOR EACH ROW
BEGIN
    INSERT INTO log_fruit(source_id, action, data)
    VALUES (NEW.id, 'UPDATE', JSON_OBJECT('id', NEW.id, 'name', NEW.name, 'price', NEW.price, 'tags', NEW.tags, 'updated_at', NEW.updated_at, 'created_at', NEW.created_at));
END;

CREATE TRIGGER fruit_after_delete AFTER DELETE ON fruit
FOR EACH ROW
BEGIN
    INSERT INTO log_fruit(source_id, action, data)
    VALUES (OLD.id, 'DELETE', JSON_OBJECT());
END;
```
