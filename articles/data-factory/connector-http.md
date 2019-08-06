---
title: Gegevens kopiëren van een HTTP-bron met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van een HTTP-cloud of on-premises bron naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.openlocfilehash: a668bb2e0e3381abefaac93a0fb63f0d33bac5a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65234058"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Gegevens kopiëren van een HTTP-eindpunt met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](v1/data-factory-http-connector.md)
> * [Huidige versie](connector-http.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van een HTTP-eindpunt. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

Het verschil tussen deze HTTP-connector, de [REST-connector](connector-rest.md) en de [Web-connector voor tabel](connector-web-table.md) zijn:

- **REST-connector** specifiek ondersteuning kopiëren van gegevens uit RESTful-API's; 
- **HTTP-connector** is generiek gegevens ophalen van een HTTP-eindpunt, bijvoorbeeld om bestand te downloaden. Voordat de REST-connector beschikbaar is, kan het geval zijn de HTTP-connector gebruiken om gegevens te kopiëren van RESTful-API, die wordt ondersteund, maar minder functionele vergelijken met de REST-connector.
- **Web-connector voor tabel** extraheert inhoud van de webpagina van een HTML-tabel.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van een HTTP-bron naar elke ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

U kunt deze HTTP-connector te gebruiken:

- Gegevens ophalen uit een HTTP/S-eindpunt met behulp van de HTTP **ophalen** of **POST** methoden.
- Gegevens ophalen met behulp van een van de volgende verificaties: **Anonieme**, **Basic**, **Digest**, **Windows**, of **ClientCertificate**.
- Het HTTP-antwoord als kopiëren-is of met behulp van parseren [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

> [!TIP]
> Als u wilt testen een HTTP-aanvraag voor het ophalen van gegevens voordat u de HTTP-connector in Data Factory configureert, meer informatie over de API-specificatie voor de koptekst en hoofdtekst van de vereisten. U kunt hulpprogramma's zoals Postman of een webbrowser gebruiken om te valideren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor de HTTP-connector zijn.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de HTTP-gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **HttpServer**. | Ja |
| url | De basis-URL naar de webserver. | Ja |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL in te schakelen wanneer u verbinding met een HTTP-eindpunt maakt. Als uw HTTPS-server een zelfondertekend certificaat gebruikt, deze eigenschap instellen op **false**. | Nee<br /> (de standaardwaarde is **waar**) |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn **anoniem**, **Basic**, **Digest**, **Windows**, en **ClientCertificate**. <br><br> Zie de secties die volgen van deze tabel voor meer eigenschappen en JSON-voorbeelden voor deze verificatietypen. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. U kunt de Azure Integration Runtime of een zelf-hostende integratie-Runtime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Indien niet opgegeven, wordt deze eigenschap gebruikt de standaard Azure Integration Runtime. |Nee |

### <a name="using-basic-digest-or-windows-authentication"></a>Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie

Stel de **authenticationType** eigenschap **Basic**, **Digest**, of **Windows**. Naast de algemene eigenschappen die worden beschreven in de vorige sectie, moet u de volgende eigenschappen opgeven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| userName | De gebruikersnaam gebruiken voor toegang tot het HTTP-eindpunt. | Ja |
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

### <a name="using-clientcertificate-authentication"></a>Met behulp van ClientCertificate verificatie

ClientCertificate om verificatie te gebruiken, stelt de **authenticationType** eigenschap **ClientCertificate**. Naast de algemene eigenschappen die worden beschreven in de vorige sectie, moet u de volgende eigenschappen opgeven:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| embeddedCertData | De gegevens van de met base64 gecodeerde certificaat. | Geef óf een **embeddedCertData** of **certThumbprint**. |
| certThumbprint | De vingerafdruk van het certificaat dat geïnstalleerd op het certificaatarchief van de zelf-hostende Integration Runtime-machine. Geldt alleen wanneer de zelf-hostende Integration Runtime-type is opgegeven in de **connectVia** eigenschap. | Geef óf een **embeddedCertData** of **certThumbprint**. |
| password | Het wachtwoord dat is gekoppeld aan het certificaat. Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |

Als u **certThumbprint** voor verificatie en het certificaat is geïnstalleerd in het persoonlijke archief van de lokale computer, Leesmachtiging verlenen aan de zelf-hostende Integration Runtime:

1. Open de Microsoft Management Console (MMC). Voeg de **certificaten** -module die is gericht op **lokale Computer**.
2. Vouw **certificaten** > **persoonlijke**, en selecteer vervolgens **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief, en selecteer vervolgens **alle taken** > **persoonlijke sleutels beheren**.
3. Op de **Security** tabblad, voegt u het gebruikersaccount waarmee de hostservice van Integration Runtime (DIAHostService) wordt uitgevoerd, met leestoegang tot het certificaat.

**Voorbeeld 1: Met behulp van certThumbprint**

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

**Voorbeeld 2: Met behulp van embeddedCertData**

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

- Voor **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling gegevensset](#parquet-and-delimited-text-format-dataset) sectie.
- Voor andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere gegevensset indeling](#other-format-dataset) sectie.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet en tekst met scheidingstekens indeling gegevensset

Het kopiëren van gegevens van HTTP in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteund Instellingen. De volgende eigenschappen worden ondersteund voor HTTP onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap    | Description                                                  | Vereist |
| ----------- | ------------------------------------------------------------ | -------- |
| type        | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **HttpServerLocation**. | Ja      |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat.       | Nee       |

> [!NOTE]
> De ondersteunde grootte van HTTP-verzoek nettolading is ongeveer 500 KB. Als de grootte van de nettolading die u wilt doorgeven aan uw webeindpunt groter dan 500 KB is, kunt u overwegen de nettolading in kleinere chunks batchverwerking.

> [!NOTE]
> **HttpFile** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup-activiteit voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

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

### <a name="other-format-dataset"></a>Andere indeling-gegevensset

Het kopiëren van gegevens van HTTP in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de gegevensset moet worden ingesteld op **HttpFile**. | Ja |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat. Als deze eigenschap is niet opgegeven, wordt alleen de URL die opgegeven in de definitie van de gekoppelde service gebruikt. | Nee |
| requestMethod | De HTTP-methode. Toegestane waarden zijn **ophalen** (standaard) en **Post**. | Nee |
| additionalHeaders | Extra kopteksten die HTTP-aanvraag. | Nee |
| requestBody | De hoofdtekst van de HTTP-aanvraag. | Nee |
| format | Als u wilt ophalen van gegevens uit de HTTP-eindpunt als-is zonder parseren en kopieer de gegevens in een store op basis van bestanden, gaat de **indeling** sectie in de invoer en uitvoer gegevenssetdefinities.<br/><br/>Als u de inhoud van de HTTP-reactie parseren tijdens het kopiëren wilt, worden de volgende indeling bestandstypen ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Onder **indeling**, stel de **type** eigenschap op een van deze waarden. Zie voor meer informatie, [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling](supported-file-formats-and-compression-codecs.md#parquet-format). |Nee |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Ondersteunde typen: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus:  **Optimale** en **snelste**. |Nee |

> [!NOTE]
> De ondersteunde grootte van HTTP-verzoek nettolading is ongeveer 500 KB. Als de grootte van de nettolading die u wilt doorgeven aan uw webeindpunt groter dan 500 KB is, kunt u overwegen de nettolading in kleinere chunks batchverwerking.

**Voorbeeld 1: Met behulp van de Get-methode (standaard)**

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

**Voorbeeld 2: Met behulp van de Post-methode**

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

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de HTTP-bron.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP als bron

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Het kopiëren van gegevens van HTTP in **Parquet of gescheiden tekstopmaak**, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor HTTP onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **HttpReadSetting**. | Ja      |
| requestMethod            | De HTTP-methode. <br>Toegestane waarden zijn **ophalen** (standaard) en **Post**. | Nee       |
| addtionalHeaders         | Extra kopteksten die HTTP-aanvraag.                             | Nee       |
| requestBody              | De hoofdtekst van de HTTP-aanvraag.                               | Nee       |
| requestTimeout           | De time-out (de **TimeSpan** waarde) voor de HTTP-aanvraag reageert. Deze waarde is de time-out voor een antwoord, niet de time-out voor lezen van gegevens van de reactie. De standaardwaarde is **00:01:40**. | Nee       |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **HttpSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

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

#### <a name="other-format-source"></a>De bron van andere indeling

Het kopiëren van gegevens van HTTP in **ORC/Avro/JSON/binaire indeling**, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de bron voor kopiëren-activiteit moet worden ingesteld op **HttpSource**. | Ja |
| httpRequestTimeout | De time-out (de **TimeSpan** waarde) voor de HTTP-aanvraag reageert. Deze waarde is de time-out voor een antwoord, niet de time-out voor lezen van gegevens van de reactie. De standaardwaarde is **00:01:40**.  | Nee |

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
