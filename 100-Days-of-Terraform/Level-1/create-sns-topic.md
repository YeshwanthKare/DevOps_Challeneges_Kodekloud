# README - Create AWS SNS Topic Using Terraform

## Objective

Create an Amazon SNS Topic named `nautilus-notifications` using Terraform.

---

## Terraform Configuration

Create a file named `main.tf` in the Terraform working directory:

```hcl
resource "aws_sns_topic" "nautilus_notifications" {
  name = "nautilus-notifications"
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

### Validate the Configuration

```bash
terraform validate
```

### Review the Execution Plan

```bash
terraform plan
```

Expected output:

```text
# aws_sns_topic.nautilus_notifications will be created
+ resource "aws_sns_topic" "nautilus_notifications" {
    name = "nautilus-notifications"
  }
```

### Apply the Configuration

```bash
terraform apply
```

Type:

```text
yes
```

when prompted.

---

## Verify the SNS Topic

List SNS topics:

```bash
aws sns list-topics
```

Example output:

```text
{
  "Topics": [
    {
      "TopicArn": "arn:aws:sns:us-east-1:123456789012:nautilus-notifications"
    }
  ]
}
```

---

## Verify Terraform State

Confirm Terraform is managing the resource:

```bash
terraform state list
```

Expected output:

```text
aws_sns_topic.nautilus_notifications
```

---

## Confirm No Pending Changes

Run:

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

---

## Cleanup (Optional)

To remove the SNS topic:

```bash
terraform destroy
```

Confirm by typing:

```text
yes
```

---

## Resource Summary

| Resource Type | Name                   |
| ------------- | ---------------------- |
| SNS Topic     | nautilus-notifications |
