# AWS CloudFormation S3 Bucket and EC2 Instance Lab

## Overview

This lab demonstrates how to use AWS CloudFormation to provision infrastructure as code. The template creates:

- An Amazon S3 bucket
- An Amazon EC2 instance
- CloudFormation outputs for resource information

The stack was initially created and later updated using the `update-stack` command as the template evolved.

---

# Objective

Create and manage AWS resources using CloudFormation:

| Resource               | Purpose                                       |
| ---------------------- | --------------------------------------------- |
| S3 Bucket              | Object storage                                |
| EC2 Instance           | Compute resource                              |
| CloudFormation Outputs | Display resource information after deployment |

---

# Template Location

```text
/root/s3-and-ec2.yaml
```

---

# CloudFormation Template

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: CloudFormation template to create an S3 bucket

Parameters:
  InstanceName:
    Type: String
    Default: InstanceName

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "${AWS::StackName}-aws-dev-associate-kodekloud"
      AccessControl: Private

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-08a0d1e16fc3f61ea
      Tags:
        - Key: Name
          Value: !Ref InstanceName

Outputs:
  S3BucketName:
    Description: The name of the S3 bucket
    Value: !Ref MyS3Bucket

  EC2PublicIP:
    Description: Public IP of the EC2 instance
    Value: !GetAtt MyEC2Instance.PublicIp
```

---

# Validation

Validate the template before deployment:

```bash
aws cloudformation validate-template \
  --template-body file:///root/s3-and-ec2.yaml
```

Example output:

```json
{
  "Parameters": [],
  "Description": "CloudFormation template to create an S3 bucket"
}
```

---

# Initial Stack Creation

Create the CloudFormation stack:

```bash
aws cloudformation create-stack \
  --stack-name s3-stack \
  --template-body file:///root/s3-and-ec2.yaml
```

Example output:

```json
{
  "StackId": "arn:aws:cloudformation:us-east-1:637423448740:stack/s3-stack/..."
}
```

---

# Updating an Existing Stack

After modifying the template to include:

- EC2 instance resource
- CloudFormation outputs
- Additional parameters

the stack already existed, so CloudFormation returned:

```text
An error occurred (AlreadyExistsException) when calling the CreateStack operation:
Stack [s3-stack] already exists
```

Instead of recreating the stack, use:

```bash
aws cloudformation update-stack \
  --stack-name s3-stack \
  --template-body file:///root/s3-and-ec2.yaml
```

Example output:

```json
{
  "StackId": "arn:aws:cloudformation:us-east-1:637423448740:stack/s3-stack/..."
}
```

This updates the existing stack with the new resources and outputs.

---

# Checking Stack Status

Monitor stack progress:

```bash
aws cloudformation describe-stacks \
  --stack-name s3-stack
```

Or:

```bash
aws cloudformation describe-stack-events \
  --stack-name s3-stack
```

---

# Viewing Stack Outputs

Retrieve configured outputs:

```bash
aws cloudformation describe-stacks \
  --stack-name s3-stack \
  --query "Stacks[0].Outputs"
```

Example output:

```json
[
  {
    "OutputKey": "S3BucketName",
    "OutputValue": "s3-stack-aws-dev-associate-kodekloud"
  },
  {
    "OutputKey": "EC2PublicIP",
    "OutputValue": "54.xxx.xxx.xxx"
  }
]
```

---

# Verification

## Verify S3 Bucket

```bash
aws s3 ls
```

Expected:

```text
s3-stack-aws-dev-associate-kodekloud
```

---

## Verify EC2 Instance

```bash
aws ec2 describe-instances
```

Or retrieve the public IP from stack outputs:

```bash
aws cloudformation describe-stacks \
  --stack-name s3-stack \
  --query "Stacks[0].Outputs"
```

---

# Troubleshooting

## Stack Already Exists

Error:

```text
AlreadyExistsException
```

Solution:

```bash
aws cloudformation update-stack \
  --stack-name s3-stack \
  --template-body file:///root/s3-and-ec2.yaml
```

---

## Template Validation Errors

Validate before deployment:

```bash
aws cloudformation validate-template \
  --template-body file:///root/s3-and-ec2.yaml
```

---

## Bucket Name Conflicts

S3 bucket names must be globally unique.

Using:

```yaml
BucketName: !Sub "${AWS::StackName}-aws-dev-associate-kodekloud"
```

helps ensure uniqueness by incorporating the stack name.

---

# Commands Used

```bash
# Validate template
aws cloudformation validate-template \
  --template-body file:///root/s3-and-ec2.yaml

# Create stack
aws cloudformation create-stack \
  --stack-name s3-stack \
  --template-body file:///root/s3-and-ec2.yaml

# Update existing stack
aws cloudformation update-stack \
  --stack-name s3-stack \
  --template-body file:///root/s3-and-ec2.yaml

# View outputs
aws cloudformation describe-stacks \
  --stack-name s3-stack \
  --query "Stacks[0].Outputs"
```

---

# Result

Successfully created and managed AWS infrastructure using CloudFormation. The stack provisions:

- A private S3 bucket named using the stack name and required suffix.
- A t2.micro EC2 instance.
- Stack outputs exposing the bucket name and EC2 public IP.

The stack lifecycle was managed through both `create-stack` and `update-stack` operations, demonstrating how CloudFormation handles infrastructure updates without recreating existing resources.
