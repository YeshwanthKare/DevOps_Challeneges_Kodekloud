# Fixing a MariaDB Service That Fails to Start

## Issue

While attempting to start MariaDB, the service failed with the following error:

```bash
sudo systemctl start mariadb
```

Output:

```text
Job for mariadb.service failed because the control process exited with error code.
See "systemctl status mariadb.service" and "journalctl -xeu mariadb.service" for details.
```

Checking the service status showed:

```bash
sudo systemctl status mariadb
```

Output:

```text
mariadb.service - MariaDB 10.5 database server
Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled)
Active: failed (Result: exit-code)

Database MariaDB is not initialized, but the directory /var/lib/mysql ...
Make sure the /var/lib/mysql is empty before running mariadb-install-db
```

---

## Root Cause Analysis

MariaDB was configured to use:

```text
/var/lib/mysql
```

as its data directory.

Verify the configured data directory:

```bash
sudo grep -R "datadir" /etc/my.cnf /etc/my.cnf.d/* 2>/dev/null
```

Output:

```text
/etc/my.cnf.d/mariadb-server.cnf:datadir=/var/lib/mysql
```

However, the directory did not exist:

```bash
ls /var/lib/mysql
```

Output:

```text
No such file or directory
```

An unexpected directory was present instead:

```bash
sudo ls -lah /var/lib/mysqld
```

Output:

```text
total 16K
drwxr-xr-x 1 mysql mysql 4.0K Apr 29 13:16 .
drwxr-xr-x 1 root  root  4.0K Jul 10 10:14 ..
```

The directory existed but was empty and did not match the configured MariaDB data directory.

---

## Solution

### Step 1: Create the Correct Data Directory

```bash
sudo mkdir -p /var/lib/mysql
sudo chown mysql:mysql /var/lib/mysql
```

---

### Step 2: Initialize the MariaDB Database

```bash
sudo mariadb-install-db --user=mysql --datadir=/var/lib/mysql
```

If the command is unavailable, use:

```bash
sudo mysql_install_db --user=mysql --datadir=/var/lib/mysql
```

This creates the required system tables and initializes the database.

---

### Step 3: Start MariaDB

```bash
sudo systemctl start mariadb
```

---

### Step 4: Verify the Service Status

```bash
sudo systemctl status mariadb
```

Expected output:

```text
Active: active (running)
```

---

### Step 5: Verify Service Enablement

```bash
sudo systemctl is-enabled mariadb
```

Expected output:

```text
enabled
```

---

## Useful Troubleshooting Commands

### Check Service Status

```bash
sudo systemctl status mariadb -l
```

### View Detailed Logs

```bash
sudo journalctl -xeu mariadb.service --no-pager
```

### Verify MariaDB Configuration

```bash
sudo grep -R "datadir" /etc/my.cnf /etc/my.cnf.d/* 2>/dev/null
```

### Check Data Directory Contents

```bash
sudo ls -lah /var/lib/mysql
```

---

## Summary

The MariaDB service failed because its configured data directory (`/var/lib/mysql`) was missing. Creating the directory, assigning the correct ownership, initializing the database, and restarting the service resolved the issue successfully.
