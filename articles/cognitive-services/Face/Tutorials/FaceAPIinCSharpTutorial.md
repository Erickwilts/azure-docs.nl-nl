---
title: 'Zelfstudie: Gegevens van gezichten in een afbeelding detecteren en weergeven met behulp van de .NET SDK'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie maakt u een Windows-app die gebruikmaakt van de Face-API om gezichten in een afbeelding te herkennen en omlijsten.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 54069fbaa8ad06d257ab835ed3b170fecb76d800
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603348"
---
# <a name="tutorial-create-a-wpf-app-to-display-face-data-in-an-image"></a>Zelfstudie: Een WPF-app maken om gegevens van gezichten in een afbeelding weer te geven

In deze zelfstudie leert u hoe u met de Face-API van Azure, via de .NET client SDK, Detecteer gezichten in een afbeelding worden gebruikt en die gegevens in de gebruikersinterface. U maakt een Windows Presentation Framework (WPF)-toepassing die gezichten gedetecteerd, tekent een kader rond elk gezicht en een beschrijving van het gezicht wordt weergegeven in de statusbalk. 

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> - Een WPF-toepassing maken
> - De clientbibliotheek van de Face-API installeren
> - De clientbibliotheek gebruiken om gezichten in een afbeelding te detecteren
> - Een kader rond elk gedetecteerd gezicht tekenen
> - Een beschrijving van het gemarkeerde gezicht op de statusbalk weergeven

![Schermafbeelding met gedetecteerde gezichten omlijst door rechthoeken](../Images/getting-started-cs-detected.png)

De volledige voorbeeldcode is beschikbaar in de opslagplaats [Cognitive Face CSharp sample](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) (Cognitive Face CSharp-voorbeeld) op GitHub.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint. 


## <a name="prerequisites"></a>Vereisten

- Een Face-API-abonnementssleutel. U kunt een abonnementssleutel voor een gratis proefversie downloaden van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op de Face-API-service en uw sleutel op te halen.
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

Volg deze stappen voor het maken van een nieuw WPF-toepassingsproject.

1. Open het dialoogvenster Nieuw project in Visual Studio. Vouw **Geïnstalleerd** en vervolgens **Visual C#** uit en selecteer **WPF App (.NET Framework)** .
1. Noem de toepassing **FaceTutorial** en klik vervolgens op **OK**.
1. Download de vereiste NuGet-pakketten. Klik met de rechtermuisknop op uw project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Zoek en installeer vervolgens het volgende pakket:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-the-initial-code"></a>De initiële code toevoegen

In deze sectie voegt u het basisframework van de app toe zonder de gezichtsbepalende kenmerken.

### <a name="create-the-ui"></a>Gebruikersinterface maken

Open *MainWindow.xaml* en vervang de inhoud door de volgende code&mdash;deze code maakt de UI-venster. De `FacePhoto_MouseMove` en `BrowseButton_Click` methoden zijn gebeurtenis-handlers die u later wilt definiëren.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?range=1-18)]

### <a name="create-the-main-class"></a>Hoofdklasse maken

Open *MainWindow.xaml.cs* en voeg de naamruimten van de clientbibliotheek te, samen met de overige noodzakelijke naamruimten. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=1-12)]

Voeg vervolgens de volgende code in de klasse **MainWindow** in. Deze code maakt een **FaceClient** instantie met de abonnementssleutel, die u zelf moet invoeren. U moet de tekenreeks regio instellen `faceEndpoint` naar de juiste regio voor uw abonnement (Zie de [Face-API-documenten](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor een lijst van alle eindpunten voor de regio).

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=18-46)]

Plak vervolgens de volgende code in de methode **MainWindow**.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=50-61)]

Voeg tot slot de methoden **BrowseButton_Click** en **FacePhoto_MouseMove** aan de klasse toe. Deze methoden komen overeen met de gebeurtenis-handlers gedeclareerd in *MainWindow.xaml*. Met de methode **BrowseButton_Click** wordt het dialoogvenster **OpenFileDialog** gemaakt, waarmee de gebruiker een JPG-afbeelding kan selecteren. Daarna wordt de afbeelding in het hoofdvenster weergegeven. U voegt de resterende code voor **BrowseButton_Click** en **FacePhoto_MouseMove** later toe. Kijk ook naar de verwijzing `faceList`, een lijst met **DetectedFace**-objecten. Deze referentie is waar uw app wordt opgeslagen en aanroepen van de werkelijke face-gegevens.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=64-90,146)]

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=148-150,187)]

### <a name="try-the-app"></a>De app uitproberen

Druk op **Start** in het menu om uw app te testen. Als het app-venster wordt geopend, klikt u in de linkerbenedenhoek op **Bladeren**. Het dialoogvenster **Bestand openen** verschijnt. Selecteer een afbeelding in uw bestandssysteem en controleer of deze in het venster wordt weergegeven. Sluit vervolgens de app en ga door naar de volgende stap.

![Schermafbeelding met ongewijzigde afbeelding van gezichten](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Afbeelding uploaden en gezichten detecteren

Uw app detecteert gezichten door de methode **FaceClient.Face.DetectWithStreamAsync** aan te roepen. Deze verpakt de [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-REST API voor het uploaden van een lokale afbeelding.

Voeg de volgende methode in de klasse **MainWindow** in, onder de methode **FacePhoto_MouseMove**. Deze methode definieert een lijst met face kenmerken om op te halen en leest het ingediende installatiekopiebestand in een **Stream**. Vervolgens worden beide objecten doorgegeven aan de methodeaanroep **DetectWithStreamAsync**.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=189-226)]

## <a name="draw-rectangles-around-faces"></a>Rechthoeken rond gezichten tekenen

Vervolgens voegt u de code toe om een rechthoek rond elk gedetecteerd gezicht in de afbeelding te tekenen. Voeg in de klasse **MainWindow** de volgende code toe aan het eind van de methode **BrowseButton_Click**, na de regel `FacePhoto.Source = bitmapSource`. Deze code wordt een lijst met gedetecteerde gezichten van de aanroep aan **UploadAndDetectFaces**. Vervolgens wordt een rechthoek rond elk gezicht getekend en de aangepaste afbeelding weergegeven in het hoofdvenster.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=92-145)]

## <a name="describe-the-faces"></a>De gezichten beschrijven

Voeg de volgende methode toe aan de klasse **MainWindow**, onder de methode **UploadAndDetectFaces**. Deze methode zet de opgehaalde face-kenmerken in een tekenreeks met een beschrijving van het gezicht.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=228-286)]

## <a name="display-the-face-description"></a>De beschrijving van het gezicht weergeven

Voeg de volgende code toe aan de methode **FacePhoto_MouseMove**. Deze gebeurtenis-handler geeft de tekenreeks met de beschrijving van het gezicht in `faceDescriptionStatusBar` weer als de cursor boven een gedetecteerd vierkant rond het gezicht wordt geplaatst.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=151-186)]


## <a name="run-the-app"></a>De app uitvoeren

Voer de toepassing uit en blader naar een afbeelding met een gezicht. Wacht enkele seconden, zodat de Face-service kan reageren. U zou nu een rode rechthoek rond de gezichten in de afbeelding moeten zien. Als u de muisaanwijzer boven een gezichtsrechthoek beweegt, moet de beschrijving van dat gezicht op de statusbalk worden weergegeven.

![Schermafbeelding met gedetecteerde gezichten omlijst door rechthoeken](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het basisproces geleerd voor het gebruik van de .NET-SDK van de Face-service en een toepassing gemaakt om gezichten in een afbeelding te detecteren en omlijsten. Meer informatie over de details van gezichtsdetectie.

> [!div class="nextstepaction"]
> [Gezichten in afbeeldingen detecteren](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
