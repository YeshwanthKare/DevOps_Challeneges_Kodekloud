# Create an AWS EBS Volume Using Terraform

## Lab Objective

Create an AWS EBS volume using Terraform with the following requirements:

- Volume Name: `nautilus-volume`
- Volume Type: `gp3`
- Volume Size: `2 GiB`
- Region: `us-east-1`
- Terraform working directory: `/home/bob/terraform`

---

## Step 1: Navigate to the Terraform Directory

```bash
cd /home/bob/terraform
```

---

## Step 2: Create the `main.tf` File

Create a file named `main.tf` and add the following configuration:

```hcl
# Get the default VPC in us-east-1
data "aws_vpc" "default" {
  default = true
}

# Get available Availability Zones
data "aws_availability_zones" "available" {
  state = "available"
}

# Create EBS Volume
resource "aws_ebs_volume" "nautilus_volume" {
  availability_zone = data.aws_availability_zones.available.names[0]
  size              = 2
  type              = "gp3"

  tags = {
    Name = "nautilus-volume"
  }
}
```

---

## Step 3: Initialize Terraform

Initialize the Terraform working directory and download the required provider plugins.

```bash
terraform init
```

Example output:

```bash
Initializing the backend...
Initializing provider plugins...

- Finding hashicorp/aws versions matching "5.91.0"...
- Installing hashicorp/aws v5.91.0...
- Installed hashicorp/aws v5.91.0 (signed by HashiCorp)

Terraform has been successfully initialized!
```

---

## Step 4: Review the Execution Plan

Verify the resources Terraform will create.

```bash
terraform plan
```

Expected plan summary:

```bash
Plan: 1 to add, 0 to change, 0 to destroy.
```

Terraform should show creation of:

```bash
aws_ebs_volume.nautilus_volume
```

With:

```bash
availability_zone = "us-east-1a"
size              = 2
type              = "gp3"

tags = {
  Name = "nautilus-volume"
}
```

---

## Step 5: Apply the Configuration

Create the EBS volume.

```bash
terraform apply
```

When prompted:

```bash
Enter a value:
```

Type:

```bash
yes
```

---

## Step 6: Verify Successful Creation

Example output:

```bash
aws_ebs_volume.nautilus_volume: Creating...
aws_ebs_volume.nautilus_volume: Creation complete after 10s [id=vol-xxxxxxxxxxxxxxxxx]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

---

## Resource Details

| Property          | Value                           |
| ----------------- | ------------------------------- |
| Name              | nautilus-volume                 |
| Type              | gp3                             |
| Size              | 2 GiB                           |
| Region            | us-east-1                       |
| Availability Zone | First available AZ in us-east-1 |
| Resource Type     | aws_ebs_volume                  |

---

## Useful Terraform Commands

### Initialize Terraform

```bash
terraform init
```

### Review Changes

```bash
terraform plan
```

### Create Resources

```bash
terraform apply
```

### Destroy Resources

```bash
terraform destroy
```

---

## Summary

This Terraform configuration creates:

- One AWS EBS Volume
- Volume Name: `nautilus-volume`
- Volume Type: `gp3`
- Volume Size: `2 GiB`
- Created in the `us-east-1` region
- Tagged for easy identification in AWS

The volume is provisioned dynamically in the first available Availability Zone returned by AWS.
