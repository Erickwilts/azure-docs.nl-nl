---
title: 'Snelstart: cURL en REST gebruiken om Knowledge Base te beheren - QnA Maker'
description: In deze quickstart ziet u hoe uw Knowledge Base maakt, publiceert en er query's op uitvoert met de REST API's.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 04/13/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: quickstart
ms.openlocfilehash: 11a7bd8655d1b5606c3b53ed78e796bc42f85b2e
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2020
ms.locfileid: "91777425"
---
# <a name="quickstart-use-curl-and-rest-to-manage-knowledge-base"></a>Snelstart: cURL en REST gebruiken om Knowledge Base te beheren

In deze quickstart wordt beschreven hoe u uw Knowledge Base maakt, publiceert en er query's op uitvoert. Met QnA Maker worden automatisch vragen en antwoorden opgehaald uit semi-gestructureerde inhoud, zoals veelgestelde vragen, vanuit [gegevensbronnen](../Concepts/knowledge-base.md). Het model voor de knowledge base wordt gedefinieerd in de JSON die in de hoofdtekst van de API-aanvraag wordt verzonden.

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Vereisten

* De huidige versie van [cURL](https://curl.haxx.se/). In deze quickstarts worden verschillende opdrachtregelopties gebruikt, die worden vermeld in de [cURL-documentatie](https://curl.haxx.se/docs/manpage.html).
* U moet een [QnA Maker-resource](../How-To/set-up-qnamaker-service-azure.md) hebben om de sleutel en resourcenaam te gebruiken. U hebt de **resourcenaam** ingevoerd tijdens het maken van de resource, daarna is de sleutel voor u gemaakt. De resourcenaam is gebruikt als het subdomein voor uw eindpunt. Als u uw sleutel en resourcenaam wilt ophalen, selecteert u **Quickstart** voor uw resource in Azure Portal. De resourcenaam is het eerste subdomein van de eindpunt-URL:

    `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0`

> [!CAUTION]
> In de volgende BASH-voorbeelden wordt het regelvoortzettingsteken `\` gebruikt. Als in uw console of terminal een ander regelvoortzettingsteken wordt gebruikt, gebruikt u dit teken.

## <a name="create-a-knowledge-base"></a>Een kennisdatabase maken

Als u een Knowledge Base wilt maken met de REST API's en cURL, hebt u de volgende informatie nodig:

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|QnA Maker-resourcesleutel|Param `-h` voor de header `Ocp-Apim-Subscription-Key`|Verifiëren bij QnA Maker-service|
|JSON met een beschrijving van de Knowledge Base|Param `-d`|[Voorbeelden](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create#examples) van JSON|
|Grootte van de JSON in bytes|Param `-h` voor de header `Content-Size`||

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam, resourcesleutel, JSON-waarden en JSON-grootte.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/create \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:107" \
-d '{ name: "QnA Maker FAQ",urls: [ "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs"]}'
```

De cURL-reactie van QnA Maker omvat de `operationId`, die vereist is [om de status van de bewerking op te halen](#get-status-of-operation).

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2020-02-27T04:11:22Z",
  "lastActionTimestamp": "2020-02-27T04:11:22Z",
  "userId": "9596077b3e0441eb93d5080d6a15c64b",
  "operationId": "95a4f700-9899-4c98-bda8-5449af9faef8"
}
```

## <a name="get-status-of-operation"></a>De status van de bewerking ophalen

Wanneer u een Knowledge Base maakt, omvat de reactie informatie om de status te bepalen, omdat de bewerking asynchroon is.

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|Bewerkings-id|URL-route|`/operations/REPLACE-WITH-YOUR-OPERATION-ID`|
|QnA Maker-resourcesleutel|Param `-h` voor de header `Ocp-Apim-Subscription-Key`|Verifiëren bij QnA Maker-service|

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam, resourcesleutel en bewerkings-id.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/operations/REPLACE-WITH-YOUR-OPERATION-ID \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

De cURL-reactie omvat de status. Als de bewerkingsstatus is geslaagd, dan bevat `resourceLocation` de Knowledge Base-id.

```json
{
   "operationState": "Succeeded",
   "createdTimestamp": "2020-02-27T04:54:07Z",
   "lastActionTimestamp": "2020-02-27T04:54:19Z",
   "resourceLocation": "/knowledgebases/fe3971b7-cfaa-41fa-8d9f-6ceb673eb865",
   "userId": "f596077b3e0441eb93d5080d6a15c64b",
   "operationId": "f293f218-d080-48f0-a766-47993e9b26a8"
}
```

## <a name="publish-knowledge-base"></a>Knowledge base publiceren

Voordat u een query uitvoert op de Knowledge Base, moet u het volgende doen:
* Knowledge base publiceren
* De runtime-eindpuntsleutel ophalen

Met deze taak wordt de Knowledge Base gepubliceerd. De runtime-eindpuntsleutel ophalen is een [afzonderlijke taak](#get-published-knowledge-bases-runtime-endpoint-key).

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|QnA Maker-resourcesleutel|Param `-h` voor de header `Ocp-Apim-Subscription-Key`|Verifiëren bij QnA Maker-service|
|Knowledge Base-id|URL-route|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam, resourcesleutel en Knowledge Base-id.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-v \
-X POST \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY" \
--data-raw ''
```

De antwoordstatus is 204 zonder resultaten. Gebruik de opdrachtregelparameter `-v` om de uitgebreide uitvoer van de cURL-opdracht te bekijken. Deze omvat ook de HTTP-status.

## <a name="get-published-knowledge-bases-runtime-endpoint-key"></a>De runtime-eindpuntsleutel van de gepubliceerde Knowledge Base ophalen

Voordat u een query uitvoert op de Knowledge Base, moet u het volgende doen:
* Knowledge base publiceren
* De runtime-eindpuntsleutel ophalen

Met deze taak wordt de runtime-eindpuntsleutel opgehaald. Het publiceren van de Knowledge Base is een [afzonderlijke taak](#publish-knowledge-base).

De runtime-eindpuntsleutel is dezelfde sleutel voor alle Knowledge Base die gebruikmaken van de QnA Maker-resource.

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|QnA Maker-resourcesleutel|Param `-h` voor de header `Ocp-Apim-Subscription-Key`|Verifiëren bij QnA Maker-service|

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam en resourcesleutel.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/endpointkeys \
-X GET \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```


De cURL-reactie omvat de runtime-eindpuntsleutels. Gebruik één van de sleutels wanneer u een query uitvoert om een antwoord van de Knowledge Base te krijgen.

```json
{
  "primaryEndpointKey": "93e88a14-694a-44d5-883b-184a68aa8530",
  "secondaryEndpointKey": "92c98c16-ca31-4294-8626-6c57454a5063",
  "installedVersion": "4.0.5",
  "lastStableVersion": "4.0.6"
}
```

## <a name="query-for-answer-from-published-knowledge-base"></a>Query uitvoeren voor een antwoord van de gepubliceerde Knowledge Base

Een antwoord krijgen van de Knowledge Base wordt vanuit een afzonderlijke runtime gedaan dan het beheer van de Knowledge Base. Omdat het een afzonderlijke runtime is, moet u verifiëren met een runtime-sleutel.

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|QnA Maker-runtimesleutel|Param `-h` voor de header `Authorization`|De sleutel maakt deel uit van een tekenreeks die het woord `Endpointkey ` bevat. Verifiëren bij QnA Maker-service|
|Knowledge Base-id|URL-route|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|
|JSON die query beschrijft|Param `-d`|[Parameters voor aanvraagbody](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#request-body) en [voorbeelden](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer#examples) van JSON|
|Grootte van de JSON in bytes|Param `-h` voor de header `Content-Size`||

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam, resourcesleutel en Knowledge Base-id.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.azurewebsites.net/qnamaker/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID/generateAnswer \
-X POST \
-H "Authorization: EndpointKey REPLACE-WITH-YOUR-RUNTIME-KEY" \
-H "Content-Type:application/json" \
-H "Content-Size:159" \
-d '{"question": "How are QnA Maker and LUIS used together?","top": 6,"isTest": true,  "scoreThreshold": 20, "strictFilters": [], "userId": "sd53lsY="}'
```

Een geslaagde reactie omvat het beste antwoord samen met andere informatie die een clienttoepassing, zoals een chatbot, moet weergeven om de gebruiker te beantwoorden.

## <a name="delete-knowledge-base"></a>Een Knowledge Base verwijderen

Wanneer u klaar bent met de Knowledge Base, kunt u die verwijderen.

|Informatie|cURL-configuratie|Doel|
|--|--|--|
|QnA Maker-resourcenaam|URL|gebruikt voor het maken van een URL|
|QnA Maker-resourcesleutel|Param `-h` voor de header `Ocp-Apim-Subscription-Key`|Verifiëren bij QnA Maker-service|
|Knowledge Base-id|URL-route|`/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID`|

De cURL-opdracht wordt uitgevoerd vanuit een BASH-shell. Bewerk deze opdracht met uw eigen resourcenaam, resourcesleutel en Knowledge Base-id.

```bash
curl https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.com/qnamaker/v4.0/knowledgebases/REPLACE-WITH-YOUR-KNOWLEDGE-BASE-ID \
-X DELETE \
-v \
-H "Ocp-Apim-Subscription-Key: REPLACE-WITH-YOUR-RESOURCE-KEY"
```

De antwoordstatus is 204 zonder resultaten. Gebruik de opdrachtregelparameter `-v` om de uitgebreide uitvoer van de cURL-opdracht te bekijken. Deze omvat ook de HTTP-status.

## <a name="additional-resources"></a>Aanvullende bronnen

* Referentiedocumentatie voor [creatie](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* Referentiedocumentatie voor [runtime](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/)
* [Voorbeelden van BASH-scripts met cURL](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/curl/QnAMaker)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST-API voor QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
