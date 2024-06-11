There are two backup methods. _Static Backup_ and _Dynamic Backup_.

# Static Backup

_Static Backup_ requires __there is no any transaction running in db system__. This could __promise that the backuped copy must be consistency__.

# Dynamic Backup

_Dynamic Backup_ __running while there are some active transaction__. This means __some of the data in backup may outdated and not consistent__.

## Log Files

To solve the inconsistency of _Dynamic Backup_, we use _Log Files_ to __record all newly executed transaction info while we do _Dynamic Backup___.

![IMG_2161](https://github.com/Oya-Learning-Notes/SQL-Learning-Note/assets/61616918/e9a163f0-7373-496e-bfcf-a83ee63c4f55)

As the image shows above. After backup finished, we use log file to amend the original backup to make it become consistent.

## Transaction Error Recover

Reversely scan log file, __undo all operation until the start of this transaction__.

## System Error Recover

Perform `REDO` to finished transaction. Perform `UNDO` to non-finished transaction.

Finished transaction means that both have `start transaction` and `commit` mark in the log file.

# Log File With Check Point

To avoid scan whole log file everytime, we introduce _Checkpoint_.

## Mechanism

- Create checkpoint from time to time.
- Use _Restart_ file to store checkpoint position.
- Scan from lastest checkpoint position to error position.

## When Error

- Find neariest checkpoint.
- Put all active task into UNDO.
- Scan start from checkpoint to error, mutating UNDO and REDO list using following rules:
    - Find `start transaction`: Put to UNDO.
    - Find `commit`: Put to REDO.
- When scan finished, UNDO and REDO is the list we want.

> Textbook P328