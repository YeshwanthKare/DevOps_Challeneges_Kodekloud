# Disable Direct Root SSH Login on All App Servers

## Objective

As part of security hardening, disable direct root login through SSH on all application servers in the Stratos Datacenter.

## Step 1: Connect to All App Servers

Open separate terminal sessions and connect to each application server.

### App Server 1

```bash
ssh tony@stapp01
```

### App Server 2

```bash
ssh steve@stapp02
```

### App Server 3

```bash
ssh banner@stapp03
```

---

## Step 2: Edit the SSH Configuration

Open the SSH daemon configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

Search for the `PermitRootLogin` directive:

```text
/PermitRootLogin
```

You may find one of the following:

```text
#PermitRootLogin yes
#PermitRootLogin prohibit-password
PermitRootLogin yes
```

Modify the configuration so it reads:

```text
PermitRootLogin no
```

### Using Vi Editor

1. Press `i` to enter **INSERT** mode.
2. Remove the `#` if the line is commented.
3. Change the value to `no`.
4. Press `Esc`.
5. Type `:wq` and press **Enter** to save and exit.

---

## Step 3: Validate the SSH Configuration

Check for syntax errors before restarting the service:

```bash
sudo sshd -t
```

If no output is displayed, the configuration is valid.

---

## Step 4: Restart the SSH Service

Apply the configuration changes:

```bash
sudo systemctl restart sshd
```

---

## Step 5: Verify SSH Service Status

Ensure the SSH service is running correctly:

```bash
sudo systemctl status sshd
```

Expected output:

```text
Active: active (running)
```

---

## Step 6: Verify the Configuration

Confirm that root login has been disabled:

```bash
sudo grep "^PermitRootLogin" /etc/ssh/sshd_config
```

Expected output:

```text
PermitRootLogin no
```

---

## Step 7: Test Root Login Restriction

> Keep your existing SSH session open while performing this test.

From a new terminal, attempt to connect as root:

```bash
ssh root@stapp01
```

Expected result:

```text
Permission denied
```

or

```text
Access denied
```

Repeat the verification on all application servers if required.

---

## Verification Summary

- Connected to all app servers.
- Updated `/etc/ssh/sshd_config`.
- Set `PermitRootLogin no`.
- Validated configuration using `sshd -t`.
- Restarted the SSH service.
- Verified the configuration change.
- Confirmed that direct root SSH login is disabled.

## Final Configuration

```text
PermitRootLogin no
```

This ensures that users must authenticate using a non-root account and elevate privileges using `sudo` when administrative access is required.
