---
title: Common Data Model-indeling
description: Gegevens transformeren met behulp van het meta gegevens systeem van common data model
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: daperlov
ms.openlocfilehash: 3c4f2df074bc7feaa42704942a3fd238ab4b333a
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86083777"
---
# <a name="common-data-model-format-in-azure-data-factory"></a>Gemeen schappelijke gegevens model indeling in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Het gegevens systeem van het common data model (CDM) maakt het mogelijk om gegevens en de betekenis ervan eenvoudig te delen tussen toepassingen en bedrijfs processen. Zie het overzicht van [common data model](https://docs.microsoft.com/common-data-model/) voor meer informatie.

In Azure Data Factory kunnen gebruikers transformeren naar en van CDM-entiteiten die zijn opgeslagen in [Azure data Lake Store Gen2](connector-azure-data-lake-storage.md) (ADLS Gen2) met toewijzing van gegevens stromen. U kunt kiezen uit model.js-en manifest stijl CDM-bronnen en schrijven naar CDM-manifest bestanden.

> [!NOTE]
> De standaard-connector voor gegevens modellen (CDM) voor ADF-gegevens stromen is momenteel beschikbaar als open bare preview.

## <a name="mapping-data-flow-properties"></a>Eigenschappen van gegevens stroom toewijzen

Het common data model is beschikbaar als een [inline-gegevensset](data-flow-source.md#inline-datasets) in het toewijzen van gegevens stromen als een bron en een sink.

> [!NOTE]
> Bij het schrijven van CDM-entiteiten moet er al een bestaande CDM-entiteits definitie (meta gegevens schema) zijn gedefinieerd. Met de Sink van de ADF-gegevens stroom wordt dat bestand van de CDM-entiteit gelezen en wordt het schema in uw Sink voor veld toewijzing geïmporteerd.

### <a name="source-properties"></a>Bron eigenschappen

De onderstaande tabel geeft een lijst van de eigenschappen die worden ondersteund door een CDM-bron. U kunt deze eigenschappen bewerken op het tabblad **bron opties** .

| Naam | Beschrijving | Vereist | Toegestane waarden | Eigenschap gegevens stroom script |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Indeling | Indeling moet`cdm` | ja | `cdm` | indeling |
| Meta gegevens indeling | Waar de entiteits verwijzing naar de gegevens zich bevindt. Als u CDM versie 1,0 gebruikt, kiest u Manifest. Als u vóór 1,0 een CDM-versie gebruikt, kiest u model.jsop. | Yes | `'manifest'` of `'model'` | manifestType |
| Hoofd locatie: container | Container naam van de map CDM | ja | Tekenreeks | System |
| Hoofd locatie: mappad | Locatie van de hoofdmap van de map CDM | ja | Tekenreeks | folderPath |
| Manifest bestand: pad naar entiteit | Mappad van de entiteit binnen de hoofdmap | nee | Tekenreeks | entityPath |
| Manifest bestand: manifest naam | De naam van het manifest bestand. Standaard waarde is ' default '  | No | Tekenreeks | manifestnaam |
| Filteren op laatst gewijzigd | Kiezen of bestanden moeten worden gefilterd op basis van het tijdstip waarop deze voor het laatst zijn gewijzigd | nee | Tijdstempel | modifiedAfter <br> modifiedBefore | 
| Gekoppelde schema service | De gekoppelde service waar de verzameling zich bevindt | Ja, als u een manifest gebruikt | `'adlsgen2'` of `'github'` | corpusStore | 
| Container voor entiteits verwijzing | Container verzameling is in | Ja, als u Manifest en verzameling in ADLS Gen2 gebruikt | Tekenreeks | adlsgen2_fileSystem |
| Opslag plaats voor entiteit verwijzing | Naam van de GitHub-opslagplaats | Ja, als u Manifest en verzameling in GitHub gebruikt | Tekenreeks | github_repository |
| Vertakking van entiteits verwijzing | GitHub-opslag plaats vertakking | Ja, als u Manifest en verzameling in GitHub gebruikt | Tekenreeks |  github_branch |
| Map verzameling | de hoofd locatie van de verzameling | Ja, als u een manifest gebruikt | Tekenreeks | corpusPath |
| Verzameling entiteit | Pad naar entiteits verwijzing | ja | Tekenreeks | vennootschap |
| Geen bestanden gevonden | Als deze eigenschap waar is, wordt er geen fout gegenereerd als er geen bestanden worden gevonden | nee | `true` of `false` | ignoreNoFilesFound |

#### <a name="import-schema"></a>Schema importeren

CDM is alleen beschikbaar als een inline-gegevensset en heeft standaard geen bijbehorend schema. Als u kolom meta gegevens wilt ophalen, klikt u op de knop **schema importeren** op het tabblad **projectie** . Hiermee kunt u verwijzen naar de kolom namen en gegevens typen die door de verzameling zijn opgegeven. Als u het schema wilt importeren, moet een foutopsporingssessie voor [gegevens stromen](concepts-data-flow-debug-mode.md) actief zijn en moet u een bestaand CDM-entiteits definitie bestand hebben om naar te verwijzen.

> [!NOTE]
>  Wanneer u model.jsgebruikt voor het bron type dat afkomstig is van Power BI of het Power platform gegevens stromen, kan het zijn dat het fout verzameling pad is null of leeg is van de bron transformatie. Dit komt waarschijnlijk door het format teren van problemen met het pad van de partitie locatie in de model.jsin het bestand. Voer de volgende stappen uit om dit probleem op te lossen: 

1. Het model.jsbestand openen in een tekst editor
2. Zoek de partities. Locatie-eigenschap 
3. Wijzig ' blob.core.windows.net ' in ' dfs.core.windows.net '
4. De code ring '% 2F ' in de URL naar '/' oplossen
 

### <a name="cdm-source-data-flow-script-example"></a>Voor beeld van CDM-bron gegevensstroom script

```
source(output(
        ProductSizeId as integer,
        ProductColor as integer,
        CustomerId as string,
        Note as string,
        LastModifiedDate as timestamp
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    manifestType: 'manifest',
    manifestName: 'ProductManifest',
    entityPath: 'Product',
    corpusPath: 'Products',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    folderPath: 'ProductData',
    fileSystem: 'data') ~> CDMSource
```

### <a name="sink-properties"></a>Eigenschappen van Sink

De onderstaande tabel geeft een lijst van de eigenschappen die worden ondersteund door een CDM-sink. U kunt deze eigenschappen bewerken op het tabblad **instellingen** .

| Naam | Beschrijving | Vereist | Toegestane waarden | Eigenschap gegevens stroom script |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Indeling | Indeling moet`cdm` | ja | `cdm` | indeling |
| Hoofd locatie: container | Container naam van de map CDM | ja | Tekenreeks | System |
| Hoofd locatie: mappad | Locatie van de hoofdmap van de map CDM | ja | Tekenreeks | folderPath |
| Manifest bestand: pad naar entiteit | Mappad van de entiteit binnen de hoofdmap | nee | Tekenreeks | entityPath |
| Manifest bestand: manifest naam | De naam van het manifest bestand. Standaard waarde is ' default ' | No | Tekenreeks | manifestnaam |
| Gekoppelde schema service | De gekoppelde service waar de verzameling zich bevindt | ja | `'adlsgen2'` of `'github'` | corpusStore | 
| Container voor entiteits verwijzing | Container verzameling is in | Ja, als verzameling in ADLS Gen2 | Tekenreeks | adlsgen2_fileSystem |
| Opslag plaats voor entiteit verwijzing | Naam van de GitHub-opslagplaats | Ja, als verzameling in GitHub | Tekenreeks | github_repository |
| Vertakking van entiteits verwijzing | GitHub-opslag plaats vertakking | Ja, als verzameling in GitHub | Tekenreeks |  github_branch |
| Map verzameling | de hoofd locatie van de verzameling | ja | Tekenreeks | corpusPath |
| Verzameling entiteit | Pad naar entiteits verwijzing | ja | Tekenreeks | vennootschap |
| Pad partitioneren | Locatie waar de partitie wordt geschreven | nee | Tekenreeks | partitionPath |
| De map wissen | Als de doelmap vóór het schrijven is gewist | nee | `true` of `false` | afkappen |
| Type indeling | Kiezen om de Parquet-indeling op te geven | nee | `parquet`Indien opgegeven | subformat |
| Kolom scheidings teken | Als u naar DelimitedText schrijft, kolommen beperken | Ja, als u naar DelimitedText schrijft | Tekenreeks | columnDelimiter |
| Eerste rij als koptekst | Als u DelimitedText gebruikt, of de kolom namen worden toegevoegd als koptekst | nee | `true` of `false` | columnNamesAsHeader |

### <a name="cdm-sink-data-flow-script-example"></a>Script voor beeld van CDM Sink-gegevens stroom

Het gekoppelde gegevensstroom script is:

```
CDMSource sink(allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    entityPath: 'ProductSize',
    manifestName: 'ProductSizeManifest',
    corpusPath: 'Products',
    partitionPath: 'adf',
    folderPath: 'ProductSizeData',
    fileSystem: 'cdm',
    subformat: 'parquet',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    truncate: true,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> CDMSink

```

## <a name="next-steps"></a>Volgende stappen

Maak een [bron transformatie](data-flow-source.md) in de toewijzing van gegevens stroom.
