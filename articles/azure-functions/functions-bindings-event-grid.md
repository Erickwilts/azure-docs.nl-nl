---
title: Trigger Gebeurtenisraster voor Azure Functions
description: Lees hoe u aan het verwerken van Event Grid-gebeurtenissen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/04/2018
ms.author: cshoe
ms.openlocfilehash: 50ca0dd2555902ee9195abdd7a8fb71f81222cc8
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342288"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Trigger Gebeurtenisraster voor Azure Functions

In dit artikel wordt uitgelegd hoe u voor het afhandelen van [Event Grid](../event-grid/overview.md) gebeurtenissen in Azure Functions.

Event Grid is een Azure-service waarmee HTTP-aanvragen om u te waarschuwen over gebeurtenissen die worden verzonden *uitgevers*. Een uitgever is de service of resource dat afkomstig is van de gebeurtenis. Een Azure blob storage-account is bijvoorbeeld een uitgever en [een blob uploaden of de verwijdering is een gebeurtenis](../storage/blobs/storage-blob-event-overview.md). Sommige [Azure-services hebben ingebouwde ondersteuning voor het publiceren van gebeurtenissen naar Event Grid](../event-grid/overview.md#event-sources).

Gebeurtenis *handlers* ontvangen en verwerken van gebeurtenissen. Azure Functions is een van verschillende [Azure-services die ingebouwde ondersteuning hebben voor het verwerken van Event Grid-gebeurtenissen](../event-grid/overview.md#event-handlers). In dit artikel leert u hoe u een Event Grid-trigger met een functie aanroepen wanneer een gebeurtenis wordt ontvangen van Event Grid.

Als u liever, kunt u een HTTP-trigger voor het afhandelen van Event Grid-gebeurtenissen; Zie [een HTTP-trigger gebruiken als een trigger Gebeurtenisraster](#use-an-http-trigger-as-an-event-grid-trigger) verderop in dit artikel. Op dit moment kunt u een Event Grid-trigger voor een Azure Functions-app wanneer de gebeurtenis wordt geleverd in de [een CloudEvents-schema](../event-grid/cloudevents-schema.md). Gebruik in plaats daarvan een HTTP-trigger.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De trigger van Event Grid is opgegeven in de [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet-pakket versie 2.x. Broncode voor het pakket is in de [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x) GitHub-opslagplaats.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De trigger van Event Grid is opgegeven in de [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Voorbeeld

Zie het voorbeeld taalspecifieke voor een trigger van Event Grid:

* C#
* [C# script (.csx)](#c-script-example)
* [Java](#trigger---java-examples)
* [JavaScript](#javascript-example)
* [Python](#python-example)

Zie voor een voorbeeld van de trigger HTTP [over het gebruik van HTTP-trigger](#use-an-http-trigger-as-an-event-grid-trigger) verderop in dit artikel.

### <a name="c-2x"></a>C# (2.x)

Het volgende voorbeeld ziet u een functies 2.x [C#-functie](functions-dotnet-class-library.md) die wordt gebonden aan `EventGridEvent`:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Zie voor meer informatie, pakketten, [kenmerken](#attributes), [configuratie](#configuration), en [gebruik](#usage).

### <a name="c-version-1x"></a>C#(Versie 1.x)

Het volgende voorbeeld ziet u een functies 1.x [C#-functie](functions-dotnet-class-library.md) die wordt gebonden aan `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

### <a name="c-script-example"></a>Voorbeeld van C#-script

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-script-version-2x"></a>C#script (versie 2.x)

Hier volgt Functions 2.x C#-script-code die wordt gebonden aan `EventGridEvent`:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

Zie voor meer informatie, pakketten, [kenmerken](#attributes), [configuratie](#configuration), en [gebruik](#usage).

#### <a name="c-script-version-1x"></a>C#script (versie 1.x)

Hier volgt Functions 1.x C#-script-code die wordt gebonden aan `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

### <a name="javascript-example"></a>JavaScript-voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Dit is de JavaScript-code:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

### <a name="python-example"></a>Python-voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [funkce Pythonu](functions-reference-python.md) die gebruikmaakt van de binding.

Hier volgt de binding-gegevens de *function.json* bestand:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "event",
      "direction": "in"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Hier volgt de Python-code:

```python
import logging
import azure.functions as func


def main(event: func.EventGridEvent):
    logging.info("Python Event Grid function processed a request.")
    logging.info("  Subject: %s", event.subject)
    logging.info("  Time: %s", event.event_time)
    logging.info("  Data: %s", event.get_json())
```

### <a name="trigger---java-examples"></a>Trigger - Java-voorbeelden

Deze sectie bevat de volgende voorbeelden:

* [Trigger Gebeurtenisraster, queryreeks-parameter](#event-grid-trigger-string-parameter-java)
* [Event Grid trigger, POJO parameter](#event-grid-trigger-pojo-parameter-java)

De volgende voorbeelden ziet u de binding van de trigger in een *function.json* bestand en [Java-functies](functions-reference-java.md) die de binding kunt gebruiken en afdrukken van een gebeurtenis, ontvangt eerst de gebeurtenis als ```String``` en het tweede als een POJO.

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ]
}
```

#### <a name="event-grid-trigger-string-parameter-java"></a>Event Grid trigger, String parameter (Java)

```java
  @FunctionName("eventGridMonitorString")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    String content, 
    final ExecutionContext context) {
      // log 
      context.getLogger().info("Event content: " + content);      
  }
```

#### <a name="event-grid-trigger-pojo-parameter-java"></a>Event Grid trigger, POJO parameter (Java)

In dit voorbeeld wordt de volgende POJO, dat de eigenschappen op het hoogste niveau van een Event Grid-gebeurtenis:

```java
import java.util.Date;
import java.util.Map;

public class EventSchema {

  public String topic;
  public String subject;
  public String eventType;
  public Date eventTime;
  public String id;
  public String dataVersion;
  public String metadataVersion;
  public Map<String, Object> data;

}
```

Bij aankomst, JSON-nettolading van de gebeurtenis is opgeheven geserialiseerd in de ```EventSchema``` POJO voor gebruik door de functie. Hiermee wordt de functie voor toegang tot de eigenschappen van de gebeurtenis in een objectgeoriënteerde manier.

```java
  @FunctionName("eventGridMonitor")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    EventSchema event, 
    final ExecutionContext context) {
      // log 
      context.getLogger().info("Event content: ");
      context.getLogger().info("Subject: " + event.subject);
      context.getLogger().info("Time: " + event.eventTime); // automatically converted to Date by the runtime
      context.getLogger().info("Id: " + event.id);
      context.getLogger().info("Data: " + event.data);
  }
```

In de [Java functions runtime library](/java/api/overview/azure/functions/runtime), gebruikt u de `EventGridTrigger` aantekening op waarvan de waarde afkomstig van EventGrid zijn kan parameters. Parameters met deze aantekeningen ertoe leiden dat de functie moet worden uitgevoerd wanneer een gebeurtenis wordt ontvangen.  Deze aantekening kan worden gebruikt met systeemeigen Java-typen, pojo's of null-waarden met behulp van `Optional<T>`.

## <a name="attributes"></a>Kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) kenmerk.

Hier volgt een `EventGridTrigger` kenmerk in een handtekeningmethode:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

Zie voor een compleet voorbeeld C# voorbeeld.

## <a name="configuration"></a>Configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand. Er zijn geen parameters van constructor of de eigenschappen instellen in de `EventGridTrigger` kenmerk.

|de eigenschap Function.JSON |Description|
|---------|---------|
| **type** | Vereist: moet worden ingesteld op `eventGridTrigger`. |
| **direction** | Vereist: moet worden ingesteld op `in`. |
| **name** | Vereist: de naam van de variabele die wordt gebruikt in functiecode aan te geven voor de parameter die gegevens van de gebeurtenis ontvangt. |

## <a name="usage"></a>Gebruik

Voor C# en F# functies in Azure 1.x functies, kunt u de volgende parametertypen voor de trigger van Event Grid:

* `JObject`
* `string`

Voor C# en F# functies in Azure Functions 2.x gebruikt, hebt u ook de mogelijkheid met het volgende parametertype voor de trigger van Event Grid:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`-Definieert de eigenschappen voor de overeenkomende velden op alle gebeurtenistypen.

> [!NOTE]
> In v1 functies als u probeert te binden aan `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`, de compiler wordt een "afgeschaft" bericht weergeven en u kunt het gebruik van `Microsoft.Azure.EventGrid.Models.EventGridEvent` in plaats daarvan. Voor het gebruik van het nieuwe type dat verwijst naar de [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet verpakt en volledig te kwalificeren de `EventGridEvent` typenaam door het voorvoegsel met `Microsoft.Azure.EventGrid.Models`. Zie voor meer informatie over hoe u om te verwijzen naar NuGet-pakketten in een C#-scriptfunctie [met behulp van NuGet-pakketten](functions-reference-csharp.md#using-nuget-packages)

Voor JavaScript-functies, de parameter met de naam van de *function.json* `name` eigenschap bevat een verwijzing naar de event-object.

## <a name="event-schema"></a>Gebeurtenisschema

Gegevens voor een Event Grid-gebeurtenis wordt ontvangen als een JSON-object in de hoofdtekst van een HTTP-aanvraag. De JSON ziet eruit zoals in het volgende voorbeeld:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Het voorbeeld is een matrix met één element. Event Grid altijd verzendt een matrix en meer dan een gebeurtenis in de matrix kan worden verzonden. De runtime aanroept uw functie eenmaal voor elk matrixelement.

De eigenschappen op het hoogste niveau in de JSON-gegevens van de gebeurtenis zijn gelijk in alle gebeurtenistypen, terwijl de inhoud van de `data` eigenschap zijn specifiek voor elk gebeurtenistype. Het voorbeeld is voor een blob storage-gebeurtenis.

Zie voor een uitleg van de gemeenschappelijke en gebeurtenis-specifieke eigenschappen [gebeurteniseigenschappen](../event-grid/event-schema.md#event-properties) in de documentatie voor Event Grid.

De `EventGridEvent` Hiermee geeft u alleen de eigenschappen van de op het hoogste niveau; de `Data` eigenschap is een `JObject`.

## <a name="create-a-subscription"></a>Een abonnement maken

Voor het starten van Event Grid HTTP-aanvragen ontvangen, maakt u een Event Grid-abonnement waarmee de eindpunt-URL die de functie activeert.

### <a name="azure-portal"></a>Azure Portal

Selecteer voor de functies die u hebt ontwikkeld in de Azure-portal met de trigger van Event Grid, **toevoegen Event Grid-abonnement**.

![Abonnement maken in portal](media/functions-bindings-event-grid/portal-sub-create.png)

Als u deze koppeling selecteren, de portal wordt geopend de **gebeurtenisabonnement maken** pagina met de eindpunt-URL zijn ingevuld.

![Eindpunt-URL automatisch ingevuld](media/functions-bindings-event-grid/endpoint-url.png)

Zie voor meer informatie over het maken van abonnementen met behulp van de Azure-portal [aangepaste gebeurtenis maken - Azure-portal](../event-grid/custom-event-quickstart-portal.md) in de documentatie voor Event Grid.

### <a name="azure-cli"></a>Azure-CLI

Een abonnement maken met behulp van [de Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), gebruikt u de [az eventgrid gebeurtenisabonnement maken](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create) opdracht.

De opdracht moet de eindpunt-URL die de functie activeert. Het volgende voorbeeld ziet u de versie-specifieke URL-patroon:

#### <a name="version-2x-runtime"></a>Runtime versie 2.x

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>Versie 1.x runtime

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

De systeemsleutel is een autorisatiesleutel die moet worden opgenomen in de eindpunt-URL voor een Event Grid-trigger. De volgende sectie wordt uitgelegd hoe u de systeemsleutel ophalen.

Hier volgt een voorbeeld waarin ze zich op blob storage-accounts (met een tijdelijke aanduiding voor de systeemsleutel abonneren):

#### <a name="version-2x-runtime"></a>Runtime versie 2.x

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>Versie 1.x runtime

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

Zie voor meer informatie over het maken van een abonnement [de blob storage-Quick-Start](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) of de andere snelstartgidsen van Event Grid.

### <a name="get-the-system-key"></a>Het systeemsleutel ophalen

U kunt de systeemsleutel krijgen met behulp van de volgende API (HTTP GET):

#### <a name="version-2x-runtime"></a>Runtime versie 2.x

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>Versie 1.x runtime

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

Dit is een beheer-API, zodat u hiervoor de functie-app [hoofdsleutel](functions-bindings-http-webhook.md#authorization-keys). Is niet hetzelfde als de systeemsleutel (voor het aanroepen van een functie van de trigger Gebeurtenisraster) met de hoofdsleutel (voor administratieve taken uitvoeren voor de functie-app). Wanneer u zich op een Event Grid-onderwerp abonneert, moet u de sleutel van het systeem.

Hier volgt een voorbeeld van het antwoord dat de systeemsleutel biedt:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

U kunt de hoofdsleutel ophalen voor uw functie-app uit de **functie app-instellingen** tabblad in de portal.

> [!IMPORTANT]
> De hoofdsleutel biedt beheerderstoegang tot uw functie-app. Geen deze sleutel delen met derden of deze in native clienttoepassingen te distribueren.

Zie voor meer informatie, [sleutels voor de verificatieregel](functions-bindings-http-webhook.md#authorization-keys) in het referentieartikel van HTTP-trigger.

U kunt ook kunt u een HTTP PUT om op te geven van de sleutelwaarde uzelf verzenden.

## <a name="local-testing-with-viewer-web-app"></a>Lokaal testen met de viewer-web-app

Als u wilt testen van een trigger Gebeurtenisraster lokaal hebt om op te halen van Event Grid HTTP-aanvragen die worden geleverd bij de oorsprong in de cloud naar uw lokale computer. Eén manier om dat te doen is door het vastleggen van aanvragen voor online- en handmatig opnieuw verzenden van deze op uw lokale computer:

1. [Een viewer voor web-app maken](#create-a-viewer-web-app) die berichten voor gebeurtenissen worden vastgelegd.
1. [Een Event Grid-abonnement maken](#create-an-event-grid-subscription) die gebeurtenissen naar de viewer-app verzonden.
1. [Een aanvraag genereert](#generate-a-request) en kopieer de aanvraagtekst van de viewer-app.
1. [De aanvraag voor het handmatig boeken](#manually-post-the-request) functie activeren in de localhost-URL van uw Event Grid.

Wanneer u klaar bent testen, kunt u hetzelfde abonnement voor de productie door het eindpunt bij te werken. Gebruik de [az eventgrid gebeurtenisabonnement update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI-opdracht.

### <a name="create-a-viewer-web-app"></a>Een viewer voor web-app maken

Als u wilt vastleggen gebeurtenisberichten vereenvoudigen, kunt u implementeren een [vooraf gemaakte web-app](https://github.com/Azure-Samples/azure-event-grid-viewer) waarmee de gebeurtenisberichten worden weergegeven. De geïmplementeerde oplossing omvat een App Service-plan, een App Service-web-app en broncode van GitHub.

Selecteer **Implementeren in Azure** om de oplossing voor uw abonnement te implementeren. Geef in Azure Portal waarden op voor de parameters.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

De implementatie kan enkele minuten duren. Controleer of uw web-app wordt uitgevoerd nadat de implementatie is voltooid. Navigeer in een webbrowser naar: `https://<your-site-name>.azurewebsites.net`

De site wordt weergegeven, maar er zijn nog geen gebeurtenissen op gepubliceerd.

![Nieuwe site weergeven](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Een Event Grid-abonnement maken

Maken van een Event Grid-abonnement van het type dat u wilt testen en wijs hieraan de URL van uw web-app als het eindpunt voor de gebeurtenismelding. Het eindpunt voor uw web-app moet het achtervoegsel `/api/updates/` bevatten. De volledige URL is dus `https://<your-site-name>.azurewebsites.net/api/updates`

Zie voor meer informatie over het maken van abonnementen met behulp van de Azure-portal [aangepaste gebeurtenis maken - Azure-portal](../event-grid/custom-event-quickstart-portal.md) in de documentatie voor Event Grid.

### <a name="generate-a-request"></a>Een aanvraag genereren

Een gebeurtenis die HTTP-verkeer naar het eindpunt van uw web-app wordt gegenereerd.  Bijvoorbeeld, als u een blob storage-abonnement hebt gemaakt, uploaden of verwijderen van een blob. Wanneer een aanvraag weergegeven in uw web-app wordt, kopieert u de aanvraagtekst.

De validatie van abonnementsaanvraag worden eerst; ontvangen validatie van aanvragen te negeren en de aanvraag van systeemgebeurtenis te kopiëren.

![Hoofdtekst van de aanvraag van web-app kopiëren](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>Handmatig boeken van de aanvraag

Uw Event Grid-functie lokaal uitvoeren.

Gebruik een hulpprogramma zoals [Postman](https://www.getpostman.com/) of [curl](https://curl.haxx.se/docs/httpscripting.html) te maken van een HTTP POST-aanvraag:

* Stel een `Content-Type: application/json` header.
* Stel een `aeg-event-type: Notification` header.
* Plak de RequestBin-gegevens in de aanvraagtekst.
* Een bericht in de URL van de trigger-functie van Event Grid.
  * Gebruik het volgende patroon voor 2.x:

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * Voor 1.x-gebruik:

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

De `functionName` parameter moet de naam die is opgegeven de `FunctionName` kenmerk.

De volgende schermafbeeldingen tonen de headers en de hoofdtekst in Postman:

![Headers in Postman](media/functions-bindings-event-grid/postman2.png)

![Het hoofdgedeelte van de aanvraag in Postman](media/functions-bindings-event-grid/postman.png)

De functie Event Grid-trigger wordt uitgevoerd en toont de logboeken die lijkt op het volgende voorbeeld:

![Voorbeeld van Event Grid trigger functielogboeken](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Lokaal testen met ngrok

Een andere manier voor het testen van een Event Grid-trigger lokaal is voor het automatiseren van de HTTP-verbinding tussen Internet en de ontwikkelcomputer. U kunt dit doen met een hulpprogramma zoals [ngrok](https://ngrok.com/):

1. [Maak een eindpunt ngrok](#create-an-ngrok-endpoint).
1. [Uitvoeren van de functie van de trigger Gebeurtenisraster](#run-the-event-grid-trigger-function).
1. [Een Event Grid-abonnement maken](#create-a-subscription) die gebeurtenissen naar het eindpunt ngrok verzonden.
1. [Een gebeurtenis activeren](#trigger-an-event).

Wanneer u klaar bent testen, kunt u hetzelfde abonnement voor de productie door het eindpunt bij te werken. Gebruik de [az eventgrid gebeurtenisabonnement update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI-opdracht.

### <a name="create-an-ngrok-endpoint"></a>Een eindpunt ngrok maken

Download *ngrok.exe* van [ngrok](https://ngrok.com/), en uit te voeren met de volgende opdracht:

```
ngrok http -host-header=localhost 7071
```

De - host-headerparameter is nodig omdat de functions-runtime wordt verwacht aanvragen van localhost dat wanneer deze wordt uitgevoerd op localhost. 7071 is het standaardpoortnummer wanneer de runtime wordt lokaal uitgevoerd.

De opdracht maakt u de volgende uitvoer:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

U gebruikt de `https://{subdomain}.ngrok.io` URL voor uw Event Grid-abonnement.

### <a name="run-the-event-grid-trigger-function"></a>De functie van de trigger Gebeurtenisraster uitvoeren

De URL ngrok krijgt niet speciale handelingen door Event Grid, u, zodat uw functie moet lokaal worden uitgevoerd wanneer het abonnement wordt gemaakt. Als dit niet is, wordt de validatie-antwoord wordt niet verzonden en mislukt het maken van het abonnement.

### <a name="create-a-subscription"></a>Een abonnement maken

Maken van een Event Grid-abonnement van het type dat u wilt testen en wijs hieraan uw ngrok-eindpunt.

Gebruik dit patroon eindpunt voor functies 2.x:

```
https://{SUBDOMAIN}.ngrok.io/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
```

Gebruik dit patroon eindpunt voor functies 1.x:

```
https://{SUBDOMAIN}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
```

De `{FUNCTION_NAME}` parameter moet de naam die is opgegeven de `FunctionName` kenmerk.

Hier volgt een voorbeeld met de Azure CLI:

```azurecli
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/runtime/webhooks/eventgrid?functionName=EventGridTrigger
```

Zie voor meer informatie over het maken van een abonnement [maken van een abonnement](#create-a-subscription) eerder in dit artikel.

### <a name="trigger-an-event"></a>Een gebeurtenis activeren

Een gebeurtenis die HTTP-verkeer naar uw ngrok-eindpunt wordt gegenereerd.  Bijvoorbeeld, als u een blob storage-abonnement hebt gemaakt, uploaden of verwijderen van een blob.

De functie Event Grid-trigger wordt uitgevoerd en toont de logboeken die lijkt op het volgende voorbeeld:

![Voorbeeld van Event Grid trigger functielogboeken](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Een HTTP-trigger gebruiken als een trigger Gebeurtenisraster

Event Grid-gebeurtenissen worden ontvangen als HTTP-aanvragen, zodat u gebeurtenissen verwerken kunt met behulp van een HTTP-trigger in plaats van een Event Grid-trigger. Een mogelijke reden om dat te doen is om op te halen meer controle over de eindpunt-URL die de functie activeert. Een andere reden is wanneer u nodig hebt voor het ontvangen van gebeurtenissen in de [een CloudEvents-schema](../event-grid/cloudevents-schema.md). De trigger Gebeurtenisraster ondersteunt op dit moment niet de een CloudEvents-schema. De voorbeelden in deze sectie ziet oplossingen voor Event Grid-schema- en een CloudEvents-schema.

Als u een HTTP-trigger gebruikt, hebt u code schrijven voor wat de trigger van Event Grid automatisch worden:

* Stuurt een antwoord validatie naar een [validatie abonnementsaanvraag](../event-grid/security-authentication.md#webhook-event-delivery).
* Hiermee wordt de functie één keer per element van de gebeurtenis-matrix die is opgenomen in de aanvraagtekst.

Zie voor meer informatie over de URL moet worden gebruikt voor het aanroepen van de functie lokaal of wanneer deze wordt uitgevoerd in Azure, de [HTTP-trigger binding-referentiedocumentatie](functions-bindings-http-webhook.md)

### <a name="event-grid-schema"></a>Event Grid-schema

Het volgende voorbeeld van C#-code voor een HTTP-trigger simuleert Event Grid trigger gedrag. Dit voorbeeld gebruiken voor gebeurtenissen die in de Event Grid-schema worden geleverd.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.LogInformation("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.LogInformation($"Subject: {eventGridEvent.Subject}");
        log.LogInformation($"Time: {eventGridEvent.EventTime}");
        log.LogInformation($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

De volgende JavaScript-code voor een HTTP-trigger simuleert Event Grid trigger gedrag. Dit voorbeeld gebruiken voor gebeurtenissen die in de Event Grid-schema worden geleverd.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = messages[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        // Event Grid schema delivers events in an array.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Uw code gebeurtenisafhandeling gaat binnen de lus via de `messages` matrix.

### <a name="cloudevents-schema"></a>Een CloudEvents-schema

Het volgende voorbeeld van C#-code voor een HTTP-trigger simuleert Event Grid trigger gedrag.  Dit voorbeeld gebruiken voor gebeurtenissen die worden geleverd in de een CloudEvents-schema.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    if (message.Type == JTokenType.Array)
    {
        // If the request is for subscription validation, send back the validation code.
        if (string.Equals((string)message[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
        {
            log.LogInformation("Validate request received");
            return req.CreateResponse<object>(new
            {
                validationResponse = message[0]["data"]["validationCode"]
            });
        }
    }
    else
    {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        log.LogInformation($"Source: {message["source"]}");
        log.LogInformation($"Time: {message["eventTime"]}");
        log.LogInformation($"Event data: {message["data"].ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

De volgende JavaScript-code voor een HTTP-trigger simuleert Event Grid trigger gedrag. Dit voorbeeld gebruiken voor gebeurtenissen die worden geleverd in de een CloudEvents-schema.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var message = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (message.length > 0 && message[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = message[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
    context.done();
};
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Meer informatie over Event Grid](../event-grid/overview.md)
