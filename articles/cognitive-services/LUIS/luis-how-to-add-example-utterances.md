---
title: Voorbeelden van utterances toevoegen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Als u wilt leren Language Understanding (LUIS), moet u voorbeeld utterances toevoegen aan een doel.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: badf351f8336e501b3ee1c035fcb389a570750c0
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65072859"
---
# <a name="add-an-entity-to-example-utterances"></a>Een entiteit toevoegen aan de voorbeeld-uitingen 

Voorbeeld uitingen zijn voorbeelden van de tekst van de gebruiker vragen of opdrachten. Om te leren Language Understanding (LUIS), die u nodig hebt om toe te voegen [voorbeeld uitingen](luis-concept-utterance.md) naar een [bedoeling](luis-concept-intent.md).

Normaal gesproken een utterance voorbeeld eerst aan een doel toevoegen en vervolgens maken van entiteiten en label uitingen op de pagina intentie. Als u entiteiten in plaats daarvan eerst maakt, Zie [entiteiten toevoegen](luis-how-to-add-entities.md).

## <a name="marking-entities-in-example-utterances"></a>Entiteiten in de voorbeeld-uitingen markeren

Als u tekst in het voorbeeld utterance markeren voor een entiteit selecteert, wordt een in-place pop-upmenu wordt weergegeven. Gebruik dit menu te maken of een entiteit te selecteren. 

Bepaalde Entiteitstypen, zoals de vooraf gemaakte entiteiten en entiteiten reguliere expressie kunnen niet worden gemarkeerd in de voorbeeld-utterance omdat ze automatisch worden gelabeld. 

## <a name="add-a-simple-entity"></a>Een eenvoudige entiteit toevoegen

In de volgende procedure maakt u maakt en een aangepaste entiteit in de volgende utterance tag op de intentie pagina:

```text
Are there any SQL server jobs?
```

1. Selecteer `SQL server` in de utterance label als een enkele entiteit. In het vak van de entiteit vervolgkeuzelijst die wordt weergegeven, kunt u een bestaande entiteit selecteren of een nieuwe entiteit toevoegen. Als u wilt een nieuwe entiteit toevoegen, typt u de naam `Job` in het tekstvak in en selecteer vervolgens **nieuwe entiteit maken**.

    ![Schermafbeelding van het invoeren van de naam van de entiteit](./media/luis-how-to-add-example-utterances/create-simple-entity.png)

    > [!NOTE]
    > Bij het selecteren van woorden op tag als entiteiten:
    > * Selecteer op van één woord. 
    > * Selecteer voor een set van twee of meer woorden, aan het begin en vervolgens aan het einde van de set.

1. In de **welk type entiteit wilt u maken?** pop-vak, Controleer of de naam van de entiteit en selecteer de **eenvoudige** entiteitstype en selecteer vervolgens **gedaan**.

    Een [woordgroepenlijst](luis-concept-feature.md) wordt meestal gebruikt om het signaal van een enkele entiteit te verbeteren.

## <a name="add-a-list-entity"></a>Een lijst met entiteit toevoegen

Lijst met entiteiten vertegenwoordigen een set exact overeenkomende tekst komt overeen met van verwante woorden in uw systeem. 

Lijst van de afdeling van een bedrijf, u kunt hebben genormaliseerd waarden: `Accounting` en `Human Resources`. De naam van elke genormaliseerde heeft synoniemen. Voor een afdeling, kunnen deze synoniemen elke afdeling acroniemen, getallen of slang bevatten. U hoeft niet te weten van alle waarden bij het maken van de entiteit. U kunt meer toevoegen nadat u hebt bekeken real-user-uitingen door synoniemen.

1. In een utterance voorbeeld op de **Intents** pagina, selecteert u het woord of woordgroep die u wilt dat in de nieuwe lijst. Wanneer de entiteit vervolgkeuzelijst wordt weergegeven, voert u de naam voor de nieuwe entiteit in de lijst in het bovenste tekstvak in en selecteert **nieuwe entiteit maken**.   

1. In de **welk type entiteit wilt u maken?** pop-, de naam van de entiteit en selecteert u **lijst** als het type. Synoniemen van dit item in de lijst toevoegen, en selecteer vervolgens **gedaan**. 

    ![Schermafbeelding van het invoeren van de lijst met entiteit synoniemen](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    U kunt meer lijstitems of meer item synoniemen toevoegen door andere uitingen labels of door het bewerken van de entiteit op basis van de **entiteiten** in het linkernavigatievenster. [Bewerken](luis-how-to-add-entities.md#add-list-entities) de entiteiten kunt u de opties van het invoeren van aanvullende artikelen met synoniemen overeenkomt of een lijst importeren. 

## <a name="add-composite-entity"></a>Een samengestelde entiteit toevoegen

Samengestelde entiteiten zijn gemaakt op basis van bestaande **entiteiten** in een bovenliggende entiteit. 

Ervan uitgaande dat de utterance `Does John Smith work in Seattle?`, een samengestelde utterance entiteit informatie van de naam van de werknemer kunt terugkeren `John Smith`, en de locatie `Seattle` in een samengestelde entiteit. De onderliggende entiteiten moeten al bestaan in de app en worden gemarkeerd in het voorbeeld utterance voordat u de samengestelde entiteit maakt.

1. Als u wilt de onderliggende entiteiten in een samengestelde entiteit, selecteer de **eerste** gelabeld entiteit (meest linkse) in de utterance voor de samengestelde entiteit. Een vervolgkeuzelijst wordt weergegeven om de opties voor deze selectie weer te geven.

1. Selecteer **verpakken in samengestelde entiteit** uit de vervolgkeuzelijst. 

1. Selecteer het laatste woord van de samengestelde entiteit (meest rechtse). U ziet dat een groene lijn volgt de samengestelde entiteit. Dit is de visuele indicator voor een samengestelde entiteit en moet worden opgenomen in alle woorden in de samengestelde entiteit op basis van de meest linkse onderliggende entiteit naar de meest rechtse onderliggende entiteit.

1. Voer de naam van de samengestelde entiteit in de vervolgkeuzelijst.

    Wanneer u de entiteiten correct inpakt, is er een groene lijn onder de volledige woordgroep.

1. Valideren van de samengestelde entiteitsgegevens op de **welk type entiteit wilt u maken?** pop-upvenster vak Selecteer **gedaan**.

    ![Schermafbeelding van de entiteitsgegevens pop](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. De samengestelde entiteit wordt weergegeven met zowel blauw belangrijke functies voor afzonderlijke entiteiten en een groene lijn voor de hele samengestelde entiteit. 

    ![Schermafbeelding van de intenties pagina, met samengestelde entiteit](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-entitys-role-to-utterance"></a>De rol van de entiteit toevoegen aan utterance

Een rol is een benoemde subtype van een entiteit, bepaald door de context van de utterance. U kunt een entiteit binnen een utterance als de entiteit markeren of Selecteer een rol binnen die entiteit. Elke entiteit rollen, inclusief de aangepaste entiteiten die machine geleerd zijn (eenvoudige entiteiten en samengestelde entiteiten) kunt hebben, zich niet hebt geleerd van een machine (vooraf gemaakte entiteiten, reguliere expressie entiteiten, lijst met entiteiten). 

Informatie over [het markeren van een utterance met behulp van entiteit](tutorial-entity-roles.md) uit een praktische handleiding. 

## <a name="entity-status-predictions"></a>Entiteit status voorspellingen

Wanneer u een nieuwe utterance in de portal LUIS invoert, kan de utterance entiteit voorspelling fouten hebben. De fout voorspelling is een verschil tussen hoe een entiteit wordt aangeduid in vergelijking met hoe LUIS de entiteit heeft voorspeld. 

Dit verschil wordt visueel weergegeven in de portal LUIS met een rood onderstreept in de utterance. De rode lijn weergegeven tussen vierkante haken entiteit of buiten de vierkante haken. 

![Schermafbeelding van de entiteit status voorspelling discrepantie](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Selecteer de woorden die zijn onderstreept in rood in de utterance. 

Het van de entiteit wordt weergegeven de **entiteit status** met een rood uitroepteken als er een discrepantie voorspelling. De status van de entiteit met informatie over het verschil tussen entiteiten met het label en voorspelde Selecteer **entiteit status** Selecteer vervolgens het item aan de rechterkant.

![Schermafbeelding van de entiteit status selecteren](./media/luis-how-to-add-example-utterances/entity-prediction-error-correction.png)

De rode-regel kan worden weergegeven op een van de volgende tijden:

   * Wanneer een utterance wordt ingevoerd, maar voordat u wordt de entiteit aangeduid
   * Wanneer de entiteit-label wordt toegepast
   * Als het label van de entiteit wordt verwijderd
   * Als meer dan één entiteit label voor de tekst wordt voorspeld 

De volgende oplossingen helpen bij het oplossen van de entiteit voorspelling afwijking:

|Entiteit|Visuele indicator|voorspelling|Oplossing|
|--|--|--|--|
|Utterance hebt ingevoerd, entiteit is niet nog gelabeld.|rode lijn|Voorspelling is juist.|De entiteit met de voorspelde waarde van label.|
|Niet-gelabelde tekst|rode lijn|Onjuiste voorspelling|De huidige uitingen met behulp van deze onjuist entiteit moeten worden gecontroleerd via alle intents. De huidige uitingen hebt LUIS mistaught dat deze tekst de voorspelde entiteit is.
|Correct gelabelde tekst|blauwe entiteit wilt markeren, rood onderstreept|Onjuiste voorspelling|Geef meer uitingen aan de correct gelabelde entiteit op verschillende locaties en het gebruik. De huidige uitingen zijn niet voldoende is om te leren van LUIS dat dit is de entiteit is of vergelijkbare entiteiten worden weergegeven in de context. Vergelijkbare entiteit moet worden gecombineerd in één enkele entiteit zodat LUIS is niet worden verward. Een andere oplossing is het toevoegen van een woordgroepenlijst om te verbeteren van de betekenis van de woorden. |
|Onjuist gelabelde tekst|blauwe entiteit wilt markeren, rood onderstreept|Juiste voorspelling| Geef meer uitingen aan de correct gelabelde entiteit op verschillende locaties en het gebruik. 

## <a name="other-actions"></a>Andere acties

U kunt acties uitvoeren op de voorbeeld-uitingen als een geselecteerde groep of als een afzonderlijk item. Groepen van voorbeeld van de geselecteerde uitingen veranderen de contextafhankelijke menu boven de lijst. Enkele items kunnen gebruikmaken van het contextafhankelijke menu boven de lijst en de afzonderlijke contextuele weglatingsteken aan het einde van elke rij utterance. 

### <a name="remove-entity-labels-from-utterances"></a>Entiteit verwijderen van uitingen

U kunt machine geleerd entiteit labels verwijderen uit een utterance op de pagina intentie. Als de entiteit niet machine geleerd, kan deze kan niet worden verwijderd uit een utterance. Als u een niet-machine-geleerd entiteit verwijderen uit de utterance moet, moet u de entiteit verwijderen uit de hele app. 

Als een label hebt geleerd van een machine entiteit uit een utterance verwijderen, selecteert u de entiteit in de utterance. Selecteer vervolgens **Label verwijderen** in de entiteit vervolgkeuzelijst die wordt weergegeven.

### <a name="add-prebuilt-entity-label"></a>Vooraf gedefinieerde entiteit label toevoegen

Wanneer u de vooraf gemaakte entiteiten aan uw LUIS-app toevoegt, moet u geen tag uitingen met deze entiteiten. Zie voor meer informatie over het vooraf gemaakte entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

### <a name="add-regular-expression-entity-label"></a>Reguliere expressie entiteit label toevoegen

Als u de reguliere expressie entiteiten aan uw LUIS-app toevoegt, moet u geen code uitingen met deze entiteiten. Zie voor meer informatie over de reguliere expressie entiteiten en hoe u bestanden toevoegt, [entiteiten toevoegen](luis-how-to-add-entities.md#add-regular-expression-entities-for-highly-structured-concepts).


### <a name="create-a-pattern-from-an-utterance"></a>Maken van een patroon van een utterance

Zie [toevoegen patroon uit bestaande utterance op de pagina kunt u lezen wat of entiteit](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).


### <a name="add-patternany-entity"></a>Pattern.any entiteit toevoegen

Als u de entiteiten pattern.any aan uw LUIS-app toevoegt, kunt u uitingen met deze entiteiten kan geen label. Ze zijn alleen geldig in patronen. Zie voor meer informatie over pattern.any entiteiten en het toe te voegen, [entiteiten toevoegen](luis-how-to-add-entities.md#add-patternany-entities-to-capture-free-form-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Uw app na het wijzigen van model met uitingen trainen

Na het toevoegen, bewerken of verwijderen van uitingen, [trainen](luis-how-to-train.md) en [publiceren](luis-how-to-publish-app.md) uw app voor uw wijzigingen zijn doorgevoerd in eindpunt query's. 

## <a name="next-steps"></a>Volgende stappen

Na de labels uitingen in uw intenties, u kunt nu maken een [samengestelde entiteit](luis-how-to-add-entities.md).
