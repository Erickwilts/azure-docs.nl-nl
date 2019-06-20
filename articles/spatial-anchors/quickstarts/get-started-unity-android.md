---
title: Snelstartgids - een Unity Android-app maken met Azure ruimtelijke ankers | Microsoft Docs
description: In deze quickstart leert u een Android-app maken met Unity en met behulp van Spatial Anchors.
author: craigktreasure
manager: aliemami
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 642abfb99b40d67802b7194ad225ebcd2872a72b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "67135113"
---
# <a name="quickstart-create-a-unity-android-app-with-azure-spatial-anchors"></a>Quickstart: Een Unity Android-app maken met Azure ruimtelijke ankers

Deze quickstart bevat informatie over het maken van een Unity Android-app met [Azure ruimtelijke ankers](../overview.md). Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een ARCore Android-app met Unity gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * Build-instellingen voor Unity voorbereiden
> * De ARCore SDK voor Unity downloaden en importeren
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Het Android Studio-project exporteren
> * Implementeren en uitvoeren op een Android-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Een Windows- of macOS-machine met <a href="https://unity3d.com/get-unity/download" target="_blank">Unity 2018.3 +</a> en <a href="https://developer.android.com/studio/" target="_blank">Android Studio 3.3 +</a>.
  - Indien uitgevoerd op Windows, u moet ook <a href="https://git-scm.com/download/win" target="_blank">Git voor Windows</a>.
  - Als het wordt uitgevoerd op macOS, krijgt u Git geïnstalleerd via HomeBrew. Voer de volgende opdracht in een enkele regel van de Terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. Voer `brew install git`.
- Een <a href="https://developer.android.com/studio/debug/dev-options" target="_blank">door een ontwikkelaar geactiveerd</a> en <a href="https://developers.google.com/ar/discover/supported-devices" target="_blank">voor ARCore geschikt</a> Android-apparaat.
- Uw app moet gebruikmaken van versie **1.7** van de SDK ARCore voor Unity.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Downloaden en openen van het voorbeeldproject Unity

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

[!INCLUDE [Android Unity Build Settings](../../../includes/spatial-anchors-unity-android-build-settings.md)]

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

Ga in het deelvenster **Project** naar `Assets/AzureSpatialAnchorsPlugin/Examples` en open het scènebestand `AzureSpatialAnchorsBasicDemo.unity`.

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

Sla de scène op door **Bestand** -> **Opslaan** te selecteren.

## <a name="export-the-android-studio-project"></a>Exporteer het Android Studio-project

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

Controleer of er geen vinkje staat voor **Export Project**. Klik op **Build And Run**. U wordt gevraag het `.apk`-bestand op te slaan. Geef het een willekeurige naam.

Volg de instructies in de app om een anker te plaatsen en terug te halen.

> [!NOTE]
> Als u bij het uitvoeren van de app de camera niet als achtergrond ziet (u ziet bijvoorbeeld lege, blauwe of andere structuren) dan moet u waarschijnlijk assets opnieuw in Unity importeren. De app stoppen. Kies in het bovenste menu in Unity **Assets -> Alles opnieuw importeren**. Voer de vervolgens opnieuw app uit.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelfstudie: Spatial Anchors met meerdere apparaten delen](../tutorials/tutorial-share-anchors-across-devices.md)
