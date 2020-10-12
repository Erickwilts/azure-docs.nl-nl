---
title: Actie groepen maken met Resource Manager-sjablonen
description: Meer informatie over het maken van een actie groep met behulp van een Azure Resource Manager sjabloon.
author: dkamstra
services: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 5c02812d4c97b94667fcddcb275243e7a9b36b29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87321901"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Een actie groep maken met een resource manager-sjabloon
In dit artikel wordt beschreven hoe u een [Azure Resource Manager sjabloon](../../azure-resource-manager/templates/template-syntax.md) gebruikt om actie groepen te configureren. Met behulp van sjablonen kunt u automatisch actie groepen instellen die opnieuw kunnen worden gebruikt in bepaalde typen waarschuwingen. Deze actie groepen zorgen ervoor dat alle juiste partijen worden gewaarschuwd wanneer een waarschuwing wordt geactiveerd.

De basis stappen zijn:

1. Maak een sjabloon als een JSON-bestand waarin wordt beschreven hoe u de actie groep maakt.

2. Implementeer de sjabloon met behulp van [een implementatie methode](../../azure-resource-manager/templates/deploy-powershell.md).

Eerst beschrijven we hoe u een resource manager-sjabloon maakt voor een actie groep waar de actie definities in de sjabloon worden vastgelegd. Ten tweede beschrijven we hoe u een sjabloon maakt die de informatie over de configuratie van de webhook als invoer parameters gebruikt wanneer de sjabloon wordt geïmplementeerd.

## <a name="resource-manager-templates-for-an-action-group"></a>Resource Manager-sjablonen voor een actie groep

Als u een actie groep wilt maken op basis van een resource manager-sjabloon, maakt u een resource van het type `Microsoft.Insights/actionGroups` . Vervolgens vult u alle gerelateerde eigenschappen in. Hier volgen twee voorbeeld sjablonen die een actie groep maken.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [actiegroepen](./action-groups.md).
* Meer informatie over [waarschuwingen](alerts-overview.md).
* Meer informatie over het toevoegen [van waarschuwingen met behulp van een resource manager-sjabloon](./alerts-activity-log.md).

