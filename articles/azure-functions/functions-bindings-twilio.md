---
title: Azure Functions Twilio-binding
description: Over het gebruik van Twilio-bindingen met Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure functions, functies, gebeurtenisverwerking, dynamische Computing, serverloze architectuur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 07/09/2018
ms.author: cshoe
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c63b81e5461af5407d260651b79ec80e79fc9b4d
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67479974"
---
# <a name="twilio-binding-for-azure-functions"></a>Twilio-binding voor Azure Functions

In dit artikel wordt uitgelegd hoe u SMS-berichten verzenden met behulp van [Twilio](https://www.twilio.com/) bindingen in Azure Functions. Azure Functions ondersteunt uitvoerbindingen voor Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De Twilio-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Twilio](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.Twilio/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De Twilio-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Twilio](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example---functions-1x"></a>Voorbeeld - functies 1.x

Zie het voorbeeld taalspecifieke:

* [C#](#c-example)
* [C# script (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die verstuurt een tekstbericht wanneer ze worden geactiveerd door een wachtrijbericht.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

In dit voorbeeld wordt de `TwilioSms` kenmerk met de geretourneerde waarde van de methode. Een alternatief is het gebruik van het kenmerk met een `out SMSMessage` parameter of een `ICollector<SMSMessage>` of `IAsyncCollector<SMSMessage>` parameter.

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u een Twilio-Uitvoerbinding een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie gebruikt een `out` parameter voor het verzenden van een SMS-bericht.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Hier volgt een C#-scriptcode:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

U kunt niet gebruiken in synchrone code uitvoerparameters. Hier volgt een asynchrone C#-code-voorbeeldscript:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u een Twilio-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="example---functions-2x"></a>Voorbeeld - functies 2.x

Zie het voorbeeld taalspecifieke:

* [2.x C#](#2x-c-example)
* [2.x C# script (.csx)](#2x-c-script-example)
* [2.x JavaScript](#2x-javascript-example)

### <a name="2x-c-example"></a>2.x C#-voorbeeld

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die verstuurt een tekstbericht wanneer ze worden geactiveerd door een wachtrijbericht.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;
namespace TwilioQueueOutput
{
    public static class QueueTwilio
    {
        [FunctionName("QueueTwilio")]
        [return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX")]
        public static CreateMessageOptions Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
        ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed: {order}");

            var message = new CreateMessageOptions(new PhoneNumber(order["mobileNumber"].ToString()))
            {
                Body = $"Hello {order["name"]}, thanks for your order!"
            };

            return message;
        }
    }
}
```

In dit voorbeeld wordt de `TwilioSms` kenmerk met de geretourneerde waarde van de methode. Een alternatief is het gebruik van het kenmerk met een `out CreateMessageOptions` parameter of een `ICollector<CreateMessageOptions>` of `IAsyncCollector<CreateMessageOptions>` parameter.

### <a name="2x-c-script-example"></a>Voorbeeld van een script 2.x C#

Het volgende voorbeeld ziet u een Twilio-Uitvoerbinding een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie gebruikt een `out` parameter voor het verzenden van een SMS-bericht.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSidSetting": "TwilioAccountSid",
  "authTokenSetting": "TwilioAuthToken",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Hier volgt een C#-scriptcode:

```cs
#r "Newtonsoft.Json"
#r "Twilio"
#r "Microsoft.Azure.WebJobs.Extensions.Twilio"

using System;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Microsoft.Azure.WebJobs.Extensions.Twilio;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static void Run(string myQueueItem, out CreateMessageOptions message,  ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // You must initialize the CreateMessageOptions variable with the "To" phone number.
    message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message.
    message.Body = msg;
}
```

U kunt niet gebruiken in synchrone code uitvoerparameters. Hier volgt een asynchrone C#-code-voorbeeldscript:

```cs
#r "Newtonsoft.Json"
#r "Twilio"
#r "Microsoft.Azure.WebJobs.Extensions.Twilio"

using System;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;
using Microsoft.Azure.WebJobs.Extensions.Twilio;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static async Task Run(string myQueueItem, IAsyncCollector<CreateMessageOptions> message,  ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // You must initialize the CreateMessageOptions variable with the "To" phone number.
    CreateMessageOptions smsText = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message.
    smsText.Body = msg;

    await message.AddAsync(smsText);
}
```

### <a name="2x-javascript-example"></a>Voorbeeld van de 2.x JavaScript

Het volgende voorbeeld ziet u een Twilio-Uitvoerbinding een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Hier volgt binding-gegevens de *function.json* bestand:

Voorbeeld van de function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSidSetting": "TwilioAccountSid",
  "authTokenSetting": "TwilioAuthToken",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message in the binding, you must at least
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. The "To" number 
    // must be specified in code. 
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) kenmerk.

Zie voor meer informatie over de kenmerkeigenschappen die u kunt configureren, [configuratie](#configuration). Hier volgt een `TwilioSms` kenmerk voorbeeld in een handtekeningmethode:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX")]
public static CreateMessageOptions Run(
[QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, ILogger log)
{
    ...
}
 ```

Zie voor een compleet voorbeeld [C#-voorbeeld](#c-example).

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `TwilioSms` kenmerk.

| V1 function.json eigenschap | de eigenschap function.json v2 | De kenmerkeigenschap |Description|
|---------|---------|---------|----------------------|
|**type**|**type**| Moet worden ingesteld op `twilioSms`.|
|**direction**|**direction**| Moet worden ingesteld op `out`.|
|**name**|**name**| Naam van de variabele die wordt gebruikt in de functiecode aan te geven voor de Twilio-SMS-bericht. |
|**accountSid**|**accountSidSetting**| **AccountSidSetting**| Deze waarde moet worden ingesteld op de naam van een app-instelling met uw Twilio-Account-Sid bijvoorbeeld TwilioAccountSid. Als niet is ingesteld, de standaardapp-instelling is de naam 'AzureWebJobsTwilioAccountSid'. |
|**authToken**|**authTokenSetting**|**authTokenSetting**| Deze waarde moet worden ingesteld op de naam van een app-instelling met uw Twilio-verificatietoken bijvoorbeeld TwilioAccountAuthToken. Als niet is ingesteld, de standaardapp-instelling is de naam 'AzureWebJobsTwilioAuthToken'. |
|**to**| N.V.T. - opgeven in code | **Aan**| Deze waarde is ingesteld voor het telefoonnummer dat naar de SMS-tekst wordt verzonden.|
|**from**|**from** | **From**| Deze waarde is ingesteld voor het telefoonnummer dat door de SMS-tekst wordt verzonden.|
|**body**|**body** | **Hoofdtekst**| Deze waarde kan harde code voor de SMS-bericht als u niet wilt dynamisch instellen in de code voor uw functie worden gebruikt. |  

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
