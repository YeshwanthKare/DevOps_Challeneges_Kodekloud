# README - Azure ARM Template Virtual Network Deployment

## Objective

Modify and deploy an Azure ARM template to create a Virtual Network with the following requirements:

- Virtual Network name: `arm-vnet-xfusion`
- Tag `displayName`: `arm-vnet-xfusion`
- Address Space: `192.168.0.0/16`
- Additional tag:
  - `Environment = KKE-xfusion`

---

## Prerequisites

- Azure CLI installed and authenticated.
- ARM template located at:

```bash
/root/arm-templates/vnet-deployment-template.json
```

---

## Step 1: Update the ARM Template

Edit the template:

```bash
vi /root/arm-templates/vnet-deployment-template.json
```

### Update Virtual Network Name

Change:

```json
"name": "existing-vnet-name"
```

To:

```json
"name": "arm-vnet-xfusion"
```

---

### Update Address Space

Change:

```json
"addressPrefixes": [
    "10.0.0.0/16"
]
```

To:

```json
"addressPrefixes": [
    "192.168.0.0/16"
]
```

---

### Update Tags

Modify the tags section:

```json
"tags": {
    "displayName": "arm-vnet-xfusion",
    "Environment": "KKE-xfusion"
}
```

---

## Step 2: Deploy the ARM Template

Deploy the template using Azure CLI:

```bash
az deployment group create \
  --name "vnet-deployment" \
  --resource-group "kml_rg_main-8f5167c873254179" \
  --template-file "/root/arm-templates/vnet-deployment-template.json"
```

---

## Step 3: Verify Deployment

Expected output:

```json
{
  "provisioningState": "Succeeded"
}
```

Verify the created Virtual Network:

```bash
az network vnet show \
  --name arm-vnet-xfusion \
  --resource-group kml_rg_main-8f5167c873254179
```

---

## Validation Checklist

- Virtual Network name is `arm-vnet-xfusion`
- `displayName` tag is `arm-vnet-xfusion`
- `Environment` tag is `KKE-xfusion`
- Address space is `192.168.0.0/16`
- Deployment status is `Succeeded`

---

## Useful Commands

### Validate ARM Template

```bash
az deployment group validate \
  --resource-group kml_rg_main-8f5167c873254179 \
  --template-file /root/arm-templates/vnet-deployment-template.json
```

### Show Deployment Details

```bash
az deployment group show \
  --name vnet-deployment \
  --resource-group kml_rg_main-8f5167c873254179
```

### List Virtual Networks

```bash
az network vnet list \
  --resource-group kml_rg_main-8f5167c873254179 \
  --output table
```

## Result

Successfully modified and deployed the ARM template, creating the Virtual Network `arm-vnet-xfusion` with the required address space and tags in Azure.
