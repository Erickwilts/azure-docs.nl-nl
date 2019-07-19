---
title: 'Quickstart: Tekstinhoud analyseren in Python - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Tekstinhoud analyseren op diverse soorten ongewenst materiaal met behulp van de Content Moderator SDK voor Python
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 01c153f2f8836b7d99de57af60b8623e54c6d6fe
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311931"
---
[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Snelstart: Tekstinhoud op ongewenst materiaal analyseren in Python

In dit artikel vindt u informatie en codevoorbeelden om aan de slag te gaan met de Content Moderator SDK voor Python. U leert hoe u kunt filteren op termen en tekstinhoud classificeren met het doel toezicht te houden op mogelijk ongewenst materiaal.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten
- Een abonnementssleutel voor Content Moderator. Volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en uw sleutel op te halen.
- [Python 2.7+ of 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)-hulpprogramma

## <a name="get-the-content-moderator-sdk"></a>De Content Moderator SDK ophalen

Installeer de Python-SDK voor Content Moderator door de opdrachtprompt te openen en de volgende opdracht uit te voeren:

```bash
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Modules importeren

Maak een nieuw Python-script met de naam _ContentModeratorQS.py_ en voeg de volgende code toe voor het importeren van de benodigde onderdelen van de SDK. De module Pretty Print is opgenomen voor een gemakkelijker te lezen JSON-antwoord.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=imports)]


## <a name="initialize-variables"></a>Variabelen initialiseren

Vervolgens voegt u variabelen toe voor uw abonnementssleutel en de eindpunt-URL voor Content Moderator. U moet de naam `CONTENT_MODERATOR_SUBSCRIPTION_KEY` toevoegen aan de omgevings variabelen en uw abonnements sleutel als waarde toevoegen. Voor de basis-URL van het `CONTENT_MODERATOR_ENDPOINT` eind punt moet u de omgevings variabelen toevoegen met uw regio-specifieke URL als `https://westus.api.cognitive.microsoft.com`waarde. Abonnementssleutels voor een gratis proefversie worden gegenereerd in de regio **westus**.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=authentication)]

Een teken reeks met tekst van meerdere regels van een bestand wordt gecontroleerd. Neem het bestand [content_moderator_text_moderation. txt](https://github.com/Azure-Samples/cognitive-services-content-moderator-samples/blob/master/documentation-samples/python/content_moderator_text_moderation.txt) op in de lokale hoofdmap en voeg de bestands naam toe aan de variabelen:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModerationFile)]

## <a name="query-the-moderator-service"></a>Query uitvoeren op de service Moderator

Maak een **ContentModeratorClient**-instantie met behulp van uw abonnementssleutel en de eindpunt-URL. 

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=client)]

Gebruik vervolgens uw client met het **TextModerationOperations** -exemplaar van de gebruiker om de toezicht-API met de `screen_text`functie aan te roepen. Zie de naslagdocumentatie **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python)** voor meer informatie over het aanroepen.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/content_moderator_quickstart.py?name=textModeration)]

## <a name="check-the-printed-response"></a>Controleer het afgedrukte antwoord

Voer het voor beeld uit en bevestig het antwoord. Het moet zijn voltooid en een **scherm** exemplaar heeft geretourneerd. Hieronder wordt een geslaagd resultaat afgedrukt:

De voorbeeldtekst die wordt gebruikt in deze snelstart resulteert in de volgende uitvoer:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudig Python-script ontwikkeld dat de Content Moderator-service gebruikt om relevante informatie over een bepaald tekstvoorbeeld te retourneren. Nu leert u meer over wat de verschillende vlaggen en classificaties betekenen, zodat u kunt beslissen welke gegevens u nodig hebt en hoe uw app ermee om moet gaan.

> [!div class="nextstepaction"]
> [Teksttoezichthandleiding](text-moderation-api.md)
