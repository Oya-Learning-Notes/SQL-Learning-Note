# MySQL Learning Note - Basic

## Create User

We can use the following command:

```SQL
CREATE USER IF NOT EXIST 'username'@'localhost' IDENTIFIED BY 'password of this user';
```

Here the `localhost` part could be omitted which then would be default to `%`.

To check the user list, use:

```SQL
SELECT user, host FROM mysql.user;
```

## Grant Privileges

On common senario is that you want to give a user all control to a specified database. You can use following command to achieve this:

```SQL
GRANT ALL ON 'tablename'.* to 'username'@'localhost';
```

If you don't specified the host when you create account, then here you should not add the host part.

One things important that is you should run `FLUSH PRIVILEGES` to make this privileges changes comes into effects immediately without restarting MySQL. Check [this page](https://www.scaler.com/topics/mysql-flush-privileges/#:~:text=It%20is%20used%20to%20refresh,restart%20of%20the%20MySQL%20server.) for more info.

**When using command like `GRANT`**

Based on testing, it seems that the privileges changes performed by instructions like `GRANT`, `REVOKE` etc doesn't necessarily require a `FLUSH` command.

**When directly editing mysql.user**

However, if you directly modified the info inside `mysql.user`, then flush privileges becomes a compulsory.

![image.png](https://s2.loli.net/2024/02/29/etPSCZskwxf7jDp.png)

Here from the picture, we changed the username from `test_account` to `test_account_1` by directly editing field inside mysql.user WITHOUT calling `FLUSH PRIVILEGES`. And from the pic, we are failed to connect with both old and new username.

And after execute command `FLUSH PRIVILEGES`:

![image.png](https://s2.loli.net/2024/02/29/OZ19f6qIeQlSwzP.png)

We could successfully connect with the new username `test_account_1`.

## Delete User

In SQL if we want to delete a user, we need to use `DROP` command.

```SQL
DROP USER 'username' IF EXIST;
```

More info on [MySQL Official Docs](https://dev.mysql.com/doc/refman/8.0/en/drop-user.html).

If there is an opened session by the user you dropped, the drop action will be deferred to after the open session is closed. And this behaviour is by design based on the official docs.