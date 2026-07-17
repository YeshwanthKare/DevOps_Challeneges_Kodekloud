# IAM Policy Creation with Terraform

## Overview

This project provisions an AWS IAM policy named **iampolicy_jim** using Terraform. The policy grants read-only access to Amazon EC2 resources, allowing users to view EC2 instances, AMIs, and snapshots through the AWS Management Console and API.

---

## Terraform Configuration

```hcl
resource "aws_iam_policy" "iampolicy_jim" {
  name        = "iampolicy_jim"
  description = "Read-only access to EC2 instances, AMIs, and snapshots"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots"
        ]
        Resource = "*"
      }
    ]
  })
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

### Apply the Configuration

```bash
terraform apply
```

Enter `yes` when prompted to create the IAM policy.

---

## Verification

Verify that the policy was created successfully:

```bash
aws iam list-policies --scope Local | grep iampolicy_jim
```

Or retrieve the policy details:

```bash
aws iam get-policy \
  --policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/iampolicy_jim
```

---

## Permissions Granted

The policy provides the following read-only EC2 permissions:

- `ec2:DescribeInstances`
- `ec2:DescribeImages`
- `ec2:DescribeSnapshots`

These permissions allow users to view:

- EC2 Instances
- Amazon Machine Images (AMIs)
- EBS Snapshots

---

## Result

After successful deployment, the IAM policy **iampolicy_jim** is available in AWS and can be attached to IAM users, groups, or roles requiring read-only access to EC2 resources.
