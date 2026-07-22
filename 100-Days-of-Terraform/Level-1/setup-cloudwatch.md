# AWS CloudWatch Log Group and Log Stream Provisioning with Terraform

## Objective

Provision AWS CloudWatch logging resources using Terraform:

- Create a CloudWatch Log Group named `datacenter-log-group`
- Create a CloudWatch Log Stream named `datacenter-log-stream`
- Manage the resources through Terraform in the `/home/bob/terraform` working directory

---

# Terraform Configuration

Create the file:

```text
/home/bob/terraform/main.tf
```

Add the following configuration:

```hcl
resource "aws_cloudwatch_log_group" "datacenter_log_group" {
  name = "datacenter-log-group"

  tags = {
    Name = "datacenter-log-group"
  }
}

resource "aws_cloudwatch_log_stream" "datacenter_log_stream" {
  name           = "datacenter-log-stream"
  log_group_name = aws_cloudwatch_log_group.datacenter_log_group.name
}
```

---

# Initialize Terraform

Navigate to the Terraform directory:

```bash
cd /home/bob/terraform
```

Initialize Terraform:

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

# Validate the Configuration

Review the execution plan:

```bash
terraform plan
```

Expected resources:

```text
+ aws_cloudwatch_log_group.datacenter_log_group
+ aws_cloudwatch_log_stream.datacenter_log_stream
```

---

# Apply the Configuration

Create the resources:

```bash
terraform apply
```

Confirm when prompted:

```text
Enter a value: yes
```

Expected result:

```text
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

# Verify the Resources

List CloudWatch Log Groups:

```bash
aws logs describe-log-groups \
  --log-group-name-prefix datacenter-log-group
```

List Log Streams:

```bash
aws logs describe-log-streams \
  --log-group-name datacenter-log-group
```

Expected output:

```text
Log Group:
datacenter-log-group

Log Stream:
datacenter-log-stream
```

---

# Verify Terraform State

Check managed resources:

```bash
terraform state list
```

Expected output:

```text
aws_cloudwatch_log_group.datacenter_log_group
aws_cloudwatch_log_stream.datacenter_log_stream
```

---

# Validation Checklist

- `main.tf` created in `/home/bob/terraform`
- CloudWatch Log Group `datacenter-log-group` created
- CloudWatch Log Stream `datacenter-log-stream` created
- Terraform apply completed successfully
- Resources visible in AWS CloudWatch
- Terraform state updated correctly

---

# Useful Terraform Commands

Initialize project:

```bash
terraform init
```

Review planned changes:

```bash
terraform plan
```

Apply configuration:

```bash
terraform apply
```

Check current state:

```bash
terraform state list
```

Destroy resources (optional):

```bash
terraform destroy
```

---

# Outcome

A CloudWatch Log Group named `datacenter-log-group` and a Log Stream named `datacenter-log-stream` have been successfully provisioned and are managed through Terraform.
