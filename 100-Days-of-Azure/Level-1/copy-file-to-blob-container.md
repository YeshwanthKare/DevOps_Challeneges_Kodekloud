# Azure Blob Storage - Uploading Local Files to a Storage Container

## Overview

This guide demonstrates how to upload a local file to an Azure Blob Storage container using the Azure CLI. The example uses the following resources:

- **Storage Account:** `xfusionst24251`
- **Container:** `xfusion-blob-7147`

---

## Prerequisites

Ensure you are authenticated to Azure:

```bash
az login
```

Verify your account:

```bash
az account show
```

---

## Upload a Local File

### Syntax

```bash
az storage blob upload \
  --account-name <storage-account-name> \
  --container-name <container-name> \
  --name <blob-name> \
  --file <local-file-path> \
  --auth-mode login
```

### Example

Upload `sample.txt` from the current directory:

```bash
az storage blob upload \
  --account-name xfusionst24251 \
  --container-name xfusion-blob-7147 \
  --name sample.txt \
  --file ./sample.txt \
  --auth-mode login
```

Expected output:

```json
{
  "etag": "\"0x8DXXXXXXXXXXXX\"",
  "lastModified": "2026-07-19T10:15:00+00:00"
}
```

---

## Upload While Preserving the Original Filename

```bash
FILE=sample.txt

az storage blob upload \
  --account-name xfusionst24251 \
  --container-name xfusion-blob-7147 \
  --name "$FILE" \
  --file "$FILE" \
  --auth-mode login
```

---

## List Uploaded Blobs

Verify that the file exists in the container:

```bash
az storage blob list \
  --account-name xfusionst24251 \
  --container-name xfusion-blob-7147 \
  --auth-mode login \
  --output table
```

Example output:

```text
Name         Blob Type    Blob Tier
-----------  -----------  ---------
sample.txt   BlockBlob    Hot
```

---

## Download a Blob

```bash
az storage blob download \
  --account-name xfusionst24251 \
  --container-name xfusion-blob-7147 \
  --name sample.txt \
  --file downloaded-sample.txt \
  --auth-mode login
```

---

## Delete a Blob

```bash
az storage blob delete \
  --account-name xfusionst24251 \
  --container-name xfusion-blob-7147 \
  --name sample.txt \
  --auth-mode login
```

---

## Useful Commands

### List Containers

```bash
az storage container list \
  --account-name xfusionst24251 \
  --auth-mode login \
  --output table
```

### Show Storage Account Details

```bash
az storage account show \
  --name xfusionst24251 \
  --resource-group kml_rg_main-9ecec3db63c645f8
```

---

## Summary

| Action          | Command                     |
| --------------- | --------------------------- |
| Upload file     | `az storage blob upload`    |
| List blobs      | `az storage blob list`      |
| Download blob   | `az storage blob download`  |
| Delete blob     | `az storage blob delete`    |
| List containers | `az storage container list` |

This process allows you to securely upload local files into Azure Blob Storage for centralized and scalable cloud storage.
