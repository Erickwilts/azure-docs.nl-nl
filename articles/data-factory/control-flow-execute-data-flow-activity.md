---
title: Activiteit gegevens stroom
description: Gegevens stromen uitvoeren vanuit een data factory pijp lijn.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: makromer
ms.date: 04/30/2020
ms.openlocfilehash: 5593b0d633b133c8a8295634b674218d5e6c6daf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89485034"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Gegevens stroom activiteit in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Gebruik de activiteit gegevens stroom om gegevens te transformeren en te verplaatsen via toewijzing van gegevens stromen. Zie [overzicht van gegevens stroom toewijzen](concepts-data-flow-overview.md) als u geen ervaring hebt met gegevens stromen

## <a name="syntax"></a>Syntax

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "compute": {
         "coreCount": 8,
         "computeType": "General"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Type-eigenschappen

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
stroom | De verwijzing naar de gegevens stroom die wordt uitgevoerd | DataFlowReference | Ja
integrationRuntime | De compute-omgeving waarop de gegevens stroom wordt uitgevoerd. Als deze niet is opgegeven, wordt Azure Integration runtime automatisch opgelost. | IntegrationRuntimeReference | Nee
compute. coreCount | Het aantal kern geheugens dat in het Spark-cluster wordt gebruikt. Kan alleen worden opgegeven als Azure Integration runtime automatisch wordt opgelost | 8, 16, 32, 48, 80, 144, 272 | Nee
compute. computeType | Het type berekening dat in het Spark-cluster wordt gebruikt. Kan alleen worden opgegeven als Azure Integration runtime automatisch wordt opgelost | "Algemeen", "ComputeOptimized", "MemoryOptimized" | Nee
staging. linkedService | Als u een Azure Synapse Analytics-bron of-sink gebruikt, wordt het opslag account dat wordt gebruikt voor poly base staging | Linkedservicereference is | Alleen als de gegevens stroom leest of schrijft naar een Azure Synapse-analyse
staging. folderPath | Als u een Azure Synapse Analytics-bron of-sink gebruikt, wordt het mappad in het Blob Storage-account dat wordt gebruikt voor poly base staging | Tekenreeks | Alleen als de gegevens stroom leest of schrijft naar Azure Synapse Analytics

![Gegevens stroom uitvoeren](media/data-flow/activity-data-flow.png "Gegevens stroom uitvoeren")

### <a name="dynamically-size-data-flow-compute-at-runtime"></a>Data flow-reken kracht tijdens runtime dynamisch aanpassen

De eigenschappen kern aantal en reken type kunnen dynamisch worden ingesteld om te worden aangepast aan de grootte van uw binnenkomende bron gegevens tijdens runtime. Gebruik pijplijn activiteiten zoals lookup of Get meta data om de grootte van de gegevens van de bron gegevensset te vinden. Gebruik vervolgens dynamische inhoud toevoegen in de eigenschappen van de gegevens stroom activiteit.

![Dynamische gegevens stroom](media/data-flow/dyna1.png "Dynamische gegevens stroom")

[Hier volgt een korte zelf studie over de video waarin deze techniek wordt uitgelegd](https://www.youtube.com/watch?v=jWSkJdtiJNM)

### <a name="data-flow-integration-runtime"></a>Data flow Integration runtime

Kies welke Integration Runtime moet worden gebruikt voor de uitvoering van de activiteit van de gegevens stroom. Data Factory maakt standaard gebruik van het automatisch oplossen van Azure Integration runtime met vier worker-kernen en geen TTL (time to Live). Deze IR heeft een reken type voor algemeen gebruik en wordt uitgevoerd in dezelfde regio als uw fabriek. U kunt uw eigen Azure Integration Runtimes maken voor het definiëren van specifieke regio's, reken type, kern aantallen en TTL voor de uitvoering van de gegevens stroom activiteit.

Voor de uitvoering van pijp lijnen is het cluster een taak cluster, dat enkele minuten in beslag neemt voordat de uitvoering wordt gestart. Als er geen TTL is opgegeven, is deze opstart tijd vereist op elke pijplijn uitvoering. Als u een TTL opgeeft, blijft een warme cluster groep actief gedurende de tijd die na de laatste uitvoering is opgegeven, wat resulteert in kortere opstart tijden. Als u bijvoorbeeld een TTL van 60 minuten hebt en een gegevens stroom eenmaal per uur uitvoert, blijft de cluster groep actief. Zie [Azure Integration runtime](concepts-integration-runtime.md)voor meer informatie.

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!IMPORTANT]
> De Integration Runtime selectie in de activiteit gegevens stroom is alleen van toepassing op *geactiveerde uitvoeringen* van de pijp lijn. Fout opsporing voor de pijp lijn met gegevens stromen worden uitgevoerd op het cluster dat is opgegeven in de foutopsporingssessie.

### <a name="polybase"></a>PolyBase

Als u een Azure Synapse Analytics (voorheen SQL Data Warehouse) als sink of bron gebruikt, moet u een faserings locatie voor het laden van poly base-batches kiezen. Met poly Base kan batch in bulk worden geladen in plaats van de gegevensrij per rij te laden. Poly base verlaagt drastisch de laad tijd in azure Synapse Analytics.

## <a name="parameterizing-data-flows"></a>Parameterizing-gegevens stromen

### <a name="parameterized-datasets"></a>Gegevens sets met para meters

Als uw gegevens stroom gebruikmaakt van parameter gegevens sets, stelt u de parameter waarden in op het tabblad **instellingen** .

![Data flow-para meters uitvoeren](media/data-flow/params.png "Parameters")

### <a name="parameterized-data-flows"></a>Gegevens stromen met para meters

Als uw gegevens stroom is para meters, stelt u de dynamische waarden van de para meters voor de gegevens stroom in op het tabblad **para meters** . U kunt de taal van de ADF-pipeline-expressie of de data flow-expressie taal gebruiken om dynamische of letterlijke parameter waarden toe te wijzen. Zie [Data flow-para meters](parameters-data-flow.md)voor meer informatie.

### <a name="parameterized-compute-properties"></a>Reken eigenschappen met para meters.

U kunt het aantal kernen of het reken type para meters als u de Azure Integration runtime automatisch oplossen gebruikt en waarden opgeeft voor compute. coreCount en compute. computeType.

![Voor beeld van para meter voor gegevens stroom uitvoeren](media/data-flow/parameterize-compute.png "Parameter voorbeeld")

## <a name="pipeline-debug-of-data-flow-activity"></a>Pijp lijn fout opsporing van gegevens stroom activiteit

Als u een pijp lijn voor fout opsporing wilt uitvoeren met een activiteit voor gegevens stromen, moet u overschakelen op de modus voor het opsporen van gegevens stromen via de schuif regelaar voor **fout opsporing van gegevens stromen** op de bovenste balk. Met de foutopsporingsmodus kunt u de gegevens stroom uitvoeren op een actief Spark-cluster. Zie [debug mode (foutopsporingsmodus](concepts-data-flow-debug-mode.md)) voor meer informatie.

![Knop fout opsporing](media/data-flow/debugbutton.png "Knop fout opsporing")

De pijp lijn voor fout opsporing wordt uitgevoerd op het actieve debug-cluster, niet de Integration runtime-omgeving die is opgegeven in de instellingen voor de activiteit van de gegevens stroom. U kunt de compute-omgeving voor fout opsporing kiezen wanneer u de foutopsporingsmodus opstart.

## <a name="monitoring-the-data-flow-activity"></a>De activiteit gegevens stroom bewaken

De activiteit gegevens stroom heeft een speciale bewakings ervaring waarbij u gegevens over partitionering, fase tijd en gegevens afkomst kunt weer geven. Open het deel venster bewaking via het pictogram bril onder **acties**. Zie [gegevens stromen bewaken](concepts-data-flow-monitoring.md)voor meer informatie.

### <a name="use-data-flow-activity-results-in-a-subsequent-activity"></a>Resultaten van de gegevens stroom activiteit gebruiken in een volgende activiteit

De gegevens stroom activiteit voert metrische waarden uit op basis van het aantal rijen dat naar elke sink is geschreven en rijen die van elke bron zijn gelezen. Deze resultaten worden geretourneerd in de `output` sectie van het resultaat van de uitvoering van de activiteit. De metrische gegevens worden in de indeling van de onderstaande JSON weer gegeven.

``` json
{
    "runStatus": {
        "metrics": {
            "<your sink name1>": {
                "rowsWritten": <number of rows written>,
                "sinkProcessingTime": <sink processing time in ms>,
                "sources": {
                    "<your source name1>": {
                        "rowsRead": <number of rows read>
                    },
                    "<your source name2>": {
                        "rowsRead": <number of rows read>
                    },
                    ...
                }
            },
            "<your sink name2>": {
                ...
            },
            ...
        }
    }
}
```

Als u bijvoorbeeld wilt zoeken naar het aantal rijen dat is geschreven naar een Sink met de naam ' sink1 ' in een activiteit met de naam ' dataflowActivity ', gebruikt u `@activity('dataflowActivity').output.runStatus.metrics.sink1.rowsWritten` .

Als u het aantal rijen wilt ophalen dat is gelezen uit een bron met de naam ' source1 ' die in die sink is gebruikt, gebruikt u `@activity('dataflowActivity').output.runStatus.metrics.sink1.sources.source1.rowsRead` .

> [!NOTE]
> Als een Sink nul rijen heeft geschreven, wordt deze niet weer gegeven in metrische gegevens. Aanwezigheid kan worden gecontroleerd met behulp van de `contains` functie. U kunt bijvoorbeeld `contains(activity('dataflowActivity').output.runStatus.metrics, 'sink1')` controleren of er rijen zijn geschreven naar sink1.

## <a name="next-steps"></a>Volgende stappen

Zie controle stroom activiteiten die worden ondersteund door Data Factory: 

- [If Condition Activity](control-flow-if-condition-activity.md)
- [Activiteit uitvoeren van pijplijn](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Activiteit ophalen van metagegevens](control-flow-get-metadata-activity.md)
- [Opzoekactiviteit](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
- [Until-activiteit](control-flow-until-activity.md)
