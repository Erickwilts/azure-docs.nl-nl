---
title: Test-app in de portal van LUIS
titleSuffix: Language Understanding - Azure Cognitive Services
description: Met Language Understanding (LUIS) kunt werken continu op uw toepassing het verfijnen en verbeteren van de taal begrijpen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 51c6a58567b35c9b8486d8634b0bed1af7218994
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199068"
---
# <a name="test-your-luis-app-in-the-luis-portal"></a>Test uw LUIS-app in de portal LUIS
<a name="train-your-app"></a>
[Testen](luis-concept-test.md) een app is een iteratief proces. Na het trainen van uw LUIS-app, door deze te testen met voorbeeldgegevens uitingen om te controleren of de intenties en entiteiten correct worden herkend. Als ze niet zijn, moet u opnieuw updates voor de LUIS-app, trainen en testen. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>

## <a name="test-an-utterance"></a>Een utterance testen

1. Toegang tot uw app door het selecteren van de naam ervan op de **mijn Apps** pagina. 

2. Toegang krijgen tot de **Test** deelvenster dia-out, selecteer **Test** in het bovenste gedeelte van uw toepassing.

    ![Pagina Train & Test-App](./media/luis-how-to-interactive-test/test.png)

3. Voer een utterance in het tekstvak in en selecteer Enter. U kunt typen zoals veel uitingen testen als u wilt in de **testen**, maar slechts één utterance tegelijk.

4. De utterance, de belangrijkste doel en de score zijn toegevoegd aan de lijst met uitingen onder het tekstvak.

    ![Interactieve testen identificeert de verkeerde bedoelingen](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="inspect-score"></a>Controleren van score

Controleren van details van de testresultaten in het **inspecteren** deelvenster. 
 
1. Met de **Test** dia-out Configuratiescherm openen, selecteer **inspecteren** voor een utterance die u wilt vergelijken. 

    ![Selecteer de knop controleren voor meer informatie over de resultaten van de](./media/luis-how-to-interactive-test/inspect.png)

2. De **inspectie** deelvenster wordt weergegeven. Het deelvenster bevat de scoring-doel, evenals de geïdentificeerde entiteiten boven. Het deelvenster toont het resultaat van de geselecteerde utterance.

    ![Het deelvenster bevat de scoring-doel, evenals de geïdentificeerde entiteiten boven. Het deelvenster toont het resultaat van de geselecteerde utterance.](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Juiste boven scoring-doel

1. Als boven scoren van intentie onjuist is, selecteert u de **bewerken** knop.

2.  Selecteer in de vervolgkeuzelijst het juiste type voor de utterance.

    ![Juiste doel selecteren](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Sentiment-resultaten weergeven

Als **sentimentanalyse** is geconfigureerd op de **[publiceren](luis-how-to-publish-app.md#enable-sentiment-analysis)** pagina, de resultaten van de omvatten het gevoel gevonden in de utterance. 

![Afbeelding van het testvenster met sentimentanalyse](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Corrigeer de bedoeling komt overeen met patroon

Als u [patronen](luis-concept-patterns.md) en de utterance overeenkomen met een patroon, maar de verkeerde bedoelingen is voorspeld, selecteert u de **bewerken** koppelen door het patroon en selecteer vervolgens het juiste doel.

## <a name="compare-with-published-version"></a>Vergelijken met de gepubliceerde versie

U kunt de actieve versie van uw app met de gepubliceerde testen [eindpunt](luis-glossary.md#endpoint) versie. In de **inspecteren** Configuratiescherm, selecteer **vergelijken met gepubliceerd**. Alle testen op basis van het gepubliceerde model wordt afgetrokken van saldo van uw Azure-abonnement quotum. 

![Vergelijken met gepubliceerd](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Eindpunt JSON in test-deelvenster weergeven
U vindt het eindpunt JSON geretourneerd voor de vergelijking door het selecteren van de **weergeven JSON-weergave**.

![Gepubliceerde JSON-antwoord](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Aanvullende instellingen in het Configuratiescherm van de test

### <a name="luis-endpoint"></a>LUIS-eindpunt

Als u verschillende LUIS eindpunten hebt, gebruikt u de **extra instellingen** koppeling op de testmachine deelvenster voor het wijzigen van het eindpunt dat wordt gebruikt voor het testen van de gepubliceerd. Als u niet zeker weet welk eindpunt te gebruiken, selecteert u de standaard **Starter_Key**. 

![Deelvenster testen met de overige instellingen koppeling gemarkeerd](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Bing Spell Check-correcties in test-deelvenster weergeven

Vereisten voor het weergeven van de spellingcorrecties: 

* Gepubliceerde app
* Bing spellingcontrole [servicesleutel](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api). De servicesleutel is niet opgeslagen en moet opnieuw worden ingesteld voor elke browsersessie. 

Gebruik de volgende procedure om op te nemen de [Bing spellingcontrole versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) service in het deelvenster resultaten. 

1. In de **Test** in het deelvenster een utterance invoeren. Wanneer de utterance wordt voorspeld, selecteert u **[inspecteren](#inspect-score)** onder de utterance die u hebt ingevoerd. 

2. Wanneer de **inspecteren** deelvenster wordt geopend, selecteer  **[vergelijken met gepubliceerde](#compare-with-published-version)** . 

3. Wanneer de **gepubliceerd** deelvenster wordt geopend, selecteer  **[extra instellingen](#additional-settings-in-test-panel)** .

4. Voer in het pop-updialoogvenster uw **Bing spellingcontrole** servicesleutel. 
    ![Voer de sleutel van de Bing Spell Check-service](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

5. Voer een query met een onjuiste spelling zoals `book flite to seattle` en voert u selecteert. De onjuiste spelling van het woord `flite` wordt vervangen in de query verzonden naar LUIS en de resulterende JSON bevat zowel de oorspronkelijke query als `query`, en de gecorrigeerde spelling in de query als `alteredQuery`.

    ![Gecorrigeerde spelling JSON](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>

## <a name="batch-testing"></a>Batchgewijs testen
Zie batch testen [concepten](luis-concept-batch-test.md) en leer [hoe](luis-how-to-batch-test.md) tegelijk uitingen testen.

## <a name="next-steps"></a>Volgende stappen

Als testen geeft aan dat uw LUIS-app niet wordt herkend door de juiste intenties en entiteiten, kunt u uw LUIS-app om nauwkeurigheid te verbeteren door meer uitingen labels of het toevoegen van functies werken. 

* [Label van de voorgestelde uitingen van LUIS](luis-how-to-review-endpoint-utterances.md) 
* [Functies gebruiken om uw LUIS-app-prestaties te verbeteren](luis-how-to-add-features.md) 
