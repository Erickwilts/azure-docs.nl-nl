---
title: Gemiddeld tekst met behulp van de tekst toezicht-API - Content Moderator
titlesuffix: Azure Cognitive Services
description: Maak een proefrit met beheer van tekst met behulp van de Text-API voor beheer in de online-console.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 04/30/2019
ms.openlocfilehash: edf4a3e9d9e9b51ac44f839cababa9d14bc0d17a
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228060"
---
# <a name="moderate-text-from-the-api-console"></a>Gemiddeld tekst van de API-console

Gebruik de [tekst toezicht-API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) in Azure Content Moderator voor het scannen van de tekstinhoud van uw. De bewerking scant uw taalgebruik en vergelijkt de inhoud op basis van aangepaste en gedeelde zwarte lijsten.

## <a name="get-your-api-key"></a>Haal uw API-sleutel

Voordat u de API in de online-console uitproberen kan, moet u de abonnementssleutel van uw. Dit bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie [Overzicht](overview.md) voor meer informatie.

## <a name="navigate-to-the-api-reference"></a>Navigeer naar de API-verwijzing

Ga naar de [tekst Afbeeldingstoezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  De **tekst - scherm** pagina wordt geopend.

## <a name="open-the-api-console"></a>De API-console openen

Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Tekst - scherm pagina regio selecteren](images/test-drive-region.png)

  De **tekst - scherm** API-console wordt geopend.

## <a name="select-the-inputs"></a>Selecteer de invoer

### <a name="parameters"></a>Parameters

Selecteer de queryparameters die u wilt gebruiken in uw tekst-scherm. In dit voorbeeld gebruikt u de standaardwaarde voor **taal**. U kunt ook het leeg laten omdat de bewerking automatisch de taal die waarschijnlijk als onderdeel van de uitvoering ervan detecteert.

> [!NOTE]
> Voor de **taal** parameter toewijzen `eng` of laat het veld leeg om te zien van de computer-ondersteund **classificatie** antwoord (preview-functie). **Deze functie ondersteunt alleen Engels**.
>
> Voor **grof taalgebruik voorwaarden** detectie, gebruik de [ISO 639-3-code](http://www-01.sil.org/iso639-3/codes.asp) van de ondersteunde talen die worden vermeld in dit artikel of laat het veld leeg.

Voor **AutoCorrectie**, **PII**, en **classificeren (preview)**, selecteer **waar**. Laat de **ListId** veld leeg.

  ![Tekst - scherm console queryparameters](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>Inhoudstype

Voor **Content-Type**, selecteer het type inhoud dat u wilt dat op het scherm. In dit voorbeeld gebruikt u de standaard **text/plain** type inhoud. In de **Ocp-Apim-Subscription-Key** voert u de abonnementssleutel van uw.

### <a name="sample-text-to-scan"></a>Voorbeeldtekst om te scannen

In de **aanvraagtekst** Voer wat tekst. Het volgende voorbeeld ziet een opzettelijke typefout in de tekst.

> [!NOTE]
> Ongeldige sociaal-fiscaal nummer in het volgende voorbeeldtekst is opzettelijk. Het doel is het overbrengen van de Voorbeeldinvoer en indeling van uitvoer.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="analyze-the-response"></a>Het antwoord analyseren

Het volgende antwoord bevat de verschillende inzichten van de API. Het bevat potentieel grof taalgebruik, PII-classificatie (preview) en de versie automatisch worden gecorrigeerd.

> [!NOTE]
> De functie 'Classificatie' geautomatiseerd is in preview en ondersteunt alleen Engels.

```json
{"OriginalText":"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
"NormalizedText":"Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation":null,
"PII":{  
  "Email":[  
    {  
      "Detected":"abcdef@abcd.com",
      "SubType":"Regular",
      "Text":"abcdef@abcd.com",
      "Index":32
    }
  ],
  "IPA":[  
    {  
      "SubType":"IPV4",
      "Text":"255.255.255.255",
      "Index":72
    }
  ],
  "Phone":[  
    {  
      "CountryCode":"US",
      "Text":"6657789887",
      "Index":56
    },
    {  
      "CountryCode":"US",
      "Text":"870 608 4000",
      "Index":211
    },
    {  
      "CountryCode":"UK",
      "Text":"+44 870 608 4000",
      "Index":207
    },
    {  
      "CountryCode":"UK",
      "Text":"0344 800 2400",
      "Index":227
    },
    {  
      "CountryCode":"UK",
      "Text":"0800 820 3300",
      "Index":244
    }
  ],
  "Address":[  
    {  
      "Text":"1 Microsoft Way, Redmond, WA 98052",
      "Index":89
    }
  ],
  "SSN":[  
    {  
      "Text":"999999999",
      "Index":56
    },
    {  
      "Text":"999-99-9999",
      "Index":266
    }
  ]
},
"Classification":{  
  "ReviewRecommended":true,
  "Category1":{  
    "Score":1.5113095059859916E-06
  },
  "Category2":{  
    "Score":0.12747249007225037
  },
  "Category3":{  
    "Score":0.98799997568130493
  }
},
"Language":"eng",
"Terms":[  
  {  
    "Index":21,
    "OriginalIndex":21,
    "ListId":0,
    "Term":"crap"
  }
],
"Status":{  
  "Code":3000,
  "Description":"OK",
  "Exception":null
},
"TrackingId":"2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

Voor een gedetailleerde beschrijving van alle secties in het JSON-antwoord, raadpleegt u de [teksttoezicht](text-moderation-api.md) conceptuele handleiding.

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [tekst toezicht .NET snelstartgids](text-moderation-quickstart-dotnet.md) om te integreren in uw toepassing.
