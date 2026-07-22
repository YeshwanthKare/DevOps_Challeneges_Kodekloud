# Azure VM Deployment with Static Public IP

## Objective

Deploy an Azure Virtual Machine named `datacenter-vm` with a static public IP address named `datacenter-pip`, configure SSH key-based authentication, and verify SSH connectivity.

---

# Prerequisites

## Retrieve Azure Credentials

On the `azure-client` host, retrieve the lab credentials:

```bash
showcreds
```

## Login to Azure

```bash
az login
```

Follow the authentication prompts using the provided lab credentials.

---

# Step 1: Generate an SSH Key Pair

Generate a new SSH key pair on the `azure-client` host:

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
```

Verify the public key exists:

```bash
ls -l ~/.ssh/id_rsa.pub
```

---

# Step 2: Create the Azure Virtual Machine

Create the VM with a static public IP address:

```bash
az vm create \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-vm \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --size Standard_B1s \
  --public-ip-address datacenter-pip \
  --public-ip-sku Standard \
  --storage-sku StandardSSD_LRS \
  --ssh-key-values ~/.ssh/id_rsa.pub
```

### Parameters Explained

| Parameter                            | Description                        |
| ------------------------------------ | ---------------------------------- |
| `--name`                             | VM name                            |
| `--image Ubuntu2204`                 | Ubuntu Server 22.04 image          |
| `--admin-username azureuser`         | Default SSH user                   |
| `--size Standard_B1s`                | VM size                            |
| `--public-ip-address datacenter-pip` | Creates and associates a public IP |
| `--public-ip-sku Standard`           | Static public IP                   |
| `--ssh-key-values`                   | SSH public key for authentication  |

---

# Step 3: Verify VM Creation

Check the VM status:

```bash
az vm show \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-vm \
  --show-details
```

Retrieve the public IP:

```bash
az vm list-ip-addresses \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-vm \
  --output table
```

Example output:

```text
VirtualMachine    PublicIP
---------------   ---------------
datacenter-vm     20.x.x.x
```

---

# Step 4: Verify Static Public IP

Verify the public IP configuration:

```bash
az network public-ip show \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-pip \
  --query "{Name:name,Allocation:publicIPAllocationMethod,IPAddress:ipAddress}" \
  --output table
```

Expected output:

```text
Name            Allocation   IPAddress
--------------  -----------  ----------
datacenter-pip  Static       20.x.x.x
```

---

# Step 5: Test SSH Connectivity

Connect to the VM using the generated SSH key:

```bash
ssh azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh azureuser@20.x.x.x
```

Expected result:

```text
Welcome to Ubuntu 22.04 LTS
```

No password should be requested because SSH key authentication is being used.

---

# Validation Checklist

- VM `datacenter-vm` created successfully.
- VM size is `Standard_B1s`.
- Ubuntu image deployed.
- SSH key pair generated on `azure-client`.
- Public key associated with the VM.
- Static public IP `datacenter-pip` created and attached.
- VM accessible via SSH using the generated key.
- No password required during SSH login.

---

# Useful Commands

View VM details:

```bash
az vm show \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-vm
```

View public IP:

```bash
az network public-ip show \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-pip
```

Delete the VM (if required):

```bash
az vm delete \
  --resource-group kml_rg_main-d4e19d1d16b243b1 \
  --name datacenter-vm
```
