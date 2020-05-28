---
title: Voorwaardelijke implementatie met sjablonen
description: Hierin wordt beschreven hoe u een resource voorwaardelijk kunt implementeren in een Azure Resource Manager sjabloon.
ms.topic: conceptual
ms.date: 12/03/2019
ms.openlocfilehash: 001a1a7d6d15fe29b0f3184b75892f4ec75cef27
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84017491"
---
# <a name="conditional-deployment-in-arm-templates"></a>Voorwaardelijke implementatie in ARM-sjablonen

Soms moet u optioneel een resource implementeren in een Azure Resource Manager ARM-sjabloon. Gebruik het- `condition` element om op te geven of de resource is geïmplementeerd. De waarde voor dit element wordt omgezet in True of false. Als de waarde True is, wordt de resource gemaakt. Als de waarde False is, wordt de resource niet gemaakt. De waarde kan alleen worden toegepast op de hele resource.

## <a name="new-or-existing-resource"></a>Nieuwe of bestaande resource

U kunt voorwaardelijke implementatie gebruiken om een nieuwe resource te maken of een bestaande te gebruiken. In het volgende voor beeld ziet u hoe u een voor waarde gebruikt om een nieuw opslag account te implementeren of een bestaand opslag account te gebruiken.

```json
{
  "condition": "[equals(parameters('newOrExisting'),'new')]",
  "type": "Microsoft.Storage/storageAccounts",
  "apiVersion": "2017-06-01",
  "name": "[variables('storageAccountName')]",
  "location": "[parameters('location')]",
  "sku": {
    "name": "[variables('storageAccountType')]"
  },
  "kind": "Storage",
  "properties": {}
}
```

Wanneer de para meter **newOrExisting** is ingesteld op **Nieuw**, wordt de voor waarde geëvalueerd als waar. Het opslag account wordt geïmplementeerd. Als **newOrExisting** echter is ingesteld op **Exists**, wordt de voor waarde geëvalueerd als False en wordt het opslag account niet geïmplementeerd.

`condition`Zie [VM met een nieuwe of bestaande Virtual Network, opslag en openbaar IP-adres](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions)voor een complete voorbeeld sjabloon die gebruikmaakt van het-element.

## <a name="allow-condition"></a>Voor waarde toestaan

U kunt een parameter waarde door geven die aangeeft of een voor waarde is toegestaan. In het volgende voor beeld wordt een SQL-Server geïmplementeerd en worden optioneel Azure Ip's toegestaan.

```json
{
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2015-05-01-preview",
  "name": "[parameters('serverName')]",
  "location": "[parameters('location')]",
  "properties": {
    "administratorLogin": "[parameters('administratorLogin')]",
    "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
    "version": "12.0"
  },
  "resources": [
    {
      "condition": "[parameters('allowAzureIPs')]",
      "type": "firewallRules",
      "apiVersion": "2015-05-01-preview",
      "name": "AllowAllWindowsAzureIps",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Sql/servers/', parameters('serverName'))]"
      ],
      "properties": {
        "endIpAddress": "0.0.0.0",
        "startIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Zie voor de volledige sjabloon de [logische Azure SQL-Server](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server).

## <a name="runtime-functions"></a>Runtime-functies

Als u een [verwijzing](template-functions-resource.md#reference) of [lijst](template-functions-resource.md#list) functie gebruikt met een resource die voorwaardelijk is geïmplementeerd, wordt de functie geëvalueerd, zelfs als de resource niet is geïmplementeerd. Er wordt een fout bericht weer geven als de functie verwijst naar een resource die niet bestaat.

Gebruik de functie [als](template-functions-logical.md#if) om ervoor te zorgen dat de functie alleen wordt geëvalueerd voor omstandigheden wanneer de resource wordt geïmplementeerd. Zie de [functie als](template-functions-logical.md#if) voor een voorbeeld sjabloon die gebruikmaakt van if en verwijst naar een voorwaardelijk geïmplementeerde resource.

U stelt een [resource in die afhankelijk](define-resource-dependency.md) is van een voorwaardelijke resource, op dezelfde manier als andere resources. Wanneer een voorwaardelijke resource niet is geïmplementeerd, wordt deze automatisch door Azure Resource Manager verwijderd uit de vereiste afhankelijkheden.

## <a name="condition-with-complete-mode"></a>Voor waarde met volledige modus

Als u een sjabloon implementeert met de [modus volledig](deployment-modes.md) en een resource wordt niet geïmplementeerd omdat de voor waarde wordt geëvalueerd als onwaar, is het resultaat afhankelijk van de rest API versie die u gebruikt om de sjabloon te implementeren. Als u een eerdere versie dan 2019-05-10 gebruikt, wordt de resource **niet verwijderd**. Met 2019-05-10 of hoger wordt de resource **verwijderd**. Met de nieuwste versies van Azure PowerShell en Azure CLI wordt de resource verwijderd wanneer de voor waarde ONWAAR is.

## <a name="next-steps"></a>Volgende stappen

* Zie [Aanbevolen procedures voor arm](template-best-practices.md)-sjablonen voor aanbevelingen voor het maken van een sjabloon.
* Zie [resource herhaling in arm-sjablonen](copy-resources.md)als u meerdere exemplaren van een resource wilt maken.
