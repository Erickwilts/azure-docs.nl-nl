---
title: 'Quickstart: Water, C# (.NET Core) - spraakservices'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C# onder .NET Core in Windows of macOS met behulp van de spraak-SDK
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: 873145cf9d418433ba241ce06d7d594fb3e6322b
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465723"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-core"></a>Quickstart: Spraak herkennen met de Speech-SDK voor .NET Core

Snelstartgidsen zijn ook beschikbaar voor [tekst naar spraak](quickstart-text-to-speech-dotnetcore.md) en [spraakomzetting](quickstart-translate-speech-dotnetcore-windows.md).

Indien gewenst, kies een andere programmeertaal en/of de omgeving:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C# consoletoepassing voor .NET Core in Windows of macOS met behulp van de Cognitive Services [spraak SDK](speech-sdk.md). Doel is om realtime spraak naar tekst om te zetten vanuit de microfoon van uw pc. De toepassing is gemaakt met het [NuGet-pakket van de Speech SDK](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> .NET Core is een open-source, platformoverschrijdend .NET-platform waarmee de [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-specificatie wordt geïmplementeerd.

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [.NET Core-SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang daarin alle code door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Oplossing bouwen gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Fouten opsporen** > **Foutopsporing starten** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met optie Foutopsporing starten gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Start foutopsporing voor de app")

1. Een consolevenster wordt weergegeven, waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. Uw stem is verzonden naar de Services voor spraak en getranscribeerde tekst, die in hetzelfde venster wordt weergegeven.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
