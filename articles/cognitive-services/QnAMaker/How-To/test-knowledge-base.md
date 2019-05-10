---
title: Het testen van een knowledge base - QnA Maker
titlesuffix: Azure Cognitive Services
description: Testen van uw kennisdatabase QnA Maker is een belangrijk onderdeel van een iteratief proces voor het verbeteren van de nauwkeurigheid van de antwoorden die worden geretourneerd. De knowledge base via een verbeterde chat-interface waarmee ook kunt dat u wijzigingen aanbrengen, kunt u testen.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 2c596b49d5587b07fe75cefde72e897478dc3dc8
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472109"
---
# <a name="test-your-knowledge-base-interactively-in-qna-maker"></a>Test uw knowledge base interactief in QnA Maker

Testen van uw kennisdatabase QnA Maker is een belangrijk onderdeel van een iteratief proces voor het verbeteren van de nauwkeurigheid van de antwoorden die worden geretourneerd. De knowledge base via een verbeterde chat-interface waarmee ook kunt dat u wijzigingen aanbrengen, kunt u testen.

## <a name="test-answer-matching"></a>Test antwoorden vergelijken

1. Toegang tot uw knowledge base door het selecteren van de naam ervan op de **mijn knowledge bases** pagina.
1. Voor toegang tot het deelvenster van de dia-out Test, selecteer **Test** in het bovenste gedeelte van uw toepassing.
1. Voer een query in het tekstvak in en selecteer Enter.
1. De best overeenkomende antwoord in de kennisdatabase wordt geretourneerd als antwoord.

## <a name="clear-test-panel"></a>Deelvenster wissen testen

Schakel alle ingevoerde test-query's en de resultaten van de testconsole, selecteert u **beginnen** in de linkerbovenhoek van het paneel Test.

## <a name="close-test-panel"></a>Deelvenster sluiten testen

Als u wilt het Test-deelvenster sluiten, selecteert u de **Test** knop opnieuw. Tijdens de Test-deelvenster geopend is, kunt u de Knowledge Base-inhoud niet bewerken.

## <a name="inspect-score"></a>Controleren van score

U inspecteren details van de testresultaten in het deelvenster inspecteren.

1.  Met behulp van de Test dia-out deelvenster geopend, selecteert u **inspecteren** voor meer informatie over de respons.

    ![Antwoorden controleren](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Het deelvenster controle wordt weergegeven. Het deelvenster bevat de scoring-doel, evenals de geïdentificeerde entiteiten boven. Het deelvenster toont het resultaat van de geselecteerde utterance.

## <a name="correct-the-top-scoring-answer"></a>Corrigeer de bovenkant scoring-antwoord

Als de bovenkant scoring-antwoord onjuist is, selecteert u het juiste antwoord in de lijst en selecteer **opslaan en trainen**.

![Corrigeer de bovenkant scoring-antwoord](../media/qnamaker-how-to-test-kb/choose-answer.png)

## <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen

U kunt andere vormen van een vraag toevoegen aan een bepaald antwoord. Typ de alternatieve vindt u antwoorden op in het tekstvak in en klikt u op enter om te toe te voegen. Selecteer **opslaan en trainen** voor het opslaan van de updates.

![Alternatieve vragen toevoegen](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

## <a name="add-a-new-answer"></a>Een nieuw antwoord toevoegen

U kunt een nieuw antwoord toevoegen als een van de bestaande antwoorden die zijn afgestemd onjuist zijn of het antwoord niet in het knowledge base bestaat (geen goede overeenkomst gevonden in de KB). 

Aan de onderkant van de lijst met antwoorden in het tekstvak in te voeren van een nieuw antwoord gebruiken en druk op enter om deze te voegen. 

Selecteer **opslaan en trainen** om vast te leggen dit antwoord. Een nieuwe set met vraag-antwoord is nu is toegevoegd aan uw knowledge base. 

> [!NOTE]
> Alleen alle wijzigingen aan uw knowledge base opgeslagen wanneer u op de **opslaan en trainen** knop.

## <a name="test-the-published-knowledge-base"></a>De gepubliceerde knowledge base testen

In het deelvenster kunt u de gepubliceerde versie van het knowledge base testen. Nadat u de KB hebt gepubliceerd, selecteert u de **gepubliceerd KB** vak en verzenden van een query voor het ophalen van resultaten van de gepubliceerde KB.

![Testen op basis van een gepubliceerde KB](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase publiceren](./publish-knowledge-base.md)
