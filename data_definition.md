
# Create Table
To create table using SQL, we use `CREATE TABLE` statement.

```sql
CREATE TABLE table_name(
	col_name DATA_TYPE column_constraints,
);
```

The `column_constraints` part could be some restraints to this column, for example:

- UNIQUE
- PRIMARY KEY
- AUTO_INCREMENT
- NOT NULL
- etc.

For example if you want to create an `id` column as a primary key, then:

```sql
id BIGINT PRIMARY
```

If you want to create a foreign key, then:

```sql
major_id BIGINT {FOREIGN KEY | REFERENCE} other_table_name(col_name)
```

Check MySQL Manual for more info.

Once we created our new table, we can use `DESCRIBE` (or its synonym `DESC`) to check the shape of your newly created table.

```sql
DESCRIBE table_name;
```

# Alter Table

When we want to add, modify or delete (drop) our columns inside tables, use `ALTER TABLE` statement.

## Add Columns

If you want to add columns to the table, use `ADD COLUMN` statement.

```sql
ALTER TABLE table_name
ADD COLUMN col_name col_options;

ALTER TABLE table_name
ADD COLUMN (
    col_name col_options,
    ...
);
```

## Modify Column

If you want to make change to already exist columns:

```sql
ALTER TABLE table_name
{ MODIFY col_name … |
	CHANGE col_name new_col_name … |
	RENAME col_name TO new_col_name }
;
```

From above we know if you don’t need to modify the name of the column, `MODIFY` would be a better choice.

## Delete Columns

To delete a column in current table, use `DROP` statement.

```sql
ALTER TABLE table_name
DROP column_name;
```

## Add Constraints

If you want to add restriction/constraint to current table, you could use `ADD CONSTRAINT` statement. The following is an example to add a `FOREIGN KEY` constraint to an existing table.

```sql
DESC students;
DESC majors;
ALTER TABLE students
ADD CONSTRAINT FOREIGN KEY (major_id) REFERENCES majors(id) ON DELETE RESTRICT ON UPDATE CASCADE;
```

You can also add something like primary key, unique key or index.

```sql
ALTER TABLE students
ADD CONSTRAINT UNIQUE KEY (col_name),
ADD INDEX (major_id);
```

Check out [MySQL Manual](https://dev.mysql.com/doc/refman/8.3/en/alter-table.html#alter-table-redefine-column) for more info.
