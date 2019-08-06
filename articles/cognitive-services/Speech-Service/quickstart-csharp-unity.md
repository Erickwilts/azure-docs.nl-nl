---
title: 'Quickstart: Spraak herkennen, unit-Speech-Service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een toepassing voor spraak-naar-tekst te maken met Unity en de Speech-SDK voor Unity (bètaversie). Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/24/2019
ms.author: jhakulin
ms.openlocfilehash: 1b6e60edd86cff2d657b562f05351e20571c0909
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815365"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Quickstart: Spraak herkennen met de Speech-SDK voor Unity (bètaversie)

Quick starts zijn ook beschikbaar voor [tekst naar spraak](quickstart-text-to-speech-csharp-unity.md).

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een toepassing voor spraak-naar-tekst te maken met [Unity](https://unity3d.com/) en de Speech-SDK voor Unity (bètaversie).
Wanneer u klaar bent, kunt u met uw apparaat praten om spraak naar tekst in realtime te transcriberen.
Als u niet bekend bent met Unity, raden we u aan om de [hand leiding](https://docs.unity3d.com/Manual/UnityManual.html) voor de Unity-gebruiker te bestuderen voordat u uw toepassing ontwikkelt.

> [!NOTE]
> De Speech-SDK voor Unity is momenteel alleen beschikbaar als bètaversie.
> Het ondersteunt Windows Desktop (x86 en x64) of Universeel Windows-platform (x86, x64, ARM/ARM64) en Android (x86, ARM32/64).

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

- [Unit 2018,3 of hoger](https://store.unity.com/) met [Unity 2019,1 voegt ondersteuning toe voor UWP ARM64](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Versie 15,9 of hoger van Visual Studio 2017 is ook acceptabel.
  - Voor ARM64-ondersteuning installeert u de [optionele build tools voor ARM64 en de Windows 10 SDK voor ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).
- Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
- Toegang tot de microfoon van uw computer.

## <a name="create-a-unity-project"></a>Een Unity-project maken

1. Eenheid openen. Als u unit voor de eerste keer gebruikt, wordt het venster **eenheids naaf** *<version number>* weer gegeven. (U kunt unit hub ook rechtstreeks openen om dit venster weer te geven.)

   [![Venster eenheids naaf](media/sdk/qs-csharp-unity-hub.png)](media/sdk/qs-csharp-unity-hub.png#lightbox)
1. Selecteer **Nieuw**. Het venster **een nieuw project met eenheid** *<version number>* maken wordt weer gegeven.

   [![Een nieuw project maken in Unity-hub](media/sdk/qs-csharp-unity-create-a-new-project.png)](media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. Voer in **project naam** **csharp eenheid**in.
1. In **sjablonen**, als **3D** niet al is geselecteerd, selecteert u deze.
1. In **locatie**selecteert of maakt u een map waarin u het project wilt opslaan.
1. Selecteer **Maken**.

Na een beetje tijd wordt het venster Unity editor weer gegeven.

## <a name="install-the-speech-sdk"></a>De Speech-SDK installeren

Voer de volgende stappen uit om de Speech SDK voor Unit te installeren:

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

1. Down load en open de [Speech SDK voor Unity (bèta)](https://aka.ms/csspeech/unitypackage), die wordt geleverd als een eenheids Asset-pakket (. unitypackage). Wanneer het Asset-pakket wordt geopend, wordt het dialoog venster **eenheids pakket importeren** weer gegeven.

   [![Het dialoog venster unit package-pakket importeren in de eenheids editor](media/sdk/qs-csharp-unity-01-import.png)](media/sdk/qs-csharp-unity-01-import.png#lightbox)
1. Zorg ervoor dat alle bestanden zijn geselecteerd en selecteer **importeren**. Na enkele ogen blikken wordt het eenheids Asset-pakket in uw project geïmporteerd.

Voor meer informatie over het importeren van Asset-pakketten in unit-eenheid raadpleegt u de [documentatie](https://docs.unity3d.com/Manual/AssetPackages.html)van Unity.

## <a name="add-ui"></a>Gebruikersinterface toevoegen

We gaan nu een minimale gebruikers interface toevoegen aan onze scène. Deze gebruikers interface bestaat uit een knop voor het activeren van spraak herkenning en een tekst veld om het resultaat weer te geven. In het [ **hiërarchie** venster](https://docs.unity3d.com/Manual/Hierarchy.html)wordt een voor beeld weer gegeven van een scène die is gemaakt met het nieuwe project.

1. Selecteer boven in het **hiërarchie** venster de**knop** **gebruikers interface** >  **maken** > .

   Met deze actie maakt u drie spel objecten die u kunt zien in het **hiërarchie** venster: een **knop** object , een canvas object dat de knop bevat en een **Event System** -object.

   [![Eenheids Editor-omgeving](media/sdk/qs-csharp-unity-editor-window.png)](media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [Navigeer door de **scène** weergave](https://docs.unity3d.com/Manual/SceneViewNavigation.html) zodat u een goede weer gave hebt van het canvas en de knop in de [ **scène** weergave](https://docs.unity3d.com/Manual/UsingTheSceneView.html).

1. Stel in [het venster **Inspector** ](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechter kant) de eigenschappen **POS X** en **POS Y** in op **0**, zodat de knop in het midden van het canvas wordt gecentreerd.

1. Selecteer in het venster **hiërarchie** de optie**gebruikers interface** > -**tekst** **maken** > om een **tekst** object te maken.

1. Stel in het venster **Inspector** de eigenschappen **POS X** en **POS Y** in op **0** en **120**en stel de eigenschappen **width** en **Height** in op **240** en **120**. Deze waarden zorgen ervoor dat het tekst veld en de knop elkaar niet overlappen.

Wanneer u klaar bent, moet de **scène** weergave er ongeveer als volgt uitzien:

[![Scène weergave in de eenheids editor](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

Voer de volgende stappen uit om de voorbeeld script code voor het Unity-project toe te voegen:

1. Selecteer in [het venster Project](https://docs.unity3d.com/Manual/ProjectView.html)de optie **C# script** **maken**  >  om een nieuw C# script toe te voegen.

   [![Project venster in de eenheids editor](media/sdk/qs-csharp-unity-project-window.png)](media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Naam van het script `HelloWorld`.

1. Dubbel klik `HelloWorld` om het zojuist gemaakte script te bewerken.

   > [!NOTE]
   > Als u de code-editor wilt configureren voor gebruik door eenheid voor bewerken, selecteert u**voor keuren** **bewerken** > en gaat u naar de voor keuren voor **externe hulpprogram ma's** . Zie voor meer informatie de [hand leiding voor Unity User](https://docs.unity3d.com/Manual/Preferences.html).

1. Vervang het bestaande script door de volgende code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Zoek en vervang de teken `YourSubscriptionKey` reeks door uw abonnement sleutel voor spraak Services.

1. Zoek en vervang de teken `YourServiceRegion` reeks door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla het gewijzigde script op.

Ga nu terug naar de eenheids editor en voeg het script als een onderdeel toe aan een van de spel objecten:

1. Selecteer in het venster **hiërarchie** het object **canvas** .

1. Selecteer de knop **onderdeel toevoegen** in het **item** venster.

   [![Inspector-venster in de eenheids editor](media/sdk/qs-csharp-unity-inspector-window.png)](media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Zoek in de vervolg keuzelijst naar het `HelloWorld` script dat u hierboven hebt gemaakt en voeg dit toe. Een **Hallo wereld sectie (script)** wordt weer gegeven in het **Inspector** -venster, met daarin twee niet-geïnitialiseerde eigenschappen, **uitvoer tekst** en de **knop & herstelpuntstatus starten**. De eigenschappen van de Unity-component komen `HelloWorld` overeen met de open bare eigenschappen van de klasse.

1. Selecteer de object kiezer van de eigenschap **Start & herstelpuntstatus** (het kleine cirkel pictogram rechts van de eigenschap) en kies het **knop** object dat u eerder hebt gemaakt.

1. Selecteer de object kiezer van de eigenschap **uitvoer tekst** en kies het **tekst** object dat u eerder hebt gemaakt.

   > [!NOTE]
   > De knop heeft ook een genest Text-object. Zorg ervoor dat u deze niet per ongeluk voor tekst uitvoer kiest (of wijzig de naam van een van de tekst objecten met behulp van het veld **naam** in het venster **controle** om Verwar ring te voor komen).

## <a name="run-the-application-in-the-unity-editor"></a>De toepassing uitvoeren in de Unity-editor

U bent nu klaar om de toepassing uit te voeren in de Unity-editor.

1. Selecteer in de werk balk Unity editor (onder de menu balk) de knop **afspelen** (een naar rechts wijzende drie hoek).

1. Ga naar de [ **Game** weergave](https://docs.unity3d.com/Manual/GameView.html)en wacht tot het **tekst** object wordt weer gegeven **Klik op de knop om spraak herkenning te herkennen**. (Er wordt **nieuwe tekst** weer gegeven wanneer de toepassing niet is gestart of niet gereed is om te reageren.)

1. Selecteer de knop en spreek een Engelse zin of zin in op de microfoon van uw computer. Uw spraak wordt verzonden naar de spraak Services en getranscribeerd naar tekst, die wordt weer gegeven in de **Game** weergave.

   [![Game weergave in de eenheids editor](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Controleer het [ **console** venster](https://docs.unity3d.com/Manual/Console.html) voor fout opsporing van berichten. Als het **console** venster niet wordt weer gegeven, gaat u naar de menu balk en selecteert u **venster** > **Algemeen** > **console** om deze weer te geven.

1. Wanneer u klaar bent met het herkennen van spraak, selecteert u de knop **afspelen** in de werk balk eenheids editor om de toepassing te stoppen.

## <a name="additional-options-to-run-this-application"></a>Extra opties voor het uitvoeren van deze toepassing

Deze toepassing kan ook worden geïmplementeerd als een Android-app, een zelfstandige Windows-app of een UWP-toepassing.
Zie voor meer informatie onze [voorbeeld opslagplaats](https://aka.ms/csspeech/samples). In `quickstart/csharp-unity` de map wordt de configuratie voor deze aanvullende doelen beschreven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Een model trainen voor Custom Speech](how-to-custom-speech-train-model.md)
