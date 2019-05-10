---
title: Pushmeldingen verzenden naar iOS-apps met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een iOS-toepassing.
services: notification-hubs
documentationcenter: ios
keywords: pushmelding,pushmeldingen,ios-pushmeldingen
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 2793f64528baa2b9a84c671fc73bbe399e2387ea
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411620"
---
# <a name="tutorial-push-notifications-to-ios-apps-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar iOS met Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In deze zelfstudie gebruikt u Azure Notification Hubs om pushmeldingen te verzenden naar een iOS-toepassing. U maakt een lege iOS-app die pushmeldingen ontvangt met de [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Het bestand met de aanvraag voor certificaatondertekening genereren
> * Uw app registreren voor pushmeldingen
> * Een inrichtingsprofiel voor de app maken
> * De Notification Hub configureren voor iOS-pushmeldingen
> * Uw iOS-app verbinden met Notification Hubs
> * Testpushmeldingen verzenden
> * Controleren of uw app meldingen ontvangt

De volledige code voor deze zelfstudie vindt u [op GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/master/Samples). 

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis Azure-account](https://azure.microsoft.com/free) maken.
* [Windows Azure Messaging Framework]
* Meest recente versie van [Xcode]
* Een apparaat dat compatibel is met iOS 10 (of een hogere versie)
* [Apple Developer Program](https://developer.apple.com/programs/)-lidmaatschap
  
  > [!NOTE]
  > Vanwege configuratievereisten voor pushmeldingen moet u pushmeldingen op een fysiek iOS-apparaat (iPhone of iPad) implementeren en testen in plaats van in de iOS-simulator.
  
Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor iOS-apps.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>De Notification Hub voor iOS-pushmeldingen configureren

In deze sectie maakt u een Notification Hub en configureert u verificatie met APNS met het **.p12**-pushcertificaat dat u eerder hebt gemaakt. Als u een Notification Hub wilt gebruiken die u al hebt gemaakt, kunt u doorgaan naar stap 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-your-notification-hub-with-apns-information"></a>Uw Notification Hub configureren met APNS-gegevens

1. Selecteer **Apple (APNS)** onder **Notification Services**.
2. Selecteer **Certificaat**.
3. Selecteer het **bestandspictogram**.
4. Selecteer het **.p12**-bestand dat u eerder hebt geëxporteerd.
5. Geef het juiste **wachtwoord** op.
6. Selecteer de modus **Sandbox**. Gebruik **Productie** alleen als u pushmeldingen wilt verzenden naar gebruikers die uw app in de winkel hebben aangeschaft.

    ![APNS-certificering in Azure Portal configureren][7]

De Notification Hub is nu geconfigureerd om te werken met APNS en u hebt de verbindingsreeksen om uw app te registreren en pushmeldingen te verzenden.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Uw iOS-app verbinden met Notification Hubs

1. Maak in Xcode een nieuw iOS-project en selecteer de sjabloon **Single View Application** (Toepassing met één weergave).

    ![Xcode - Toepassing voor één weergave][8]

2. Bij het instellen van de opties voor het nieuwe project moet u dezelfde **productnaam** en **organisatie-id** gebruiken als bij het instellen van de bundel-id in de Apple Developer-portal.

    ![Xcode - Projectopties][11]

3. Klik onder Projectnavigator op de naam van uw project, klik op het tabblad **Algemeen** en zoek naar **Ondertekening**. Zorg ervoor dat u het juiste team voor uw Apple Developer-account selecteert. XCode moet automatisch het profiel voor inrichting openen dat u eerder op basis van uw bundel-id hebt gemaakt.

    Als u het nieuwe profiel voor inrichting dat u hebt gemaakt in Xcode niet ziet, vernieuwt u de profielen voor uw identiteit voor ondertekening. Klik op **Xcode** in de menubalk, klik op **Preferences** (Voorkeuren), klik op het tabblad **Account** en klik op de knop **View Details** (Details weergeven), klik op uw identiteit voor ondertekening en klik vervolgens op de knop voor vernieuwen in de rechterbenedenhoek.

    ![Xcode - Profiel voor inrichting][9]

4. Selecteer het tabblad **Mogelijkheden** en zorg ervoor dat Pushmeldingen is ingeschakeld

    ![Xcode - pushmogelijkheden][12]

5. De Azure Notification Hubs SDK-modules toevoegen.

   U kunt de Azure Notification Hubs SDK integreren in uw app met behulp van [Cocoapods](https://cocoapods.org) of door handmatig de binaire bestanden toe te voegen aan uw project.

   - Integratie via Cocoapods

     Voeg de volgende afhankelijkheden aan uw `podfile` om op te nemen van Azure Notification Hubs SDK in uw app.

     ```ruby
     pod 'AzureNotificationHubs-iOS'
     ```

     Voer `pod install` uw nieuw gedefinieerde pod installeert en opent de `.xcworkspace`.

     > [!NOTE]
     > Als er een foutbericht wordt weergegeven als ```[!] Unable to find a specification for `AzureNotificationHubs-iOS` ``` tijdens de uitvoering `pod install`, voert u `pod repo update` om de meest recente schillen ophalen uit de opslagplaats Cocoapods en voer vervolgens `pod install`.

   - Integratie via Carthage

     Voeg de volgende afhankelijkheden aan uw `Cartfile` om op te nemen van Azure Notification Hubs SDK in uw app.

     ```ruby
     github "Azure/azure-notificationhubs-ios"
     ```

     Vervolgens, bijwerken en build-afhankelijkheden:

     ```shell
     $ carthage update
     ```

     Zie voor meer informatie over het gebruik van Carthage de [Carthage GitHub-opslagplaats](https://github.com/Carthage/Carthage).

   - Integratie met de binaire bestanden kopiëren naar uw project

     1. Download de [Azure Notification Hubs SDK](https://github.com/Azure/azure-notificationhubs-ios/releases) framework geleverd als een zip-bestand en pak het uit.

     2. Klik met de rechtermuisknop op uw project in Xcode en klik op de optie **Add Files to** (Bestanden toevoegen aan) om de map **WindowsAzureMessaging.framework** aan uw Xcode-project toe te voegen. Selecteer **Options** (Opties), zorg ervoor dat **Copy items if needed** (Copy items if needed) is geselecteerd en klik op **Add** (Toevoegen).

        ![Azure SDK uitpakken][10]

6. Voeg een nieuw headerbestand toe aan uw project met de naam `HubInfo.h`. Dit bestand bevat de constanten voor uw Notification Hub. Voeg de volgende definities toe en vervang de tijdelijke aanduidingen voor tekenreeksen door uw *hubnaam* en de *DefaultListenSharedAccessSignature* die u eerder hebt genoteerd.

    ```objc
    #ifndef HubInfo_h
    #define HubInfo_h

        #define HUBNAME @"<Enter the name of your hub>"
        #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"

    #endif /* HubInfo_h */
    ```

7. Open uw bestand `AppDelegate.h` en voeg de volgende instructies voor importeren toe:

    ```objc
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    #import <UserNotifications/UserNotifications.h>
    #import "HubInfo.h"
    ```
8. Voeg afhankelijk van uw iOS-versie in uw bestand `AppDelegate.m` de volgende code toe aan de `didFinishLaunchingWithOptions`-methode. Deze code registreert uw apparaatingang met APNs:

    ```objc
    UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
        UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

    [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
    [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```

9. Voeg in hetzelfde bestand de volgende methoden toe:

    ```objc
        - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
        SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                    notificationHubPath:HUBNAME];

        [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
            else {
                [self MessageBox:@"Registration Status" message:@"Registered"];
            }
        }];
        }

    -(void)MessageBox:(NSString *) title message:(NSString *)messageText
    {
        UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:messageText preferredStyle:UIAlertControllerStyleAlert];
        UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil];
        [alert addAction:okAction];
        [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }
    ```

    Deze code maakt verbinding met de Notification Hub met de verbindingsgegevens die u hebt opgegeven in HubInfo.h. Er wordt vervolgens een apparaattoken aan de Notification Hub toegekend, zodat de Notification Hub meldingen kan verzenden.

10. Voeg in hetzelfde bestand de volgende methode toe om een **UIAlert** weer te geven als de melding is ontvangen terwijl de app actief is:

    ```objc
    - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
        NSLog(@"%@", userInfo);
        [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
    }
    ```

11. Ontwikkel en voer de app op uw apparaat uit om te controleren of er geen fouten zijn.

## <a name="send-test-push-notifications"></a>Testpushmeldingen verzenden

U kunt ontvangst van meldingen in uw app testen met de optie *Test verzenden* in [Azure Portal]. Er wordt dan een pushmelding als test naar uw apparaat verzonden.

![Azure Portal - Test verzenden][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="verify-that-your-app-receives-push-notifications"></a>Controleren of uw app pushmeldingen ontvangt

Als u pushmeldingen op iOS wilt testen, moet u de app implementeren op een fysiek iOS-apparaat. U kunt geen Apple pushmeldingen verzenden via de iOS-simulator.

1. Voer de app uit en controleer of de registratie is gelukt en druk vervolgens op **OK**.

    ![De registratie van pushmeldingen in iOS-apps testen][33]

2. Vervolgens verstuurt u als test een pushmelding vanuit [Azure Portal], zoals in de vorige sectie wordt beschreven.

3. De pushmelding wordt verzonden naar alle apparaten die zijn geregistreerd voor het ontvangen van meldingen van de specifieke Notification Hub.

    ![Het ontvangen van pushmeldingen in iOS-apps testen][35]

## <a name="next-steps"></a>Volgende stappen

In dit eenvoudige voorbeeld hebt u pushmeldingen uitgezonden naar al uw geregistreerde iOS-apparaten. Als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke iOS-apparaten, gaat u verder met de volgende zelfstudie:

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke apparaten](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png
[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png

<!-- URLs. -->
[Windows Azure Messaging Framework]: https://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: https://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: https://go.microsoft.com/fwlink/p/?LinkId=272456
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Notify Users for iOS with .NET backend]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-ios-xplat-segmented-apns-push-notification.md
[Local and Push Notification Programming Guide]: https://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
