---
title: Gegevens kopiëren van en naar Salesforce met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens uit Salesforce aan ondersteunde sink-gegevensopslag of ondersteunde bron-gegevensopslag met Salesforce met behulp van een kopieeractiviteit in een data factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/19/2019
ms.author: jingwang
ms.openlocfilehash: 6056df9aa9079887bfb06ca20ad564eb52baff38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60546546"
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Salesforce met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-salesforce-connector.md)
> * [Huidige versie](connector-salesforce.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar Salesforce. Dit is gebaseerd op de [overzicht van Kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Salesforce kopiëren naar een ondersteunde sink-gegevensopslag. U kunt ook gegevens uit een ondersteund brongegevensarchief kopiëren naar Salesforce. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Salesforce-connector:

- Salesforce Developer, Professional, Enterprise, or Unlimited editions.
- Kopiëren van gegevens van en naar Salesforce-productie, sandbox- en aangepaste domein.

De Salesforce-connector is gebaseerd op de API voor Salesforce-REST/bulksgewijs met [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) voor gegevens kopiëren van en [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) om gegevens te kopiëren.

## <a name="prerequisites"></a>Vereisten

API-machtiging moet zijn ingeschakeld in Salesforce. Zie voor meer informatie, [inschakelen API-toegang in Salesforce door machtigingenset](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>SalesForce-aanvraaglimieten

SalesForce heeft limieten voor totaal aantal API-aanvragen en gelijktijdige API-aanvragen. Houd rekening met de volgende punten:

- Als het aantal gelijktijdige aanvragen van de limiet overschrijdt, beperking optreedt en u willekeurige mislukte tests ziet.
- Als het totale aantal aanvragen heeft de limiet overschrijdt, wordt het Salesforce-account is geblokkeerd voor 24 uur.

U kunt ook het foutbericht 'REQUEST_LIMIT_EXCEEDED' in beide scenario's. Zie voor meer informatie de sectie 'Aanvraaglimieten API' in [Salesforce developer limieten](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor de Salesforce-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Salesforce die zijn gekoppeld.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op **Salesforce**. |Ja |
| environmentUrl | Geef de URL van het exemplaar van Salesforce. <br> -Standaard is `"https://login.salesforce.com"`. <br> -Om gegevens te kopiëren van sandbox, geef `"https://test.salesforce.com"`. <br> -Om gegevens te kopiëren uit aangepaste domein, opgeven, bijvoorbeeld `"https://[domain].my.salesforce.com"`. |Nee |
| username |Geef een gebruikersnaam voor het gebruikersaccount. |Ja |
| password |Geef een wachtwoord voor het gebruikersaccount.<br/><br/>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| securityToken |Geef een beveiligingstoken voor het gebruikersaccount. Zie voor instructies over het opnieuw instellen en ophalen van een beveiligingstoken [ophalen van een beveiligingstoken](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Zie voor meer informatie over beveiligingstokens het in het algemeen, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Niet voor bron, Ja voor sink als de bron gekoppelde service beschikt niet over integratieruntime |

>[!IMPORTANT]
>Wanneer u gegevens naar Salesforce kopiëren, kan de standaard Azure Integration Runtime kan niet worden gebruikt voor het uitvoeren van de kopie. Met andere woorden, als uw bron gekoppelde service beschikt niet over een opgegeven integration-runtime expliciet [maken van een Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Salesforce-exemplaar. Koppel de Salesforce gekoppelde service zoals in het volgende voorbeeld.

**Voorbeeld: Store-referenties in Data Factory**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: Store-referenties in Key Vault**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Salesforce-gegevensset.

Om gegevens te kopiëren van en naar Salesforce, stel de eigenschap type van de gegevensset in **SalesforceObject**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SalesforceObject**.  | Ja |
| objectApiName | De Salesforce-objectnaam om gegevens uit te halen. | Nee voor bron, Ja voor sink |

> [!IMPORTANT]
> Het gedeelte "__c" van **API-naam** nodig is voor een aangepaste object.

![De verbinding voor Factory Salesforce API-naam](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Voorbeeld:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Voor achterwaartse compatibiliteit: Wanneer u gegevens van Salesforce, kopieert als u de vorige "RelationalTable" type gegevensset gebruikt, blijft deze werken terwijl ziet u een suggestie om over te schakelen naar de nieuwe 'SalesforceObject'-type.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **RelationalTable**. | Ja |
| tableName | Naam van de tabel in Salesforce. | Nee (als 'query' in de bron van de activiteit is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Salesforce-bron en sink.

### <a name="salesforce-as-a-source-type"></a>SalesForce als een brontype

Voor het kopiëren van gegevens uit Salesforce, stelt u het brontype in de kopieeractiviteit naar **SalesforceSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **SalesforceSource**. | Ja |
| query |De aangepaste query gebruiken om gegevens te lezen. U kunt [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query of 92 SQL-query. Bekijk meer tips in [tips query](#query-tips) sectie. Als de query is niet opgegeven, worden alle gegevens van de Salesforce-object dat is opgegeven in 'objectApiName' in de gegevensset worden opgehaald. | Nee (als 'objectApiName' in de gegevensset is opgegeven) |
| readBehavior | Hiermee wordt aangegeven of de records van bestaande query, of query alle records ook verwijderd die zijn. Indien niet opgegeven, is het standaardgedrag is het eerste. <br>Toegestane waarden: **query** (standaard), **queryAll**.  | Nee |

> [!IMPORTANT]
> Het gedeelte "__c" van **API-naam** nodig is voor een aangepaste object.

![Data Factory Salesforce-verbinding de naam van de API-lijst](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Voor achterwaartse compatibiliteit: Wanneer u gegevens van Salesforce, kopieert als u de vorige versie van de 'RelationalSource'-type gebruikt, wordt de bron blijven werken terwijl ziet u een suggestie om over te schakelen naar de nieuwe 'SalesforceSource'-type.

### <a name="salesforce-as-a-sink-type"></a>SalesForce als een sink-type

Om gegevens te kopiëren naar Salesforce, stelt u het sink-type in de kopieeractiviteit naar **SalesforceSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **SalesforceSink**. | Ja |
| writeBehavior | Het gedrag van het schrijven voor de bewerking.<br/>Toegestane waarden zijn **invoegen** en **Upsert**. | Nee (de standaardinstelling is invoegen) |
| externalIdFieldName | De naam van de externe ID-veld voor de upsert-bewerking. Het opgegeven veld moet worden gedefinieerd als 'Externe Id-veld' in het Salesforce-object. Er kan geen NULL-waarden in de bijbehorende invoergegevens. | Ja voor "Upsert" |
| writeBatchSize | Het aantal rijen van de gegevens die naar Salesforce is geschreven in elke batch. | Nee (de standaardinstelling is 5.000) |
| ignoreNullValues | Hiermee wordt aangegeven of NULL-waarden van invoergegevens tijdens een schrijfactie negeren.<br/>Toegestane waarden zijn **waar** en **false**.<br>- **True**: Laat de gegevens in het doelobject ongewijzigd wanneer u een upsert of update-bewerking. Voeg een gedefinieerde standaardwaarde wanneer u een insert-bewerking.<br/>- **False**: Als u een upsert of update-bewerking doet, moet u de gegevens in het doelobject bijwerken op NULL. Voeg een NULL-waarde als u een insert-bewerking. | Nee (de standaardinstelling is false) |

**Voorbeeld: SalesForce-sink in een kopieeractiviteit**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Querytips

### <a name="retrieve-data-from-a-salesforce-report"></a>Gegevens ophalen uit een Salesforce-rapport

U kunt gegevens uit Salesforce-rapporten ophalen door het opgeven van een query als `{call "<report name>"}`. Een voorbeeld is `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Verwijderde records uit de Prullenbak van Salesforce opgehaald

Om te vragen het voorlopig verwijderde records uit de Prullenbak van Salesforce, kunt u `readBehavior` als `queryAll`. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>Verschil tussen SOQL en SQL-querysyntaxis

Het kopiëren van gegevens uit Salesforce, kunt u SOQL query of SQL-query. Houd er rekening mee dat deze twee verschillende syntaxis en functionaliteit ondersteuning heeft, niet combineren. U worden voorgesteld om de SOQL-query die wordt ondersteund door Salesforce te gebruiken. De volgende tabel bevat de belangrijkste verschillen:

| Syntaxis | SOQL modus | SQL-modus |
|:--- |:--- |:--- |
| Kolom selecteren | Moet de velden die moeten worden gekopieerd in de query, bijvoorbeeld opsommen `SELECT field1, filed2 FROM objectname` | `SELECT *` naast de kolom selecteren wordt ondersteund. |
| Aanhalingstekens | Gearchiveerde/objectnamen kunnen niet worden vermeld. | Veld/objectnamen kunnen tussen aanhalingstekens staan, bijvoorbeeld `SELECT "id" FROM "Account"` |
| Datum-/ tijdindeling |  Raadpleeg de details [hier](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) en voorbeelden in de volgende sectie. | Raadpleeg de details [hier](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017) en voorbeelden in de volgende sectie. |
| Booleaanse waarden | Weergegeven als `False` en `True`, bijvoorbeeld `SELECT … WHERE IsDeleted=True`. | Weergegeven als 0 of 1, bijvoorbeeld `SELECT … WHERE IsDeleted=1`. |
| Naam van kolom wijzigen | Wordt niet ondersteund. | Ondersteund, bijvoorbeeld: `SELECT a AS b FROM …`. |
| Relatie | Ondersteund, bijvoorbeeld `Account_vod__r.nvs_Country__c`. | Wordt niet ondersteund. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Gegevens ophalen met behulp van een where component voor de datum/tijd-kolom

Wanneer u de SOQL of SQL-query opgeven, betaalt u aandacht wordt besteed aan het verschil van datum/tijd-indeling. Bijvoorbeeld:

* **Voorbeeld van SOQL**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL-voorbeeld**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformedquerytruncated"></a>Fout van MALFORMED_QUERY: ingekort

Als u de fout van bereikt "MALFORMED_QUERY: "Wordt afgekapt, normaal gesproken is vanwege hebt typekolom JunctionIdList in gegevens en Salesforce geldt een beperking voor ondersteuning van dergelijke gegevens met een groot aantal rijen. Als u wilt oplossen, wilt uitsluiten JunctionIdList kolom of Beperk het aantal rijen dat u wilt kopiëren (u kunt partitioneren op meerdere kopiëren voor de uitvoeringen van activiteit).

## <a name="data-type-mapping-for-salesforce"></a>Toewijzing voor Salesforce-gegevenstype

Wanneer u gegevens van Salesforce worden gekopieerd, worden de volgende toewijzingen uit Salesforce-gegevenstypen gebruikt om Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe de kopieerbewerking het schema en de gegevens van een brontype aan de sink toegewezen, [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| SalesForce-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| Auto Number |String |
| Checkbox |Boolean |
| Currency |Decimal |
| Date |DateTime |
| Date/Time |DateTime |
| Email |String |
| Id |String |
| Lookup Relationship |String |
| Multi-Select Picklist |String |
| Number |Decimal |
| Percent |Decimal |
| Phone |String |
| Picklist |String |
| Text |String |
| Text Area |String |
| Text Area (Long) |String |
| Text Area (Rich) |String |
| Text (Encrypted) |String |
| URL |String |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
