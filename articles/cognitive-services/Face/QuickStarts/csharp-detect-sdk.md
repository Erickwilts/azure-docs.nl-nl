---
title: 'Quickstart: Gezichten in een afbeelding detecteren met de Azure Face .NET SDK'
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u de Azure Face SDK met C# om gezichten in een afbeelding te detecteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: bf4d11a18932d20e7dcc7580ebe5aa4e060c5a88
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606780"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-net-sdk"></a>Quickstart: Gezichten in een afbeelding detecteren met behulp van de Face .NET SDK

In deze snelstart gebruikt u de SDK van de Face-service met C# om menselijke gezichten in een afbeelding te detecteren. Voor een werkvoorbeeld van de code in deze snelstart gaat u naar het Face-project in de opslagplaats [Cognitive Services Vision csharp quickstarts](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face) in GitHub.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

- Een Face-API-abonnementssleutel. U kunt een abonnementssleutel voor een gratis proefversie downloaden van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op de Face-API-service en uw sleutel op te halen.
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

1. Maak in Visual Studio een nieuw **Console-app (.NET Framework)** -project en noem het **FaceDetection**. 
1. Als uw oplossing nog meer projecten bevat, selecteert u deze als het enkele opstartproject.
1. Download de vereiste NuGet-pakketten. Klik met de rechtermuisknop op het project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en selecteer **voorlopige release opnemen**. Zoek en installeer vervolgens het volgende pakket:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)
1. Zorg ervoor dat u hebt de nieuwste versies van alle NuGet-pakketten geïnstalleerd voor uw project. Klik met de rechtermuisknop op het project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Klik op de **Updates** tabblad en installeer de nieuwste versies van alle pakketten die worden weergegeven.

## <a name="add-face-detection-code"></a>Gezichtsdetectiecode toevoegen

Open het bestand *Program.cs* van het nieuwe project. Hier voegt u de code toe voor het laden van afbeeldingen en detecteren van gezichten.

### <a name="include-namespaces"></a>Naamruimten opnemen

Voeg aan het begin van het bestand *Program.cs* de volgende `using`-instructies toe.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=1-7)]

### <a name="add-essential-fields"></a>Essentiële velden toevoegen

Voeg de **programma** klasse met de volgende velden. Met deze gegevens wordt aangegeven hoe de Face-service moet worden verbonden en waar de invoergegevens kunnen worden opgehaald. U moet het veld `subscriptionKey` bijwerken met de waarde van de abonnementssleutel en mogelijk moet u de tekenreeks `faceEndpoint` wijzigen, zodat deze de juiste regio-id bevat. U moet ook de waarden `localImagePath` en/of `remoteImageUrl` instellen op paden die verwijzen naar de werkelijke afbeeldingsbestanden.

Het veld `faceAttributes` is gewoon een matrix met bepaalde kenmerktypen. Dit veld geeft aan welke informatie over de gedetecteerde gezichten moet worden opgehaald.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=9-34)]

### <a name="create-and-use-the-face-client"></a>De Face-client maken en gebruiken

Voeg vervolgens de **Main** -methode van de **programma** klasse met de volgende code. Hierdoor wordt een Face-API-client ingesteld.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=36-41)]

Ook in de **Main** methode, voeg de volgende code voor het gebruik van de zojuist gemaakte Face-client voor het detecteren van gezichten in een lokale en externe-installatiekopie. Hierna worden de detectiemethoden gedefinieerd. 

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=43-50)]

### <a name="detect-faces"></a>Gezichten detecteren

Voeg de volgende methode toe aan de klasse **Program**. Met deze methode wordt de Face-serviceclient gebruikt om gezichten te detecteren in een externe afbeelding, waarnaar wordt verwezen via een URL. Hierbij de `faceAttributes` veld&mdash;de **DetectedFace** objecten die zijn toegevoegd aan `faceList` heeft de opgegeven kenmerken (in dit geval, leeftijd en geslacht).

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=52-74)]

Voeg op dezelfde manier de methode **DetectLocalAsync** toe. Met deze methode wordt de Face-serviceclient gebruikt om gezichten te detecteren in een lokale afbeelding, waarnaar wordt verwezen via een bestandspad.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=76-101)]

### <a name="retrieve-and-display-face-attributes"></a>Gezichtskenmerken ophalen en weergeven

Definieer vervolgens de methode **GetFaceAttributes**. Hiermee wordt een tekenreeks geretourneerd met de relevante kenmerkgegevens.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=103-116)]

Definieer tot slot de methode **DisplayAttributes** om de gegevens van de gezichtskenmerken naar de console-uitvoer te schrijven. Vervolgens kunt u sluiten de klasse en de naamruimte.

[!code-csharp[](~/cognitive-services-vision-csharp-sdk-quickstarts/Face/Program.cs?range=118-125)]

## <a name="run-the-app"></a>De app uitvoeren

Een geslaagd antwoord retourneert het geslacht en de leeftijd van elk gezicht in de afbeelding. Bijvoorbeeld:

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudige .NET-consoletoepassing gemaakt, die gebruik kan maken van de service Face-API om gezichten te detecteren in zowel lokale als externe afbeeldingen. Volg hierna een meer gedetailleerde zelfstudie om te zien hoe u informatie over gezichten op een intuïtieve manier kunt presenteren aan de gebruiker.

> [!div class="nextstepaction"]
> [Zelfstudie: Een WPF-app maken om gezichten in een afbeelding te detecteren en te analyseren](../Tutorials/FaceAPIinCSharpTutorial.md)
