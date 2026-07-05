# Create a VPC using Terraform (AWS)

## Objective

Create an AWS VPC named **`nautilus-vpc`** in region **`us-east-1`** using Terraform.

Only the VPC resource is required. No subnets, gateways, or additional networking components are needed.

---

## Prerequisites

- Terraform installed
- AWS credentials configured
- Working directory: `/home/bob/terraform`

---

## Step 1: Navigate to Working Directory

```bash
cd /home/bob/terraform
```

---

## Step 2: Create `main.tf`

Create a single Terraform configuration file:

```bash
vi main.tf
```

---

## Step 3: Add Terraform Configuration

Paste the following content into `main.tf`:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "nautilus-vpc"
  }
}
```

---

## Step 4: Initialize Terraform

```bash
terraform init
```

---

## Step 5: Apply Configuration

```bash
terraform apply -auto-approve
```

---

## Step 6: Verify Deployment

Check Terraform state:

```bash
terraform state list
```

Or verify using AWS CLI:

```bash
aws ec2 describe-vpcs
```

---

## Result

A VPC will be created with the following details:

- Name: `nautilus-vpc`
- Region: `us-east-1`
- CIDR Block: `10.0.0.0/16`

---

## Notes

- Only the VPC is required for this task.
- No additional AWS networking resources (subnets, route tables, IGW) are needed.
- Ensure AWS credentials are properly configured before running Terraform.
