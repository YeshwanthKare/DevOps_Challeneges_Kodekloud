# AWS IAM Group Creation Using Terraform

## Overview

As part of the AWS cloud migration initiative, the **mark DevOps Team** required the creation of an IAM group named **iamgroup_mark** using Terraform.

This task demonstrates Infrastructure as Code (IaC) principles by provisioning AWS IAM resources through Terraform instead of manually creating them in the AWS Management Console.

---

# Objective

Create an AWS IAM group with the following specifications:

| Property           | Value                 |
| ------------------ | --------------------- |
| Resource Type      | IAM Group             |
| Group Name         | `iamgroup_mark`       |
| Tool               | Terraform             |
| Working Directory  | `/home/bob/terraform` |
| Configuration File | `main.tf`             |

---

# Terraform Configuration

## main.tf

```hcl
resource "aws_iam_group" "iamgroup_mark" {
  name = "iamgroup_mark"
}
```

---

# Prerequisites

Before deploying the configuration, ensure:

- Terraform is installed.
- AWS credentials are configured.
- The AWS provider is available.
- Appropriate IAM permissions exist to create IAM groups.

Verify Terraform installation:

```bash
terraform version
```

---

# Deployment Steps

## Step 1: Navigate to the Terraform Directory

```bash
cd /home/bob/terraform
```

---

## Step 2: Initialize Terraform

Initialize the working directory and download the required provider plugins.

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

## Step 3: Validate the Execution Plan

Review the infrastructure changes Terraform intends to perform.

```bash
terraform plan
```

Expected output:

```text
# aws_iam_group.iamgroup_mark will be created

+ resource "aws_iam_group" "iamgroup_mark" {
    + name = "iamgroup_mark"
  }

Plan: 1 to add, 0 to change, 0 to destroy.
```

---

## Step 4: Apply the Configuration

Create the IAM group in AWS.

```bash
terraform apply
```

When prompted, enter:

```text
yes
```

Expected output:

```text
aws_iam_group.iamgroup_mark: Creating...
aws_iam_group.iamgroup_mark: Creation complete

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

# Verification

Verify the IAM group was successfully created:

```bash
terraform state list
```

Expected output:

```text
aws_iam_group.iamgroup_mark
```

You can also verify through the AWS CLI:

```bash
aws iam get-group --group-name iamgroup_mark
```

Expected output:

```json
{
  "Group": {
    "GroupName": "iamgroup_mark"
  }
}
```

---

# Terraform Resource Details

| Attribute     | Value           |
| ------------- | --------------- |
| Resource Type | `aws_iam_group` |
| Resource Name | `iamgroup_mark` |
| Group Name    | `iamgroup_mark` |
| Path          | `/` (default)   |

---

# Cleanup (Optional)

To remove the IAM group:

```bash
terraform destroy
```

Confirm by entering:

```text
yes
```

---

# Benefits of Using Terraform

- Infrastructure as Code (IaC)
- Repeatable deployments
- Version-controlled infrastructure
- Reduced manual configuration errors
- Automated provisioning and lifecycle management

---

# Result

The AWS IAM group **iamgroup_mark** was successfully provisioned using Terraform from the `/home/bob/terraform` working directory. The infrastructure is now managed as code and can be recreated, modified, or destroyed consistently through Terraform workflows.
