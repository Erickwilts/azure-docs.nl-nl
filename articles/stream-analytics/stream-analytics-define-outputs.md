---
title: Uitvoer van Azure Stream Analytics
description: In dit artikel worden de opties voor gegevens uitvoer beschreven die beschikbaar zijn voor Azure Stream Analytics.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.custom: contperfq1
ms.date: 10/2/2020
ms.openlocfilehash: 95607b78ff80566b76b8e6aa20462957249015b4
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097648"
---
# <a name="outputs-from-azure-stream-analytics"></a>Uitvoer van Azure Stream Analytics

Een Azure Stream Analytics-taak bestaat uit een invoer, query en een uitvoer. Er zijn verschillende uitvoer typen waarnaar u getransformeerde gegevens kunt verzenden. In dit artikel vindt u een overzicht van de ondersteunde Stream Analytics uitvoer. Wanneer u uw Stream Analytics query ontwerpt, raadpleegt u de naam van de uitvoer met behulp van de [component into](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics). U kunt één uitvoer per taak of meerdere uitvoer per streaming taak gebruiken (als u deze nodig hebt) door meerdere INTO-componenten aan de query toe te voegen.

Als u Stream Analytics taak uitvoer wilt maken, bewerken en testen, kunt u de [Azure Portal](stream-analytics-quick-create-portal.md#configure-job-output), [Azure POWERSHELL](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.net API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [rest API](https://docs.microsoft.com/rest/api/streamanalytics/)en [Visual Studio](stream-analytics-quick-create-vs.md)gebruiken.

Sommige typen uitvoer ondersteunen [partitionering](#partitioning)en [uitvoer batch formaten](#output-batch-size) zijn afhankelijk van de door voer te optimaliseren. De volgende tabel bevat de functies die worden ondersteund voor elk uitvoer type:

| Uitvoertype | Partitionering | Beveiliging | 
|-------------|--------------|----------|
|[Azure Data Lake Storage Gen 1](azure-data-lake-storage-gen1-output.md)|Ja|Azure Active Directory-gebruiker </br> MSI|
|[Azure SQL Database](sql-database-output.md)|Ja, optioneel.|SQL-gebruikers authenticatie </br> MSI (preview-versie)|
|[Azure Synapse Analytics](azure-synapse-analytics-output.md)|Ja|SQL-gebruikers authenticatie|
|[Blob-opslag en Azure Data Lake gen 2](blob-storage-azure-data-lake-gen2-output.md)|Ja|MSI </br> Toegangssleutel|
|[Azure Event Hubs](event-hubs-output.md)|Ja, de partitie sleutel kolom moet worden ingesteld in de uitvoer configuratie.|Toegangssleutel|
|[Power BI](power-bi-output.md)|Nee|Azure Active Directory-gebruiker </br> MSI|
|[Azure Table storage](table-storage-output.md)|Ja|Accountsleutel|
|[Azure Service Bus-wachtrijen](service-bus-queues-output.md)|Ja|Toegangssleutel|
|[Azure Service Bus onderwerpen](service-bus-topics-output.md)|Ja|Toegangssleutel|
|[Azure Cosmos DB](azure-cosmos-db-output.md)|Ja|Toegangssleutel|
|[Azure Functions](azure-functions-output.md)|Ja|Toegangssleutel|

## <a name="partitioning"></a>Partitionering

Stream Analytics ondersteunt partities voor alle uitvoer, met uitzonde ring van Power BI. Voor meer informatie over partitie sleutels en het aantal uitvoer schrijvers raadpleegt u het artikel voor het specifieke uitvoer type waarin u bent geïnteresseerd. Alle uitvoer artikelen zijn gekoppeld in de vorige sectie.  

Voor een geavanceerde afstemming van de partities kan ook het aantal uitvoer schrijvers worden beheerd met behulp van een `INTO <partition count>` ( [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)Zie) component in uw query. Dit kan handig zijn bij het bereiken van een gewenste taak topologie. Als uw uitvoer adapter niet is gepartitioneerd, veroorzaakt een gebrek aan gegevens in één invoer partitie een vertraging tot de duur van de late aankomst. In dergelijke gevallen wordt de uitvoer samengevoegd met één schrijver, wat kan leiden tot knel punten in de pijp lijn. Zie [Azure stream Analytics overwegingen voor gebeurtenis orders](stream-analytics-out-of-order-and-late-events.md)voor meer informatie over het beleid voor late ontvangst.

## <a name="output-batch-size"></a>Grootte van uitvoer batch

Alle uitvoer bewerkingen ondersteunen batch verwerking, maar slechts een bepaalde ondersteunings Batch grootte expliciet. Azure Stream Analytics gebruikt batches van het formaat variable om gebeurtenissen te verwerken en te schrijven naar uitvoer. Normaal gesp roken schrijft de Stream Analytics engine niet één bericht tegelijk en worden batches gebruikt voor efficiëntie. Wanneer het aantal binnenkomende en uitgaande gebeurtenissen hoog is, gebruikt Stream Analytics grotere batches. Wanneer het uitvoer bedrag laag is, worden kleinere batches gebruikt om latentie laag te blijven.

## <a name="parquet-output-batching-window-properties"></a>Eigenschappen van Parquet-uitvoer Batch venster

Wanneer u Azure Resource Manager-sjabloon implementatie of het REST API gebruikt, zijn de twee eigenschappen van het batch venster:

1. *timeWindow*

   De maximale wacht tijd per batch. De waarde moet een teken reeks van time span zijn. Bijvoorbeeld "00:02:00" voor twee minuten. Na deze periode wordt de batch naar de uitvoer geschreven, zelfs als niet aan de vereiste voor de minimum rijen wordt voldaan. De standaard waarde is 1 minuut en het toegestane maximum is 2 uur. Als uw BLOB-uitvoer een patroon frequentie voor het pad heeft, kan de wacht tijd niet hoger zijn dan het tijds bereik van de partitie.

2. *sizeWindow*

   Het aantal minimum rijen per batch. Voor Parquet maakt elke batch een nieuw bestand. De huidige standaard waarde is 2.000 rijen en het toegestane maximum is 10.000 rijen.

Deze batch venster Eigenschappen worden alleen ondersteund door API versie **2017-04-01-preview** . Hieronder ziet u een voor beeld van de JSON-nettolading voor een REST API aanroep:

```json
"type": "stream",
      "serialization": {
        "type": "Parquet",
        "properties": {}
      },
      "timeWindow": "00:02:00",
      "sizeWindow": "2000",
      "datasource": {
        "type": "Microsoft.Storage/Blob",
        "properties": {
          "storageAccounts" : [
          {
            "accountName": "{accountName}",
            "accountKey": "{accountKey}",
          }
          ],
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>
> [Snelstart: Een Stream Analytics-taak maken via Azure Portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
