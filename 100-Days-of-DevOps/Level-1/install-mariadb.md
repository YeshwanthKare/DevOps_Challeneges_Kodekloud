# README - Install and Configure MariaDB on CentOS

## Overview

This guide covers:

- Installing MariaDB on CentOS
- Starting and enabling the MariaDB service
- Securing the installation
- Creating a database
- Creating a database user with a password
- Granting full permissions on the database

---

## Step 1: Install MariaDB

Update the package repository and install MariaDB:

```bash
sudo yum install -y mariadb-server
```

Verify installation:

```bash
mysql --version
```

---

## Step 2: Start and Enable MariaDB

Start the service:

```bash
sudo systemctl start mariadb
```

Enable MariaDB to start automatically at boot:

```bash
sudo systemctl enable mariadb
```

Check service status:

```bash
sudo systemctl status mariadb
```

---

## Step 3: Secure MariaDB Installation

Run the security script:

```bash
sudo mysql_secure_installation
```

Recommended options:

- Set root password: **Yes**
- Remove anonymous users: **Yes**
- Disallow remote root login: **Yes**
- Remove test database: **Yes**
- Reload privilege tables: **Yes**

---

## Step 4: Log in to MariaDB

```bash
mysql -u root -p
```

Enter the root password when prompted.

---

## Step 5: Create a Database

Example database:

```sql
CREATE DATABASE companydb;
```

Verify:

```sql
SHOW DATABASES;
```

---

## Step 6: Create a Database User

Create a user with a password:

```sql
CREATE USER 'companyuser'@'localhost' IDENTIFIED BY 'StrongPassword123';
```

Verify:

```sql
SELECT User, Host FROM mysql.user;
```

---

## Step 7: Grant Full Permissions

Grant all privileges on the database:

```sql
GRANT ALL PRIVILEGES ON companydb.* TO 'companyuser'@'localhost';
```

Apply the changes:

```sql
FLUSH PRIVILEGES;
```

---

## Step 8: Verify Permissions

Check granted privileges:

```sql
SHOW GRANTS FOR 'companyuser'@'localhost';
```

Expected output:

```sql
GRANT ALL PRIVILEGES ON `companydb`.* TO 'companyuser'@'localhost';
```

---

## Step 9: Test User Access

Exit MariaDB:

```sql
EXIT;
```

Connect using the new user:

```bash
mysql -u companyuser -p companydb
```

If the login succeeds, the user has access to the database.

---

## Useful Commands

Check MariaDB status:

```bash
sudo systemctl status mariadb
```

Restart MariaDB:

```bash
sudo systemctl restart mariadb
```

Stop MariaDB:

```bash
sudo systemctl stop mariadb
```

List databases:

```sql
SHOW DATABASES;
```

List users:

```sql
SELECT User, Host FROM mysql.user;
```

---

## Example Configuration

```sql
CREATE DATABASE companydb;

CREATE USER 'companyuser'@'localhost'
IDENTIFIED BY 'StrongPassword123';

GRANT ALL PRIVILEGES ON companydb.* TO 'companyuser'@'localhost';

FLUSH PRIVILEGES;
```

This configuration creates:

- Database: `companydb`
- User: `companyuser`
- Password: `StrongPassword123`
- Full access to `companydb`
