# EC2 Instance Setup - xfusion-ec2

## Overview

This document describes the steps performed to create an AWS EC2 instance named `xfusion-ec2` and configure secure passwordless SSH access from the `aws-client` host using an SSH key.

## Requirements

- AWS Account
- AWS CLI configured on `aws-client`
- Region: `us-east-1`
- EC2 instance type: `t2.micro`
- SSH key name: `id_rsa`

---

## 1. Verify AWS Credentials

AWS credentials were validated from the `aws-client` machine.

Command:

```bash
aws sts get-caller-identity
```

Example output:

```json
{
  "UserId": "AIDATSC5BTUTENU6LSJIG",
  "Account": "245008276774",
  "Arn": "arn:aws:iam::245008276774:user/kk_labs_user_219114"
}
```

---

## 2. Generate SSH Key on aws-client

Create an RSA key pair under `/root/.ssh/`.

Command:

```bash
ssh-keygen
```

Generated files:

```
/root/.ssh/id_rsa
/root/.ssh/id_rsa.pub
```

Verify:

```bash
ls -l /root/.ssh/id_rsa*
```

---

## 3. Import SSH Public Key into AWS EC2

The public key was registered as an EC2 Key Pair.

Command:

```bash
aws ec2 import-key-pair \
 --key-name id_rsa \
 --public-key-material fileb:///root/.ssh/id_rsa.pub \
 --region us-east-1
```

Verify:

```bash
aws ec2 describe-key-pairs \
 --key-names id_rsa \
 --region us-east-1
```

Example:

```
KeyName: id_rsa
KeyType: rsa
KeyPairId: key-0267d917e802d8c0c
```

---

## 4. Select Amazon Linux AMI

The latest Amazon Linux AMI was retrieved:

Command:

```bash
aws ec2 describe-images \
 --owners amazon \
 --filters "Name=name,Values=al2023-ami-*" \
 --query 'Images | sort_by(@,&CreationDate)[-1].[ImageId,Name]' \
 --output table \
 --region us-east-1
```

Example:

```
AMI ID:
ami-0f471498ff04f1a83
```

---

## 5. Create EC2 Instance

The EC2 instance was created with:

| Parameter     | Value                 |
| ------------- | --------------------- |
| Instance Name | xfusion-ec2           |
| Instance Type | t2.micro              |
| Region        | us-east-1             |
| Key Pair      | id_rsa                |
| AMI           | ami-0f471498ff04f1a83 |

Command:

```bash
aws ec2 run-instances \
 --image-id ami-0f471498ff04f1a83 \
 --instance-type t2.micro \
 --key-name id_rsa \
 --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=xfusion-ec2}]' \
 --region us-east-1
```

---

## 6. Verify Instance Status

Command:

```bash
aws ec2 describe-instances \
 --filters "Name=tag:Name,Values=xfusion-ec2" \
 --query "Reservations[*].Instances[*].[InstanceId,PublicIpAddress,State.Name]" \
 --output table \
 --region us-east-1
```

Example:

```
Instance ID              Public IP        Status
---------------------------------------------------
i-051018c1708041eda      100.59.14.212    running
```

---

## 7. Configure Security Group for SSH

List all the security groups

```
aws ec2 describe-security-groups \
 --region us-east-1 \
 --query "SecurityGroups[*].[GroupId,GroupName,VpcId]" \
 --output table
```

SSH access requires inbound TCP port 22.

Security group:

```
sg-0e4e534934005f2d0
```

Allow SSH:

```bash
aws ec2 authorize-security-group-ingress \
 --group-id sg-0e4e534934005f2d0 \
 --protocol tcp \
 --port 22 \
 --cidr 0.0.0.0/0 \
 --region us-east-1
```

---

## 8. Connect to EC2 Instance

SSH command:

```bash
ssh -i /root/.ssh/id_rsa ec2-user@<PUBLIC_IP>
```

Example:

```bash
ssh -i /root/.ssh/id_rsa ec2-user@100.59.14.212
```

---

## 9. Configure Root User SSH Access

After connecting to the instance:

Create root SSH directory:

```bash
sudo mkdir -p /root/.ssh
sudo chmod 700 /root/.ssh
```

Copy authorized keys:

```bash
sudo cp /home/ec2-user/.ssh/authorized_keys \
/root/.ssh/authorized_keys
```

Set permissions:

```bash
sudo chown -R root:root /root/.ssh
sudo chmod 600 /root/.ssh/authorized_keys
```

---

## 10. Enable Root SSH Login

Edit SSH configuration:

```bash
sudo vi /etc/ssh/sshd_config
```

Enable:

```
PermitRootLogin yes
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

## 11. Validate Passwordless Root Login

From `aws-client`:

```bash
ssh root@<PUBLIC_IP>
```

Successful login confirms:

- SSH key authentication works
- Root user can authenticate
- Passwordless SSH from `aws-client` is configured

---

## Final Configuration Summary

| Resource          | Value               |
| ----------------- | ------------------- |
| AWS Region        | us-east-1           |
| EC2 Instance Name | xfusion-ec2         |
| Instance Type     | t2.micro            |
| Instance ID       | i-051018c1708041eda |
| Key Pair          | id_rsa              |
| SSH Source Host   | aws-client          |
| SSH User          | root                |
| Authentication    | SSH Public Key      |

## Notes

- IAM access key creation was not required for this task.
- The IAM error for `iam:CreateAccessKey` can be ignored.
- Only EC2 permissions and SSH key configuration are required.
