# README - Create AWS Kinesis Data Stream Using Terraform

## Overview

This guide demonstrates how to create an AWS Kinesis Data Stream named **`devops-stream`** using Terraform. The stream is configured in **On-Demand** mode, allowing AWS to automatically scale capacity based on incoming traffic.

---

## Requirements

- AWS account with appropriate permissions
- Terraform installed
- Working directory: `/home/bob/terraform`

---

## Terraform Configuration

Create a file named `main.tf` in `/home/bob/terraform`:

```hcl
resource "aws_kinesis_stream" "devops_stream" {
  name = "devops-stream"

  stream_mode_details {
    stream_mode = "ON_DEMAND"
  }

  tags = {
    Name = "devops-stream"
  }
}
```

---

## Deployment Steps

### Navigate to the Terraform Directory

```bash
cd /home/bob/terraform
```

### Initialize Terraform

```bash
terraform init
```

### Review the Execution Plan

```bash
terraform plan
```

Expected output should indicate that the Kinesis stream will be created.

### Apply the Configuration

```bash
terraform apply
```

Type `yes` when prompted.

---

## Verification

Confirm the stream was created successfully:

```bash
terraform state list
```

Expected output:

```text
aws_kinesis_stream.devops_stream
```

You can also verify from AWS:

```bash
aws kinesis describe-stream-summary \
  --stream-name devops-stream \
  --region us-east-1
```

---

## Final Validation

Before submitting the task, ensure Terraform reports no pending changes:

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

---

## Resource Summary

| Resource Type       | Name                               |
| ------------------- | ---------------------------------- |
| Kinesis Data Stream | `devops-stream`                    |
| Stream Mode         | `ON_DEMAND`                        |
| Terraform Resource  | `aws_kinesis_stream.devops_stream` |

---

## Cleanup (Optional)

To remove the stream:

```bash
terraform destroy
```

Type `yes` when prompted.
