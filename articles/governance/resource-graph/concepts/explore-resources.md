---
title: Uw Azure-resources verkennen
description: Informatie over het gebruik van de querytaal van Resource-grafiek aan uw resources verkennen en ontdekken hoe ze zijn verbonden.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/23/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0b4a75558f5e82b707ae5d012acef4d2c5c4b7a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64723804"
---
# <a name="explore-your-azure-resources-with-resource-graph"></a>Azure-resources verkennen met Resource Graph

Azure Resource Graph biedt de mogelijkheid om te verkennen en detecteren van uw Azure-resources, snel en op schaal. Gebouwd voor snelle antwoorden, is een uitstekende manier om meer informatie over uw omgeving en ook over de eigenschappen van uw Azure-resources.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="explore-virtual-machines"></a>Verken virtual machines

Een algemene resource in Azure is een virtuele machine. Virtuele machines hebben als een brontype veel eigenschappen die kunnen worden opgevraagd. Elke eigenschap bevat een optie voor het filteren of zoeken naar exact de resource die u zoekt.

### <a name="virtual-machine-discovery"></a>Detectie van de virtuele machine

Laten we beginnen met een eenvoudige query voor het ophalen van een enkele virtuele machine uit onze omgeving en bekijk de eigenschappen geretourneerd.

```kusto
where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1" | ConvertTo-Json -Depth 100
```

> [!NOTE]
> Azure PowerShell `Search-AzGraph` cmdlet retourneert een **PSCustomObject** standaard. De uitvoer zien er hetzelfde uit als u wat wordt geretourneerd door de Azure CLI, hebben de `ConvertTo-Json` cmdlet wordt gebruikt. De standaardwaarde voor **diepte** is _2_. Instellen op _100_ moet alle geretourneerde niveaus converteren.

De structuur van de JSON-resultaten die vergelijkbaar is met het volgende voorbeeld:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1",
    "plan": {},
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_B2s"
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535",
            "resourceGroup": "MyResourceGroup"
          }
        ]
      },
      "osProfile": {
        "adminUsername": "localAdmin",
        "computerName": "ContosoVM1",
        "secrets": [],
        "windowsConfiguration": {
          "enableAutomaticUpdates": true,
          "provisionVMAgent": true
        }
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "dataDisks": [],
        "imageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": 127,
          "managedDisk": {
            "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
            "resourceGroup": "MyResourceGroup",
            "storageAccountType": "Premium_LRS"
          },
          "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
          "osType": "Windows"
        }
      },
      "vmId": "bbb9b451-6dc7-4117-bec5-c971eb1118c6"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {},
    "subscriptionId": "<subscriptionId>",
    "tags": {},
    "type": "microsoft.compute/virtualmachines"
  }
]
```

De eigenschappen Vertel ons meer informatie over de virtuele machine-resource zelf, alles van SKU, besturingssysteem, schijven, labels, en de resourcegroep en het abonnement is een lid van.

### <a name="virtual-machines-by-location"></a>Virtuele machines per locatie

We nemen we hebben geleerd over de bron van de virtuele machines, gebruiken de **locatie** eigenschap voor het tellen van alle virtuele machines op locatie. Voor het bijwerken van de query, we de limiet verwijderen en samenvatten van het aantal waarden van de locatie.

```kusto
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by location
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

De structuur van de JSON-resultaten die vergelijkbaar is met het volgende voorbeeld:

```json
[
  {
    "count_": 386,
    "location": "eastus"
  },
  {
    "count_": 215,
    "location": "southcentralus"
  },
  {
    "count_": 59,
    "location": "westus"
  }
]
```

Nu zien we het aantal virtuele machines die we in elke Azure-regio hebben.

### <a name="virtual-machines-by-sku"></a>Virtuele machines per SKU

Ga terug naar de oorspronkelijke virtuele machine-eigenschappen, gaan we proberen te vinden van alle virtuele machines waarvoor een SKU-grootte van **Standard_B2s**. Kijken naar de JSON geretourneerd, zien we dat deze opgeslagen **properties.hardwareprofile.vmsize**. De query voor het zoeken van alle virtuele machines die overeenkomen met deze grootte en retourneert alleen de naam van de virtuele machine en de regio wordt bijgewerkt.

```kusto
where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| project name, resourceGroup"
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

### <a name="virtual-machines-connected-to-premium-managed-disks"></a>Virtuele machines die zijn verbonden met premium-beheerde schijven

Als we wilden om op te halen van de details van premium-beheerde schijven die zijn gekoppeld aan deze **Standard_B2s** virtuele machines, breidt u de query voor het geven van de resource-ID van de beheerde schijven.

```kusto
where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| extend disk = properties.storageProfile.osDisk.managedDisk
| where disk.storageAccountType == 'Premium_LRS'
| project disk.id
```

> [!NOTE]
> Een andere manier om op te halen van de SKU zou zijn met behulp van de **aliassen** eigenschap **Microsoft.Compute/virtualMachines/sku.name**. Zie de [aliassen weergeven](../samples/starter.md#show-aliases) en [afzonderlijke alias waarden weergeven](../samples/starter.md#distinct-alias-values) voorbeelden.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

```azurepowershell-interactive
  Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

Het resultaat is een lijst van schijf-id's.

### <a name="managed-disk-discovery"></a>Detectie van de beheerde schijf

Met de eerste record uit de vorige query verkennen we de eigenschappen die aanwezig zijn op de beheerde schijf die is gekoppeld aan de eerste virtuele machine. De bijgewerkte query maakt gebruik van de schijf-ID en het type gewijzigd.

Voorbeeld van uitvoer van de vorige query, bijvoorbeeld:

```json
[
  {
    "disk_id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166"
  }
]
```

```kusto
where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'
```

Voordat u de query uitvoert, hoe we wist de **type** zou nu moeten **Microsoft.Compute/disks**?
Als u de volledige ID bekijkt, ziet u **/providers/Microsoft.Compute/disks/** als onderdeel van de tekenreeks. Dit fragment tekenreeks biedt u een hint over welk type moet worden gezocht. Een alternatieve methode zijn voor het verwijderen van de limiet per type en in plaats daarvan alleen zoeken op het veld ID. Als de ID uniek is, slechts één record zou worden geretourneerd en de **type** eigenschap erop dat gegevens bevat.

> [!NOTE]
> Voor dit voorbeeld werkt, moet u het veld ID vervangen door een resultaat van uw eigen omgeving.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

De structuur van de JSON-resultaten die vergelijkbaar is met het volgende voorbeeld:

```json
[
  {
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "plan": {},
    "properties": {
      "creationData": {
        "createOption": "Empty"
      },
      "diskSizeGB": 127,
      "diskState": "ActiveSAS",
      "provisioningState": "Succeeded",
      "timeCreated": "2018-09-14T12:17:32.2570000Z"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {
      "name": "Premium_LRS",
      "tier": "Premium"
    },
    "subscriptionId": "<subscriptionId>",
    "tags": {
      "environment": "prod"
    },
    "type": "microsoft.compute/disks"
  }
]
```

## <a name="explore-virtual-machines-to-find-public-ip-addresses"></a>Verken virtual machines om te vinden van openbare IP-adressen

Dit Azure CLI-set van query's eerst worden gevonden en alle de netwerkinterfaces (NIC)-resources die zijn verbonden met virtuele machines worden opgeslagen. Vervolgens gebruikt de lijst met NIC's om te vinden van elk IP-adresresource die een openbaar IP-adres is en deze waarden worden opgeslagen. Ten slotte biedt een lijst van de openbare IP-adressen.

```azurecli-interactive
# Use Resource Graph to get all NICs and store in the 'nic' variable
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20" --output table | tail -n +3 > nics.txt

# Review the output of the query stored in 'nics.txt'
cat nics.txt
```

Gebruik de `nics.txt` bestand in de volgende query voor de netwerkinterface van de gerelateerde resources informatie wanneer er een openbaar IP-adres dat is gekoppeld aan de NIC.

```azurecli-interactive
# Use Resource Graph with the 'nics.txt' file to get all related public IP addresses and store in 'publicIp.txt' file
az graph query -q="where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$(awk -vORS="','" '{print $0}' nics.txt | sed 's/,$//')') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp" --output table | tail -n +3 > ips.txt

# Review the output of the query stored in 'ips.txt'
cat ips.txt
```

Laatste, gebruik de lijst met resources voor openbaar IP-adres die zijn opgeslagen in `ips.txt` het werkelijke openbare IP-adres van deze verkrijgen en weergeven.

```azurecli-interactive
# Use Resource Graph with the 'ips.txt' file to get the IP address of the public IP address resources
az graph query -q="where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$(awk -vORS="','" '{print $0}' ips.txt | sed 's/,$//')') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip" --output table
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [querytaal](query-language.md)
- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md)
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md)