# Copy MySQL / MariaDB database table date to a new table

There are two easy methods for backing up / copying a database table to a new table in MySQL or MariaDB.

The first method will copy just the table data and *not* any indexes or triggers, whilst the second method will copy over the indexes and triggers as well as the data.

## Copy to new table without indexes and triggers

Note: this will *not* copy the `AUTO_INCREMENT` value, will *not* carry over foreign key constraints, and will *not* copy indexes and triggers.

```sql
CREATE TABLE my_table_copy AS SELECT * FROM my_table;
```

## Copy to new table with indexes and triggers

Note: this will *not* copy the `AUTO_INCREMENT` value, and will *not* carry over foreign key constraints. Indexes and triggers *will* be copied.

```sql
CREATE TABLE my_table_copy LIKE my_table; 
INSERT INTO my_table_copy SELECT * FROM my_table;
```
