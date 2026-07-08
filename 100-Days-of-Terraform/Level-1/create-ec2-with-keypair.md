# Create an EC2 Instance with Key Pair Using Terraform

## Overview

This project demonstrates how to create an AWS EC2 instance using Terraform with SSH key pair authentication.

The Terraform configuration creates only:

- EC2 Instance
- Key Pair association

No additional AWS resources are created:

- Security Groups
- VPC
- Subnets
- Elastic IP
- IAM Roles

---

# Prerequisites

Ensure the following tools are installed:

- AWS CLI
- Terraform
- SSH client

Verify AWS CLI authentication:

```bash
aws sts get-caller-identity
```

Expected output:

```json
{
  "UserId": "xxxxxxxx",
  "Account": "xxxxxxxx",
  "Arn": "arn:aws:iam::xxxxxxxx:user/example"
}
```

---

# Step 1: Generate SSH Key Pair

An SSH key pair is required to securely connect to the EC2 instance.

Generate a new RSA key pair:

```bash
ssh-keygen -t rsa -b 4096 -f my-key
```

During generation:

```
Enter passphrase:
```

Press:

```
Enter
```

for no passphrase.

This creates two files:

```
my-key
```

Private key file.

```
my-key.pub
```

Public key file.

---

## Verify Generated Keys

Check the files:

```bash
ls -l my-key*
```

Example output:

```
-rw-------  my-key
-rw-r--r--  my-key.pub
```

---

## Secure the Private Key

The private key should only be readable by the owner.

Run:

```bash
chmod 400 my-key
```

Verify:

```bash
ls -l my-key
```

Expected:

```
-r-------- my-key
```

---

# Step 2: Create AWS EC2 Key Pair

AWS needs the public key uploaded before it can be attached to an EC2 instance.

Import the public key:

```bash
aws ec2 import-key-pair \
--key-name my-key-pair \
--public-key-material fileb://my-key.pub
```

Explanation:

```
my-key-pair
```

AWS key pair name.

```
my-key.pub
```

Public key generated from SSH.

---

## Verify AWS Key Pair

List available key pairs:

```bash
aws ec2 describe-key-pairs
```

Expected:

```
my-key-pair
```

---

# Step 3: Create Terraform Directory

Create a Terraform project directory:

```bash
mkdir terraform
cd terraform
```

---

# Step 4: Create Terraform Configuration

Create the Terraform file:

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


resource "aws_instance" "example" {

  ami = "ami-0c7217cdde317cfec"

  instance_type = "t2.micro"

  key_name = "my-key-pair"


  tags = {
    Name = "terraform-ec2-instance"
  }
}
```

---

# Terraform Configuration Explanation

## Provider Configuration

```hcl
provider "aws" {
  region = "us-east-1"
}
```

Defines AWS as the cloud provider.

The EC2 instance will be created in:

```
us-east-1
```

---

## EC2 Resource

```hcl
resource "aws_instance" "example"
```

Creates an AWS EC2 instance.

---

## AMI

```hcl
ami = "ami-0c7217cdde317cfec"
```

Defines the operating system image.

---

## Instance Type

```hcl
instance_type = "t2.micro"
```

Creates a small general-purpose EC2 instance.

---

## Key Pair

```hcl
key_name = "my-key-pair"
```

Attaches the AWS key pair created earlier.

The private key:

```
my-key
```

will be used for SSH access.

---

# Step 5: Initialize Terraform

Initialize the Terraform project:

```bash
terraform init
```

Expected:

```
Terraform has been successfully initialized!
```

---

# Step 6: Validate Terraform Configuration

Check for syntax issues:

```bash
terraform validate
```

Expected:

```
Success! The configuration is valid.
```

---

# Step 7: Review Terraform Plan

Before creating resources, review the execution plan:

```bash
terraform plan
```

Expected:

```
Plan: 1 to add, 0 to change, 0 to destroy.
```

Terraform will create:

```
aws_instance.example
```

---

# Step 8: Create EC2 Instance

Deploy the EC2 instance:

```bash
terraform apply
```

Terraform will ask:

```
Do you want to perform these actions?
```

Type:

```
yes
```

Example:

```
Apply complete! Resources: 1 added.
```

---

# Step 9: Verify EC2 Instance

Check Terraform resources:

```bash
terraform show
```

Using AWS CLI:

```bash
aws ec2 describe-instances
```

Verify:

- Instance ID
- Running state
- Public IP address
- Attached key pair

---

# Step 10: Connect to EC2 Using SSH

Use the private key:

```bash
ssh -i my-key ec2-user@<PUBLIC-IP>
```

Example:

```bash
ssh -i my-key ec2-user@54.123.45.67
```

If Ubuntu is used:

```bash
ssh -i my-key ubuntu@<PUBLIC-IP>
```

---

# Step 11: Verify Key Authentication

Inside the EC2 instance:

```bash
whoami
```

Check system information:

```bash
uname -a
```

---

# Step 12: Destroy Terraform Resources

To delete the EC2 instance:

```bash
terraform destroy
```

Confirm:

```
yes
```

Terraform removes:

```
aws_instance.example
```

---

# Complete Workflow Summary

## Generate SSH Key

```bash
ssh-keygen -t rsa -b 4096 -f my-key
```

---

## Secure Private Key

```bash
chmod 400 my-key
```

---

## Upload Public Key to AWS

```bash
aws ec2 import-key-pair \
--key-name my-key-pair \
--public-key-material fileb://my-key.pub
```

---

## Terraform Deployment

```bash
terraform init

terraform validate

terraform plan

terraform apply
```

---

## SSH Access

```bash
ssh -i my-key ec2-user@<public-ip>
```

---

## Cleanup

```bash
terraform destroy
```

---

# Key Takeaways

- SSH key pairs provide secure passwordless authentication.
- The private key remains on the local machine.
- The public key is uploaded to AWS.
- Terraform uses `key_name` to attach the key pair to EC2.
- Terraform manages the EC2 lifecycle.
- Only the EC2 instance is created; no networking or security resources are configured.
