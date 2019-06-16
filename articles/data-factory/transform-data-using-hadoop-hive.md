---
title: Gegevens transformeren met behulp van Hadoop Hive-activiteit in Azure Data Factory | Microsoft Docs
description: Lees hoe u de Hive-activiteit in een Azure data factory kunt gebruiken om uit te voeren van Hive-query's op een on-demand/uw eigen HDInsight-cluster.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 3852b2d18b48be63cbc612159facb6273f23dc2b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60848088"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Gegevens transformeren met behulp van Hadoop Hive-activiteit in Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-hive-activity.md)
> * [Huidige versie](transform-data-using-hadoop-hive.md)

De HDInsight Hive-activiteit in een Data Factory [pijplijn](concepts-pipelines-activities.md) Hive-query's uitvoert op [uw eigen](compute-linked-services.md#azure-hdinsight-linked-service) of [op aanvraag](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-cluster. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](transform-data.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten geeft.

Als u niet bekend bent met Azure Data Factory, Lees [Inleiding tot Azure Data Factory](introduction.md) en voer de [zelfstudie: gegevens transformeren](tutorial-transform-data-spark-powershell.md) voordat het lezen van dit artikel. 

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```
## <a name="syntax-details"></a>Syntaxis van de details
| Eigenschap            | Description                                                  | Vereist |
| ------------------- | ------------------------------------------------------------ | -------- |
| name                | Naam van de activiteit                                         | Ja      |
| description         | Beschrijving van het doel waarvoor de activiteit wordt gebruikt                | Nee       |
| type                | Voor Hive-activiteit wordt het activiteitstype HDinsightHive        | Ja      |
| linkedServiceName   | Verwijzing naar het HDInsight-cluster geregistreerd als een gekoppelde service in Data Factory. Zie voor meer informatie over deze gekoppelde service, [gekoppelde services berekenen](compute-linked-services.md) artikel. | Ja      |
| scriptLinkedService | Verwijzing naar een gekoppelde Azure Storage-Service gebruikt voor het opslaan van het Hive-script moet worden uitgevoerd. Als u deze gekoppelde Service niet opgeeft, wordt de Azure Storage gekoppelde Service gedefinieerd in de gekoppelde HDInsight-Service wordt gebruikt. | Nee       |
| scriptPath          | Geef het pad naar het scriptbestand in de Azure-opslag waarnaar wordt verwezen door de scriptLinkedService met de opgeslagen. De bestandsnaam is hoofdlettergevoelig. | Ja      |
| getDebugInfo        | Geeft aan wanneer de logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door scriptLinkedService. Toegestane waarden: Geen altijd of fout. Standaardwaarde: Geen. | Nee       |
| arguments           | Hiermee geeft u een matrix van de argumenten voor een Hadoop-taak. De argumenten worden doorgegeven als opdrachtregelargumenten aan elke taak. | Nee       |
| defines             | Geef parameters op als sleutel/waarde-paren voor verwijzende binnen het Hive-script. | Nee       |
| queryTimeout        | Query uitvoeren op time-outwaarde (in minuten). Heeft de van toepassing als het HDInsight-cluster met Enterprise-beveiligingspakket is ingeschakeld is. | Nee       |

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen waarin wordt uitgelegd hoe het transformeren van gegevens op andere manieren: 

* [U-SQL-activiteit](transform-data-using-data-lake-analytics.md)
* [Pig-activiteit](transform-data-using-hadoop-pig.md)
* [MapReduce-activiteit](transform-data-using-hadoop-map-reduce.md)
* [Hadoop-streamingactiviteit](transform-data-using-hadoop-streaming.md)
* [Spark-activiteit](transform-data-using-spark.md)
* [.NET aangepaste activiteit](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch Execution-activiteit](transform-data-using-machine-learning.md)
* [Opgeslagen procedureactiviteit](transform-data-using-stored-procedure.md)
