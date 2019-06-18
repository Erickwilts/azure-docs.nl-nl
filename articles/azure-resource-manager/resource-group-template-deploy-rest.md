---
title: Resources implementeren met REST-API en een sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager en Resource Manager REST API voor het implementeren van resources naar Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: tomfitz
ms.openlocfilehash: 0490cf6837cb413bc2e869424cd430fd4a824dc9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66688866"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API

In dit artikel wordt uitgelegd hoe u de Resource Manager REST-API gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources naar Azure.  

U kunt uw sjabloon opnemen in de hoofdtekst van de aanvraag of een koppeling naar een bestand. Wanneer u een bestand, kan een lokaal bestand of een extern bestand dat is beschikbaar via een URI zijn. Als de sjabloon in een storage-account is, kunt u beperken van toegang aan de sjabloon en de token van een shared access signature (SAS) opgeven tijdens de implementatie.

## <a name="deployment-scope"></a>Reikwijdte van de implementatie

U kunt uw implementatie om een beheergroep, een Azure-abonnement of resourcegroep doelgroep. In de meeste gevallen zult u richt de implementaties voor een resourcegroep. Management-groep of abonnement implementaties gebruiken voor het toepassen van beleid en de roltoewijzingen voor het opgegeven bereik. U kunt ook abonnementimplementaties gebruiken een resourcegroep maken en implementeren van resources toe. Afhankelijk van het bereik van de implementatie, kunt u verschillende opdrachten gebruiken.

Om te implementeren op een **resourcegroep**, gebruikt u [implementaties - maken](/rest/api/resources/deployments/createorupdate). De aanvraag wordt verzonden naar:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Om te implementeren op een **abonnement**, gebruikt u [implementaties - Scope maken op abonnement](/rest/api/resources/deployments/createorupdateatsubscriptionscope). De aanvraag wordt verzonden naar:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Om te implementeren op een **beheergroep**, gebruikt u [implementaties - maken op Management groepsbereik](/rest/api/resources/deployments/createorupdateatmanagementgroupscope). De aanvraag wordt verzonden naar:

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

De voorbeelden in dit artikel gebruikt brongroepimplementaties. Zie voor meer informatie over de implementatie van abonnement [maken van resourcegroepen en resources op het abonnementsniveau](deploy-to-subscription.md).

## <a name="deploy-with-the-rest-api"></a>Implementeren met de REST-API

1. Stel [algemene parameters en headers](/rest/api/azure/), met inbegrip van verificatietokens.

1. Als u een bestaande resourcegroep hebt, maakt u een resourcegroep. Geef uw abonnements-ID, de naam van de nieuwe resourcegroep en locatie die u nodig hebt voor uw oplossing. Zie voor meer informatie, [een resourcegroep maken](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
   ```

   Met een aanvraagtekst, zoals:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Controleren of uw implementatie voordat deze wordt uitgevoerd door het uitvoeren van de [controleren of de sjabloonimplementatie van een](/rest/api/resources/deployments/validate) bewerking. Bij het testen van de implementatie, parameters opgeven precies zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

1. Voor het implementeren van een sjabloon, Geef uw abonnements-ID, de naam van de resourcegroep, de naam van de implementatie in de aanvraag-URI. 

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   Geef een koppeling naar uw sjabloon en de parameterbestanden-bestand in de aanvraagtekst. U ziet dat de **modus** is ingesteld op **incrementele**. Als u wilt uitvoeren van een volledige implementatie, **modus** naar **voltooid**. Wees voorzichtig bij het gebruik van de volledige-modus, kunt u per ongeluk resources die zich niet in uw sjabloon verwijderen.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Als u wilt vastleggen van de inhoud van de reactie en/of de inhoud van de aanvraag, **debugSetting** in de aanvraag.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    U kunt uw storage-account instellen om te gebruiken van een token shared access signature (SAS). Zie voor meer informatie, [toegang delegeren met een Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

1. In plaats van koppelingen naar bestanden voor de sjabloon en parameters, kunt u ze opneemt in de aanvraagtekst. Het volgende voorbeeld ziet u de hoofdtekst van de aanvraag met de sjabloon en de parameterbestanden inline:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Als u de status van de sjabloonimplementatie, gebruikt [implementaties - ophalen](/rest/api/resources/deployments/get).

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Deze functie wordt ook wel bekend als *Rollback bij fout*. Wanneer een implementatie is mislukt, kunt u automatisch opnieuw implementeren de implementatie van een eerdere, geslaagde geschiedenis van uw implementatie. Opnieuw implementeren, gebruikt u de `onErrorDeployment` eigenschap in de aanvraagtekst. Deze functionaliteit is handig als u een bekende goede status voor de Infrastructuurimplementatie van uw hebt en wilt terugkeren naar deze status. Er zijn een aantal aanvullende opmerkingen en beperkingen:

- Het opnieuw distribueren wordt uitgevoerd, precies zoals deze eerder is uitgevoerd met dezelfde parameters. U kunt de parameters niet wijzigen.
- De vorige implementatie wordt uitgevoerd met behulp van de [volledige modus](./deployment-modes.md#complete-mode). Alle resources die niet zijn opgenomen in de vorige implementatie worden verwijderd en de resourceconfiguraties zijn ingesteld op de vorige status. Zorg ervoor dat u volledig inzicht in de [implementatiemodi](./deployment-modes.md).
- Het opnieuw implementeren zijn alleen van invloed op de resources en eventuele wijzigingen in gegevens worden niet beïnvloed.
- Deze functie wordt alleen ondersteund voor implementaties van de resourcegroep, geen abonnement op implementaties. Zie voor meer informatie over de implementatie voor het niveau van abonnement [maken van resourcegroepen en resources op het abonnementsniveau](./deploy-to-subscription.md).

Als u wilt deze optie gebruikt, moeten uw implementaties unieke namen hebben, zodat ze kunnen worden geïdentificeerd in de geschiedenis. Als u geen unieke namen, overschrijft de huidige mislukte implementatie mogelijk de eerder geslaagde implementatie in de geschiedenis. U kunt deze optie alleen gebruiken met niveau root-implementaties. Implementaties van een geneste sjabloon zijn niet beschikbaar voor opnieuw implementeren.

Als u wilt implementeren de laatste geslaagde implementatie als de huidige implementatie mislukt, gebruikt u:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

Als u wilt een specifieke implementatie opnieuw implementeren als de huidige implementatie mislukt, gebruikt u:

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

De opgegeven implementatie moet zijn geslaagd.

## <a name="parameter-file"></a>Parameterbestand

Als u een parameterbestand parameterwaarden doorgeven tijdens de implementatie hebt gebruikt, moet u een JSON-bestand maken met een indeling die vergelijkbaar is met het volgende voorbeeld:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               },
               "secretName": "sqlAdminPassword"
            }
        }
   }
}
```

De grootte van de parameterbestand mag niet meer dan 64 KB.

Als u wilt een gevoelige waarde opgeven voor een parameter (zoals een wachtwoord), moet u die waarde toevoegen aan een key vault. De key vault ophalen tijdens de implementatie, zoals wordt weergegeven in het vorige voorbeeld. Zie voor meer informatie, [beveiligde waarden doorgeven tijdens implementatie](resource-manager-keyvault-parameter.md). 

## <a name="next-steps"></a>Volgende stappen

- Als u wilt opgeven voor het verwerken van resources die aanwezig zijn in de resourcegroep, maar niet zijn gedefinieerd in de sjabloon, Zie [Azure Resource Manager-implementatiemodi](deployment-modes.md).
- Zie voor meer informatie over het verwerken van asynchrone REST-bewerkingen, [asynchrone bewerkingen van Azure bijhouden](resource-manager-async-operations.md).
- Zie voor meer informatie over sjablonen, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).

