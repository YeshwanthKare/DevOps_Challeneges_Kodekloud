# AWS S3 Public Bucket Creation Using Terraform

## Overview

As part of the AWS migration initiative, the Nautilus DevOps team was tasked with creating a publicly accessible Amazon S3 bucket using Terraform.

The objective was to provision an S3 bucket named **`devops-s3-22000`** in the **`us-east-1`** region and configure it for public read access using an appropriate ACL.

---

## Requirements

### Bucket Configuration

- Bucket Name: `devops-s3-22000`
- Region: `us-east-1`
- Access Type: Public
- Provisioning Tool: Terraform

### Public Access Requirements

- Public ACLs must be allowed.
- Bucket policy restrictions must be disabled.
- Bucket ACL must be configured as `public-read`.

---

## Terraform Configuration

### Create S3 Bucket

```hcl
resource "aws_s3_bucket" "devops_s3_22000" {
  bucket = "devops-s3-22000"

  tags = {
    Name = "devops-s3-22000"
  }
}
```

### Disable Public Access Restrictions

```hcl
resource "aws_s3_bucket_public_access_block" "devops_s3_22000" {
  bucket = aws_s3_bucket.devops_s3_22000.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```

### Configure Bucket Ownership Controls

```hcl
resource "aws_s3_bucket_ownership_controls" "devops_s3_22000" {
  bucket = aws_s3_bucket.devops_s3_22000.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}
```

### Apply Public Read ACL

```hcl
resource "aws_s3_bucket_acl" "devops_s3_22000" {
  depends_on = [
    aws_s3_bucket_public_access_block.devops_s3_22000,
    aws_s3_bucket_ownership_controls.devops_s3_22000
  ]

  bucket = aws_s3_bucket.devops_s3_22000.id
  acl    = "public-read"
}
```

---

## Deployment Steps

### Initialize Terraform

```bash
cd /home/bob/terraform
terraform init
```

### Validate Configuration

```bash
terraform validate
```

### Format Configuration

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
aws s3 ls | grep devops-s3-22000
```

### Verify Bucket ACL

```bash
aws s3api get-bucket-acl \
  --bucket devops-s3-22000 \
  --region us-east-1
```

Expected result:

- Bucket exists in `us-east-1`
- ACL includes `READ` permission for public access
- Bucket is publicly accessible

---

## Resources Created

| Resource Type       | Resource Name   |
| ------------------- | --------------- |
| S3 Bucket           | devops-s3-22000 |
| Public Access Block | devops_s3_22000 |
| Ownership Controls  | devops_s3_22000 |
| Bucket ACL          | public-read     |

---

## Final Outcome

The Terraform configuration successfully:

- Created the S3 bucket **`devops-s3-22000`**
- Configured ownership controls
- Disabled public access restrictions
- Applied a **public-read** ACL
- Made the bucket publicly accessible as required by the migration project

This implementation satisfies all stated lab requirements for creating a public S3 bucket using Terraform in the **us-east-1** region.
