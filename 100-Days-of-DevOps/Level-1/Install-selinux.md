# Install Required SELinux Packages and Permanently Disable SELinux

## Objective

On **App Server 1**, perform the following tasks:

- Install the required SELinux packages.
- Permanently disable SELinux.
- No immediate reboot is required. The change will take effect after the next scheduled reboot.

---

## What is SELinux?

**SELinux (Security-Enhanced Linux)** is a Linux kernel security module that implements **Mandatory Access Control (MAC)**.

Unlike traditional Linux file permissions, SELinux uses security policies to control how users, processes, and services can interact with files and system resources.

This provides an additional layer of protection against unauthorized access and security vulnerabilities.

---

## Step 1: Connect to App Server 1

```bash
ssh tony@stapp01
```

---

## Step 2: Verify the Operating System

Check the operating system version to determine the appropriate package manager.

```bash
cat /etc/*release
```

Example output:

```text
CentOS Linux release 8.x
```

Since the server is running CentOS, use `yum` to install the required packages.

---

## Step 3: Install Required SELinux Packages

Install the SELinux policy packages and management utilities:

```bash
sudo yum install -y selinux-policy selinux-policy-targeted policycoreutils
```

### Package Description

| Package                   | Purpose                                            |
| ------------------------- | -------------------------------------------------- |
| `selinux-policy`          | Base SELinux policy package                        |
| `selinux-policy-targeted` | Default SELinux policy targeting specific services |
| `policycoreutils`         | Utilities used to manage and configure SELinux     |

---

## Step 4: Disable SELinux Permanently

Edit the SELinux configuration file:

```bash
sudo vi /etc/selinux/config
```

Locate the following line:

```text
SELINUX=enforcing
```

Change it to:

```text
SELINUX=disabled
```

Save and exit the editor:

```text
:wq
```

---

## Step 5: Verify the Configuration Change

Confirm that SELinux has been configured to be disabled after the next reboot:

```bash
grep SELINUX= /etc/selinux/config
```

Expected output:

```text
SELINUX=disabled
```

---

## Verification

Check the current SELinux status:

```bash
sestatus
```

Example output before reboot:

```text
SELinux status:                 enabled
Current mode:                   enforcing
Mode from config file:          disabled
```

This indicates that SELinux is still active but will be disabled after the next system reboot.

---

## Summary

- Verified the operating system version.
- Installed the required SELinux packages.
- Updated `/etc/selinux/config`.
- Set `SELINUX=disabled`.
- Verified the configuration change.

---

## Result

The following SELinux packages are installed:

```text
selinux-policy
selinux-policy-targeted
policycoreutils
```

SELinux has been permanently disabled by updating:

```text
/etc/selinux/config
```

Configuration:

```text
SELINUX=disabled
```

The change will take effect after the next reboot.
