# Issues In Concurrency

- __Lost Update.__ Two _Transactions_ read a same data `X`, and update `X` respectively. The data of former update will be covered by latter one, thus lost.
- __Dirty Read.__ Read a pre-write data that has been rollbacked later.
- __Outdated Read. (不可重复读)__ Read a value that has been updated later.
- __Phantom.__ Read rows with condition, which's result will change because of later insertion or deletion.

> Textbook P335.

# Using Locks

## Locks Type

- `X` Exclusive lock.
- `S` Shared Lock.

## Locks Levels

- 读未提交
- 读已提交
- 可重复读
- 可串行化

|                    | Update Lost | Dirty Read | Read Outdated | Phantom |
|--------------------|-------------|------------|---------------|---------|
| Read Non-Submitted | ok          |            |               |         |
| Read Submitted     | ok          | ok         |               |         |
| Duplicate Read     | ok          | ok         | ok            |         |
| Streamable         | ok          | ok         | ok            | ok      |

> `ok` means solved in this level.

## Read Non-Submitted

This also called _Level-1 Lock Protocal (一级封锁协议)_.

- `XLOCK` when write, until _Transaction End_.

> Notice: _Transaction End_ includes `submit` and `rollback`.

## Read Submitted

_Level-2 Lock Protocal_.

- All in _Level-1 Lock Protocal_.
- `SLOCK` when read, until _Read End_.

## Duplicate Read

_Level-3 Lock Protocal_.

- All in _Level-1 Lock Protocal_.
- `SLOCK` when read, until _Transaction End_.

> Notice: Only difference of this level and level-2 is when to release `S` lock.

# 冲突可串行操作

针对于并发调度。如果并发调度结果，和按照某种顺序串行执行两个事务，结果相同，则称这个并发调度策略为：可串行化调度(正确调度)

## 办法之一

办法之一就是，不改变冲突操作的前提下，调换其他操作的位置，得到的调度序列一定可串行化（充分条件，意味着不是所有正确序列都满足这个）.

> 冲突操作：不同Transaction对于同一个对象的读写或者写写。

# 两阶段锁（Two-Phase Lock, 2PL）

如果所有Transaction都遵循两阶段锁，则无论如何调度，都可串行化。（同样是 充分条件）。

两阶段锁，指的是，一段加锁，一段解锁。不能一会加锁一会解锁。