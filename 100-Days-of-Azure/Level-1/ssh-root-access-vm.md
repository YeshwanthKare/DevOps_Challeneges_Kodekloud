# Azure VM SSH Key Configuration for Root Access

## Overview

The Nautilus DevOps team required secure, passwordless SSH access to an Azure virtual machine by adding the root user's public SSH key from the Azure client host to the target VM.

The objective was to configure the **`datacenter-vm`** virtual machine so that the root user on the Azure client host could connect directly using SSH key authentication.

---

## Requirements

### VM Information

| Parameter         | Value                 |
| ----------------- | --------------------- |
| VM Name           | datacenter-vm         |
| Region            | centralus             |
| Default User      | azureuser             |
| Public Key Source | /root/.ssh/id_rsa.pub |
| Target User       | root                  |

### Objectives

- Retrieve the root user's public SSH key from the Azure client host.
- Add the key to the root user's `authorized_keys` file on `datacenter-vm`.
- Configure proper SSH permissions.
- Verify passwordless SSH access as the root user.

---

## Step 1: Verify VM Information

List Azure virtual machines and obtain the VM public IP address:

```bash
az vm list -d -o table
```

Or retrieve the public IP directly:

```bash
az vm show \
  --resource-group <RESOURCE_GROUP> \
  --name datacenter-vm \
  -d \
  --query publicIps \
  -o tsv
```

---

## Step 2: Retrieve the Root User Public Key

On the Azure client host:

```bash
cat /root/.ssh/id_rsa.pub
```

Example output:

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...
```

Copy the complete key.

---

## Step 3: Connect to the Azure VM

Connect using the default Azure user:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

---

## Step 4: Configure Root SSH Access

Create the root SSH directory if it does not exist:

```bash
sudo mkdir -p /root/.ssh
```

Set secure permissions:

```bash
sudo chmod 700 /root/.ssh
```

Create or edit the authorized keys file:

```bash
sudo vi /root/.ssh/authorized_keys
```

Paste the copied public key into the file and save.

---

## Step 5: Configure Permissions

Set the correct ownership and permissions:

```bash
sudo chmod 600 /root/.ssh/authorized_keys
sudo chown -R root:root /root/.ssh
```

Verify:

```bash
sudo ls -ld /root/.ssh
sudo ls -l /root/.ssh/authorized_keys
```

Expected:

```text
drwx------ root root /root/.ssh
-rw------- root root authorized_keys
```

---

## Step 6: Verify SSH Configuration

Check SSH daemon settings:

```bash
sudo grep -E "PermitRootLogin|PubkeyAuthentication" /etc/ssh/sshd_config
```

Ensure the following settings are present:

```text
PermitRootLogin prohibit-password
PubkeyAuthentication yes
```

If changes are made, restart SSH:

```bash
sudo systemctl restart sshd
```

or

```bash
sudo systemctl restart ssh
```

depending on the Linux distribution.

---

## Step 7: Verify Passwordless Root Login

Exit the VM and test root access from the Azure client host:

```bash
ssh root@<VM_PUBLIC_IP>
```

No password prompt should appear.

---

## Verification Commands

Check user identity:

```bash
ssh root@<VM_PUBLIC_IP> whoami
```

Expected output:

```text
root
```

Check hostname:

```bash
ssh root@<VM_PUBLIC_IP> hostname
```

Expected output:

```text
datacenter-vm
```

---

## Security Considerations

- SSH authentication uses public/private key pairs instead of passwords.
- Root login is restricted to key-based authentication.
- Proper file permissions prevent unauthorized access to SSH credentials.
- Passwordless access improves automation while maintaining security.

---

## Commands Summary

```bash
# Azure Client Host
cat /root/.ssh/id_rsa.pub

az vm list -d -o table

ssh azureuser@<VM_PUBLIC_IP>

# Azure VM
sudo mkdir -p /root/.ssh
sudo chmod 700 /root/.ssh

sudo vi /root/.ssh/authorized_keys

sudo chmod 600 /root/.ssh/authorized_keys
sudo chown -R root:root /root/.ssh

sudo grep -E "PermitRootLogin|PubkeyAuthentication" /etc/ssh/sshd_config
sudo systemctl restart sshd

# Verification
ssh root@<VM_PUBLIC_IP>
ssh root@<VM_PUBLIC_IP> whoami
```

---

## Final Outcome

The root user's public SSH key from the Azure client host was successfully added to the **`datacenter-vm`** virtual machine. Proper SSH permissions were configured, and passwordless SSH authentication for the root user was verified successfully.
