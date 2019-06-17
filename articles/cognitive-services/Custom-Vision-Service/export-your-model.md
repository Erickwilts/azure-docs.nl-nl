---
title: Uw model exporteren naar mobiele - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Informatie over het exporteren van het model voor gebruik bij het maken van mobiele toepassingen.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: 7bf8217f5076c0a95d4db6c1c7cbea7bc93b91f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65550543"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Exporteren van het model voor gebruik met een mobiel apparaat

Custom Vision Service kunt classificaties om uit te voeren offline worden geëxporteerd. U kunt uw geëxporteerde classificatie insluiten in een toepassing en het lokaal uitvoeren op een apparaat voor realtime classificatie.

Custom Vision Service ondersteunt de volgende uitvoer:

* __Tensorflow__ voor __Android__.
* __CoreML__ voor __iOS11__.
* __U kunt ONNX__ voor __Windows ML__.
* Een Windows- of Linux __container__. De container bevat een Tensorflow modelleren en code voor het gebruik van de Custom Vision Service-API-service. 

> [!IMPORTANT]
> Custom Vision Service alleen exporteert __comprimeren__ domeinen. De modellen die worden gegenereerd door compact domeinen zijn geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. Classificaties die zijn gebouwd met een compact domein mogelijk iets minder nauwkeurig dan een standaard domein met dezelfde hoeveelheid trainingsgegevens.
>
> Zie voor meer informatie over het verbeteren van uw classificaties, de [verbetering van de classificatie](getting-started-improving-your-classifier.md) document.

## <a name="convert-to-a-compact-domain"></a>Converteren naar een compacte domein

> [!NOTE]
> De stappen in deze sectie zijn alleen van toepassing als u een bestaande classificatie die niet is ingesteld op het domein te comprimeren.

Als u wilt converteren van het domein van een bestaande classificatie, gebruikt u de volgende stappen uit:

1. Uit de [Custom vision pagina](https://customvision.ai), selecteer de __Start__ pictogram om een lijst van uw projecten weer te geven. U kunt ook de [ https://customvision.ai/projects ](https://customvision.ai/projects) om te zien van uw projecten.

    ![Afbeelding van de oorspronkelijke lijst met pictogram en projecten](./media/export-your-model/projects-list.png)

2. Selecteer een project en selecteer vervolgens de __Gear__ pictogram in de rechterbovenhoek van de pagina.

    ![Afbeelding van het tandwielpictogram](./media/export-your-model/gear-icon.png)

3. In de __domeinen__ sectie, selecteer een __comprimeren__ domein. Selecteer __wijzigingen opslaan__ de wijzigingen op te slaan.

    ![Afbeelding van domeinen selecteren](./media/export-your-model/domains.png)

4. Vanaf de bovenkant van de pagina, selecteer __Train__ trainen met behulp van het nieuwe domein.

## <a name="export-your-model"></a>Uw model exporteren

Voor het exporteren van het model na het opnieuw trainen, gebruikt u de volgende stappen uit:

1. Ga naar de **prestaties** tabblad en selecteer __exporteren__. 

    ![Afbeelding van het pictogram voor exporteren](./media/export-your-model/export.png)

    > [!TIP]
    > Als de __exporteren__ vermelding is niet beschikbaar, en vervolgens een compacte domein maakt geen gebruik van de geselecteerde iteratie. Gebruik de __iteraties__ sectie van deze pagina moet een iteratie die gebruikmaakt van een compacte domein selecteren en selecteer vervolgens __exporteren__.

2. Selecteer de exportindeling en selecteer vervolgens __exporteren__ voor het downloaden van het model.

## <a name="next-steps"></a>Volgende stappen

Het model van uw geëxporteerde integreren in een toepassing door een van de volgende artikelen of voorbeelden verkennen:

* [Uw model Tensorflow gebruiken met Python](export-model-python.md)
* [Uw ONNX-model gebruiken met Windows-Machine Learning](custom-vision-onnx-windows-ml.md)
* Zie het voorbeeld voor [CoreML model in een iOS-toepassing](https://go.microsoft.com/fwlink/?linkid=857726) voor realtime afbeeldingsclassificatie met Swift.
* Zie het voorbeeld voor [Tensorflow-model in een Android-toepassing](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) voor realtime afbeeldingsclassificatie op Android.
* Zie het voorbeeld voor [CoreML model met Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) voor realtime afbeeldingsclassificatie in een Xamarin iOS-app.
