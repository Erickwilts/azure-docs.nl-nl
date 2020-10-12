---
title: Azure Event Hubs als Event Grid bron
description: Beschrijft de eigenschappen die worden gegeven voor Event hubs-gebeurtenissen met Azure Event Grid
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 960aa1fe7184e1d02d28fdc135907119fee8f123
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86113680"
---
# <a name="azure-event-hubs-as-an-event-grid-source"></a>Azure Event Hubs als Event Grid bron

In dit artikel vindt u de eigenschappen en het schema voor Event hubs-gebeurtenissen.Zie [Azure Event grid-gebeurtenis schema](event-schema.md)voor een inleiding tot gebeurtenis schema's.

## <a name="event-grid-event-schema"></a>Event Grid-gebeurtenisschema

### <a name="available-event-types"></a>Beschik bare gebeurtenis typen

Event Hubs verzendt het gebeurtenis type **micro soft. EventHub. CaptureFileCreated** wanneer een capture-bestand wordt gemaakt.

### <a name="example-event"></a>Voorbeeld gebeurtenis

Deze voorbeeld gebeurtenis toont het schema van een event hubs-gebeurtenis die optreedt wanneer de functie Capture een bestand opslaat: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

### <a name="event-properties"></a>Gebeurtenis eigenschappen

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| onderwerp | tekenreeks | Volledige bronpad naar de bron van de gebeurtenis. Dit veld kan niet worden geschreven. Event Grid biedt deze waarde. |
| onderwerp | tekenreeks | Het door de uitgever gedefinieerde pad naar het gebeurtenisonderwerp. |
| Type | tekenreeks | Een van de geregistreerde gebeurtenistypen voor deze gebeurtenisbron. |
| eventTime | tekenreeks | Het tijdstip waarop de gebeurtenis is gegenereerd op basis van de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Event hub-gebeurtenis gegevens. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema voor de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevens object heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| fileUrl | tekenreeks | Het pad naar het opname bestand. |
| Type | tekenreeks | Het bestands type van het opname bestand. |
| partitionId | tekenreeks | De Shard-ID. |
| sizeInBytes | geheel getal | De bestands grootte. |
| eventCount | geheel getal | Het aantal gebeurtenissen in het bestand. |
| firstSequenceNumber | geheel getal | Het kleinste Volg nummer uit de wachtrij. |
| lastSequenceNumber | geheel getal | Het laatste Volg nummer van de wachtrij. |
| firstEnqueueTime | tekenreeks | De eerste keer van de wachtrij. |
| lastEnqueueTime | tekenreeks | De laatste tijd van de wachtrij. |

## <a name="tutorials-and-how-tos"></a>Zelfstudies en handleidingen

|Titel  |Beschrijving  |
|---------|---------|
| [Zelf studie: stream big data naar een Data Warehouse](event-grid-event-hubs-integration.md) | Wanneer Event Hubs een opname bestand maakt, wordt Event Grid een gebeurtenis naar een functie-app verzonden. De app haalt het opname bestand op en migreert gegevens naar een Data Warehouse. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Wat is Event Grid?](overview.md) voor een inleiding tot Azure Event Grid.
* Zie [Event grid Subscription schema](subscription-creation-schema.md)voor meer informatie over het maken van een Azure Event grid-abonnement.
* Zie [Stream Big data in een Data Warehouse](event-grid-event-hubs-integration.md)voor meer informatie over het afhandelen van Event hubs-gebeurtenissen.