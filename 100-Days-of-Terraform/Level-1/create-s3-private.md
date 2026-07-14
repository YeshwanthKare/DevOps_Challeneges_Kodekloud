# AWS S3 Private Bucket Creation Using Terraform

## Overview

As part of the AWS migration initiative, the Nautilus DevOps team was tasked with creating a secure Amazon S3 bucket using Terraform.

The objective was to provision an S3 bucket named **`nautilus-s3-6062`** and ensure that **all public access is blocked**, making the bucket completely private.

---

## Requirements

### Bucket Configuration

| Parameter         | Value            |
| ----------------- | ---------------- |
| Bucket Name       | nautilus-s3-6062 |
| Region            | us-east-1        |
| Access Type       | Private          |
| Provisioning Tool | Terraform        |

### Security Requirements

- Block all public ACLs
- Block all public bucket policies
- Ignore any public ACLs
- Restrict public bucket access
- Apply a private ACL

---

## Terraform Configuration

### Create the S3 Bucket

```hcl
resource "aws_s3_bucket" "nautilus_s3_6062" {
  bucket = "nautilus-s3-6062"

  tags = {
    Name = "nautilus-s3-6062"
  }
}
```

### Block All Public Access

```hcl
resource "aws_s3_bucket_public_access_block" "nautilus_s3_6062" {
  bucket = aws_s3_bucket.nautilus_s3_6062.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

### Apply Private ACL

```hcl
resource "aws_s3_bucket_acl" "nautilus_s3_6062" {
  bucket = aws_s3_bucket.nautilus_s3_6062.id
  acl    = "private"
}
```

---

## Deployment Steps

### Navigate to Terraform Directory

```bash
cd /home/bob/terraform
```

### Initialize Terraform

```bash
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Format Terraform Files

```bash
terraform fmt
```

### Create Resources

```bash
terraform apply -auto-approve
```

---

## Verification

### Verify Bucket Creation

```bash
aws s3 ls | grep nautilus-s3-6062
```

Expected output:

```text
nautilus-s3-6062
```

### Verify Public Access Block Settings

```bash
aws s3api get-public-access-block \
  --bucket nautilus-s3-6062
```

Expected result:

```json
{
  "PublicAccessBlockConfiguration": {
    "BlockPublicAcls": true,
    "IgnorePublicAcls": true,
    "BlockPublicPolicy": true,
    "RestrictPublicBuckets": true
  }
}
```

### Verify Bucket ACL

```bash
aws s3api get-bucket-acl \
  --bucket nautilus-s3-6062
```

Expected result:

- ACL is set to `private`
- No public read permissions exist

---

## Resources Created

| Resource Type       | Resource Name    |
| ------------------- | ---------------- |
| S3 Bucket           | nautilus-s3-6062 |
| Public Access Block | nautilus_s3_6062 |
| Bucket ACL          | private          |

---

## Security Benefits

The configuration ensures:

- No public ACLs can be applied.
- No public bucket policies can be attached.
- Existing public ACLs are ignored.
- Bucket access is restricted to authorized AWS identities only.
- Data remains private and protected.

---

## Commands Summary

```bash
cd /home/bob/terraform

terraform init
terraform validate
terraform fmt
terraform apply -auto-approve

aws s3 ls | grep nautilus-s3-6062

aws s3api get-public-access-block \
  --bucket nautilus-s3-6062

aws s3api get-bucket-acl \
  --bucket nautilus-s3-6062
```

---

## Final Outcome

The Terraform configuration successfully created the **`nautilus-s3-6062`** S3 bucket and enforced a private security posture by blocking all forms of public access. The bucket is now suitable for securely storing application and migration data within the AWS environment.
