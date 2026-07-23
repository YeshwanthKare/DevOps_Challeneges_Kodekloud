# Terraform - AWS CloudFormation Stack for S3 Bucket

## Overview

The Nautilus DevOps team required automation for AWS infrastructure deployment using Terraform and AWS CloudFormation.

This task creates an AWS CloudFormation stack named `xfusion-stack` using Terraform. The CloudFormation stack provisions an S3 bucket named `xfusion-bucket-4881` with versioning enabled.

---

## Requirements

| Requirement                 | Details                  |
| --------------------------- | ------------------------ |
| Terraform Working Directory | `/home/bob/terraform`    |
| Terraform File              | `main.tf`                |
| Cloud Provider              | AWS                      |
| AWS Region                  | `us-east-1`              |
| Stack Name                  | `xfusion-stack`          |
| Resource Type               | AWS CloudFormation Stack |
| S3 Bucket Name              | `xfusion-bucket-4881`    |
| S3 Versioning               | Enabled                  |

---

## 1. Terraform Configuration

Created the Terraform configuration file:

```bash
cd /home/bob/terraform

vi main.tf
```

---

## 2. Provider Configuration

Configured the AWS provider:

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}
```

---

## 3. CloudFormation Stack Resource

Terraform creates the CloudFormation stack:

```hcl
resource "aws_cloudformation_stack" "xfusion_stack" {
  name = "xfusion-stack"

  template_body = jsonencode({
    AWSTemplateFormatVersion = "2010-09-09"

    Description = "CloudFormation stack to provision an S3 bucket with versioning enabled"

    Resources = {
      XFusionS3Bucket = {
        Type = "AWS::S3::Bucket"

        Properties = {
          BucketName = "xfusion-bucket-4881"

          VersioningConfiguration = {
            Status = "Enabled"
          }
        }
      }
    }
  })
}
```

---

## 4. Initialize Terraform

Initialize Terraform providers:

```bash
terraform init
```

Expected:

```text
Terraform has been successfully initialized!
```

---

## 5. Validate Configuration

Validate the Terraform syntax:

```bash
terraform validate
```

Expected:

```text
Success! The configuration is valid.
```

---

## 6. Review Terraform Plan

Generate the execution plan:

```bash
terraform plan
```

The plan creates:

```text
aws_cloudformation_stack.xfusion_stack
```

---

## 7. Deploy Infrastructure

Apply the Terraform configuration:

```bash
terraform apply
```

Confirm with:

```text
yes
```

Terraform creates:

```text
CloudFormation Stack:
xfusion-stack
```

---

## 8. Verify CloudFormation Stack

Using AWS CLI:

```bash
aws cloudformation describe-stacks \
 --stack-name xfusion-stack \
 --region us-east-1
```

Expected:

```json
{
  "StackName": "xfusion-stack",
  "StackStatus": "CREATE_COMPLETE"
}
```

---

## 9. Verify S3 Bucket

Check the bucket:

```bash
aws s3api get-bucket-versioning \
 --bucket xfusion-bucket-4881 \
 --region us-east-1
```

Expected:

```json
{
  "Status": "Enabled"
}
```

---

## Final Infrastructure

Created resources:

| Resource             | Name                  | Status  |
| -------------------- | --------------------- | ------- |
| CloudFormation Stack | `xfusion-stack`       | Created |
| S3 Bucket            | `xfusion-bucket-4881` | Created |
| S3 Versioning        | Enabled               | Active  |

---

## Validation Checklist

| Check                                   | Status |
| --------------------------------------- | ------ |
| Created `main.tf` only                  | ✅     |
| Used Terraform for deployment           | ✅     |
| Created CloudFormation stack            | ✅     |
| Stack name is `xfusion-stack`           | ✅     |
| S3 bucket name is `xfusion-bucket-4881` | ✅     |
| S3 versioning enabled                   | ✅     |
| Region configured as `us-east-1`        | ✅     |

## Conclusion

The AWS CloudFormation stack `xfusion-stack` was successfully provisioned using Terraform. The stack creates the required S3 bucket `xfusion-bucket-4881` with versioning enabled.
