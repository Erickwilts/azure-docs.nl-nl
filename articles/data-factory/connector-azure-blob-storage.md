---
title: Gegevens kopiëren en transformeren in Azure Blob-opslag
description: Meer informatie over het kopiëren van gegevens naar en van Blob Storage en het transformeren van gegevens in Blob Storage met behulp van Data Factory.
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/15/2020
ms.openlocfilehash: ad24f8db2f8f212ebcccf661241016b438ec730c
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655386"
---
# <a name="copy-and-transform-data-in-azure-blob-storage-by-using-azure-data-factory"></a>Gegevens kopiëren en transformeren in Azure Blob-opslag met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory service die u gebruikt:"]
> * [Versie 1](v1/data-factory-azure-blob-connector.md)
> * [Huidige versie](connector-azure-blob-storage.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens te kopiëren van en naar Azure Blob-opslag, en gegevens stroom te gebruiken om gegevens te transformeren in Azure Blob-opslag. Lees het [artikel Inleiding](introduction.md)voor meer informatie over Azure Data Factory.

>[!TIP]
>Voor het migratie scenario van data Lake of Data Warehouse leert u meer over [het gebruik van Azure Data Factory om gegevens van uw data Lake of Data Warehouse te migreren naar Azure](data-migration-guidance-overview.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze Azure Blob-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieer activiteit](copy-activity-overview.md) met een [ondersteunde bron/Sink-matrix](copy-activity-overview.md)
- [Gegevens stroom toewijzen](concepts-data-flow-overview.md)
- [Opzoek activiteit](control-flow-lookup-activity.md)
- [GetMetadata-activiteit](control-flow-get-metadata-activity.md)
- [Activiteit verwijderen](delete-activity.md)

Voor kopieer activiteit ondersteunt deze Blob Storage-connector:

- Blobs kopiëren naar en van Azure Storage-accounts voor algemeen gebruik en warme/cool Blob Storage. 
- Blobs kopiëren met behulp van account sleutel, service-Shared Access-hand tekening, Service-Principal of beheerde identiteiten voor Azure-bronnen verificaties.
- Het kopiëren van blobs uit blok-, toevoeg-of pagina-blobs en het kopiëren van gegevens naar alleen blok-blobs.
- Blobs kopiëren als is of parseren en genereren van blobs met [ondersteunde bestands indelingen en compressie-codecs](supported-file-formats-and-compression-codecs.md).
- [Meta gegevens van bestand behouden tijdens kopiëren](#preserve-metadata-during-copy).

>[!IMPORTANT]
>Als u de optie **vertrouwde micro soft-Services toegang geven tot dit opslag account hebt** ingeschakeld voor Azure Storage Firewall-instellingen en Azure Integration runtime wilt gebruiken om verbinding te maken met uw Blob Storage, moet u [beheerde identiteits verificatie](#managed-identity)gebruiken.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory entiteiten die specifiek zijn voor Blob Storage.

## <a name="linked-service-properties"></a>Eigenschappen van gekoppelde service

Azure Blob-connector biedt ondersteuning voor de volgende verificatie typen: Raadpleeg de bijbehorende sectie over de details.

- [Verificatie van account sleutel](#account-key-authentication)
- [Verificatie van de Shared Access-hand tekening](#shared-access-signature-authentication)
- [Verificatie van service-principal](#service-principal-authentication)
- [Beheerde identiteiten voor Azure-bronnen verificatie](#managed-identity)

>[!NOTE]
>Wanneer u poly base gebruikt voor het laden van gegevens in SQL Data Warehouse, moet u, als uw bron of faserings-Blob-opslag is geconfigureerd met Virtual Network-eind punt, beheerde identiteits verificatie gebruiken zoals vereist door poly base en zelf-hostende Integration Runtime gebruiken met versie 3,18 of hoger. Zie de sectie [beheerde identiteits verificatie](#managed-identity) met meer configuratie vereisten.

>[!NOTE]
>HDInsights en Azure Machine Learning-activiteiten bieden alleen ondersteuning voor de sleutel verificatie van Azure Blob-opslag accounts.

### <a name="account-key-authentication"></a>Verificatie van account sleutel

De volgende eigenschappen worden ondersteund voor het gebruik van verificatie van de opslag account sleutel:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobStorage** (aanbevolen) of **opslag** (Zie de opmerkingen hieronder). |Ja |
| Verbindings | Geef de gegevens op die nodig zijn om verbinding te maken met opslag voor de Connections Tring-eigenschap. <br/> U kunt ook account sleutel in Azure Key Vault plaatsen en de `accountKey` configuratie uit de Connection String halen. Raadpleeg de volgende voor beelden en [Sla referenties op in azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt Azure Integration Runtime of zelf-hostende Integration Runtime gebruiken (als uw gegevens archief zich in een particulier netwerk bevindt). Als u niets opgeeft, wordt de standaard Azure Integration Runtime gebruikt. |Nee |

>[!NOTE]
>Secundair eind punt van BLOB service wordt niet ondersteund bij het gebruik van account sleutel verificatie. U kunt andere verificatie typen gebruiken.

>[!NOTE]
>Als u de gekoppelde service van het type ' opslag ' gebruikt, wordt deze nog steeds ondersteund als-is. u wordt aangeraden dit nieuwe ' AzureBlobStorage ' gekoppelde Service Type vooruit te gebruiken.

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld: account sleutel opslaan in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;",
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

### <a name="shared-access-signature-authentication"></a>Verificatie van de Shared Access-hand tekening

Een Shared Access Signature biedt gedelegeerde toegang tot resources in uw opslag account. U kunt een gedeelde hand tekening voor toegang gebruiken om een opgegeven periode een client met beperkte machtigingen te verlenen aan objecten in uw opslag account. U hoeft de toegangs sleutels van uw account niet te delen. De Shared Access-hand tekening is een URI die in de query parameters omvat alle informatie die nodig is voor geverifieerde toegang tot een opslag resource. Om toegang te krijgen tot opslag resources met de hand tekening voor gedeelde toegang, hoeft de client alleen de gedeelde toegangs handtekening door te geven aan de juiste constructor of methode. Zie voor meer informatie over hand tekeningen voor gedeelde toegang, [Shared Access Signatures: inzicht in het model voor de Shared Access-hand tekening](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
>- Data Factory ondersteunt nu zowel **gedeelde toegangs handtekeningen van services** als **hand tekeningen voor gedeelde toegang van accounts**. Zie voor meer informatie over gedeelde toegangs handtekeningen [beperkte toegang verlenen tot Azure storage-resources met behulp van Shared Access signatures (SAS)](../storage/common/storage-sas-overview.md).
>- In latere configuratie van de gegevensset is het mappad het absolute pad vanaf container niveau. U moet een uitlijning configureren met het pad in de SAS-URI.

De volgende eigenschappen worden ondersteund om verificatie van de Shared Access-hand tekening te gebruiken:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobStorage** (aanbevolen) of **opslag** (Zie de opmerkingen hieronder). |Ja |
| sasUri | Geef de URI van de Shared Access-hand tekening op voor de opslag resources, zoals BLOB/container. <br/>Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory. U kunt ook SAS-token in Azure Key Vault plaatsen om automatisch te draaien en het token gedeelte te verwijderen. Raadpleeg de volgende voor beelden en [Sla referenties op in azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. |Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt de Azure Integration Runtime of de zelf-hostende Integration Runtime gebruiken (als uw gegevens archief zich in een particulier netwerk bevindt). Als u niets opgeeft, wordt de standaard Azure Integration Runtime gebruikt. |Nee |

>[!NOTE]
>Als u de gekoppelde service van het type ' opslag ' gebruikt, wordt deze nog steeds ondersteund als-is. u wordt aangeraden dit nieuwe ' AzureBlobStorage ' gekoppelde Service Type vooruit te gebruiken.

**Voorbeeld:**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource e.g. https://<accountname>.blob.core.windows.net/?sv=<storage version>&amp;st=<start time>&amp;se=<expire time>&amp;sr=<resource>&amp;sp=<permissions>&amp;sip=<ip range>&amp;spr=<protocol>&amp;sig=<signature>>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld: account sleutel opslaan in Azure Key Vault**

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource without token e.g. https://<accountname>.blob.core.windows.net/>"
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

Houd rekening met de volgende punten wanneer u een URI voor een Shared Access-hand tekening maakt:

- Stel de juiste lees-en schrijf machtigingen voor objecten in op basis van de manier waarop de gekoppelde service (lezen, schrijven, lezen/schrijven) wordt gebruikt in uw data factory.
- Stel de **verloop tijd** op de juiste wijze in. Zorg ervoor dat de toegang tot opslag objecten niet verloopt binnen de actieve periode van de pijp lijn.
- De URI moet worden gemaakt in de juiste container/Blob op basis van de behoefte. Met een URI van een Shared Access Signature naar een BLOB kan Data Factory toegang krijgen tot die specifieke blob. Met een URI voor Shared Access Signature naar een BLOB storage-container kan Data Factory de blobs in die container door lopen. Als u later toegang wilt bieden tot meer of minder objecten of als u de URI van de Shared Access-hand tekening wilt bijwerken, moet u de gekoppelde service bijwerken met de nieuwe URI.

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Raadpleeg voor Azure Storage Service-Principal-verificatie in het algemeen het [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory](../storage/common/storage-auth-aad.md).

Voer de volgende stappen uit om Service-Principal-verificatie te gebruiken:

1. Registreer een toepassings entiteit in Azure Active Directory (Azure AD) door [uw toepassing te registreren bij een Azure AD-Tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt om de gekoppelde service te definiëren:

    - Toepassings-id
    - Toepassings sleutel
    - Tenant-id

2. Verleen de service-principal de juiste machtiging in Azure Blob-opslag. Raadpleeg de [toegangs rechten voor het Azure Storage van gegevens beheren met RBAC](../storage/common/storage-auth-aad-rbac.md) met meer informatie over de rollen.

    - Ken **als bron**, in toegangs beheer (IAM), ten minste de rol van **BLOB-gegevens lezer voor opslag** toe.
    - Ken **als Sink**, in toegangs beheer (IAM), ten minste de rol van **BLOB data contributor voor opslag** .

Deze eigenschappen worden ondersteund voor een gekoppelde Azure Blob Storage-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobStorage**. |Ja |
| Serviceendpoint toegevoegd | Geef het Azure Blob Storage-service-eind punt met het patroon van op `https://<accountName>.blob.core.windows.net/` . |Ja |
| servicePrincipalId | Geef de client-ID van de toepassing op. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing op. Markeer dit veld als **SecureString** om het veilig op te slaan in Data Factory, of om te [verwijzen naar een geheim dat is opgeslagen in azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | Geef de Tenant gegevens op (domein naam of Tenant-ID) waaronder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechter bovenhoek van de Azure Portal aan te wijzen. | Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt Azure Integration Runtime of zelf-hostende Integration Runtime gebruiken (als uw gegevens archief zich in een particulier netwerk bevindt). Als u niets opgeeft, wordt de standaard Azure Integration Runtime gebruikt. |Nee |

>[!NOTE]
>Service-Principal-verificatie wordt alleen ondersteund door de gekoppelde service van het type ' AzureBlobStorage ', maar niet naar de gekoppelde service van het type ' opslag '.

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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Beheerde identiteiten voor Azure-bronnen verificatie

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor Azure-resources](data-factory-service-identity.md)die deze specifieke Data Factory vertegenwoordigt. U kunt deze beheerde identiteit rechtstreeks gebruiken voor de verificatie van Blob-opslag, vergelijkbaar met het gebruik van uw eigen service-principal. Hiermee kan deze toegewezen Factory toegang krijgen tot gegevens en deze kopiëren van/naar uw Blob-opslag.

Raadpleeg [toegang tot Azure Storage verifiëren met behulp van Azure Active Directory](../storage/common/storage-auth-aad.md) voor Azure Storage verificatie in het algemeen. Voer de volgende stappen uit om beheerde identiteiten te gebruiken voor Azure-bronnen verificatie:

1. [Haal Data Factory beheerde identiteits gegevens](data-factory-service-identity.md#retrieve-managed-identity) op door de waarde van de **beheerde ID-object-id** die samen met uw fabriek is gegenereerd, te kopiëren.

2. Verleen de beheerde identiteit de juiste machtiging in Azure Blob-opslag. Raadpleeg de [toegangs rechten voor het Azure Storage van gegevens beheren met RBAC](../storage/common/storage-auth-aad-rbac.md) met meer informatie over de rollen.

    - Ken **als bron**, in toegangs beheer (IAM), ten minste de rol van **BLOB-gegevens lezer voor opslag** toe.
    - Ken **als Sink**, in toegangs beheer (IAM), ten minste de rol van **BLOB data contributor voor opslag** .

>[!IMPORTANT]
>Als u poly base gebruikt voor SQL Database het laden van gegevens uit een BLOB (als bron of als fase ring) in SQL Data Warehouse, moet u, wanneer u beheerde identiteits verificatie voor BLOB gebruikt, ervoor zorgen dat u de stappen 1 en 2 in [deze richt lijnen](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) aan de hand van de gegevens van de SQL database server registreert bij Azure Active Directory (Azure AD) en 2). de rest wordt afgehandeld door Data Factory. Als uw Blobopslag is geconfigureerd met een Azure Virtual Network-eind punt, moet u de beheerde identiteits verificatie gebruiken zoals vereist is door poly Base om poly Base te gebruiken voor het laden van gegevens uit de opslag.

Deze eigenschappen worden ondersteund voor een gekoppelde Azure Blob Storage-service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobStorage**. |Ja |
| Serviceendpoint toegevoegd | Geef het Azure Blob Storage-service-eind punt met het patroon van op `https://<accountName>.blob.core.windows.net/` . |Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt Azure Integration Runtime of zelf-hostende Integration Runtime gebruiken (als uw gegevens archief zich in een particulier netwerk bevindt). Als u niets opgeeft, wordt de standaard Azure Integration Runtime gebruikt. |Nee |

> [!NOTE]
> Beheerde identiteiten voor Azure-bronnen authenticatie wordt alleen ondersteund door de gekoppelde service van het type ' AzureBlobStorage ', maar niet naar de vorige gekoppelde service van het type ' opslag '. 

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

Zie het artikel [gegevens sets](concepts-datasets-linked-services.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevens sets. 

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

De volgende eigenschappen worden ondersteund voor Azure Blob onder `location` instellingen in gegevensset op basis van indeling:

| Eigenschap   | Beschrijving                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type van de locatie in DataSet moet worden ingesteld op **AzureBlobStorageLocation**. | Ja      |
| container  | De BLOB-container.                                          | Ja      |
| folderPath | Het pad naar de map onder de opgegeven container. Als u een Joker teken wilt gebruiken om de map te filteren, slaat u deze instelling over en geeft u de instellingen voor de activiteit bron op. | Nee       |
| fileName   | De bestands naam onder de opgegeven container + folderPath. Als u Joker tekens wilt gebruiken om bestanden te filteren, slaat u deze instelling over en geeft u de instellingen van de activiteit bron op. | Nee       |

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

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie het artikel [pijp lijnen](concepts-pipelines-activities.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Blob-opslag bron en Sink.

### <a name="blob-storage-as-a-source-type"></a>Blob-opslag als bron type

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

De volgende eigenschappen worden ondersteund voor Azure Blob onder `storeSettings` instellingen in op indeling gebaseerde Kopieer Bron:

| Eigenschap                 | Beschrijving                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobStorageReadSettings**. | Ja                                           |
| ***Zoek de bestanden die moeten worden gekopieerd:*** |  |  |
| OPTIE 1: statisch pad<br> | Kopiëren uit de opgegeven container of map of het pad naar het bestand dat is opgegeven in de gegevensset. Als u alle blobs uit een container/map wilt kopiëren, moet u ook opgeven `wildcardFileName` als `*` . |  |
| OPTIE 2: BLOB-voor voegsel<br>-voor voegsel | Voor voegsel voor de blobnaam onder de opgegeven container die in de gegevensset is geconfigureerd voor het filteren van bron-blobs. Blobs waarvan de naam begint met `container_in_dataset/this_prefix` zijn geselecteerd. Er wordt gebruikgemaakt van het service-side filter van de blob, dat betere prestaties biedt dan het filter voor joker tekens. | Nee                                                          |
| OPTIE 3: Joker teken<br>- wildcardFolderPath | Het mappad met Joker tekens onder de opgegeven container die in de gegevensset is geconfigureerd om bron mappen te filteren. <br>Toegestane joker tekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); gebruik `^` om te escapen als uw werkelijke mapnaam Joker teken of escape-teken bevat. <br>Bekijk meer voor beelden in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| OPTIE 3: Joker teken<br>- wildcardFileName | De naam van het bestand met Joker tekens onder de opgegeven container + folderPath/wildcardFolderPath voor het filteren van bron bestanden. <br>Toegestane joker tekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); gebruik `^` om te escapen als uw werkelijke mapnaam Joker teken of escape-teken bevat.  Bekijk meer voor beelden in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). | Ja |
| OPTIE 4: een lijst met bestanden<br>- fileListPath | Hiermee wordt aangegeven dat een opgegeven bestandenset moet worden gekopieerd. Wijs naar een tekst bestand met een lijst met bestanden die u wilt kopiëren, één bestand per regel dat het relatieve pad is naar het pad dat is geconfigureerd in de gegevensset.<br/>Wanneer u deze optie gebruikt, geeft u geen bestands naam op in DataSet. Meer voor beelden vindt u in [voor beelden van bestands lijsten](#file-list-examples). |Nee |
| ***Aanvullende instellingen:*** |  | |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen vanuit de opgegeven map. Houd er rekening mee dat wanneer recursief is ingesteld op True en de Sink een archief op basis van bestanden is, een lege map of submap niet wordt gekopieerd of gemaakt bij de sink. <br>Toegestane waarden zijn **True** (standaard) en **Onwaar**.<br>Deze eigenschap is niet van toepassing wanneer u configureert `fileListPath` . |Nee |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: laatst gewijzigd. <br>De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br> De eigenschappen kunnen NULL zijn, wat betekent dat er geen filter voor bestands kenmerken wordt toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` heeft datetime-waarde `modifiedDatetimeEnd` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter dan of gelijk is aan de datum/tijd-waarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` heeft datetime-waarde `modifiedDatetimeStart` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is kleiner is dan de datum/tijd-waarde wordt geselecteerd.<br/>Deze eigenschap is niet van toepassing wanneer u configureert `fileListPath` . | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee                                            |

> [!NOTE]
> Voor een Parquet/gescheiden tekst indeling wordt de bron van de Kopieer activiteit van het type **BlobSource** die in de volgende sectie wordt vermeld, nog steeds ondersteund als-is voor achterwaartse compatibiliteit. U wordt aangeraden dit nieuwe model verder te gebruiken en de gebruikers interface van de ADF-ontwerp functie is overgeschakeld op het genereren van deze nieuwe typen.

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
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
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

### <a name="blob-storage-as-a-sink-type"></a>Blob-opslag als Sink-type

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

De volgende eigenschappen worden ondersteund voor Azure Blob onder `storeSettings` instellingen in op indeling gebaseerde kopie-Sink:

| Eigenschap                 | Beschrijving                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobStorageWriteSettings**. | Ja      |
| copyBehavior             | Hiermee wordt het Kopieer gedrag gedefinieerd wanneer de bron bestanden van een gegevens archief op basis van een bestand zijn.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard instelling)</b>: behoudt de bestands hiërarchie in de doelmap. Het relatieve pad van het bron bestand naar de bronmap is identiek aan het relatieve pad van het doel bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden in de bronmap bevinden zich in het eerste niveau van de doelmap. De doel bestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: alle bestanden van de bronmap worden samengevoegd met één bestand. Als de naam van het bestand of de blob is opgegeven, is de naam van het samengevoegde bestand de opgegeven naam. Anders is het een automatisch gegenereerde bestands naam. | Nee       |
| blockSizeInMB | Geef de blok grootte in MB op die wordt gebruikt voor het schrijven van gegevens voor blok-blobs. Meer informatie [over blok-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs). <br/>De toegestane waarde ligt **tussen 4 en 100 MB**. <br/>Standaard bepaalt ADF automatisch de blok grootte op basis van het type en de gegevens van het bron archief. Voor een niet-binaire kopie naar een blob is de standaard blok grootte 100 MB, zodat deze in Maxi maal 4,95 TB gegevens past. Het is mogelijk niet optimaal wanneer uw gegevens niet groot zijn, met name wanneer u een zelf-hostend Integration Runtime gebruikt met een slecht netwerk als gevolg van een time-out voor de bewerking of het prestatie probleem. U kunt expliciet een blok grootte opgeven, terwijl u er zeker van wilt zijn dat blockSizeInMB * 50.000 groot genoeg is om de gegevens op te slaan, anders mislukt het uitvoeren van de Kopieer activiteit. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee       |

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
                    "type": "AzureBlobStorageWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Voor beelden van map-en bestands filter

In deze sectie wordt het resulterende gedrag van het mappad en de bestands naam met Joker teken filters beschreven.

| folderPath | fileName | recursieve | De structuur van de bronmap en het filter resultaat ( **vetgedrukte** bestanden worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `container/Folder*` | (leeg, standaard instelling gebruiken) | onjuist | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand2. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `container/Folder*` | (leeg, standaard instelling gebruiken) | waar | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand2. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `container/Folder*` | `*.csv` | onjuist | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `container/Folder*` | `*.csv` | waar | container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |

### <a name="file-list-examples"></a>Voor beelden van bestands lijst

In deze sectie wordt het resulterende gedrag van het gebruik van het pad naar een bestands lijst in de bron van de Kopieer activiteit beschreven.

Ervan uitgaande dat u de volgende structuur van de bronmap hebt en de bestanden vet wilt kopiëren:

| Voorbeeld bron structuur                                      | Inhoud in FileListToCopy. txt                             | ADF-configuratie                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| container<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Metagegevensarchiefmethode<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy. txt | Bestand1. CSV<br>Subfolder1/File3. CSV<br>Subfolder1/File5. CSV | **In gegevensset:**<br>Verpakking`container`<br>-Mappad:`FolderA`<br><br>**Bron van Kopieer activiteit:**<br>-Pad naar bestands lijst:`container/Metadata/FileListToCopy.txt` <br><br>Het pad naar de bestands lijst verwijst naar een tekst bestand in hetzelfde gegevens archief dat een lijst bevat met bestanden die u wilt kopiëren, één bestand per regel met het relatieve pad naar het pad dat is geconfigureerd in de gegevensset. |

### <a name="some-recursive-and-copybehavior-examples"></a>Enkele recursieve en copyBehavior-voor beelden

In deze sectie wordt het resulterende gedrag van de Kopieer bewerking voor verschillende combi Naties van recursieve en copyBehavior waarden beschreven.

| recursieve | copyBehavior | Structuur van bronmap | Resulterende doel |
|:--- |:--- |:--- |:--- |
| waar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| waar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;De inhoud van bestand1 + Bestand2 + File3 + File4 + File5 worden samengevoegd met een bestand met een automatisch gegenereerde bestands naam. |
| onjuist |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/><br/>Subfolder1 met File3, File4 en File5 is niet opgehaald. |
| onjuist |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 wordt gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor Bestand2<br/><br/>Subfolder1 met File3, File4 en File5 is niet opgehaald. |
| onjuist |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doelmap Map1 is gemaakt met de volgende structuur<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;De inhoud van bestand1 + Bestand2 wordt samengevoegd met een bestand met een automatisch gegenereerde bestands naam. automatisch gegenereerde naam voor bestand1<br/><br/>Subfolder1 met File3, File4 en File5 is niet opgehaald. |

## <a name="preserve-metadata-during-copy"></a>Meta gegevens bewaren tijdens kopiëren

Wanneer u bestanden kopieert van Amazon S3/Azure Blob/Azure Data Lake Storage Gen2 naar Azure Data Lake Storage Gen2/Azure Blob, kunt u ervoor kiezen om de meta gegevens van het bestand te behouden. Meer informatie over het [bewaren van meta gegevens](copy-activity-preserve-metadata.md#preserve-metadata).

## <a name="mapping-data-flow-properties"></a>Eigenschappen van gegevens stroom toewijzen

Bij het transformeren van gegevens in de toewijzing van gegevens stroom kunt u bestanden lezen en schrijven van Azure Blob Storage in JSON, AVRO, tekst met scheidings tekens of Parquet-indeling. Zie [bron transformatie](data-flow-source.md) en [sink-trans formatie](data-flow-sink.md) in de functie gegevens stroom toewijzen voor meer informatie.

### <a name="source-transformation"></a>Bron transformatie

In de bron transformatie kunt u lezen uit een container, map of afzonderlijk bestand in Azure Blob Storage. Op het tabblad **bron opties** kunt u beheren hoe de bestanden worden gelezen. 

![Bron opties](media/data-flow/sourceOptions1.png "Bron opties")

**Pad met Joker tekens:** Als u een Joker teken gebruikt, wordt ADF geadviseerd om elke overeenkomende map en elk bestand in één bron transformatie te verwerken. Dit is een efficiënte manier om meerdere bestanden binnen één stroom te verwerken. Voeg meerdere Joker teken patronen toe met het plus teken dat wordt weer gegeven bij het aanwijzen van het bestaande Joker teken patroon.

Kies in uw bron container een reeks bestanden die overeenkomen met een patroon. Alleen container kan worden opgegeven in de gegevensset. Het pad naar uw Joker teken moet daarom ook uw mappad van de hoofdmap bevatten.

Voor beelden van joker tekens:

* ```*```Vertegenwoordigt een wille keurige set tekens
* ```**```Recursief nesten van mappen
* ```?```Vervangt één teken
* ```[]```Komt overeen met een van de tekens in de vier Kante haken

* ```/data/sales/**/*.csv```Hiermee worden alle CSV-bestanden opgehaald onder/Data/Sales
* ```/data/sales/20??/**/```Hiermee worden alle bestanden in de twintigste eeuw opgehaald
* ```/data/sales/*/*/*.csv```Haalt CSV-bestanden op twee niveaus onder/Data/Sales
* ```/data/sales/2004/*/12/[XY]1?.csv```Haalt alle CSV-bestanden in 2004 december op, beginnend met X of Y, voorafgegaan door een getal van twee cijfers

Basispad **:** Als u mappen in de bestands bron hebt gepartitioneerd met een ```key=value``` indeling (bijvoorbeeld Year = 2019), kunt u het hoogste niveau van die partitie mappen structuur toewijzen aan een kolom naam in de gegevensstroom gegevens stroom.

Stel eerst een Joker teken in om alle paden op te nemen van de gepartitioneerde mappen plus de blad bestanden die u wilt lezen.

![Bron Bestands instellingen voor partitioneren](media/data-flow/partfile2.png "Instelling voor partitie bestand")

Gebruik de instelling basis pad partitie om te definiëren wat het hoogste niveau van de mappen structuur is. Wanneer u de inhoud van uw gegevens bekijkt via een voor beeld van de gegevens, ziet u dat de opgeloste partities in elk van uw mapniveau worden toegevoegd.

![Basispad voor partitie](media/data-flow/partfile1.png "Voor beeld van basispad")

**Lijst met bestanden:** Dit is een bestandenset. Maak een tekst bestand met een lijst met relatieve padgegevens die moeten worden verwerkt. Wijs dit tekst bestand aan.

**Kolom voor het opslaan van de bestands naam:** Sla de naam van het bron bestand op in een kolom in uw gegevens. Voer hier een nieuwe kolom naam in om de teken reeks voor de bestands naam op te slaan.

**Na voltooiing:** U kunt niets doen met het bron bestand nadat de gegevens stroom is uitgevoerd, het bron bestand te verwijderen of het bron bestand te verplaatsen. De paden voor de verplaatsing zijn relatief.

Als u bron bestanden naar een andere locatie wilt verplaatsen, selecteert u eerst verplaatsen voor bestands bewerking. Stel vervolgens de map uit. Als u geen joker tekens gebruikt voor uw pad, is de instelling van ' van ' dezelfde map als de bronmap.

Als u een bronpad met Joker teken hebt, ziet uw syntaxis er als volgt uit:

```/data/sales/20??/**/*.csv```

U kunt "van" opgeven als

```/data/sales```

En "aan" als

```/backup/priorSales```

In dit geval worden alle bestanden die zijn gebrond onder/Data/Sales verplaatst naar/backup/priorSales.

> [!NOTE]
> Bestands bewerkingen worden alleen uitgevoerd wanneer u de gegevens stroom start vanuit een pijplijn uitvoering (een uitvoering van een pijp lijn of uitvoering) die gebruikmaakt van de activiteit gegevens stroom uitvoeren in een pijp lijn. Bestands bewerkingen *worden niet* uitgevoerd in de modus voor fout opsporing van gegevens stromen.

**Filteren op laatst gewijzigd:** U kunt filteren op de bestanden die u verwerkt door een datum bereik op te geven wanneer ze voor het laatst zijn gewijzigd. Alle datum-tijden zijn in UTC. 

### <a name="sink-properties"></a>Eigenschappen van Sink

In de Sink-trans formatie kunt u schrijven naar een container of map in Azure Blob Storage. op het tabblad **instellingen** kunt u beheren hoe de bestanden worden geschreven.

![Sink-opties](media/data-flow/file-sink-settings.png "Sink-opties")

**De map wissen:** Hiermee wordt bepaald of de doelmap wordt gewist voordat de gegevens worden geschreven.

**Optie Bestands naam:** Hiermee wordt bepaald hoe de doel bestanden in de doelmap worden genoemd. De opties voor de bestands naam zijn:
   * **Standaard**: Spark toestaan om bestanden een naam te geven op basis van de standaard waarden van een deel.
   * **Patroon**: Voer een patroon in voor het inventariseren van de uitvoer bestanden per partitie. Bijvoorbeeld: **leningen [n]. CSV** maken loans1. CSV, loans2. CSV, enzovoort.
   * **Per partitie**: Voer één bestands naam per partitie in.
   * **Als gegevens in kolom**: Stel het uitvoer bestand in op de waarde van een kolom. Het pad is relatief ten opzichte van de container gegevensset, niet de doelmap. Als u een pad naar een map in uw gegevensset hebt, wordt deze overschreven.
   * **Uitvoer naar één bestand**: de gepartitioneerde uitvoer bestanden combi neren in één bestand met een naam. Het pad is relatief ten opzichte van de map DataSet. Houd er rekening mee dat de samenvoegings bewerking mogelijk kan mislukken op basis van de knooppunt grootte. Deze optie wordt niet aanbevolen voor grote gegevens sets.

**Alle aanhalings tekens:** Hiermee wordt bepaald of alle waarden tussen aanhalings tekens moeten worden inge sloten

## <a name="lookup-activity-properties"></a>Eigenschappen van opzoek activiteit

Controleer de [opzoek activiteit](control-flow-lookup-activity.md)voor meer informatie over de eigenschappen.

## <a name="getmetadata-activity-properties"></a>Eigenschappen van GetMetadata-activiteit

Als u meer wilt weten over de eigenschappen, controleert u de [GetMetadata-activiteit](control-flow-get-metadata-activity.md) 

## <a name="delete-activity-properties"></a>Eigenschappen van activiteit verwijderen

Als u meer wilt weten over de eigenschappen, controleert u de [activiteit verwijderen](delete-activity.md)

## <a name="legacy-models"></a>Verouderde modellen

>[!NOTE]
>De volgende modellen worden nog steeds ondersteund voor compatibiliteit met eerdere versies. U wordt aangeraden het nieuwe model te gebruiken dat hierboven wordt beschreven en de gebruikers interface van de ADF-ontwerp functie is overgeschakeld op het genereren van het nieuwe model.

### <a name="legacy-dataset-model"></a>Verouderd gegevensset-model

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de DataSet moet worden ingesteld op **AzureBlob**. |Ja |
| folderPath | Pad naar de container en de map in de Blob-opslag. <br/><br/>Het Joker teken filter wordt ondersteund voor het pad zonder container naam. Toegestane joker tekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); gebruik `^` om te escapen als uw werkelijke mapnaam Joker teken of escape-teken bevat. <br/><br/>Voor beelden: myblobcontainer/myblobfolder/, meer voor beelden weer geven in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). |Ja voor de activiteit kopiëren/opzoeken, geen voor GetMetadata-activiteit |
| fileName | De **naam of het Joker teken filter** voor de BLOB (s) onder de opgegeven folderPath. Als u geen waarde opgeeft voor deze eigenschap, wijst de gegevensset naar alle blobs in de map. <br/><br/>Voor het filter zijn toegestane joker tekens: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voor beeld 1:`"fileName": "*.csv"`<br/>-Voor beeld 2:`"fileName": "???20180427.txt"`<br/>Gebruik `^` om te escapen als uw werkelijke bestands naam Joker teken of escape-teken bevat.<br/><br/>Als er geen bestands naam is opgegeven voor een uitvoer-gegevensset en **preserveHierarchy** niet is opgegeven in de Sink van de activiteit, genereert de Kopieer activiteit automatisch de naam van de blob met het volgende patroon: "*gegevens. [ Run ID-GUID van activiteit]. [GUID if FlattenHierarchy]. [indeling indien geconfigureerd]. [compressie indien geconfigureerd]*', bijvoorbeeld ' data. 0a405f8a-93ff-4c6f-b3be-f69616f1df7a. txt. gz '; Als u vanuit tabellaire bron kopieert met behulp van de tabel naam in plaats van een query, is het naam patroon '*[tabel naam]. [ indeling]. [compressie indien geconfigureerd]*", bijvoorbeeld" mytable. csv ". |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk: laatst gewijzigd. De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br/><br/> Houd er rekening mee dat de prestaties van het verplaatsen van gegevens worden beïnvloed door deze instelling in te scha kelen wanneer u bestands filter van enorme hoeveel heden bestanden wilt uitvoeren. <br/><br/> De eigenschappen kunnen NULL zijn, wat betekent dat er geen filter voor bestands kenmerken wordt toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` heeft datetime-waarde `modifiedDatetimeEnd` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter dan of gelijk is aan de datum/tijd-waarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` heeft datetime-waarde `modifiedDatetimeStart` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is kleiner is dan de datum/tijd-waarde wordt geselecteerd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk: laatst gewijzigd. De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br/><br/> Houd er rekening mee dat de prestaties van het verplaatsen van gegevens worden beïnvloed door deze instelling in te scha kelen wanneer u bestands filter van enorme hoeveel heden bestanden wilt uitvoeren. <br/><br/> De eigenschappen kunnen NULL zijn, wat betekent dat er geen filter voor bestands kenmerken wordt toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` heeft datetime-waarde `modifiedDatetimeEnd` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter dan of gelijk is aan de datum/tijd-waarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` heeft datetime-waarde `modifiedDatetimeStart` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is kleiner is dan de datum/tijd-waarde wordt geselecteerd.| Nee |
| formaat | Als u bestanden wilt kopiëren als zich bevindt tussen archieven op basis van bestanden (binaire kopie), slaat u de sectie opmaak in zowel de definitie van de invoer-als uitvoer gegevensset over.<br/><br/>Als u bestanden wilt parseren of genereren met een specifieke indeling, worden de volgende typen bestands indelingen ondersteund: **TextFormat**, **JsonFormat**, **Avro Format**, **OrcFormat**en **ParquetFormat**. Stel de eigenschap **type** onder **indeling** in op een van deze waarden. Zie de secties [tekst indeling](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro](supported-file-formats-and-compression-codecs-legacy.md#avro-format)-indeling, [Orc-indeling](supported-file-formats-and-compression-codecs-legacy.md#orc-format)en [Parquet-indeling](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) voor meer informatie. |Nee (alleen voor het scenario binair kopiëren) |
| compressie | Geef het type en compressie niveau voor de gegevens op. Zie [ondersteunde bestands indelingen en compressie-codecs](supported-file-formats-and-compression-codecs-legacy.md#compression-support)voor meer informatie.<br/>Ondersteunde typen zijn **gzip**, **Deflate**, **bzip2**en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimaal** en **snelst**. |Nee |

>[!TIP]
>Als u alle blobs in een map wilt kopiëren, geeft u alleen **FolderPath** op.<br>Als u één blob met een opgegeven naam wilt kopiëren, geeft u **FolderPath** op met een mappictogram en een **Bestands** naam met de bestands namen.<br>Als u een subset van blobs in een map wilt kopiëren, geeft u **FolderPath** op met een mappictogram en een **Bestands naam** met een Joker teken filter. 

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

### <a name="legacy-copy-activity-source-model"></a>Bron model van verouderde Kopieer activiteit

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **BlobSource**. |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen vanuit de opgegeven map. Houd er rekening mee dat wanneer recursief is ingesteld op True en de Sink een archief op basis van bestanden is, een lege map of submap niet wordt gekopieerd of gemaakt bij de sink.<br/>Toegestane waarden zijn **True** (standaard) en **Onwaar**. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee |

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

### <a name="legacy-copy-activity-sink-model"></a>Sink model voor verouderde Kopieer activiteit

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de Sink voor kopieer activiteiten moet worden ingesteld op **BlobSink**. |Ja |
| copyBehavior | Hiermee wordt het Kopieer gedrag gedefinieerd wanneer de bron bestanden van een gegevens archief op basis van een bestand zijn.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard instelling)</b>: behoudt de bestands hiërarchie in de doelmap. Het relatieve pad van het bron bestand naar de bronmap is identiek aan het relatieve pad van het doel bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden in de bronmap bevinden zich in het eerste niveau van de doelmap. De doel bestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: alle bestanden van de bronmap worden samengevoegd met één bestand. Als de naam van het bestand of de blob is opgegeven, is de naam van het samengevoegde bestand de opgegeven naam. Anders is het een automatisch gegenereerde bestands naam. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met opslag archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee |

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

## <a name="next-steps"></a>Volgende stappen

Zie [ondersteunde gegevens archieven](copy-activity-overview.md#supported-data-stores-and-formats)voor een lijst met gegevens archieven die worden ondersteund als bronnen en sinks op basis van de Kopieer activiteit in Data Factory.
