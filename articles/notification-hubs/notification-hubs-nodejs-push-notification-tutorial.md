---
title: Push meldingen verzenden met Azure Notification Hubs en node. js
description: Meer informatie over het gebruik van Notification Hubs voor het verzenden van push meldingen van een node. js-toepassing.
keywords: push melding, Push meldingen, node. js-push, Ios-push
services: notification-hubs
documentationcenter: nodejs
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 129127a2a43cd9a86e0a1e1cf538358b62381257
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "67706231"
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Push meldingen verzenden met Azure Notification Hubs en node. js

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Overzicht

> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u in slechts een paar minuten een gratis proef account maken via de [gratis proef versie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

In deze hand leiding wordt beschreven hoe u push meldingen kunt verzenden met behulp van Azure Notification Hubs rechtstreeks vanuit een [node. js](https://nodejs.org) -toepassing.

De gedekte scenario's zijn het verzenden van push meldingen naar toepassingen op de volgende platforms:

- Android
- iOS
- Universeel Windows-platform
- Windows Phone

## <a name="notification-hubs"></a>Notification Hubs

Azure Notification Hubs biedt een gebruiks vriendelijke, schaal bare infra structuur met meerdere platformen voor het verzenden van push meldingen naar mobiele apparaten. Zie de pagina [Azure notification hubs](https://msdn.microsoft.com/library/windowsazure/jj927170.aspx) voor meer informatie over de service-infra structuur.

## <a name="create-a-nodejs-application"></a>Een node. js-toepassing maken

De eerste stap in deze zelf studie is het maken van een nieuwe, lege node. js-toepassing. Zie voor instructies over het maken van een node. js-toepassing [een node. js-toepassing maken en implementeren in azure web site][nodejswebsite], [node. js Cloud service][Node.js Cloud Service] met behulp van Windows Power shell of website [met webmatrix][webmatrix].

## <a name="configure-your-application-to-use-notification-hubs"></a>Uw toepassing configureren voor het gebruik van Notification Hubs

Als u Azure Notification Hubs wilt gebruiken, moet u het node. js Azure- [pakket](https://www.npmjs.com/package/azure)downloaden en gebruiken, dat een ingebouwde set van helper-bibliotheken bevat die communiceren met de Push Notification rest-Services.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik node Package Manager (NPM) om het pakket te verkrijgen

1. Gebruik een opdracht regel interface zoals **Power shell** (Windows), **Terminal** (Mac) of **bash** (Linux) en navigeer naar de map waarin u de lege toepassing hebt gemaakt.
2. Voer `npm install azure-sb` uit in het opdracht venster.
3. U kunt de `ls` of `dir` -opdracht hand matig uitvoeren om te `node_modules` controleren of een map is gemaakt.
4. Zoek in die map naar het **Azure** -pakket, dat de bibliotheken bevat die u nodig hebt om toegang te krijgen tot de notification hub.

> [!NOTE]
> Meer informatie over het installeren van NPM vindt u in de officiële [NPM-blog](https://blog.npmjs.org/post/85484771375/how-to-install-npm).

### <a name="import-the-module"></a>De module importeren
Gebruik een tekst editor om het volgende toe te voegen aan de `server.js` bovenkant van het bestand van de toepassing:

```javascript
var azure = require('azure-sb');
```

### <a name="set-up-an-azure-notification-hub-connection"></a>Een Azure notification hub-verbinding instellen

Met `NotificationHubService` het object kunt u werken met Notification hubs. Met de volgende code wordt `NotificationHubService` een object gemaakt voor de notification `hubname`hub met de naam. Voeg het toe aan de bovenkant van `server.js` het bestand, na de instructie voor het importeren van de Azure-module:

```javascript
var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');
```

Haal de verbindings `connectionstring` waarde uit de [Azure Portal] door de volgende stappen uit te voeren:

1. Klik in het navigatie deel venster links op **Bladeren**.
2. Selecteer **Notification hubs**en zoek vervolgens de hub die u wilt gebruiken voor het voor beeld. U kunt de [zelf studie aan de slag met Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) raadplegen als u hulp nodig hebt bij het maken van een nieuwe notification hub.
3. Selecteer **instellingen**.
4. Klik op **toegangs beleid**. U ziet zowel gedeelde als volledige toegang verbindings reeksen.

![Azure Portal-Notification Hubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> U kunt de connection string ook ophalen met behulp van de cmdlet **Get-AzureSbNamespace** die door [Azure PowerShell](/powershell/azureps-cmdlets-docs) wordt verschaft of de **Azure SB-naam ruimte opdracht weer geven** met de [Azure-opdracht regel interface (Azure CLI)](../cli-install-nodejs.md).

## <a name="general-architecture"></a>Algemene architectuur

Het `NotificationHubService` object bevat de volgende object instanties voor het verzenden van push meldingen naar specifieke apparaten en toepassingen:

- **Android** : gebruik het `GcmService` object dat beschikbaar is op`notificationHubService.gcm`
- **IOS** : gebruik het `ApnsService` object dat toegankelijk is op`notificationHubService.apns`
- **Windows Phone** -gebruik het `MpnsService` object, dat beschikbaar is op`notificationHubService.mpns`
- **Universeel Windows-platform** -gebruik het `WnsService` object, dat beschikbaar is op`notificationHubService.wns`

### <a name="how-to-send-push-notifications-to-android-applications"></a>Procedure: Push meldingen verzenden naar Android-toepassingen

Het `GcmService` object biedt een `send` methode die kan worden gebruikt voor het verzenden van push meldingen naar Android-toepassingen. De `send` methode accepteert de volgende para meters:

- **Tags** : de tag-id. Als er geen tag wordt gegeven, wordt de melding verzonden naar alle clients.
- **Payload** : de nettolading van de JSON-of onbewerkte teken reeks van het bericht.
- **Call back** -de functie call back.

Zie de [Payload-documentatie](https://distriqt.github.io/ANE-PushNotifications/m.FCM-GCM%20Payload)voor meer informatie over de indeling van de nettolading.

De volgende code gebruikt het `GcmService` exemplaar dat wordt weer `NotificationHubService` gegeven door de om een push melding naar alle geregistreerde clients te verzenden.

```javascript
var payload = {
  data: {
    message: 'Hello!'
  }
};
notificationHubService.gcm.send(null, payload, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Procedure: Push meldingen verzenden naar iOS-toepassingen

Hetzelfde als met de hierboven beschreven Android-toepassingen `ApnsService` biedt het object `send` een methode die kan worden gebruikt voor het verzenden van push meldingen naar IOS-toepassingen. De `send` methode accepteert de volgende para meters:

- **Tags** : de tag-id. Als er geen tag wordt gegeven, wordt de melding verzonden naar alle clients.
- **Payload** : de JSON of de teken reeks payload van het bericht.
- **Call back** -de functie call back.

Zie de sectie **meldings lading** van het document [Local and Push Notification Programming Guide (Engelstalig](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) ) voor meer informatie over de indeling van de nettolading.

De volgende code gebruikt het `ApnsService` exemplaar dat wordt weer `NotificationHubService` gegeven door de om een waarschuwings bericht te verzenden naar alle clients:

```javascript
var payload={
    alert: 'Hello!'
  };
notificationHubService.apns.send(null, payload, function(error){
  if(!error){
      // notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Procedure: Push meldingen verzenden naar Windows Phone-toepassingen

Het `MpnsService` object biedt een `send` methode die kan worden gebruikt om Push meldingen te verzenden naar Windows Phone-toepassingen. De `send` methode accepteert de volgende para meters:

- **Tags** : de tag-id. Als er geen tag wordt gegeven, wordt de melding verzonden naar alle clients.
- **Payload** : de XML-nettolading van het bericht.
- **Doel** naamvoorpop-upmeldingen.`toast`  -  `token`voor tegel meldingen.
- **NotificationClass** : de prioriteit van de melding. Zie de sectie **http-header-elementen** van de [Push meldingen van een server](https://msdn.microsoft.com/library/hh221551.aspx) document voor geldige waarden.
- **Opties** -optionele aanvraag headers.
- **Call back** -de functie call back.

Bekijk de `TargetName` [Push meldingen van een server](https://msdn.microsoft.com/library/hh221551.aspx) pagina voor een lijst met geldige `NotificationClass` en koptekst opties.

De volgende voorbeeld code gebruikt het `MpnsService` exemplaar dat wordt weer `NotificationHubService` gegeven door de push melding voor het verzenden van een pop-upbericht:

```javascript
var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Procedure: Push meldingen verzenden naar Universeel Windows-platform-toepassingen (UWP)

Het `WnsService` object biedt een `send` methode die kan worden gebruikt om Push meldingen te verzenden naar universeel Windows-platform-toepassingen.  De `send` methode accepteert de volgende para meters:

- **Tags** : de tag-id. Als er geen tag wordt gegeven, wordt de melding verzonden naar alle geregistreerde clients.
- **Payload** -de XML-nettolading voor berichten.
- **Type** : het meldings type.
- **Opties** -optionele aanvraag headers.
- **Call back** -de functie call back.

Voor een lijst met geldige typen en aanvraag headers raadpleegt u [Push Notification Service-aanvraag en-antwoord headers](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

De volgende code gebruikt het `WnsService` exemplaar dat wordt weer `NotificationHubService` gegeven door de om een push melding naar een pop-upbericht te verzenden naar een UWP-app:

```javascript
var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
  if(!error){
      // notification sent
  }
});
```

## <a name="next-steps"></a>Volgende stappen

Met de voorbeeld fragmenten hierboven kunt u eenvoudig service-infra structuur bouwen voor het leveren van push meldingen naar een groot aantal verschillende apparaten. Nu u de basis principes van het gebruik van Notification Hubs met node. js hebt geleerd, volgt u deze koppelingen voor meer informatie over hoe u deze mogelijkheden verder kunt uitbreiden.

- Zie de MSDN-referentie voor [Azure notification hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
- Ga naar de [Azure SDK voor Node] opslagplaats op github voor meer voor beelden en meer informatie over de implementatie.

[Azure SDK voor Node]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: https://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs
[webmatrix]: https://docs.microsoft.com/aspnet/web-pages/videos/introduction/create-a-website-using-webmatrix
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
