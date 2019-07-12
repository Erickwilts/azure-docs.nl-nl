---
title: Stream-gegevens als invoer in Azure Stream Analytics
description: Meer informatie over het instellen van een verbinding in Azure Stream Analytics. Invoer bevatten een gegevensstroom van gebeurtenissen en ook verwijzen naar gegevens.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 1f03f9e68640edd73d2f6bb55cf205a609450658
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620502"
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Stream-gegevens als invoer in Stream Analytics

Stream Analytics is een uitstekende integratie met Azure-gegevensstromen als invoer van drie soorten resources:

- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Deze invoer resources kunnen bevinden zich in hetzelfde Azure-abonnement als uw Stream Analytics-taak of een ander abonnement.

### <a name="compression"></a>Compressie

Stream Analytics biedt ondersteuning voor compressie voor alle stream input gegevensbronnen. Van de ondersteunde compressietypen zijn: None, GZip, en Deflate-compressie. Ondersteuning voor compressie is niet beschikbaar voor referentiegegevens. Als de invoerindeling Avro-gegevens die zijn gecomprimeerd is, wordt dit transparant verwerkt. U hoeft niet te geven van compressietype met Avro-serialisatie. 

## <a name="create-edit-or-test-inputs"></a>Maken, bewerken of invoer testen

U kunt de [Azure-portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md), en [Visual Studio Code](quick-create-vs-code.md) toevoegen en weergeven of bewerken van bestaande invoer op uw streaming-taak. U kunt ook de invoer-verbindingen testen en [query's testen](stream-analytics-manage-job.md#test-your-query) van voorbeeldgegevens uit Azure portal, [Visual Studio](stream-analytics-vs-tools-local-run.md), en [Visual Studio Code](vscode-local-run.md). Wanneer u een query schrijven, kunt u de invoer in de component FROM weergeven. U krijgt de lijst met beschikbare invoer van de **Query** pagina in de portal. Als u gebruiken van meerdere invoergegevens wilt, kunt u `JOIN` ze of meerdere schrijven `SELECT` query's.


## <a name="stream-data-from-event-hubs"></a>Gegevens streamen vanuit Event Hubs

Azure Event Hubs biedt zeer schaalbare gebeurtenis ingestors publiceren / abonneren. Een event hub kan miljoenen gebeurtenissen per seconde verzamelen, zodat u kunt verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door uw verbonden apparaten en toepassingen. Samen bieden Event Hubs en Stream Analytics u een end-to-end oplossing voor realtime analyses. Eventhubs kunt u gebeurtenissen feed in Azure in realtime, en deze gebeurtenissen in realtime kunnen worden verwerkt door Stream Analytics-taken. Bijvoorbeeld, kunt u web klikken, sensorwaarden, serverlogs, of online gebeurtenissen verzenden naar Event Hubs. Vervolgens kunt u Stream Analytics-taken voor het gebruik van Event Hubs als de invoer-gegevensstromen voor realtime filteren, aggregeren en correlatie.

`EventEnqueuedUtcTime` de tijdstempel van de aankomst van een gebeurtenis in een event hub en de standaard-tijdstempel van gebeurtenissen die afkomstig zijn van Gebeurtenishubs met Stream Analytics. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) trefwoord.

### <a name="event-hubs-consumer-groups"></a>Event Hubs-Consumer-groepen

Configureer elke Stream Analytics event hub-gegevens invoeren in een eigen consumentengroep hebben. Wanneer een taak bevat een self-join of is van meerdere invoergegevens, sommige invoer kan worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkele consumergroep. Om te voorkomen dat de Event Hubs-limiet van vijf gebruikers per consumergroep per partitie, maar het is een aanbevolen procedure om een consumentengroep voor elke Stream Analytics-taak toe te wijzen. Er is ook een limiet van 20 consumergroepen voor een Standard-laag event hub. Zie voor meer informatie, [oplossen Azure Stream Analytics-invoer](stream-analytics-troubleshoot-input.md).

### <a name="create-an-input-from-event-hubs"></a>Invoer van Event Hubs maken

De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure portal op de invoer van de stream-gegevens van een event hub:

| Eigenschap | Description |
| --- | --- |
| **Invoeralias** |Een beschrijvende naam die u in de query van de taak gebruiken om te verwijzen naar deze gegevens. |
| **Abonnement** | Kies het abonnement waarin de Event hub-bron bestaat. | 
| **Event Hub-naamruimte** | De Event Hub-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe event hub maakt, maakt u ook de naamruimte. |
| **Event Hub-naam** | De naam van de event hub te gebruiken als invoer. |
| **De naam van een Event Hub-beleid** | Het beleid voor gedeelde toegang dat toegang tot de Event Hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Deze optie wordt automatisch ingevuld, tenzij u de optie voor de Event Hub-instellingen handmatig.|
| **Event Hub-consumentengroep** (aanbevolen) | Het is raadzaam een afzonderlijke consumergroep gebruiken voor elke Stream Analytics-taak. Deze tekenreeks Hiermee geeft u de consumentengroep te gebruiken voor opname van gegevens uit de event hub. Als er geen consumentengroep is opgegeven, wordt in de Stream Analytics-taak de consumentengroep $Default gebruikt.  |
| **Serialisatie-indeling voor gebeurtenissen** | De serialisatie-indeling (JSON, CSV of Avro) van de gegevensstroom inkomende.  Zorg ervoor dat de JSON-indeling worden uitgelijnd met de specificatie en toonaangevende 0 voor decimale getallen bevat geen. |
| **Codering** | UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Gebeurteniscompressietype** | Het compressietype gebruikt om te lezen van de inkomende gegevensstroom, zoals None (standaard), GZip en Deflate. |

Wanneer uw gegevens afkomstig zijn uit een Event Hub-Stroominvoer, hebt u toegang tot de volgende metagegevensvelden in uw Stream Analytics-query:

| Eigenschap | Description |
| --- | --- |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door Stream Analytics. |
| **EventEnqueuedUtcTime** |De datum en tijd waarop de gebeurtenis is ontvangen door Event Hubs. |
| **PartitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query zoals in het volgende voorbeeld:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> Als u Event Hub als een eindpunt voor IoT Hub-Routes, u kunt toegang tot de IoT Hub metagegevens via de [functie GetMetadataPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue).
> 

## <a name="stream-data-from-iot-hub"></a>Stream-gegevens van IoT Hub

Azure IoT Hub is een zeer schaalbare publish-subscribe abonnementsgebeurtenissen die zijn geoptimaliseerd voor IoT-scenario's.

De standaard-tijdstempel van gebeurtenissen die afkomstig zijn van een IoT-Hub in Stream Analytics is de tijdstempel die de gebeurtenis in de IoT-Hub die is ontvangen `EventEnqueuedUtcTime`. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) trefwoord.

### <a name="iot-hub-consumer-groups"></a>IOT Hub Consumer-groepen

Configureer elke Stream Analytics IoT-Hub invoeren in een eigen consumentengroep hebben. Wanneer een taak een self-join bevat of wanneer er meerdere invoergegevens, kan sommige invoer worden gelezen door meer dan één lezer downstream. Deze situatie heeft gevolgen voor het aantal lezers in een enkele consumergroep. Om te voorkomen dat de Azure IoT Hub-limiet van vijf gebruikers per consumergroep per partitie, is het een aanbevolen procedure om een consumentengroep voor elke Stream Analytics-taak toe te wijzen.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Een IoT-Hub configureren als een gegevensstroom invoer

De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure portal als u een IoT-Hub als een stream input configureert.

| Eigenschap | Description |
| --- | --- |
| **Invoeralias** | Een beschrijvende naam die u in de query van de taak gebruiken om te verwijzen naar deze gegevens.|
| **Abonnement** | Kies het abonnement waarin de IoT Hub-bron bestaat. | 
| **IoT Hub** | De naam van de IoT-Hub kunt u gebruiken als invoer. |
| **Endpoint** | Het eindpunt voor de IoT-Hub.|
| **Naam van het gedeelde toegangsbeleid** | Het beleid voor gedeelde toegang dat toegang tot de IoT-Hub biedt. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. |
| **Beleid voor gedeelde toegangssleutel** | De gedeelde toegangssleutel toegang verlenen aan de IoT-Hub.  Deze optie wordt automatisch ingevuld, tenzij u de optie voor de Iot-Hub instellingen handmatig. |
| **Consumentengroep** | Het is zeer raadzaam dat u een andere consumergroep voor elke Stream Analytics-taak gebruiken. De consumergroep wordt gebruikt voor opname van gegevens van de IoT-Hub. Stream Analytics maakt gebruik van de consumentengroep $Default, tenzij u iets anders opgeeft.  |
| **Serialisatie-indeling voor gebeurtenissen** | De serialisatie-indeling (JSON, CSV of Avro) van de gegevensstroom inkomende.  Zorg ervoor dat de JSON-indeling worden uitgelijnd met de specificatie en toonaangevende 0 voor decimale getallen bevat geen. |
| **Codering** | UTF-8 is momenteel de enige ondersteunde coderingsindeling. |
| **Gebeurteniscompressietype** | Het compressietype gebruikt om te lezen van de inkomende gegevensstroom, zoals None (standaard), GZip en Deflate. |


Wanneer u streaminggegevens vanuit een IoT-Hub, hebt u toegang tot de volgende metagegevensvelden in de Stream Analytics-query:

| Eigenschap | Description |
| --- | --- |
| **EventProcessedUtcTime** | De datum en tijd waarop de gebeurtenis is verwerkt. |
| **EventEnqueuedUtcTime** | De datum en tijd waarop de gebeurtenis is ontvangen door de IoT Hub. |
| **PartitionId** | De op nul gebaseerde partitie-ID voor de invoer-adapter. |
| **IoTHub.MessageId** | Een ID die wordt gebruikt voor het correleren van communicatie in twee richtingen in IoT Hub. |
| **IoTHub.CorrelationId** | Een ID die wordt gebruikt voor antwoorden en feedback van IoT-Hub. |
| **IoTHub.ConnectionDeviceId** | De verificatie-ID gebruikt om dit bericht te verzenden. Deze waarde wordt vermeld op servicebound berichten door de IoT Hub. |
| **IoTHub.ConnectionDeviceGenerationId** | De generatie-ID van het geverifieerde apparaat dat is gebruikt om dit bericht te verzenden. Deze waarde wordt vermeld op servicebound berichten door de IoT Hub. |
| **IoTHub.EnqueuedTime** | De tijd waarop het bericht is ontvangen door de IoT Hub. |


## <a name="stream-data-from-blob-storage"></a>Stream-gegevens uit Blob storage
Voor scenario's met grote hoeveelheden ongestructureerde gegevens opslaan in de cloud, biedt Azure Blob-opslag een kosteneffectieve en schaalbare oplossing. Gegevens in Blob storage wordt meestal beschouwd als de gegevens in rust; blob-gegevens kunnen echter worden verwerkt als een gegevensstroom door Stream Analytics. 

De verwerking van logboekbestanden is een veelgebruikte scenario voor het gebruik van Blob storage invoer met Stream Analytics. In dit scenario telemetrie-gegevensbestanden zijn vastgelegd in een systeem en moeten worden geparseerd en verwerkt om bruikbare gegevens te extraheren.

De standaard-tijdstempel van gebeurtenissen van Blob storage in Stream Analytics is het tijdstempel dat de blob het laatst is gewijzigd, dat is `BlobLastModifiedUtcTime`. Voor het verwerken van de gegevens als een stroom met behulp van een tijdstempel in het geval van nettolading, moet u de [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) trefwoord. Een Stream Analytics-taak haalt gegevens op uit Azure Blob storage invoer per seconde als de blobbestand beschikbaar is. Als de blob-bestand niet beschikbaar is, is er een exponentieel uitstel met een maximale vertraging van 90 seconden.

CSV-indeling invoer moeten een veldnamenrij en geef de velden voor de gegevensset en alle koptekstvelden rij moeten uniek zijn.

> [!NOTE]
> Stream Analytics biedt geen ondersteuning voor inhoud toevoegen aan een bestaand blobbestand. Stream Analytics wordt elk bestand slechts één keer weergeven en eventuele wijzigingen die in het bestand plaatsvinden nadat de taak van de gegevens lezen niet worden verwerkt. Beste manier is om alle gegevens voor een blob-bestand tegelijk uploaden en vervolgens aanvullende nieuwere gebeurtenissen toevoegen aan een andere, nieuwe blob-bestand.

Uploaden van een zeer groot aantal blobs in één keer kan leiden tot Stream Analytics voor het lezen van enkele blobs in zeldzame gevallen overslaan. Het verdient aanbeveling ten minste 2 seconden elkaar naar de Blob storage voor blobs uploaden. Als deze optie niet haalbaar is is, kunt u Event Hubs tot grote volumes van de stroom van gebeurtenissen. 

### <a name="configure-blob-storage-as-a-stream-input"></a>Blob-opslag als een stroom invoer configureren 

De volgende tabel bevat uitleg over elke eigenschap in de **nieuwe invoer** pagina in de Azure portal wanneer u blobopslag als een stroom invoer configureren.

| Eigenschap | Description |
| --- | --- |
| **Invoeralias** | Een beschrijvende naam die u in de query van de taak gebruiken om te verwijzen naar deze gegevens. |
| **Abonnement** | Kies het abonnement waarin de IoT Hub-bron bestaat. | 
| **Opslagaccount** | De naam van het opslagaccount waar de blob-bestanden zich bevinden. |
| **Opslagaccountsleutel** | De geheime sleutel die is gekoppeld aan de storage-account. Deze optie wordt automatisch ingevuld, tenzij u de optie voor de Blob-Opslaginstellingen handmatig. |
| **Container** | De container voor de invoer van de blob. Containers bieden een logische groepering van blobs die zijn opgeslagen in de Microsoft Azure Blob-service. Wanneer u een blob geüpload naar de Azure Blob storage-service, moet u een container voor die blob opgeven. U kunt een **Use existing** container of **nieuw** hebben een nieuwe container gemaakt.|
| **Padpatroon** (optioneel) | Het pad dat wordt gebruikt om de blobs in de opgegeven container te vinden. Binnen het pad kunt u een of meer exemplaren van de volgende drie variabelen opgeven: `{date}`, `{time}`, of `{partition}`<br/><br/>Voorbeeld 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Voorbeeld 2: `cluster1/logs/{date}`<br/><br/>De `*` teken is niet een toegestane waarde voor het padvoorvoegsel. Alleen geldige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure blob-tekens</a> zijn toegestaan. Geen geen namen van containers of bestandsnamen. |
| **Datumnotatie** (optioneel) | Als u de datum-variabele in het pad voor de datumnotatie waarin de bestanden zijn ingedeeld gebruiken. Voorbeeld: `YYYY/MM/DD` |
| **Tijdnotatie** (optioneel) |  Als u de variabele in het pad voor de indeling waarin de bestanden zijn ingedeeld. De enige ondersteunde waarde is momenteel `HH` uur. |
| **Serialisatie-indeling voor gebeurtenissen** | De serialisatie-indeling (JSON, CSV of Avro) van de gegevensstroom inkomende.  Zorg ervoor dat de JSON-indeling worden uitgelijnd met de specificatie en toonaangevende 0 voor decimale getallen bevat geen. |
| **Codering** | Voor CSV en JSON is UTF-8 momenteel de enige ondersteunde coderingsindeling. |
| **Compressie** | Het compressietype gebruikt om te lezen van de inkomende gegevensstroom, zoals None (standaard), GZip en Deflate. |

Wanneer uw gegevens afkomstig zijn uit een Blob storage-bron, hebt u toegang tot de volgende metagegevensvelden in uw Stream Analytics-query:

| Eigenschap | Description |
| --- | --- |
| **BlobName** |De naam van de blob die de gebeurtenis afkomstig zijn uit. |
| **EventProcessedUtcTime** |De datum en tijd waarop de gebeurtenis is verwerkt door Stream Analytics. |
| **BlobLastModifiedUtcTime** |De datum en tijd waarop de blob voor het laatst is gewijzigd. |
| **PartitionId** |De op nul gebaseerde partitie-ID voor de invoer-adapter. |

Bijvoorbeeld, kunt met behulp van deze velden, u een query zoals in het volgende voorbeeld:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Snelstart: Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
