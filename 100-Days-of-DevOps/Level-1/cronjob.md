# Create a Root Cron Job on All Application Servers

## Objective

Configure a cron job on all application servers that runs as the **root** user and writes the text:

```text
hello
```

to the file:

```text
/tmp/cron_text
```

every 5 minutes.

---

## Important Note

Do **not** perform this task on the `jump-host`.

The `jump-host` is a lightweight container and does not run services such as `crond` using `systemd`.

You must connect to each application server individually:

- `stapp01`
- `stapp02`
- `stapp03`

---

# App Server 1 (stapp01)

## Connect to the Server

```bash
ssh tony@stapp01
```

---

## Switch to Root User

```bash
sudo su -
```

---

## Install Cron Package

```bash
yum install cronie -y
```

---

## Enable and Start the Cron Service

```bash
systemctl enable crond
systemctl start crond
```

---

## Edit Root User Crontab

```bash
crontab -e
```

Add the following line:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

Save and exit the editor.

---

## Verify Root Crontab

```bash
crontab -l
```

Expected output:

```text
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Exit Back to Jump Host

```bash
exit
exit
```

---

# App Server 2 (stapp02)

## Connect to the Server

```bash
ssh steve@stapp02
```

---

## Switch to Root User

```bash
sudo su -
```

---

## Install Cron Package

```bash
yum install cronie -y
```

---

## Enable and Start the Cron Service

```bash
systemctl enable crond
systemctl start crond
```

---

## Edit Root User Crontab

```bash
crontab -e
```

Add:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Verify Root Crontab

```bash
crontab -l
```

Expected output:

```text
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Exit Back to Jump Host

```bash
exit
exit
```

---

# App Server 3 (stapp03)

## Connect to the Server

```bash
ssh banner@stapp03
```

---

## Switch to Root User

```bash
sudo su -
```

---

## Install Cron Package

```bash
yum install cronie -y
```

---

## Enable and Start the Cron Service

```bash
systemctl enable crond
systemctl start crond
```

---

## Edit Root User Crontab

```bash
crontab -e
```

Add:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Verify Root Crontab

```bash
crontab -l
```

Expected output:

```text
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Exit Back to Jump Host

```bash
exit
exit
```

---

# Alternative Non-Interactive Method

Instead of using the editor, you can write the cron job directly:

```bash
echo "*/5 * * * * echo hello > /tmp/cron_text" | crontab -
```

Verify:

```bash
crontab -l
```

---

# Verify Cron Service Status

On each server:

```bash
systemctl status crond
```

Expected output:

```text
Active: active (running)
```

---

# Validation

Verify that the root crontab contains:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

and that the cron service is running on:

- stapp01
- stapp02
- stapp03

---

# Summary

This task:

- Installs the `cronie` package.
- Enables and starts the `crond` service.
- Creates a cron job for the root user.
- Executes every 5 minutes.
- Writes `hello` to `/tmp/cron_text`.
- Applies the configuration to all application servers.
