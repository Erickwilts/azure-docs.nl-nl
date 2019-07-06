---
title: 'Quickstart: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Java'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg tags toe, upload afbeeldingen, train uw project en doe een voorspelling met behulp van de Java-SDK.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: areddish
ms.openlocfilehash: 62f60e0a143bee7e8392792719b30aca1d629880
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606859"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Quickstart: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Java

Dit artikel biedt informatie en voorbeeldcode om u op weg te helpen met de Custom Vision-SDK voor Java een afbeeldingsclassificatiemodel te maken. Wanneer u het project hebt gemaakt, kunt u tags toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en het eindpunt gebruiken om afbeeldingen programmatisch te testen. Gebruik dit voorbeeld als een sjabloon om uw eigen Java-toepassing te maken. Zie de [handleiding voor browsers](getting-started-build-a-classifier.md) als u het ontwikkelproces wilt doorlopen en een classificatiemodel wilt gebruiken _zonder_ code.

## <a name="prerequisites"></a>Vereisten

- Een Java IDE naar keuze
- [JDK 7 of 8](https://aka.ms/azure-jdks) is geïnstalleerd.
- Maven is geïnstalleerd

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>De Custom Vision-SDK en voorbeeldcode ophalen

Als u een Java-app wilt schrijven die gebruikmaakt van Custom Vision, hebt u de Maven-pakketten van Custom Vision nodig. Deze pakketten zijn opgenomen in het voorbeeldproject u downloadt, maar u kunt toegang tot deze afzonderlijk hier.

U kunt de Custom Vision-SDK uit de centrale maven-opslagplaats installeren:

- [Training-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Voorspelling-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Kloon of download het project [Voorbeelden voor Cognitive Services Java SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Ga naar de map **Vision/CustomVision/** .

Met dit Java-project wordt een nieuwe afbeeldingsclassificatieproject van Custom Vision gemaakt met de naam __Sample Java Project__. Dit kan worden geopend via de [Custom Vision-website](https://customvision.ai/). Vervolgens worden afbeeldingen geüpload om een classificatie te trainen en te testen. In dit project moet met de classificatie worden vastgesteld of een boom een __Canadese den__ of een __Japanse sierkers__ is.

[!INCLUDE [get-keys](includes/get-keys.md)]

Het programma is zodanig geconfigureerd dat uw belangrijkste gegevens als omgevingsvariabelen worden opgeslagen. Stel deze variabelen in via de map **Vision/CustomVision** in PowerShell. Voer daarna de volgende opdrachten in:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>De code begrijpen

Laad het `Vision/CustomVision`-project in uw Java IDE en open het bestand _CustomVisionSamples.java_. Zoek de **runSample** methode en uitcommentarieer de **ObjectDetection_Sample** methodeaanroep&mdash;deze methode voert u het scenario voor het detecteren van object, die niet wordt behandeld in deze handleiding. De methode **ImageClassification_Sample** implementeert de primaire functie van dit voorbeeld, namelijk het navigeren naar de definitie ervan en het inspecteren van de code.

### <a name="create-a-custom-vision-service-project"></a>Een Custom Vision Service-project maken

Met het eerste deel van de code wordt een afbeeldingsclassificatieproject gemaakt. Het project wordt weergegeven op de [Custom Vision-website](https://customvision.ai/), die u eerder hebt bezocht. 

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=57-63)]

### <a name="create-tags-in-the-project"></a>Labels maken in het project

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=65-74)]

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

De voorbeeldafbeeldingen zijn opgenomen in de map **src/main/resources** van het project. Ze worden daar gelezen en met de bijbehorende labels naar de service geüpload.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=76-87)]

Het vorige codefragment maakt gebruik van twee hulpfuncties die de afbeeldingen als resourcestreams ophalen en ze naar de service uploaden.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=277-314)]

### <a name="train-the-classifier-and-publish"></a>De classificatie te trainen en publiceren

Deze code wordt de eerste versie in het project wordt gemaakt en vervolgens publiceert dat iteratie naar het eindpunt van de voorspelling. De naam van de gepubliceerde iteratie kan worden gebruikt om voorspelling aanvragen te verzenden. Een iteratie is niet beschikbaar in de voorspelling eindpunt totdat deze is gepubliceerd.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=89-99)]

### <a name="use-the-prediction-endpoint"></a>Voorspellingseindpunt gebruiken

Het voorspellingseindpunt, dat door het `predictor`-object hier wordt weergegeven, is de verwijzing waarmee u een afbeelding naar het huidige model kunt verzenden en een classificatievoorspelling kunt verkrijgen. In dit voorbeeld wordt `predictor` elders gedefinieerd met de omgevingsvariabele voor de voorspellingssleutel.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=108-120)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Als u de oplossing met Maven wilt compileren en uitvoeren, voert u de volgende opdracht uit in de projectmap in PowerShell:

```powershell
mvn compile exec:java
```

De console-uitvoer van de toepassing moet er uitzien zoals de volgende tekst:

```console
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

U kunt dan controleren of de voorspelling van de tekstafbeelding (de laatste regels van de uitvoer) correct is.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van het afbeeldingsclassificatieproces in code kan worden uitgevoerd. Met dit voorbeeld wordt één trainingsinteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](test-your-model.md)
