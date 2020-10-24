---
title: Eind punten en routes beheren (Api's en CLI)
titleSuffix: Azure Digital Twins
description: Zie eind punten en gebeurtenis routes instellen en beheren voor Azure Digital Apparaatdubbels-gegevens.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 10/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: ce922e3ce39bc3df9f4c242558644922e5713300
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494812"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Eind punten en routes beheren in azure Digital Apparaatdubbels (Api's en CLI)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

In azure Digital Apparaatdubbels kunt u [gebeurtenis meldingen](how-to-interpret-event-data.md) naar downstream-Services of verbonden reken bronnen sturen. Dit wordt gedaan door eerst **eind punten** in te stellen die de gebeurtenissen kunnen ontvangen. U kunt vervolgens  [**gebeurtenis routes**](concepts-route-events.md) maken om op te geven welke gebeurtenissen worden gegenereerd door Azure Digital apparaatdubbels naar welke eind punten worden verzonden.

Eind punten en routes kunnen worden beheerd met de [gebeurtenis routes api's](/rest/api/digital-twins/dataplane/eventroutes), de [.NET-SDK (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet-preview&preserve-view=true)of de [Azure Digital apparaatdubbels cli](how-to-use-cli.md). Dit artikel begeleidt u bij het proces van het maken van eind punten en routes via deze mechanismen.

Ze kunnen ook worden beheerd via de [Azure Portal](https://portal.azure.com). Voor een versie van dit artikel die gebruikmaakt van de portal, Zie [*How-to: Manage endpoints and routes (Portal)*](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Vereisten

* U hebt een **Azure-account** nodig (u kunt deze [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)gratis instellen)
* U hebt een **Azure Digital apparaatdubbels-exemplaar** in uw Azure-abonnement nodig. Als u nog geen exemplaar hebt, kunt u er een maken met behulp van de stappen in [*How-to: een instantie en verificatie instellen*](how-to-set-up-instance-portal.md). Laat de volgende waarden van Setup handig zijn om later in dit artikel te gebruiken:
    - Exemplaarnaam
    - Resourcegroep
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>Een eind punt maken voor Azure Digital Apparaatdubbels

Dit zijn de ondersteunde typen eind punten die u voor uw exemplaar kunt maken:
* [Event Grid](../event-grid/overview.md)
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Zie [*kiezen tussen Azure Messa ging Services*](../event-grid/compare-messaging-services.md)voor meer informatie over de verschillende typen eind punten.

Als u een eind punt wilt koppelen aan Azure Digital Apparaatdubbels, moet u het gebeurtenis grid-onderwerp, Event Hub of Service Bus die u voor het eind punt gebruikt, al bestaan. 

### <a name="create-an-event-grid-endpoint"></a>Een Event Grid-eind punt maken

In het volgende voor beeld ziet u hoe u een event grid-type-eind punt maakt met behulp van de Azure CLI. U kunt [Azure Cloud shell](https://shell.azure.com)gebruiken of [de CLI lokaal installeren](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

Eerst maakt u een event grid-onderwerp. U kunt de volgende opdracht gebruiken of de stappen in meer detail weer geven door naar [de sectie *een aangepast onderwerp maken* ](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) van de Snelstartgids Event grid *aangepaste gebeurtenissen* te gaan.

```azurecli-interactive
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Voer deze opdracht uit om een lijst uit te voeren van Azure-regionamen die kunnen worden doorgegeven aan opdrachten in de Azure CLI:
> ```azurecli-interactive
> az account list-locations -o table
> ```

Zodra u het onderwerp hebt gemaakt, kunt u dit koppelen aan Azure Digital Apparaatdubbels met de volgende [Azure Digital APPARAATDUBBELS cli-opdracht](how-to-use-cli.md):

```azurecli-interactive
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Het onderwerp Event grid is nu beschikbaar als een eind punt in azure Digital Apparaatdubbels, onder de naam die is opgegeven met het `--endpoint-name` argument. Normaal gesp roken gebruikt u die naam als het doel van een **gebeurtenis route**, die u [later in dit artikel](#create-an-event-route) maakt met behulp van de Azure Digital apparaatdubbels Service-API.

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>Een Event Hubs-of Service Bus-eind punt maken

Het proces voor het maken van Event Hubs-of Service Bus-eind punten is vergelijkbaar met het hierboven weer gegeven Event Grid proces.

Maak eerst uw resources die u als eind punt gaat gebruiken. Wat is er vereist:
* Service Bus: _Service Bus naam ruimte_, _Service Bus onderwerp_, _autorisatie regel_
* Event Hubs: _Event hubs naam ruimte_, _Event hub_, _autorisatie regel_

Gebruik vervolgens de volgende opdrachten om de eind punten in azure Digital Apparaatdubbels te maken: 

* Service Bus-onderwerp toevoegen-eind punt (hiervoor is een vooraf gemaakte Service Bus resource vereist)
```azurecli-interactive 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Event Hubs eind punt toevoegen (hiervoor is vooraf gemaakte Event Hubs resource vereist)
```azurecli-interactive
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

### <a name="create-an-endpoint-with-dead-lettering"></a>Een eind punt maken met onbestelbare berichten

Wanneer een eind punt een gebeurtenis binnen een bepaalde tijds periode niet kan leveren of nadat de gebeurtenis een bepaald aantal keren is geprobeerd, kan de gebeurtenis worden verzonden naar een opslag account. Dit proces wordt **onbestelbare berichten**genoemd.

Als u een eind punt wilt maken waarvoor onbestelbare berichten zijn ingeschakeld, moet u de [arm-api's](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) gebruiken om uw eind punt te maken. 

Voordat u de locatie van de onbestelbare letter instelt, moet u een opslag account hebben met een container. U geeft de URL voor deze container op wanneer u het eind punt maakt. De onbestelbare letter wordt gegeven als container-URL met een SAS-token. Dit token heeft alleen `write` machtigingen nodig voor de doel container in het opslag account. De volledig opgemaakte URL heeft de volgende indeling: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`

Zie voor meer informatie over SAS-tokens: [beperkte toegang verlenen tot Azure storage resources met behulp van Shared Access signatures (SAS)](/azure/storage/common/storage-sas-overview)

Zie voor meer informatie over onbestelbare berichten [*concepten: gebeurtenis routes*](concepts-route-events.md#dead-letter-events).

#### <a name="configuring-the-endpoint"></a>Het eind punt configureren

Wanneer u een eind punt maakt, voegt `deadLetterSecret` u een toe aan het `properties` object in de hoofd tekst van de aanvraag, die een container-URL en een SAS-token voor uw opslag account bevat.

```json
{
  "properties": {
    "endpointType": "EventGrid",
    "TopicEndpoint": "https://contosoGrid.westus2-1.eventgrid.azure.net/api/events",
    "accessKey1": "xxxxxxxxxxx",
    "accessKey2": "xxxxxxxxxxx",
    "deadLetterSecret":"https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>"
  }
}
```

Zie de Azure Digital Apparaatdubbels REST API documentation: [endpoints-DigitalTwinsEndpoint CreateOrUpdate](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate)voor meer informatie.

### <a name="message-storage-schema"></a>Schema voor bericht opslag

Onbestelbare berichten worden opgeslagen in de volgende indeling in uw opslag account:

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

Onbestelbare berichten komen overeen met het schema van de oorspronkelijke gebeurtenis die is bedoeld om aan uw oorspronkelijke eind punt te worden geleverd.

Hier volgt een voor beeld van een bericht met een onbestelbare melding voor een dubbele taak voor het maken van een [waarschuwing](how-to-interpret-event-data.md#digital-twin-life-cycle-notifications):

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="create-an-event-route"></a>Een gebeurtenis route maken

Als u gegevens daad werkelijk van Azure Digital Apparaatdubbels naar een eind punt wilt verzenden, moet u een **gebeurtenis route**definiëren. Met Azure Digital Apparaatdubbels **EventRoutes api's** kunnen ontwikkel aars de gebeurtenis stroom, in het hele systeem en op downstream-Services, interactiviteit. Lees meer over gebeurtenis routes in [*concepten: route ring van Azure Digital apparaatdubbels-gebeurtenissen*](concepts-route-events.md).

In de voor beelden in deze sectie wordt de [.net (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet-preview&preserve-view=true)gebruikt.

Voor **waarde: u**moet eind punten maken zoals eerder in dit artikel wordt beschreven voordat u kunt verdergaan om een route te maken. U kunt door gaan met het maken van een gebeurtenis route wanneer de eind punten zijn ingesteld.

>[!NOTE]
>Als u onlangs uw eind punten hebt geïmplementeerd, controleert u of de implementatie is voltooid **voordat** u deze voor een nieuwe gebeurtenis route probeert te gebruiken. Als de implementatie van de route mislukt omdat de eind punten niet gereed zijn, wacht u enkele minuten en probeert u het opnieuw.
>
> Als u deze stroom bijwerkt, kunt u hiervoor het beste een account maken van 2-3 minuten wacht tijd voor de eindpunt service om de implementatie te volt ooien voordat u doorgaat met de installatie van.

### <a name="creation-code-with-apis-and-the-c-sdk"></a>Code maken met Api's en de C#-SDK

Gebeurtenis routes worden gedefinieerd met behulp van [Data plan-api's](how-to-use-apis-sdks.md#overview-data-plane-apis). 

Een route definitie kan deze elementen bevatten:
* De naam van de route die u wilt gebruiken
* De naam van het eind punt dat u wilt gebruiken
* Een filter dat definieert welke gebeurtenissen naar het eind punt worden verzonden 

Als er geen route naam is, worden er geen berichten meer doorgestuurd buiten Azure Digital Apparaatdubbels. Als er een route naam en het filter is `true` , worden alle berichten naar het eind punt doorgestuurd. Als er een route naam en een ander filter worden toegevoegd, worden berichten gefilterd op basis van het filter.

Voor één route moeten meerdere meldingen en gebeurtenis typen worden geselecteerd. 

`CreateEventRoute` is de SDK-aanroep die wordt gebruikt om een gebeurtenis route toe te voegen. Hier volgt een voor beeld van het gebruik:

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter = "true"; //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> Alle SDK-functies zijn beschikbaar in synchrone en asynchrone versies.

### <a name="event-route-sample-code"></a>Voorbeeld code voor gebeurtenis routering

In het volgende code voorbeeld ziet u hoe u een gebeurtenis route kunt maken, weer geven en verwijderen:
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointName} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

## <a name="filter-events"></a>Gebeurtenissen filteren

Als u geen filtert, ontvangen eind punten diverse gebeurtenissen van Azure Digital Apparaatdubbels:
* Telemetrie die wordt geactiveerd door [Digital apparaatdubbels](concepts-twins-graph.md) met de Azure Digital APPARAATDUBBELS Service API
* Dubbele eigenschaps wijzigings meldingen, die worden geactiveerd bij wijzigingen in de eigenschappen voor elke dubbele in het Azure Digital Apparaatdubbels-exemplaar
* Levenscyclus gebeurtenissen, die worden geactiveerd wanneer apparaatdubbels of relaties worden gemaakt of verwijderd

U kunt de verzonden gebeurtenissen beperken door een **filter** voor een eind punt toe te voegen aan de route van uw gebeurtenis.

Als u een filter wilt toevoegen, kunt u een PUT-aanvraag naar *https://{YourHost}/EventRoutes/myNewRoute? API-Version = 2020-10-31* gebruiken met de volgende hoofd tekst:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Dit zijn de ondersteunde route filters. Gebruik de details in de kolom *filter tekst schema* om de `<filter-text>` tijdelijke aanduiding te vervangen in de hoofd tekst van de aanvraag.

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>Eind punten en routes beheren met CLI

Eind punten en routes kunnen ook worden beheerd met behulp van de Azure Digital Apparaatdubbels CLI. Zie [*How-to: use the Azure Digital APPARAATDUBBELS cli*](how-to-use-cli.md)(Engelstalig) voor meer informatie over het gebruik van de CLI en de opdrachten die beschikbaar zijn.

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende typen gebeurtenis berichten die u kunt ontvangen:
* [*Instructies: gebeurtenis gegevens interpreteren*](how-to-interpret-event-data.md)