# AWS CloudWatch Alarm Configuration with Terraform

## Overview

This project demonstrates how to use Terraform to provision AWS infrastructure and configure monitoring through Amazon CloudWatch.

The objective is to create:

- An EC2 instance for monitoring
- A CloudWatch alarm named **xfusion-alarm**
- CPU utilization monitoring for the EC2 instance
- Alarm triggering when CPU usage exceeds **80%**
- A monitoring period of **5 minutes (300 seconds)**
- A single evaluation period

All resources are managed through Terraform within the working directory:

```bash
/home/bob/terraform
```

---

## Requirements

### CloudWatch Alarm Specifications

| Parameter           | Value                   |
| ------------------- | ----------------------- |
| Alarm Name          | xfusion-alarm           |
| Metric              | CPUUtilization          |
| Namespace           | AWS/EC2                 |
| Threshold           | 80                      |
| Comparison Operator | GreaterThanThreshold    |
| Evaluation Periods  | 1                       |
| Period              | 300 Seconds (5 Minutes) |
| Statistic           | Average                 |

---

## Terraform Configuration

### main.tf

```hcl
# Create EC2 instance for monitoring
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  tags = {
    Name = "datacenter-ec2"
  }
}

# Create CloudWatch alarm
resource "aws_cloudwatch_metric_alarm" "xfusion_alarm" {
  alarm_name          = "xfusion-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1

  metric_name = "CPUUtilization"
  namespace   = "AWS/EC2"
  statistic   = "Average"

  period    = 300
  threshold = 80

  alarm_description = "Alarm when CPU utilization exceeds 80%"

  dimensions = {
    InstanceId = aws_instance.datacenter_ec2.id
  }
}
```

---

## Deployment Steps

### 1. Navigate to Terraform Directory

```bash
cd /home/bob/terraform
```

### 2. Initialize Terraform

```bash
terraform init
```

This downloads and installs the required AWS provider plugins.

### 3. Validate the Configuration

```bash
terraform plan
```

Terraform will display the execution plan showing:

- 1 EC2 instance to be created
- 1 CloudWatch alarm to be created

### 4. Apply the Configuration

```bash
terraform apply
```

Approve the deployment when prompted:

```text
Enter a value: yes
```

---

## Resources Created

### EC2 Instance

| Property      | Value                 |
| ------------- | --------------------- |
| Name          | datacenter-ec2        |
| Instance Type | t2.micro              |
| AMI           | ami-0c101f26f147fa7fd |

### CloudWatch Alarm

| Property           | Value          |
| ------------------ | -------------- |
| Alarm Name         | xfusion-alarm  |
| Metric             | CPUUtilization |
| Threshold          | 80%            |
| Period             | 300 Seconds    |
| Evaluation Periods | 1              |
| Statistic          | Average        |

---

## Verification

Verify that the resources were created successfully:

```bash
terraform state list
```

Expected output:

```text
aws_instance.datacenter_ec2
aws_cloudwatch_metric_alarm.xfusion_alarm
```

You can also verify the alarm from the AWS Console:

1. Open CloudWatch
2. Navigate to Alarms
3. Locate **xfusion-alarm**
4. Confirm it monitors CPU utilization for the created EC2 instance

---

## Expected Result

After successful deployment:

- An EC2 instance named **datacenter-ec2** is running.
- A CloudWatch alarm named **xfusion-alarm** exists.
- The alarm monitors EC2 CPU utilization.
- The alarm enters the ALARM state when average CPU usage exceeds **80%** during a **5-minute** evaluation period.

---

## Cleanup

To remove all created resources:

```bash
terraform destroy
```

Confirm with:

```text
yes
```

Terraform will delete both the EC2 instance and the CloudWatch alarm.
