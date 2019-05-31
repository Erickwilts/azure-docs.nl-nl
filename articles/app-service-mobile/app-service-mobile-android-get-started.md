---
title: Een Android-app maken in Azure App Service Mobile Apps | Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met back-ends voor mobiele apps van Azure voor Android-ontwikkeling.
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: conceptual
ms.date: 5/6/2019
ms.author: crdun
ms.openlocfilehash: 72fd043321aefe74e95b34ec24c7be2ffd409439
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240332"
---
# <a name="create-an-android-app"></a>Een Android-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Android-app met een back-end voor een mobiele apps van Azure.  U maakt zowel een nieuwe back-end voor een mobiele app als een eenvoudige Android-app voor *takenlijsten* die app-gegevens opslaat in Azure.

Het volgen van deze zelfstudie is een vereiste voor alle andere Android-zelfstudies over het gebruik van de functie Mobile Apps in Azure App Service.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* De [Android-hulpprogramma's voor ontwikkelaars](https://developer.android.com/sdk/index.html), waaronder Integrated Development Environment van Android Studio en het meest recente Android-platform.
* Azure Mobile Android SDK.
* Een [actief Azure-account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Een nieuwe back-end voor mobiele apps van Azure maken
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>De verbinding met een database maken en configureren van het project client en server
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-android-app"></a>De Android-app uitvoeren
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
