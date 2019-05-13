---
title: Gegevens kopiëren van Impala met behulp van Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Impala naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een data factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: f86931aad4eab697e4a0d2dfc47a6d4ff5bfc256
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61401175"
---
# <a name="copy-data-from-impala-by-using-azure-data-factory-preview"></a>Gegevens kopiëren van Impala met behulp van Azure Data Factory (Preview)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Impala. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uitproberen en feedback geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een Impala kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

 Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen. Daarom moet u niet handmatig een stuurprogramma voor het gebruik van deze connector hebt geïnstalleerd.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor de Impala-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Impala service gekoppelde.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Impala**. | Ja |
| host | De IP-adres of de hostnaam naam van de Impala-server (dat wil zeggen, 192.168.222.160).  | Ja |
| port | De TCP-poort die de Impala-server wordt gebruikt om te luisteren naar clientverbindingen. De standaardwaarde is 21050.  | Nee |
| authenticationType | Het verificatietype te gebruiken. <br/>Toegestane waarden zijn **anoniem**, **SASLUsername**, en **UsernameAndPassword**. | Ja |
| username | De gebruikersnaam die wordt gebruikt voor toegang tot de Impala-server. De standaardwaarde is anoniem wanneer u SASLUsername gebruikt.  | Nee |
| password | Het wachtwoord dat overeenkomt met de naam van de gebruiker wanneer u UsernameAndPassword gebruikt. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| enableSsl | Hiermee geeft u op of de verbindingen met de server zijn versleuteld met behulp van SSL. De standaardwaarde is **false**.  | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met vertrouwde CA-certificaten gebruikt om te controleren of de server wanneer u verbinding via SSL maakt. Deze eigenschap kan alleen worden ingesteld als u SSL op zelfgehoste Cloudintegratieruntime. De standaardwaarde is de cacerts.pem-bestand met de integratieruntime geïnstalleerd.  | Nee |
| useSystemTrustStore | Hiermee bepaalt u of u een CA-certificaat uit het archief van de vertrouwensrelatie systeem- of uit een opgegeven PEM-bestand. De standaardwaarde is **false**.  | Nee |
| allowHostNameCNMismatch | Hiermee geeft u op of de naam van een Certificeringsinstantie uitgegeven SSL-certificaat zodat deze overeenkomen met de hostnaam van de server wanneer u verbinding via SSL maakt vereist. De standaardwaarde is **false**.  | Nee |
| allowSelfSignedServerCert | Hiermee geeft u op of zelfondertekende certificaten van de server is toegestaan. De standaardwaarde is **false**.  | Nee |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Impala-gegevensset.

Om gegevens te kopiëren uit een Impala, stel de eigenschap type van de gegevensset in **ImpalaObject**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **ImpalaObject** | Ja |
| tableName | Naam van de tabel. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het brontype Impala.

### <a name="impala-as-a-source-type"></a>Impala als een brontype

Om gegevens te kopiëren uit een Impala, stelt u het brontype in de kopieeractiviteit naar **ImpalaSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **ImpalaSource**. | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
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
                "type": "ImpalaSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
