# PostgreSQL Database Setup for Nautilus Application

## Overview

As a prerequisite for deploying a new Nautilus application, a PostgreSQL database and dedicated database user were configured on the Nautilus Database Server.

The goal was to create a database user with a secure password and provision a database owned by that user, ensuring full access privileges.

---

## Requirements

### Database User

| Parameter | Value           |
| --------- | --------------- |
| Username  | `kodekloud_gem` |
| Password  | `B4zNgHA7Ya`    |

### Database

| Parameter     | Value            |
| ------------- | ---------------- |
| Database Name | `kodekloud_db10` |
| Owner         | `kodekloud_gem`  |
| Encoding      | `UTF8`           |

---

## Verify PostgreSQL Service

Enable and verify the PostgreSQL service:

```bash
sudo systemctl enable postgresql
sudo systemctl status postgresql
```

Expected status:

```text
Active: active (running)
```

---

## Connect to PostgreSQL

Switch to the PostgreSQL administrative shell:

```bash
sudo -u postgres psql
```

Expected output:

```text
psql (13.x)
Type "help" for help.
```

---

## Create Database User

Execute the following SQL statement:

```sql
CREATE ROLE kodekloud_gem LOGIN PASSWORD 'B4zNgHA7Ya';
```

Expected output:

```text
CREATE ROLE
```

---

## Create Database

Create the database and assign ownership to the new user:

```sql
CREATE DATABASE kodekloud_db10
ENCODING 'UTF8'
OWNER kodekloud_gem;
```

Expected output:

```text
CREATE DATABASE
```

---

## Verify Configuration

List available roles:

```sql
\du
```

Verify database ownership:

```sql
\l
```

Expected result:

```text
Database: kodekloud_db10
Owner:    kodekloud_gem
Encoding: UTF8
```

---

## Exit PostgreSQL

```sql
\q
```

---

## Result

Successfully completed the PostgreSQL configuration:

- Created database user: `kodekloud_gem`
- Configured password: `B4zNgHA7Ya`
- Created database: `kodekloud_db10`
- Assigned ownership and full privileges to `kodekloud_gem`
- Verified PostgreSQL service is running and enabled

The PostgreSQL environment is now ready for application deployment.
