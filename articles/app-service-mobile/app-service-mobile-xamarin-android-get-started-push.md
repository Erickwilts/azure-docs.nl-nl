---
title: Pushmeldingen toevoegen aan uw Xamarin.Android-app | Microsoft Docs
description: Informatie over het gebruik van Azure App Service en Azure Notification Hubs om pushmeldingen te verzenden aan uw Xamarin.Android-app
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: f2c58d64a662a6e2be355c1f672c01e5a679b3f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127935"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Pushmeldingen toevoegen aan uw Xamarin.Android-app

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie voegt u pushmeldingen kunt verzenden naar de [Xamarin.Android-snelstartgids](app-service-mobile-windows-store-dotnet-get-started.md) project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade Quick Start-serverproject niet gebruikt, moet u de push notification-uitbreidingspakket. Zie voor meer informatie de [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) handleiding.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie vereist de installatie:

* Een actief Google-account. U kunt zich aanmelden voor een Google-account op [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Google Cloud Messaging-clientonderdeel](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Een Notification Hub configureren

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Inschakelen van Firebase Cloud Messaging

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Azure voor het verzenden van pushmeldingen aanvragen configureren

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Bijwerken van het serverproject voor het verzenden van pushmeldingen te verzenden

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Het clientproject voor push-meldingen configureren

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Push notifications code toevoegen aan uw app

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Testpushmeldingen in uw app

U kunt de app testen met behulp van een virtueel apparaat in de emulator. Er zijn extra configuratiestappen vereist wanneer u een emulator.

1. Het virtuele apparaat moet Google APIs ingesteld als het doel in het beheer van Android Virtual Device (AVD) hebben.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **instellingen** > **account toevoegen**, volgt u de aanwijzingen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. De todolist-app als voordat uitvoeren en voeg een nieuwe taak. Deze keer wordt het meldingspictogram van een weergegeven in het systeemvak. U kunt de meldingenlijst als u wilt weergeven van de volledige tekst van de melding kunt openen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
