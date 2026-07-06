# Create an AWS VPC with an Amazon-Provided IPv6 CIDR Block Using Terraform

## Objective

Create a VPC named **`datacenter-vpc`** in the **`us-east-1`** AWS region using Terraform.

The VPC must:

- Be named `datacenter-vpc`
- Use an IPv4 CIDR block
- Automatically receive an Amazon-provided IPv6 CIDR block
- Be created using a single Terraform file named `main.tf`

---

## Working Directory

```bash
cd /home/bob/terraform
```

---

## Create the Terraform Configuration

Create the `main.tf` file:

```bash
vi main.tf
```

Add the following configuration:

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

resource "aws_vpc" "datacenter_vpc" {
  cidr_block                       = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true
  enable_dns_hostnames             = true
  enable_dns_support               = true

  tags = {
    Name = "datacenter-vpc"
  }
}
```

---

## Configuration Breakdown

### AWS Provider

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Specifies that all resources will be created in the **us-east-1** region.

---

### VPC Resource

```hcl
resource "aws_vpc" "datacenter_vpc"
```

Creates a new AWS VPC resource.

---

### IPv4 CIDR Block

```hcl
cidr_block = "10.0.0.0/16"
```

Defines the primary IPv4 network range for the VPC.

---

### Amazon-Provided IPv6 CIDR Block

```hcl
assign_generated_ipv6_cidr_block = true
```

Requests an IPv6 CIDR block directly from Amazon's IPv6 address pool.

AWS automatically assigns and associates the IPv6 range with the VPC.

---

### DNS Support

```hcl
enable_dns_hostnames = true
enable_dns_support   = true
```

Enables:

- DNS resolution within the VPC
- Automatic DNS hostnames for instances

---

### Resource Tags

```hcl
tags = {
  Name = "datacenter-vpc"
}
```

Assigns the VPC name tag.

---

## Initialize Terraform

Download the required provider plugins:

```bash
terraform init
```

---

## Validate the Configuration

Generate and review the execution plan:

```bash
terraform plan
```

---

## Deploy the Infrastructure

Create the VPC:

```bash
terraform apply
```

When prompted, enter:

```text
yes
```

---

## Verify the Deployment

List resources managed by Terraform:

```bash
terraform state list
```

View the VPC details:

```bash
aws ec2 describe-vpcs
```

---

## Expected Result

The following VPC will be created:

| Property      | Value           |
| ------------- | --------------- |
| Name          | datacenter-vpc  |
| Region        | us-east-1       |
| IPv4 CIDR     | 10.0.0.0/16     |
| IPv6 CIDR     | Amazon-provided |
| DNS Support   | Enabled         |
| DNS Hostnames | Enabled         |

---

## Key Points

- AWS requires an IPv4 CIDR block when creating a VPC.
- Setting `assign_generated_ipv6_cidr_block = true` enables automatic IPv6 assignment.
- The IPv6 CIDR block is generated and managed by AWS.
- The resulting VPC supports both IPv4 and IPv6 networking (dual-stack).

---

## Summary

This Terraform configuration creates a VPC named **`datacenter-vpc`** in **`us-east-1`** with:

- IPv4 networking (`10.0.0.0/16`)
- Amazon-provided IPv6 addressing
- DNS support enabled
- DNS hostnames enabled

The VPC is ready to host future AWS infrastructure as part of a phased cloud migration strategy.
