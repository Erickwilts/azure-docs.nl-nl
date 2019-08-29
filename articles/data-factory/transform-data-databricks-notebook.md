---
title: Gegevens transformeren met Databricks notebook-Azure | Microsoft Docs
description: Meer informatie over het verwerken of transformeren van gegevens door een Databricks-notebook uit te voeren.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: db437c7699c7fddc2b04175537446f53c4c4bc85
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2019
ms.locfileid: "70140819"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Gegevens transformeren door een Databricks-notebook uit te voeren

Met de activiteit Azure Databricks notebook in een [Data Factory pijp lijn](concepts-pipelines-activities.md) wordt een Databricks-notebook in uw Azure Databricks-werk ruimte uitgevoerd. In dit artikel vindt u een overzicht van het artikel over de [activiteiten](transform-data.md) voor gegevens transformatie, dat een algemene informatie bevat over de gegevens transformatie en de ondersteunde transformatie activiteiten. Azure Databricks is een beheerd platform voor het uitvoeren van Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>Definitie van Databricks-notebook-activiteit

Hier volgt een voor beeld van de JSON-definitie van een Databricks-notebook activiteit:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Eigenschappen van Databricks notebook-activiteit

In de volgende tabel worden de JSON-eigenschappen beschreven die in de JSON-definitie worden gebruikt:

|Eigenschap|Description|Vereist|
|---|---|---|
|name|De naam van de activiteit in de pijp lijn.|Ja|
|description|Tekst die beschrijft wat de activiteit doet.|Nee|
|Type|Voor Databricks notebook-activiteit is het type activiteit DatabricksNotebook.|Ja|
|linkedServiceName|De naam van de gekoppelde Databricks-service waarop de Databricks-notebook wordt uitgevoerd. Zie het artikel [Compute linked Services](compute-linked-services.md) (Engelstalig) voor meer informatie over deze gekoppelde service.|Ja|
|notebookPath|Het absolute pad van de notebook dat moet worden uitgevoerd in de Databricks-werk ruimte. Dit pad moet beginnen met een slash.|Ja|
|baseParameters|Een matrix met sleutel-waardeparen. Basis parameters kunnen worden gebruikt voor elke uitvoering van de activiteit. Als het notitie blok een para meter accepteert die niet is opgegeven, wordt de standaard waarde van het notitie blok gebruikt. Meer informatie over para meters in [Databricks](https://docs.databricks.com/api/latest/jobs.html#jobsparampair)-notebooks.|Nee|
|Library|Een lijst met bibliotheken die op het cluster moeten worden geïnstalleerd waarmee de taak wordt uitgevoerd. Dit kan een matrix van \<teken reeks, object > zijn.|Nee|


## <a name="supported-libraries-for-databricks-activities"></a>Ondersteunde bibliotheken voor Databricks-activiteiten

In de bovenstaande definitie van de Databricks-activiteit geeft u deze typen tape wisselaars op: *jar*, *ei*, *WHL*, *maven*, *pypi*, *krans*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
    {
            "whl": "dbfs:/mnt/libraries/mlflow-0.0.1.dev0-py2-none-any.whl"
        },
        {
            "whl": "dbfs:/mnt/libraries/wheel-libraries.wheelhouse.zip"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Zie de [Databricks-documentatie](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) voor bibliotheek typen voor meer informatie.

## <a name="how-to-upload-a-library-in-databricks"></a>Een bibliotheek uploaden in Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[De gebruikers interface van Databricks werk ruimte gebruiken](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

U kunt de [DATABRICKS cli (installatie)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)gebruiken om het dbfs-pad op te halen van de bibliotheek die wordt toegevoegd met behulp van de gebruikers interface. 

De jar-bibliotheken worden normaal gesp roken opgeslagen onder dbfs:/File Store/potten tijdens het gebruik van de gebruikers interface. U kunt alle weer geven via de CLI: *databricks FS ls dbfs:/File Store/potten*.



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Bibliotheek kopiëren met behulp van Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Voor beeld: *databricks FS CP sparkpi-assembly-0.1. jar dbfs:/File Store/potten*
