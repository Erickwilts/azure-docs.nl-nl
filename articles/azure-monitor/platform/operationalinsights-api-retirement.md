---
title: Azure Monitor-API buiten gebruik stellen
description: Beschrijft de buiten gebruiks telling van oudere versies van de OperationalInsights-resource provider-API.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/02/2020
ms.openlocfilehash: cf48e26133326d43754b38df6f3b2caaf7a587ab
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978085"
---
# <a name="operationalinsights-api-version-retirement"></a>OperationalInsights API-versie buiten gebruik stellen
Micro soft biedt een kennisgeving van ten minste twaalf maanden voor het buiten gebruik stellen van een API om de overgang naar een nieuwere/ondersteunde versie te versoepelen. We hebben een nieuwe versie (2020-08-01) uitgebracht voor de Api's van de **OperationalInsights** -resource provider en zullen eerdere API-versies op 31 oktober 2023 buiten gebruik stellen. Aangezien nieuwe functies en functionaliteit en optimalisaties alleen worden toegevoegd aan de huidige API, moet u zo snel mogelijk een upgrade naar de nieuwste API-versie uitvoeren.

Na 31 oktober 2023 worden eerdere Api's-versies van 2020-08-01 niet meer ondersteund in Azure Monitor. Als u liever geen upgrade uitvoert, worden aanvragen die zijn verzonden vanuit eerdere versies, nog steeds verwerkt door de Azure Monitor-service tot en met 31 oktober 2023.

## <a name="migration-steps"></a>Migratiestappen
Afhankelijk van de configuratie methode die u gebruikt, moet u de nieuwe versie bijwerken in **rest** -aanvragen en **Resource Manager-sjablonen**. Volg de onderstaande voor beelden om de API-versie bij te werken:

1. REST API aanvragen gebruiken de API-versie in de URL van de aanvraag. Vervang die versie door de nieuwste versie (2020-08-01), zoals wordt weer gegeven in het volgende voor beeld.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure Resource Manager sjablonen gebruiken de API-versie in de eigenschap **apiVersion** van de resource. Vervang die versie door de nieuwste versie (2020-08-01), zoals wordt weer gegeven in het volgende voor beeld.


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>Volgende stappen

- Zie de [Naslag informatie voor de OperationalInsights-API](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/allversions).
