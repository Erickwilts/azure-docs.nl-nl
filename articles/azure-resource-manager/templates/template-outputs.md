---
title: Uitvoer in sjablonen
description: Hierin wordt beschreven hoe u uitvoer waarden definieert in een Azure Resource Manager sjabloon.
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 203bfc66e9515ef14a5fe1315ef5b9ee07075041
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "79460021"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Uitvoer in Azure Resource Manager sjabloon

In dit artikel wordt beschreven hoe u uitvoer waarden definieert in uw Azure Resource Manager sjabloon. U gebruikt uitvoer wanneer u waarden van de geïmplementeerde resources moet retour neren.

## <a name="define-output-values"></a>Uitvoer waarden definiëren

In het volgende voor beeld ziet u hoe u de resource-ID voor een openbaar IP-adres als resultaat kunt geven:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>Voorwaardelijke uitvoer

In de sectie outputs kunt u voorwaardelijk een waarde Retour neren. Normaal gesp roken gebruikt u de voor waarde in de uitvoer wanneer u een resource [voorwaardelijk hebt geïmplementeerd](conditional-resource-deployment.md) . In het volgende voor beeld ziet u hoe u de resource-ID voor een openbaar IP-adres voorwaardelijk kunt retour neren op basis van het feit of er een nieuw pakket is geïmplementeerd:

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Zie [voorwaardelijke uitvoer sjabloon](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json)voor een eenvoudig voor beeld van voorwaardelijke uitvoer.

## <a name="dynamic-number-of-outputs"></a>Dynamisch aantal uitvoer bewerkingen

In sommige scenario's weet u niet het aantal exemplaren van een waarde die u moet retour neren bij het maken van de sjabloon. U kunt een variabele aantal waarden retour neren met behulp van het element **copy** .

```json
"outputs": {
  "storageEndpoints": {
    "type": "array",
    "copy": {
      "count": "[parameters('storageCount')]",
      "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
    }
  }
}
```

Zie [herhalingen in azure Resource Manager-sjablonen](copy-outputs.md)voor meer informatie.

## <a name="linked-templates"></a>Gekoppelde sjablonen

Gebruik de functie [Reference](template-functions-resource.md#reference) in de bovenliggende sjabloon om de uitvoer waarde van een gekoppelde sjabloon op te halen. De syntaxis van de bovenliggende sjabloon is:

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

Bij het ophalen van een uitvoer eigenschap van een gekoppelde sjabloon, mag de naam van de eigenschap geen streepje bevatten.

In het volgende voor beeld ziet u hoe u het IP-adres instelt op een load balancer door een waarde op te halen uit een gekoppelde sjabloon.

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

U kunt de functie niet gebruiken `reference` in het gedeelte outputs van een [geneste sjabloon](linked-templates.md#nested-template). Als u de waarden voor een geïmplementeerde resource in een geneste sjabloon wilt retour neren, converteert u de geneste sjabloon naar een gekoppelde sjabloon.

## <a name="get-output-values"></a>Uitvoer waarden ophalen

Wanneer de implementatie is geslaagd, worden de uitvoer waarden automatisch geretourneerd in de resultaten van de implementatie.

Als u uitvoer waarden wilt ophalen uit de implementatie geschiedenis, kunt u script gebruiken.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli-interactive
az deployment group show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="example-templates"></a>Voorbeeld sjablonen

In de volgende voor beelden ziet u scenario's voor het gebruik van uitvoer.

|Template  |Description  |
|---------|---------|
|[Variabelen kopiëren](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Maakt complexe variabelen en voert deze waarden uit. Implementeert geen resources. |
|[Openbaar IP-adres](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Hiermee maakt u een openbaar IP-adres en voert u de resource-ID uit. |
|[Load Balancer](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Koppelingen naar de vorige sjabloon. Maakt gebruik van de resource-ID in de uitvoer bij het maken van de load balancer. |

## <a name="next-steps"></a>Volgende stappen

* Zie [inzicht krijgen in de structuur en de syntaxis van Azure Resource Manager-sjablonen](template-syntax.md)voor meer informatie over de beschik bare eigenschappen voor uitvoer.
