---
title: Testen en opnieuw trainen van een model - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Leer hoe u een installatiekopie testen en vervolgens het model trainen worden gebruikt.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: d516cee81aef66ec58399cb5ff23c89db16bf2ab
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60816668"
---
# <a name="test-and-retrain-a-model-with-custom-vision-service"></a>Testen en opnieuw trainen van een model met Custom Vision Service

Nadat u uw model te trainen, kunt u snel testen met behulp van een lokaal opgeslagen afbeelding of een online-installatiekopie. De test maakt gebruik van de meest recent getrainde iteratie.

## <a name="test-your-model"></a>Uw model testen

1. Uit de [Custom Vision webpagina](https://customvision.ai), selecteert u uw project. Selecteer **snelle Test** aan de rechterkant van de bovenste menubalk. Deze actie opent u een venster met het label **snelle Test**.

    ![De knop snel testen wordt weergegeven in de rechterbovenhoek van het venster.](./media/test-your-model/quick-test-button.png)

2. In de **snelle Test** venster, klikt u in de **afbeelding verzenden** veld en voer de URL van de installatiekopie die u wilt gebruiken voor de test. Als u een lokaal opgeslagen afbeelding in plaats daarvan gebruiken wilt, klikt u op de **door lokale bestanden bladeren** en selecteer een bestand van de lokale installatiekopie.

    ![Afbeelding van de installatiekopie-pagina verzenden](./media/test-your-model/submit-image.png)

De installatiekopie die u selecteert, wordt weergegeven in het midden van de pagina. En vervolgens de resultaten worden weergegeven onder de afbeelding in de vorm van een tabel met twee kolommen, met het label **Tags** en **vertrouwen**. Nadat u de resultaten bekijkt, kunt u sluiten de **snelle Test** venster.

U kunt nu deze testafbeelding toevoegen aan uw model en vervolgens opnieuw uw model te trainen.

## <a name="use-the-predicted-image-for-training"></a>De installatiekopie van het voorspelde gebruiken voor training

Gebruik de volgende stappen voor het gebruik van de installatiekopie van het eerder verzonden voor training:

1. Voor installatiekopieën die worden verzonden naar de classificatie opent de [Custom Vision webpagina](https://customvision.ai) en selecteer de __voorspellingen__ tabblad.

    ![Afbeelding van het tabblad voorspellingen](./media/test-your-model/predictions-tab.png)

    > [!TIP]
    > De standaardweergave ziet u afbeeldingen van de huidige iteratie. U kunt de __iteratie__ vervolgkeuzelijst veld om installatiekopieën verzonden tijdens de vorige iteraties weer te geven.

2. Beweeg de muisaanwijzer over een afbeelding om te zien van de labels die door de classificatie zijn voorspeld.

    > [!TIP]
    > Afbeeldingen worden gerangschikt, zodat de afbeeldingen die de meeste winst aan de classificatie bieden kunnen aan de bovenkant zijn. Selecteer een andere sortering, gebruikt u de __sorteren__ sectie.

    Als u wilt een afbeelding toevoegen aan uw trainingsgegevens, selecteer de installatiekopie, selecteert u het label en selecteer vervolgens __opslaan en sluiten__. De installatiekopie wordt verwijderd uit __voorspellingen__ en toegevoegd aan de training-installatiekopieën. U kunt deze weergeven door de __Trainingsafbeeldingen__ tabblad.

    ![Afbeelding van de labels pagina](./media/test-your-model/tag-image.png)

3. Gebruik de __Train__ knop de classificatie te trainen.

## <a name="next-steps"></a>Volgende stappen

[De classificatie verbeteren](getting-started-improving-your-classifier.md)
