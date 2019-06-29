---
title: Apps ontwikkelen met de spraak-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van apps met behulp van de spraak-SDK.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: ae075dbc922932a4eaffd9126560c159d33459d0
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466964"
---
# <a name="ship-an-application"></a>Verzenden van een toepassing

Bekijk de [spraak SDK licentie](https://aka.ms/csspeech/license201809), evenals de [software van derden kennisgevingen](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html) wanneer u de Azure Cognitive Services Speech SDK distribueert. Bekijk ook de [privacyverklaring van Microsoft](https://aka.ms/csspeech/privacy).

Afhankelijk van het platform, er verschillende afhankelijkheden bestaan voor het uitvoeren van uw toepassing.

## <a name="windows"></a>Windows

De Cognitive Services Speech SDK is getest op Windows 10 en Windows Server 2016.

De Cognitive Services Speech SDK vereist de [Microsoft Visual C++ Redistributable voor Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) op het systeem. U kunt downloaden installatieprogramma's voor de nieuwste versie van de `Microsoft Visual C++ Redistributable for Visual Studio 2019` hier:

- [Win32](https://aka.ms/vs/16/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/16/release/vc_redist.x64.exe)

Als uw toepassing gebruikmaakt van beheerde code, de `.NET Framework 4.6.1` of hoger is vereist op de doelcomputer.

De Media Foundation-bibliotheken moeten worden geïnstalleerd voor de invoer van de microfoon. Deze bibliotheken maken deel uit van Windows 10 en Windows Server 2016. Het is mogelijk het gebruik van de spraak-SDK zonder deze bibliotheken, zolang een microfoon niet wordt gebruikt als de audio-invoerapparaat.

De vereiste spraak SDK-bestanden kunnen worden geïmplementeerd in dezelfde map als uw toepassing. Op deze manier uw toepassing hebben rechtstreeks toegang tot de bibliotheken. Zorg ervoor dat u selecteert de juiste versie (Win32/x64) die overeenkomt met uw toepassing.

| Name | Function
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core-SDK, vereist voor de systeemeigen en beheerde implementatie
| `Microsoft.CognitiveServices.Speech.csharp.dll` | vereist voor de beheerde implementatie

>[!NOTE]
> Vanaf de release 1.3.0 het bestand `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (geleverd in eerdere versies) wordt niet meer nodig hebt. De functionaliteit is nu geïntegreerd in de core-SDK.

>[!NOTE]
> Voor het Windows Forms-App (.NET Framework) C# project, zorg ervoor dat de bibliotheken zijn opgenomen in de implementatie-instellingen van uw project. U kunt dit onder controleren `Properties -> Publish Section`. Klik op de `Application Files` knop en de bijbehorende bibliotheken van de schuif omlaag in lijst vinden. Zorg ervoor dat de waarde is ingesteld op `Included`. Visual Studio bevat het bestand wanneer het project wordt gepubliceerd/geïmplementeerd.

## <a name="linux"></a>Linux

De spraak-SDK ondersteunt momenteel de Ubuntu 16.04, Ubuntu 18.04 en 9 van Debian-distributies.
Voor een systeemeigen toepassing, moet u voor het verzenden van de spraak-SDK-bibliotheek, `libMicrosoft.CognitiveServices.Speech.core.so`.
Zorg ervoor dat u selecteert u de versie (x86, x64) die overeenkomt met uw toepassing. Afhankelijk van de Linux-versie moet u ook mogelijk om op te nemen van de volgende afhankelijkheden:

* De gedeelde bibliotheken van de GNU C-bibliotheek (met inbegrip van de bibliotheek POSIX Threads Programming `libpthreads`)
* De OpenSSL-bibliotheek (`libssl.so.1.0.0` of `libssl.so.1.0.2`)
* De gedeelde bibliotheek voor ALSA toepassingen (`libasound.so.2`)

Op Ubuntu, moeten de bibliotheken GNU C al standaard worden geïnstalleerd. De laatste drie kan worden geïnstalleerd met behulp van deze opdrachten:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

Op Debian 9 deze pakketten te installeren:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
