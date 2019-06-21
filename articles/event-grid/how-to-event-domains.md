---
title: Grote gegevenssets in Azure Event Grid-onderwerpen met gebeurtenis domeinen beheren
description: Laat zien hoe u grote sets van Azure Event Grid-onderwerpen beheren en publiceren van gebeurtenissen naar ze met behulp van de gebeurtenis domeinen.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 01/17/2019
ms.openlocfilehash: 0042b0bd8c6ed9e9d253c44151dcf0588c742b48
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137849"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Onderwerpen beheren en publiceren van gebeurtenissen via event-domeinen

Dit artikel wordt beschreven hoe u:

* Maken van een Event Grid-domein
* Abonneren op event grid-onderwerpen
* Een lijst met sleutels
* Gebeurtenissen publiceren aan een domein

Zie voor meer informatie over gebeurtenis domeinen [inzicht in de domeinen van de gebeurtenis voor het beheren van Event Grid-onderwerpen](event-domains.md).

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="create-an-event-domain"></a>Het domein van een gebeurtenis maken

Voor het beheren van grote gegevenssets van onderwerpen, maakt u een gebeurtenis-domein.

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name> \
  -l <location>
```

Gebruik voor PowerShell:

```azurepowershell-interactive
New-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain-name> `
  -Location <location>
```

Is gemaakt, retourneert de volgende waarden:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Houd er rekening mee de `endpoint` en `id` zoals ze zijn vereist voor het beheren van het domein en gebeurtenissen publiceren.

## <a name="manage-access-to-topics"></a>Toegang tot onderwerpen beheren

Beheer van toegang tot onderwerpen wordt uitgevoerd via [roltoewijzing](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli). Roltoewijzing gebruikt op rollen gebaseerd toegangsbeheer voor het beperken van bewerkingen op Azure-resources tot gemachtigde gebruikers met een bepaald bereik.

Event Grid heeft twee ingebouwde rollen, kunt u bepaalde gebruikers om toegang te verlenen over verschillende onderwerpen binnen een domein. Deze rollen zijn `EventGrid EventSubscription Contributor (Preview)`, waarmee voor maken en verwijderen van abonnementen, en `EventGrid EventSubscription Reader (Preview)`, zodat alleen kunt lijst gebeurtenisabonnementen.

De volgende limieten van de Azure CLI-opdracht `alice@contoso.com` te maken en verwijderen van gebeurtenisabonnementen alleen op onderwerp `demotopic1`:

```azurecli-interactive
az role assignment create \
  --assignee alice@contoso.com \
  --role "EventGrid EventSubscription Contributor (Preview)" \
  --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

De volgende PowerShell-opdracht limieten `alice@contoso.com` te maken en verwijderen van gebeurtenisabonnementen alleen op onderwerp `demotopic1`:

```azurepowershell-interactive
New-AzureRmRoleAssignment `
  -SignInName alice@contoso.com `
  -RoleDefinitionName "EventGrid EventSubscription Contributor (Preview)" `
  -Scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

Zie voor meer informatie over het beheren van toegang voor Event Grid bewerkingen [Event Grid-beveiliging en verificatie](./security-authentication.md).

## <a name="create-topics-and-subscriptions"></a>Onderwerpen en abonnementen maken

De Event Grid-service maakt en beheert het desbetreffende onderwerp in een domein op basis van de aanroep voor het maken van een gebeurtenisabonnement voor een onderwerp domein automatisch. Er is geen afzonderlijke stap het maken van een onderwerp in een domein. Op dezelfde manier als een abonnement op de laatste gebeurtenis voor een onderwerp wordt verwijderd, is het onderwerp ook verwijderd.

Abonneren op een onderwerp in een domein is hetzelfde als het abonneren op een andere Azure-resource. Geef de gebeurtenis-ID geretourneerd bij het maken van het domein eerder voor de resource-ID van bron. Als u wilt instellen in het onderwerp dat u zich wilt abonneren, `/topics/<my-topic>` aan het einde van de bron-resource-ID. Geef de gebeurtenis-ID zonder op te geven alle onderwerpen die voor het maken van een domein bereik gebeurtenisabonnement die alle gebeurtenissen in het domein ontvangt.

Normaal gesproken de gebruiker u toegangsrechten voor in dat de voorgaande sectie het abonnement wordt gemaakt. Ter vereenvoudiging van dit artikel maakt u het abonnement. 

Gebruik voor Azure CLI:

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --source-resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" \
  --endpoint https://contoso.azurewebsites.net/api/updates
```

Gebruik voor PowerShell:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" `
  -EventSubscriptionName <event-subscription> `
  -Endpoint https://contoso.azurewebsites.net/api/updates
```

Als u een test-eindpunt voor de gebeurtenissen abonneren nodig hebt, kunt u altijd implementeren een [vooraf gemaakte web-app](https://github.com/Azure-Samples/azure-event-grid-viewer) waarmee de binnenkomende gebeurtenissen worden weergegeven. U kunt uw gebeurtenissen verzenden naar de website van uw test op `https://<your-site-name>.azurewebsites.net/api/updates`.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Machtigingen die zijn ingesteld voor een onderwerp worden opgeslagen in Azure Active Directory en moeten expliciet worden verwijderd. Een gebeurtenisabonnement verwijderen wordt niet intrekken van een gebruikerstoegang voor het maken van gebeurtenisabonnementen als ze schrijftoegang voor een onderwerp hebben.


## <a name="publish-events-to-an-event-grid-domain"></a>Publiceren van gebeurtenissen naar een Event Grid-domein

Publiceren van gebeurtenissen naar een domein is hetzelfde als [publiceren naar een aangepast onderwerp](./post-to-custom-topic.md). Echter, in plaats van publiceren naar het aangepaste onderwerp u alle gebeurtenissen naar het eindpunt van het domein. In de gegevens van de JSON-gebeurtenis geeft u het onderwerp dat u wilt dat de gebeurtenissen om naar te gaan. De volgende reeks gebeurtenissen zou leiden tot gebeurtenis met `"id": "1111"` onderwerp `demotopic1` tijdens gebeurtenis met `"id": "2222"` worden verzonden naar onderwerp `demotopic2`:

```json
[{
  "topic": "demotopic1",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "demotopic2",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Als u het eindpunt van het domein met Azure CLI, gebruikt

```azurecli-interactive
az eventgrid domain show \
  -g <my-resource-group> \
  -n <my-domain>
```

Als u de sleutels voor een domein, gebruikt u:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

Als u het eindpunt van het domein met PowerShell, gebruikt

```azurepowershell-interactive
Get-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Als u de sleutels voor een domein, gebruikt u:

```azurepowershell-interactive
Get-AzureRmEventGridDomainKey `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

En vervolgens met uw favoriete methode van het maken van een HTTP POST naar uw gebeurtenissen publiceren naar uw Event Grid-domein.

## <a name="search-lists-of-topics-or-subscriptions"></a>Zoeken naar een lijst met onderwerpen of abonnementen

Event Grid van API's ondersteunen om te zoeken en beheren van een groot aantal onderwerpen of abonnementen maken, aanbieding en van paginering.

### <a name="using-cli"></a>CLI gebruiken

Voor het gebruik van deze Zorg ervoor dat u de versie van de Azure CLI Event Grid-extensie 0.4.1 of hoger.

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic list \
    --odata-query "contains(name, 'my-test-filter')"
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de geavanceerde concepten in gebeurtenis-domeinen en waarom ze handig zijn, de [conceptueel overzicht van domeinen van de gebeurtenis](event-domains.md).
