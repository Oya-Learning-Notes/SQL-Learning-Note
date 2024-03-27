# Foreign Key Briefs

Foreign key is a restraint type in MySQL which used to estabilish connections between different columns between various databases.

For example you have a `students` database and `majors` database, then you may want to add the `selected_major` column inside `students` database which could have some connection to `majors` database.

For example, you want to make sure:

- `selected_major` used to store the `id` of the major inside `majors`.
- Value in column `selected_major` always legal, means the `id` must exist inside `majors` database.
- Update the `id` simultaneously inside `students` when you do some update inside `majors`.
- etc.

If you want the features above, then a _Foreign Key_ will be a great choice.

## Before Create Foreign Key

Here I'd like to take _MySQL_ for example.

When creating foreign key in MySQL, we need to first make sure:

- The corresponding columns must have _similar types_ .
- The corresponding columns in target database and original database are all _indexed_ .
- If there has any value in the column of target database, then it must not break the foreign key constraint rules. (For example there could not have a `id` in `selected_major` which didn't exist in `majors`)

Here are just basic rules, for more info please check out [MySQL Manual - Foreign Key](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html)


## Create Foreign Key

Below is an example adding foreign key in exist table.

```sql
ALTER TABLE students
    ADD COLUMN major_id BIGINT,
    ADD INDEX (major_id),
    ADD CONSTRAINT FOREIGN KEY (major_id) REFERENCES majors (id)
        ON DELETE RESTRICT
        ON UPDATE CASCADE;
```

Notice that we must add index to this newly created column `major_id` in order to add a _foreign key_ constraint to it.

## Remove Foreign Key Constraint

To remove a _foreign key_ constraint, we must first get its symbol. To check all constraints in a table, we can use `SHOW CREATE TABLE table_name` statement.

Now suppose the symbol of the restraint we want to remove is `students_ibfk_1`, then we can use:

```sql
ALTER TABLE students DROP FOREIGN KEY (students_ibfk_1);
```