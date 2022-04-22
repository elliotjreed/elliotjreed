# Install MariaBD (MySQL) on Ubuntu 20.04 or 22.04 and allow remote access

The following assumes you are logged in as the `root` user on your system.

If you are logged in as another user then first run the following to get into an interactive `sudo` shell:

```bash
sudo -i
```

First we should ensure the system is up-to-date by running:

```bash
apt-get update && apt-get full-upgrade -y
```

To install MariaDB run:

```bash
apt-get install mariadb-server -y
```

The `mariadb` service should automatically start in the background once the installation has completed, but you can check by running:

```bash
systemctl status mariadb
```

If all is well you should see the following at the beginning of the output:

```
● mariadb.service - MariaDB 10.3.34 database server
     Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-04-22 15:17:04 UTC; 17s ago
       Docs: man:mysqld(8)
             https://mariadb.com/kb/en/library/systemd/
   Main PID: 1664 (mysqld)
     Status: "Taking your SQL requests now..."
      Tasks: 31 (limit: 1131)
     Memory: 63.7M
     CGroup: /system.slice/mariadb.service
             └─1664 /usr/sbin/mysqld
```

Note: "q" on the keyboard will exit that output.

Next to run the secure installation script, run:

```bash
mysql_secure_installation
```

```bash
Enter current password for root (enter for none): 
```bash

It will ask you for the current root password, this will be empty by default so just hit "enter".

```bash
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n]
```

Now enter "y" and then enter a strong password. Then enter it again to confirm when it asks.

```bash
Remove anonymous users? [Y/n]
```

Enter "y" again to remove the anonymous users.

```bash
Disallow root login remotely? [Y/n] 
```

Enter "y" here too (we'll set up a separate user for logging in remotely shortly).

```bash
Remove test database and access to it? [Y/n] 
```

Again enter "y" here, you don't need the test database.

```bash
Reload privilege tables now? [Y/n] 
```

And again enter "y" here.

Now you should be able to access MariaDB by running the following and entering the password from earlier when prompted:

```bash
mysql -uroot -p
```

Inside the `mysql` client, create a new user by running the following with a username (instead of "elliot", unless that's your name too) and password:

```sql
CREATE USER 'elliot'@'%' IDENTIFIED BY 'averystrongpassword';
```

Now you can grant some permissions to the new user (replace the "elliot" username with the one you used in the previous step):

```sql
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, SELECT, REFERENCES, RELOAD on *.* TO 'elliot'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

Now you should be able to access MariaDB using your new username and password:

```bash
mysql -uelliot -p
```



If you want to be able to access the database remotely you will need to comment-out the `bind-address` line in `/etc/mysql/mariadb.conf.d/50-server.cnf`:

```bash
nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Find the following line:

```
# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 127.0.0.1
```

and replace the `bind-address` line with:

```
#bind-address            = 127.0.0.1
```

Once you have saved the file, restart the `mariadb` service by running:

```bash
systemctl restart mariadb
```

Now you should be able to access your database remotely, for example:

```bash
mysql -u elliot -h 123.45.67.89 -p
```

Where the `-h` IP address is the IP address of the server you've created.

One more thing to note if you are connecting via GUI client or programming script, the port number is _3306_.

Once you are in the MariaDB server, you'll probably want to create a database, so log in again:

```bash
mysql -u elliot -h 123.45.67.89 -p
```

and run:

```sql
CREATE DATABASE mydatabase;
```
