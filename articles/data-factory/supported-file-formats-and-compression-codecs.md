---
title: Ondersteunde bestandsindelingen in Azure Data Factory | Microsoft Docs
description: Dit onderwerp beschrijft de bestandsindelingen en compressie-codes die worden ondersteund door de connectors op basis van bestanden in Azure Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: jingwang
ms.openlocfilehash: 9f6edc45316eaeceb75da643ed64b39382712852
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165946"
---
# <a name="supported-file-formats-and-compression-codecs-in-azure-data-factory"></a>Ondersteunde indelingen en codecs voor de compressie in Azure Data Factory

*In dit artikel is van toepassing op de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestandssysteem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), en [ SFTP](connector-sftp.md).*

Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. Als u wilt **parseren of bestanden met een specifieke indeling genereren**, Azure Data Factory ondersteunt de volgende indeling bestandstypen:

* [Tekstindeling](#text-format)
* [JSON-indeling](#json-format)
* [Parquet-indeling](#parquet-format)
* [ORC-indeling](#orc-format)
* [Avro-indeling](#avro-format)

> [!TIP]
> Informatie over hoe copy activity in uw gegevens om op te vangen van kaarten [schematoewijzing in de kopieeractiviteit](copy-activity-schema-and-type-mapping.md).

## <a name="text-format"></a>Tekstindeling

>[!NOTE]
>Data Factory nieuwe geïntroduceerd gescheiden tekst opmaken DataSet, Zie [gescheiden tekstopmaak](format-delimited-text.md) artikel met informatie. De volgende configuraties op bestanden gebaseerde gegevens store gegevensset wordt nog steeds ondersteund als-is voor achterwaartse compabitility. U worden gebruik van het nieuwe model voortaan voorgesteld.

Als u wilt lezen uit een tekstbestand of schrijven naar een tekstbestand, stelt u de `type` eigenschap in de `format` sectie van de gegevensset in **TextFormat**. U kunt ook de volgende **optionele** eigenschappen opgeven in het gedeelte `format`. Raadpleeg het gedeelte [TextFormat-voorbeeld](#textformat-example) voor configuratie-instructies.

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| columnDelimiter |Het teken dat wordt gebruikt voor het scheiden van kolommen in een bestand. U kunt overwegen een zeldzaam niet-afdrukbaar teken die mogelijk niet aanwezig zijn in uw gegevens gebruiken. Geef bijvoorbeeld '\u0001', waarmee de Start van de kop (SOH). |Er is slechts één teken toegestaan. De **standaardwaarde** is een **komma (',')**. <br/><br/>Raadpleeg voor het gebruik van een Unicode-teken, [Unicode-tekens](https://en.wikipedia.org/wiki/List_of_Unicode_characters) om op te halen van de bijbehorende code voor deze. |Nee |
| rowDelimiter |Het teken dat wordt gebruikt voor het scheiden van rijen in een bestand. |Er is slechts één teken toegestaan. De **standaardwaarde** is een van de volgende leeswaarden **['\r\n', '\r', '\n']** en de schrijfwaarde **'\r\n'**. |Nee |
| escapeChar |Dit speciale teken wordt gebruikt om een scheidingsteken voor kolommen van de inhoud van het invoerbestand om te zetten. <br/><br/>Het is niet mogelijk om zowel escapeChar als quoteChar voor een tabel op te geven. |Er is slechts één teken toegestaan. Er is geen standaardwaarde. <br/><br/>Voorbeeld: als u met door komma's hebt (', ') als de kolom, maar u wilt het kommateken in de tekst (voorbeeld: "Hallo, wereld"), kunt u '$' als het escape-teken definiëren en gebruiken van de tekenreeks "Hallo$, wereld" in de bron. |Nee |
| quoteChar |Het teken dat wordt gebruikt om een tekenreekswaarde te citeren. De scheidingstekens voor kolommen en rijen binnen de aanhalingstekens worden beschouwd als onderdeel van de tekenreekswaarde. Deze eigenschap is van toepassing op gegevenssets voor invoer en uitvoer.<br/><br/>Het is niet mogelijk om zowel escapeChar als quoteChar voor een tabel op te geven. |Er is slechts één teken toegestaan. Er is geen standaardwaarde. <br/><br/>Voorbeeld: als u kolommen scheidt met komma's (', '), maar u het kommateken in een tekst wilt gebruiken (voorbeeld: <Hallo, wereld>), kunt u " (dubbel aanhalingsteken) als het aanhalingsteken opgeven en de tekenreeks "Hallo, wereld" in de bron gebruiken. |Nee |
| nullValue |Een of meer tekens die worden gebruikt om een null-waarde te vertegenwoordigen. |Een of meer tekens. De **standaardwaarden** zijn **'\N' en 'NULL'** voor lezen en **'\N'** voor schrijven. |Nee |
| encodingName |Geef de coderingsnaam op. |Een geldige coderingsnaam. Zie [De eigenschap Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Voorbeeld: windows 1250 of shift_jis. De **standaardwaarde** is **UTF-8**. |Nee |
| firstRowAsHeader |Hiermee geeft u op of de eerste rij als een header moet worden gezien. Bij een gegevensset voor invoer leest Data Factory de eerste rij als een header. Bij een gegevensset voor uitvoer schrijft Data Factory de eerste rij als een header. <br/><br/>Zie [Gebruiksscenario's`firstRowAsHeader` en `skipLineCount` ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. |True<br/><b>False (standaard)</b> |Nee |
| skipLineCount |Geeft het aantal **niet-lege** rijen dat moet worden overgeslagen bij het lezen van gegevens in invoerbestanden. Als zowel skipLineCount als firstRowAsHeader is opgegeven, worden de regels eerst overgeslagen en wordt de headerinformatie gelezen uit het invoerbestand. <br/><br/>Zie [Gebruiksscenario's`firstRowAsHeader` en `skipLineCount` ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. |Geheel getal |Nee |
| treatEmptyAsNull |Hiermee geeft u aan of null of lege tekenreeks moeten worden behandeld als een null-waarde bij het lezen van gegevens uit een invoerbestand. |**True (standaard)**<br/>False |Nee |

### <a name="textformat-example"></a>Voorbeeld van TextFormat

In de volgende JSON-definitie voor een gegevensset, worden enkele van de optionele eigenschappen opgegeven.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

Gebruik een `escapeChar` in plaats van `quoteChar`, vervang de regel door `quoteChar` met het volgende escapeChar:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenario's voor het gebruik van firstRowAsHeader en skipLineCount

* U wilt kopiëren uit een bron die geen bestand naar een tekstbestand en wilt een headerregel met metagegevens van het schema toevoegen (bijvoorbeeld: SQL-schema). Geef voor `firstRowAsHeader` 'True' op in de uitvoergegevensset voor dit scenario.
* U wilt kopiëren vanuit een tekstbestand met een headerregel naar een sink die geen bestand is en wilt die regel verwijderen. Geef voor `firstRowAsHeader` 'True' op in de invoergegevensset.
* U wilt kopiëren uit een tekstbestand en wilt een paar regels aan het begin overslaan die geen gegevens of headerinformatie bevatten. Geef `skipLineCount` op om aan te geven hoeveel regels er moeten worden overgeslagen. Als de rest van het bestand een headerregel bevat, kunt u ook `firstRowAsHeader` opgeven. Als zowel `skipLineCount` als `firstRowAsHeader` is opgegeven, worden de regels eerst overgeslagen en wordt de headerinformatie gelezen uit het invoerbestand

## <a name="json-format"></a>JSON-indeling

Naar **importeren/exporteren als een JSON-bestand-in / uit Azure Cosmos DB**, Zie de sectie van de Import/export-JSON-documenten in [gegevens verplaatsen naar/van Azure Cosmos DB](connector-azure-cosmos-db.md) artikel.

Als u wilt voor het parseren van JSON-bestanden of de gegevens in JSON-indeling wilt schrijven, stelt u de `type` eigenschap in de `format` gedeelte **JsonFormat**. U kunt ook de volgende **optionele** eigenschappen opgeven in het gedeelte `format`. Zie het gedeelte [JsonFormat-voorbeeld](#jsonformat-example) voor configuratie-instructies.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| filePattern |Hiermee geeft u het patroon aan van gegevens die zijn opgeslagen in elk JSON-bestand. Toegestane waarden zijn **setOfObjects** en **arrayOfObjects**. De **standaardwaarde** is **setOfObjects**. Zie het gedeelte [JSON-bestandpatronen](#json-file-patterns) voor meer informatie over deze patronen. |Nee |
| jsonNodeReference | Als u wilt bladeren en gegevens wilt ophalen uit de objecten in een matrixveld met hetzelfde patroon, geeft u het JSON-pad van die matrix op. Deze eigenschap wordt alleen ondersteund bij kopiëren van gegevens **van** JSON-bestanden. | Nee |
| jsonPathDefinition | Hiermee geeft u de JSON-padexpressie aan voor elke kolomtoewijzing met een aangepaste kolomnaam (begin met een kleine letter). Deze eigenschap wordt alleen ondersteund bij kopiëren van gegevens **van** JSON-bestanden, en u kunt gegevens ophalen uit object of een matrix. <br/><br/> Voor velden onder het hoofdobject begint u met root $; voor velden binnen de matrix die is gekozen door de eigenschap `jsonNodeReference`, begint u vanaf het element van de matrix. Zie het gedeelte [JsonFormat-voorbeeld](#jsonformat-example) voor configuratie-instructies. | Nee |
| encodingName |Geef de coderingsnaam op. Zie voor de lijst met geldige namen: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) eigenschap. Bijvoorbeeld: windows 1250 of shift_jis. De **standaard** waarde is: **UTF-8**. |Nee |
| nestingSeparator |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. De standaardwaarde is '.' (punt). |Nee |

>[!NOTE]
>Voor het geval van gegevens in de matrix met een kruis toepassen in meerdere rijen (case 1 -> voorbeeld 2 in [JsonFormat voorbeelden](#jsonformat-example)), kunt u slechts één matrix met behulp van de eigenschap Vouw `jsonNodeReference`.

### <a name="json-file-patterns"></a>JSON-bestandpatronen

Kopieeractiviteit kan de volgende patronen van JSON-bestanden parseren:

- **Type I: setOfObjects**

    Elk bestand bevat één object of meerdere door regels gescheiden/samengevoegde objecten. Wanneer deze optie is geselecteerd in een uitvoergegevensset, produceert de kopieerbewerking een enkel JSON-bestand met één object per regel (door regels gescheiden).

    * **voorbeeld van JSON-bestand met één object**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **voorbeeld van JSON-bestand dat door regels is gescheiden**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **voorbeeld van JSON-bestand met samengevoegde objecten**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Type II: arrayOfObjects**

    Elk bestand bevat een matrix met objecten.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a>Voorbeeld van JsonFormat

**Geval 1: Kopiëren van gegevens uit JSON-bestanden**

**Voorbeeld 1: gegevens ophalen uit object en matrix**

In dit voorbeeld kunt u verwachten dat één JSON-hoofdobject wordt toegewezen aan één record in het tabelresultaat. Als u een JSON-bestand hebt met de volgende inhoud:

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagementProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```

en u wilt het kopiëren naar een Azure SQL-tabel in de volgende indeling door gegevens te extraheren uit de objecten en matrix:

| Id | deviceType | targetResourceType | resourceManagementProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | Pc | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 13/1/2017 11:24:37 uur |

De invoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Met name:

- Het gedeelte `structure` definieert de aangepaste kolomnamen en het bijbehorende gegevenstype tijdens het converteren van gegevens in tabelvorm. Dit gedeelte is **optioneel**, tenzij u kolommen moet toewijzen. Zie voor meer informatie, [bronkolommen gegevensset worden toegewezen aan de gegevensset doelkolommen](copy-activity-schema-and-type-mapping.md).
- Met `jsonPathDefinition` geeft u het JSON-pad op voor elke kolom die aangeeft waar de gegevens moeten worden opgehaald. U kunt gebruiken om gegevens te kopiëren van de matrix, `array[x].property` om op te halen van de waarde van de opgegeven eigenschap van de `xth` object, of u kunt gebruiken `array[*].property` gezocht naar de waarde van een object met deze eigenschap.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagementProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagementProcessRunId": "$.context.custom.dimensions[1].ResourceManagementProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}
        }
    }
}
```

**Voorbeeld 2: meerdere objecten met hetzelfde patroon uit een matrix toepassen**

In dit voorbeeld probeert u een JSON-hoofdobject te transformeren naar meerdere records in een tabelresultaat. Als u een JSON-bestand hebt met de volgende inhoud:

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```

en u wilt het kopiëren naar een Azure SQL-tabel in de volgende indeling, door de gegevens binnen de matrix af te vlakken en samen te voegen met de algemene root-gegevens:

| `ordernumber` | `orderdate` | `order_pd` | `order_price` | `city` |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P2 | 13 | `[{"sanmateo":"No 1"}]` |
| 01 | 20170122 | P3 | 231 | `[{"sanmateo":"No 1"}]` |


De invoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Met name:

- Het gedeelte `structure` definieert de aangepaste kolomnamen en het bijbehorende gegevenstype tijdens het converteren van gegevens in tabelvorm. Dit gedeelte is **optioneel**, tenzij u kolommen moet toewijzen. Zie voor meer informatie, [bronkolommen gegevensset worden toegewezen aan de gegevensset doelkolommen](copy-activity-schema-and-type-mapping.md).
- `jsonNodeReference` Hiermee wordt aangegeven en dat er gegevens ophalen uit de objecten met hetzelfde patroon onder **matrix** `orderlines`.
- Met `jsonPathDefinition` geeft u het JSON-pad op voor elke kolom die aangeeft waar de gegevens moeten worden opgehaald. In dit voorbeeld `ordernumber`, `orderdate`, en `city` zijn onder het hoofdobject met JSON-pad begint met `$.`, terwijl `order_pd` en `order_price` zijn gedefinieerd met het pad dat is afgeleid van het matrixelement zonder `$.` .

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}
        }
    }
}
```

**Houd rekening met de volgende punten:**

* Als `structure` en `jsonPathDefinition` niet zijn gedefinieerd in de gegevensset van Data Factory, detecteert de kopieerbewerking het schema van het eerste object en wordt het hele object afgevlakt.
* Als de JSON-invoer een matrix heeft, zet de kopieerbewerking de volledige matrix-waarde standaard om in een tekenreeks. U kunt ervoor kiezen om er gegevens uit op te halen met behulp van `jsonNodeReference` en/of `jsonPathDefinition`, of deze stap over te slaan door deze niet op te geven in `jsonPathDefinition`.
* Als er dubbele namen op hetzelfde niveau voorkomen, gebruikt de kopieerbewerking de laatste.
* Eigenschapnamen zijn hoofdlettergevoelig. Twee eigenschappen met dezelfde naam maar met een verschil in hoofdletters en kleine letters worden behandeld als twee afzonderlijke eigenschappen.

**Geval 2: Gegevens schrijven naar een JSON-bestand**

Als u de volgende tabel in SQL-Database hebt:

| Id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

en voor elke record die u verwacht te schrijven naar een JSON-object in de volgende indeling:

```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

De uitvoergegevensset met het type **JsonFormat** wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen belangrijke onderdelen). Meer specifiek, `structure` gedeelte definieert de namen van aangepaste eigenschappen in het doelbestand, `nestingSeparator` (de standaardwaarde is '. ') worden gebruikt voor identificatie van de nestlaag van de naam. Dit gedeelte is **optioneel**, tenzij u de naam van de eigenschap wilt wijzigen ten opzichte van de naam van de bronkolom of sommige eigenschappen wilt nesten.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="parquet-format"></a>Parquet-indeling

>[!NOTE]
>Data Factory nieuwe Parquet-indeling DataSet geïntroduceerd, Zie [Parquet-indeling](format-parquet.md) artikel met informatie. De volgende configuraties op bestanden gebaseerde gegevens store gegevensset wordt nog steeds ondersteund als-is voor achterwaartse compabitility. U worden gebruik van het nieuwe model voortaan voorgesteld.

Als u de Parquet-bestanden wilt parseren of de gegevens in Parquet-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **ParquetFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "ParquetFormat"
}
```

Houd rekening met de volgende punten:

* Complexe gegevenstypen niet worden ondersteund (MAP, LIST).
* Witruimte in de naam van kolom wordt niet ondersteund.
* Parquet-bestand heeft de volgende opties voor compressie: NONE, SNAPPY, GZIP en LZO. Data Factory ondersteunt het lezen van gegevens in Parquet-bestanden in een van deze gecomprimeerde indelingen behalve LZO: deze wordt de compressiecodec in de metagegevens van de gegevens niet lezen. Bij het schrijven naar een Parquet-bestand kiest Data Factory echter SNAPPY, de standaardinstelling voor Parquet. Er is momenteel geen optie om dit gedrag te overschrijven.

> [!IMPORTANT]
> Voor kopiëren gemachtigd door zelfgehoste Cloudintegratieruntime bijvoorbeeld tussen on-premises en cloud gegevensarchieven, als u niet Parquet-bestanden kopieert **als-is**, moet u voor het installeren van de **64-bits JRE 8 (Java Runtime Environment) of OpenJDK** op uw computer IR. Zie de volgende alinea met meer informatie.

Voor het exemplaar wordt uitgevoerd in zelfgehoste IR en Parquet-bestand serialisatie/deserialisatie, ADF wordt gezocht naar de Java-runtime door te controleren in de eerste plaats het register *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* voor JRE, als dat niet wordt gevonden, ten tweede controle systeemvariabele *`JAVA_HOME`* voor OpenJDK.

- **Gebruik Java Runtime Environment**: De 64-bits-IR is 64-bits JRE vereist. U vindt deze in [hier](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Gebruik OpenJDK**: dit wordt ondersteund sinds IR versie 3.13. Pakket de jvm.dll met alle andere vereiste assembly's van OpenJDK in zelfgehoste IR-machine en set-systeemomgevingsvariabele JAVA_HOME dienovereenkomstig.

>[!TIP]
>Als u kopieert gegevens naar/vanuit Parquet-indeling met behulp van zelfgehoste Cloudintegratieruntime en druk op fout met de melding "Er is een fout opgetreden bij het aanroepen van java, bericht: **java.lang.OutOfMemoryError:Java heap ruimte**', kunt u een omgevingsvariabele toevoegen `_JAVA_OPTIONS` in de computer die als host fungeert voor de zelfgehoste IR om aan te passen de min/max heap-grootte voor JVM voor die te maken van dergelijke kopiëren, klikt u vervolgens de pijplijn opnieuw uitvoeren.

![JVM-heap-grootte instellen op zelfgehoste IR](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Voorbeeld: set-variabele `_JAVA_OPTIONS` met waarde `-Xms256m -Xmx16g`. De vlag `Xms` Hiermee geeft u de eerste toewijzing van de geheugengroep voor een Java Virtual Machine (JVM), terwijl `Xmx` geeft de maximale hoeveelheid geheugen toegewezen groep. Dit betekent dat JVM wordt gestart met `Xms` hoeveelheid geheugen en kunnen maximaal gebruiken `Xmx` hoeveelheid geheugen. Standaard ADF gebruiken van 64MB min en max 1G.

### <a name="data-type-mapping-for-parquet-files"></a>Gegevenstype toewijzing voor Parquet-bestanden

| Data factory tussentijdse gegevenstype | Parquet primitief Type | Parquet oorspronkelijke Type (deserialiseren) | Parquet oorspronkelijke Type (serialiseren) |
|:--- |:--- |:--- |:--- |
| Booleaans | Booleaans | N/A | N/A |
| SByte | Int32 | Int8 | Int8 |
| Byte | Int32 | UInt8 | Int16 |
| Int16 | Int32 | Int16 | Int16 |
| UInt16 | Int32 | UInt16 | Int32 |
| Int32 | Int32 | Int32 | Int32 |
| UInt32 | Int64 | UInt32 | Int64 |
| Int64 | Int64 | Int64 | Int64 |
| UInt64 | Int64 binaire | UInt64 | decimaal |
| Enkelvoudig | Float | N/A | N/A |
| Double-waarde | Double-waarde | N/A | N/A |
| decimaal | Binair bestand | decimaal | decimaal |
| Reeks | Binair bestand | Utf8 | Utf8 |
| DateTime | Int96 | N/A | N/A |
| TimeSpan | Int96 | N/A | N/A |
| DateTimeOffset | Int96 | N/A | N/A |
| ByteArray | Binair bestand | N/A | N/A |
| GUID | Binair bestand | Utf8 | Utf8 |
| CHAR | Binair bestand | Utf8 | Utf8 |
| CharArray | Niet ondersteund | N/A | N/A |

## <a name="orc-format"></a>ORC-indeling

Als u de ORC-bestanden wilt parseren of de gegevens in ORC-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **OrcFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "OrcFormat"
}
```

Houd rekening met de volgende punten:

* Complexe gegevenstypen niet worden ondersteund (STRUCT, MAP, LIST, UNION).
* Witruimte in de naam van kolom wordt niet ondersteund.
* ORC-bestand heeft drie [opties voor compressie](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory ondersteunt het lezen van gegevens uit ORC-bestanden in een van deze gecomprimeerde indelingen. Hierbij wordt de compressiecodec in de metagegevens gebruikt om de gegevens te lezen. Bij het schrijven naar een ORC-bestand kiest Data Factory echter ZLIB, de standaardinstelling voor ORC. Er is momenteel geen optie om dit gedrag te overschrijven.

> [!IMPORTANT]
> Voor kopiëren gemachtigd door zelfgehoste Cloudintegratieruntime bijvoorbeeld tussen on-premises en cloud gegevensarchieven, als u niet ORC-bestanden kopieert **als-is**, moet u voor het installeren van de **64-bits JRE 8 (Java Runtime Environment) of OpenJDK**  op uw computer IR. Zie de volgende alinea met meer informatie.

Voor het exemplaar wordt uitgevoerd in zelfgehoste IR en ORC-bestand serialisatie/deserialisatie, ADF wordt gezocht naar de Java-runtime door te controleren in de eerste plaats het register *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* voor JRE, als dat niet wordt gevonden, ten tweede controle systeemvariabele *`JAVA_HOME`* voor OpenJDK.

- **Gebruik Java Runtime Environment**: De 64-bits-IR is 64-bits JRE vereist. U vindt deze in [hier](https://go.microsoft.com/fwlink/?LinkId=808605).
- **Gebruik OpenJDK**: dit wordt ondersteund sinds IR versie 3.13. Pakket de jvm.dll met alle andere vereiste assembly's van OpenJDK in zelfgehoste IR-machine en set-systeemomgevingsvariabele JAVA_HOME dienovereenkomstig.

### <a name="data-type-mapping-for-orc-files"></a>Gegevenstype toewijzing voor ORC-bestanden

| Data factory tussentijdse gegevenstype | ORC-typen |
|:--- |:--- |
| Booleaans | Booleaans |
| SByte | Byte |
| Byte | Kort |
| Int16 | Kort |
| UInt16 | Int |
| Int32 | Int |
| UInt32 | Lang |
| Int64 | Lang |
| UInt64 | Reeks |
| Enkelvoudig | Float |
| Double-waarde | Double-waarde |
| decimaal | decimaal |
| Reeks | Reeks |
| DateTime | Tijdstempel |
| DateTimeOffset | Tijdstempel |
| TimeSpan | Tijdstempel |
| ByteArray | Binair bestand |
| GUID | Reeks |
| CHAR | CHAR(1) |

## <a name="avro-format"></a>AVRO-indeling

Als u de Avro-bestanden wilt parseren of de gegevens in Avro-indeling wilt schrijven, stelt u de eigenschap `format` `type` in op **AvroFormat**. U hoeft geen eigenschappen op te geven in het gedeelte Indeling binnen het gedeelte typeProperties. Voorbeeld:

```json
"format":
{
    "type": "AvroFormat",
}
```

Als u de Avro-indeling wilt gebruiken in een Hive-tabel, kunt u de [Zelfstudie voor Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe) raadplegen.

Houd rekening met de volgende punten:

* [Complexe gegevenstypen](https://avro.apache.org/docs/current/spec.html#schema_complex) worden niet ondersteund (records, enums, matrices, kaarten, samenvoegingen, en vaste bestanden).

## <a name="compression-support"></a>Compressieondersteuning voor

Azure Data Factory ondersteunt comprimeren/decomprimeren van gegevens tijdens het kopiëren. Wanneer u opgeeft `compression` eigenschap in een invoergegevensset, de kopieerbewerking de gecomprimeerde gegevens lezen van de bron- en decomprimeren; en wanneer u de eigenschap opgeeft in een uitvoergegevensset, de copy-activiteit gecomprimeerd en vervolgens gegevens schrijven naar de sink. Hier volgen enkele eenvoudige voorbeeldscenario's:

* Lezen GZIP gecomprimeerde gegevens uit een Azure-blob, het decomprimeren en resultaatgegevens schrijven naar een Azure SQL database. U definieert de Azure Blob-invoergegevensset met het `compression` `type` eigenschap zoals GZIP.
* Gegevens lezen uit een bestand met tekst zonder opmaak van on-premises bestandssysteem, comprimeren met behulp van GZip-indeling en de gecomprimeerde gegevens schrijven naar een Azure-blob. Definieert u een Azure Blob-uitvoergegevensset met de `compression` `type` eigenschap zoals GZip.
* ZIP-bestand lezen van FTP-server, decomprimeren voor het ophalen van de bestanden in, en deze bestanden worden neergezet in Azure Data Lake Store. U definieert een invoergegevensset FTP met de `compression` `type` eigenschap ZipDeflate.
* Een gecomprimeerde GZIP-gegevens lezen uit een Azure-blob, het decomprimeren, comprimeren met behulp van BZIP2 en resultaatgegevens schrijven naar een Azure-blob. U definieert de Azure Blob-invoergegevensset met `compression` `type` ingesteld op GZIP- en de uitvoergegevensset met `compression` `type` ingesteld op BZIP2.

Als u compressie voor een gegevensset, gebruikt u de **compressie** eigenschap in de gegevensset JSON zoals in het volgende voorbeeld:

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "fileName": "pagecounts.csv.gz",
            "folderPath": "compression/file/",
            "format": {
                "type": "TextFormat"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

De **compressie** sectie heeft twee eigenschappen:

* **Type:** de compressiecodec, dit kan **GZIP**, **Deflate**, **BZIP2**, of **ZipDeflate**.
* **Niveau:** de compressieverhouding, dit kan **optimale** of **snelst**.

  * **Fastest:** De compressie-bewerking moet zo snel mogelijk worden voltooid, zelfs als het resulterende bestand is niet optimaal gecomprimeerd.
  * **Optimale**: De compressie-bewerking moet optimaal zijn gecomprimeerd, zelfs als de bewerking duurt het langer om te voltooien.

    Zie voor meer informatie, [compressieniveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) onderwerp.

> [!NOTE]
> Compressie-instellingen worden niet ondersteund voor gegevens in de **AvroFormat**, **OrcFormat**, of **ParquetFormat**. Bij het lezen van bestanden in de volgende indelingen, wordt in Data Factory wordt gedetecteerd en wordt de compressiecodec in de metagegevens. Bij het schrijven naar bestanden in de volgende indelingen, kiest Data Factory de compressiecodec standaard voor deze indeling. Bijvoorbeeld, ZLIB voor OrcFormat en SNAPPY voor ParquetFormat.

## <a name="unsupported-file-types-and-compression-formats"></a>Niet-ondersteunde bestandstypen en compressie-indelingen

De functies van de uitbreidbaarheid van Azure Data Factory kunt u transformatie van bestanden die niet worden ondersteund.
Twee opties zijn onder andere Azure Functions en aangepaste taken met behulp van Azure Batch.

U kunt een voorbeeld dat gebruikmaakt van een Azure-functie om te zien [Pak de inhoud van een bestand met tar](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV2/UntarAzureFilesWithAzureFunction). Zie voor meer informatie, [Azure Functions-activiteit](https://docs.microsoft.com/azure/data-factory/control-flow-azure-function-activity).

U kunt ook deze functionaliteit met behulp van een aangepaste dotnet-activiteit maken. Meer informatie vindt u [hier](https://docs.microsoft.com/azure/data-factory/transform-data-using-dotnet-custom-activity)

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor gegevensarchieven die op bestanden gebaseerde ondersteund door Azure Data Factory:

- [Azure Blob Storage-connector](connector-azure-blob-storage.md)
- [Azure Data Lake Store-connector](connector-azure-data-lake-store.md)
- [Amazon S3-connector](connector-amazon-simple-storage-service.md)
- [Bestandssysteemconnector](connector-file-system.md)
- [FTP-connector](connector-ftp.md)
- [SFTP-connector](connector-sftp.md)
- [HDFS-connector](connector-hdfs.md)
- [HTTP-connector](connector-http.md)
