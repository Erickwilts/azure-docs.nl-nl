---
title: Tags toepassen van inhoud naar afbeeldingen - Computer Vision
titleSuffix: Azure Cognitive Services
description: Meer informatie over concepten met betrekking tot de taggingfunctie van de Computer Vision-API-installatiekopieën.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: aeb03566a650fe46286d77913e0d36dcbb19f436
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60759670"
---
# <a name="applying-content-tags-to-images"></a>Inhoudstags toepassen op afbeeldingen

Computer Vision retourneert tags op basis van duizenden herkenbare objecten, levende wezens, landschappen en acties. Wanneer tags ambigu of niet algemeen bekend zijn, geeft de API-reactie tips om de betekenis van de tag in de context van een bekende situatie te verduidelijken. Tags zijn niet ingedeeld als taxonomie en er bestaan geen overnamehiërarchieën. Een verzameling inhoudstags vormt de basis voor een 'beschrijving van de afbeelding, weergegeven als voor mensen leesbare taal opgemaakt in volledige zinnen. Houd er rekening mee dat Engels op dit moment de enige ondersteunde taal is voor de beschrijving van afbeeldingen.

Computer Vision-algoritmen uitvoer na het uploaden van een afbeelding of een afbeeldings-URL op te geven, tags op basis van de objecten, levende wezens en acties die zijn geïdentificeerd in de afbeelding. U kunt tagging niet alleen gebruiken voor het hoofdonderwerp, zoals een persoon op de voorgrond, maar ook voor de omgeving (binnen of buiten), meubels, gereedschap, planten, dieren, accessoires, gadgets enzovoort.

## <a name="image-tagging-example"></a>Voorbeeld van de installatiekopie taggen

Computer Vision geretourneerd bij het labelen van visuele kenmerken die zijn gedetecteerd in het voorbeeld ziet u de volgende JSON-antwoord.

![Een blauwe huis en de voorste yard](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [categoriseren van beelden](concept-categorizing-images.md) en [met een beschrijving van installatiekopieën](concept-describing-images.md).
