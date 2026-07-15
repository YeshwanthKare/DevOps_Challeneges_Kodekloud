# Create an AWS IAM User Using Terraform

## Objective

Create an AWS IAM user named **iamuser_rose** using Terraform.

## Terraform Configuration

```hcl
resource "aws_iam_user" "iamuser_rose" {
  name = "iamuser_rose"

  tags = {
    Name = "iamuser_rose"
  }
}
```

## Prerequisites

- An AWS account
- AWS CLI configured with appropriate permissions
- Terraform installed

Verify Terraform installation:

```bash
terraform version
```

Verify AWS credentials:

```bash
aws sts get-caller-identity
```

## Implementation Steps

### 1. Create Terraform Configuration

Create a file named `main.tf` and add the IAM user resource:

```hcl
resource "aws_iam_user" "iamuser_rose" {
  name = "iamuser_rose"

  tags = {
    Name = "iamuser_rose"
  }
}
```

### 2. Initialize Terraform

Initialize the working directory and download required providers:

```bash
terraform init
```

### 3. Review the Execution Plan

Generate and review the execution plan:

```bash
terraform plan
```

Expected action:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

### 4. Apply the Configuration

Create the IAM user:

```bash
terraform apply
```

Confirm by entering:

```text
yes
```

### 5. Verify the IAM User

Using Terraform state:

```bash
terraform state list
```

Expected output:

```text
aws_iam_user.iamuser_rose
```

Using AWS CLI:

```bash
aws iam get-user --user-name iamuser_rose
```

## Cleanup

To remove the IAM user and associated Terraform-managed resources:

```bash
terraform destroy
```

Confirm by entering:

```text
yes
```

## Result

Successfully created an AWS IAM user named **iamuser_rose** using Terraform and verified its existence in AWS IAM.
