# README - Dataset & Model Versioning with Git and DVC

## Overview

This exercise demonstrates how to use **Git** and **DVC (Data Version Control)** to manage multiple versions of datasets and machine learning models. The goal is to preserve the current project state as **v1.0**, create a new branch containing an improved dataset, retrain the model, and verify that switching branches restores the correct data and model versions.

---

## Project Location

```bash
cd /root/code/fraud-detection
```

---

## Step 1: Tag the Current Version

Create a Git tag for the baseline dataset and model.

```bash
git tag v1.0
```

Verify:

```bash
git tag
```

Output:

```text
v1.0
```

---

## Step 2: Create the Improved Version Branch

Create and switch to a new branch:

```bash
git checkout -b v2-improved
```

Verify:

```bash
git branch
```

Output:

```text
* v2-improved
  main
```

---

## Step 3: Replace the Dataset

Copy the improved dataset into the tracked dataset location:

```bash
cp data/raw/transactions_v2.csv data/raw/transactions.csv
```

Update DVC tracking:

```bash
dvc add data/raw/transactions.csv
```

---

## Step 4: Retrain the Pipeline

Run the entire DVC pipeline using the updated dataset:

```bash
dvc repro
```

This regenerates:

```text
data/processed/*
models/model.pkl
dvc.lock
```

---

## Step 5: Commit the New Version

Commit the updated dataset metadata and pipeline outputs:

```bash
git add data/raw/transactions.csv.dvc dvc.lock
git commit -m "Add improved dataset and retrained model"
```

---

## Step 6: Restore the Original Version

Switch back to the main branch:

```bash
git checkout main
```

Restore DVC-managed files:

```bash
dvc checkout
```

Verify:

```bash
dvc status
```

Expected output:

```text
Data and pipelines are up to date.
```

---

## Verification

### Confirm Current Branch

```bash
git branch --show-current
```

Expected:

```text
main
```

### Verify Tag

```bash
git tag
```

Expected:

```text
v1.0
```

### Compare Dataset & Model Versions

View DVC metadata from each version:

```bash
git show v1.0:dvc.lock
```

```bash
git show v2-improved:dvc.lock
```

The dataset and model hashes should differ between versions.

---

## Useful Commands

View branches:

```bash
git branch
```

View tags:

```bash
git tag
```

Check DVC status:

```bash
dvc status
```

Restore tracked files:

```bash
dvc checkout
```

Compare lock files:

```bash
git show main:dvc.lock
git show v2-improved:dvc.lock
```

---

## Outcome

- Tagged the baseline dataset and model as **v1.0**.
- Created a new branch **v2-improved**.
- Replaced the dataset with an improved version.
- Retrained the model using the updated data.
- Committed the new dataset and model version.
- Verified that switching back to **main** restores the original dataset and model state.
