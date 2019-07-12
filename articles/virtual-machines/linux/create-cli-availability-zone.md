---
title: Een zones ingedeelde Linux-VM maken met de Azure CLI | Microsoft Docs
description: Een Linux-VM maken in een beschikbaarheidszone met Azure CLI
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: gwallace
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 87263d11828ff5122122ef36850fade87949bcac
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671617"
---
# <a name="create-a-linux-virtual-machine-in-an-availability-zone-with-the-azure-cli"></a>Een Linux-machine maken in een beschikbaarheidszone met Azure CLI

In dit artikel wordt stapsgewijs uitgelegd met de Azure CLI een Linux-VM maken in een Azure-beschikbaarheidszone. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

Als u een beschikbaarheidszone wilt gebruiken, maakt u een virtuele machine in een [ondersteunde Azure-regio](../../availability-zones/az-overview.md#services-support-by-region).

Zorg ervoor dat u de meest recente hebt geïnstalleerd [Azure CLI](/cli/azure/install-az-cli2) en aangemeld bij een Azure-account met [az login](/cli/azure/reference-index).


## <a name="check-vm-sku-availability"></a>Beschikbaarheid van VM-SKU controleren
De beschikbaarheid van VM-grootten, of SKU's, kan verschillen per regio en zone. Als hulp bij de planning voor het gebruik van de beschikbaarheidszones kunt u een overzicht maken van de beschikbare VM-SKU's per Azure-regio en zone. Hiermee zorgt u ervoor dat u een correcte VM-grootte kiest en de gewenste tolerantie in zones verkrijgt. Voor meer informatie over de verschillende VM-typen en -grootten raadpleegt u het [Overzicht van de VM-grootten](sizes.md).

U vindt de beschikbare VM-SKU's met de [az vm list-skus](/cli/azure/vm) opdracht. Met het volgende voorbeeld wordt een lijst weergegeven van de beschikbare VM-SKU's in de regio *eastus2*:

```azurecli
az vm list-skus --location eastus2 --output table
```

De uitvoer is vergelijkbaar met het volgende verkorte voorbeeld, waarin de beschikbaarheidszones worden weergegeven waarin elke VM-grootte beschikbaar is:

```azurecli
ResourceType      Locations  Name               [...]    Tier       Size     Zones
----------------  ---------  -----------------           ---------  -------  -------
virtualMachines   eastus2    Standard_DS1_v2             Standard   DS1_v2   1,2,3
virtualMachines   eastus2    Standard_DS2_v2             Standard   DS2_v2   1,2,3
[...]
virtualMachines   eastus2    Standard_F1s                Standard   F1s      1,2,3
virtualMachines   eastus2    Standard_F2s                Standard   F2s      1,2,3
[...]
virtualMachines   eastus2    Standard_D2s_v3             Standard   D2_v3    1,2,3
virtualMachines   eastus2    Standard_D4s_v3             Standard   D4_v3    1,2,3
[...]
virtualMachines   eastus2    Standard_E2_v3              Standard   E2_v3    1,2,3
virtualMachines   eastus2    Standard_E4_v3              Standard   E4_v3    1,2,3
```


## <a name="create-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group).  

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Voordat een virtuele machine wordt gemaakt, moet een resourcegroep worden gemaakt. In dit voorbeeld wordt een resourcegroep met de naam *myResourceGroupVM* wordt gemaakt in de *eastus2* regio. VS-Oost 2 is een van de Azure-regio's die ondersteuning biedt voor beschikbaarheidszones.

```azurecli 
az group create --name myResourceGroupVM --location eastus2
```

De resourcegroep wordt opgegeven bij het maken of wijzigen van een virtuele machine, die kan worden weergegeven in dit artikel.

## <a name="create-virtual-machine"></a>Virtuele machine maken

Maak een virtuele machine met de opdracht [az vm create](/cli/azure/vm). 

Wanneer u een virtuele machine maakt, zijn er diverse opties beschikbaar zoals besturingssysteeminstallatiekopie, schijfgrootte en beheerdersreferenties. In dit voorbeeld wordt een virtuele machine gemaakt met de naam *myVM* waarop Ubuntu-server wordt uitgevoerd. De virtuele machine wordt gemaakt in de beschikbaarheidszone *1*. De virtuele machine wordt standaard gemaakt in de *Standard_DS1_v2* grootte.

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --location eastus2 --image UbuntuLTS --generate-ssh-keys --zone 1
```

Het maken van de virtuele machine kan een paar minuten duren. Wanneer de virtuele machine is gemaakt, biedt de Azure CLI informatie over de virtuele machine. Noteer de `zones` waarde waarmee wordt aangegeven van de beschikbaarheidszone waarin de virtuele machine wordt uitgevoerd. 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus2",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM",
  "zones": "1"
}
```

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Zone voor beheerde schijf- en IP-adres bevestigen

Wanneer de virtuele machine in een beschikbaarheidszone is geïmplementeerd, wordt een beheerde schijf voor de virtuele machine in dezelfde beschikbaarheidszone gemaakt. Een openbaar IP-adres wordt standaard ook gemaakt in de zone. De volgende voorbeelden krijg informatie over deze resources.

Om te bevestigen dat de beheerde schijf van de virtuele machine in de beschikbaarheidszone bevindt, gebruikt u de [az vm show](/cli/azure/vm) opdracht om de schijf-ID. In dit voorbeeld wordt de schijf-ID opgeslagen in een variabele die wordt gebruikt in een latere stap. 

```azurecli-interactive
osdiskname=$(az vm show -g myResourceGroupVM -n myVM --query "storageProfile.osDisk.name" -o tsv)
```
U krijgt nu informatie over de beheerde schijf:

```azurecli-interactive
az disk show --resource-group myResourceGroupVM --name $osdiskname
```

In de uitvoer ziet u dat de beheerde schijf zich in dezelfde beschikbaarheidszone bevindt als de virtuele machine:

```azurecli
{
  "creationData": {
    "createOption": "FromImage",
    "imageReference": {
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westeurope/Publishers/Canonical/ArtifactTypes/VMImage/Offers/UbuntuServer/Skus/16.04-LTS/Versions/latest",
      "lun": null
    },
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/disks/osdisk_761c570dab",
  "location": "eastus2",
  "managedBy": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "name": "myVM_osdisk_761c570dab",
  "osType": "Linux",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroupVM",
  "sku": {
    "name": "Premium_LRS",
    "tier": "Premium"
  },
  "tags": {},
  "timeCreated": "2018-03-05T22:16:06.892752+00:00",
  "type": "Microsoft.Compute/disks",
  "zones": [
    "1"
  ]
}
```

Gebruik de [az vm list-ip-adressen](/cli/azure/vm) opdracht om de naam van openbare IP-adresresource in *myVM*. In dit voorbeeld wordt de naam van de opgeslagen in een variabele die wordt gebruikt in een latere stap.

```azurecli
ipaddressname=$(az vm list-ip-addresses -g myResourceGroupVM -n myVM --query "[].virtualMachine.network.publicIpAddresses[].name" -o tsv)
```

U krijgt nu informatie over het IP-adres:

```azurecli
az network public-ip show --resource-group myResourceGroupVM --name $ipaddressname
```

De uitvoer ziet u dat het IP-adres zich in dezelfde beschikbaarheidszone als de virtuele machine:

```azurecli
{
  "dnsSettings": null,
  "etag": "W/\"b7ad25eb-3191-4c8f-9cec-c5e4a3a37d35\"",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "52.174.34.95",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupVM/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "myResourceGroupVM",
    "subnet": null
  },
  "location": "eastUS2",
  "name": "myVMPublicIP",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "myResourceGroupVM",
  "resourceGuid": "8c70a073-09be-4504-0000-000000000000",
  "tags": {},
  "type": "Microsoft.Network/publicIPAddresses",
  "zones": [
    "1"
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een VM maakt in een beschikbaarheidszone. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.




