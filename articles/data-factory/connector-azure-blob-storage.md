---
title: Gegevens kopiëren naar of van Azure Blob-opslag met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens van ondersteunde bron-gegevensopslag naar Azure Blob storage of van Blob storage naar een ondersteunde sink-gegevensopslag, met behulp van Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 316406b25558b7652329bf2690eaebfa4b52c4ce
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142939"
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Gegevens kopiëren naar of van Azure Blob-opslag met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-azure-blob-connector.md)
> * [Huidige versie](connector-azure-blob-storage.md)

In dit artikel bevat een overzicht van hoe u het kopiëren van gegevens naar en vanuit Azure Blob storage. Lees voor meer informatie over Azure Data Factory, de [inleidende artikel](introduction.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze Azure Blob-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieeractiviteit](copy-activity-overview.md) met [ondersteunde bron/sink-matrix](copy-activity-overview.md)
- [Toewijzing van de gegevensstroom](concepts-data-flow-overview.md)
- [Activiteit Lookup](control-flow-lookup-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)

Deze Blob storage-connector ondersteunt name:

- Kopiëren van blobs en naar Azure-opslagaccounts voor algemeen gebruik en warme/koude blob-opslag. 
- Blobs kopiëren met behulp van de sleutel, service-handtekening voor gedeelde toegang, service-principal of beheerde identiteiten van Azure-resources-verificaties.
- Kopiëren blobs uit blokken, toevoegen of pagina-blobs en kopiëren van gegevens naar alleen blok-blobs.
- Blobs kopiëren of parseren of genereren van blobs met [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md).

>[!NOTE]
>Als u inschakelt de _'Vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan'_ optie voor Azure Storage firewall-instellingen, met behulp van Azure Integration Runtime verbinding maken met Blob-opslag mislukken met een niet-toegestane fout, omdat de ADF niet Als een vertrouwde Microsoft-service behandeld. Maak in plaats daarvan verbinding via een zelfgehoste Cloudintegratieruntime.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Blob-opslag.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

Azure Blob-connector ondersteunt de volgende verificatietypen, verwijzen naar de overeenkomstige sectie voor meer informatie:

- [Verificatie van account-sleutel](#account-key-authentication)
- [Shared access signature-verificatie](#shared-access-signature-authentication)
- [Verificatie van service-principal](#service-principal-authentication)
- [Beheerde identiteiten voor verificatie van de Azure-resources](#managed-identity)

>[!NOTE]
>HDInsights, Azure Machine Learning en Azure SQL Data Warehouse PolyBase load alleen ondersteuning voor verificatie met Azure Blob storage-account sleutel.

### <a name="account-key-authentication"></a>Verificatie van account-sleutel

Voor het gebruik van storage-account sleutelverificatie, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Azure BLOB Storage** (aanbevolen) of **AzureStorage** (Zie opmerkingen hieronder). |Ja |
| connectionString | Geef de informatie die nodig zijn voor het verbinding maken met opslag voor de connectionString-eigenschap. <br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook accountsleutel plaatsen in Azure Key Vault en pull de `accountKey` configuratie buiten de verbindingsreeks. Raadpleeg de volgende voorbeelden en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Als u "AzureStorage" type gekoppelde service zijn gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u het gebruik van deze nieuwe 'Azure BLOB Storage"gekoppeld servicetype voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: accountsleutel opslaan in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;"
            },
            "accountKey": { 
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

### <a name="shared-access-signature-authentication"></a>Shared access signature-verificatie

Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. U kunt een shared access signature toekennen van dat een client beperkte machtigingen voor objecten in uw storage-account voor een opgegeven periode. U hebt geen delen van de toegangssleutels van uw account. De shared access signature is een URI die in de queryparameters alle informatie die nodig zijn voor geverifieerde toegang tot een opslagresource omvat. Voor toegang tot de storage-resources met de shared access signature, moet de client alleen worden doorgegeven in de handtekening voor gedeelde toegang tot de juiste constructor of methode. Zie voor meer informatie over handtekeningen voor gedeelde toegang, [handtekeningen voor gedeelde toegang: Inzicht in het shared access signature-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory ondersteunt nu zowel **service gedeelde toegangshandtekeningen** en **account gedeelde toegangshandtekeningen**. Zie voor meer informatie over deze twee typen en hoe u een van deze [typen handtekeningen voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures).
>- In latere configuratie van de gegevensset is het pad naar het absolute pad vanaf het niveau van de container. U moet een afgestemd op het pad naar uw SAS-URI configureren.

> [!TIP]
> U kunt de volgende PowerShell-opdrachten uitvoeren voor het genereren van een gedeelde-toegangshandtekening van de service voor uw opslagaccount. Vervang de tijdelijke aanduidingen en de benodigde machtiging verlenen.
> `$context = New-AzStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Voor het gebruik van shared access signature-verificatie, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Azure BLOB Storage** (aanbevolen) of **AzureStorage** (Zie opmerkingen hieronder). |Ja |
| sasUri | Geef de shared access signature URI voor de Storage-resources, zoals de blob-container. <br/>Dit veld markeert als een SecureString Bewaar deze zorgvuldig in Data Factory. U kunt ook SAS-token plaatsen in Azure Key Vault gebruikmaken van automatisch draaien en het token gedeelte verwijderen. Raadpleeg de volgende voorbeelden en [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de Azure Integration Runtime of de zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Als u "AzureStorage" type gekoppelde service zijn gebruikt, wordt deze nog steeds ondersteund als-is, terwijl u het gebruik van deze nieuwe 'Azure BLOB Storage"gekoppeld servicetype voortaan worden voorgesteld.

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<container>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: accountsleutel opslaan in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<container>.blob.core.windows.net/>"
            },
            "sasToken": { 
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

Wanneer u een shared access signature URI maakt, moet u de volgende punten overwegen:

- Geschikt voor lezen/schrijven-machtigingen instellen voor objecten die zijn gebaseerd op hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw data factory.
- Stel **verlooptijd** op de juiste wijze. Zorg ervoor dat de toegang tot de Storage-objecten niet binnen een actieve periode van de pijplijn verloopt.
- De URI moet worden gemaakt op de juiste container/blob op basis van de behoeften. Een shared access signature URI naar een blob kan Data Factory voor toegang tot die specifieke blob. Een shared access signature URI naar een Blob storage-container kunt Data Factory u doorloopt de blobs in deze container. Voor toegang tot meer of minder objecten later of bijwerken van de shared access signature URI, vergeet niet om bij te werken van de gekoppelde service met de nieuwe URI.

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Voor verificatie van Azure Storage-service-principal in het algemeen, raadpleegt u [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory](../storage/common/storage-auth-aad.md).

Volg deze stappen voor het gebruik van service-principal verificatie:

1. Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) door [uw toepassing registreren bij een Azure AD-tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

    - Toepassings-id
    - Toepassingssleutel
    - Tenant-id

2. Geef de service principal juiste toestemming in Azure Blob-opslag. Raadpleeg [beheren rechten voor het Azure Storage-gegevens met RBAC](../storage/common/storage-auth-aad-rbac.md) met meer informatie over de rollen.

    - **Als bron**, toegang beheren (IAM), ten minste verlenen **gegevenslezer voor Opslagblob** rol.
    - **Als sink**, toegang beheren (IAM), ten minste verlenen **Gegevensbijdrager voor Blob** rol.

Deze eigenschappen worden ondersteund voor een Azure Blob storage gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Azure BLOB Storage**. |Ja |
| serviceEndpoint | Geef het Azure Blob storage service-eindpunt met het patroon van `https://<accountName>.blob.core.windows.net/`. |Ja |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing. Dit veld als markeert een **SecureString** voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. Deze ophalen door de muis in de rechterbovenhoek van de Azure-portal. | Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Service-principal verificatie wordt alleen ondersteund door 'Azure BLOB Storage'-type gekoppelde service, maar niet vorige "AzureStorage" type gekoppelde service.

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Beheerde identiteiten voor verificatie van de Azure-resources

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor de Azure-resources](data-factory-service-identity.md), die staat voor deze specifieke data factory. U kunt deze beheerde identiteit rechtstreeks gebruiken voor Blob storage-verificatie zijn vergelijkbaar met het gebruik van uw eigen service-principal. Hiermee kunt deze aangewezen factory toegang en gegevens kopiëren van/naar de Blob-opslag.

Raadpleeg [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory](../storage/common/storage-auth-aad.md) voor verificatie in het algemeen Azure Storage. Voor het gebruik van beheerde identiteiten voor verificatie van de Azure-resources, de volgende stappen uit:

1. [Ophalen van gegevens van data factory beheerde identiteit](data-factory-service-identity.md#retrieve-managed-identity) door de waarde van "SERVICE-identiteit TOEPASSINGS-ID gegenereerd samen met uw factory kopiëren.

2. Verleen de juiste machtiging beheerde identiteit in Azure Blob-opslag. Raadpleeg [beheren rechten voor het Azure Storage-gegevens met RBAC](../storage/common/storage-auth-aad-rbac.md) met meer informatie over de rollen.

    - **Als bron**, toegang beheren (IAM), ten minste verlenen **gegevenslezer voor Opslagblob** rol.
    - **Als sink**, toegang beheren (IAM), ten minste verlenen **Gegevensbijdrager voor Blob** rol.

Deze eigenschappen worden ondersteund voor een Azure Blob storage gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Azure BLOB Storage**. |Ja |
| serviceEndpoint | Geef het Azure Blob storage service-eindpunt met het patroon van `https://<accountName>.blob.core.windows.net/`. |Ja |
| connectVia | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als uw gegevensopslag in een particulier netwerk is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

> [!NOTE]
> Beheerde identiteiten voor Azure-resources verificatie wordt alleen ondersteund door "Azure BLOB Storage" type gekoppelde service, maar niet vorige "AzureStorage" type gekoppelde service. 

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {            
            "serviceEndpoint": "https://<accountName>.blob.core.windows.net/"
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

Als u wilt kopiëren van gegevens naar en van Blob-opslag in Parquet of gescheiden tekstopmaak, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de gegevensset op basis van indeling en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure-Blob onder `location` instellingen in de indeling op basis van gegevensset:

| Eigenschap   | Description                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type van de locatie in de gegevensset moet worden ingesteld op **AzureBlobStorageLocation**. | Ja      |
| container  | De blob-container.                                          | Ja      |
| folderPath | Het pad naar map onder de opgegeven container. Als u wilt met jokertekens map filteren, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |
| fileName   | De bestandsnaam van het onder de opgegeven container + folderPath. Als u wilt een jokerteken gebruiken om te filteren van bestanden, deze instelling overslaan en geef in instellingen voor de bron. | Nee       |

> [!NOTE]
>
> **AzureBlob** type gegevensset met Parquet/tekstindeling die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor het kopiëren/Lookup/de GET metadata activity voor compatibiliteit met eerdere versies, maar werkt niet met gegevensstroom toewijzen. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder"
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

Als u wilt kopiëren van gegevens naar en van Blob-opslag in ORC/Avro/JSON/binaire indeling, stel de eigenschap type van de gegevensset in **AzureBlob**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **AzureBlob**. |Ja |
| folderPath | Pad naar de container en map in de blob-opslag. <br/><br/>Filteren op jokerteken wordt ondersteund voor het pad met uitzondering van de containernaam van de. Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br/><br/>Voorbeelden: myblobcontainer/myblobfolder/meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). |Ja voor kopiëren/Lookup-activiteit, niet voor de activiteit GetMetadata |
| fileName | **Naam of het jokerteken filter** voor de BLOB (s) onder het opgegeven 'folderPath'. Als u een waarde voor deze eigenschap niet opgeeft, wordt de gegevensset verwijst naar alle blobs in de map. <br/><br/>Voor het filter toegestane jokertekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voorbeeld 1: `"fileName": "*.csv"`<br/>-Voorbeeld 2: `"fileName": "???20180427.txt"`<br/>Gebruik `^` als escape voor als de bestandsnaam van uw werkelijke jokertekens of deze escape-teken in.<br/><br/>Wanneer de bestandsnaam is niet opgegeven voor een uitvoergegevensset en **preserveHierarchy** is niet opgegeven in de activiteit-sink, de kopieeractiviteit wordt automatisch gegenereerd met de naam van de blob met het volgende patroon: "*Gegevens. [activiteit uitgevoerd GUID-ID]. [GUID als FlattenHierarchy]. [de indeling als geconfigureerd]. [compressie als geconfigureerd]* ", bijvoorbeeld 'Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz'; Als u vanuit in tabelvorm bron met de tabelnaam van de in plaats van de query kopieert, het naampatroon is '*[tabelnaam]. [ indeling]. [compressie als geconfigureerd]* ", bijvoorbeeld 'MyTable.csv'. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br/><br/> Let erop dat de algehele prestaties van de verplaatsing van gegevens zullen worden beïnvloed door deze instelling is ingeschakeld als u wilt filter-bestand uit de enorme hoeveelheden bestanden. <br/><br/> De eigenschappen kunnen niet null zijn dat betekenen dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd.| Nee |
| format | Als u wilt kopiëren van bestanden als tussen winkels op basis van bestanden (binaire kopie), moet u de sectie indeling in de invoer en uitvoer gegevenssetdefinities overslaan.<br/><br/>Als u wilt parseren of bestanden met een specifieke indeling genereren, worden de volgende indeling bestandstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, en **ParquetFormat**. Stel de **type** eigenschap onder **indeling** op een van deze waarden. Zie voor meer informatie de [tekstindeling](supported-file-formats-and-compression-codecs.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs.md#json-format), [Avro-indeling](supported-file-formats-and-compression-codecs.md#avro-format), [Orc-indeling](supported-file-formats-and-compression-codecs.md#orc-format), en [Parquet-indeling ](supported-file-formats-and-compression-codecs.md#parquet-format) secties. |Nee (alleen voor binaire kopie-scenario) |
| compression | Geef het type en het niveau van compressie voor de gegevens. Zie voor meer informatie, [ondersteunde indelingen en codecs voor compressie](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Ondersteunde typen zijn **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimale** en **snelst**. |Nee |

>[!TIP]
>Als u wilt alle blobs in een map kopieert, geef **folderPath** alleen.<br>Voor het kopiëren van één blob met een specifieke naam, geef **folderPath** met maponderdeel en **fileName** met bestandsnaam.<br>Als u wilt kopiëren van een subset van de blobs in een map, geef **folderPath** met maponderdeel en **fileName** met filteren op jokerteken. 

**Voorbeeld:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Blob storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Blob storage-bron en sink.

### <a name="blob-storage-as-a-source-type"></a>BLOB-opslag als een brontype

- Voor het kopiëren van **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling bron](#parquet-and-delimited-text-format-source) sectie.
- Voor het kopiëren van andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere bron indeling](#other-format-source) sectie.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet en tekst met scheidingstekens indeling bron

Om gegevens te kopiëren van Blob-opslag in Parquet of gescheiden tekstopmaak, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op de bron voor kopiëren-indeling op basis van activiteit en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure-Blob onder `storeSettings` instellingen in de bron voor kopiëren-indeling op basis van:

| Eigenschap                 | Description                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobStorageReadSetting**. | Ja                                           |
| recursive                | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink. Toegestane waarden zijn **waar** (standaard) en **false**. | Nee                                            |
| wildcardFolderPath       | Het pad met jokertekens onder de opgegeven container in de gegevensset aan filter bronmappen geconfigureerd. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in. <br>Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| wildcardFileName         | De naam van het bestand met jokertekens onder de opgegeven container + folderPath/wildcardFolderPath naar de bronbestanden filter. <br>Jokertekens zijn toegestaan: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); Gebruik `^` als escape voor als de naam van uw map zelf jokertekens of deze escape-teken in.  Meer voorbeelden in [mappen en bestanden filteren voorbeelden](#folder-and-file-filter-examples). | Ja als `fileName` niet is opgegeven in de gegevensset |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: Het laatst is gewijzigd. De bestanden worden geselecteerd als hun laatst gewijzigd binnen het tijdsinterval tussen zijn `modifiedDatetimeStart` en `modifiedDatetimeEnd`. De tijd wordt toegepast op de UTC-tijdzone in de notatie ' 2018-12-01T05:00:00Z '. <br> De eigenschappen is NULL. Dit betekent dat er geen kenmerk bestandsfilter worden toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` datum / tijdwaarde heeft, maar `modifiedDatetimeEnd` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde groter dan is of gelijk is aan de datum / tijdwaarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` datum / tijdwaarde heeft, maar `modifiedDatetimeStart` NULL is, betekent dit dat de bestanden waarvan het kenmerk Laatst gewijzigde lager is dan de datum / tijdwaarde wordt geselecteerd. | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee                                            |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **BlobSource** type activiteit kopieerbron vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
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
                    "type": "AzureBlobStorageReadSetting",
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

Om gegevens te kopiëren van Blob-opslag in ORC/Avro/JSON/binaire indeling, stelt u het brontype in de kopieeractiviteit naar **BlobSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **BlobSource**. |Ja |
| recursive | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. Houd er rekening mee dat wanneer recursieve is ingesteld op true en de sink is een opslagplaats op basis van bestanden, een lege map of submap is niet gekopieerd of gemaakt in de sink.<br/>Toegestane waarden zijn **waar** (standaard) en **false**. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
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
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>BLOB-opslag als een sink-type

- Voor kopiëren naar **Parquet en gescheiden tekstopmaak**, verwijzen naar [Parquet en tekst met scheidingstekens indeling sink](#parquet-and-delimited-text-format-sink) sectie.
- Voor het kopiëren naar andere indelingen, zoals **ORC/Avro/JSON/binaire indeling**, verwijzen naar [andere indeling sink](#other-format-sink) sectie.

#### <a name="parquet-and-delimited-text-format-sink"></a>Parquet en tekst met scheidingstekens indeling sink

Als u wilt kopiëren van gegevens naar Blob-opslag in Parquet of gescheiden tekstopmaak, verwijzen naar [Parquet-indeling](format-parquet.md) en [gescheiden tekstopmaak](format-delimited-text.md) artikel op kopiëren-indeling op basis van activiteit-sink- en ondersteunde instellingen. De volgende eigenschappen worden ondersteund voor Azure-Blob onder `storeSettings` instellingen in op basis van opmaak kopiëren sink:

| Eigenschap                 | Description                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobStorageWriteSetting**. | Ja      |
| copyBehavior             | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de naam van het bestand of de blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee       |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee       |

> [!NOTE]
> Voor Parquet/gescheiden tekstopmaak **BlobSink** type activiteit-sink voor kopiëren die worden vermeld in de volgende sectie wordt nog steeds ondersteund als-is voor voor achterwaartse compatibiliteit. U gebruik van dit nieuwe model voortaan worden voorgesteld, en de gebruikersinterface ontwerpen ADF is overgeschakeld naar deze nieuwe typen genereren.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
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
                    "type": "AzureBlobStorageWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Andere indeling-sink

Om gegevens te kopiëren naar Blob-opslag, stelt u het sink-type in de kopieeractiviteit naar **BlobSink**. De volgende eigenschappen worden ondersteund in de **sink** sectie.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op **BlobSink**. |Ja |
| copyBehavior | Definieert het gedrag kopiëren wanneer de bron bestanden vanuit een bestandsgebaseerde gegevensarchief is.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard)</b>: Hiermee behoudt u de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><b>-FlattenHierarchy</b>: Alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de naam van het bestand of de blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de naam van een automatisch gegenereerde bestand. | Nee |
| maxConcurrentConnections | Het nummer van de verbindingen gelijktijdig verbinding maken met opslag-store. Geef alleen als u wilt beperken, de gelijktijdige verbinding met het gegevensarchief. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Map en bestand filter voorbeelden

Deze sectie beschrijft het resulterende gedrag van de map pad en de naam met jokertekens filters.

| folderPath | fileName | recursive | Bron van de structuur en het filter resultaat map (bestanden in **vet** worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (leeg, gebruikt u standaard) | false | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | (leeg, gebruikt u standaard) | true | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | false | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `container/Folder*` | `*.csv` | true | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Enkele voorbeelden van recursieve en copyBehavior

Deze sectie beschrijft het resulterende gedrag van de kopieerbewerking voor de verschillende combinaties van recursieve en copyBehavior waarden.

| recursive | copyBehavior | Structuur van de gegevensbron | Resulterende doel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| true |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Het doel Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 bestand2 + bestand3 + File4 + File5 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. |
| false |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | de doelmap Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand2<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |
| false |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1 + bestand2 inhoud worden samengevoegd in één bestand met een automatisch gegenereerde naam. automatisch gegenereerde naam voor File1<br/><br/>Subfolder1 bestand3 File4 en File5 is niet opgehaald. |

## <a name="mapping-data-flow-properties"></a>Eigenschappen van fouttoewijzing gegevensstroom

Informatie over de details van [bron transformatie](data-flow-source.md) en [sink-transformatie](data-flow-sink.md) in de gegevensstroom toewijzen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
