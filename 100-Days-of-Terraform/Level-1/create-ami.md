# Create an AMI from an Existing EC2 Instance Using Terraform

## Objective

As part of the AWS migration effort, the Nautilus DevOps team needs to create an Amazon Machine Image (AMI) from an existing EC2 instance using Terraform.

The AMI will serve as a reusable template for launching future instances with the same configuration as the source instance.

---

## Requirements

1. Use Terraform to create an AMI from the existing EC2 instance named **devops-ec2**.
2. Name the AMI **devops-ec2-ami**.
3. Ensure the AMI reaches the **available** state.
4. Update the existing `main.tf` file located in:

```text
/home/bob/terraform
```

5. Do **not** create any additional `.tf` files.

---

## Prerequisites

Before proceeding, ensure:

- Terraform is installed.
- AWS credentials are configured.
- An EC2 instance named **devops-ec2** already exists.
- Access to the Terraform working directory:

```bash
cd /home/bob/terraform
```

---

## Step 1: Open the Terraform Working Directory

Navigate to the Terraform project directory:

```bash
cd /home/bob/terraform
```

Open the existing `main.tf` file and update it.

---

## Step 2: Create the AMI Resource

Add the following Terraform configuration to `main.tf`:

```hcl
resource "aws_ami_from_instance" "devops_ec2_ami" {
  name               = "devops-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

### Explanation

| Parameter            | Description                   |
| -------------------- | ----------------------------- |
| `name`               | Name of the AMI to be created |
| `source_instance_id` | ID of the source EC2 instance |

The dependency is automatically managed because the AMI references the EC2 instance ID.

---

## Example Configuration

If the EC2 instance is already managed by Terraform, the complete configuration may look like:

```hcl
resource "aws_instance" "ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  vpc_security_group_ids = ["sg-d39d1fb2eb08ed1dd"]

  tags = {
    Name = "devops-ec2"
  }
}

resource "aws_ami_from_instance" "devops_ec2_ami" {
  name               = "devops-ec2-ami"
  source_instance_id = aws_instance.ec2.id
}
```

---

## Step 3: Initialize Terraform

Initialize the Terraform working directory:

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

## Step 4: Review the Execution Plan

Generate and review the execution plan:

```bash
terraform plan
```

Verify that Terraform plans to create:

```text
aws_ami_from_instance.devops_ec2_ami
```

---

## Step 5: Apply the Configuration

Create the AMI:

```bash
terraform apply
```

Confirm when prompted:

```text
Enter a value: yes
```

Terraform will begin creating the AMI.

---

## Step 6: Verify the AMI State

Check that the AMI has reached the **available** state.

### Using AWS CLI

List the AMI by name:

```bash
aws ec2 describe-images \
  --owners self \
  --filters "Name=name,Values=devops-ec2-ami" \
  --query 'Images[*].[ImageId,Name,State]' \
  --output table
```

Expected output:

```text
-----------------------------------------
|            DescribeImages             |
+----------------------+---------------+
| ami-xxxxxxxxxxxxxxx  | available     |
+----------------------+---------------+
```

### Alternative Check

Retrieve only the state:

```bash
aws ec2 describe-images \
  --owners self \
  --filters "Name=name,Values=devops-ec2-ami" \
  --query 'Images[*].State' \
  --output text
```

Expected result:

```text
available
```

---

## Verify Terraform State

Confirm that Terraform is managing the AMI resource:

```bash
terraform state list
```

Expected output:

```text
aws_ami_from_instance.devops_ec2_ami
```

Inspect the resource:

```bash
terraform state show aws_ami_from_instance.devops_ec2_ami
```

Look for:

```text
state = available
```

---

## Common Issues

### AMI Remains in Pending State

Creating an AMI can take several minutes depending on the size of the source instance and attached volumes.

Verify periodically:

```bash
aws ec2 describe-images \
  --owners self \
  --filters "Name=name,Values=devops-ec2-ami" \
  --query 'Images[*].State'
```

---

### Source Instance Not Found

Error:

```text
InvalidInstanceID.NotFound
```

Verify that the EC2 instance exists and that the correct instance ID is being referenced.

---

### AWS Credentials Not Configured

Error:

```text
No valid credential sources found
```

Configure AWS credentials before running Terraform.

---

## Validation Checklist

- [x] Updated `/home/bob/terraform/main.tf`
- [x] Created AMI resource using Terraform
- [x] AMI name is `devops-ec2-ami`
- [x] Terraform applied successfully
- [x] AMI state verified as `available`

---

## Conclusion

The existing EC2 instance **devops-ec2** has been successfully used to create an AMI named **devops-ec2-ami** using Terraform. After applying the configuration and verifying the image state, the AMI is available for launching new EC2 instances and supporting future migration activities.
