# README - Create IAM Policy `iampolicy_john` (EC2 Read-Only Access)

## Overview

This task creates an AWS IAM policy named **`iampolicy_john`** that grants **read-only access** to Amazon EC2 resources. The policy allows users to view EC2 instances, AMIs, snapshots, volumes, security groups, and other EC2-related resources through the AWS Management Console and CLI.

---

## Policy Requirements

- **Policy Name:** `iampolicy_john`
- **Region:** `us-east-1`
- **Access Level:** Read-only
- **Service:** Amazon EC2

---

## IAM Policy JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["ec2:Describe*"],
      "Resource": "*"
    }
  ]
}
```

---

## Terraform Configuration

Create the following `main.tf` file:

```hcl
resource "aws_iam_policy" "iampolicy_john" {
  name        = "iampolicy_john"
  description = "Read-only access to EC2 resources"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:Describe*"
        ]
        Resource = "*"
      }
    ]
  })
}
```

---

## Deployment Steps

### Initialize Terraform

```bash
cd /home/bob/terraform
terraform init
```

### Review the Execution Plan

```bash
terraform plan
```

### Apply the Configuration

```bash
terraform apply
```

Type `yes` when prompted.

---

## Verification

List the IAM policy:

```bash
aws iam list-policies --scope Local
```

Get policy details:

```bash
aws iam get-policy --policy-arn <policy-arn>
```

---

## Outcome

The IAM policy **`iampolicy_john`** is successfully created and grants read-only access to Amazon EC2 resources using the `ec2:Describe*` permission set.
