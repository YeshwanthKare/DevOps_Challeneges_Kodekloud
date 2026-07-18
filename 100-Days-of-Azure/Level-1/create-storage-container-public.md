# Azure Storage Account and Public Blob Container Creation

## Overview

As part of the Azure data migration initiative, a new Storage Account and a publicly accessible Blob Container were created to store application data within the Azure environment.

The configuration enables **anonymous read access for containers and blobs**, allowing public access to blob data when required.

---

## Resources Created

| Resource Type   | Name                           |
| --------------- | ------------------------------ |
| Storage Account | `xfusionst24251`               |
| Blob Container  | `xfusion-blob-7147`            |
| Resource Group  | `kml_rg_main-9ecec3db63c645f8` |
| Region          | `eastus`                       |

---

## Prerequisites

Retrieve Azure credentials from the Azure client host:

```bash id="9z9mwl"
showcreds
```

Login to Azure:

```bash id="6h9v79"
az login
```

---

## Create the Storage Account

```bash id="l8j7ku"
az storage account create \
  --name xfusionst24251 \
  --resource-group kml_rg_main-9ecec3db63c645f8 \
  --location eastus \
  --sku Standard_RAGRS \
  --kind StorageV2 \
  --access-tier Hot \
  --allow-blob-public-access true \
  --allow-shared-key-access true \
  --min-tls-version TLS1_2 \
  --public-network-access Enabled \
  --routing-choice MicrosoftRouting \
  --enable-hierarchical-namespace false \
  --encryption-services blob
```

### Configuration Highlights

- Storage Account Type: `StorageV2`
- Replication: `Standard_RAGRS`
- Access Tier: `Hot`
- Public Blob Access: Enabled
- TLS Version: `TLS1_2`
- Encryption: Enabled for Blob Storage

---

## Create the Public Blob Container

```bash id="huxm9m"
az storage container create \
  --name xfusion-blob-7147 \
  --account-name xfusionst24251 \
  --public-access container \
  --auth-mode login
```

### Public Access Level

```text
container
```

This setting enables:

- Anonymous read access to blobs
- Anonymous listing of blobs within the container

---

## Verification

### Verify Storage Account

```bash id="m92s1r"
az storage account show \
  --name xfusionst24251 \
  --resource-group kml_rg_main-9ecec3db63c645f8
```

### Verify Container

```bash id="egpq2r"
az storage container show \
  --name xfusion-blob-7147 \
  --account-name xfusionst24251 \
  --auth-mode login
```

### Verify Public Access Setting

```bash id="31t5ub"
az storage container show \
  --name xfusion-blob-7147 \
  --account-name xfusionst24251 \
  --query properties.publicAccess
```

Expected output:

```text
container
```

---

## Result

Successfully created:

- Azure Storage Account: `xfusionst24251`
- Public Blob Container: `xfusion-blob-7147`

The storage account allows blob public access, and the container is configured with **anonymous read access for both containers and blobs**, meeting the migration requirements.
