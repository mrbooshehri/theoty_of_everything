# Permission denied

## Problem
```
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```

## Solution
Basically it means that: db_users using it, will be "authenticated" by the
system user credentials. You can see if your root user is set up like this by
doing the following:

```bash
sudo mysql -u root # I had to use "sudo" since it was a new installation

mysql> USE mysql;
mysql> SELECT User, Host, plugin FROM mysql.user;

+------------------+-----------------------+
| User             | plugin                |
+------------------+-----------------------+
| root             | auth_socket           |
| mysql.sys        | mysql_native_password |
| debian-sys-maint | mysql_native_password |
+------------------+-----------------------+
```

There are two ways to solve this:

1. You can set the root user to use the mysql_native_password plugin
1. You can create a new db_user with you system_user (recommended)

**Option 1:**
```bash
sudo mysql -u root # I had to use "sudo" since it was a new installation

mysql> USE mysql;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;

sudo service mysql restart
```
**Option 2:** (replace YOUR_SYSTEM_USER with the username you have)

```bash
sudo mysql -u root # I had to use "sudo" since it was a new installation

mysql> USE mysql;
mysql> CREATE USER 'YOUR_SYSTEM_USER'@'localhost' IDENTIFIED BY 'YOUR_PASSWD';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'YOUR_SYSTEM_USER'@'localhost';
mysql> UPDATE user SET plugin='auth_socket' WHERE User='YOUR_SYSTEM_USER';
mysql> FLUSH PRIVILEGES;
mysql> exit;

sudo service mysql restart
```

Tags:
```
#mysql #permission_denied #permission #denied
```

Related:
```
* https://stackoverflow.com/questions/39281594/error-1698-28000-access-denied-for-user-rootlocalhost
```

