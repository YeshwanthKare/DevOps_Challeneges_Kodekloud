# Grant Executable Permissions to a Script for All Users

## Objective

Grant executable permissions to the script **`/tmp/xfusioncorp.sh`** on **App Server 1** and ensure that **all users** can execute it.

---

## Understanding the Requirement

For a shell script to run successfully:

- It must be **readable (`r`)** so the shell can read its contents.
- It must be **executable (`x`)** so the operating system can execute it.

Granting only execute permissions (`x`) without read permissions (`r`) can cause the script to fail when executed.

---

## Step 1: Connect to App Server 1

```bash
ssh banner@stapp01
```

---

## Step 2: Grant Read and Execute Permissions to All Users

Use the following command to add **read (`r`)** and **execute (`x`)** permissions for **all users (`a`)**:

```bash
sudo chmod a+rx /tmp/xfusioncorp.sh
```

### Alternative Method

You can achieve the same result using numeric permissions:

```bash
sudo chmod 755 /tmp/xfusioncorp.sh
```

This sets the permissions to:

```text
rwxr-xr-x
```

Where:

| User Type | Permissions |
| --------- | ----------- |
| Owner     | rwx         |
| Group     | r-x         |
| Others    | r-x         |

---

## Step 3: Verify the Permissions

Check the file permissions:

```bash
ls -l /tmp/xfusioncorp.sh
```

Expected output:

```text
-rwxr-xr-x 1 root root ... /tmp/xfusioncorp.sh
```

---

## Common Mistake

Using:

```bash
sudo chmod a+x /tmp/xfusioncorp.sh
```

may result in:

```text
---x--x--x 1 root root ... /tmp/xfusioncorp.sh
```

This grants execute permissions but does **not** grant read permissions.

Since the shell needs to read the script before executing it, this configuration is insufficient.

---

## Verification Checklist

- Connected to `stapp01`
- Granted read permissions (`r`)
- Granted execute permissions (`x`)
- Verified permissions using `ls -l`
- Confirmed all users can execute the script

---

## Final Command

```bash
sudo chmod a+rx /tmp/xfusioncorp.sh
```

## Expected Result

```text
-rwxr-xr-x
```

The script is now readable and executable by all users on the system.
