---
title: Gegevens kopiëren van een HTTP-bron met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit een Cloud of een on-premises HTTP-bron naar ondersteunde Sink-gegevens archieven door gebruik te maken van een Kopieer activiteit in een Azure Data Factory-pijp lijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: 8d6cc131c0c2baf7cc0a6600946870615d99e030
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839803"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Gegevens kopiëren van een HTTP-eind punt met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-http-connector.md)
> * [Huidige versie](connector-http.md)

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens van een HTTP-eind punt te kopiëren. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

Het verschil tussen deze HTTP-connector, de [rest-connector](connector-rest.md) en de [Web Table-connector](connector-web-table.md) zijn:

- **Rest-connector** biedt specifiek ondersteuning voor het kopiëren van gegevens uit rest-api's; 
- **Http-connector** is algemeen om gegevens op te halen uit een http-eind punt, bijvoorbeeld om het bestand te downloaden. Voordat REST-connector beschikbaar wordt, kunt u de HTTP-connector gebruiken om gegevens te kopiëren van de REST-API, die wordt ondersteund, maar minder functioneel is vergeleken met de REST-connector.
- Met **Web Table connector** wordt tabel inhoud geëxtraheerd van een HTML-webpagina.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van een HTTP-bron kopiëren naar elk ondersteund Sink-gegevens archief. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

U kunt deze HTTP-connector gebruiken voor het volgende:

- Gegevens ophalen van een HTTP/S-eind punt met behulp van de HTTP **Get** -of **post** -methoden.
- Gegevens ophalen met behulp van een van de volgende authenticaties: **Anoniem**, **basis**, **Digest**, **Windows**of **ClientCertificate**.
- Kopieer het HTTP-antwoord alsof het is of Parset het met behulp van [ondersteunde bestands indelingen en compressie](supported-file-formats-and-compression-codecs.md)-codecs.

> [!TIP]
> Als u een HTTP-aanvraag voor het ophalen van gegevens wilt testen voordat u de HTTP-connector in Data Factory configureert, kunt u meer informatie vinden over de API-specificatie voor vereisten voor koptekst en hoofd tekst. U kunt de hulpprogram ma's zoals postman of een webbrowser gebruiken om te valideren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die u kunt gebruiken voor het definiëren van Data Factory entiteiten die specifiek zijn voor de HTTP-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de HTTP-gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap **type** moet worden ingesteld op **HttpServer**. | Ja |
| url | De basis-URL naar de webserver. | Ja |
| enableServerCertificateValidation | Geef op of de SSL-certificaat validatie van de server moet worden ingeschakeld wanneer u verbinding maakt met een HTTP-eind punt. Als uw HTTPS-server een zelfondertekend certificaat gebruikt, stelt u deze eigenschap inop ONWAAR. | Nee<br /> (de standaard waarde is **True**) |
| authenticationType | Hiermee geeft u het verificatie type op. Toegestane waarden zijn **anoniem**, **basis**, **Digest**, **Windows**en **ClientCertificate**. <br><br> Zie de secties die volgen op deze tabel voor meer eigenschappen en JSON-voor beelden voor deze verificatie typen. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt de Azure Integration Runtime of een zelf-hostend Integration Runtime gebruiken (als uw gegevens archief zich in een particulier netwerk bevindt). Als deze eigenschap niet is opgegeven, wordt de standaard Azure Integration Runtime gebruikt. |Nee |

### <a name="using-basic-digest-or-windows-authentication"></a>Basis verificatie, verificatie samenvatting of Windows-authenticatie gebruiken

Stel de eigenschap **authenticationType** in op **Basic**, **Digest**of **Windows**. Naast de algemene eigenschappen die in de voor gaande sectie worden beschreven, geeft u de volgende eigenschappen op:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| userName | De gebruikers naam die moet worden gebruikt voor toegang tot het HTTP-eind punt. | Ja |
| password | Het wachtwoord voor de gebruiker (de **userName** waarde). Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |

**Voorbeeld**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
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

### <a name="using-clientcertificate-authentication"></a>Gebruik van ClientCertificate-verificatie

Als u ClientCertificate-verificatie wilt gebruiken, stelt u de eigenschap **authenticationType** in op **ClientCertificate**. Naast de algemene eigenschappen die in de voor gaande sectie worden beschreven, geeft u de volgende eigenschappen op:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| embeddedCertData | Met base64 gecodeerde certificaat gegevens. | Geef **embeddedCertData** of **certThumbprint**op. |
| certThumbprint | De vinger afdruk van het certificaat dat is geïnstalleerd op uw zelf-hostende certificaat archief van Integration Runtime computer. Is alleen van toepassing wanneer het zelf-hostende type Integration Runtime is opgegeven in de eigenschap **connectVia** . | Geef **embeddedCertData** of **certThumbprint**op. |
| password | Het wacht woord dat is gekoppeld aan het certificaat. Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |

Als u **certThumbprint** gebruikt voor verificatie en het certificaat is geïnstalleerd in het persoonlijke archief van de lokale computer, verleent u lees machtigingen voor de zelf-hostende Integration runtime:

1. Open de micro soft Management Console (MMC). Voeg de module **certificaten** toe die gericht is op de **lokale computer**.
2. Vouw **certificaten** > **persoonlijk**uit en selecteer vervolgens **certificaten**.
3. Klik met de rechter muisknop op het certificaat in het persoonlijke archief en selecteer vervolgens **alle taken** > **persoonlijke sleutels beheren**.
3. Voeg op het tabblad **beveiliging** het gebruikers account toe waaronder de Integration runtime host-service (DIAHostService) wordt uitgevoerd, met lees toegang tot het certificaat.

**Voor beeld 1: CertThumbprint gebruiken**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld 2: EmbeddedCertData gebruiken**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
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

- Raadpleeg voor **Parquet, tekst met scheidings tekens en binaire indeling**, de sectie [Parquet, tekst met scheidings tekens en gegevensset voor binaire indeling](#format-based-dataset) .
- Raadpleeg voor andere indelingen, zoals de **Orc/Avro/JSON-indeling**, de sectie [andere indelings gegevensset](#other-format-dataset) .

### <a name="format-based-dataset"></a>Parquet, gescheiden tekst en binaire indeling gegevensset

Als u gegevens wilt kopiëren naar en van **Parquet, tekst met scheidings tekens of binaire indeling**, raadpleegt u de [Parquet-indeling](format-parquet.md), [tekst indeling met scheidings tekens](format-delimited-text.md) en het artikel [binaire indeling](format-binary.md) op op indeling gebaseerde gegevensset en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor http onder `location` instellingen in gegevensset op basis van indeling:

| Eigenschap    | Description                                                  | Vereist |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | De eigenschap type onder `location` in DataSet moet worden ingesteld op **HttpServerLocation**. | Ja      |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat.       | Nee       |

> [!NOTE]
> De ondersteunde Payload-grootte van de HTTP-aanvraag is ongeveer 500 KB. Als de payload-grootte die u wilt door geven aan uw web-eind punt groter is dan 500 KB, kunt u de payload in kleinere segmenten batcheren.

> [!NOTE]
> **HttpFile** van het type dataset met Parquet/Text-indeling die in de volgende sectie wordt vermeld, wordt nog steeds ondersteund als-is voor de activiteit Copy/lookup voor achterwaartse compatibiliteit. U wordt aangeraden dit nieuwe model verder te gebruiken en de gebruikers interface van de ADF-ontwerp functie is overgeschakeld op het genereren van deze nieuwe typen.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HttpServerLocation",
                "relativeUrl": "<relative url>"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Gegevensset voor andere indeling

De volgende eigenschappen worden ondersteund voor het kopiëren van gegevens uit HTTP in de **Orc/Avro/JSON-indeling**:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap **type** van de DataSet moet worden ingesteld op **HttpFile**. | Ja |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat. Als deze eigenschap niet is opgegeven, wordt alleen de URL gebruikt die in de definitie van de gekoppelde service is opgegeven. | Nee |
| requestMethod | De HTTP-methode. Toegestane waarden zijn **Get** (standaard) en **post**. | Nee |
| additionalHeaders | Aanvullende HTTP-aanvraag headers. | Nee |
| requestBody | De hoofd tekst van de HTTP-aanvraag. | Nee |
| format | Als u gegevens wilt ophalen uit het HTTP-eind punt zonder te parseren, en vervolgens de gegevens naar een op bestanden gebaseerde opslag kopie wilt kopiëren, slaat u de sectie **opmaak** in zowel de definitie van de invoer-als uitvoer gegevensset over.<br/><br/>Als u de inhoud van het HTTP-antwoord tijdens het kopiëren wilt parseren, worden de volgende typen bestands indelingen ondersteund: **TextFormat**, **JsonFormat**, **Avro Format**, **OrcFormat**en **ParquetFormat**. Stel onder **indeling**de eigenschap **type** in op een van deze waarden. Zie [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [tekst indeling](supported-file-formats-and-compression-codecs.md#text-format), [Avro](supported-file-formats-and-compression-codecs.md#avro-format)-indeling, Orc- [indeling](supported-file-formats-and-compression-codecs.md#orc-format)en Parquet- [indeling](supported-file-formats-and-compression-codecs.md#parquet-format)voor meer informatie. |Nee |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Ondersteunde typen: **Gzip**,Deflate, **bzip2**en **ZipDeflate**.<br/>Ondersteunde niveaus:  **Optimaal** en **snelst**. |Nee |

> [!NOTE]
> De ondersteunde Payload-grootte van de HTTP-aanvraag is ongeveer 500 KB. Als de payload-grootte die u wilt door geven aan uw web-eind punt groter is dan 500 KB, kunt u de payload in kleinere segmenten batcheren.

**Voor beeld 1: De Get-methode gebruiken (standaard)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Voor beeld 2: De post-methode gebruiken**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

In deze sectie vindt u een lijst met eigenschappen die door de HTTP-bron worden ondersteund.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP als bron

- Als u wilt kopiëren van **Parquet, tekst met scheidings tekens en binaire indeling**, raadpleegt u de sectie [Parquet, tekst met scheidings tekens en bron voor binaire indeling](#format-based-source) .
- Als u wilt kopiëren van andere indelingen, zoals de **Orc/Avro/JSON-indeling**, raadpleegt u de sectie [andere indelings bron](#other-format-source) .

#### <a name="format-based-source"></a>Parquet, tekst met scheidings tekens en binaire indelings bron

Als u gegevens wilt kopiëren uit een **Parquet, een tekst met scheidings tekens of binaire indeling**, raadpleegt u de [Parquet-indeling](format-parquet.md), [tekst indeling met scheidings tekens](format-delimited-text.md) en het artikel [binaire indeling](format-binary.md) op op indeling gebaseerde Kopieer activiteit bron en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor http onder `storeSettings` instellingen in een op indeling gebaseerde kopie bron:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **HttpReadSetting**. | Ja      |
| requestMethod            | De HTTP-methode. <br>Toegestane waarden zijn **Get** (standaard) en **post**. | Nee       |
| addtionalHeaders         | Aanvullende HTTP-aanvraag headers.                             | Nee       |
| requestBody              | De hoofd tekst van de HTTP-aanvraag.                               | Nee       |
| requestTimeout           | De time-out (de time **span** -waarde) voor de HTTP-aanvraag om een antwoord te krijgen. Deze waarde is de time-out voor het verkrijgen van een reactie, niet de time-out voor het lezen van antwoord gegevens. De standaard waarde is **00:01:40**. | Nee       |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee       |

> [!NOTE]
> Voor een Parquet/gescheiden tekst indeling wordt de bron van de Kopieer activiteit van het type **http** die in de volgende sectie wordt vermeld, nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U wordt aangeraden dit nieuwe model verder te gebruiken en de gebruikers interface van de ADF-ontwerp functie is overgeschakeld op het genereren van deze nieuwe typen.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
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
                    "type": "HttpReadSetting",
                    "requestMethod": "Post",
                    "additionalHeaders": "<header key: header value>\n<header key: header value>\n",
                    "requestBody": "<body for POST HTTP request>"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Andere indelings bron

Als u gegevens wilt kopiëren van HTTP in **Orc/Avro/JSON-indeling**, worden de volgende eigenschappen ondersteund in de sectie **bron** van de Kopieer activiteit:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap **type** van de bron van de Kopieer activiteit moet zijn ingesteld op **http**. | Ja |
| httpRequestTimeout | De time-out (de time **span** -waarde) voor de HTTP-aanvraag om een antwoord te krijgen. Deze waarde is de time-out voor het verkrijgen van een reactie, niet de time-out voor het lezen van antwoord gegevens. De standaard waarde is **00:01:40**.  | Nee |

**Voorbeeld**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).
