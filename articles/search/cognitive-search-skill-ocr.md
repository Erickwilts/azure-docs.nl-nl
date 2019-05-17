---
title: OCR cognitief zoeken vaardigheid - Azure Search
description: Haal de tekst uit bestanden met behulp van optische tekenherkenning (OCR) in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 6d9b68bda2a6cff533286d9ee944abf1c92cc2bf
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523242"
---
# <a name="ocr-cognitive-skill"></a>OCR cognitieve vaardigheden

Optische tekenherkenning (OCR) vaardigheid herkent gedrukte en handgeschreven tekst in afbeeldingen. Deze vaardigheid maakt gebruik van de machine learning-modellen die worden geleverd door [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) in Cognitive Services. De **OCR** vaardigheid worden toegewezen aan de volgende functionaliteit:

+ Wanneer textExtractionAlgorithm is ingesteld op 'handgeschreven', de ["RecognizeText"](../cognitive-services/computer-vision/quickstarts-sdk/csharp-hand-text-sdk.md) functionaliteit wordt gebruikt.
+ Wanneer textExtractionAlgorithm is ingesteld op 'afdrukken', de ["OCR"](../cognitive-services/computer-vision/concept-extracting-text-ocr.md) functionaliteit wordt gebruikt voor andere talen dan Engels. Voor Engels, de nieuwe ['Herkennen tekst'](../cognitive-services/computer-vision/concept-recognizing-text.md) functionaliteit voor gedrukte tekst wordt gebruikt.

De **OCR** vaardigheid haalt tekst op uit de afbeeldingsbestanden. Ondersteunde bestandsindelingen zijn onder andere:

+ .JPEG
+ .JPG
+ .PNG
+ .BMP
+ .GIF
+ .TIFF

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Description |
|--------------------|-------------|
| detectOrientation | Hiermee kunt automatische detectie van de afdrukstand. <br/> Geldige waarden: true / false.|
|defaultLanguageCode | <p>  De taalcode van de invoertekst. Enkele ondersteunde talen: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>CS (Tsjechië) <br/>da (Denemarken) <br/>NL (Nederlands) <br/>NL-(Engels) <br/>Fi (Fins)  <br/>FR (Frans) <br/>  de (Duits) <br/>EL (Grieks) <br/> hu (Hongaars) <br/> Deze (Italiaans) <br/>  Japan (Japans) <br/> Ko (Koreaans) <br/> NB (Noors) <br/>   PL (Pools) <br/> PT (Portugees) <br/>  RU (Russisch) <br/>  ES (Spaans) <br/>  SV (Zweeds) <br/>  tr (Turkish) <br/> ar (Arabisch) <br/> ro (Roemeens) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Slowakije). <br/>  UNK (onbekend) <br/><br/> Als de taal niet opgegeven of null zijn is, wordt de taal worden ingesteld op Engels. Als de taal is expliciet ingesteld op 'unk', worden de taal automatisch gedetecteerd. </p> |
| textExtractionAlgorithm | "afgedrukt" of 'handgeschreven'. De "handgeschreven" tekst herkenning van OCR-algoritme is momenteel in preview en alleen ondersteund in het Engels. |
|lineEnding | De waarde die moet worden gebruikt tussen elke regel gedetecteerd. Mogelijke waarden: 'Space','CarriageReturn','LineFeed'.  De standaardwaarde is 'Ruimte' |

## <a name="skill-inputs"></a>Kwalificatie invoer

| Naam invoeren      | Description                                          |
|---------------|------------------------------------------------------|
| image         | Complexe Type. Momenteel wordt alleen werkt met "/ document/normalized_images"-veld, die worden geproduceerd door de indexeerfunctie Azure Blob als ```imageAction``` is ingesteld op een andere waarde dan ```none```. Zie de [voorbeeld](#sample-output) voor meer informatie.|


## <a name="skill-outputs"></a>Kwalificatie uitvoer
| Naam van de uitvoer     | Description                   |
|---------------|-------------------------------|
| tekst          | Tekst zonder opmaak is geëxtraheerd uit de afbeelding.   |
| layoutText    | Complexe type met de beschrijving de geëxtraheerde tekst en de locatie waar de tekst is gevonden.|


## <a name="sample-definition"></a>Van voorbeelddefinitie

```json
{
  "skills": [
    {
      "description": "Extracts text (plain and structured) from image.",
      "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
      "context": "/document/normalized_images/*",
      "defaultLanguageCode": null,
      "detectOrientation": true,
      "inputs": [
        {
          "name": "image",
          "source": "/document/normalized_images/*"
        }
      ],
      "outputs": [
        {
          "name": "text",
          "targetName": "myText"
        },
        {
          "name": "layoutText",
          "targetName": "myLayoutText"
        }
      ]
    }
  ]
}
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Voorbeeld van de tekst en layoutText uitvoer

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Voorbeeld: Samenvoegen van ingesloten afbeeldingen met de inhoud van het document geëxtraheerde tekst.

Een veelvoorkomende use-case voor tekst samenvoegen is de mogelijkheid om samen te voegen van de tekstweergave van installatiekopieën (tekst uit een OCR-vaardigheden of het bijschrift van een installatiekopie) in het veld inhoud van een document.

De vaardigheden van het volgende voorbeeld maakt een *merged_text* veld. Dit veld bevat de tekstinhoud van het document en de tekst OCRed van elk van de afbeeldingen in dit document is ingesloten.

#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
      "description": "Extract text (plain and structured) from image.",
      "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
      "context": "/document/normalized_images/*",
      "defaultLanguageCode": "en",
      "detectOrientation": true,
      "inputs": [
        {
          "name": "image",
          "source": "/document/normalized_images/*"
        }
      ],
      "outputs": [
        {
          "name": "text"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset"
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
Het bovenstaande voorbeeld van de vaardigheden wordt ervan uitgegaan dat een veld genormaliseerd installatiekopieën bestaat. Voor het genereren van dit veld, stel de *imageAction* configuratie in de definitie van de indexeerfunctie *generateNormalizedImages* zoals hieronder wordt weergegeven:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters": {
    "configuration": {
      "dataToExtract":"contentAndMetadata",
      "imageAction":"generateNormalizedImages"
    }
  }
}
```

## <a name="see-also"></a>Zie ook
+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [TextMerger vaardigheid](cognitive-search-skill-textmerger.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Indexeerfunctie (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
