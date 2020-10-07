---
title: 'Quickstart: Een Xamarin iOS-app maken'
description: In deze quickstart leert u een iOS-app maken met Xamarin en met behulp van Spatial Anchors.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 07/22/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 12fb644430d4bfd6444c6bbf09d37b4662ca6fd1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "87810102"
---
# <a name="quickstart-create-a-xamarin-ios-app-with-azure-spatial-anchors"></a>Quickstart: Een Xamarin iOS-app maken met Azure Spatial Anchors

In deze quickstart wordt besproken hoe u een iOS-app met Xamarin maakt met behulp van [Azure Spatial Anchors](../overview.md). Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een iOS-app gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Implementeren en uitvoeren op een iOS-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:
- Een Mac met macOS High Sierra (10.13) of hoger met:
  - De meest recente versie van Xcode en iOS SDK die is geïnstalleerd vanuit de [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  - Een up-to-date versie van <a href="https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019" target="_blank">Visual Studio voor Mac 8.1+</a>.
  - <a href="https://git-scm.com/download/mac" target="_blank">Git voor macOS</a>.
  - <a href="https://git-lfs.github.com/">Git LFS</a>.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Voorbeeldproject openen

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Open `Xamarin/SampleXamarin.sln` in Visual Studio.

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

De volgende stap is om de app te configureren voor gebruik van uw account-id en accountsleutel. U hebt ze naar een teksteditor gekopieerd bij het [instellen van de Spatial Anchors-resource](#create-a-spatial-anchors-resource).

Open `Xamarin/SampleXamarin.Common/AccountDetails.cs`.

Zoek het veld `SpatialAnchorsAccountKey` en vervang `Set me` met de accountsleutel.

Zoek het veld `SpatialAnchorsAccountId` en vervang `Set me` met de account-id.

Zoek het veld `SpatialAnchorsAccountDomain` en vervang `Set me` door het accountdomein.

## <a name="deploy-the-app-to-your-ios-device"></a>De app implementeren op uw iOS-apparaat

Start het iOS-apparaat, meld u aan en maak verbinding met de computer via een USB-kabel.

Stel het opstartproject in op **SampleXamarin.iOS**, wijzig de **oplossingsconfiguratie** in **Release** en selecteer het apparaat waarop u wilt implementeren in de vervolgkeuzelijst Apparaat selecteren.

![Configuratie van Visual Studio](./media/get-started-xamarin-iOS/visual-studio-macos-configuration.jpg)

Selecteer **Uitvoeren** > **Starten zonder fout opsporing** om uw app te implementeren en te starten.

Selecteer in de app **Basisinstellingen** om de demo uit te voeren en volg de instructies om een bladwijzer te plaatsen en opnieuw aan te roepen.

> ![Schermafbeelding 1](./media/get-started-xamarin-ios/screenshot-1.jpg)
> ![Schermafbeelding 2](./media/get-started-xamarin-ios/screenshot-2.jpg)
> ![Schermafbeelding 3](./media/get-started-xamarin-ios/screenshot-3.jpg)

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
