# What Is Transaction

_Transaction_ (事物) is an operation sequence that user defined which have `ACID` characristics.

_ACID_ stands for:

- Atomic
- Consistency
- Isolation
- Durability

<details title='About ACID'>

**Atomic**: All operations in one transaction should be all done or all rollbacked.

**Consistency**: Since all transaction is all successful or all rollback, so if a database is consistent before transaction, it must be consistent after this transaction too.

**Isolation**: Different transactions will NOT affect each others.

**Durability**: Once transaction committed, it will permanently changed database.

</details>

Here is an example of using _Transaction_.

```sql
begin transaction
    balance_a = select ...;
    if (balance_a < 0) then
        rollback;
    else
        balance_2 = ...;
        commit;
```

# Transaction Undo

If the __execution do NOT reach `commit`, explicit `rollback` or error__, then database will __forcibly UNDO all previous changes__ of this transaction.

> Textbook P323