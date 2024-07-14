# CREATE TABLE

## AUTO INCREMENT

* `LAST_INSERT_ID()` is unique to each session and will not be affected by other concurrent insert operations
* Only work for an INSERT
* an UPDATE that does not create a new record, there is no new AUTO_INCREMENT value generated

```sql
SELECT LAST_INSERT_ID() AS id;
```

```sql
SELECT AUTO_INCREMENT 
FROM information_schema.tables
WHERE table_schema = 'poc'
AND table_name = 'fruit';
```

## Constraints

```sql
SELECT CONSTRAINT_NAME, CONSTRAINT_TYPE
FROM information_schema.TABLE_CONSTRAINTS
WHERE TABLE_SCHEMA = 'poc'
AND TABLE_NAME = 'fruit'
```

```sql
SET FOREIGN_KEY_CHECKS=0;
TRUNCATE TABLE fruit;
SET FOREIGN_KEY_CHECKS=1;
```

## Simple

```sql
CREATE TABLE fruit (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(20,2),
    tag JSON,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## One to many

* `tag` is parent table
* `company` is parent table
* `fruit` is child table
* `tag` and `fruit` are 1-to-many
* `company` and `fruit` are 1-to-many

```sql
CREATE TABLE ref_tag (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255)
);

CREATE TABLE ref_company (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255)
);

CREATE TABLE fruit (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    price DECIMAL(20,2),
    tag_id INT,
    company_id INT,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_fruit_ref_tag_id FOREIGN KEY (tag_id) REFERENCES ref_tag(id),
    CONSTRAINT fk_fruit_ref_company_id FOREIGN KEY (company_id) REFERENCES ref_company(id)
);
```

## Many to many

* `tag` is parent table
* `fruit_tags` is junction/child table
* `fruit` is child table
* `tag` and `fruit` are many-to-many

```sql
CREATE TABLE fruit (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL NOT NULL,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE ref_tag (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE fruit_tag (
    fruit_id INT UNSIGNED,
    tag_id INT UNSIGNED,
    PRIMARY KEY (fruit_id, tag_id),
    CONSTRAINT fk_fruit_tag_fruit_id  FOREIGN KEY (fruit_id) REFERENCES fruit(id),
    CONSTRAINT fk_fruit_tag_ref_tag_id FOREIGN KEY (tag_id) REFERENCES ref_tag(id)
);
```
