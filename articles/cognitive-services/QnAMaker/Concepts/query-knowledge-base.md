---
title: Query's uitvoeren op de Knowledge Base-QnA Maker
description: Een Knowledge Base moet worden gepubliceerd. Zodra de Knowledge Base is gepubliceerd, wordt deze in de runtime-Voorspellings eindpunt opgevraagd met behulp van de generateAnswer-API.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: cb777aa16fada50811cce1bbf49f28662c62b49b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843309"
---
# <a name="query-the-knowledge-base-for-answers"></a>De Knowledge Base doorzoeken op antwoorden

Een Knowledge Base moet worden gepubliceerd. Zodra de Knowledge Base is gepubliceerd, wordt deze in de runtime-Voorspellings eindpunt opgevraagd met behulp van de generateAnswer-API. De query bevat de vraag tekst en andere instellingen, zodat QnA Maker de best mogelijke overeenkomst met een antwoord kunt selecteren.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Hoe QnA Maker een gebruikers query verwerkt om het beste antwoord te selecteren

De getrainde en [gepubliceerde](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) QnA Maker Knowledge Base ontvangt een gebruikers query, vanuit een bot of andere client toepassing, op de [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage). Het volgende diagram illustreert het proces wanneer de query van de gebruiker wordt ontvangen.

![Het classificatie model proces voor een gebruikers query](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

### <a name="ranker-process"></a>Classificatie proces

Het proces wordt uitgelegd in de volgende tabel.

|Stap|Doel|
|--|--|
|1|De client toepassing verzendt de query van de gebruiker naar de [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage).|
|2|QnA Maker de gebruikers query met taal detectie, spellingen en woord afbreekers verwerkt.|
|3|Deze voor verwerking wordt gebruikt om de query van de gebruiker voor de beste Zoek resultaten te wijzigen.|
|4|Deze gewijzigde query wordt verzonden naar een Azure Cognitive Search-index, die het `top` aantal resultaten ontvangt. Als het juiste antwoord niet in deze resultaten voor komt, verhoogt u de waarde van `top` enigszins. Over het algemeen geldt een waarde van 10 voor `top` van 90% van de query's.|
|5|QnA Maker maakt gebruik van syntactische en semantische parametrisatie om de gelijkenis tussen de gebruikers query en de opgehaalde QnA-resultaten te bepalen.|
|6|Het door de machine geleerde rangorde model maakt gebruik van de verschillende functies uit stap 5 om de betrouwbaarheids scores en de nieuwe volg orde van de rang schikking te bepalen.|
|7|De nieuwe resultaten worden weer gegeven in de rang orde van de client toepassing.|
|||

De functies die worden gebruikt, zijn, maar zijn niet beperkt tot semantiek op woord niveau, urgentie op term niveau in een verzameling en diep opgedane semantische modellen om de gelijkenis en relevantie tussen twee teken reeksen te bepalen.

## <a name="http-request-and-response-with-endpoint"></a>HTTP-aanvraag en-antwoord met een eind punt
Wanneer u uw Knowledge Base publiceert, maakt de service een op REST gebaseerd HTTP-eind punt dat kan worden geïntegreerd in uw toepassing, meestal een chat-bot.

### <a name="the-user-query-request-to-generate-an-answer"></a>De aanvraag van de gebruikers query voor het genereren van een antwoord

Een gebruikers query is de vraag of de eind gebruiker de Knowledge Base vraagt, bijvoorbeeld `How do I add a collaborator to my app?`. De query is vaak in een natuurlijke taal indeling of enkele tref woorden die de vraag vertegenwoordigen, zoals `help with collaborators`. De query wordt naar uw Knowledge Base verzonden vanuit een HTTP-aanvraag in uw client toepassing.

```json
{
    "question": "How do I add a collaborator to my app?",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "QuestionType",
        "value": "Support"
    }],
    "userId": "sd53lsY="
}
```

U bepaalt het antwoord door eigenschappen zoals [scoreThreshold](./confidence-score.md#choose-a-score-threshold), [Top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers)en [strictFilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags)in te stellen.

Gebruik [gespreks context](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) met [multi-turn-functionaliteit](../how-to/multiturn-conversation.md) om de conversatie te laten verfijnen om de vragen en antwoorden op te lossen, om het juiste en laatste antwoord te vinden.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Het antwoord van een aanroep om een antwoord te genereren

Het HTTP-antwoord is het antwoord dat is opgehaald uit de Knowledge Base, op basis van de beste overeenkomst voor een bepaalde gebruikers query. Het antwoord bevat het antwoord en de Voorspellings Score. Als u om meer dan één hoofd antwoord met de eigenschap `top` wordt gevraagd, krijgt u meer dan een bovenste antwoord, elk met een score.

```json
{
    "answers": [
        {
            "questions": [
                "How do I add a collaborator to my app?",
                "What access control is provided for the app?",
                "How do I find user management and security?"
            ],
            "answer": "Use the Azure portal to add a collaborator using Access Control (IAM)",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "QuestionType",
                    "value": "Support"
                },
                {
                    "name": "ToolDependency",
                    "value": "Azure Portal"
                }
            ]
        }
    ]
}
```


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Betrouwbaarheidsscore](./confidence-score.md)
