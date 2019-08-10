---
title: 'Quick Start: een Xamarin iOS-app maken met ruimtelijke ankers van Azure | Microsoft Docs'
description: In deze Quick Start leert u hoe u een iOS-app kunt bouwen met Xamarin met behulp van ruimtelijke ankers.
author: craigktreasure
manager: virivera
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: ef25cf07326220be36ce8f67267428ffe1ac0728
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931642"
---
# <a name="quickstart-create-a-xamarin-ios-app-with-azure-spatial-anchors"></a>Quickstart: Een Xamarin iOS-app maken met ruimtelijke ankers van Azure

In deze Quick Start wordt beschreven hoe u een iOS-app met Xamarin maakt met behulp van [Azure spatiale ankers](../overview.md). Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Wanneer u klaar bent, hebt u een iOS-app waarmee u een ruimtelijk anker kunt opslaan en intrekken.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Implementeren en uitvoeren op een iOS-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:
- Een Mac met macOS High Sierra (10,13) of hoger met:
  - De meest recente versie van Xcode en iOS SDK is geïnstalleerd uit de [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  - Een up-to-date versie van <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio voor Mac 8.1 +</a>.
  - <a href="https://git-scm.com/download/mac" target="_blank">Git voor macOS</a>

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Voorbeeldproject openen

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Open `Xamarin/SampleXamarin.sln` in Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

De volgende stap is het configureren van de app voor het gebruik van uw account-id en de account sleutel. U hebt deze in een tekst editor gekopieerd bij [het instellen van de bron voor ruimtelijke ankers](#create-a-spatial-anchors-resource).

Open `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Zoek het veld `SpatialAnchorsAccountKey` en vervang `Set me` met de accountsleutel.

Zoek het veld `SpatialAnchorsAccountId` en vervang `Set me` met de account-id.

## <a name="deploy-the-app-to-your-ios-device"></a>De app implementeren op uw iOS-apparaat

Schakel het iOS-apparaat in, Meld u aan en verbind het met de computer via een USB-kabel.

Stel het opstart project in op **SampleXamarin. IOS**, wijzig de **oplossings configuratie** in **release**en selecteer in de vervolg keuzelijst apparaat selecteren het apparaat dat u wilt implementeren.

![Configuratie van Visual Studio](./media/get-started-xamarin-iOS/visual-studio-macos-configuration.jpg)

Selecteer Start **uitvoeren** > **zonder fout opsporing** om uw app te implementeren en te starten.

Selecteer in de app **basis** om de demo uit te voeren en volg de instructies voor het plaatsen en intrekken van een anker.

> ![Scherm opname](./media/get-started-xamarin-ios/screenshot-1.jpg)
> 1![scherm](./media/get-started-xamarin-ios/screenshot-2.jpg)
> afbeelding2![scherm afbeelding 3](./media/get-started-xamarin-ios/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
