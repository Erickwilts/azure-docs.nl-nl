---
title: Word uitlijning - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Word uitlijningsgegevens ontvangen van de Translator Text-API.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-pawal
ms.custom: seodec18
ms.openlocfilehash: 5d48e2dd49eb3c2f449825f988618db3c6b04c0c
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66387290"
---
# <a name="how-to-receive-word-alignment-information"></a>Hoe word uitlijning van de informatie te ontvangen

## <a name="receiving-word-alignment-information"></a>Word uitlijning informatie ontvangt
Van de uitlijningsgegevens ontvangen, gebruikt u de methode vertalen en de optionele includeAlignment-parameter opnemen.

## <a name="alignment-information-format"></a>Uitlijning van de gegevens opmaken
Uitlijning wordt geretourneerd als een string-waarde van de volgende indeling voor elk woord in de bron. De gegevens van elk woord wordt gescheiden door een spatie, met inbegrip van niet-een door spaties gescheiden talen (scripts) zoals Chinees:

[[SourceTextStartIndex]:[SourceTextEndIndex]:[TgtTextStartIndex]:[TgtTextEndIndex]] *

Voorbeeld van de uitlijning van de tekenreeks: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

Met andere woorden, de puntkomma's gescheiden begin en einde index, het streepje worden gescheiden van de talen en ruimte worden gescheiden van de woorden. Een woord kan uitlijnen met nul, één of meerdere woorden in de andere taal en het is mogelijk dat de uitgelijnde woorden niet-aaneengesloten. Als er geen uitlijning informatie beschikbaar is, wordt de uitlijning van element niet leeg zijn. De methode in dat geval geen fout geretourneerd.

## <a name="restrictions"></a>Beperkingen
Uitlijning wordt alleen op dit moment voor een subset van de taal-paren geretourneerd:
* in het Engels in een andere taal;
* vanuit een andere taal in het Engels, met uitzondering van vereenvoudigd Chinees, traditioneel Chinees en Lets naar het Engels
* uit Japans Koreaans of Koreaans naar het Japanse u ontvangt geen uitlijning informatie als de zin een blik vertaling is. Voorbeeld van een voorgedefinieerde vertaling is 'Dit is een test', 'Fijn dat' en andere zinnen hoge frequentie.

## <a name="example"></a>Voorbeeld

Voorbeeld van JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
