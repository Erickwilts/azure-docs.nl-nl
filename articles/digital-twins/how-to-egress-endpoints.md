---
title: Uitgangen en eind punten in azure Digital Apparaatdubbels | Microsoft Docs
description: Richt lijnen voor het maken van eind punten met Azure Digital Apparaatdubbels.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: alinast
ms.openlocfilehash: 3c33992ce3c130d6c06e0709a9c4ddcab4fff159
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69013941"
---
# <a name="egress-and-endpoints"></a>Uitgangen en eind punten

Azure Digital Apparaatdubbels- *eind punten* vertegenwoordigen een bericht of gebeurtenis Broker binnen het Azure-abonnement van een gebruiker. Gebeurtenissen en berichten kunnen worden verzonden naar de onderwerpen over Azure Event Hubs, Azure Event Grid en Azure Service Bus.

Gebeurtenissen worden doorgestuurd naar eind punten volgens de vooraf gedefinieerde voor keuren voor route ring. Gebruikers opgeven welke *gebeurtenis typen* elk eind punt kunnen ontvangen.

Raadpleeg voor meer informatie over gebeurtenissen, route ring en gebeurtenis typen routerings [gebeurtenissen en berichten in azure Digital apparaatdubbels](./concepts-events-routing.md).

## <a name="events"></a>Events

Gebeurtenissen worden verzonden door IoT-objecten (zoals apparaten en Sens oren) voor verwerking door Azure Message en Event brokers. Gebeurtenissen worden gedefinieerd door de volgende [Azure Event grid gebeurtenis schema verwijzing](../event-grid/event-schema.md).

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Kenmerk | type | Description |
| --- | --- | --- |
| id | string | De unieke id voor de gebeurtenis. |
| subject | string | Het door de uitgever gedefinieerde pad naar het gebeurtenis onderwerp. |
| data | object | Gebeurtenis gegevens die specifiek zijn voor de resource provider. |
| eventType | string | Een van de geregistreerde gebeurtenis typen voor deze gebeurtenis bron. |
| eventTime | string | Het tijdstip waarop de gebeurtenis is gegenereerd op basis van de UTC-tijd van de provider. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schema versie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid levert deze waarde. |
| topic | string | Volledige bronpad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid levert deze waarde. |

Voor meer informatie over het Event Grid-gebeurtenis schema:

- Raadpleeg de [Naslag informatie over het Azure Event grid-gebeurtenis schema](../event-grid/event-schema.md).
- Lees de [Naslag informatie voor Azure EventGrid node. js SDK EventGridEvent](https://docs.microsoft.com/javascript/api/azure-eventgrid/eventgridevent?view=azure-node-latest).

## <a name="event-types"></a>Gebeurtenis typen

De typen gebeurtenissen classificeren de aard van de gebeurtenis en worden ingesteld in het veld type. Beschik bare gebeurtenis typen vindt u in de volgende lijst:

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

De gebeurtenis indelingen voor elk gebeurtenis type worden verder beschreven in de volgende subsecties.

### <a name="topologyoperation"></a>TopologyOperation

**TopologyOperation** is van toepassing op de grafiek wijzigingen. De eigenschap **subject** geeft het type object aan dat wordt beïnvloed. Deze gebeurtenis kan worden geactiveerd door de volgende typen objecten:

- Apparaat
- DeviceBlobMetadata
- DeviceExtendedProperty
- ExtendedPropertyKey
- Extended type
- KeyStore
- Rapport
- RoleDefinition
- Sensor
- SensorBlobMetadata
- SensorExtendedProperty
- Spatiebalk
- SpaceBlobMetadata
- SpaceExtendedProperty
- SpaceResource
- SpaceRoleAssignment
- Systeem
- Gebruiker
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Value | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom** is een gebeurtenis die door een door de gebruiker gedefinieerde functie (UDF) wordt verzonden.
  
> [!IMPORTANT]  
> Deze gebeurtenis moet expliciet worden verzonden vanuit de UDF zelf.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "568fd394-380b-46fa-925a-ebb96f658cce",
  "subject": "UdfCustom",
  "data": {
    "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "ResourceType": "Space",
    "Payload": "\"Room is not available or air quality is poor\"",
    "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
  },
  "eventType": "UdfCustom",
  "eventTime": "2018-10-02T06:50:15.198Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Value | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

### <a name="sensorchange"></a>SensorChange

**SensorChange** is een update van de status van een sensor op basis van wijzigingen in telemetrie.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
  "subject": "SensorChange",
  "data": {
    "Type": "Classic",
    "DataType": "Motion",
    "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "Value": "False",
    "PreviousValue": "True",
    "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
    "MessageType": "sensor",
    "Properties": {
      "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
      "ms-activity-id": "ct22YwXEGJ5u.605.0"
    }
  },
  "eventType": "SensorChange",
  "eventTime": "2018-04-17T17:46:18.5452993Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Value | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

### <a name="spacechange"></a>SpaceChange

**SpaceChange** is een update van de status van een ruimte op basis van wijzigingen in telemetrie.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
  "subject": "SpaceChange",
  "data": {
    "Type": null,
    "DataType": "AvailableAndFresh",
    "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "Value": "Room is not available or air quality is poor",
    "PreviousValue": null,
    "RawData": null,
    "transactionId": null,
    "EventTimestamp": null,
    "MessageType": null,
    "Properties": null,
    "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
  },
  "eventType": "SpaceChange",
  "eventTime": "2018-10-02T06:50:20.128Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Value | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

### <a name="devicemessage"></a>DeviceMessage

Met behulp van **DeviceMessage**kunt u een **EventHub** -verbinding opgeven waarmee onbewerkte telemetriegegevens ook kunnen worden gerouteerd vanuit Azure Digital apparaatdubbels.

> [!NOTE]
> - **DeviceMessage** is alleen Combineer bare met **EventHub**. U kunt **DeviceMessage** niet combi neren met een van de andere gebeurtenis typen.
> - U kunt slechts één eind punt opgeven van de combi natie van het type **EventHub** of **DeviceMessage**.

## <a name="configure-endpoints"></a>Eindpunten configureren

Eindpunt beheer wordt uitgeoefend via de eind punt-API.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

De volgende voor beelden laten zien hoe u de ondersteunde eind punten kunt configureren.

>[!IMPORTANT]
> Betaal voorzichtig met het kenmerk **eventTypes** . Hiermee wordt gedefinieerd welke gebeurtenis typen door het eind punt worden verwerkt en bepalen ze de route ring.

Een geverifieerde HTTP POST-aanvraag voor:

```plaintext
YOUR_MANAGEMENT_API_URL/endpoints
```

- Route naar Service Bus gebeurtenis typen **SensorChange**, **SpaceChange**en **TopologyOperation**:

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME"
  }
  ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naam ruimte van uw eind punt |
    | YOUR_PRIMARY_KEY | De primaire connection string gebruikt voor verificatie |
    | YOUR_SECONDARY_KEY | De secundaire connection string gebruikt voor verificatie |
    | YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

- Route naar Event Grid gebeurtenis typen **SensorChange**, **SpaceChange**en **TopologyOperation**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME.westus-1.eventgrid.azure.net"
  }
  ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_PRIMARY_KEY | De primaire connection string gebruikt voor verificatie|
    | YOUR_SECONDARY_KEY | De secundaire connection string gebruikt voor verificatie |
    | YOUR_TOPIC_NAME | De naam van het aangepaste onderwerp |

- Route naar Event Hubs gebeurtenis typen **SensorChange**, **SpaceChange**en **TopologyOperation**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naam ruimte van uw eind punt |
    | YOUR_PRIMARY_KEY | De primaire connection string gebruikt voor verificatie |
    | YOUR_SECONDARY_KEY | De secundaire connection string gebruikt voor verificatie |
    | YOUR_EVENT_HUB_NAME | De naam van uw Event Hub |

- Route naar Event Hubs gebeurtenis type **DeviceMessage**. De opname van `EntityPath` in de **Connections Tring** is verplicht:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naam ruimte van uw eind punt |
    | YOUR_PRIMARY_KEY | De primaire connection string gebruikt voor verificatie |
    | YOUR_SECONDARY_KEY | De secundaire connection string gebruikt voor verificatie |
    | YOUR_EVENT_HUB_NAME | De naam van uw Event Hub |

> [!NOTE]  
> Na het maken van een nieuw eind punt kan het tot vijf tot tien minuten duren voordat gebeurtenissen worden ontvangen op het eind punt.

## <a name="primary-and-secondary-connection-keys"></a>Primaire en secundaire verbindings sleutels

Wanneer een primaire verbindings sleutel niet-geautoriseerd wordt, probeert het systeem automatisch de secundaire verbindings sleutel. Hiermee wordt een back-up gemaakt en kunt u de primaire sleutel zonder problemen verifiëren en bijwerken via de eind punt-API.

Als zowel de primaire als secundaire verbindings sleutel niet-geautoriseerd zijn, voert het systeem een exponentiële wacht tijd van Maxi maal 30 minuten in. Gebeurtenissen worden verwijderd op elke geactiveerde time-outtijd.

Wanneer het systeem zich in een wacht tijd bevindt, kan het tot 30 minuten duren voordat de verbindings sleutels via de eind punten van de API worden bijgewerkt.

## <a name="unreachable-endpoints"></a>Onbereikbare eind punten

Wanneer een eind punt onbereikbaar wordt, voert het systeem een exponentiële wacht tijd van Maxi maal 30 minuten in. Gebeurtenissen worden verwijderd op elke geactiveerde time-outtijd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie [over het gebruik van Azure Digital Apparaatdubbels Swagger](how-to-use-swagger.md).

- Meer informatie over [route ring van gebeurtenissen en berichten](concepts-events-routing.md) in azure Digital apparaatdubbels.
