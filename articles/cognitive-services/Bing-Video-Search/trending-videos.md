---
title: Het web zoeken naar trending video's met behulp van de Bing video's zoeken-API
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Bing video's zoeken-API op het web zoeken naar trending video's.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 486cf2e3bcf851f23011bb2fb8d91691d6190698
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61431917"
---
# <a name="get-trending-videos-with-the-bing-video-search-api"></a>Trending video's met de Bing webzoekopdrachten-API voor Video ophalen 

De Bing video's zoeken-API kunt u zoeken naar hedendaagse trending video's van op Internet en in verschillende categorieën. 

## <a name="get-request"></a>Aanvraag ophalen

Voor vandaag trending video's van de Bing video's zoeken-API, de volgende GET-aanvraag te verzenden:  
  
```cURL
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="market-support"></a>Market ondersteuning kennen

De volgende markten ondersteuning voor trending video's.  
 
-   en-AU (Engels, Australië)  
-   NL-CA (Engels, Canada)  
-   en-GB (Engels, Groot-Brittannië)  
-   NL-ID (Engels, Indonesië)  
-   NL-Internet Explorer (Engels, Ierland)  
-   NL-IN-(Engels, India)  
-   NL-NZ (Engels, Nieuw-Zeeland)  
-   NL-PH (Engels, Filipijnen)  
-   en-Lidmaatschappen (Engels, Singapore)  
-   en-US (Engels, Verenigde Staten)  
-   NL-WW (Engels, wereldwijd statistische-code)  
-   NL-ZA-(Engels, Zuid-Afrika)  
-   zh-CN (Chinees, China)

## <a name="example-json-response"></a>Voorbeeld van JSON-antwoord  

Het volgende voorbeeld ziet een API-reactie die trending video's die worden vermeld op categorie en subcategorie bevat. Het antwoord bevat ook een banner video's, die het meest populaire trending video's en kunnen afkomstig zijn uit een of meer categorieën.  

```json
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inzicht verkrijgen over video](video-insights.md)