---
title: Intenties toevoegen-LUIS
titleSuffix: Azure Cognitive Services
description: Intents toevoegen aan uw LUIS-app voor het identificeren van groepen van vragen of de opdrachten die het hetzelfde doel hebben.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/29/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 84799c2866e7b887a7b509280b073814e7653638
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638203"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Intenties toevoegen om te bepalen wat de gebruikers intentie van uitingen zijn

Voeg [intents](luis-concept-intent.md) naar uw LUIS-app en het identificeren van vragen of opdrachten die hetzelfde doel hebben. 

Intents worden beheerd vanuit de bovenste navigatiebalk staat **bouwen** sectie, klikt u vervolgens in het linkerpaneel **Intents**. 

## <a name="add-intent"></a>Intentie toevoegen

1. Selecteer **Create new intent** op de pagina **Intents**.

1. In de **maken van nieuwe bedoeling** dialoogvenster vak, voer de naam van de intentie `GetEmployeeInformation`, en klikt u op **gedaan**.

    ![Doel toevoegen](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Een voorbeeld-utterance toevoegen

Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Als u wilt leren Language Understanding (LUIS), moet u voorbeeld utterances toevoegen aan een doel.

1. Op de **GetEmployeeInformation** intentie details pagina, voer een relevante utterance die u kunt van uw gebruikers, zoals verwachten `Does John Smith work in Seattle?` in het tekstvak onder de naam van de intentie en druk op Enter.
 
    ![Schermafbeelding van de intenties pagina, met utterance gemarkeerd](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS alle uitingen geconverteerd naar kleine letters en voegt spaties rond tokens zoals afbreekstreepjes bevatten.

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Voorspellings fouten in intentie 

Een voor beeld van een utterance in een intentie heeft mogelijk een intentie Voorspellings fout tussen de bedoeling van het voor beeld utterance is op dit moment en de Voorspellings intentie die tijdens de training is bepaald. 

Als u utterance-Voorspellings fouten wilt vinden en deze wilt corrigeren, gebruikt u de **evaluatie** opties van de **filter** optie onjuist en onduidelijk gecombineerd met de **weergave** optie van **gedetailleerde weer gave**. 

![Gebruik de filter optie om utterance Voorspellings fouten te vinden en deze op te lossen.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Wanneer de filters en weer gave worden toegepast en er voorbeeld uitingen zijn met fouten, worden de uitingen en de problemen weer gegeven in de lijst voor beeld utterance.

![! [Wanneer de filters en weer gave worden toegepast en er voorbeeld uitingen zijn met fouten, worden de uitingen en de problemen weer gegeven in de lijst voor beeld utterance.] (./media/luis-how-to-add-intents/find-errors-in-utterances.png)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Elke rij toont de huidige Voorspellings Score van de training voor het voor beeld utterance, het dichtstbijzijnde aantal punten, het verschil in deze twee scores. 

### <a name="fixing-intents"></a>Intenties herstellen

Gebruik het [dash board samen vatting](luis-how-to-use-dashboard.md)voor meer informatie over het oplossen van problemen met het Voorspellings beleid. Het dash board samen vatting bevat een analyse voor de laatste training van de actieve versie en biedt de beste suggesties voor het oplossen van uw model.  

## <a name="add-a-custom-entity"></a>Een aangepaste entiteit toevoegen

Zodra een utterance wordt toegevoegd aan een doel, kunt u tekst uit de utterance om een aangepaste entiteit te maken. Een aangepaste entiteit is een manier om de labeltekst voor uitpakken, samen met de juiste intentie. 

Zie [entiteit toevoegen aan utterance](luis-how-to-add-example-utterances.md) voor meer informatie.

## <a name="entity-prediction-discrepancy-errors"></a>Entiteit voorspelling discrepantie fouten 

De entiteit wordt dit onderstreept in rood om aan te geven een [entiteit voorspelling discrepantie](luis-how-to-add-example-utterances.md#entity-status-predictions). Omdat dit het eerste exemplaar van een entiteit, er zijn niet voldoende voorbeelden voor LUIS hebben een hoge betrouwbaarheid die deze tekst is gecodeerd met de juiste entiteit. Dit verschil wordt verwijderd wanneer de app wordt getraind. 

![Schermafbeelding van de intenties de pagina met details, de naam van de aangepaste entiteit in het blauw is gemarkeerd](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

De tekst is in blauw, die wijzen op een entiteit gemarkeerd.  

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

Zie voor meer informatie, [vooraf gedefinieerde entiteit](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

## <a name="using-the-contextual-toolbar"></a>Met behulp van de contextuele werkbalk

Wanneer een of meer voor beeld-uitingen in de lijst zijn geselecteerd door het selectie vakje links van een utterance in te scha kelen, kunt u op de werk balk boven de lijst utterance de volgende acties uitvoeren:

* Opnieuw toewijzen van doel: utterance(s) verplaatsen naar een ander doel
* Utterance(s) verwijderen
* Entiteit filters: alleen weergeven met gefilterde entiteiten uitingen
* Alles weergeven / alleen fouten: uitingen met voorspelling fouten weergeven of weergeven van alle uitingen
* Entiteiten/Tokens weergave: entiteiten weergeven met namen van entiteiten of onbewerkte tekst van utterance weergeven
* Vergrootglas: zoek uitingen met specifieke tekst

## <a name="working-with-an-individual-utterance"></a>Werken met een afzonderlijke utterance

De volgende acties kunnen worden uitgevoerd op een afzonderlijke utterance in het menu van de drie puntjes aan de rechterkant van de utterance:

* Bewerken: de tekst van de utterance wijzigen
* Verwijderen: Verwijder de utterance uit het doel. Als u nog steeds de utterance wilt, een betere methode is om te verplaatsen naar de **geen** intentie. 
* Een patroon toevoegen: Een patroon biedt u de mogelijkheid om een gemeen schappelijk utterance te maken en vervang bare tekst te markeren en tekst te vervangen, waardoor de nood zaak van meer uitingen in de intentie wordt verkleind. 

De **met het label bedoeling** kolom kunt u het doel van de utterance wijzigen.

## <a name="train-your-app-after-changing-model-with-intents"></a>Uw app na het wijzigen van model met intents trainen

Na het toevoegen, bewerken of verwijderen van intents, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app zodat uw wijzigingen worden toegepast op eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het toevoegen van [voorbeeld uitingen](luis-how-to-add-example-utterances.md) met entiteiten. 
