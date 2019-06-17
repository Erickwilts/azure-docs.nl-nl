---
title: 'Quickstart: Gebruik cURL antwoord ophalen uit knowledge base - QnA Maker'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids helpt u bij het ophalen van een antwoord uit uw knowledge base met cURL.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: 817a19d5cabd7d20dc17154f29b4430e6b96c5fa
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072041"
---
# <a name="quickstart-get-answer-from-knowledge-base-using-curl"></a>Quickstart: Antwoord ophalen uit knowledge base met cURL

In deze snelstartgids op basis van een cURL begeleidt u bij het ophalen van een antwoord uit uw knowledge base.

## <a name="prerequisites"></a>Vereisten

* Latest [**cURL**](https://curl.haxx.se/).
* U moet over een [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md) en een [knowledge base met vragen en antwoorden](../Tutorials/create-publish-query-in-portal.md) beschikken.

## <a name="publish-to-get-endpoint"></a>Publiceren om eindpunten te verkrijgen

Wanneer u klaar bent voor het genereren van een antwoord op een vraag uit uw knowledge base, [publiceert](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) u uw knowledge base.

## <a name="use-production-endpoint-with-curl"></a>Productie-eindpunt gebruiken met cURL

Wanneer uw knowledge base is gepubliceerd, geeft de pagina **Publiceren** de instellingen van de HTTP-aanvraag weer voor het genereren van een antwoord. De **CURL** tabblad bevat de instellingen die vereist zijn voor het genereren van een antwoord van het opdrachtregelprogramma [CURL](https://www.getpostman.com).

[![Resultaten publiceren](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

Voor het genereren van een antwoord met CURL, voert u de volgende stappen uit:

1. Kopieer de tekst op het tabblad CURL. 
1. Open een opdrachtregel of de terminal en plak de tekst.
1. Bewerk de vraag om te worden relevant zijn voor uw knowledge base. Wees voorzichtig niet te verwijderen van de betreffende JSON rond de vraag.
1. Voer de opdracht. 
1. Het antwoord bevat de relevante informatie over het antwoord. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>Faseringseindpunt gebruiken met cURL

Als u wilt dat op een antwoord van de staging-eindpunt, gebruikt u de `isTest` hoofdtekst van de eigenschap.

```json
isTest:true
```

## <a name="next-steps"></a>Volgende stappen

De pagina publiceren bevat ook informatie om [een antwoord genereren](get-answer-from-kb-using-postman.md) met Postman. 

> [!div class="nextstepaction"]
> [Metagegevens gebruiken tijdens het genereren van een antwoord](../How-to/metadata-generateanswer-usage.md)
