---
title: Bekijk de Azure Media Services Video Indexer-uitvoer geproduceerd door de v2-API
titlesuffix: Azure Media Services
description: In dit onderwerp wordt de Video Indexer-uitvoer geproduceerd door de v2-API gecontroleerd.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 205dc7d9e69788ea29a48ff342844a4b74e143bd
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799084"
---
# <a name="examine-the-video-indexer-output-produced-by-api"></a>Bekijk de Video Indexer-uitvoer geproduceerd door de API

Wanneer u aanroepen de **Video-Index ophalen** API en de status van het antwoord is OK, kunt u een gedetailleerde JSON-uitvoer als de inhoud van de reactie. De JSON-inhoud bevat details van de opgegeven inzichten in video's. De inzichten dimensies zoals bevatten: transscripts, optisch, gezichten, onderwerpen, blokken, enzovoort. De afmetingen zijn instanties van tijd bereiken die worden weergegeven wanneer elke dimensie wordt weergegeven in de video.  

U kunt ook visueel overzicht van de video-inzichten bekijken door te drukken de **afspelen** knop op de video op de [Video Indexer](https://www.videoindexer.ai/) website. Zie voor meer informatie, [weergeven en bewerken inzichten in video's](video-indexer-view-edit.md).

![Inzichten](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

In dit artikel onderzoekt de JSON-inhoud die wordt geretourneerd door de **Video-Index ophalen** API. 

> [!NOTE]
> Vervaldatum van de toegangstokens in Video Indexer is één uur.


## <a name="root-elements"></a>Basis-elementen

|Name|Description|
|---|---|
|accountId|Van de afspeellijst VI account-ID.|
|id|Van de afspeellijst-ID.|
|naam|De naam van de afspeellijst.|
|description|Beschrijving van de afspeellijst.|
|userName|De naam van de gebruiker die de afspeellijst gemaakt.|
|gemaakt|Aanmaaktijd van de afspeellijst.|
|privacyMode|Van de afspeellijst privacymodus (privé/openbaar).|
|state|Van de afspeellijst (geüploade, verwerking, verwerkt, is mislukt, de in quarantaine geplaatste).|
|isOwned|Geeft aan of de afspeellijst is gemaakt door de huidige gebruiker.|
|isEditable|Geeft aan of de huidige gebruiker is gemachtigd op de afspeellijst bewerken.|
|isBase|Geeft aan of de afspeellijst een base afspeellijst (video is) of een afspeellijst van andere video's (afgeleid) gemaakt.|
|durationInSeconds|De totale duur van de afspeellijst.|
|summarizedInsights|Bevat een [summarizedInsights](#summarizedinsights).
|video's|Een lijst met [video's](#videos) de afspeellijst maken.<br/>Als deze afspeellijst met samengesteld uit tijdsbereiken van andere video's (afgeleid), de video's in deze lijst bevat alleen gegevens uit de opgenomen tijdsbereik.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

In deze sectie bevat een overzicht van de inzichten.

|Kenmerk | Description|
|---|---|
|naam|De naam van de video. Bijvoorbeeld: Azure Monitor.|
|id|De ID van de video. Bijvoorbeeld: 63c6d532ff.|
|privacyMode|Uitsplitsing van uw kan hebben een van de volgende modi: **Persoonlijke**, **openbare**. **Openbare** -de video is zichtbaar voor iedereen in uw account en iedereen met een koppeling naar de video. **Persoonlijke** -de video is zichtbaar voor iedereen in uw account.|
|duur|Bevat een duur van de beschrijving van de tijd die een inzicht is opgetreden. Er is een duur in seconden.|
|thumbnailVideoId|De ID van de video van waaruit de miniatuur is gehaald.
|thumbnailId|Miniatuur van de video-id. Als u wilt de miniatuur van het werkelijke, aanroepen [Get-miniatuur](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) en deze doorgeven thumbnailVideoId en thumbnailId.|
|faces|Kan nul of meer gezichten bevatten. Zie voor meer informatie, [gezichten](#faces).|
|trefwoorden|Kan nul of meer trefwoorden bevatten. Zie voor meer informatie, [trefwoorden](#keywords).|
|stemmingen|Kan nul of meer sentimenten bevatten. Zie voor meer informatie, [sentimenten](#sentiments).|
|audioEffects| Kan nul of meer audioEffects bevatten. Zie voor meer informatie, [audioEffects](#audioEffects).|
|labels| Kan nul of meer labels bevatten. Zie voor meer informatie, [labels](#labels).|
|merken| Kan nul of meer merken bevatten. Zie voor meer informatie, [merken](#brands).|
|statistieken | Zie voor meer informatie, [statistieken](#statistics).|
|emoties| Kan nul of meer emoties bevatten. Zie voor meer informatie, [emoties](#emotions).|
|onderwerpen|Kan nul of meer onderwerpen bevatten. De [onderwerpen](#topics) dimensie.|

## <a name="videos"></a>video's

|Name|Description|
|---|---|
|accountId|Van de video VI account-ID.|
|id|Van de video-ID.|
|naam|De naam van de video.
|state|Status van de video (geüploade, verwerking, verwerkt, is mislukt, de in quarantaine geplaatste).|
|processingProgress|De voortgang tijdens de verwerking (bijvoorbeeld 20%).|
|failureCode|De foutcode als proces (bijvoorbeeld ' UnsupportedFileType') is mislukt.|
|failureMessage|Het foutbericht als kan niet worden verwerkt.|
|externalId|De video van de externe ID (indien opgegeven door de gebruiker).|
|externalUrl|Van de video externe url (indien opgegeven door de gebruiker).|
|metagegevens|De metagegevens van video's externe (indien opgegeven door de gebruiker).|
|isAdult|Geeft aan of er handmatig de video is bekeken en geïdentificeerd als een volwassene video.|
|inzichten|De insights-object. Zie voor meer informatie, [insights](#insights).|
|thumbnailId|Miniatuur van de video-id. Om op te halen van de werkelijke miniaturen aanroep [Get-miniatuur](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Thumbnail) en geeft die door de video-ID en thumbnailId.|
|publishedUrl|Een url naar de video te streamen.|
|publishedUrlProxy|Een url naar het streamen van video van (voor Apple-apparaten).|
|viewToken|Een korte levensduur hebben weergave-token voor het streamen van de video.|
|sourceLanguage|De brontaal van de video.|
|language|Huidige taal van de video (NAT).|
|indexingPreset|De definitie die wordt gebruikt om te indexeren van de video.|
|streamingPreset|De vooraf gedefinieerde instellingen gebruikt voor het publiceren van de video.|
|linguisticModelId|Het CRI model gebruikt de video te transcriberen.|
|statistieken | Zie voor meer informatie, [statistieken](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>inzichten

De inzichten zijn een set van dimensies (bijvoorbeeld regels tekst, gezichten, merken, enzovoort), waarbij elke dimensie een lijst met unieke elementen (bijvoorbeeld face1, face2, face3), en elk element heeft een eigen metagegevens en een lijst van de exemplaren (die zijn tijd van reeksen met aanvullende optionele metagegevens).

Een gezicht mogelijk een ID, een naam, een miniatuur, andere metagegevens en een lijst van de tijdelijke exemplaren (bijvoorbeeld: 00: 00:05 – 00:00:10, 00:01:00-00:02:30 en 00:41:21: 00:41:49.) Elke tijdelijke instantie kan aanvullende metagegevens hebben. Bijvoorbeeld, coördinaten van het gezichtsrechthoek (20,230,60,60).

|Versie|De codeversie|
|---|---|
|sourceLanguage|Van de video source-taal (ervan uitgaande dat één master taal). In de vorm van een [BCP-47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|language|De insights-taal (vertaald uit de source-taal). In de vorm van een [BCP-47](https://tools.ietf.org/html/bcp47) tekenreeks.|
|transcript|De [transcript](#transcript) dimensie.|
|OCR|De [OCR](#ocr) dimensie.|
|trefwoorden|De [trefwoorden](#keywords) dimensie.|
|blokken|Kan bevatten een of meer [blokken](#blocks)|
|faces|De [gezichten](#faces) dimensie.|
|labels|De [labels](#labels) dimensie.|
|foto 's|De [opnamen](#shots) dimensie.|
|merken|De [merken](#brands) dimensie.|
|audioEffects|De [audioEffects](#audioEffects) dimensie.|
|stemmingen|De [sentimenten](#sentiments) dimensie.|
|visualContentModeration|De [visualContentModeration](#visualcontentmoderation) dimensie.|
|textualContentModeration|De [textualContentModeration](#textualcontentmoderation) dimensie.|
|emoties| De [emoties](#emotions) dimensie.|
|onderwerpen|De [onderwerpen](#topics) dimensie.|

Voorbeeld:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualContentModeration": ...
}
```

#### <a name="blocks"></a>blokken

Kenmerk | Description
---|---
id|ID van het blok.|
instanties|Een lijst met tijdsbereiken van dit blok.|

#### <a name="transcript"></a>transcript

|Name|Description|
|---|---|
|id|De regel-ID.|
|tekst|Het transcript zelf.|
|language|De transcript-taal. Bedoeld ter ondersteuning van transcript waarbij elke regel een andere taal kan hebben.|
|instanties|Een lijst met bereiken van tijd waarop deze regel wordt weergegeven. Als de instantie transcript is, is er slechts 1 exemplaar.|

Voorbeeld:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>OCR

|Name|Description|
|---|---|
|id|De OCR-regel-ID.|
|tekst|De tekst.|
|vertrouwen|De opname-vertrouwen.|
|language|De OCR-taal.|
|instanties|Een lijst met tijdsbereik waar deze OCR weergegeven (de dezelfde OCR kan meerdere keren voorkomen).|
|Hoogte|De hoogte van de rechthoek OCR|
|top|De belangrijkste locatie in pixels|
|Links| De linker-locatie in pixels|
|Breedte|De breedte van de rechthoek OCR|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 675.971,
      "height": 35,
      "language": "en-US",
      "left": 31,
      "top": 97,
      "width": 400,      
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>trefwoorden

|Name|Description|
|---|---|
|id|Het sleutelwoord-ID.|
|tekst|De tekst trefwoord.|
|vertrouwen|Van het sleutelwoord erkenning vertrouwen.|
|language|Het sleutelwoord-taal (wanneer het wordt omgezet).|
|instanties|Een lijst met tijdsbereik waar dit sleutelwoord wordt weergegeven (een trefwoord kan meerdere keren voorkomen).|

```json
{
    id: 0,
    text: "technology",
    confidence: 1,
    language: "en-US",
    instances: [{
            adjustedStart: "0:05:15.782",
            adjustedEnd: "0:05:16.249",
            start: "0:05:15.782",
            end: "0:05:16.249"
    },
    {
            adjustedStart: "0:04:54.761",
            adjustedEnd: "0:04:55.228",
            start: "0:04:54.761",
            end: "0:04:55.228"
    }]
}
```

#### <a name="faces"></a>faces

|Name|Description|
|---|---|
|id|De face-ID.|
|naam|De naam van het gezicht. Kan het zijn ' Onbekende #, 0, een geïdentificeerde beroemdheden of een persoon met de klant.|
|vertrouwen|De face id vertrouwen.|
|description|Een beschrijving van de beroemdheden. |
|thumbnailId|De ID van de miniatuur van die gezicht.|
|knownPersonId|Als dit een bekende persoon, in de interne-ID is.|
|referenceId|Als het een Bing beroemdheden, het Bing-ID.|
|referenceType|Op dit moment alleen Bing.|
|titel|Als het een beroemdheden, de titel "(bijvoorbeeld van Microsoft CEO).|
|imageUrl|Als het een beroemdheden, de afbeeldings-url.|
|instanties|Dit zijn exemplaren van waar het gezicht wordt weergegeven in de opgegeven periode. Elk exemplaar heeft ook een thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>labels

|Name|Description|
|---|---|
|id|De label-ID.|
|naam|De labelnaam (bijvoorbeeld 'Computer', 'Tv-programma's ').|
|language|De taal met de label-naam (wanneer het wordt omgezet). BCP-47|
|instanties|Een lijst met tijdsbereik waar dit label wordt weergegeven (een label kan meerdere keren voorkomen). Elk exemplaar heeft een veld vertrouwen. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="scenes"></a>scenes

|Name|Description|
|---|---|
|id|De id van de scène.|
|instanties|Een lijst met tijdsbereiken van deze scène (een scène kan slechts 1 exemplaar hebben).|

```json
"scenes":[  
    {  
      "id":0,
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },
    {  
      "id":1,
      "instances":[  
          {  
            "start":"0:00:06.34",
            "end":"0:00:47.047",
            "duration":"0:00:40.707"
          }
      ]
    },

]
```

#### <a name="shots"></a>foto 's

|Name|Description|
|---|---|
|id|Id van de schermopname.|
|keyFrames|Een lijst met hoofdframes in beeld (elk heeft een ID en een lijst met instanties tijdsbereik). Elk exemplaar sleutelframes heeft een veld thumbnailId, waarin de miniatuur van het sleutelframes-id.|
|instanties|Een lijst met tijdsbereiken van deze schermopname (een opname kan slechts 1 exemplaar hebben).|

```json
"shots":[  
    {  
      "id":0,
      "keyFrames":[  
          {  
            "id":0,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:00.209",
                  "end":"0:00:00.251",
                  "duration":"0:00:00.042"
                }
            ]
          },
          {  
            "id":1,
            "instances":[  
                {  
                  "thumbnailId":"00000000-0000-0000-0000-000000000000",
                  "start":"0:00:04.755",
                  "end":"0:00:04.797",
                  "duration":"0:00:00.042"
                }
            ]
          }
      ],
      "instances":[  
          {  
            "start":"0:00:00",
            "end":"0:00:06.34",
            "duration":"0:00:06.34"
          }
      ]
    },

]
```

#### <a name="brands"></a>merken

Bedrijfs- en merknamen gedetecteerd in de spraak-naar-tekst transcriptie en/of Video OCR. Dit omvat geen visual herkenning van merken of logo detectie.

|Name|Description|
|---|---|
|id|De merk-ID.|
|naam|De naam van merken.|
|referenceId | Het achtervoegsel van het merk wikipedia-url. Bijvoorbeeld, "Target_Corporation" is het achtervoegsel van [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Het merk de Wikipedia-url, indien aanwezig. Bijvoorbeeld [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|De beschrijving van merken.|
|codes|Een lijst met vooraf gedefinieerde tags die gekoppeld aan deze merk zijn.|
|vertrouwen|De waarde van de betrouwbaarheid van de Video Indexer merk detector (0-1).|
|instanties|Een lijst met tijdsbereiken van deze merk. Elk exemplaar heeft een brandType, waarmee wordt aangegeven of deze huisstijl in het transcript of OCR weergegeven.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistieken

|Name|Description|
|---|---|
|CorrespondenceCount|Het aantal berichten in de video.|
|SpeakerWordCount|Het aantal woorden per spreker.|
|SpeakerNumberOfFragments|De hoeveelheid van de spreker in een video heeft fragmenten.|
|SpeakerLongestMonolog|Van de spreker langste monolog. Als de spreker silences binnen de monolog is opgenomen. Stilte aan het begin en het einde van de monolog wordt verwijderd.| 
|SpeakerTalkToListenRatio|De berekening is gebaseerd op de tijd die op van de spreker monolog (zonder de stilte in tussen) gedeeld door de totale tijd van de video. De tijd wordt afgerond op de derde decimaalteken.|

#### <a name="a-idaudioeffectsaudioeffects"></a><a id="audioEffects"/>audioEffects

|Name|Description|
|---|---|
|id|De id van de audio-effect.|
|type|Het type audio-effect (bijvoorbeeld klappen, spraak, stilte).|
|instanties|Een lijst met tijdsbereik waar deze audio-effect werden weergegeven.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>stemmingen

Sentimenten worden samengevoegd door hun sentimentType veld (positieve/neutraal/negatieve). Bijvoorbeeld, 0-0.1, 0.1 0.2.

|Name|Description|
|---|---|
|id|De id van de gevoelsscore.|
|averageScore |Het gemiddelde van alle scores van alle exemplaren van dat type sentiment - positieve/neutraal/negatieve|
|instanties|Een lijst met tijdsbereik waar deze sentiment werd weergegeven.|
|sentimentType |Het type kan worden 'Positief', 'Neutrale' of 'Negatieve'.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

Het blok visualContentModeration bevat tijdsbereik die Video Indexer hebben mogelijk inhoud voor volwassenen gevonden. Als visualContentModeration leeg is, is er geen inhoud voor volwassenen die is geïdentificeerd.

Video's die zijn gevonden voor volwassenen of ongepaste inhoud mogelijk beschikbaar voor alleen persoonlijke weergave. Gebruikers hebben de optie voor het verzenden van een aanvraag voor een menselijke beoordeling van de inhoud in dit geval kan het kenmerk IsAdult het resultaat van de menselijke beoordeling bevat.

|Name|Description|
|---|---|
|id|De id van de visual inhoudstoezicht.|
|adultScore|De erotiekscore (van content moderator).|
|racyScore|De erotiekscore (van inhoudstoezicht).|
|instanties|Een lijst met tijdsbereik waar deze visual inhoudstoezicht werd weergegeven.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualcontentmoderation"></a>textualContentModeration 

|Name|Description|
|---|---|
|id|De tekstuele inhoudstoezicht-ID.|
|bannedWordsCount |Het nummer van verboden woorden.|
|bannedWordsRatio |De verhouding van het totale aantal woorden.|

#### <a name="emotions"></a>emoties

Video Indexer identificeert willekeurige emoties op basis van de spraak- en audio-hints. De geïdentificeerde emoties kan zijn: zelf, verdriet, boosheid of angst.

|Name|Description|
|---|---|
|id|De emotion-ID.|
|type|De emotion-even die is geïdentificeerd op basis van de spraak- en audio hints. De emotie kan vreugde, verdriet, boosheid of angst zijn.|
|instanties|Een lijst met tijdsbereik waar deze emoties werd weergegeven.|

```json
"emotions": [{
    "id": 0,
    "type": "Fear",
    "instances": [{
      "adjustedStart": "0:00:39.47",
      "adjustedEnd": "0:00:45.56",
      "start": "0:00:39.47",
      "end": "0:00:45.56"
    },
    {
      "adjustedStart": "0:07:19.57",
      "adjustedEnd": "0:07:23.25",
      "start": "0:07:19.57",
      "end": "0:07:23.25"
    }]
  },
  {
    "id": 1,
    "type": "Anger",
    "instances": [{
      "adjustedStart": "0:03:55.99",
      "adjustedEnd": "0:04:05.06",
      "start": "0:03:55.99",
      "end": "0:04:05.06"
    },
    {
      "adjustedStart": "0:04:56.5",
      "adjustedEnd": "0:05:04.35",
      "start": "0:04:56.5",
      "end": "0:05:04.35"
    }]
  },
  {
    "id": 2,
    "type": "Joy",
    "instances": [{
      "adjustedStart": "0:12:23.68",
      "adjustedEnd": "0:12:34.76",
      "start": "0:12:23.68",
      "end": "0:12:34.76"
    },
    {
      "adjustedStart": "0:12:46.73",
      "adjustedEnd": "0:12:52.8",
      "start": "0:12:46.73",
      "end": "0:12:52.8"
    },
    {
      "adjustedStart": "0:30:11.29",
      "adjustedEnd": "0:30:16.43",
      "start": "0:30:11.29",
      "end": "0:30:16.43"
    },
    {
      "adjustedStart": "0:41:37.23",
      "adjustedEnd": "0:41:39.85",
      "start": "0:41:37.23",
      "end": "0:41:39.85"
    }]
  },
  {
    "id": 3,
    "type": "Sad",
    "instances": [{
      "adjustedStart": "0:13:38.67",
      "adjustedEnd": "0:13:41.3",
      "start": "0:13:38.67",
      "end": "0:13:41.3"
    },
    {
      "adjustedStart": "0:28:08.88",
      "adjustedEnd": "0:28:18.16",
      "start": "0:28:08.88",
      "end": "0:28:18.16"
    }]
  }
],
```

#### <a name="topics"></a>onderwerpen

Video Indexer maakt Deductie van de belangrijkste onderwerpen uit transcripties. Indien mogelijk, niveau van de 1e [IPTC](https://iptc.org/standards/media-topics/) taxonomie is opgenomen. 

|Name|Description|
|---|---|
|id|De onderwerp-ID.|
|naam|De onderwerpnaam, bijvoorbeeld: "Pharmaceuticals".|
|referenceId|Breadcrumbs zetten op basis van de hiërarchie van onderwerpen. Bijvoorbeeld: ' Status en wellbeing / medicijnen en gezondheidszorg / Pharmaceuticals '.|
|vertrouwen|De betrouwbaarheidsscore binnen het bereik [0,1]. Hoger is meer vertrouwen hebben.|
|language|De taal die wordt gebruikt in het onderwerp.|
|iptcName|De media IPTC code naam, als wordt gedetecteerd.|
|instanties |Video Indexer biedt op dit moment niet een onderwerp tijdsintervallen, indexeren, zodat de gehele video wordt gebruikt als het interval.|

```json
"topics": [{
    "id": 0,
    "name": "INTERNATIONAL RELATIONS",
    "referenceId": "POLITICS AND GOVERNMENT/FOREIGN POLICY/INTERNATIONAL RELATIONS",
    "referenceType": "VideoIndexer",
    "confidence": 1,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}, {
    "id": 1,
    "name": "Politics and Government",
    "referenceType": "VideoIndexer",
    "iptcName": "Politics",
    "confidence": 0.9041,
    "language": "en-US",
    "instances": [{
        "adjustedStart": "0:00:00",
        "adjustedEnd": "0:03:36.25",
        "start": "0:00:00",
        "end": "0:03:36.25"
    }]
}]
. . .
```

## <a name="next-steps"></a>Volgende stappen

[Video Indexer Developer Portal](https://api-portal.videoindexer.ai)

Zie voor meer informatie over het widgets insluiten in uw toepassing [widgets insluiten van Video Indexer in uw toepassingen](video-indexer-embed-widgets.md). 

