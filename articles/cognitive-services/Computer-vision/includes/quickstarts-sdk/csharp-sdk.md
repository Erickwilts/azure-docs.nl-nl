---
title: 'Quickstart: Computer Vision-clientbibliotheek voor .NET'
description: Ga in deze quickstart aan de slag met de Computer Vision-clientbibliotheek voor .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 3ec3e44c667d6821c4a6dc0779a760b65de5046e
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321842"
---
<a name="HOLTop"></a>

[Referentiedocumentatie](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet) | [Broncode van bibliotheek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ComputerVision) | [Pakket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [Voorbeelden](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement - [Een gratis abonnement maken](https://azure.microsoft.com/free/cognitive-services/)
* De nieuwste versie van de [.NET Core SDK](https://dotnet.microsoft.com/download/).
* Zodra u een Azure-abonnement hebt, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Een Computer Vision-resource maken"  target="_blank">maakt u een Computer Vision-resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> in Azure Portal om uw sleutel en eindpunt op te halen. Nadat de app is geïmplementeerd, klikt u op **Ga naar resource**.
    * U hebt de sleutel en het eindpunt nodig van de resource die u maakt, om de toepassing te verbinden met de Computer Vision-service. Later in de quickstart plakt u uw sleutel en eindpunt in de onderstaande code.
    * U kunt de gratis prijscategorie (`F0`) gebruiken om de service uit te proberen, en later upgraden naar een betaalde laag voor productie.
* [Maak omgevingsvariabelen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor de sleutel- en eindpunt-URL, met respectievelijk de namen `COMPUTER_VISION_SUBSCRIPTION_KEY` en `COMPUTER_VISION_ENDPOINT`.

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-c-application"></a>Een nieuwe C#-toepassing maken

Maak een nieuwe .NET Core-toepassing in uw favoriete editor of IDE. 

Gebruik in een consolevenster (zoals cmd, PowerShell of Bash) de opdracht `dotnet new` om een nieuwe console-app te maken met de naam `computer-vision-quickstart`. Met deze opdracht maakt u een eenvoudig Hallo wereld-C#-project met één bronbestand: *ComputerVisionQuickstart.cs*.

```dotnetcli
dotnet new console -n computer-vision-quickstart
```

Wijzig uw map in de zojuist gemaakte app-map. U kunt de toepassing maken met:

```dotnetcli
dotnet build
```

De build-uitvoer mag geen waarschuwingen of fouten bevatten. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Open vanuit de projectmap het bestand *ComputerVisionQuickstart.cs* in uw favoriete editor of IDE. Voeg de volgende `using`-instructies toe:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_using)]

Maak in de klasse **Programma** van de toepassing variabelen voor het Azure-eindpunt en de Azure-sleutel van uw resource.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_vars)]

### <a name="install-the-client-library"></a>De clientbibliotheek installeren

Installeer in de toepassingsmap de Computer Vision-clientbibliotheek voor .NET met de volgende opdracht:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 6.0.0-preview.1
```

Als u de Visual Studio-IDE, gebruikt, is de clientbibliotheek beschikbaar als downloadbaar NuGet-pakket.

## <a name="object-model"></a>Objectmodel

De volgende klassen en interfaces verwerken enkele van de belangrijkste functies van de Computer Vision .NET SDK.

|Naam|Beschrijving|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet) | Deze klasse is nodig voor alle Computer Vision-functionaliteit. U instantieert deze klasse met uw abonnementsgegevens en gebruikt deze om de meeste afbeeldingsbewerkingen uit te voeren.|
|[ComputerVisionClientExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions?view=azure-dotnet)| Deze klasse bevat aanvullende methoden voor de **ComputerVisionClient**.|
|[VisualFeatureTypes](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)| Deze opsomming definieert de verschillende typen afbeeldingsanalyse die kunnen worden uitgevoerd in een standaard analysebewerking. U geeft een set VisualFeatureTypes-waarden op, afhankelijk van uw behoeften. |

## <a name="code-examples"></a>Codevoorbeelden

Deze codefragmenten laten zien hoe u de volgende taken kunt uitvoeren met de Computer Vision-clientbibliotheek voor .NET:

* [De client verifiëren](#authenticate-the-client)
* [Een afbeelding analyseren](#analyze-an-image)
* [Afgedrukte en handgeschreven tekst lezen](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>De client verifiëren

> [!NOTE]
> In deze quickstart wordt ervan uitgegaan dat u [omgevingsvariabelen hebt gemaakt](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor uw Computer Vision-sleutel en -eindpunt, respectievelijk met de naam `COMPUTER_VISION_SUBSCRIPTION_KEY` en `COMPUTER_VISION_ENDPOINT`.

Instantieer in een nieuwe methode een client met uw eindpunt en sleutel. Maak een **[ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.apikeyserviceclientcredentials?view=azure-dotnet)** -object met uw sleutel en maak hiermee en met uw eindpunt een **[ComputerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet)** -object.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_auth)]

Waarschijnlijk wilt u deze methode aanroepen in de `Main`-methode.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_client)]

## <a name="analyze-an-image"></a>Een afbeelding analyseren

Met de volgende code wordt een methode, `AnalyzeImageUrl`, gedefinieerd waarmee via het clientobject een externe afbeelding wordt geanalyseerd en de resultaten worden afgedrukt. Met de methode wordt een tekstbeschrijving, categorisatie, een lijst met tags, gedetecteerde gezichten, markeringen vanwege inhoud voor volwassenen, hoofdkleuren en afbeeldingstype geretourneerd.

Voeg de methodeaanroep toe aan de `Main`-methode.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyzeinmain)]

### <a name="set-up-test-image"></a>Testafbeelding instellen

Sla in uw klasse **Programma** een verwijzing op naar de URL van de afbeelding die u wilt analyseren.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyze_url)]

> [!NOTE]
> U kunt ook een lokale afbeelding analyseren. Zie de voorbeeldcode op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) voor scenario's met betrekking tot lokale afbeeldingen.

### <a name="specify-visual-features"></a>Visuele kenmerken opgeven

Definieer uw nieuwe methode voor het analyseren van afbeeldingen. Voeg de onderstaande code toe, waarmee u vervolgens visuele kenmerken opgeeft die u wilt extraheren in uw analyse. Zie de opsomming **[VisualFeatureTypes](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)** voor een volledige lijst.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_visualfeatures)]

Voeg een van de volgende codeblokken in uw **AnalyzeImageUrl**-methode in om de functies ervan te implementeren. Vergeet niet om aan het einde een afsluitend haakje toe te voegen.

```csharp
}
```

### <a name="analyze"></a>Analyseren

Met de **AnalyzeImageAsync**-methode wordt een **ImageAnalysis**-object geretourneerd dat alle geëxtraheerde informatie bevat.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_analyze_call)]

In de volgende secties ziet u hoe u deze gegevens uitgebreid kunt parseren.

### <a name="get-image-description"></a>Beschrijving van afbeelding ophalen

Met de volgende code wordt de lijst met gegenereerde bijschriften voor de afbeelding opgehaald. Zie [Afbeeldingen beschrijven](../../concept-describing-images.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_describe)]

### <a name="get-image-category"></a>Categorie van de afbeelding ophalen

Met de volgende code wordt de gedetecteerde categorie van de afbeelding opgehaald. Zie [Afbeeldingen categoriseren](../../concept-categorizing-images.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_categorize)]

### <a name="get-image-tags"></a>Afbeeldingstags ophalen

Met de volgende code wordt de set met gedetecteerde tags in de afbeelding opgehaald. Zie [Inhoudstags](../../concept-tagging-images.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_tags)]

### <a name="detect-objects"></a>Objecten detecteren

Met de volgende code worden algemene objecten in de afbeelding gedetecteerd en worden deze in de console afgedrukt. Zie [Objectdetectie](../../concept-object-detection.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_objects)]

### <a name="detect-brands"></a>Merken detecteren

Met de volgende code worden bedrijfsmerken en -logo's in de afbeelding gedetecteerd en worden deze in de console afgedrukt. Zie [Merkdetectie](../../concept-brand-detection.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_brands)]

### <a name="detect-faces"></a>Gezichten detecteren

Met de volgende code worden de gedetecteerde gezichten in de afbeelding met hun rechthoekcoördinaten als resultaat gegeven en worden gezichtskenmerken geselecteerd. Zie [Gezichtsdetectie](../../concept-detecting-faces.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_faces)]

### <a name="detect-adult-racy-or-gory-content"></a>Inhoud voor volwassenen, of ongepaste of bloederige inhoud detecteren

Met de volgende code wordt de gedetecteerde aanwezigheid van inhoud voor volwassenen afgedrukt in de afbeelding. Zie [Inhoud voor volwassenen, of ongepaste, bloederige inhoud](../../concept-detecting-adult-content.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Kleurenschema van de afbeelding ophalen

Met de volgende code worden de gedetecteerde kleurkenmerken in de afbeelding afgedrukt, zoals de dominante kleuren en accentkleur. Zie [Kleurenschema's](../../concept-detecting-color-schemes.md) voor meer informatie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Domeinspecifieke inhoud ophalen

Computer Vision kan speciale modellen gebruiken om verdere analyse van afbeeldingen uit te voeren. Zie [Domeinspecifieke inhoud](../../concept-detecting-domain-content.md) voor meer informatie. 

Met de volgende code worden gegevens over gedetecteerde beroemdheden in de afbeelding geparseerd.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_celebs)]

Met de volgende code worden gegevens over gedetecteerde bezienswaardigheden in de afbeelding geparseerd.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Het afbeeldingstype ophalen

Met de volgende code wordt informatie over het type afbeelding afgedrukt&mdash;, of het nu een illustratie of lijntekening is.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Afgedrukte en handgeschreven tekst lezen

Computer Vision kan zichtbare tekst in een afbeelding lezen en deze converteren naar een tekenstroom. Zie voor meer informatie over tekstherkenning het conceptuele document [Optical Character Recognition (OCR)](../../concept-recognizing-text.md#read-api). De code in deze sectie maakt gebruik van de nieuwste [release van de Computer Vision-SDK voor Read 3.0](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/6.0.0-preview.1) en definieert een methode, `BatchReadFileUrl`, die gebruikmaakt van het clientobject om tekst in de afbeelding te detecteren en extraheren.

Voeg de methodeaanroep toe aan de `Main`-methode.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_extracttextinmain)]

### <a name="set-up-test-image"></a>Testafbeelding instellen

Sla in uw klasse **Programma** een verwijzing op naar de URL van de afbeelding waaruit u tekst wilt extraheren. Dit codefragment bevat voorbeeldafbeeldingen van gedrukte en handgeschreven tekst.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_readtext_url)]

> [!NOTE]
> U kunt ook tekst extraheren uit een lokale afbeelding. Zie de voorbeeldcode op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) voor scenario's met betrekking tot lokale afbeeldingen.

### <a name="call-the-read-api"></a>De Read-API aanroepen

Definieer de nieuwe methode voor het lezen van tekst. Voeg de onderstaande code toe, die de methode **ReadAsync** aanroept voor de gegeven afbeelding. Hiermee wordt een bewerkings-id geretourneerd en wordt een asynchroon proces gestart om de inhoud van de afbeelding te lezen.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_read_url)]

### <a name="get-read-results"></a>Leesresultaten ophalen

Vervolgens haalt u de bewerkings-id op die wordt geretourneerd vanaf de **ReadAsync**-aanroep en gebruikt u deze om de service te doorzoeken op de resultaten van de bewerking. Met de volgende code wordt de bewerking gecontroleerd totdat de resultaten worden geretourneerd. Vervolgens worden de geëxtraheerde tekstgegevens afgedrukt naar de console.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_read_response)]

### <a name="display-read-results"></a>Leesresultaten weergeven

Voeg de volgende code toe om de opgehaalde tekstgegevens te parseren en weer te geven, en voltooi de methodedefinitie.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/ComputerVision/ComputerVisionQuickstart.cs?name=snippet_read_display)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing op vanuit uw toepassingsmap met de opdracht `dotnet run`.

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u een Cognitive Services-abonnement wilt opschonen en verwijderen, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook alle bijbehorende resources verwijderd.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Referentie voor de Computer Vision-API (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet)

* [Wat is Computer Vision?](../../overview.md)
* De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs).
