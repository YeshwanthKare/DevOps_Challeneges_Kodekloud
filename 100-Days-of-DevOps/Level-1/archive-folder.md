# Website Content Archiving Automation Script

## Overview

The production support team at xFusionCorp Industries requires a Bash script to automate the archiving of website content hosted on **App Server 2**. The script creates a ZIP archive of the website files, stores it locally, and copies it to the Nautilus Storage Server for long-term retention.

---

## Objective

Create a Bash script named:

```bash
/scripts/official_archive.sh
```

The script must:

1. Create a ZIP archive of:

   ```bash
   /var/www/html/official
   ```

2. Save the archive as:

   ```bash
   /archives/xfusioncorp_official.zip
   ```

3. Copy the archive to the Nautilus Storage Server:

   ```bash
   natasha@ststor01:/archives/
   ```

4. Run without prompting for a password during file transfer.

5. Avoid using `sudo` inside the script.

---

## Prerequisites

### Install ZIP Package

The `zip` package must be installed manually on App Server 2 before running the script.

```bash
sudo yum install -y zip
```

Verify installation:

```bash
zip -v
```

---

## Configure Passwordless SSH

### Generate SSH Key

If no SSH key exists:

```bash
ssh-keygen -t ed25519
```

Press **Enter** to accept the default file location.

---

### Copy Public Key to Storage Server

```bash
ssh-copy-id natasha@ststor01
```

Enter the storage server password when prompted.

---

### Verify Passwordless Access

```bash
ssh natasha@ststor01
```

The login should succeed without requesting a password.

Exit the session:

```bash
exit
```

---

## Create Script Directory

```bash
mkdir -p /scripts
```

---

## Create the Archiving Script

Create the script file:

```bash
vi /scripts/official_archive.sh
```

Add the following content:

```bash
#!/bin/bash

zip -r /archives/xfusioncorp_official.zip /var/www/html/official
scp /archives/xfusioncorp_official.zip natasha@ststor01:/archives/
```

Save and exit.

---

## Make the Script Executable

```bash
chmod 755 /scripts/official_archive.sh
```

Verify permissions:

```bash
ls -l /scripts/official_archive.sh
```

Expected output:

```text
-rwxr-xr-x 1 steve steve ... /scripts/official_archive.sh
```

---

## Execute the Script

Run:

```bash
/scripts/official_archive.sh
```

Example output:

```text
adding: var/www/html/official/
adding: var/www/html/official/index.html
xfusioncorp_official.zip 100%
```

---

## Validation

### Verify Local Archive

```bash
ls -l /archives/xfusioncorp_official.zip
```

Expected:

```text
-rw-r--r-- 1 steve steve ... xfusioncorp_official.zip
```

---

### Verify Archive on Storage Server

```bash
ssh natasha@ststor01
ls -l /archives/xfusioncorp_official.zip
```

Expected:

```text
-rw-r--r-- 1 natasha natasha ... xfusioncorp_official.zip
```

---

## Script Explanation

```bash
zip -r /archives/xfusioncorp_official.zip /var/www/html/official
```

Creates a recursive ZIP archive of the website content.

```bash
scp /archives/xfusioncorp_official.zip natasha@ststor01:/archives/
```

Copies the archive to the Nautilus Storage Server.

---

## Complete Command Summary

```bash
sudo yum install -y zip

ssh-keygen -t ed25519

ssh-copy-id natasha@ststor01

mkdir -p /scripts

cat > /scripts/official_archive.sh <<'EOF'
#!/bin/bash

zip -r /archives/xfusioncorp_official.zip /var/www/html/official
scp /archives/xfusioncorp_official.zip natasha@ststor01:/archives/
EOF

chmod 755 /scripts/official_archive.sh

/scripts/official_archive.sh
```

---

## Validation Checklist

- [x] ZIP package installed on App Server 2
- [x] Passwordless SSH configured to Storage Server
- [x] Script created at `/scripts/official_archive.sh`
- [x] Archive name is `xfusioncorp_official.zip`
- [x] Archive stored in `/archives/`
- [x] Archive copied to Storage Server `/archives/`
- [x] No `sudo` commands used inside the script
- [x] Script executable by the App Server 2 user
