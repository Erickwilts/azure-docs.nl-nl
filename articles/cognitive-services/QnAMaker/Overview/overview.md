---
title: Wat is QnA Maker?
titleSuffix: Azure Cognitive Services
description: QnA Maker is een cloud-API-service die aangepaste machine learning intelligence toepast op een gebruikersvraag in natuurlijke taal om daarop het beste antwoord te kunnen geven.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 04/05/2019
ms.author: tulasim
ms.openlocfilehash: 963769315302ba4e7d1600253b617c7cb0f02bc5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794249"
---
# <a name="what-is-qna-maker"></a>Wat is QnA Maker?

QnA Maker is een cloudgebaseerde API-service voor het maken van een vraag- en antwoordlaag over uw gegevens in gespreksstijl. 

Met QnA Maker kunt u een kennisbank (KB) maken van uw semi-gestructureerde inhoud, zoals URL's naar FAQ's (veelgestelde vragen), producthandleidingen, ondersteuningsdocumenten en aangepaste vragen en antwoorden. De QnA Maker-service beantwoordt de in natuurlijke taal gestelde vragen van uw gebruikers door deze te koppelen aan de best mogelijke antwoorden uit de QnA's in uw kennisbank.

In de eenvoudig te gebruiken [webportal](https://qnamaker.ai) kunt u uw service maken, beheren, trainen en publiceren zonder ontwikkelaarservaring. Wanneer de service is gepubliceerd op een eindpunt, kan een clienttoepassing zoals een chatbot de conversatie met een gebruiker beheren om vragen te krijgen en erop te reageren met antwoorden. 

![Overzicht](../media/qnamaker-overview-learnabout/overview.png)

## <a name="key-qna-maker-processes"></a>Belangrijke QnA Maker-processen

QnA Maker biedt twee belangrijke services voor uw gegevens:

* **Extractie**: gestructureerde vraag-antwoordgegevens worden geëxtraheerd uit gestructureerde en semi-gestructureerde [gegevensbronnen](../Concepts/data-sources-supported.md), zoals veelgestelde vragen en producthandleidingen. Deze extractie kan worden gedaan bij het [maken](https://aka.ms/qnamaker-docs-createkb) van de KB of later, als onderdeel van het bewerkingsproces.

* **Matching**: nadat uw kennisbank [is getraind en getest](https://aka.ms/qnamaker-docs-trainkb), [publiceert](https://aka.ms/qnamaker-docs-publishkb) u deze. Dit maakt een eindpunt mogelijk voor uw QnA Maker-knowledge base, die u vervolgens kunt gebruiken in uw bot of clientapp. Dit eindpunt accepteert een gebruikersvraag en reageert met het beste antwoord in de knowledge base, samen met een betrouwbaarheidsscore voor de match.

```JSON
{
    "answers": [
        {
            "questions": [
                "How do I share a knowledge base with other?"
            ],
            "answer": "Sharing works at the level of a QnA Maker service, i.e. all knowledge bases in the services will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base)how to collaborate on a knowledge base.",
            "score": 70.95,
            "id": 4,
            "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
            "metadata": []
        }
    ]
}

```

## <a name="qna-maker-architecture"></a>QnA Maker-architectuur

De architectuur van QnA Maker bestaat uit de volgende twee onderdelen:

1. **QnA Maker-beheerservices**: het beheer voor een QnA Maker-knowledge base dat alle fasen omvat, van het maken tot en met het bijwerken, trainen en publiceren. Deze activiteiten kunnen worden uitgevoerd via de [portal](https://qnamaker.ai) of de [beheer-API's](https://go.microsoft.com/fwlink/?linkid=2092179). 

2. **QnA Maker-gegevens en -runtime**: dit wordt geïmplementeerd in uw Azure-abonnement in de door u opgegeven regio. Uw KB-inhoud wordt opgeslagen in [Azure Search](https://azure.microsoft.com/services/search/) en het eindpunt wordt geïmplementeerd als een [app-service](https://azure.microsoft.com/services/app-service/). U kunt er ook voor kiezen om een ​​[Application Insights](https://azure.microsoft.com/services/application-insights/)-bron te implementeren voor analyse.

![Architectuur](../media/qnamaker-overview-learnabout/architecture.png)


## <a name="service-highlights"></a>Belangrijke functies van de service

- Een complete **zonder code** ervaring aan [maken van een bot](../Quickstarts/create-publish-knowledge-base.md#create-a-bot) uit een knowledge base.
- **Er is geen netwerkbeperking voor voorspellingen**. Betaal voor het hosten van de service en niet voor het aantal transacties. Bekijk de [prijzenpagina](https://aka.ms/qnamaker-docs-pricing) voor meer informatie.
- **Schalen naar behoefte**. Kies de juiste SKU’s van de afzonderlijke componenten die bij uw scenario passen. Zie hoe u [capaciteit kiest](https://aka.ms/qnamaker-docs-capacity) voor uw QnA Maker-service.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een QnA Maker-service maken](../how-to/set-up-qnamaker-service-azure.md)
