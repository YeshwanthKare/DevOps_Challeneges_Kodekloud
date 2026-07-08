# Configure Passwordless SSH Access from Jump Host to App Servers

## Objective

Configure passwordless SSH authentication from the `thor` user on the **jump host** to all application servers in the Stratos Datacenter.

The SSH access must use the respective application server users:

| Server  | User   |
| ------- | ------ |
| stapp01 | tony   |
| stapp02 | steve  |
| stapp03 | banner |

After completion, the `thor` user should be able to SSH into all servers without entering a password.

---

# Step 1: Login to the Jump Host

Login as the `thor` user:

```bash
ssh thor@jump-host
```

Confirm the current user:

```bash
whoami
```

Expected:

```text
thor
```

---

# Step 2: Check Existing SSH Keys

Check whether an SSH key already exists:

```bash
ls -la ~/.ssh
```

Look for:

```text
id_rsa
id_rsa.pub
```

or:

```text
id_ed25519
id_ed25519.pub
```

---

# Step 3: Generate an SSH Key Pair

If no SSH key exists, generate one:

```bash
ssh-keygen
```

Accept the default options:

```text
Enter
Enter
Enter
```

This creates:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

Verify:

```bash
ls ~/.ssh
```

---

# Step 4: Copy SSH Key to App Servers

Use `ssh-copy-id` to install the public key on each server.

---

## Configure App Server 1

Copy the key to user `tony`:

```bash
ssh-copy-id tony@stapp01
```

Enter the password when prompted.

Expected:

```text
Number of key(s) added: 1
```

---

## Configure App Server 2

Copy the key to user `steve`:

```bash
ssh-copy-id steve@stapp02
```

Enter the password when prompted.

---

## Configure App Server 3

Copy the key to user `banner`:

```bash
ssh-copy-id banner@stapp03
```

Enter the password when prompted.

---

# Step 5: Test Passwordless SSH Access

Test App Server 1:

```bash
ssh tony@stapp01
```

Expected:

- Login succeeds
- No password prompt appears

---

Test App Server 2:

```bash
ssh steve@stapp02
```

---

Test App Server 3:

```bash
ssh banner@stapp03
```

---

# Step 6: Verify Authorized Keys

After logging into each server, verify the public key exists:

```bash
cat ~/.ssh/authorized_keys
```

The output should contain the public key from:

```bash
/home/thor/.ssh/id_rsa.pub
```

or:

```bash
/home/thor/.ssh/id_ed25519.pub
```

---

# Troubleshooting

## Check SSH Connection Manually

If `ssh-copy-id` fails, first verify normal SSH login:

```bash
ssh tony@stapp01
```

If login fails with:

```text
Permission denied
```

verify:

- Username is correct
- Password is correct
- SSH service is running
- The account exists on the server

---

## Remove Old SSH Keys (Optional Reset)

If starting from scratch:

Remove keys on the jump host:

```bash
rm -f ~/.ssh/id_rsa ~/.ssh/id_rsa.pub
```

Generate a new key:

```bash
ssh-keygen
```

Copy it again:

```bash
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```

---

# Final Validation

Run these commands from the jump host:

```bash
ssh tony@stapp01 hostname
```

```bash
ssh steve@stapp02 hostname
```

```bash
ssh banner@stapp03 hostname
```

All commands should execute successfully without requesting passwords.

---

# Summary

This task completes the following:

- Created an SSH key pair for the `thor` user.
- Installed the public key on all application servers.
- Enabled passwordless SSH authentication.
- Verified access using the required sudo users:

```text
thor → tony@stapp01
thor → steve@stapp02
thor → banner@stapp03
```

Passwordless SSH access is now configured successfully.
