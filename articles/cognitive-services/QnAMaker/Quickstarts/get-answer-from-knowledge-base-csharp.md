---
title: 'Quickstart: Antwoord uit knowledge base ophalen - REST, C# - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Deze C# REST-quickstart begeleidt u bij het programmatisch ophalen van een antwoord uit een knowledge base.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 020403268665633bf3d71cf43258ced87b4f837d
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249719"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-c"></a>Antwoorden vinden op vragen met behulp van een knowledge base met C#

In deze quickstart wordt beschreven hoe u programmatisch een antwoord uit een gepubliceerde QnA Maker-knowledge base kunt ophalen. De knowledge base bevat vragen en antwoorden uit [gegevensbronnen](../Concepts/data-sources-supported.md) zoals veelgestelde vragen over. De [vraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) wordt verzonden naar de QnA Maker-service. De [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) bevat het antwoord boven voorspeld. 


## <a name="prerequisites"></a>Vereisten

* De meest recente [**Visual Studio Community edition**](https://www.visualstudio.com/downloads/).
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer **Sleutels** onder **Resourcebeheer** in het Azure-dashboard voor de QnA Maker-resource om uw sleutel op te halen. 
* Pagina Instellingen voor **Publiceren**. Als u nog geen knowledge base hebt gepubliceerd, moet u een lege knowledge base maken, een knowledge base importeren op de pagina **Instellingen** en de knowledge base publiceren. U kunt [deze eenvoudige knowledge base](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv) downloaden en gebruiken. 

    De pagina Instellingen voor Publiceren bevat de waarde voor POST-route, Host en EndpointKey. 

    ![Instellingen voor Publiceren](../media/qnamaker-quickstart-get-answer/publish-settings.png)

De code voor deze quickstart bevindt zich in de opslagplaats [https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-knowledge-base-project"></a>Een project met knowledge base maken

1. Open Visual Studio 2019 Community-editie.
1. Maak een nieuw project-consoletoepassing (.NET Core) en de naam van het project QnaMakerQuickstart. Accepteer de standaardwaarden voor de overige instellingen.

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

Boven in het bestand Program.cs vervangt u de enige using-instructie door de volgende regels. U voegt op deze manier de benodigde afhankelijkheden toe aan het project:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=1-3 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>De vereiste constanten toevoegen

Boven aan de klasse `Program` in de `Main` voegt u de vereiste constanten toe voor toegang tot QnA Maker. Deze waarden vindt u op de pagina **Publiceren** nadat u de knowledge base hebt gepubliceerd. 

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=14-30 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Een POST-aanvraag toevoegen om vragen te verzenden en antwoorden op te halen

Met de volgende code wordt een HTTPS-aanvraag naar de QnA Maker-API verzonden om de vraag naar de knowledge base te verzenden en het antwoord te ontvangen:

[!code-csharp[Add a POST request to send question to knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=32-57 "Add a POST request to send question to knowledge base")]

De waarde van de header van `Authorization` bevat de tekenreeks `EndpointKey`. 

Meer informatie over de [aanvraag](../how-to/metadata-generateanswer-usage.md#generateanswer-request) en [antwoord](../how-to/metadata-generateanswer-usage.md#generateanswer-response). 

## <a name="build-and-run-the-program"></a>Het programma bouwen en uitvoeren

Bouw het programma en voer het uit vanuit Visual Studio. De aanvraag wordt automatisch verzonden naar de QnA Maker-API, waarna het antwoord wordt weergegeven in het consolevenster.

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://go.microsoft.com/fwlink/?linkid=2092179)
