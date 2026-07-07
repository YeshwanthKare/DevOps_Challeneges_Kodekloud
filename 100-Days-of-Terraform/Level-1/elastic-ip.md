# Allocate an AWS Elastic IP Using Terraform

## Objective

Create an Elastic IP (EIP) named **devops-eip** in AWS using Terraform.

### Requirements

- Create an Elastic IP resource.
- Name the Elastic IP **devops-eip** using tags.
- Use Terraform.
- Create the configuration in:

```text
/home/bob/terraform/main.tf
```

- Do not create any additional `.tf` files.

---

# Terraform Configuration

Create the `main.tf` file:

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

resource "aws_eip" "devops_eip" {
  domain = "vpc"

  tags = {
    Name = "devops-eip"
  }
}
```

---

# Navigate to the Terraform Working Directory

```bash
cd /home/bob/terraform
```

---

# Initialize Terraform

Download and initialize the required AWS provider plugins:

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

# Validate the Configuration

Check the Terraform syntax before deployment:

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```

---

# Review the Execution Plan

Preview the resources Terraform will create:

```bash
terraform plan
```

You should see Terraform planning to create:

```text
aws_eip.devops_eip
```

---

# Apply the Configuration

Create the Elastic IP resource:

```bash
terraform apply
```

When prompted:

```text
Do you want to perform these actions?
```

Enter:

```text
yes
```

Terraform will allocate the Elastic IP.

---

# Verify the Deployment

View the Terraform-managed resources:

```bash
terraform state list
```

Expected output:

```text
aws_eip.devops_eip
```

Display resource details:

```bash
terraform show
```

Example output:

```text
resource "aws_eip" "devops_eip" {
  public_ip = "x.x.x.x"
}
```

---

# Verify in AWS CLI

List allocated Elastic IPs:

```bash
aws ec2 describe-addresses
```

Example output:

```json
{
  "Addresses": [
    {
      "PublicIp": "x.x.x.x"
    }
  ]
}
```

---

# Understanding the Configuration

## AWS Provider

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Specifies that Terraform will create resources in the AWS `us-east-1` region.

---

## Elastic IP Resource

```hcl
resource "aws_eip" "devops_eip"
```

Creates an AWS Elastic IP resource.

---

## Domain Setting

```hcl
domain = "vpc"
```

Allocates the Elastic IP for use within a VPC.

This is the standard and recommended configuration for modern AWS environments.

---

## Resource Tag

```hcl
tags = {
  Name = "devops-eip"
}
```

Assigns the name:

```text
devops-eip
```

to the Elastic IP resource.

---

# Cleanup (Optional)

To remove the Elastic IP:

```bash
terraform destroy
```

Confirm when prompted:

```text
yes
```

---

# Summary

This task accomplishes the following:

- Creates an AWS Elastic IP.
- Names the resource **devops-eip**.
- Uses Terraform for infrastructure provisioning.
- Deploys the resource in the **us-east-1** region.
- Allocates the Elastic IP for use within a VPC using:

```hcl
domain = "vpc"
```

The Elastic IP is now available for association with EC2 instances, NAT Gateways, or other supported AWS resources.
