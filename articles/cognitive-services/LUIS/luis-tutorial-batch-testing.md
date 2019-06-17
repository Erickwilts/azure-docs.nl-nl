---
title: Batch-test
titleSuffix: Azure Cognitive Services
description: Deze zelfstudie wordt gedemonstreerd hoe u met batch testen utterance voorspelling problemen in uw app vinden en corrigeren.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: af04ca19961abcfc7ee218824a4a1a804f7ad79c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146160"
---
# <a name="tutorial-batch-test-data-sets"></a>Zelfstudie: Gegevenssets voor batch-test

Deze zelfstudie wordt gedemonstreerd hoe u met batch testen utterance voorspelling problemen in uw app vinden en corrigeren.  

Batch testen, kunt u voor het valideren van de actieve status van het model met een bekende set gelabelde uitingen en entiteiten getraind. In de JSON-indeling batch-bestand, de utterances toevoegen en instellen van de entiteit-labels die u nodig hebt binnen de utterance voorspeld. 

Vereisten voor het testen van batch:

* Maximaal 1000 uitingen per test. 
* Geen dubbele waarden. 
* Entiteitstypen toegestaan: alleen veilig geleerd entiteiten van eenvoudige en samengesteld. Testen van de batch is alleen nuttig voor het veilig geleerd intenties en entiteiten.

Wanneer u een app dan in deze zelfstudie gebruikt, doen *niet* gebruikt u de voorbeeld-uitingen al toegevoegd aan een doel. 

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Voorbeeld-app importeren
> * Maak een batchbestand testen 
> * Een batch-test uitvoeren
> * Bekijk de resultaten
> * Fouten herstellen 
> * Testen van de batch

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Voorbeeld-app importeren

Ga door met de in de laatste zelfstudie gemaakt app, **Human Resources**. 

Voer de volgende stappen uit:

1.  Download het [JSON-bestand van de app](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-review-HumanResources.json) en sla het op.

2. Importeer de JSON in een nieuwe app.

3. Ga naar het gedeelte **Beheren**, open het tabblad **Versies**, kloon de versie en noem deze `batchtest`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam wordt gebruikt als onderdeel van de URL-route, kan de naam geen tekens bevatten die niet zijn toegestaan in een URL. 

4. Train de app.

## <a name="batch-file"></a>Batch-bestand

1. Maak `HumanResources-jobs-batch.json` in een teksteditor of [downloaden](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-jobs-batch.json) deze. 

2. In het JSON-indeling batchbestand utterances met toevoegen de **bedoeling** gewenste voorspelde in de test. 

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>Voer de batch

1. Selecteer **Test** in de bovenste navigatiebalk. 

2. Selecteer **Batch testen deelvenster** in het deelvenster aan de rechterkant. 

    [![Schermafbeelding van LUIS-app met behulp van Batch test deelvenster gemarkeerd](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Selecteer **importeren gegevensset**.

    [![Schermafbeelding van LUIS-app met importeren gegevensset gemarkeerd](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Kies de locatie van het bestand van de `HumanResources-jobs-batch.json` bestand.

5. Naam van de gegevensset `intents only` en selecteer **gedaan**.

    ![Bestand selecteren](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. Selecteer de knop **Run**. 

7. Selecteer **resultaten**.

8. Bekijk de resultaten in de grafiek en legenda.

    [![Schermafbeelding van LUIS-app met de resultaten van batch](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Bekijk de resultaten van batch

De batch worden vier kwadranten van de resultaten weergegeven. Aan de rechterkant van de grafiek is een filter. Het filter is standaard ingesteld op het eerste doel in de lijst. Het filter bevat alle intenties en entiteiten alleen eenvoudige en samengesteld. Wanneer u selecteert een [sectie van de grafiek](luis-concept-batch-test.md#batch-test-results) of een punt in de grafiek, de bijbehorende utterance(s) weer te geven onder de grafiek. 

Bij het aanwijzen van de grafiek, kan muiswiel vergroten of verkleinen van de weergave in de grafiek. Dit is handig wanneer er veel punten op de grafiek geclusterde nauw samen. 

De grafiek is in vier kwadranten, met twee van de secties in het rood weergegeven. **Dit zijn de secties zich richten op**. 

### <a name="getjobinformation-test-results"></a>De resultaten van GetJobInformation

De **GetJobInformation** weergegeven in het filter de resultaten weergeven dat 2 van de vier voorspellingen gelukt is. Selecteer de naam **fout-positief** boven de bovenste juiste quadrant om te zien van de uitingen onder de grafiek. 

![LUIS batch test uitingen](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

Waarom zijn twee van de voorspelde als uitingen **ApplyForJob**, in plaats van de juiste intentie **GetJobInformation**? De twee intenties zijn zeer nauw verwant woord keuze en word-indeling. Bovendien zijn er bijna drie keer zoveel voorbeelden voor **ApplyForJob** dan **GetJobInformation**. Deze eenvormigheid van uitingen voorbeeld weegt **ApplyForJob** van intentie voordeel. 

U ziet dat beide intents de dezelfde telling van fouten hebben. Een onjuiste voorspelling in één doel is van invloed op de andere intentie ook. Beide zijn fouten opgetreden omdat de uitingen zijn onjuist voorspeld voor één doel, en ook onjuist niet voor een ander doel voorspeld. 

![LUIS batch test filter fouten](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

De bijbehorende boven uitingen punt in de **fout-positief** sectie zijn `Can I apply for any database jobs with this resume?` en `Can I apply for any database jobs with this resume?`. Voor de eerste utterance, het woord `resume` is alleen gebruikt **ApplyForJob**. Voor de tweede utterance, het woord `apply` is alleen gebruikt de **ApplyForJob** intentie.

## <a name="fix-the-app"></a>De app oplossen

Het doel van deze sectie is om alle de uitingen voorspeld correct voor **GetJobInformation** door de app op te lossen. 

Een schijnbaar snelle oplossing zou zijn om toe te voegen deze uitingen van batch-bestand met de juiste intent. Dat is niet wat u wilt echter doen. Wilt u LUIS om goed te voorspellen deze uitingen zonder dat ze toe te voegen als voorbeelden. 

U vraagt zich misschien ook af over het verwijderen van uitingen van **ApplyForJob** totdat de hoeveelheid utterance hetzelfde als is **GetJobInformation**. Die de resultaten van het probleem kan mogelijk maar zou LUIS belemmeren van het voorspellen van dit doel nauwkeurig zodra. 

De eerste oplossing is het toevoegen van meer uitingen naar **GetJobInformation**. De tweede oplossing is het verminderen van het gewicht van woorden, zoals `resume` en `apply` naar de **ApplyForJob** intentie. 

### <a name="add-more-utterances"></a>Meer utterances toevoegen

1. Sluit het deelvenster van de test batch door het selecteren van de **testen** knop in het bovenste navigatievenster. 

2. Selecteer **GetJobInformation** in de lijst intents. 

3. Toevoegen van meer uitingen die zijn verschillend voor de lengte, word keuze en word-indeling, zorg ervoor dat u de voorwaarden `resume`, `c.v.`, en `apply`:

    |Voorbeeld-uitingen voor **GetJobInformation** doel|
    |--|
    |De nieuwe taak in het datawarehouse voor een stocker vereist dat ik met een cv toepassen?|
    |Waar zijn de taken dakbedekking vandaag?|
    |Ik gehoord dat er is een medische codering taak waarvoor een hervatten.|
    |Ik wil graag een taak helpen college kinderen hun c.v.s. schrijven |
    |Hier is mijn hervatten, op zoek naar een nieuw bericht op de beroepsopleiding met computers.|
    |Welke functies zijn beschikbaar in de onderliggende en vanaf thuis care?|
    |Is er een Stagiair helpdesk op de krant?|
    |Mijn opsommen Ik ben goed bij het analyseren van inkoop, budgetten en verloren geld weergeven Is er iets voor dit type werk?|
    |Waar zijn de aarde door zoomen taken nu?|
    |Ik heb acht jaar als een EMS-stuurprogramma gewerkt. Nieuwe taken?|
    |Nieuwe food afhandeling van taken vereist toepassing?|
    |Hoeveel nieuwe taken in het werk yard zijn beschikbaar?|
    |Is er een nieuw HR-bericht voor arbeid relaties en onderhandelingen?|
    |Ik heb een masters in bibliotheken en -beheer. Alle nieuwe functies?|
    |Zijn er geen babysitting taken voor 13 jaar olds in de plaats vandaag?|

    Kan geen label de **taak** entiteit in de uitingen. Deze sectie van de zelfstudie is gericht op alleen de intentie voorspelling.

4. De app door het selecteren van de trein **Train** in het bovenste navigatievenster rechts.

## <a name="verify-the-new-model"></a>Controleer of het nieuwe model

Om te controleren dat de uitingen in de batch-test correct worden voorspeld, moet u de batch-test opnieuw uitvoeren.

1. Selecteer **Test** in de bovenste navigatiebalk. Als de resultaten van de batch nog steeds geopend zijn, selecteert u **terug naar lijst met**.  

2. Selecteer het weglatingsteken (***...*** ) aan de rechterkant van de batchnaam en selecteer **gegevensset uitvoeren**. Wacht totdat de batch-test is voltooid. U ziet dat de **resultaten** knop is nu groen. Dit betekent dat de hele batch is uitgevoerd.

3. Selecteer **resultaten**. De intenties moeten alle groen pictogrammen aan de linkerkant van de intentie namen hebben. 

    ![Schermafbeelding van LUIS met batch resultaten knop gemarkeerd](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)

## <a name="create-batch-file-with-entities"></a>Batch-bestand met entiteiten maken 

Als u wilt controleren of entiteiten in een batch-test, moeten de entiteiten worden met het label in de batch-JSON-bestand. Alleen de machine geleerde entiteiten worden gebruikt: eenvoudige en samengestelde entiteiten. Voeg geen niet-machine-geleerde entiteiten doordat ze via reguliere expressies altijd worden gevonden, of expliciete tekst komt overeen met.

De variatie van entiteiten voor het totale aantal woord ([token](luis-glossary.md#token)) aantal kan gevolgen hebben voor de voorspelling kwaliteit. Zorg ervoor dat de opgegeven met de intent met gelabelde uitingen trainingsgegevens bevat verschillende lengtes van entiteit. 

Wanneer het eerst schrijven en testen van batch-bestanden, het is raadzaam om te beginnen met een paar uitingen en entiteiten die u kent werkt, evenals enkele die u mogelijk te bekijken welke vereiste onjuist worden voorspeld. Dit helpt u te focussen op de probleemgebieden snel. Na het testen van de **GetJobInformation** en **ApplyForJob** intents met behulp van verschillende andere taaknamen, die niet zijn voorspeld, deze test batchbestand is ontwikkeld om te zien of er een probleem voorspelling met bepaalde waarden voor **taak** entiteit. 

De waarde van een **taak** entiteit, opgegeven in de test-uitingen is meestal een of twee woorden, met een paar voorbeelden wordt meer woorden. Als _uw eigen_ human resources-app heeft doorgaans taaknamen van vele woorden bestaat, wordt de voorbeeld-uitingen met het label met **taak** entiteit in deze app niet goed werkt.

1. Maak `HumanResources-entities-batch.json` in een teksteditor zoals [VSCode](https://code.visualstudio.com/) of [downloaden](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-entities-batch.json) deze.


2. In de JSON-indeling batch-bestand, Voeg een matrix met objecten die uitingen met de **bedoeling** gewenste voorspelde in de test, evenals de locaties van alle entiteiten in de utterance. Aangezien een entiteit op basis van tokens, zorg starten en stoppen van elke entiteit op een teken. Niet beginnen of eindigen van de utterance op een spatie. Dit zorgt ervoor dat een fout opgetreden tijdens het importeren van de batch-bestand.  

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>Voer de batch met entiteiten

1. Selecteer **Test** in de bovenste navigatiebalk. 

2. Selecteer **Batch testen deelvenster** in het deelvenster aan de rechterkant. 

3. Selecteer **importeren gegevensset**.

4. Kies de locatie van het bestand system van de `HumanResources-entities-batch.json` bestand.

5. Naam van de gegevensset `entities` en selecteer **gedaan**.

6. Selecteer de knop **Run**. Wacht totdat de test is voltooid.

7. Selecteer **resultaten**.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="review-entity-batch-results"></a>Bekijk de resultaten van de entiteit-batch

De grafiek wordt geopend met alle intents correct voorspeld. Schuif omlaag in het filter aan de rechterkant te vinden van de entiteit voorspellingen met fouten. 

1. Selecteer de **taak** entiteit in het filter.

    ![Fout entiteit voorspellingen in filter](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    De wijzigingen van de grafiek om de voorspellingen van de entiteit weer te geven. 

2. Selecteer **False negatieve** links in het onderste quadrant van de grafiek. Gebruik vervolgens de toetsenbord combinatie CTRL + E om over te schakelen in de weergave van het token. 

    [![Token-weergave van voorspellingen van de entiteit](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)
    
    Controleren van de uitingen onder de grafiek ziet u een consistente fout wanneer de taaknaam bevat `SQL`. Controleren van de voorbeeld-uitingen en de lijst met woorden, SQL is slechts één keer gebruikt, en alleen als onderdeel van een grotere taaknaam `sql/oracle database administrator`.

## <a name="fix-the-app-based-on-entity-batch-results"></a>De app op basis van resultaten in de batch entiteiten oplossen

Oplossen van de app vereist LUIS om correct te bepalen de variaties van de SQL-taken. Er zijn verschillende opties voor die oplossing. 

* Meer voorbeeld uitingen, die gebruikmaken van SQL en deze woorden labelen als een taak entiteit expliciet toevoegen. 
* Meer SQL-taken expliciet toevoegen aan de woordgroepenlijst met

Deze taken blijven voor u te doen.

Toevoegen van een [patroon](luis-concept-patterns.md) voordat de entiteit correct wordt voorspeld, gaat niet om het probleem te verhelpen. Dit is omdat het patroon niet overeen met totdat alle entiteiten in het patroon worden gedetecteerd. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

De zelfstudie gebruikt een batch-test om problemen met het huidige model. Het model is vast en getest met de batch-bestand om te controleren of dat de wijziging juist is.

> [!div class="nextstepaction"]
> [Meer informatie over patronen](luis-tutorial-pattern.md)

