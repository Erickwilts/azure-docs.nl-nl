---
title: Entiteit erkenning cognitief zoeken vaardigheid - Azure Search
description: Verschillende typen entiteiten extraheren uit tekst in een Azure Search cognitief zoeken-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f05161dbbfd9293cd7b1cbf447bb7ca1c313250c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65023447"
---
#    <a name="entity-recognition-cognitive-skill"></a>Entiteit herkenning van cognitieve vaardigheden

De **entiteit erkenning** vaardigheid entiteiten van verschillende typen geëxtraheerd uit tekst. Deze vaardigheid maakt gebruik van de machine learning-modellen die worden geleverd door [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) in Cognitive Services.

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.EntityRecognitionSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u moet het opsplitsen van uw gegevens voordat deze naar de extractor sleuteluitdrukkingen verzonden, kunt u overwegen de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig en zijn optioneel.

| Parameternaam     | Description |
|--------------------|-------------|
| categorieën    | Matrix van categorieën die moeten worden geëxtraheerd.  Mogelijke categorietypen: `"Person"`, `"Location"`, `"Organization"`, `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"`. Als er geen categorie is opgegeven, worden alle typen worden geretourneerd.|
|defaultLanguageCode |  De taalcode van de invoertekst. De volgende talen worden ondersteund: `de, en, es, fr, it`|
|minimumPrecision | Niet-gebruikte. Gereserveerd voor toekomstig gebruik. |
|includeTypelessEntities | Als de waarde in op true als de tekst bevat van een entiteit erg bekend is, maar kan niet worden onderverdeeld in een van de ondersteunde categorieën, deze wordt geretourneerd als onderdeel van de `"entities"` complexe uitvoerveld. 
Dit zijn de entiteiten die goed bekend zijn, maar niet zijn geclassificeerd als onderdeel van de huidige ondersteunde 'categorieën'. Bijvoorbeeld ' Windows 10 ' is een bekende entiteit (product), maar 'Producten' zich niet in de categorieën die tegenwoordig wordt ondersteund. De standaardwaarde is `false` |


## <a name="skill-inputs"></a>Kwalificatie invoer

| Voer een naam in      | Description                   |
|---------------|-------------------------------|
| languageCode  | Optioneel. De standaardwaarde is `"en"`.  |
| text          | De tekst te analyseren.          |

## <a name="skill-outputs"></a>Kwalificatie uitvoer

> [!NOTE]
> Niet alle categorieën van de entiteit worden ondersteund voor alle talen. Alleen _en_, _es_ ondersteuning voor extractie van `"Quantity"`, `"Datetime"`, `"URL"`, `"Email"` typen.

| Naam van de uitvoer     | Description                   |
|---------------|-------------------------------|
| personen      | Een matrix met tekenreeksen waarbij elke tekenreeks de naam van een persoon vertegenwoordigt. |
| locations  | Een matrix met tekenreeksen waarbij elke tekenreeks een locatie vertegenwoordigt. |
| organizations  | Een matrix met tekenreeksen waarbij elke tekenreeks een organisatie vertegenwoordigt. |
| hoeveelheden  | Een matrix met tekenreeksen waarbij elke tekenreeks een hoeveelheid vertegenwoordigt. |
| datum/tijd  | Een matrix met tekenreeksen, waarbij elke tekenreeks een datum/tijd vertegenwoordigt (zoals deze wordt weergegeven in de tekst) waarde. |
| URL 's | Een matrix met tekenreeksen, waarbij elke tekenreeks een URL vertegenwoordigt |
| e-mailberichten | Een matrix met tekenreeksen, waarbij elke tekenreeks een e-mailbericht vertegenwoordigt |
| namedEntities | Een matrix van complexe typen die de volgende velden bevat: <ul><li>category</li> <li>waarde (de naam van de werkelijke entiteit)</li><li>offset (de locatie waar deze is gevonden in de tekst)</li><li>vertrouwen (wordt niet gebruikt voor nu. Wordt ingesteld op een waarde van-1)</li></ul> |
| Entiteiten | Een matrix van complexe typen die uitgebreide informatie over de entiteiten die zijn geëxtraheerd uit tekst, met de volgende velden bevat <ul><li> naam (de naam van de werkelijke entiteit. Hiermee wordt een formulier "genormaliseerde")</li><li> wikipediaId</li><li>wikipediaLanguage</li><li>wikipediaUrl (een koppeling naar Wikipedia-pagina voor de entiteit)</li><li>bingId</li><li>type (de categorie van de entiteit herkend)</li><li>subType (alleen beschikbaar voor bepaalde categorieën, dit biedt een meer gedetailleerd overzicht van het entiteitstype)</li><li> komt overeen met (een complexe verzameling met)<ul><li>tekst (de onbewerkte tekst voor de entiteit)</li><li>offset (de locatie waar deze is gevonden)</li><li>lengte (de lengte van de entiteit onbewerkte tekst)</li></ul></li></ul> |

##  <a name="sample-definition"></a>Van voorbeelddefinitie

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person", "Email"],
    "defaultLanguageCode": "en",
    "includeTypelessEntities": true,
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      },
      {
        "name": "emails",
        "targetName": "contact"
      },
      {
        "name": "entities"
      }
    ]
  }
```
##  <a name="sample-input"></a>Van Voorbeeldinvoer

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Contoso corporation was founded by John Smith. They can be reached at contact@contoso.com",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "John Smith"],
        "emails":["contact@contoso.com"],
        "namedEntities": 
        [
          {
            "category":"Person",
            "value": "John Smith",
            "offset": 35,
            "confidence": -1
          }
        ],
        "entities":  
        [
          {
            "name":"John Smith",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Person",
            "subType": null,
            "matches": [{
                "text": "John Smith",
                "offset": 35,
                "length": 10
            }]
          },
          {
            "name": "contact@contoso.com",
            "wikipediaId": null,
            "wikipediaLanguage": null,
            "wikipediaUrl": null,
            "bingId": null,
            "type": "Email",
            "subType": null,
            "matches": [
            {
                "text": "contact@contoso.com",
                "offset": 70,
                "length": 19
            }]
          },
          {
            "name": "Contoso",
            "wikipediaId": "Contoso",
            "wikipediaLanguage": "en",
            "wikipediaUrl": "https://en.wikipedia.org/wiki/Contoso",
            "bingId": "349f014e-7a37-e619-0374-787ebb288113",
            "type": null,
            "subType": null,
            "matches": [
            {
                "text": "Contoso",
                "offset": 0,
                "length": 7
            }]
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Foutgevallen
Als de taal van het document niet ondersteund wordt, wordt een fout geretourneerd en er zijn geen entiteiten worden opgehaald.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
