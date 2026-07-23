# Git Repository Clone - demo.git

## Overview

The DevOps team created a new Git repository for the Nautilus application. The repository was initially unused, and the application development team required a copy of this repository on the Storage Server in the Stratos DC.

This document describes the steps performed to clone the repository using the required user and destination path.

---

## Task Requirements

| Requirement       | Details                     |
| ----------------- | --------------------------- |
| Source Repository | `/opt/demo.git`             |
| Target Server     | Storage Server (`ststor01`) |
| Required User     | `natasha`                   |
| Clone Destination | `/usr/src/kodekloudrepos`   |

---

## 1. Login to Storage Server

Connected to the Storage Server using the `natasha` user.

Command:

```bash
ssh natasha@ststor01
```

---

## 2. Verify Source Repository

Checked that the Git repository exists:

```bash
ls -la /opt/demo.git
```

The repository contains Git metadata files such as:

```text
HEAD
config
description
hooks
info
objects
refs
```

This confirms `/opt/demo.git` is a valid Git repository.

---

## 3. Navigate to Target Directory

Changed to the required clone destination:

```bash
cd /usr/src/kodekloudrepos
```

---

## 4. Clone Repository

The repository was cloned without changing its name or modifying existing directories.

Command:

```bash
git clone /opt/demo.git
```

Output:

```text
Cloning into 'demo'...
done.
```

The repository was created at:

```text
/usr/src/kodekloudrepos/demo
```

---

## 5. Verify Repository Clone

Check the cloned repository:

```bash
ls -la /usr/src/kodekloudrepos
```

Expected output:

```text
demo
```

Navigate into the repository:

```bash
cd /usr/src/kodekloudrepos/demo
```

Check Git status:

```bash
git status
```

Expected output:

```text
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

---

## Final Repository Structure

```text
/usr/src/kodekloudrepos/
└── demo/
    ├── .git/
    ├── HEAD
    ├── config
    ├── description
    ├── hooks/
    ├── info/
    ├── objects/
    └── refs/
```

---

## Validation Checklist

| Check                                             | Status       |
| ------------------------------------------------- | ------------ |
| Repository source `/opt/demo.git` used            | ✅ Completed |
| Clone performed using `natasha` user              | ✅ Completed |
| Repository cloned under `/usr/src/kodekloudrepos` | ✅ Completed |
| No permission changes performed                   | ✅ Completed |
| Existing directories not modified                 | ✅ Completed |

## Conclusion

The `/opt/demo.git` repository has been successfully cloned to the Storage Server under `/usr/src/kodekloudrepos` using the `natasha` user while preserving existing permissions and repository integrity.
