---
title: Contextuele gegevens met rollen-LUIS
titleSuffix: Azure Cognitive Services
description: Zoek gerelateerde gegevens op basis van de context. Een bestemmings- en doellocatie voor de fysieke verhuizing tussen twee gebouwen zijn bijvoorbeeld aan elkaar gerelateerd.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 2eac05712dc7b3deceba52681195101f9bf2b40c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560016"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Zelfstudie: Contextueel gerelateerde gegevens ophalen uit een utterance

In deze zelfstudie zoekt u gerelateerde gegevens op basis van context. Denk bijvoorbeeld aan de locaties van oorsprong en bestemming voor een overplaatsing tussen vestigingen. Hiervoor zijn mogelijk beide gegevenselementen vereist en ze zijn aan elkaar gerelateerd.  

Een rol kan worden gebruikt met een vooraf samengesteld of aangepast entiteits type en wordt gebruikt in beide voor beelden van uitingen en patronen. 

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Nieuwe app maken
> * Intentie toevoegen 
> * Informatie over de oorsprong en bestemming ophalen met behulp van rollen
> * Trainen
> * Publiceren
> * Intents en entiteits rollen ophalen uit het eind punt

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>Verwante gegevens

Deze app bepaalt waar een werknemer moet worden verplaatst van de plaats van oorsprong naar de plaats van bestemming. Er wordt een GeographyV2-vooraf gedefinieerde entiteit gebruikt om de namen van steden te identificeren en het maakt gebruik van rollen om de locatie typen (oorsprong en bestemming) in de utterance te bepalen.

Er moet een rol worden gebruikt wanneer de entiteits gegevens die moeten worden geëxtraheerd:

* Is aan elkaar gerelateerd in de context van de utterance.
* Gebruikt een specifieke woorden keuze om elke rol aan te geven. Voorbeelden van deze woorden zijn: van/naar verlaten/gaan naar, weg van/naar.
* Beide rollen bevinden zich vaak in dezelfde utterance, waardoor LUIS kan leren van dit frequente contextuele gebruik.
* Moet als eenheid informatie worden gegroepeerd en verwerkt door de client-app.

## <a name="create-a-new-app"></a>Een nieuwe app maken

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Een intentie maken om werknemers te verplaatsen tussen steden

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Selecteer **Create new intent**. 

1. Voer in het pop-updialoogvenster `MoveEmployeeToCity` in en selecteer vervolgens **Done**. 

    ![Schermopname van het pop-updialoogvenster voor het maken van een nieuwe intentie met](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Voeg voorbeelden van utterances toe aan de intentie.

    |Voorbeelden van utterances|
    |--|
    |John W. Smith verplaatsen van Seattle naar Orlando|
    |transfer Jill Jones from Seattle to Cairo|
    |Place John Jackson away from Tampa, coming to Atlanta |
    |Debra Doughtery verplaatsen naar Tulsa van Chicago|
    |mv Jill Jones leaving Cairo headed to Tampa|
    |Shift Alice Anderson to Oakland from Redmond|
    |Carl Chamerlin from San Francisco to Redmond|
    |Transfer Steve Standish from San Diego toward Bellevue |
    |lift Tanner Thompson from Kansas city and shift to Chicago|

    [![Schermopname van LUIS met nieuwe uitingen in de intentie MoveEmployee](./media/tutorial-entity-roles/hr-enter-utterances.png)](./media/tutorial-entity-roles/hr-enter-utterances.png#lightbox)

## <a name="add-prebuilt-entity-geographyv2"></a>Vooraf samengestelde entiteit geographyV2 toevoegen

De vooraf samengestelde entiteit, geographyV2, extraheert locatie-informatie, inclusief plaatsnamen. Omdat de uitingen twee plaatsnamen heeft, met betrekking tot elkaar in de context, gebruikt u rollen om die context te extra heren.

1. Selecteer **entiteiten** in de navigatie aan de linkerkant.

1. Selecteer vooraf **samengestelde entiteit toevoegen**en selecteer `geo` vervolgens in de zoek balk om de vooraf gemaakte entiteiten te filteren. 

    ![Een vooraf gemaakte geographyV2-entiteit toevoegen aan de app](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)
1. Schakel het selectie vakje in en selecteer **gereed**.
1. Selecteer in de lijst **entiteiten** de **geographyV2** om de nieuwe entiteit te openen. 
1. Voeg twee rollen toe `Origin`, en `Destination`. 

    ![Rollen toevoegen aan vooraf samengestelde entiteit](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)
1. Selecteer  intenties in de navigatie aan de linkerkant en selecteer vervolgens de **MoveEmployeeToCity** intentie. U ziet dat de namen van steden worden aangeduid met de vooraf samengestelde entiteit **geographyV2**.
1. Selecteer de oorspronkelijke locatie in de eerste utterance van de lijst. Er wordt een vervolg keuzelijst weer gegeven. Selecteer **geographyV2** in de lijst en volg vervolgens het menu om de **oorsprong**te selecteren.
1. Gebruik de methode uit de vorige stap om alle rollen van locaties in alle uitingen te markeren. 


## <a name="add-example-utterances-to-the-none-intent"></a>Voorbeelduitingen toevoegen aan de intentie None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>De app trainen zodat de wijzigingen aan de intentie kunnen worden getest 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>De app publiceren zodat op het getrainde model query's kunnen worden uitgevoerd vanaf het eindpunt

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Voorspelling van intenties en entiteiten ophalen van eindpunt

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Ga naar het einde van de URL in de adresbalk en voer `Please move Carl Chamerlin from Tampa to Portland` in. De laatste parameter van de queryreeks is `q`, de utterance **query**. Deze utterance is niet hetzelfde als een van de gelabelde uitingen, dus het is een goede test en moet de `MoveEmployee` bedoeling retour neren met de entiteit geëxtraheerd.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "geographyV2",
          "role": "Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "geographyV2",
          "role": "Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    De juiste intentie is voor speld en de matrix entiteiten heeft zowel de oorspronkelijke als de doel rollen in de bijbehorende eigenschap **entities** .
    
## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Gerelateerde informatie

* [Concepten van entiteiten](luis-concept-entity-types.md)
* [Concepten van rollen](luis-concept-roles.md)
* [Lijst met vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md)
* [Trainen](luis-how-to-train.md)
* [Hoe u kunt publiceren](luis-how-to-publish-app.md)
* [Testen in de LUIS-portal](luis-interactive-test.md)
* [Rollen](luis-concept-roles.md)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een nieuwe intentie gemaakt en voorbeelden van utterances toegevoegd voor de contextueel geleerde gegevens van locaties van oorsprong en bestemming. Zodra de app is getraind en gepubliceerd, kan een clienttoepassing die informatie gebruiken om een verplaatsingsticket te maken met de relevante informatie.

> [!div class="nextstepaction"] 
> [Informatie over het toevoegen van een samengestelde entiteit](luis-tutorial-composite-entity.md) 
