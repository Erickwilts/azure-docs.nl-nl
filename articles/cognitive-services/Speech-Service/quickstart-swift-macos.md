---
title: 'Quickstart: Spraak, snelle spraak service herkennen'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van spraak in Swift op macOS met behulp van de Speech SDK
services: cognitive-services
author: cbasoglu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/28/2019
ms.author: cbasoglu
ms.openlocfilehash: a952d6ea46f601cc0bc97e3b0af7343defe19f68
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559276"
---
# <a name="quickstart-recognize-speech-in-swift-on-macos-using-the-speech-sdk"></a>Quickstart: Spraak in Swift op macOS herkennen met de Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u in SWIFT een macOS-app kunt maken met behulp van de SDK van Cognitive Services speech om spraak opnamen van een microfoon naar tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een [abonnements sleutel](get-started.md) voor de spraak service.
* Een macOS-computer met [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) of hoger en [CocoaPods](https://cocoapods.org/) geïnstalleerd.

## <a name="get-the-speech-sdk-for-ios"></a>De Speech-SDK voor iOS ophalen

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.6.0`. Houd er rekening mee dat deze zelf studie niet werkt zonder wijzigingen voor een eerdere versie van de SDK.

De Cognitive Services Speech SDK voor macOS wordt gedistribueerd als een framework-bundel.
Het kan worden gebruikt in Xcode-projecten als een [CocoaPod](https://cocoapods.org/), of worden https://aka.ms/csspeech/macosbinary gedownload en hand matig worden gekoppeld. Deze hand leiding maakt gebruik van een CocoaPod.

## <a name="create-an-xcode-project"></a>Een Xcode-project maken

Start Xcode en start een nieuw project starten door te klikken op **File** > **New** > **Project**.
Kies in het dialoogvenster voor het selecteren van sjablonen de sjabloon 'iOS Single View App'.

Maak in de dialoogvensters die volgen de volgende selecties:

1. Het dialoogvenster Project Options
    1. Voer een naam in voor de snelstart-app, bijvoorbeeld `helloworld`.
    1. Voer de juiste organisatie naam en een organisatie-id in als u al een Apple-ontwikkelaars account hebt. Voor testdoeleinden kunt u elke naam kiezen, zoals `testorg`. Om de app te kunnen ondertekenen, hebt u een geschikt inrichtingsprofiel nodig. Raadpleeg de [Apple-site voor ontwikkelaars](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat SWIFT is gekozen als de taal voor het project.
    1. Schakel de selectie vakjes uit om Story boards te gebruiken en om een op documenten gebaseerde toepassing te maken. De eenvoudige gebruikers interface voor de voor beeld-app wordt programmatisch gemaakt.
    1. Schakel alle selectievakjes voor tests en essentiële gegevens uit.
1. Projectmap selecteren
    1. Kies een map waarin u het project wilt plaatsen. Hiermee maakt u `helloworld` een map in de gekozen map die alle bestanden voor het Xcode-project bevat.
    1. Schakel het maken van een Git-opslagplaats uit voor dit voorbeeldproject.
1. De app moet ook het gebruik van de microfoon in het `Info.plist` bestand declareren. Klik op het bestand in het overzicht en voeg de sleutel ' beschrijving privacy-microfoon gebruik ' toe, met een waarde als ' microfoon is vereist voor spraak herkenning '.
    ![Instellingen in info. plist](media/sdk/qs-swift-ios-info-plist.png)
1. Sluit het project Xcode. Later gebruikt u een ander exemplaar na het instellen van de CocoaPods.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Plaats een nieuw header-bestand met de `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` naam in `helloworld` de map in het HelloWorld-project en plak de volgende code in deze.  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Voeg het relatieve pad `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` naar de bridging-header toe aan de SWIFT-project instellingen voor het doel HelloWorld in de kop *-C bridging koptekst* veld ![koptekst eigenschappen](media/sdk/qs-swift-ios-bridging-header.png)
1. Vervang de inhoud van het automatisch gegenereerde bestand `AppDelegate.swift` door:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/AppDelegate.swift#code)]
1. Vervang de inhoud van het automatisch gegenereerde bestand `ViewController.swift` door:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/ViewController.swift#code)]
1. Vervang `ViewController.swift`in de teken reeks `YourSubscriptionKey` door de sleutel van uw abonnement.
1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="install-the-sdk-as-a-cocoapod"></a>De SDK installeren als een CocoaPod

1. Installeer de CocoaPod dependency manager zoals beschreven in de [installatie-instructies](https://guides.cocoapods.org/using/getting-started.html).
1. Navigeer naar de map van uw voor beeld-`helloworld`app (). Plaats een tekst bestand met de naam `Podfile` en de volgende inhoud in die map:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/Podfile)]
1. Ga naar de `helloworld` map in een Terminal en voer de opdracht `pod install`uit. Hiermee wordt een `helloworld.xcworkspace` Xcode-werk ruimte gegenereerd met zowel de voor beeld-app als de spraak-SDK als een afhankelijkheid. Deze werk ruimte wordt gebruikt in het volgende.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Open de `helloworld.xcworkspace` werk ruimte in Xcode.
1. Maak de foutopsporingsuitvoer zichtbaar (**View** > **Debug Area** > **Activate Console**).
1. Kies de IOS-simulator of een IOS-apparaat dat is verbonden met uw ontwikkel computer als bestemming voor de app uit de lijst in het menu van de **product** > **bestemming** .
1. Bouw de voorbeeldcode en voer deze uit in de iOS-simulator door **Product** > **Run** in het menu te selecteren of op de knop **Play** te klikken.
1. Nadat u op de knop ' herkennen ' in de app hebt geklikt en enkele woorden hebt gedicteerd, ziet u de tekst die u hebt gesp roken op het onderste deel van het scherm.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

