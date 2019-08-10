---
title: 'Quick Start: een Xamarin Android-app maken met ruimtelijke ankers van Azure | Microsoft Docs'
description: In deze Quick Start leert u hoe u een Android-app kunt bouwen met Xamarin met behulp van ruimtelijke ankers.
author: craigktreasure
manager: virivera
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 0958961f1e66be130dd2be816f6002dd34465dc6
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931656"
---
# <a name="quickstart-create-a-xamarin-android-app-with-azure-spatial-anchors"></a>Quickstart: Een Xamarin Android-app maken met ruimtelijke ankers van Azure

In deze Quick Start wordt beschreven hoe u een Android-app met Xamarin maakt met behulp van [Azure spatiale ankers](../overview.md). Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Wanneer u klaar bent, hebt u een Android-app die een ruimtelijk anker kan opslaan en intrekken.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Spatial Anchors-account-id en -accountsleutel configureren
> * Implementeren en uitvoeren op een Android-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:
- Een Windows-of macOS-computer:
  - Als u Windows gebruikt:
    - Een up-to-date versie van <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019 16.2 +</a>.
    - <a href="https://git-scm.com/download/win" target="_blank">Git voor Windows</a>
  - Als macOS wordt gebruikt:
    - Een up-to-date versie van <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio voor Mac 8.1 +</a>.
    - <a href="https://git-scm.com/download/mac" target="_blank">Git voor macOS</a>
- De nieuwste versie van Xamarin. Android is geïnstalleerd en wordt uitgevoerd op het platform van uw keuze. Raadpleeg de installatie handleiding voor [Xamarin. Android](https://docs.microsoft.com/xamarin/android/get-started/installation/index) voor een hand leiding voor het installeren van Xamarin. Android.
- Een <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">door een ontwikkelaar geactiveerd</a> en <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">voor ARCore geschikt</a> Android-apparaat.
  - Er zijn mogelijk extra apparaatstuurprogramma's vereist om uw computer te laten communiceren met uw Android-apparaat. Zie [hier](https://developer.android.com/studio/run/device.html)voor meer informatie.
- Uw app moet gericht zijn op ARCore **1,8**.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Voorbeeldproject openen

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Open `Xamarin/SampleXamarin.sln` in Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

De volgende stap is het configureren van de app voor het gebruik van uw account-id en de account sleutel. U hebt deze in een tekst editor gekopieerd bij [het instellen van de bron voor ruimtelijke ankers](#create-a-spatial-anchors-resource).

Open `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Zoek het veld `SpatialAnchorsAccountKey` en vervang `Set me` met de accountsleutel.

Zoek het `SpatialAnchorsAccountId`-veld en vervang `Set me` met de account-id.

## <a name="deploy-the-app-to-your-android-device"></a>De app op uw Android-apparaat implementeren

Schakel het Android-apparaat in, Meld u aan en verbind het met de computer via een USB-kabel.

Stel het opstart project in op **SampleXamarin. Android**, wijzig de **oplossings configuratie** in **release**en selecteer het apparaat waarnaar u wilt implementeren in de vervolg keuzelijst apparaat selecteren.

# <a name="windowstabdeploy-windows"></a>[Windows](#tab/deploy-windows)

![Configuratie van Visual Studio](./media/get-started-xamarin-android/visual-studio-windows-configuration.png)

Selecteer **debug** > **Start Debugging** om uw app te implementeren en te starten.

# <a name="macostabdeploy-macos"></a>[MacOS](#tab/deploy-macos)

![Configuratie van Visual Studio](./media/get-started-xamarin-android/visual-studio-macos-configuration.jpg)

Selecteer Start **uitvoeren** > **zonder fout opsporing** om uw app te implementeren en te starten.

---

Selecteer in de app **basis** om de demo uit te voeren en volg de instructies voor het plaatsen en intrekken van een anker.

> ![Scherm opname](./media/get-started-xamarin-android/screenshot-1.jpg)
> 1![scherm](./media/get-started-xamarin-android/screenshot-2.jpg)
> afbeelding2![scherm afbeelding 3](./media/get-started-xamarin-android/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
