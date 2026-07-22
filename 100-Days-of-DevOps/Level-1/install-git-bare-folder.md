# Git Installation and Bare Repository Setup on Storage Server

## Objective

Prepare the Storage Server for application development by:

1. Installing the Git package using `yum`.
2. Creating a bare Git repository named `/opt/news.git`.

---

# Prerequisites

- Access to the Storage Server (`ststor01`)
- User account with sudo privileges
- Network connectivity between the jump host and storage server

---

# Step 1: Generate SSH Key Pair on Jump Host

Generate a new SSH key pair:

```bash
ssh-keygen
```

Example output:

```text
Generating public/private ed25519 key pair.
Your identification has been saved in /home/thor/.ssh/id_ed25519
Your public key has been saved in /home/thor/.ssh/id_ed25519.pub
```

---

# Step 2: Configure Passwordless SSH Access

Copy the public key to the Storage Server:

```bash
ssh-copy-id natasha@ststor01
```

Verify passwordless access:

```bash
ssh natasha@ststor01
```

---

# Step 3: Install Git on Storage Server

Connect to the Storage Server:

```bash
ssh natasha@ststor01
```

Install Git using yum:

```bash
sudo yum install -y git
```

Verify installation:

```bash
git --version
```

Example output:

```text
git version 2.52.0
```

---

# Step 4: Create the Bare Git Repository

Create the repository:

```bash
sudo git init --bare /opt/news.git
```

Expected output:

```text
Initialized empty Git repository in /opt/news.git/
```

---

# Step 5: Verify Repository Creation

List repository contents:

```bash
ls -la /opt/news.git
```

Expected output:

```text
HEAD
config
description
hooks/
info/
objects/
refs/
```

---

# Understanding a Bare Repository

A bare repository:

- Contains only Git metadata.
- Does not have a working directory.
- Is typically used as a central remote repository.
- Supports push and pull operations from multiple developers.

Example structure:

```text
/opt/news.git
├── HEAD
├── config
├── description
├── hooks/
├── info/
├── objects/
└── refs/
```

---

# Validation Checklist

- Git installed successfully using `yum`.
- Passwordless SSH access configured.
- Bare repository created at:

```text
/opt/news.git
```

- Repository initialization completed without errors.
- Repository contains standard bare Git repository directories.

---

# Useful Commands

Check Git version:

```bash
git --version
```

Verify repository type:

```bash
git --git-dir=/opt/news.git rev-parse --is-bare-repository
```

Expected output:

```text
true
```

List repository contents:

```bash
ls -la /opt/news.git
```

---

# Outcome

The Storage Server is configured with Git, and a bare repository named `/opt/news.git` is available for use as a centralized remote repository for the Nautilus development team's application project.
