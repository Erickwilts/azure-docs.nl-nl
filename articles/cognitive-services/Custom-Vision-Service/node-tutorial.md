---
title: 'Quickstart: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Node.js'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg tags toe, upload afbeeldingen, train uw project en doe een voorspelling met behulp van de Node.js-SDK.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 92bcb7f81082c1a5a66efa2fa06a6922a79905d0
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827520"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-nodejs-sdk"></a>Quickstart: Een afbeeldingsclassificatieproject maken met de Node.js-SDK van Custom Vision

Dit artikel biedt informatie en voorbeeldcode om u te helpen om met de Custom Vision-SDK met Node.js een afbeeldingsclassificatiemodel te maken. Nadat deze gemaakt, kunt u labels toevoegen, afbeeldingen uploaden, trainen van het project, verkrijgen van het project gepubliceerde voorspelling eindpunt-URL en het eindpunt voor het testen van een installatiekopie van een via een programma gebruiken. Gebruik dit voorbeeld als een sjabloon om uw eigen Node.js-toepassing te maken. Zie de [handleiding voor browsers](getting-started-build-a-classifier.md) als u het ontwikkelproces wilt doorlopen en een classificatiemodel wilt gebruiken _zonder_ code.

## <a name="prerequisites"></a>Vereisten

- [Node.js 8](https://www.nodejs.org/en/download/) of hoger geïnstalleerd.
- [npm](https://www.npmjs.com/) geïnstalleerd.

## <a name="install-the-custom-vision-sdk"></a>De Custom Vision-SDK installeren

Als u de Custom Vision Service-SDK voor Node.js wilt installeren, voert u de volgende opdracht uit in PowerShell:

```powershell
npm install @azure/cognitiveservices-customvision-training
npm install @azure/cognitiveservices-customvision-prediction
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [node-get-images](includes/node-get-images.md)]

## <a name="add-the-code"></a>Code toevoegen

Maak een nieuw bestand met de naam *sample.js* in uw projectmap.

### <a name="create-the-custom-vision-service-project"></a>Het Custom Vision Service-project maken

Als u een nieuw Custom Vision Service-project wilt maken, voegt u de volgende code aan uw script toe. Voeg uw abonnementssleutels in de juiste definities in.

```javascript
const util = require('util');
const fs = require('fs');
const TrainingApiClient = require("@azure/cognitiveservices-customvision-training");
const PredictionApiClient = require("@azure/cognitiveservices-customvision-prediction");

const setTimeoutPromise = util.promisify(setTimeout);

const trainingKey = "<your training key>";
const predictionKey = "<your prediction key>";
const predictionResourceId = "<your prediction resource id>";
const sampleDataRoot = "<path to image files>";

const endPoint = "https://southcentralus.api.cognitive.microsoft.com"

const publishIterationName = "classifyModel";

const trainer = new TrainingApiClient(trainingKey, endPoint);

(async () => {
    console.log("Creating project...");
    const sampleProject = await trainer.createProject("Sample Project")
```

### <a name="create-tags-in-the-project"></a>Labels maken in het project

Voeg de volgende code toe aan het eind van *sample.js* om classificatielabels voor uw project te maken:

```javascript
    const hemlockTag = await trainer.createTag(sampleProject.id, "Hemlock");
    const cherryTag = await trainer.createTag(sampleProject.id, "Japanese Cherry");
```

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

Als u de voorbeeldafbeeldingen aan het project wilt toevoegen, voegt u de volgende code in nadat u de tag hebt gemaakt. Met deze code wordt elke afbeelding met de bijbehorende tag geüpload. U moet het basispad naar het afbeeldingsbestand invoeren, gebaseerd op waar u het project Cognitive Services Node.js-SDK-voorbeelden hebt gedownload.

> [!NOTE]
> U moet *sampleDataRoot* wijzigen in het pad naar de afbeeldingen, gebaseerd op waar u het project Cognitive Services Node.js-SDK-voorbeelden eerder hebt gedownload.

```javascript
    console.log("Adding images...");
    let fileUploadPromises = [];

    const hemlockDir = `${sampleDataRoot}/Hemlock`;
    const hemlockFiles = fs.readdirSync(hemlockDir);
    hemlockFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${hemlockDir}/${file}`), { tagIds: [hemlockTag.id] }));
    });

    const cherryDir = `${sampleDataRoot}/Japanese Cherry`;
    const japaneseCherryFiles = fs.readdirSync(cherryDir);
    japaneseCherryFiles.forEach(file => {
        fileUploadPromises.push(trainer.createImagesFromData(sampleProject.id, fs.readFileSync(`${cherryDir}/${file}`), { tagIds: [cherryTag.id] }));
    });

    await Promise.all(fileUploadPromises);
```

### <a name="train-the-classifier-and-publish"></a>De classificatie te trainen en publiceren

Deze code wordt de eerste versie in het project wordt gemaakt en vervolgens publiceert dat iteratie naar het eindpunt van de voorspelling. De naam van de gepubliceerde iteratie kan worden gebruikt om voorspelling aanvragen te verzenden. Een iteratie is niet beschikbaar in de voorspelling eindpunt totdat deze is gepubliceerd.

```javascript
    console.log("Training...");
    let trainingIteration = await trainer.trainProject(sampleProject.id);

    // Wait for training to complete
    console.log("Training started...");
    while (trainingIteration.status == "Training") {
        console.log("Training status: " + trainingIteration.status);
        await setTimeoutPromise(1000, null);
        trainingIteration = await trainer.getIteration(sampleProject.id, trainingIteration.id)
    }
    console.log("Training status: " + trainingIteration.status);
    
    // Publish the iteration to the end point
    await trainer.publishIteration(sampleProject.id, trainingIteration.id, publishIterationName, predictionResourceId);
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Halen en gebruik de gepubliceerde iteratie op het eindpunt voor de voorspelling

Als u een afbeelding naar het voorspellingseindpunt wilt verzenden en de voorspelling wilt ophalen, voegt u de volgende code toe aan het einde van het bestand:

```javascript
    const predictor = new PredictionApiClient(predictionKey, endPoint);
    const testFile = fs.readFileSync(`${sampleDataRoot}/Test/test_image.jpg`);

    const results = await predictor.classifyImage(sampleProject.id, publishIterationName, testFile);

    // Step 6. Show results
    console.log("Results:");
    results.predictions.forEach(predictedResult => {
        console.log(`\t ${predictedResult.tagName}: ${(predictedResult.probability * 100.0).toFixed(2)}%`);
    });
})()
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer *sample.js* uit.

```powershell
node sample.js
```

Als het goed is, is de uitvoer van de toepassing vergelijkbaar met de volgende tekst:

```
Creating project...
Adding images...
Training...
Training started...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Results:
         Hemlock: 94.97%
         Japanese Cherry: 0.01%
```

U kunt vervolgens controleren of de testafbeelding (in **<base_image_url>/Images/Test/** ) juist is getagd. U kunt altijd teruggaan naar de [Custom Vision-website](https://customvision.ai) en de huidige status bekijken van het nieuwe project dat u hebt gemaakt.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van het afbeeldingsclassificatieproces in code kan worden uitgevoerd. Met dit voorbeeld wordt één trainingsinteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](test-your-model.md)
