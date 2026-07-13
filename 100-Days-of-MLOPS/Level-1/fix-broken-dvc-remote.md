# Day 12 – Configure SeaweedFS Remote Storage for DVC

## Overview

In this lab, the objective was to configure a shared remote storage backend for DVC (Data Version Control) using SeaweedFS, an S3-compatible object storage solution.

Previously, the `transactions.csv` dataset was migrated from Git to DVC. While Git now tracks only the lightweight `.dvc` metadata file, the actual dataset remains in the local DVC cache. To enable collaboration and reproducibility across the team, a remote storage backend was configured so that DVC-tracked data can be shared among developers.

The repository used in this lab:

```bash
/root/code/fraud-detection
```

---

## Objective

Configure the DVC remote named `s3` to use the SeaweedFS object store and upload the locally cached dataset to remote storage.

### Infrastructure Details

| Parameter       | Value                 |
| --------------- | --------------------- |
| Storage Backend | SeaweedFS             |
| Endpoint URL    | http://localhost:8333 |
| Bucket Name     | dvc-storage           |
| Access Key      | weedadmin             |
| Secret Key      | weedadmin123          |
| DVC Remote Name | s3                    |

---

## Initial Problem

The existing `.dvc/config` file contained an incorrect or incomplete configuration:

- Incorrect bucket reference
- Missing or incorrect endpoint URL
- No default remote configured

As a result, DVC operations such as:

```bash
dvc push
```

failed to upload data to the remote storage.

---

## Solution

### Step 1: Navigate to the Repository

```bash
cd /root/code/fraud-detection
```

Inspect the current configuration:

```bash
cat .dvc/config
```

---

### Step 2: Configure the Remote

#### Update the Bucket URL

```bash
dvc remote modify s3 url s3://dvc-storage
```

#### Configure the SeaweedFS Endpoint

```bash
dvc remote modify s3 endpointurl http://localhost:8333
```

#### Set the Default Remote

```bash
dvc remote default s3
```

---

## Final DVC Configuration

After applying the changes, the `.dvc/config` file should contain:

```ini
[core]
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```

---

## Commit Configuration Changes

Since the DVC configuration is part of the project setup, the updated configuration was committed to Git.

### Stage the Configuration

```bash
git add .dvc/config
```

### Commit Changes

```bash
git commit -m "Configure SeaweedFS S3 remote for DVC"
```

---

## Upload Data to Remote Storage

Push the DVC cache to the configured SeaweedFS bucket:

```bash
dvc push
```

Expected output:

```text
1 file pushed
```

This uploads the dataset tracked by DVC to the remote object store.

---

## Verification

### Verify DVC Remote

```bash
dvc remote list
```

Expected output:

```text
s3    s3://dvc-storage
```

### Verify DVC Status

```bash
dvc status -c
```

Expected output:

```text
Data and pipelines are up to date.
```

---

## Verify Data in SeaweedFS

Open the SeaweedFS Filer UI using the forwarded port provided in the lab environment.

Navigate to:

```text
/buckets/dvc-storage/files/md5/
```

You should see DVC-managed objects stored by their content hashes.

Example structure:

```text
md5/
├── 0a/
├── 1f/
├── 5c/
└── ...
```

This is expected behavior because DVC stores data using content-addressable storage.

---

## DVC Workflow After Configuration

### Developer 1

```bash
git add data.csv.dvc
git commit -m "Add dataset"
git push

dvc push
```

### Developer 2

```bash
git clone <repository>

dvc pull
```

Result:

- Source code is retrieved from Git.
- Datasets are retrieved from SeaweedFS.
- Everyone works with identical data versions.

---

## Benefits of Using DVC with SeaweedFS

- Keeps large datasets out of Git.
- Enables versioning of datasets and ML artifacts.
- Supports team collaboration.
- Ensures reproducible experiments.
- Reduces repository size.
- Provides centralized storage for machine learning assets.

---

## Commands Summary

```bash
cd /root/code/fraud-detection

dvc remote modify s3 url s3://dvc-storage

dvc remote modify s3 endpointurl http://localhost:8333

dvc remote default s3

git add .dvc/config

git commit -m "Configure SeaweedFS S3 remote for DVC"

dvc push
```

---

## Outcome

Successfully configured SeaweedFS as the default DVC remote storage backend, committed the configuration changes to Git, and uploaded the DVC-managed dataset to the shared object storage bucket. The `fraud-detection` project is now ready for collaborative data versioning and reproducible machine learning workflows.
