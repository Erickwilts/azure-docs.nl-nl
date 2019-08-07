---
title: 'REST zelf studie: Semi-strutured gegevens in JSON-blobs indexeren-Azure Search'
description: Meer informatie over het indexeren en doorzoeken van semi-gestructureerde Azure JSON-blobs met Azure Search REST Api's en postman.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6362e3cccd3c306a210f0241214206204b5ee96e
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840700"
---
# <a name="rest-tutorial-index-and-search-semi-structured-data-json-blobs-in-azure-search"></a>REST zelf studie: Semi-gestructureerde gegevens (JSON-blobs) indexeren en doorzoeken in Azure Search

Azure Search kunt JSON-documenten en-matrices indexeren in Azure Blob-opslag met behulp van een [Indexeer functie](search-indexer-overview.md) die het lezen van semi-gestructureerde gegevens kent. Semi-gestructureerde gegevens bevatten labels of markeringen die inhoud in de gegevens scheiden. Het verschil tussen ongestructureerde gegevens, dat volledig kan worden geïndexeerd, en formeel gestructureerde gegevens die voldoen aan een gegevens model, zoals een relationeel database schema, wordt gesplitst, dat per veld kan worden geïndexeerd.

In deze zelf studie gebruikt u de [Azure Search rest-api's](https://docs.microsoft.com/rest/api/searchservice/) en een rest-client om de volgende taken uit te voeren:

> [!div class="checklist"]
> * Een Azure Search-gegevensbron configureren voor een Azure-blobcontainer
> * Een Azure Search index maken die Doorzoek bare inhoud bevat
> * Een Indexeer functie configureren en uitvoeren om de container te lezen en Doorzoek bare inhoud uit Azure Blob-opslag te halen
> * De index doorzoeken die u zojuist hebt gemaakt

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogram ma's en gegevens worden gebruikt in deze Quick Start. 

[Een Azure Search-service maken](search-create-service-portal.md) of [een bestaande service vinden](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) onder uw huidige abonnement. U kunt voor deze zelf studie gebruikmaken van een gratis service. 

[Maak een Azure-opslag account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van de voorbeeld gegevens.

[Postman desktop-app](https://www.getpostman.com/) voor het verzenden van aanvragen naar Azure Search.

[Clinical-Trials-JSON. zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip) bevat de gegevens die in deze zelf studie worden gebruikt. Down load en pak dit bestand uit naar een eigen map. De gegevens zijn afkomstig uit [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results), GECONVERTEERD naar JSON voor deze zelf studie.

## <a name="get-a-key-and-url"></a>Een sleutel en URL ophalen

REST-aanroepen hebben voor elke aanvraag de service-URL en een toegangssleutel nodig. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij de Azure Portal](https://portal.azure.com/)en down load de URL op de pagina **overzicht** van de zoek service. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. Haal in **instellingen** > **sleutels**een beheerders sleutel op voor volledige rechten op de service. Er zijn twee uitwissel bare beheer sleutels die voor bedrijfs continuïteit worden verschaft, voor het geval dat u een voor beeld moet doen. U kunt de primaire of secundaire sleutel gebruiken op aanvragen voor het toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eind punt en toegangs sleutel ophalen](media/search-get-started-postman/get-url-key.png "Een HTTP-eind punt en toegangs sleutel ophalen")

Voor alle aanvragen is een API-sleutel vereist voor elke aanvraag die naar uw service wordt verzonden. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="prepare-sample-data"></a>Voorbeeld gegevens voorbereiden

1. [Meld u aan bij de Azure Portal](https://portal.azure.com), navigeer naar uw Azure Storage-account, klik op blobs en klik vervolgens op **+ container**.

1. [Maak een BLOB-container](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) om voorbeeld gegevens te bevatten. U kunt het niveau van open bare toegang instellen op een van de geldige waarden.

1. Nadat de container is gemaakt, opent u deze en selecteert u **uploaden** op de opdracht balk.

   ![Uploaden op de opdracht balk](media/search-semi-structured-data/upload-command-bar.png "Uploaden op de opdracht balk")

1. Navigeer naar de map met de voorbeeld bestanden. Selecteer alles en klik vervolgens op **uploaden**.

   ![Bestanden uploaden](media/search-semi-structured-data/clinicalupload.png "Bestanden uploaden")

Nadat de upload is voltooid, worden de bestanden weergegeven in hun eigen submap in de gegevenscontainer.

## <a name="set-up-postman"></a>Postman instellen

Start Postman en stel een HTTP-aanvraag in. Als u niet bekend bent met dit hulp programma, raadpleegt u [Azure Search rest-Api's verkennen met behulp van Postman](search-get-started-postman.md).

De aanvraag methode voor elke aanroep in deze zelf studie is **post**. De headersleutels zijn 'Content-type' en 'api-key'. De waarden van de headersleutels zijn respectievelijk 'application/json' en 'admin key' (admin key is een tijdelijke aanduiding voor de primaire sleutel van uw zoekopdracht). In de hoofdtekst plaatst u de werkelijke inhoud van uw aanroep. Afhankelijk van de client die u gebruikt, kunnen er enkele variaties zijn op de manier waarop u uw query samenstelt, maar dit zijn de basisprincipes.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/postmanoverview.png)

We gebruiken Postman om drie API-aanroepen te doen naar uw zoekservice; voor het maken van een gegevensbron, een index en een indexeerfunctie. De gegevensbron bevat een verwijzing naar uw opslagaccount en uw JSON-gegevens. Uw zoekservice maakt de verbinding tijdens het laden van de gegevens.

In query reeksen moeten een API-versie worden opgegeven en elke aanroep moet een **201-gemaakt**retour neren. De algemeen beschik bare API-versie voor het gebruik van `2019-05-06`JSON-matrices is.

Voer de volgende drie API-aanroepen uit vanuit de REST-client.

## <a name="create-a-data-source"></a>Een gegevensbron maken

Met de [API Create Data Source](https://docs.microsoft.com/rest/api/searchservice/create-data-source)wordt een Azure Search-object gemaakt waarmee wordt opgegeven welke gegevens moeten worden geïndexeerd.

Het eindpunt van deze aanroep is `https://[service name].search.windows.net/datasources?api-version=2019-05-06`. Vervang `[service name]` door de naam van uw zoekservice. 

Voor deze aanroep moet de aanvraag tekst de naam van uw opslag account, de sleutel van het opslag account en de naam van de BLOB-container bevatten. De opslagaccountsleutel vindt u in de **Toegangssleutels** van uw opslagaccount in Azure Portal. De locatie wordt in de volgende afbeelding weergegeven:

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/storagekeys.png)

Zorg ervoor dat u `[storage account name]`, `[storage account key]`en `[blob container name]` in de hoofd tekst van uw oproep vervangt voordat u de oproep uitvoert.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "[blob container name]"}
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "[mycontainernamehere]",
        "query": null
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

## <a name="create-an-index"></a>Een index maken
    
De tweede aanroep is [Create Index API](https://docs.microsoft.com/rest/api/searchservice/create-indexer), waarmee een Azure search index wordt gemaakt waarmee alle Doorzoek bare gegevens worden opgeslagen. Een index geeft alle parameters en hun kenmerken op.

De URL voor deze aanroep is `https://[service name].search.windows.net/indexes?api-version=2019-05-06`. Vervang `[service name]` door de naam van uw zoekservice.

Vervang eerst de URL. Kopieer daarna de volgende code, plak deze in de hoofdtekst en voer de query uit.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

## <a name="create-and-run-an-indexer"></a>Een Indexeer functie maken en uitvoeren

Een Indexeer functie verbindt de gegevens bron, importeert gegevens in de doel zoek index en biedt optioneel een schema voor het automatiseren van de gegevens vernieuwing. De REST API is [Indexeer functie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

De URL voor deze aanroep is `https://[service name].search.windows.net/indexers?api-version=2019-05-06`. Vervang `[service name]` door de naam van uw zoekservice.

Vervang eerst de URL. Kopieer en plak de volgende code in de hoofd tekst en verzend de aanvraag. De aanvraag wordt onmiddellijk verwerkt. Wanneer het antwoord terugkomt, hebt u een index die in volledige tekst kan worden doorzocht.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

Het antwoord moet er als volgt uitzien:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Uw JSON-bestanden doorzoeken

U kunt beginnen met zoeken zodra het eerste document is geladen. Gebruik voor deze taak [**Search Explorer**](search-explorer.md) in de portal.

Open in Azure Portal de pagina **overzicht** van zoek services en zoek de index die u hebt gemaakt in de lijst **indexen** .

Zorg ervoor dat u de index kiest die u zojuist hebt gemaakt. 

  ![Niet-gestructureerde zoekopdracht](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Door gebruiker gedefinieerde metagegevens doorzoeken

Ook nu kunnen gegevens worden opgevraagd op verschillende manieren: zoeken in volledige tekst, systeemeigenschappen of door de gebruiker gedefinieerde metagegevens. Systeemeigenschappen en door de gebruiker gedefinieerde metagegevens kunnen alleen worden doorzocht met de parameter `$select` als ze tijdens het maken van de doelindex zijn gemarkeerd als **Ophalen mogelijk**. Parameters in de index mogen niet meer worden gewijzigd nadat ze zijn gemaakt. Er kunnen echter wel extra parameters worden toegevoegd.

Een voorbeeld van een eenvoudige query is `$select=Gender,metadata_storage_size`. Deze retourneert alleen deze twee parameters.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/lastquery.png)

Een voorbeeld van een complexere query is `$filter=MinimumAge ge 30 and MaximumAge lt 75`. Deze retourneert alleen resultaten als de parameter MinimumAge groter is dan of gelijk is aan 30 en als MaximumAge kleiner is dan 75.

  ![Semi-gestructureerde zoekopdracht](media/search-semi-structured-data/metadatashort.png)

Ga gerust uw gang als u wilt experimenteren en zelf nog een aantal query's wilt proberen. U kunt logische operators (AND, OR, NOT) en vergelijkingsoperators (eq, ne, gt, lt, ge en le) gebruiken. Tekenreeksvergelijkingen zijn hoofdlettergevoelig.

De parameter `$filter` werkt alleen met metagegevens die bij het maken van de index zijn gemarkeerd als filterbaar.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is de resourcegroep met de Azure Search-service te verwijderen. U kunt de resourcegroep nu verwijderen om alles daarin permanent te verwijderen. De naam van de resourcegroep staat in de portal op de pagina Overzicht van de Azure Search-service.

## <a name="next-steps"></a>Volgende stappen

Er zijn verschillende benaderingen en meerdere opties voor het indexeren van JSON-blobs. Bekijk en test de verschillende opties als volgende stap om te zien wat het beste werkt voor uw scenario.

> [!div class="nextstepaction"]
> [JSON-blobs indexeren met Azure Search BLOB-Indexer](search-howto-index-json-blobs.md)
