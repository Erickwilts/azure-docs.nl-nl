---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 83a0adf98298225b52d3b4fdfa2ca861ebb70bb9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140693"
---
**Back-end van .NET (C#)** :
  
1. Met de rechtermuisknop op de serverproject in Visual Studio, en klikt u op **NuGet-pakketten beheren**, zoeken naar `Microsoft.Azure.NotificationHubs`, klikt u vervolgens op **installeren**. Hiermee installeert u de Notification Hubs-bibliotheek voor het verzenden van meldingen vanuit uw back-end.
2. Open in Visual Studio-project van de back-end, **Controllers** > **TodoItemController.cs**. Voeg het volgende toe aan de bovenkant van het bestand `using` instructie:

    ```csharp
    using Microsoft.Azure.Mobile.Server.Config;
    using Microsoft.Azure.NotificationHubs;
    ```

3. Vervang de `PostTodoItem`-methode door de volgende code:  

    ```csharp
    public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
    {
        TodoItem current = await InsertAsync(item);
        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;

        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // iOS payload
        var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }
        return CreatedAtRoute("Tables", new { id = current.Id }, current);
    }
    ```

4. Het serverproject publiceren.

**Node.js-back-end**:

1. Als u dit nog niet hebt gedaan, [downloaden van de Quick Start-project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) of gebruik anders de [online-editor in Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).    

2. Het script van de tabel todoitem.js vervangen door de volgende code:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    // When adding record, send a push notification via APNS
    table.insert(function (context) {
        // For details of the Notification Hubs JavaScript SDK, 
        // see https://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Create a payload that contains the new item Text.
        var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";

        // Execute the insert; Push as a post-execute action when results are returned as a Promise.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    context.push.apns.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
    });

    module.exports = table;
    ```

3. Tijdens het bewerken van het bestand op uw lokale computer, publiceert u opnieuw het serverproject.
