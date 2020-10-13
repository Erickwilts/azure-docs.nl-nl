---
title: Opslag van beheerde schijven converteren tussen Standard en Premium SSD
description: Het converteren van Azure Managed disks-opslag van Standard naar Premium of Premium naar Standard met behulp van de Azure CLI.
author: roygara
ms.service: virtual-machines-linux
ms.topic: how-to
ms.date: 07/12/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 10674d15a7515b01b0df6cf37bce89f153cb9b0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88870682"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Azure Managed disks-opslag converteren van Standard naar Premium of Premium naar Standard

Er zijn vier schijf typen van Azure Managed disks: Azure Ultra disks, Premium SSD, Standard SSD en standaard HDD. U kunt scha kelen tussen de drie GA-schijf typen (Premium SSD, Standard SSD en standaard HDD) op basis van uw prestatie behoeften. U kunt niet overstappen van of naar een ultra schijf. u moet een nieuwe implementeren.

Deze functionaliteit wordt niet ondersteund voor niet-beheerde schijven. U kunt echter eenvoudig een niet-beheerde [schijf converteren naar een Managed Disk](convert-unmanaged-to-managed-disks.md) om tussen schijf typen te scha kelen.

In dit artikel wordt beschreven hoe u Managed disks converteert van Standard naar Premium of Premium naar Standard met behulp van de Azure CLI. Als u het hulp programma wilt installeren of upgraden, raadpleegt u [Azure cli installeren](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Voordat u begint

* Schijf conversie vereist een herstart van de virtuele machine (VM), dus u kunt de migratie van uw schijf opslag plannen tijdens een reeds bestaand onderhouds venster.
* Voor niet-beheerde schijven moet u eerst [converteren naar Managed disks](convert-unmanaged-to-managed-disks.md) zodat u kunt scha kelen tussen opslag opties.


## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Alle beheerde schijven van een virtuele machine overschakelen tussen Premium en Standard

In dit voor beeld ziet u hoe u alle schijven van een virtuele machine converteert van Standard naar Premium-opslag of van Premium naar standaard opslag. Als u Premium Managed disks wilt gebruiken, moet uw virtuele machine gebruikmaken van een [VM-grootte](../sizes.md) die Premium-opslag ondersteunt. In dit voor beeld wordt ook overgeschakeld naar een grootte die ondersteuning biedt voor Premium-opslag.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Afzonderlijke beheerde schijven wisselen tussen Standard en Premium

Voor uw werk belasting voor ontwikkelen en testen wilt u mogelijk een combi natie van de standaard-en Premium-schijven om uw kosten te verlagen. U kunt ervoor kiezen om alleen die schijven bij te werken waarvoor betere prestaties nodig zijn. In dit voor beeld ziet u hoe u een enkele VM-schijf converteert van Standard naar Premium-opslag of van Premium naar standaard opslag. Als u Premium Managed disks wilt gebruiken, moet uw virtuele machine gebruikmaken van een [VM-grootte](../sizes.md) die Premium-opslag ondersteunt. In dit voor beeld wordt ook overgeschakeld naar een grootte die ondersteuning biedt voor Premium-opslag.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Beheerde schijven wisselen tussen Standard-HDD en Standard-SSD

In dit voor beeld ziet u hoe u een enkele VM-schijf converteert van Standard-HDD naar Standard-SSD of van Standard-SSD naar Standard-HDD.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM ID 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-between-standard-and-premium-in-azure-portal"></a>Managed disks tussen Standard en Premium in Azure Portal

Volg deze stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer de VM in de lijst met **virtuele machines**.
3. Als de virtuele machine niet is gestopt, selecteert u **stoppen** boven in het deel venster VM- **overzicht** en wacht u totdat de virtuele machine is gestopt.
4. Selecteer in het deel venster voor de VM de optie **schijven** in het menu.
5. Selecteer de schijf die u wilt converteren.
6. Selecteer **configuratie** in het menu.
7. Wijzig het **account type** van **Standard-HDD** in **Premium-SSD** of van **Premium-SSD** in **Standard-HDD**.
8. Selecteer **Opslaan**en sluit het deel venster schijf.

De update van het schijf type is direct. U kunt de virtuele machine na de conversie opnieuw opstarten.

## <a name="next-steps"></a>Volgende stappen

Een alleen-lezen kopie van een virtuele machine maken met behulp van [moment opnamen](snapshot-copy-managed-disk.md).