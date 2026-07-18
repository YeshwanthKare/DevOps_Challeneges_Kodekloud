# DynamoDB Table Creation with Terraform

## Overview

The Nautilus DevOps team required a DynamoDB table to store user-related information in AWS. Terraform was used to provision the table following Infrastructure as Code (IaC) best practices.

---

## Requirements

| Setting          | Value              |
| ---------------- | ------------------ |
| Table Name       | `datacenter-users` |
| Primary Key      | `datacenter_id`    |
| Primary Key Type | String (`S`)       |
| Billing Mode     | `PAY_PER_REQUEST`  |

---

## Terraform Configuration

Create the `main.tf` file in the Terraform working directory:

```hcl
resource "aws_dynamodb_table" "datacenter_users" {
  name         = "datacenter-users"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "datacenter_id"

  attribute {
    name = "datacenter_id"
    type = "S"
  }

  tags = {
    Name = "datacenter-users"
  }
}
```

---

## Deployment Steps

### Navigate to the Terraform Directory

```bash
cd /home/bob/terraform
```

### Initialize Terraform

```bash
terraform init
```

### Review the Execution Plan

```bash
terraform plan
```

Expected output:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

### Apply the Configuration

```bash
terraform apply
```

Confirm when prompted:

```text
yes
```

Expected output:

```text
aws_dynamodb_table.datacenter_users: Creation complete

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## Verification

Verify that the table was created successfully:

```bash
aws dynamodb describe-table \
  --table-name datacenter-users \
  --region us-east-1
```

Expected values:

```text
TableName: datacenter-users
BillingMode: PAY_PER_REQUEST
HashKey: datacenter_id
AttributeType: S
```

---

## Result

Successfully created the DynamoDB table with the following configuration:

- Table Name: `datacenter-users`
- Primary Key: `datacenter_id` (String)
- Billing Mode: `PAY_PER_REQUEST`
- Managed using Terraform
- Deployed from `/home/bob/terraform/main.tf`
