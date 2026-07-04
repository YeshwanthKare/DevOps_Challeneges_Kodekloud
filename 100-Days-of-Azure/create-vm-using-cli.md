# Create VM using the CLI

- List the Azure resourceGroup name

```
az group list --output table
```

- Create VM using the CLI

```
export MY_VM_NAME="datacenter-vm"
export MY_USERNAME=azureuser
export MY_VM_IMAGE="Ubuntu2204"
export MY_RESOURCE_GROUP_NAME="kml_rg_main-e3d7db254ab64fd2"
az vm create \
  --resource-group $MY_RESOURCE_GROUP_NAME \
  --name $MY_VM_NAME \
  --image $MY_VM_IMAGE \
  --size Standard_B2s \
  --admin-username $MY_USERNAME \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30
```

- Verify if the VM is created

```
az vm show \
  --resource-group $MY_RESOURCE_GROUP_NAME \
  --name $MY_VM_NAME \
  -d \
  -o table
```

- SSH to the VM

```
ssh azureuser@52.188.11.207
```

- Verify the disk size

```
az vm show \
  --resource-group $MY_RESOURCE_GROUP_NAME \
  --name $MY_VM_NAME \
  --query "storageProfile.osDisk.diskSizeGb"
```

- Verify Storage SKU

```
az vm show \
  --resource-group $MY_RESOURCE_GROUP_NAME \
  --name $MY_VM_NAME \
  --query "storageProfile.osDisk.managedDisk.storageAccountType"
```
