---
title: Meerdere exemplaren van Azure-resources implementeren | Microsoft Docs
description: Bewerking voor het kopiëren en matrices in een Azure Resource Manager-sjabloon gebruiken om te herhalen meerdere keren bij het implementeren van resources.
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: tomfitz
ms.openlocfilehash: 22317372a7d954286ebcb0b59aea293c746b2a58
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508169"
---
# <a name="resource-property-or-variable-iteration-in-azure-resource-manager-templates"></a>Resource, eigenschap of variabele iteratie in Azure Resource Manager-sjablonen

Dit artikel laat u het maken van meer dan één exemplaar van een resource, de variabele of de eigenschap in de Azure Resource Manager-sjabloon. Voor het maken van meerdere exemplaren toevoegen de `copy` object aan de sjabloon.

Gebruikt in combinatie met een resource, heeft het kopiëren-object in de volgende indeling:

```json
"copy": {
    "name": "<name-of-loop>",
    "count": <number-of-iterations>,
    "mode": "serial" <or> "parallel",
    "batchSize": <number-to-deploy-serially>
}
```

Gebruikt in combinatie met een variabele of een eigenschap, heeft het kopiëren-object in de volgende indeling:

```json
"copy": [
  {
      "name": "<name-of-loop>",
      "count": <number-of-iterations>,
      "input": <values-for-the-property-or-variable>
  }
]
```

Beide gebruikt zijn in meer detail in dit artikel beschreven. Zie voor een zelfstudie [zelfstudie: maken van meerdere exemplaren van resources met behulp van Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md).

Als u nodig hebt om op te geven of een resource wordt geïmplementeerd op alle, Zie [voorwaarde element](resource-group-authoring-templates.md#condition).

## <a name="copy-limits"></a>Limieten kopiëren

Als u wilt het aantal iteraties opgeven, kunt u een waarde opgeven voor de eigenschap count. Het aantal mag niet meer dan 800.

Het aantal mag geen negatief getal zijn. Als u een sjabloon met REST API-versie implementeert **2019-05-10** of hoger, kunt u het aantal instellen op nul. Eerdere versies van de REST-API ondersteund niet nul zijn voor een aantal. Op dit moment ondersteunen Azure CLI of PowerShell geen nul zijn voor een aantal, maar dat ondersteuning wordt toegevoegd in een toekomstige release.

Worden met behulp van zorgvuldige [modus implementatie voltooid](deployment-modes.md) met kopiëren. Als u met de volledige modus om een resourcegroep te implementeren, worden alle resources die niet zijn opgegeven in de sjabloon na het oplossen van de lus exemplaar verwijderd.

De limieten voor het aantal zijn hetzelfde, ongeacht of gebruikt met een resource, de variabele of de eigenschap.

## <a name="resource-iteration"></a>Herhaling van de resource

Wanneer u tijdens de implementatie beslissen moet te maken van een of meer exemplaren van een resource, Voeg een `copy` element aan het brontype. Geef het aantal iteraties en een naam op voor deze lus in het element kopiëren.

De resource te maken van meerdere keren heeft de volgende notatie:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ],
  "outputs": {}
}
```

U ziet dat de naam van elke resource bevat de `copyIndex()` functie de huidige iteratie in de lus retourneert. `copyIndex()` is gebaseerd op nul. Dus in het volgende voorbeeld:

```json
"name": "[concat('storage', copyIndex())]",
```

Hiermee maakt u deze namen:

* storage0
* storage1
* storage2.

Als u de indexwaarde wilt verschuiven, kunt u een waarde doorgeven in de functie copyIndex(). Het aantal iteraties nog steeds is opgegeven in het element kopiëren, maar de waarde van copyIndex wordt gecompenseerd door de opgegeven waarde. Dus in het volgende voorbeeld:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Hiermee maakt u deze namen:

* storage1
* storage2
* storage3

De kopieerbewerking is handig bij het werken met matrices omdat u elk element in de matrix kunt doorlopen. Gebruik de `length` functie op de matrix om op te geven van het aantal iteraties, en `copyIndex` om op te halen van de huidige index in de matrix. Dus in het volgende voorbeeld:

```json
"parameters": { 
  "org": { 
    "type": "array", 
    "defaultValue": [ 
      "contoso", 
      "fabrikam", 
      "coho" 
    ] 
  }
}, 
"resources": [ 
  { 
    "name": "[concat('storage', parameters('org')[copyIndex()])]", 
    "copy": { 
      "name": "storagecopy", 
      "count": "[length(parameters('org'))]" 
    }, 
    ...
  } 
]
```

Hiermee maakt u deze namen:

* storagecontoso
* storagefabrikam
* storagecoho

Maakt de resources parallel standaard Resource Manager. De volgorde waarin ze worden gemaakt, wordt niet gegarandeerd. U kunt echter om op te geven dat de resources worden geïmplementeerd in de reeks. Bijvoorbeeld bij het bijwerken van een productie-omgeving, kunt u de updates dus spreiden alleen een bepaald aantal op elk gewenst moment worden bijgewerkt.

Voor het implementeren serie van meer dan één exemplaar van een resource, stel `mode` naar **seriële** en `batchSize` aan het aantal exemplaren moeten worden geïmplementeerd op een tijdstip. Met de seriële modus maakt Resource Manager een afhankelijkheid op eerdere exemplaren in de lus, zodat deze niet één batch wordt gestart totdat de vorige batch is voltooid.

Bijvoorbeeld: als u wilt implementeren serie opslagaccounts twee tegelijk, gebruiken:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 4,
        "mode": "serial",
        "batchSize": 2
      }
    }
  ],
  "outputs": {}
}
```

De eigenschap mode accepteert ook **parallelle**, dit is de standaardwaarde.

Zie voor meer informatie over het gebruik van kopiëren met geneste sjablonen [via het kopiëren van](resource-group-linked-templates.md#using-copy).

## <a name="property-iteration"></a>De eigenschap herhaling

Voor het maken van meer dan één waarde voor een eigenschap van een resource, Voeg een `copy` -matrix in het eigenschapselement. Deze matrix bevat objecten en elk object bevat de volgende eigenschappen:

* naam - de naam van de eigenschap te maken van verschillende waarden voor
* aantal - het aantal waarden om te maken.
* invoer - een object dat de waarden die moeten worden toegewezen aan de eigenschap bevat  

Het volgende voorbeeld laat zien hoe om toe te passen `copy` met de eigenschap dataDisks op een virtuele machine:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
        "name": "dataDisks",
        "count": 3,
        "input": {
          "lun": "[copyIndex('dataDisks')]",
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      }],
      ...
```

U ziet dat wanneer u `copyIndex` binnen een iteratie eigenschap moet u de naam van de iteratie opgeven. U hebt geen de naam gebruikt in combinatie met iteratie van de resource op te geven.

Resource Manager breidt de `copy` matrix tijdens de implementatie. De naam van de matrix, wordt de naam van de eigenschap. De ingevoerde waarden worden de eigenschappen van het object. De geïmplementeerde sjabloon als volgt uit:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
        {
          "lun": 0,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 1,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        },
        {
          "lun": 2,
          "createOption": "Empty",
          "diskSizeGB": "1023"
        }
      ],
      ...
```

Het element kopiëren is een matrix, zodat u kunt meer dan één eigenschap voor de resource opgeven. Toevoegen van een object voor elke eigenschap te maken.

```json
{
  "name": "string",
  "type": "Microsoft.Network/loadBalancers",
  "apiVersion": "2017-10-01",
  "properties": {
    "copy": [
      {
        "name": "loadBalancingRules",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      },
      {
        "name": "probes",
        "count": "[length(parameters('loadBalancingRules'))]",
        "input": {
          ...
        }
      }
    ]
  }
}
```

U kunt de resource en de eigenschap iteratie samen gebruiken. Naslaginformatie over de herhaling van de eigenschap met de naam.

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[concat(parameters('vnetname'), copyIndex())]",
  "apiVersion": "2018-04-01",
  "copy":{
    "count": 2,
    "name": "vnetloop"
  },
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[parameters('addressPrefix')]"
      ]
    },
    "copy": [
      {
        "name": "subnets",
        "count": 2,
        "input": {
          "name": "[concat('subnet-', copyIndex('subnets'))]",
          "properties": {
            "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
          }
        }
      }
    ]
  }
}
```

## <a name="variable-iteration"></a>Variabele herhaling

U kunt meerdere exemplaren van een variabele maken met de `copy` eigenschap in de sectie met variabelen. U maakt een matrix van elementen die zijn samengesteld uit de waarde in de `input` eigenschap. U kunt de `copy` eigenschap in een variabele, of op het hoogste niveau van de sectie met variabelen. Bij het gebruik van `copyIndex` in een variabele iteratie moet u de naam van de iteratie opgeven.

Zie voor een eenvoudig voorbeeld van het maken van een matrix met waarden van verbindingsreeksen [sjabloon kopiëren-matrix](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/copy-array/azuredeploy.json).

Het volgende voorbeeld ziet verschillende manieren voor het maken van matrixvariabelen voor de met dynamisch samengestelde elementen. Het laat zien hoe u kopiëren in een variabele gebruiken om te maken van matrices van objecten en tekenreeksen. Ook ziet u hoe exemplaar op het hoogste niveau gebruiken om te maken van matrices van objecten, tekenreeksen en gehele getallen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        },
        {
          "name": "diskNames",
          "count": 5,
          "input": "[concat('myDataDisk', copyIndex('diskNames', 1))]"
        }
      ]
    },
    "copy": [
      {
        "name": "top-level-object-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('top-level-object-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('top-level-object-array')]"
        }
      },
      {
        "name": "top-level-string-array",
        "count": 5,
        "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
      },
      {
        "name": "top-level-integer-array",
        "count": 5,
        "input": "[copyIndex('top-level-integer-array')]"
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleObjectArray": {
      "value": "[variables('top-level-object-array')]",
      "type" : "array"
    },
    "exampleStringArray": {
      "value": "[variables('top-level-string-array')]",
      "type" : "array"
    },
    "exampleIntegerArray": {
      "value": "[variables('top-level-integer-array')]",
      "type" : "array"
    }
  }
}
```

Het type van de variabele die wordt gemaakt, is afhankelijk van het invoerobject. Bijvoorbeeld, de variabele met de naam **top-niveau-object-matrix** in het voorgaande voorbeeld geeft als resultaat:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  },
  {
    "name": "myDataDisk4",
    "diskSizeGB": "1",
    "diskIndex": 3
  },
  {
    "name": "myDataDisk5",
    "diskSizeGB": "1",
    "diskIndex": 4
  }
]
```

En de variabele met de naam **top-niveau-string-matrix** geeft als resultaat:

```json
[
  "myDataDisk1",
  "myDataDisk2",
  "myDataDisk3",
  "myDataDisk4",
  "myDataDisk5"
]
```

## <a name="depend-on-resources-in-a-loop"></a>Afhankelijk zijn van bronnen in een lus

U opgeven dat een resource na een andere resource wordt geïmplementeerd met behulp van de `dependsOn` element. Geef de naam van de kopie-lus in het element dependsOn voor het implementeren van een resource die afhankelijk zijn van de verzameling van resources in een lus. Het volgende voorbeeld ziet drie opslagaccounts implementeren voordat u de virtuele Machine implementeert. De definitie van de volledige virtuele Machine wordt niet weergegeven. U ziet dat de kopie-element ingesteld op de naam `storagecopy` en het element dependsOn voor de virtuele Machines ook is ingesteld op `storagecopy`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    },
    {
      "apiVersion": "2015-06-15", 
      "type": "Microsoft.Compute/virtualMachines", 
      "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
      "dependsOn": ["storagecopy"],
      ...
    }
  ],
  "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>Herhaling voor een onderliggende resource
U kunt een lus kopiëren niet gebruiken voor een onderliggende resource. Voor het maken van meer dan één exemplaar van een resource die u normaal gesproken als genest in een andere resource definieert, moet u die bron in plaats daarvan maken als een resource op het hoogste niveau. Definieert u de relatie met de bovenliggende resource via de eigenschappen van het type en de naam.

Stel bijvoorbeeld dat u doorgaans een gegevensset definiëren als een onderliggende resource binnen een data factory.

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
  "resources": [
    {
      "type": "datasets",
      "name": "exampleDataSet",
      "dependsOn": [
        "exampleDataFactory"
      ],
      ...
    }
  ]
```

Voor het maken van meer dan één gegevensset, moet u deze buiten de data factory verplaatsen. De gegevensset moet zich op hetzelfde niveau bevindt als de data factory, maar het is nog steeds een onderliggende resource van de data factory. U behoudt de relatie tussen de gegevensset en data factory via de eigenschappen van het type en de naam. Omdat het type kan niet meer worden afgeleid van de positie in de sjabloon, moet u de volledig gekwalificeerde type in de indeling: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Geef een naam voor de gegevensset met de naam van de bovenliggende resource voor het maken van een bovenliggende/onderliggende relatie met een exemplaar van de data factory. Gebruik de indeling: `{parent-resource-name}/{child-resource-name}`.  

Het volgende voorbeeld ziet u de implementatie:

```json
"resources": [
{
  "type": "Microsoft.DataFactory/datafactories",
  "name": "exampleDataFactory",
  ...
},
{
  "type": "Microsoft.DataFactory/datafactories/datasets",
  "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
  "dependsOn": [
    "exampleDataFactory"
  ],
  "copy": {
    "name": "datasetcopy",
    "count": "3"
  },
  ...
}]
```

## <a name="example-templates"></a>Voorbeeldsjablonen

De volgende voorbeelden ziet algemene scenario's voor het maken van meer dan één exemplaar van een resource of de eigenschap.

|Template  |Description  |
|---------|---------|
|[Storage kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Meer dan één opslagaccount met een index-nummer in de naam van de implementeert. |
|[Opslagruimte voor de seriële](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Verschillende opslagaccounts één implementeert tegelijk. De naam bevat het indexnummer. |
|[Opslag met een matrix kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Verschillende opslagaccounts implementeert. De naam bevat een waarde van een matrix. |
|[VM-implementatie met een variabele aantal gegevensschijven](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Aantal gegevensschijven met een virtuele machine implementeert. |
|[Kopieer variabelen](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Ziet u de verschillende manieren van het doorlopen van variabelen. |
|[Meerdere beveiligingsregels](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Diverse beveiligingsregels implementeert in een netwerkbeveiligingsgroep. Vormt de beveiligingsregels van een parameter. Zie voor de parameter [meerdere NSG parameterbestand](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een zelfstudie doorloopt, [zelfstudie: maken van meerdere exemplaren van resources met behulp van Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md).

* Als u meer informatie over de secties van een sjabloon wilt, Zie [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van uw sjabloon, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

