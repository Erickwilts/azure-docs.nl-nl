---
title: Meerdere talen indexeren voor niet-Engelse Zoek query's-Azure Search
description: Azure Search ondersteunt 56 talen, met taal analyse functies van Lucene en natuurlijke taal verwerkings technologie van micro soft.
author: yahnoosh
manager: jlembicz
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: jlembicz
ms.custom: seodec2018
ms.openlocfilehash: 403febfcb54194602051aaebe2952265c0675e9d
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854393"
---
# <a name="how-to-create-an-index-for-multiple-languages-in-azure-search"></a>Een index maken voor meerdere talen in Azure Search

Indexen kunnen velden bevatten met inhoud uit meerdere talen, zoals het maken van afzonderlijke velden voor taalspecifieke teken reeksen. Voor de beste resultaten tijdens het indexeren en opvragen van query's, wijst u een taal analyse toe die de juiste taal regels biedt. 

Azure Search biedt een groot aantal taal analyse functies van zowel Lucene als micro soft dat kan worden toegewezen aan afzonderlijke velden met behulp van de eigenschap Analyzer. U kunt ook een taal analyse opgeven in de portal, zoals beschreven in dit artikel.

## <a name="add-analyzers-to-fields"></a>Analyse functies toevoegen aan velden

Er wordt een taal analyse opgegeven wanneer een veld wordt gemaakt. Voor het toevoegen van een Analyzer aan een bestaande veld definitie moet de index worden overschreven (en opnieuw worden geladen) of moet er een nieuw veld worden gemaakt dat identiek is aan het origineel, maar met een analyse toewijzing. Vervolgens kunt u het ongebruikte veld op uw gemak verwijderen.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) en zoek uw zoek service.
1. Klik op **index toevoegen** in de opdracht balk boven aan het service dashboard om een nieuwe index te starten of open een bestaande index om een analyse programma in te stellen op nieuwe velden die u aan een bestaande index toevoegt.
1. Een veld definitie starten door een naam op te geven.
1. Kies het gegevens type EDM. String. Alleen teken reeks velden zijn zoeken in volledige tekst.
1. Stel het kenmerk **Doorzoek** bare in om de eigenschap Analyzer in te scha kelen. Een veld moet op tekst gebaseerd zijn om een taal analyse te kunnen gebruiken.
1. Kies een van de beschik bare analyse functies. 

![Taal analyseers toewijzen tijdens veld definitie](media/search-language-support/select-analyzer.png "Taal analyseers toewijzen tijdens veld definitie")

In alle Doorzoek bare velden wordt standaard de [standaard-lucene Analyzer](https://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) gebruikt. de taal neutraal. Als u de volledige lijst met ondersteunde analyse functies wilt bekijken, raadpleegt [u taal analysen toevoegen aan een Azure search-index](index-add-language-analyzers.md).

In de portal zijn analyse functies bedoeld om te worden gebruikt als-is. Als u aanpassingen of een specifieke configuratie van filters en tokenizers nodig hebt, moet u [een aangepaste Analyzer](index-add-custom-analyzers.md) in code maken. De portal biedt geen ondersteuning voor het selecteren of configureren van aangepaste analyse functies.

## <a name="query-language-specific-fields"></a>Query taal afhankelijke velden

Zodra de taal analyse voor een veld is geselecteerd, wordt het gebruikt met elke indexerings-en zoek aanvraag voor dat veld. Wanneer een query wordt uitgegeven voor meerdere velden met verschillende analyse functies, wordt de query onafhankelijk verwerkt door de toegewezen analyse functies voor elk veld.

Als de taal van de agent die een query heeft uitgegeven, bekend is, kan een zoek opdracht worden toegewezen aan een specifiek veld met behulp van de **searchFields** -query parameter. De volgende query wordt alleen uitgegeven op basis van de beschrijving in Pools:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=PolishContent&api-version=2019-05-06`

U kunt een query uitvoeren op uw index vanuit de portal, met behulp van [**Search Explorer**](search-explorer.md) om een query te plakken die vergelijkbaar is met de hierboven weer gegeven.

## <a name="boost-language-specific-fields"></a>Taalspecifieke velden verhogen

Soms is de taal van de agent waarmee een query wordt uitgegeven niet bekend. in dat geval kan de query worden uitgegeven voor alle velden tegelijk. Als dat nodig is, kan de voor keur voor de resultaten in een bepaalde taal worden gedefinieerd met behulp van [Score profielen](index-add-scoring-profiles.md). In het onderstaande voor beeld krijgen overeenkomsten die in de beschrijving in het Engels zijn gevonden, hoger ten opzichte van overeenkomsten in Pools en Frans:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2019-05-06`

## <a name="next-steps"></a>Volgende stappen

Als u een .NET-ontwikkelaar bent, kunt u taal analyse functies configureren met behulp van de [Azure Search .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Search) en de eigenschap [Analyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer?view=azure-dotnet) . 