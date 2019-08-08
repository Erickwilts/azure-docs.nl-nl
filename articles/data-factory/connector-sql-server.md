---
title: Gegevens kopiëren van en naar SQL Server met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens naar en van SQL Server Data Base die on-premises of in een Azure-VM is door gebruik te maken van Azure Data Factory.
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
ms.openlocfilehash: 5dcbb2c25511277eaf46d6c9f4afc007a180f8a6
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827869"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar SQL Server met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Azure Data Factory die u gebruikt:"]
> * [Versie 1:](v1/data-factory-sqlserver-connector.md)
> * [Huidige versie](connector-sql-server.md)

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens van en naar een SQL Server-Data Base te kopiëren. Het is gebaseerd op het artikel overzicht van de [Kopieer activiteit](copy-activity-overview.md) . Dit geeft een algemeen overzicht van de Kopieer activiteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van en naar een Data Base van SQL Server kopiëren naar elk ondersteund Sink-gegevens archief. Of u kunt gegevens van elk ondersteund brongegevens archief kopiëren naar een SQL Server-Data Base. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze SQL Server-connector ondersteunt met name:

- SQL Server versies 2016, 2014, 2012, 2008 R2, 2008 en 2005.
- Gegevens kopiëren met behulp van SQL of Windows-verificatie.
- Als bron, waarbij gegevens worden opgehaald met behulp van een SQL-query of een opgeslagen procedure.
- Het toevoegen van gegevens aan een doel tabel of het aanroepen van een opgeslagen procedure met aangepaste logica tijdens het kopiëren als een sink.

[SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-2017) wordt niet ondersteund.

>[!NOTE]
>SQL Server [Always encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) wordt nu niet ondersteund door deze connector. U kunt dit probleem omzeilen door een [algemene ODBC-Connector](connector-odbc.md) en een SQL Server ODBC-stuur programma te gebruiken. Volg [deze richt lijnen voor het](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-2017) downloaden van ODBC-stuur Programma's en het Connection String van configuraties.

## <a name="prerequisites"></a>Vereisten

Als u gegevens kopiëren van een SQL Server Data Base wilt gebruiken die niet openbaar toegankelijk is, moet u een zelf-hostende Integration runtime instellen. Zie [zelf-hostende Integration runtime](create-self-hosted-integration-runtime.md)voor meer informatie. Integration runtime biedt een ingebouwd SQL Server database stuur programma. U hoeft geen stuur Programma's hand matig te installeren wanneer u gegevens kopieert vanuit of naar de SQL Server-Data Base.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory entiteiten die specifiek zijn voor de SQL Server-database connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de SQL Server gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **sqlserver**. | Ja |
| connectionString |Geef **verbindings reeks** gegevens op die nodig zijn om verbinding te maken met de SQL Server-Data Base met behulp van SQL-verificatie of Windows-verificatie. Raadpleeg de volgende voor beelden.<br/>Markeer dit veld als **SecureString** om het veilig op te slaan in azure Data Factory. U kunt ook een wacht woord in Azure Key Vault plaatsen. Als de SQL-verificatie wordt uitgevoerd, `password` haalt u de configuratie uit het Connection String. Zie voor meer informatie het JSON-voor beeld dat volgt op de tabel en [referenties opslaan in azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| userName |Geef een gebruikers naam op als u Windows-verificatie gebruikt. Een voor beeld **is\\domeinnaam domein naam**. |Nee |
| password |Geef een wacht woord op voor het gebruikers account dat u hebt opgegeven voor de gebruikers naam. Markeer dit veld als **SecureString** om het veilig op te slaan in azure Data Factory. U kunt ook [verwijzen naar een geheim dat is opgeslagen in azure Key Vault](store-credentials-in-key-vault.md). |Nee |
| connectVia | Deze [Integration runtime](concepts-integration-runtime.md) wordt gebruikt om verbinding te maken met het gegevens archief. U kunt een zelf-hostende Integration runtime of Azure Integration runtime gebruiken als uw gegevens archief openbaar toegankelijk is. Als dat niet is opgegeven, wordt de standaard Azure Integration runtime gebruikt. |Nee |

>[!TIP]
>Als u op een fout met de fout code ' UserErrorFailedToConnectToSqlServer ' en een bericht ziet dat de sessie limiet voor de data base xxx is en is bereikt, voegt `Pooling=false` u toe aan uw Connection String en probeert u het opnieuw.

**Voor beeld 1: SQL-verificatie gebruiken**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld 2: SQL-verificatie gebruiken met een wacht woord in Azure Key Vault**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld 3: Windows-verificatie gebruiken**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;"
            },
            "userName": "<domain\\username>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die door de SQL Server-gegevensset worden ondersteund.

De volgende eigenschappen worden ondersteund voor het kopiëren van gegevens van en naar een SQL Server-Data Base:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de DataSet moet worden ingesteld op **SqlServerTable**. | Ja |
| tableName |Deze eigenschap is de naam van de tabel of weer gave in de SQL Server Data Base-instantie waarnaar de gekoppelde service verwijst. | Nee voor bron, Ja voor sink |

**Voorbeeld**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie het artikel [pijp lijnen](concepts-pipelines-activities.md) voor een volledige lijst met secties en eigenschappen die u kunt gebruiken om activiteiten te definiëren. In deze sectie vindt u een lijst met eigenschappen die worden ondersteund door de SQL Server bron en Sink.

### <a name="sql-server-as-a-source"></a>SQL Server als bron

Als u gegevens wilt kopiëren uit SQL Server, stelt u het bron type in de Kopieer activiteit in op **SqlSource**. De volgende eigenschappen worden ondersteund in de sectie bron van de Kopieer activiteit:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **SqlSource**. | Ja |
| sqlReaderQuery |Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Deze eigenschap is de naam van de opgeslagen procedure waarmee gegevens uit de bron tabel worden gelezen. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Deze para meters zijn voor de opgeslagen procedure.<br/>Toegestane waarden zijn de naam of waarde-paren. De namen en het hoofdletter gebruik van para meters moeten overeenkomen met de namen en de behuizing van de opgeslagen procedure parameters. |Nee |

**Punten om te noteren:**

- Als **sqlReaderQuery** is opgegeven voor **SqlSource**, voert de Kopieer activiteit deze query uit op basis van de SQL Server bron om de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door **sqlReaderStoredProcedureName** en **storedProcedureParameters** op te geven als voor de opgeslagen procedure para meters worden gebruikt.
- Als u **sqlReaderQuery** of **sqlReaderStoredProcedureName**niet opgeeft, worden de kolommen die zijn gedefinieerd in de sectie ' Structure ' van de JSON van de gegevensset gebruikt voor het maken van een query. De query `select column1, column2 from mytable` wordt uitgevoerd op basis van de SQL Server. Als de definitie van de gegevensset niet de structuur ' Structure ' bevat, worden alle kolommen geselecteerd in de tabel.

**Voorbeeld: SQL-query gebruiken**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Voorbeeld: Een opgeslagen procedure gebruiken**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**De definitie van de opgeslagen procedure**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sql-server-as-a-sink"></a>SQL Server als Sink

> [!TIP]
> Meer informatie over het ondersteunde schrijf gedrag, configuraties en aanbevolen procedures voor het [laden van gegevens in SQL Server](#best-practice-for-loading-data-into-sql-server).

Als u gegevens wilt kopiëren naar SQL Server, stelt u het sink-type in de Kopieer activiteit in op **SqlSink**. De volgende eigenschappen worden ondersteund in het gedeelte Sink van de Kopieer activiteit:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de Sink voor kopieer activiteiten moet worden ingesteld op **SqlSink**. | Ja |
| writeBatchSize |Het aantal rijen dat *per batch*in de SQL-tabel moet worden ingevoegd.<br/>Toegestane waarden zijn gehele getallen voor het aantal rijen. Standaard bepaalt Azure Data Factory dynamisch de juiste Batch grootte op basis van de Rijgrootte. |Nee |
| writeBatchTimeout |Met deze eigenschap geeft u de wacht tijd op waarna de batch INSERT-bewerking moet worden voltooid voordat er een time-out optreedt.<br/>Toegestane waarden zijn voor de time span. Een voor beeld 00:30:00 is 30 minuten. |Nee |
| preCopyScript |Met deze eigenschap geeft u een SQL-query op voor het uitvoeren van de Kopieer activiteit voordat u gegevens naar SQL Server schrijft. Het wordt slechts één keer per Kopieer bewerking aangeroepen. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |
| sqlWriterStoredProcedureName | De naam van de opgeslagen procedure die definieert hoe bron gegevens in een doel tabel worden toegepast. <br/>Deze opgeslagen procedure wordt *per batch aangeroepen*. Voor bewerkingen die slechts één keer worden uitgevoerd en niets te doen met bron gegevens, bijvoorbeeld verwijderen of afkappen, gebruikt u de `preCopyScript` eigenschap. | Nee |
| storedProcedureTableTypeParameterName |De parameter naam van het tabel type dat is opgegeven in de opgeslagen procedure.  |Nee |
| sqlWriterTableType |De naam van het tabel type dat moet worden gebruikt in de opgeslagen procedure. Met de Kopieer activiteit worden de gegevens in een tijdelijke tabel met dit tabel type beschikbaar gemaakt. Met de opgeslagen procedure code kunt u vervolgens de gegevens samen voegen die worden gekopieerd met bestaande gegevens. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure.<br/>Toegestane waarden zijn naam-en waardeparen. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. | Nee |

**Voor beeld 1: Gegevens toevoegen**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Voor beeld 2: Een opgeslagen procedure aanroepen tijdens het kopiëren**

Meer informatie over [het aanroepen van een opgeslagen procedure vanuit een SQL-Sink](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-sql-server"></a>Aanbevolen procedure voor het laden van gegevens in SQL Server

Wanneer u gegevens naar SQL Server kopieert, hebt u mogelijk een ander schrijf gedrag nodig:

- [Toevoegen](#append-data): Mijn bron gegevens hebben alleen nieuwe records.
- [Upsert](#upsert-data): Mijn bron gegevens hebben zowel toevoegingen als updates.
- [Overschrijven](#overwrite-the-entire-table): Ik wil elke keer de hele dimensie tabel opnieuw laden.
- [Schrijven met aangepaste logica](#write-data-with-custom-logic): Ik heb extra verwerking nodig vóór de laatste invoeging in de doel tabel.

Zie de betreffende secties voor informatie over het configureren van Azure Data Factory en aanbevolen procedures.

### <a name="append-data"></a>Gegevens toevoegen

Het toevoegen van gegevens is het standaard gedrag van deze SQL Server Sink-connector. Azure Data Factory voert een bulksgewijze invoer uit om uw tabel efficiënt te schrijven. U kunt de bron en Sink dienovereenkomstig configureren in de Kopieer activiteit.

### <a name="upsert-data"></a>Upsert-gegevens

**Optie 1:** Wanneer u een grote hoeveelheid gegevens hebt om te kopiëren, gebruikt u de volgende methode om een upsert te maken: 

- Gebruik eerst een [tijdelijke tabel](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) om alle records bulksgewijs te laden met behulp van de Kopieer activiteit. Omdat bewerkingen voor tijdelijke tabellen niet worden geregistreerd, kunt u miljoenen records in enkele seconden laden.
- Voer een opgeslagen procedure-activiteit uit in Azure Data Factory om [](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) een instructie Merge of insert/update toe te passen. Gebruik de tijdelijke tabel als bron om alle updates uit te voeren of als één trans actie in te voegen. Op deze manier wordt het aantal Retouren en logboek bewerkingen gereduceerd. Aan het einde van de opgeslagen procedure-activiteit kan de tijdelijke tabel worden afgekapt zodat deze klaar is voor de volgende upsert-cyclus.

In Azure Data Factory kunt u bijvoorbeeld een pijp lijn maken met een **Kopieer activiteit** die is gekoppeld aan een **opgeslagen procedure activiteit**. De eerste kopie kopieert gegevens uit het bron archief naar een tijdelijke data base-tabel, bijvoorbeeld **# #UpsertTempTable**, als de tabel naam in de gegevensset. Vervolgens roept de laatste een opgeslagen procedure aan om bron gegevens van de tijdelijke tabel samen te voegen in de doel tabel en de tijdelijke tabel op te schonen.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Definieer in uw data base een opgeslagen procedure met SAMENVOEG logica, zoals in het volgende voor beeld, dat verwijst naar de vorige opgeslagen procedure activiteit. Stel dat het doel de **marketing** tabel is met drie kolommen: **ProfileID**, **State**en **Category**. Voer de upsert uit op basis van de kolom **ProfileID** .

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**Optie 2:** U kunt er ook voor kiezen om [een opgeslagen procedure binnen de Kopieer activiteit](#invoke-a-stored-procedure-from-a-sql-sink)aan te roepen. Met deze benadering wordt elke rij in de bron tabel uitgevoerd in plaats van bulksgewijs invoegen als de standaard benadering in de Kopieer activiteit, wat niet van toepassing is op grootschalige upsert.

### <a name="overwrite-the-entire-table"></a>De volledige tabel overschrijven

U kunt de eigenschap **preCopyScript** in een Sink voor kopieer activiteiten configureren. In dit geval voert Azure Data Factory voor elke Kopieer activiteit die wordt uitgevoerd eerst het script uit. Vervolgens wordt de kopie uitgevoerd om de gegevens in te voegen. Als u bijvoorbeeld de volledige tabel wilt overschrijven met de meest recente gegevens, geeft u een script op om eerst alle records te verwijderen voordat u de nieuwe gegevens uit de bron laadt.

### <a name="write-data-with-custom-logic"></a>Gegevens schrijven met aangepaste logica

De stappen voor het schrijven van gegevens met aangepaste logica zijn vergelijkbaar met die in de sectie [Upsert-gegevens](#upsert-data) . Wanneer u extra verwerking wilt Toep assen voordat de laatste invoeging van bron gegevens in de doel tabel, op grote schaal, een van de volgende twee dingen doen: 

- Laden naar een tijdelijke tabel en vervolgens een opgeslagen procedure aanroepen. 
- Een opgeslagen procedure aanroepen tijdens het kopiëren.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a>Een opgeslagen procedure aanroepen vanuit een SQL-Sink

Wanneer u gegevens naar een SQL Server-Data Base kopieert, kunt u ook een door de gebruiker opgegeven opgeslagen procedure met aanvullende para meters configureren en aanroepen. De functie opgeslagen procedure maakt gebruik van [para meters met tabel waarden](https://msdn.microsoft.com/library/bb675163.aspx).

> [!TIP]
> Als u een opgeslagen procedure aanroept, wordt de gegevensrij per rij verwerkt in plaats van met behulp van een bulk bewerking. dit wordt niet aanbevolen voor grootschalige kopieën. Meer informatie over [Best practices voor het laden van gegevens in SQL Server](#best-practice-for-loading-data-into-sql-server).

U kunt een opgeslagen procedure gebruiken wanneer ingebouwde Kopieer mechanismen het doel niet behouden. Een voor beeld hiervan is wanneer u een extra verwerking wilt Toep assen vóór het uiteindelijke invoegen van bron gegevens in de doel tabel. Sommige extra verwerkings voorbeelden zijn wanneer u kolommen wilt samen voegen, aanvullende waarden wilt opzoeken en gegevens in meer dan één tabel wilt invoegen.

In het volgende voor beeld ziet u hoe u een opgeslagen procedure gebruikt om een upsert te maken in een tabel in de SQL Server-Data Base. Stel dat de invoer gegevens en de bron voor de Sink- **marketing** elk drie kolommen bevatten: **ProfileID**, **State**en **Category**. Voer de upsert uit op basis van de kolom **ProfileID** en pas deze alleen toe voor een specifieke categorie met de naam ProductA.

1. Definieer in uw data base het tabel type met de naam **sqlWriterTableType**. Het schema van het tabel type is hetzelfde als het schema dat wordt geretourneerd door de invoer gegevens.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL，
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Definieer in uw data base de opgeslagen procedure met de naam **SqlWriterStoredProcedureName**. De invoer gegevens van de opgegeven bron worden verwerkt en samen voegingen in de uitvoer tabel. De parameter naam van het tabel type in de opgeslagen procedure is hetzelfde als **TableName** gedefinieerd in de gegevensset.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. In Azure Data Factory definieert u de sectie **SQL-Sink** in de Kopieer activiteit als volgt:

    ```json
    "sink": {
        "type": "SqlSink",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "SqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="data-type-mapping-for-sql-server"></a>Toewijzing van gegevens type voor SQL Server

Wanneer u gegevens van en naar SQL Server kopieert, worden de volgende toewijzingen gebruikt van SQL Server gegevens typen om tussenliggende gegevens typen te Azure Data Factory. Zie [schema en gegevens type toewijzingen](copy-activity-schema-and-type-mapping.md)voor meer informatie over hoe de Kopieer activiteit het bron schema en het gegevens type aan de Sink koppelt.

| SQL Server gegevens type | Azure Data Factory tussentijds gegevens type |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Voor gegevens typen die worden toegewezen aan het type van de tijdelijke decimalen, Azure Data Factory op dit moment de precisie Maxi maal 28 ondersteunen. Als u gegevens hebt die een grotere nauw keurigheid dan 28 vereisen, kunt u overwegen om te converteren naar een teken reeks in een SQL-query.

## <a name="troubleshoot-connection-issues"></a>Verbindings problemen oplossen

1. Configureer uw SQL Server-exemplaar om externe verbindingen te accepteren. Start **SQL Server Management Studio**, klik met de rechter muisknop op **Server**en selecteer **Eigenschappen**. Selecteer **verbindingen** in de lijst en schakel het selectie vakje **externe verbindingen met deze server toestaan** in.

    ![Externe verbindingen inschakelen](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Zie [Configure the RAS server Configuration option](https://msdn.microsoft.com/library/ms191464.aspx)(Engelstalig) voor gedetailleerde stappen.

2. Start **SQL Server Configuration Manager**. Vouw **SQL Server netwerk configuratie** voor de gewenste instantie uit en selecteer **protocollen voor MSSQLSERVER**. Protocollen worden weer gegeven in het rechterdeel venster. Schakel TCP/IP in door met de rechter muisknop op **TCP/IP** te klikken en **inschakelen**te selecteren.

    ![TCP/IP inschakelen](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Zie [een server netwerkprotocol in-of uitschakelen](https://msdn.microsoft.com/library/ms191294.aspx)voor meer informatie en alternatieve manieren om het TCP/IP-protocol in te scha kelen.

3. Dubbel klik in hetzelfde venster op **TCP/IP** om het venster **TCP/IP-eigenschappen** te openen.
4. Schakel over naar het tabblad **IP-adressen** . Schuif omlaag om de sectie **IPAll** weer te geven. Noteer de **TCP-poort**. De standaard waarde is **1433**.
5. Maak een **regel voor de Windows Firewall** op de computer om binnenkomend verkeer via deze poort toe te staan. 
6. **Verbinding controleren**: Als u verbinding wilt maken met SQL Server met behulp van een volledig gekwalificeerde naam, gebruikt u SQL Server Management Studio vanaf een andere computer. Een voorbeeld is `"<machine>.<domain>.corp.<company>.com,1433"`.

## <a name="next-steps"></a>Volgende stappen
Zie [ondersteunde gegevens archieven](copy-activity-overview.md##supported-data-stores-and-formats)voor een lijst met gegevens archieven die worden ondersteund als bronnen en sinks op basis van de Kopieer activiteit in azure Data Factory.
