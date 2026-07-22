# AWS EC2 Instance Deployment with Elastic IP

## Objective

Deploy an Amazon EC2 instance named `xfusion-ec2` using a Linux AMI and associate an Elastic IP address named `xfusion-eip` to provide a stable public IP for application hosting.

---

# Prerequisites

- AWS account access
- Appropriate permissions to create EC2 instances and Elastic IPs
- AWS Region: **us-east-1 (N. Virginia)**

---

# Step 1: Launch the EC2 Instance

## Open the EC2 Console

1. Log in to the AWS Management Console.
2. Navigate to **EC2**.
3. Click **Launch Instance**.

---

## Configure Instance Details

### Name and Tags

| Field | Value         |
| ----- | ------------- |
| Name  | `xfusion-ec2` |

### Application and OS Image (AMI)

Select any Linux-based AMI, such as:

- Ubuntu Server 22.04 LTS
- Ubuntu Server 20.04 LTS

### Instance Type

| Field         | Value      |
| ------------- | ---------- |
| Instance Type | `t2.micro` |

### Key Pair

Select an existing key pair or create a new one for SSH access.

### Network Settings

- Use the default VPC.
- Use the default security group.
- Enable **Auto-assign Public IP**.

### Launch Instance

Click **Launch Instance** and wait until:

- Instance State = **Running**
- Status Checks = **2/2 Passed**

---

# Step 2: Allocate an Elastic IP

## Navigate to Elastic IPs

1. In the EC2 Console, go to:

```text
Network & Security → Elastic IPs
```

2. Click **Allocate Elastic IP Address**.

3. Leave the default options unchanged.

4. Click **Allocate**.

---

# Step 3: Tag the Elastic IP

1. Select the newly allocated Elastic IP.
2. Click **Tags → Manage Tags**.
3. Add the following tag:

| Key  | Value         |
| ---- | ------------- |
| Name | `xfusion-eip` |

4. Save the changes.

---

# Step 4: Associate the Elastic IP

1. Select the Elastic IP (`xfusion-eip`).
2. Click:

```text
Actions → Associate Elastic IP Address
```

3. Configure:

| Field              | Value         |
| ------------------ | ------------- |
| Resource Type      | Instance      |
| Instance           | `xfusion-ec2` |
| Private IP Address | Default       |

4. Click **Associate**.

---

# Step 5: Verify the Configuration

## Verify EC2 Instance

Navigate to:

```text
EC2 → Instances
```

Confirm:

| Property      | Expected Value     |
| ------------- | ------------------ |
| Name          | `xfusion-ec2`      |
| State         | Running            |
| Instance Type | t2.micro           |
| Public IPv4   | Elastic IP Address |

---

## Verify Elastic IP

Navigate to:

```text
EC2 → Elastic IPs
```

Confirm:

| Property            | Expected Value |
| ------------------- | -------------- |
| Name                | `xfusion-eip`  |
| Status              | Associated     |
| Associated Resource | `xfusion-ec2`  |

---

# Validation Checklist

- EC2 instance `xfusion-ec2` created successfully.
- Linux AMI used for deployment.
- Instance type set to `t2.micro`.
- Elastic IP allocated.
- Elastic IP tagged as `xfusion-eip`.
- Elastic IP associated with `xfusion-ec2`.
- Instance is running and reachable using the Elastic IP.

---

# Architecture Overview

```text
                Internet
                    │
                    ▼
          Elastic IP (xfusion-eip)
                    │
                    ▼
           EC2 Instance (xfusion-ec2)
                    │
                    ▼
             Application Hosting
```

---

# Outcome

A Linux EC2 instance named `xfusion-ec2` is deployed and associated with a static Elastic IP address named `xfusion-eip`, providing a stable public endpoint for application hosting.
