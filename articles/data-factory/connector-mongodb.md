---
title: Gegevens kopiëren van MongoDB met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens uit Mongo DB naar ondersteunde Sink-gegevens archieven door gebruik te maken van een Kopieer activiteit in een Azure Data Factory-pijp lijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: 03ad098b2f83341150a59247f47b9a4abaa1b9d2
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726095"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Gegevens kopiëren van MongoDB met behulp van Azure Data Factory

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens uit een MongoDB-data base te kopiëren. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

>[!IMPORTANT]
>ADF release deze nieuwe versie van de MongoDB-connector, die betere ondersteuning voor systeem eigen MongoDB biedt. Raadpleeg het artikel [MongoDb connector (verouderd)](connector-mongodb-legacy.md) als u de vorige MongoDb-connector gebruikt in uw oplossing die wordt ondersteund als-is voor achterwaartse compatibiliteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MongoDB-data base kopiëren naar elk ondersteund Sink-gegevens archief. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze MongoDB-connector ondersteunt **met name versie 3,4**.

## <a name="prerequisites"></a>Vereisten

Als u gegevens wilt kopiëren uit een MongoDB-data base die niet openbaar toegankelijk is, moet u een zelf-hostende Integration Runtime instellen. Zie [zelf-hostende Integration runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek zijn voor MongoDB-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MongoDB gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **MongoDbV2** |Ja |
| connectionString |Geef de MongoDB op connection string bijvoorbeeld `mongodb://[username:password@]host[:port][/[database][?options]]`. Raadpleeg de [hand leiding voor MongoDb op Connection String](https://docs.mongodb.com/manual/reference/connection-string/) voor meer informatie. <br/><br />Dit veld als markeert een **SecureString** type voor het veilig opslaan in Data Factory. U kunt ook [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| database | De naam van de data base die u wilt openen. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDbV2",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "mongodb://[username:password@]host[:port][/[database][?options]]"
            },
            "database": "myDatabase"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). De volgende eigenschappen worden ondersteund voor MongoDB-gegevensset:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **MongoDbV2Collection** | Ja |
| collectionName |De naam van de verzameling in de MongoDB-data base. |Ja |

**Voorbeeld:**

```json
{
    "name": "MongoDbDataset",
    "properties": {
        "type": "MongoDbV2Collection",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door MongoDB-bron worden ondersteund.

### <a name="mongodb-as-source"></a>MongoDB als bron

De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet worden ingesteld op: **MongoDbV2Source** | Ja |
| filter | Hiermee geeft u selectie filter op met behulp van query operators. Als u alle documenten in een verzameling wilt retour neren, laat u deze para meter weg{}of geeft u een leeg document () door. | Nee |
| cursorMethods.project | Hiermee geeft u de velden op die moeten worden geretourneerd in de documenten voor projectie. Als u alle velden in de overeenkomende documenten wilt retour neren, laat u deze para meter weg. | Nee |
| cursorMethods.sort | Hiermee geeft u de volg orde waarin de query overeenkomende documenten retourneert. Raadpleeg [cursor. sort ()](https://docs.mongodb.com/manual/reference/method/cursor.sort/#cursor.sort). | Nee |
| cursorMethods.limit | Hiermee geeft u het maximum aantal documenten op dat door de server wordt geretourneerd. Raadpleeg [cursor. Limit ()](https://docs.mongodb.com/manual/reference/method/cursor.limit/#cursor.limit).  | Nee |
| cursorMethods.skip | Hiermee geeft u het aantal documenten op dat moet worden overgeslagen en waar MongoDB begint met het retour neren van resultaten. Raadpleeg [cursor. Skip ()](https://docs.mongodb.com/manual/reference/method/cursor.skip/#cursor.skip). | Nee |
| batchSize | Hiermee geeft u het aantal documenten op dat moet worden geretourneerd in elke batch van het antwoord van de MongoDB-instantie. In de meeste gevallen heeft het wijzigen van de Batch grootte geen invloed op de gebruiker of de toepassing. Cosmos DB limieten voor elke batch kan niet groter zijn dan 40MB. Dit is de som van de grootte van het batchSize aantal documenten. Verlaag deze waarde dus als de grootte van het document groot is. | Nee<br/>(de standaard waarde is **100**) |

>[!TIP]
>ADF-ondersteuning die BSON-document verbruikt in de **strikte modus**. Zorg ervoor dat uw filter query in de strikte modus in plaats van in de shell modus is. Meer beschrijving vindt u in [MongoDb hand matig](https://docs.mongodb.com/manual/reference/mongodb-extended-json/index.html).

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
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
                "type": "MongoDbV2Source",
                "filter": "{datetimeData: {$gte: ISODate(\"2018-12-11T00:00:00.000Z\"),$lt: ISODate(\"2018-12-12T00:00:00.000Z\")}, _id: ObjectId(\"5acd7c3d0000000000000000\") }",
                "cursorMethods": {
                    "project": "{ _id : 1, name : 1, age: 1, datetimeData: 1 }",
                    "sort": "{ age : 1 }",
                    "skip": 3,
                    "limit": 3
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="export-json-documents-as-is"></a>JSON-documenten exporteren als-is

U kunt deze MongoDB-connector gebruiken om JSON-documenten te exporteren als-afkomstig van een MongoDB-verzameling naar verschillende op bestanden gebaseerde archieven of Azure Cosmos DB. Om een dergelijke schema-neutraal kopie te krijgen, slaat u de sectie ' Structure ' (ook wel *schema*genoemd) in de gegevensset en schema toewijzing in de Kopieer activiteit over.

## <a name="schema-mapping"></a>Schema toewijzing

Zie [schema toewijzing](copy-activity-schema-and-type-mapping.md#schema-mapping)als u gegevens van MongoDb naar tabellaire Sink wilt kopiëren.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
