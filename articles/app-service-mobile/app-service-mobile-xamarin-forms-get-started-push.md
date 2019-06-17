---
title: Pushmeldingen toevoegen aan uw Xamarin.Forms-app | Microsoft Docs
description: Informatie over het gebruik van Azure-services op meerdere platforms pushmeldingen verzenden naar uw Xamarin.Forms-apps.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: 99f2d9fb7c9a74e57eff3cd0b007fcee459cab88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62098693"
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Pushmeldingen toevoegen aan uw Xamarin.Forms-app

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie voegt u pushmeldingen toe aan de projecten die voortvloeien uit de [Xamarin.Forms-snelstartgids](app-service-mobile-xamarin-forms-get-started.md). Dit betekent dat een pushmelding wordt verzonden op alle platformen clients telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u de push notification-uitbreidingspakket. Zie voor meer informatie, [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Vereisten

Voor iOS-, moet u een [Apple Developer Program-lidmaatschap](https://developer.apple.com/programs/ios/) en een fysiek iOS-apparaat. De [pushmeldingen biedt geen ondersteuning voor iOS-simulator](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Een notification hub configureren

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject voor het verzenden van pushmeldingen te verzenden

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Configureren en uitvoeren van het Android-project (optioneel)

Voltooi deze sectie om in te schakelen pushmeldingen voor het Xamarin.Forms Droid-project voor Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging (FCM) inschakelen

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>De back-end voor mobiele Apps voor het verzenden van pushmeldingen aanvragen via FCM configureren

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Pushmeldingen toevoegen aan de Android-project

U kunt met de back-end geconfigureerd met FCM, onderdelen en -codes toevoegen aan de client om te registreren bij FCM. U kunt ook registreren voor pushmeldingen kunt verzenden met Azure Notification Hubs via de back-end van Mobile Apps en meldingen ontvangen.

1. In de **Droid** project, met de rechtermuisknop op **verwijzingen > NuGet-pakketten beheren...** .
1. Zoek in het venster NuGet Package Manager de **Xamarin.Firebase.Messaging** pakket en voeg deze toe aan het project.
1. In de Projecteigenschappen voor de **Droid** project, stelt u de app compileren met behulp van Android-versie 7.0 of hoger.
1. Voeg de **google-services.json** bestand gedownload uit de Firebase-console naar de hoofdmap van de **Droid** project en de bijbehorende build-actie ingesteld op **GoogleServicesJson**. Zie voor meer informatie, [toevoegen van het JSON-bestand van Google Services](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/#Add_the_Google_Services_JSON_File).

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase Cloud Messaging registreren

1. Open het bestand **AndroidManifest.xml** en voeg de volgende `<receiver>`-elementen toe aan het element `<application>`:

    ```xml
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
        </intent-filter>
    </receiver>
    ```

#### <a name="implementing-the-firebase-instance-id-service"></a>Implementatie van de Service Firebase exemplaar-ID

1. Voeg een nieuwe klasse aan de **Droid** project met de naam `FirebaseRegistrationService`, en zorg ervoor dat de volgende `using` instructies toe aan de bovenkant van het bestand aanwezig zijn:

    ```csharp
    using System.Threading.Tasks;
    using Android.App;
    using Android.Util;
    using Firebase.Iid;
    using Microsoft.WindowsAzure.MobileServices;
    ```

1. Vervang de lege `FirebaseRegistrationService` klasse met de volgende code:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class FirebaseRegistrationService : FirebaseInstanceIdService
    {
        const string TAG = "FirebaseRegistrationService";

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationTokenToAzureNotificationHub(refreshedToken);
        }

        void SendRegistrationTokenToAzureNotificationHub(string token)
        {
            // Update notification hub registration
            Task.Run(async () =>
            {
                await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
            });
        }
    }
    ```

    De `FirebaseRegistrationService` klasse is verantwoordelijk voor het genereren van beveiligingstokens die de toepassing te krijgen tot FCM. De `OnTokenRefresh` methode wordt aangeroepen wanneer de toepassing een registratietoken van FCM ontvangt. De methode haalt de token van de `FirebaseInstanceId.Instance.Token` eigenschap, die asynchroon door FCM wordt bijgewerkt. De `OnTokenRefresh` methode zelden aangeroepen, omdat het token wordt alleen bijgewerkt wanneer de toepassing wordt geïnstalleerd of verwijderd, wanneer de gebruiker toepassingsgegevens, verwijdert wanneer de toepassing de exemplaar-ID worden gewist, of wanneer de beveiliging van het token is aangetast. Bovendien wordt de FCM-exemplaar-id-service verzoeken dat de toepassing Hiermee vernieuwt u de token periodiek, doorgaans elke 6 maanden.

    De `OnTokenRefresh` methode ook roept de `SendRegistrationTokenToAzureNotificationHub` methode, die wordt gebruikt om te koppelen aan de Azure Notification Hub-registratietoken van de gebruiker.

#### <a name="registering-with-the-azure-notification-hub"></a>Registratie bij de Azure Notification Hub

1. Voeg een nieuwe klasse aan de **Droid** project met de naam `AzureNotificationHubService`, en zorg ervoor dat de volgende `using` instructies toe aan de bovenkant van het bestand aanwezig zijn:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Android.Util;
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

1. Vervang de lege `AzureNotificationHubService` klasse met de volgende code:

    ```csharp
    public class AzureNotificationHubService
    {
        const string TAG = "AzureNotificationHubService";

        public static async Task RegisterAsync(Push push, string token)
        {
            try
            {
                const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBody}
                };

                await push.RegisterAsync(token, templates);
                Log.Info("Push Installation Id: ", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
            }
        }
    }
    ```

    De `RegisterAsync` methode maakt u een sjabloon voor een eenvoudige melding als JSON en registers Sjabloonmeldingen ontvangen van de notification hub, met behulp van de Firebase-registratietoken. Dit zorgt ervoor dat het apparaat dat wordt vertegenwoordigd door de registratietoken heeft betrekking op alle meldingen worden verzonden vanuit de Azure Notification Hub.

#### <a name="displaying-the-contents-of-a-push-notification"></a>De inhoud van een Push-bericht weergeven

1. Voeg een nieuwe klasse aan de **Droid** project met de naam `FirebaseNotificationService`, en zorg ervoor dat de volgende `using` instructies toe aan de bovenkant van het bestand aanwezig zijn:

    ```csharp
    using Android.App;
    using Android.Content;
    using Android.Media;
    using Android.Support.V7.App;
    using Android.Util;
    using Firebase.Messaging;
    ```

1. Vervang de lege `FirebaseNotificationService` klasse met de volgende code:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class FirebaseNotificationService : FirebaseMessagingService
    {
        const string TAG = "FirebaseNotificationService";

        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);

            // Pull message body out of the template
            var messageBody = message.Data["message"];
            if (string.IsNullOrWhiteSpace(messageBody))
                return;

            Log.Debug(TAG, "Notification message body: " + messageBody);
            SendNotification(messageBody);
        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new NotificationCompat.Builder(this)
                .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle("New Todo Item")
                .SetContentText(messageBody)
                .SetContentIntent(pendingIntent)
                .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
                .SetAutoCancel(true);

            var notificationManager = NotificationManager.FromContext(this);
            notificationManager.Notify(0, notificationBuilder.Build());
        }
    }
    ```

    De `OnMessageReceived` methode, die wordt aangeroepen wanneer een toepassing een melding van FCM ontvangt, pakt u het bericht inhoud en roept de `SendNotification` methode. Deze methode zet de inhoud van het bericht in een lokale melding die wordt gestart, terwijl de toepassing wordt uitgevoerd, met de melding wordt weergegeven in het systeemvak.

U bent nu klaar testpushmeldingen in de app die wordt uitgevoerd op een Android-apparaat of de emulator.

### <a name="test-push-notifications-in-your-android-app"></a>Testpushmeldingen in uw Android-app

De eerste twee stappen zijn vereist, alleen wanneer u op een emulator test.

1. Zorg ervoor dat u implementeert op of foutopsporing op een apparaat of emulator die is geconfigureerd met Google Play Services. U kunt dit controleren door te controleren dat de **afspelen** apps zijn geïnstalleerd op het apparaat of emulator.
2. Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **instellingen** > **account toevoegen**. Volg de instructies in een bestaand Google-account toevoegen aan het apparaat of een nieuwe maken.
3. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **Droid** project en klik op **instellen als opstartproject**.
4. Klik op **uitvoeren** bouw het project en de app op uw Android-apparaat of emulator te starten.
5. In de app, typt u een taak en klik vervolgens op het plusteken ( **+** ) pictogram.
6. Controleer of dat er een melding wordt ontvangen wanneer een item wordt toegevoegd.

## <a name="configure-and-run-the-ios-project-optional"></a>Configureren en uitvoeren van het iOS-project (optioneel)

Deze sectie gaat over het uitvoeren van het Xamarin iOS-project voor iOS-apparaten. Als u niet met iOS-apparaten werkt, kunt u deze sectie overslaan.

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>De notification hub configureren voor APN 's

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Vervolgens configureert u de instelling van de iOS-project in Xamarin Studio of Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Pushmeldingen toevoegen aan uw iOS-app

1. In de **iOS** project, opent u AppDelegate.cs en de volgende instructie toe te voegen aan het begin van het codebestand.

    ```csharp
    using Newtonsoft.Json.Linq;
    ```

2. In de **AppDelegate** klasse, voegt u een onderdrukking voor de **RegisteredForRemoteNotifications** gebeurtenis zich registreren voor meldingen:

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application,
        NSData deviceToken)
    {
        const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

        // Register for push with your mobile app
        Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
        push.RegisterAsync(deviceToken, templates);
    }
    ```

3. In **AppDelegate**, ook toevoegen de volgende onderdrukking voor de **DidReceiveRemoteNotification** gebeurtenis-handler:

    ```csharp
    public override void DidReceiveRemoteNotification(UIApplication application,
        NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
    {
        NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

        string alert = string.Empty;
        if (aps.ContainsKey(new NSString("alert")))
            alert = (aps[new NSString("alert")] as NSString).ToString();

        //show alert
        if (!string.IsNullOrEmpty(alert))
        {
            UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
            avAlert.Show();
        }
    }
    ```

    Deze methode verwerkt de inkomende meldingen terwijl de app wordt uitgevoerd.

4. In de **AppDelegate** klasse, voeg de volgende code aan de **FinishedLaunching** methode:

    ```csharp
    // Register for push notifications.
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert
        | UIUserNotificationType.Badge
        | UIUserNotificationType.Sound,
        new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ```

    Dit biedt ondersteuning voor externe meldingen en aanvragen voor push-registratie.

Uw app wordt nu bijgewerkt ter ondersteuning van pushmeldingen.

#### <a name="test-push-notifications-in-your-ios-app"></a>Testpushmeldingen in uw iOS-app

1. Met de rechtermuisknop op het iOS-project en klik op **Set as StartUp Project**.
2. Druk op de **uitvoeren** knop of **F5** in Visual Studio het project bouwen en de app te starten in een iOS-apparaat. Klik vervolgens op **OK** te accepteren van pushmeldingen.

   > [!NOTE]
   > U moet expliciet pushmeldingen te verzenden vanuit uw app accepteren. Deze aanvraag treedt alleen op de eerste keer is dat de app wordt uitgevoerd.

3. In de app, typt u een taak en klik vervolgens op het plusteken ( **+** ) pictogram.
4. Controleer of een melding wordt ontvangen, en klik vervolgens op **OK** voor het verwijderen van de melding.

## <a name="configure-and-run-windows-projects-optional"></a>Configureren en uitvoeren van Windows-projecten (optioneel)

Deze sectie is voor het uitvoeren van de Xamarin.Forms-WinApp en WinPhone81-projecten voor Windows-apparaten. Deze stappen ook ondersteuning voor Universal Windows Platform (UWP)-projecten. Als u niet met Windows-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Uw Windows-app voor pushmeldingen registreren met Windows Notification Service (WNS)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>De notification hub configureren voor WNS

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Pushmeldingen toevoegen aan uw Windows-app

1. Open in Visual Studio, **App.xaml.cs** in een Windows-project en voeg de volgende instructies toe.

    ```csharp
    using Newtonsoft.Json.Linq;
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    using <your_TodoItemManager_portable_class_namespace>;
    ```

    Vervang `<your_TodoItemManager_portable_class_namespace>` met de naamruimte van uw draagbare project met daarin de `TodoItemManager` klasse.

2. Voeg in App.xaml.cs de volgende **InitNotificationsAsync** methode:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        const string templateBodyWNS =
            "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        JObject headers = new JObject();
        headers["X-WNS-Type"] = "wns/toast";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyWNS},
            {"headers", headers} // Needed for WNS.
        };

        await TodoItemManager.DefaultManager.CurrentClient.GetPush()
            .RegisterAsync(channel.Uri, templates);
    }
    ```

    Deze methode de push-mailmeldingskanaal opgehaald en wordt een sjabloon voor het ontvangen van Sjabloonmeldingen van de notification hub geregistreerd. De melding van een sjabloon die ondersteuning biedt voor *messageParam* voor deze client wordt geleverd.

3. Werk in App.xaml.cs de **OnLaunched** gebeurtenis-handler methodedefinitie door toe te voegen de `async` modifier. Voeg vervolgens de volgende coderegel toe aan het einde van de methode:

    ```csharp
    await InitNotificationsAsync();
    ```

    Dit zorgt ervoor dat de registratie voor pushberichten wordt gemaakt of vernieuwd zodra de app wordt gestart. Het is belangrijk dat u doet dit om ervoor te zorgen dat de WNS-kanaal is push altijd actief is.  

4. Open in Solution Explorer voor Visual Studio, de **Package.appxmanifest** bestand toe en stel **Toast geschikt** naar **Ja** onder **meldingen**.
5. De app bouwen en controleer of er geen fouten. Uw client-app moet nu registreren voor de voor Sjabloonmeldingen van de back-end voor mobiele Apps. Deze sectie te herhalen voor elke Windows-project in uw oplossing.

#### <a name="test-push-notifications-in-your-windows-app"></a>Testpushmeldingen in uw Windows-app

1. Met de rechtermuisknop op een Windows-project in Visual Studio, en klikt u op **instellen als opstartproject**.
2. Druk op de knop **Uitvoeren** om het project te bouwen en de app te starten.
3. Typ een naam voor een nieuwe taken in de app, en klik vervolgens op het plusteken ( **+** ) pictogram toe te voegen.
4. Controleer of dat er een melding wordt ontvangen wanneer het item wordt toegevoegd.

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over pushmeldingen te verzenden:

* [Pushmeldingen verzenden vanuit Azure Mobile Apps](https://developer.xamarin.com/guides/xamarin-forms/cloud-services/push-notifications/azure/)
* [Firebase Cloud-berichten](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/firebase-cloud-messaging/)
* [Externe meldingen met Firebase Cloud Messaging](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/)
* [Push notification-problemen vaststellen](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Er zijn diverse redenen waarom meldingen mogelijk verwijderd of niet doen terechtkomen op apparaten. Dit onderwerp ziet u hoe om te analyseren en te bepalen wat de oorzaak van fouten bij push-melding.

Ook kunt u doorgaan naar een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
  Ontdek hoe u gebruikers van uw app verifieert met een id-provider.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Ontdek hoe u offlineondersteuning voor uw app toevoegt met behulp van een back-end voor Mobile Apps. Offline synchroniseren zorgt gebruikers met een mobiele app kunnen werken&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;, zelfs wanneer er geen netwerkverbinding.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: https://go.microsoft.com/fwlink/p/?LinkId=272333
