---
title: Activiteit metagegevens ophalen in Azure Data Factory | Microsoft Docs
description: Lees hoe u de activiteit GetMetadata kunt gebruiken in een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: jingwang
ms.openlocfilehash: 78f63b4f46fe5479d4d0fd5849ad80536d8a137c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61346847"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Activiteit metagegevens ophalen in Azure Data Factory

De GET metadata activity kan worden gebruikt om op te halen **metagegevens** van gegevens in Azure Data Factory. Deze activiteit kan worden gebruikt in de volgende scenario's:

- De informatie over de metagegevens van de gegevens valideren
- Een pijplijn activeren wanneer gegevens gereed / beschikbaar

De volgende functionaliteit is beschikbaar in de Controlestroom:

- De uitvoer van de GET metadata Activity kan worden gebruikt in voorwaardelijke expressies validatie uitvoeren.
- Een pijplijn kan worden geactiveerd wanneer de voorwaarde is voldaan via-tot lussen

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De activiteit GetMetadata duurt een gegevensset als een vereiste invoer en uitvoer metagegevensinformatie beschikbaar als uitvoer van activiteit. Op dit moment de volgende connectors met bijbehorende ophaalbaar metagegevens worden ondersteund en de grootte van de maximale ondersteunde metagegevens is tot **1MB**.

>[!NOTE]
>Als u de activiteit GetMetadata op een zelfgehoste Cloudintegratieruntime uitvoert, wordt de meest recente mogelijkheid wordt ondersteund op versie 3.6 of hoger. 

### <a name="supported-connectors"></a>Ondersteunde connectors

**Opslag van bestanden:**

| Connector/Metadata | itemName<br>(bestand/map) | itemType<br>(bestand/map) | size<br>(bestand) | Gemaakt<br>(bestand/map) | lastModified<br>(bestand/map) |childItems<br>(map) |contentMD5<br>(bestand) | structure<br/>(bestand) | columnCount<br>(bestand) | Er bestaat<br>(bestand/map) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| Google Cloud Storage | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| Azure-blob | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| Azure Data Lake Storage Gen1 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure Data Lake Storage Gen2 | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure File Storage | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Bestandssysteem | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Voor Amazon S3 en Google Sloud opslag, de `lastModified` is van toepassing op bucket en -sleutel, maar geen virtuele map; en de `exists` is van toepassing op bucket en -sleutel, maar geen voorvoegsel of virtuele map.
- Voor Azure-Blob, het `lastModified` is van toepassing op de container en blob, maar geen virtuele map.

**Relationele database:**

| Connector/Metadata | structure | columnCount | Er bestaat |
|:--- |:--- |:--- |:--- |
| Azure SQL Database | √ | √ | √ |
| Azure SQL Database Managed Instance | √ | √ | √ |
| Azure SQL Data Warehouse | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Opties voor metagegevens

De volgende typen van de metagegevens kunnen worden opgegeven in de lijst met velden activiteit GetMetadata om op te halen:

| Metagegevenstype | Description |
|:--- |:--- |
| itemName | De naam van het bestand of map. |
| itemType | Het type van het bestand of map. Uitvoerwaarde `File` of `Folder`. |
| size | Grootte van het bestand in bytes. Van toepassing op het bestand alleen. |
| Gemaakt | Gemaakte datum en tijd van het bestand of map. |
| lastModified | Datum/tijd van het bestand of map het laatst is gewijzigd. |
| childItems | Lijst met bestanden in de opgegeven map en submappen. Van toepassing op de map alleen. Waarde voor uitvoer is een lijst met naam en type van elk onderliggend item. |
| contentMD5 | MD5-algoritme van het bestand. Van toepassing op het bestand alleen. |
| structure | De gegevensstructuur in het bestand of de relationele database-tabel. Waarde voor uitvoer is een lijst met de naam van kolom en het type van kolom. |
| columnCount | Het aantal kolommen in het bestand of een relationele tabel. |
| Er bestaat| Of een bestand/map/table of niet bestaat. Houd er rekening mee dat als 'bestaat' is opgegeven in de veldenlijst GetaMetadata, de activiteit wordt niet mislukt, zelfs wanneer het item (bestand/map/table) niet bestaat. in plaats daarvan het resultaat `exists: false` in de uitvoer. |

>[!TIP]
>Als u valideren wilt als een bestand/map/table of niet bestaat, geeft `exists` in de veldenlijst GetMetadata activiteit kunt u vervolgens controleren de `exists: true/false` als gevolg van de uitvoer van de activiteit. Als `exists` is niet geconfigureerd in de lijst met velden, de GetMetadata activiteit mislukt wanneer het object is niet gevonden.

## <a name="syntax"></a>Syntaxis

**De GET metadata activity:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Gegevensset:**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

De activiteit GetMetadata kan momenteel de volgende typen metagegevens worden opgehaald.

Eigenschap | Description | Vereist
-------- | ----------- | --------
fieldList | Bevat de soorten metagegevens die zijn vereist. Zie voor meer informatie [opties voor metagegevens](#metadata-options) gedeelte over ondersteunde metagegevens. | Ja 
Gegevensset | De verwijzing gegevensset waarvan de metagegevens van de activiteit moet worden opgehaald door de activiteit GetMetadata is. Zie [ondersteunde mogelijkheden](#supported-capabilities) sectie op ondersteunde connectors en verwijzen naar het onderwerp van de connector op de details van de syntaxis van de gegevensset. | Ja

## <a name="sample-output"></a>Voorbeelduitvoer

Het resultaat GetMetadata wordt weergegeven in de uitvoer van activiteit. Hieronder vindt u twee voorbeelden met uitgebreide metagegevens opties geselecteerd in de lijst met velden als verwijzing. Als u het resultaat van de volgende activiteit, gebruikt u het patroon van `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>De metagegevens van een bestand ophalen

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>De metagegevens van een map ophalen

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
