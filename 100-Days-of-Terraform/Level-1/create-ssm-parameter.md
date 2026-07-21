# AWS SSM Parameter Creation Using Terraform

## Objective

Create an AWS Systems Manager (SSM) Parameter Store parameter using Terraform.

### Requirements

- Parameter Name: `datacenter-ssm-parameter`
- Parameter Type: `String`
- Parameter Value: `datacenter-value`
- Terraform working directory: `/home/bob/terraform`

---

## Terraform Configuration

Create a file named `main.tf` with the following content:

```hcl
resource "aws_ssm_parameter" "datacenter-ssm-parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

> **Note:** For KodeKloud labs, keep the resource name exactly as shown if it already exists in the Terraform state. Renaming the resource can cause Terraform to destroy and recreate it, leading to `ParameterAlreadyExists` errors.

---

## Initialize Terraform

```bash
cd /home/bob/terraform

terraform init
```

---

## Validate the Configuration

```bash
terraform plan
```

Expected result:

```text
No changes. Your infrastructure matches the configuration.
```

or, if the parameter does not yet exist:

```text
Plan: 1 to add, 0 to change, 0 to destroy.
```

---

## Apply the Configuration

```bash
terraform apply
```

Type:

```text
yes
```

when prompted.

---

## Verify the Parameter

```bash
aws ssm get-parameter \
  --name datacenter-ssm-parameter \
  --region us-east-1
```

Expected output:

```json
{
  "Parameter": {
    "Name": "datacenter-ssm-parameter",
    "Type": "String",
    "Value": "datacenter-value"
  }
}
```

---

## Troubleshooting

### ParameterAlreadyExists Error

If you receive:

```text
ParameterAlreadyExists
```

it usually means the resource name in Terraform was changed, causing Terraform to attempt creating a new resource instead of managing the existing one.

Ensure the resource block matches the existing Terraform state:

```hcl
resource "aws_ssm_parameter" "datacenter-ssm-parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

Then rerun:

```bash
terraform plan
```

and confirm there are no pending changes.

---

## Validation Checklist

- SSM parameter name is `datacenter-ssm-parameter`
- Parameter type is `String`
- Parameter value is `datacenter-value`
- Terraform configuration is stored in `main.tf`
- `terraform plan` shows no pending changes
- Parameter exists in AWS Systems Manager Parameter Store

```

```
