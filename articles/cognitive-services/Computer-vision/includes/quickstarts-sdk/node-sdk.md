---
title: 'Quickstart: Computer Vision-clientbibliotheek voor Node.js'
description: Ga met deze quickstart aan de slag met de Computer Vision-clientbibliotheek voor Node.js
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 01/22/2020
ms.author: pafarley
ms.custom: devx-track-javascript
ms.openlocfilehash: dc01600d052752d4558e22fe5c5ca48fb84a8121
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405485"
---
<a name="HOLTop"></a>

[Referentiedocumentatie](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) | [Bibliotheekbroncode](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-computervision) | [Pakket (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) | [Voorbeelden](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement - [Een gratis abonnement maken](https://azure.microsoft.com/free/cognitive-services/)
* De huidige versie van [Node.js](https://nodejs.org/)
* Zodra u een Azure-abonnement hebt, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title="Een Computer Vision-resource maken"  target="_blank">maakt u een Computer Vision-resource <span class="docon docon-navigate-external x-hidden-focus"></span></a> in Azure Portal om uw sleutel en eindpunt op te halen. Nadat de app is geïmplementeerd, klikt u op **Ga naar resource**.
    * U hebt de sleutel en het eindpunt nodig van de resource die u maakt, om de toepassing te verbinden met de Computer Vision-service. Later in de quickstart plakt u uw sleutel en eindpunt in de onderstaande code.
    * U kunt de gratis prijscategorie (`F0`) gebruiken om de service uit te proberen, en later upgraden naar een betaalde laag voor productie.
* [Maak omgevingsvariabelen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor de sleutel- en eindpunt-URL, met respectievelijk de namen `COMPUTER_VISION_SUBSCRIPTION_KEY` en `COMPUTER_VISION_ENDPOINT`.

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-nodejs-application"></a>Een nieuwe Node.js-toepassing maken

Maak in een consolevenster (zoals cmd, PowerShell of Bash) een nieuwe map voor de app, en navigeer naar deze map.

```console
mkdir myapp && cd myapp
```

Voer de opdracht `npm init` uit om een knooppunttoepassing te maken met een `package.json`-bestand.

```console
npm init
```

### <a name="install-the-client-library"></a>De clientbibliotheek installeren

Installeer als volgt de NPM-pakketten `ms-rest-azure` en `@azure/cognitiveservices-computervision`:

```console
npm install @azure/cognitiveservices-computervision
```

Het `package.json`-bestand van uw app wordt bijgewerkt met de afhankelijkheden.

### <a name="prepare-the-nodejs-script"></a>Het Node.js-script voorbereiden

Maak een nieuw bestand, *index.js* en open dit in een tekstbewerkingsprogramma. Voeg de volgende importeerinstructies toe.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imports)]

Definieer vervolgens een functie `computerVision` en declareer een asynchrone serie met een primaire functie en een callback-functie. U voegt uw quickstartcode toe aan de primaire functie en roept `computerVision`, onder aan het script, aan.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_begin)]

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_functiondef_end)]

## <a name="object-model"></a>Objectmodel

De volgende klassen en interfaces verwerken enkele van de belangrijkste functies van de Computer Vision-SDK voor Node.js.

|Naam|Beschrijving|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) | Deze klasse is nodig voor alle Computer Vision-functionaliteit. U instantieert deze klasse met uw abonnementsgegevens en gebruikt deze om de meeste afbeeldingsbewerkingen uit te voeren.|
|[VisualFeatureTypes](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/visualfeaturetypes?view=azure-node-latest)| Deze opsomming definieert de verschillende typen afbeeldingsanalyse die kunnen worden uitgevoerd in een standaard analysebewerking. U geeft een set **VisualFeatureTypes**-waarden op, afhankelijk van uw behoeften. |

## <a name="code-examples"></a>Codevoorbeelden

Deze codefragmenten laten zien hoe u de volgende taken kunt uitvoeren met de Computer Vision-clientbibliotheek voor Node.js:

* [De client verifiëren](#authenticate-the-client)
* [Een afbeelding analyseren](#analyze-an-image)
* [Afgedrukte en handgeschreven tekst lezen](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>De client verifiëren

Maak variabelen voor het Azure-eindpunt en de Azure-sleutel voor uw resource. Als u de omgevingsvariabele hebt gemaakt nadat u de toepassing hebt gestart, moet u de editor, IDE of shell waarmee deze wordt uitgevoerd, sluiten en opnieuw openen om toegang te krijgen tot de variabele.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_vars)]

Instantieer een client met uw eindpunt en sleutel. Maak een [ApiKeyCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.apikeycredentials?view=azure-python)-object met uw sleutel en eindpunt en maak hiermee een [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest)-object.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_client)]

## <a name="analyze-an-image"></a>Een afbeelding analyseren

De code in deze sectie wordt gebruikt om externe afbeeldingen te analyseren om verschillende visuele functies te extraheren. U kunt deze bewerkingen uitvoeren als onderdeel van de **analyzeImage**-methode van het clientobject, of u kunt ze aanroepen met behulp van afzonderlijke methoden. Raadpleeg de [referentiedocumentatie](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) voor meer informatie.

> [!NOTE]
> U kunt ook een lokale afbeelding analyseren. Zie de voorbeeldcode op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) voor scenario's met betrekking tot lokale afbeeldingen.

### <a name="get-image-description"></a>Beschrijving van afbeelding ophalen

Met de volgende code wordt de lijst met gegenereerde bijschriften voor de afbeelding opgehaald. Zie [Afbeeldingen beschrijven](../../concept-describing-images.md) voor meer informatie.

Definieer eerst de URL van de afbeelding die u wilt analyseren:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe_image)]

Voeg daarna de volgende code toe om de afbeeldingsbeschrijving op te halen en deze via de console af te drukken.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_describe)]

### <a name="get-image-category"></a>Categorie van de afbeelding ophalen

Met de volgende code wordt de gedetecteerde categorie van de afbeelding opgehaald. Zie [Afbeeldingen categoriseren](../../concept-categorizing-images.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories)]

Definieer de hulpfunctie `formatCategories`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_categories_format)]

### <a name="get-image-tags"></a>Afbeeldingstags ophalen

Met de volgende code wordt de set met gedetecteerde tags in de afbeelding opgehaald. Zie [Inhoudstags](../../concept-tagging-images.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tags)]

Definieer de hulpfunctie `formatTags`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_tagsformat)]

### <a name="detect-objects"></a>Objecten detecteren

Met de volgende code worden algemene objecten in de afbeelding gedetecteerd en worden deze in de console afgedrukt. Zie [Objectdetectie](../../concept-object-detection.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objects)]

Definieer de hulpfunctie `formatRectObjects`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_objectformat)]

### <a name="detect-brands"></a>Merken detecteren

Met de volgende code worden bedrijfsmerken en -logo's in de afbeelding gedetecteerd en worden deze in de console afgedrukt. Zie [Merkdetectie](../../concept-brand-detection.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_brands)]

### <a name="detect-faces"></a>Gezichten detecteren

Met de volgende code worden de gedetecteerde gezichten in de afbeelding met hun rechthoekcoördinaten als resultaat gegeven en worden gezichtskenmerken geselecteerd. Zie [Gezichtsdetectie](../../concept-detecting-faces.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_faces)]

Definieer de hulpfunctie `formatRectFaces`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_formatfaces)]

### <a name="detect-adult-racy-or-gory-content"></a>Inhoud voor volwassenen, of ongepaste of bloederige inhoud detecteren

Met de volgende code wordt de gedetecteerde aanwezigheid van inhoud voor volwassenen afgedrukt in de afbeelding. Zie [Inhoud voor volwassenen, of ongepaste, bloederige inhoud](../../concept-detecting-adult-content.md) voor meer informatie.

Definieer de URL van de afbeelding die u wilt gebruiken:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult_image)]

Voeg vervolgens de volgende code toe om inhoud voor volwassenen te detecteren en de resultaten af te drukken via de console.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Kleurenschema van de afbeelding ophalen

Met de volgende code worden de gedetecteerde kleurkenmerken in de afbeelding afgedrukt, zoals de dominante kleuren en accentkleur. Zie [Kleurenschema's](../../concept-detecting-color-schemes.md) voor meer informatie.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors)]

Definieer de hulpfunctie `printColorScheme` om de details van het kleurenschema via de console af te drukken.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_colors_print)]

### <a name="get-domain-specific-content"></a>Domeinspecifieke inhoud ophalen

Computer Vision kan een speciaal model gebruiken om verdere analyse van afbeeldingen uit te voeren. Zie [Domeinspecifieke inhoud](../../concept-detecting-domain-content.md) voor meer informatie.

Definieer eerst de URL van de afbeelding die u wilt analyseren:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_domain_image)]

Met de volgende code worden gegevens over gedetecteerde bezienswaardigheden in de afbeelding geparseerd.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks)]

Definieer de hulpfunctie `formatRectDomain` om de locatiegegevens over gedetecteerde bezienswaardigheden te parseren.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_landmarks_rect)]

### <a name="get-the-image-type"></a>Het afbeeldingstype ophalen

Met de volgende code wordt informatie over het type afbeelding afgedrukt&mdash;, of het een illustratie of lijntekening is.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype)]

Definieer de hulpfunctie `describeType`:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_imagetype_describe)]

## <a name="read-printed-and-handwritten-text"></a>Afgedrukte en handgeschreven tekst lezen

Computer Vision kan zichtbare tekst in een afbeelding lezen en deze converteren naar een tekenstroom.

> [!NOTE]
> U kunt ook tekst lezen uit een lokale afbeelding. Zie de voorbeeldcode op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) voor scenario's met betrekking tot lokale afbeeldingen.

### <a name="set-up-test-images"></a>Testafbeeldingen instellen

Sla een verwijzing op van de URL van de afbeeldingen waaruit u tekst wilt extraheren.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_images)]

### <a name="call-the-recognize-api"></a>De Recognizer-API aanroepen

Voeg de onderstaande code toe. Hiermee wordt de `recognizeText`-functie aangeroepen voor de opgegeven afbeeldingen.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_call)]

Definieer de `recognizeText`-functie. Hiermee roept u de **recognizeText**-methode aan op het clientobject en wordt een bewerkings-id geretourneerd. Ook wordt een asynchroon proces gestart om de inhoud van de afbeelding te lezen. Vervolgens wordt de bewerkings-id gebruikt om de bewerking te controleren met een interval van één seconde totdat de resultaten worden geretourneerd. Vervolgens worden de geëxtraheerde resultaten geretourneerd.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_helper)]

Definieer nu de hulpfunctie `printRecText`, waarmee de resultaten van een herkenbewerking worden afgedrukt via de console.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/ComputerVision/ComputerVisionQuickstart.js?name=snippet_read_print)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de opdracht `node` in uw quickstart-bestand.

```console
node index.js
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u een Cognitive Services-abonnement wilt opschonen en verwijderen, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook alle bijbehorende resources verwijderd.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
>[Referentie voor de Computer Vision-API (Node.js)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)

* [Wat is Computer Vision?](../../Home.md)
* De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js).
