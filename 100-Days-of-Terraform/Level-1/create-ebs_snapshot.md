# AWS EBS Volume and Snapshot Using Terraform

## Objective

Create an AWS EBS volume named `nautilus-vol` and then create a snapshot named `nautilus-vol-ss` using Terraform.

### Requirements

- Region: `us-east-1`
- Volume Name: `nautilus-vol`
- Volume Type: `gp2`
- Volume Size: `5 GiB`
- Availability Zone: `us-east-1a`
- Snapshot Name: `nautilus-vol-ss`
- Snapshot Description: `Nautilus Snapshot`

---

## Terraform Configuration

Create or update the `main.tf` file with the following content:

```hcl
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name = "nautilus-vol"
  }
}

resource "aws_ebs_snapshot" "nautilus_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Nautilus Snapshot"

  tags = {
    Name = "nautilus-vol-ss"
  }
}
```

---

## Deployment Steps

### 1. Navigate to the Terraform Directory

```bash
cd /home/bob/terraform
```

### 2. Initialize Terraform

```bash
terraform init
```

Expected output:

```text
Terraform has been successfully initialized!
```

---

### 3. Review the Execution Plan

```bash
terraform plan
```

Terraform should display:

```text
Plan: 2 to add, 0 to change, 0 to destroy.
```

Resources to be created:

- `aws_ebs_volume.k8s_volume`
- `aws_ebs_snapshot.nautilus_snapshot`

---

### 4. Apply the Configuration

```bash
terraform apply
```

When prompted:

```text
Enter a value:
```

Type:

```text
yes
```

Terraform creates the EBS volume first and then creates the snapshot from that volume.

Example:

```text
aws_ebs_volume.k8s_volume: Creation complete
aws_ebs_snapshot.nautilus_snapshot: Creation complete

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## Verify Terraform Resources

Check Terraform state:

```bash
terraform state list
```

Expected output:

```text
aws_ebs_volume.k8s_volume
aws_ebs_snapshot.nautilus_snapshot
```

---

## Verify the Volume

```bash
aws ec2 describe-volumes \
  --filters Name=tag:Name,Values=nautilus-vol \
  --region us-east-1
```

---

## Verify the Snapshot

```bash
aws ec2 describe-snapshots \
  --filters Name=tag:Name,Values=nautilus-vol-ss \
  --region us-east-1
```

---

## Verify Snapshot Completion

```bash
aws ec2 describe-snapshots \
  --filters Name=tag:Name,Values=nautilus-vol-ss \
  --query "Snapshots[*].State" \
  --output text \
  --region us-east-1
```

Expected output:

```text
completed
```

---

## Cleanup (Optional)

To remove the resources managed by Terraform:

```bash
terraform destroy
```

Confirm with:

```text
yes
```

Terraform will delete:

- EBS Snapshot (`nautilus-vol-ss`)
- EBS Volume (`nautilus-vol`)

---

## Summary

This Terraform configuration:

- Creates an EBS volume named `nautilus-vol`
- Uses the created volume to generate a snapshot
- Assigns the snapshot name `nautilus-vol-ss`
- Sets the description to `Nautilus Snapshot`
- Tracks both resources in Terraform state
- Allows verification that the snapshot reaches the `completed` state before task submission
