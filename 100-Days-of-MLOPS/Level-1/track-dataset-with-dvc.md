# Track Dataset with DVC Instead of Git

## Objective

Move the dataset `data/raw/transactions.csv` from Git tracking to DVC tracking while keeping the file on disk.

### Requirements

- Remove `data/raw/transactions.csv` from Git tracking without deleting it.
- Track the dataset using DVC.
- Generate a `.dvc` pointer file.
- Ensure `data/raw/.gitignore` excludes the dataset.
- Commit the changes with the message:

```text
Track transactions dataset with DVC
```

---

## Prerequisites

- DVC is already initialized in the repository.
- Git repository already exists.
- Dataset file:

```text
data/raw/transactions.csv
```

Repository location:

```bash
cd /root/code/fraud-detection
```

---

## Step 1: Navigate to the Repository

```bash
cd /root/code/fraud-detection
```

Verify that the dataset is currently tracked by Git:

```bash
git ls-files | grep transactions.csv
```

Expected output:

```text
data/raw/transactions.csv
```

---

## Step 2: Remove the Dataset from Git Tracking

Remove the file from Git tracking while keeping it on disk:

```bash
git rm --cached data/raw/transactions.csv
```

Verify that the file still exists:

```bash
ls -l data/raw/transactions.csv
```

The file should remain available in the filesystem.

---

## Step 3: Track the Dataset with DVC

Add the dataset to DVC:

```bash
dvc add data/raw/transactions.csv
```

Expected output:

```text
To track the changes with git, run:

git add data/raw/.gitignore data/raw/transactions.csv.dvc
```

This command creates:

```text
data/raw/transactions.csv.dvc
data/raw/.gitignore
```

---

## Step 4: Verify DVC Files

List the contents of the directory:

```bash
ls -la data/raw
```

Example output:

```text
data/raw/
├── .gitignore
├── transactions.csv
└── transactions.csv.dvc
```

Inspect the generated files:

```bash
cat data/raw/.gitignore
cat data/raw/transactions.csv.dvc
```

---

## Step 5: Check Git Status

```bash
git status
```

You should see:

- `transactions.csv` marked for removal from Git.
- `.gitignore` added.
- `transactions.csv.dvc` added.

---

## Step 6: Stage the Changes

Stage the DVC files:

```bash
git add data/raw/transactions.csv.dvc
git add data/raw/.gitignore
git add -u
```

Verify:

```bash
git status
```

All changes should be staged.

---

## Step 7: Commit the Changes

Create the required Git commit:

```bash
git commit -m "Track transactions dataset with DVC"
```

Example output:

```text
[main abc1234] Track transactions dataset with DVC
 3 files changed
```

---

## Step 8: Verify the Commit

View the latest commit:

```bash
git log --oneline -n 1
```

Expected output:

```text
abc1234 Track transactions dataset with DVC
```

---

## Step 9: Verify Git No Longer Tracks the Dataset

Check tracked files:

```bash
git ls-files | grep transactions.csv
```

Expected output:

```text
data/raw/transactions.csv.dvc
```

The dataset itself should no longer be tracked by Git.

---

## Step 10: Verify DVC Tracking

Check DVC status:

```bash
dvc status
```

Expected output:

```text
Data and pipelines are up to date.
```

---

## Result

After completing these steps:

- `transactions.csv` remains on disk.
- Git no longer tracks the dataset file.
- DVC manages the dataset.
- `transactions.csv.dvc` stores the dataset metadata.
- `data/raw/.gitignore` prevents accidental Git commits of the dataset.
- The migration is recorded in Git with the commit:

```text
Track transactions dataset with DVC
```

The DVC extension will recognize the dataset and display it under the **DVC TRACKED** section in VS Code.
