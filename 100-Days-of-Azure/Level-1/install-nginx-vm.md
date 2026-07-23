# Azure VM Deployment with Nginx (Nautilus DevOps Task)

## Objective

Provision an Azure Virtual Machine named **`nautilus-vm`** in the **East US** region, configure it to automatically install and start **Nginx** during provisioning using **cloud-init**, and allow HTTP traffic (port **80**) from the Internet.

---

## Prerequisites

- Azure CLI installed
- Azure subscription credentials
- Existing Resource Group:
  - `kml_rg_main-d893116ca52648e4`

---

## Verify Available Resource Groups

```bash
az group list --output table
```

Example:

```
Name                          Location    Status
-------------------------------------------------
kml_rg_main-ebd504e5dedd473f  eastus      Succeeded
```

---

## Create the Cloud-Init Configuration

Create a file named `cloud-init.txt`.

```bash
cat > cloud-init.txt <<'EOF'
#cloud-config
package_update: true
packages:
  - nginx

runcmd:
  - systemctl enable nginx
  - systemctl start nginx
EOF
```

This cloud-init configuration performs the following tasks:

- Updates package metadata
- Installs the Nginx package
- Enables the Nginx service
- Starts the Nginx service during the first boot

---

## Create the Virtual Machine

```bash
az vm create \
  --resource-group kml_rg_main-ebd504e5dedd473f \
  --name datacenter-vm \
  --image Ubuntu2404 \
  --location eastus \
  --size Standard_B2s \
  --storage-sku StandardSSD_LRS \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data cloud-init.txt
```

Successful output:

```
VM Name: nautilus-vm
Location: eastus
Power State: VM running
```

---

## Configure Network Security Group

Allow inbound HTTP traffic on port **80**.

```bash
az vm open-port \
  --resource-group kml_rg_main-ebd504e5dedd473f \
  --name datacenter-vm \
  --port 80
```

---

## Verify the Deployment

Retrieve the VM information.

```bash
az vm show \
  --resource-group kml_rg_main-ebd504e5dedd473f \
  --name datacenter-vm \
  -d \
  --output table
```

Example output:

```
Public IP : 20.51.171.19
PowerState: VM running
```

---

## Test Nginx

Open a browser:

```
http://<PUBLIC_IP>
```

Or verify using curl:

```bash
curl http://<PUBLIC_IP>
```

Expected output:

```
Welcome to nginx!
```

---

## Troubleshooting

### VM Size Capacity Error

If the selected VM size is unavailable:

```text
SkuNotAvailable
```

Choose another supported VM size for the East US region.

---

### Storage Policy Error

If Azure returns:

```text
RequestDisallowedByPolicy
```

Use a supported storage SKU:

```bash
--storage-sku StandardSSD_LRS
```

---

### HTTP Not Accessible

Verify that port **80** is open:

```bash
az vm open-port \
  --resource-group kml_rg_main-ebd504e5dedd473f \
  --name datacenter-vm \
  --port 80
```

---

## Resources Created

| Resource       | Value                          |
| -------------- | ------------------------------ |
| Resource Group | `kml_rg_main-d893116ca52648e4` |
| VM Name        | `nautilus-vm`                  |
| Region         | `East US`                      |
| Image          | Ubuntu 24.04 LTS               |
| VM Size        | Standard_B2s                   |
| OS Disk        | StandardSSD_LRS                |
| Web Server     | Nginx                          |
| HTTP Port      | 80                             |

---

## Outcome

The deployment provisions an Ubuntu virtual machine in Azure, automatically installs and starts Nginx using cloud-init, and exposes the web server over HTTP (port 80). The VM is ready to host web applications immediately after provisioning.
