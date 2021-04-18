# Backup and Restore MySQL Docker database

To backup / make a dump of a MySQL or MariaDB database within a Docker container, just run:

```bash
docker exec DATABASECONTAINER mysqldump -u DATABASEUSER --password=DATABASEPASSWORD DATABASE > backup.sql
```

To restore a MySQL or MariaDB database from the `mysqldump`:

```bash
cat backup.sql | docker exec -i DATABASECONTAINER mysql -u DATABASEUSER --password=DATABASEPASSWORD DATABASE
```

So a real-world example might look like this:

```bash
docker exec wordpress-mysql mysqldump -u root --password=correcthorsebatterystaple wordpressdb > backup.sql
```

And restoring:

```bash
cat backup.sql | docker exec -i wordpress-mysql mysql -u root --password=correcthorsebatterystaple wordpressdb
```
