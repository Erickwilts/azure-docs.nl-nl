---
title: Gegevens kopiëren van/naar een bestandssysteem met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van het bestandssysteem op ondersteunde sink-gegevensopslag (of) uit ondersteunde bron-gegevensarchieven aan bestandssysteem met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 7280d68e43d86662814df795884dec50b5847695
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153523"
---
# <a name="copy-data-to-or-from-a-file-system-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van een bestandssysteem met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-onprem-file-system-connector.md)
> * [Huidige versie](connector-file-system.md)

In dit artikel bevat een overzicht van hoe u het kopiëren van gegevens naar en vanuit het bestandssysteem. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Dit bestand system-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Om precies ondersteunt deze bestandssysteemconnector:

- Kopiëren van bestanden van/naar lokale machine of het netwerk-bestandsshare. Voor het gebruik van een bestandsshare voor Linux installeren [Samba](https://www.samba.org/) op uw Linux-server.
- Het kopiëren van bestanden met behulp van **Windows** verificatie.
- Kopiëren van bestanden als-is of het parseren van/genereren van bestanden met de [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van/naar een bestandssysteem dat is niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar bestandssysteem.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor bestand system gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **FileServer**. | Ja |
| host | Hiermee geeft u het pad naar de hoofdmap van de map die u wilt kopiëren. Gebruik het escape-teken '\" voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden. | Ja |
| userid | Geef de ID van de gebruiker die toegang tot de server heeft. | Ja |
| password | Geef het wachtwoord voor de gebruiker (gebruikersnaam). Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

### <a name="sample-linked-service-and-dataset-definitions"></a>Voorbeeld van een gekoppelde service en definities van de gegevensset

| Scenario | 'host' in de definitie van de gekoppelde service | 'folderPath' in de definitie van de gegevensset |
|:--- |:--- |:--- |
| Lokale map op de machine Integration Runtime: <br/><br/>Voorbeelden: D:\\ \* of D:\folder\subfolder\\* |In de JSON: `D:\\`<br/>Op de gebruikersinterface: `D:\` |In JSON: `.\\` of `folder\\subfolder`<br>In de gebruikersinterface: `.\` of `folder\subfolder` |
| Externe gedeelde map: <br/><br/>Voorbeelden: \\ \\MijnServer\\delen\\ \* of \\ \\MijnServer\\delen\\map\\submap\\* |In de JSON: `\\\\myserver\\share`<br/>Op de gebruikersinterface: `\\myserver\share` |In JSON: `.\\` of `folder\\subfolder`<br/>In de gebruikersinterface: `.\` of `folder\subfolder` |

>[!NOTE]
>Wanneer u via de gebruikersinterface, moet u niet voor het invoeren van twee backslashes (`\\`) opgeven om te escapen, zoals u ook via JSON, één backslash.

**Voorbeeld:**

```json
{
    "name": "FileLinkedService",
    "properties": {
        "type": "FileServer",
        "typeProperties": {
            "host": "<host>",
            "userid": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. 

- Voor **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Voor andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Het kopiëren van gegevens naar en vanuit het bestandssysteem in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor bestandssysteem onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **FileServerLocation**. | Ja      |
| folderPath | Het pad naar map. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De naam van het bestand onder het opgegeven mappad. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
> **Bestandsshare** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup/de GET metadata activity voor compatibiliteit met eerdere versies, maar werkt niet met gegevensstroom toewijzen. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<File system linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "FileServerLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Andere indeling-gegevensset

Het kopiëren van gegevens naar en vanuit het bestandssysteem in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **FileShare** |Ja |
| folderPath | Pad naar de map. Filteren op jokerteken wordt ondersteund, jokertekens toegestaan zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: rootfolder/submap/meer voorbeelden in [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) en [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Nee |
| fileName | **Naam of het jokerteken filter** voor de bestanden die onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle bestanden in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van het bestand met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ", bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'; Als u vanuit in tabelvorm bron met de tabelnaam van de in plaats van de query kopieert, het naampatroon is '*[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ", bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Json-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn: **Optimale** en **snelste**. |Nee |

>[!TIP]
>Alle bestanden in een map wilt kopiëren, geef **folderPath** alleen.<br>Voor het kopiëren van één bestand met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de bestanden in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken.

>[!NOTE]
>Als u 'fileFilter'-eigenschap voor bestandsfilter, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe filter mogelijkheid toegevoegd aan 'fileName' voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "FileSystemDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<file system linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het bestand system bron- en sinkblobpaden.

### <a name="file-system-as-source"></a>Bestandssysteem als bron

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens van het bestandssysteem in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor bestandssysteem onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **FileServerReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens voor het filteren van bronmappen. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder het opgegeven mappad/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in.  Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **FileSystemSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "FileServerReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>De bron van andere indeling

Het kopiëren van gegevens van het bestandssysteem in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **FileSystemSource** |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee wanneer recursieve is ingesteld op true en sink is opslag op basis van bestanden, lege map/ondergeschikt-folder worden niet gekopieerd/gemaakt bij de sink.<br/>Toegestane waarden zijn: **waar** (standaard), **false** | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<file system input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="file-system-as-sink"></a>Bestandssysteem als sink

- Voor kopiëren naar **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Voor het kopiëren naar andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Gegevens kopiëren naar het bestandssysteem in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel voor kopiëren-indeling op basis van activiteit-sink en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor bestandssysteem onder `storeSettings` instellingen in op basis van opmaak kopiëren sink:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **FileServerWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de bestandsnaam is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met het gegevensarchief. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **FileSystemSink** type activiteit-sink voor kopiëren die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "FileServerWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Andere indeling-sink

Gegevens kopiëren naar het bestandssysteem in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **FileSystemSink** |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron-bestanden van gegevensopslag op basis van bestanden.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden uit de bronmap van het zich in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><b>-MergeFiles</b>: alle bestanden uit de bronmap naar één bestand worden samengevoegd. Als de naam van de bestands-/ Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. anders zou worden automatisch gegenereerde naam. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToFileSystem",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<file system output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "FileSystemSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName | recursieve | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (leeg, gebruikt u standaard) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | (leeg, gebruikt u standaard) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="recursive-and-copybehavior-examples"></a>recursieve en copyBehavior voorbeelden

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursieve | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + 5-bestand inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met automatisch gegenereerde naam. Automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 worden niet doorgevoerd. |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
