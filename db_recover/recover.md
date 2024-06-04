There are two backup methods. _Static Backup_ and _Dynamic Backup_.

# Static Backup

_Static Backup_ requires __there is no any transaction running in db system__. This could __promise that the backuped copy must be consistency__.

# Dynamic Backup

_Dynamic Backup_ __running while there are some active transaction__. This means __some of the data in backup may outdated and not consistent__.

## Log Files

To solve the inconsistency of _Dynamic Backup_, we use _Log Files_ to __record all newly executed transaction info while we do _Dynamic Backup___.

![IMG_2161](https://github.com/Oya-Learning-Notes/SQL-Learning-Note/assets/61616918/e9a163f0-7373-496e-bfcf-a83ee63c4f55)

As the image shows above. After backup finished, we use log file to amend the original backup to make it become consistent.