---
title: Azure Functions-HTTP-triggers en bindingen
description: Over het gebruik van HTTP-triggers en bindingen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure functions, functies, gebeurtenissen verwerken, webhooks, dynamisch berekenen, serverloze architectuur, HTTP-API, REST
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 53626c1d8fe0b9301883280a9f0925eb38ad1d99
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480458"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>Azure Functions-HTTP-triggers en bindingen

In dit artikel wordt uitgelegd hoe u werken met HTTP-triggers en uitvoerbindingen in Azure Functions.

Een HTTP-trigger kan worden aangepast om te reageren op [webhooks](https://en.wikipedia.org/wiki/Webhook).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

De code in dit artikel standaard functies 2.x syntaxis die gebruikmaakt van .NET Core. Zie voor informatie over de syntaxis van de 1.x, de [1.x functies sjablonen](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="packages---functions-1x"></a>Pakketten - functies 1.x

De HTTP-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-pakket versie 1.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub-opslagplaats.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pakketten - functies 2.x

De HTTP-bindingen zijn opgegeven in de [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet-pakket versie 3.x. Broncode voor het pakket is in de [azure webjobs-sdk extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub-opslagplaats.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Trigger

De HTTP-trigger kunt u een functie met een HTTP-aanvraag aanroepen. U kunt een HTTP-trigger gebruiken om te bouwen serverloze API's en webhooks reageren.

Standaard retourneert een HTTP-trigger HTTP 200 OK met een lege hoofdtekst in functies 1.x of HTTP 204 geen inhoud met een lege hoofdtekst in functies 2.x. Configureren voor het wijzigen van het antwoord een [HTTP-Uitvoerbinding](#output).

## <a name="trigger---example"></a>Trigger - voorbeeld

Zie het voorbeeld taalspecifieke:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-examples)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Trigger - voorbeeld met C#

Het volgende voorbeeld wordt een [C#-functie](functions-dotnet-class-library.md) die zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag. U ziet dat de geretourneerde waarde wordt gebruikt voor de Uitvoerbinding, maar een kenmerk van de geretourneerde waarde is niet vereist.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

### <a name="trigger---c-script-example"></a>Trigger - voorbeeld van C#-script

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [C#-scriptfunctie](functions-reference-csharp.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt C#-script-code die wordt gebonden aan `HttpRequest`:

```cs
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

U kunt koppelen aan een aangepast object in plaats van `HttpRequest`. Dit object is gemaakt op basis van de hoofdtekst van de aanvraag en geparseerd als JSON. Op deze manier kan een type worden doorgegeven aan het antwoord van de HTTP-output-binding en geretourneerd als de antwoordtekst, samen met een 200-statuscode.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static string Run(Person person, ILogger log)
{   
    return person.Name != null
        ? (ActionResult)new OkObjectResult($"Hello, {person.Name}")
        : new BadRequestObjectResult("Please pass an instance of Person.");
}

public class Person {
     public string Name {get; set;}
}
```

### <a name="trigger---f-example"></a>Trigger - F# voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [ F# functie](functions-reference-fsharp.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de F# code:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

U moet een `project.json` bestand dat gebruikmaakt van NuGet om te verwijzen naar de `FSharp.Interop.Dynamic` en `Dynamitey` assembly's, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### <a name="trigger---javascript-example"></a>Trigger - JavaScript-voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [JavaScript-functie](functions-reference-node.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

### <a name="trigger---python-example"></a>Trigger - Python-voorbeeld

Het volgende voorbeeld ziet u de binding van een trigger in een *function.json* bestand en een [funkce Pythonu](functions-reference-python.md) die gebruikmaakt van de binding. De functie zoekt naar een `name` parameter in de query-tekenreeks of de hoofdtekst van de HTTP-aanvraag.

Hier volgt de *function.json* bestand:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Hier volgt de Python-code:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

### <a name="trigger---java-examples"></a>Trigger - Java-voorbeelden

* [Parameter lezen van de query-tekenreeks](#read-parameter-from-the-query-string-java)
* [Lezen van instantie van een POST-aanvraag](#read-body-from-a-post-request-java)
* [Parameter van een route lezen](#read-parameter-from-a-route-java)
* [Lezen POJO instantie van een POST-aanvraag](#read-pojo-body-from-a-post-request-java)

De volgende voorbeelden ziet u de HTTP-trigger binding in een *function.json* bestands- en de desbetreffende [Java-functies](functions-reference-java.md) die gebruikmaken van de binding. 

Hier volgt de *function.json* bestand:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "anonymous",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

#### <a name="read-parameter-from-the-query-string-java"></a>Parameter lezen van de query-tekenreeks (Java)  

In dit voorbeeld leest een parameter, met de naam ```id```, uit de queryreeks en gebruik maken van een JSON-document geretourneerd naar de client, met inhoudstype ```application/json```. 

```java
    @FunctionName("TriggerStringGet")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("GET parameters are: " + request.getQueryParameters());

        // Get named parameter
        String id = request.getQueryParameters().getOrDefault("id", "");

        // Convert and display
        if (id.isEmpty()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String name = "fake_name";
            final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                         "\"description\": \"" + name + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-body-from-a-post-request-java"></a>Lezen van instantie van een POST-aanvraag (Java)  

In dit voorbeeld leest de hoofdtekst van een POST-aanvraag als een ```String```, en gebruikt voor het bouwen van een JSON-document geretourneerd naar de client, met inhoudstype ```application/json```.

```java
    @FunctionName("TriggerStringPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(""));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String body = request.getBody().get();
            final String jsonDocument = "{\"id\":\"123456\", " + 
                                         "\"description\": \"" + body + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-parameter-from-a-route-java"></a>De parameter lezen van een route (Java)  

In dit voorbeeld leest een verplichte parameter, met de naam ```id```, en een optionele parameter ```name``` van de route pad, en maakt gebruik te maken van een JSON-document geretourneerd naar de client, met inhoudstype ```application/json```. T

```java
    @FunctionName("TriggerStringRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "trigger/{id}/{name=EMPTY}") // name is optional and defaults to EMPTY
            HttpRequestMessage<Optional<String>> request,
            @BindingName("id") String id,
            @BindingName("name") String name,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Route parameters are: " + id);

        // Convert and display
        if (id == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                         "\"description\": \"" + name + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-pojo-body-from-a-post-request-java"></a>Lezen POJO instantie van een POST-aanvraag (Java)  

Dit is de code voor de ```ToDoItem``` klasse, waarnaar wordt verwezen in dit voorbeeld:

```java

public class ToDoItem {

  private String id;
  private String description;  

  public ToDoItem(String id, String description) {
    this.id = id;
    this.description = description;
  }

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}

```

In dit voorbeeld leest de hoofdtekst van een POST-aanvraag. Hoofdtekst van de aanvraag wordt automatisch ongedaan maken met van serienummer in een ```ToDoItem``` object en wordt geretourneerd naar de client, met inhoudstype ```application/json```. De ```ToDoItem``` parameter door de Functions-runtime is geserialiseerd als deze is toegewezen aan de ```body``` eigenschap van de ```HttpMessageResponse.Builder``` klasse.

```java
    @FunctionName("TriggerPojoPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<ToDoItem>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(null));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final ToDoItem body = request.getBody().get();
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(body)
                          .build();
        }
    }
```

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [C#-klassebibliotheken](functions-dotnet-class-library.md), gebruikt u de [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) kenmerk.

U kunt de autorisatie niveau en toegestane HTTP-methoden instellen in kenmerk constructor parameters en er zijn eigenschappen van het type en de route sjabloon webhook. Zie voor meer informatie over deze instellingen [Trigger - configuratie](#trigger---configuration). Hier volgt een `HttpTrigger` kenmerk in een handtekeningmethode:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequest req)
{
    ...
}
```

Zie voor een compleet voorbeeld [Trigger - voorbeeld met C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `HttpTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Description|
|---------|---------|----------------------|
| **type** | N.v.t.| Vereist: moet worden ingesteld op `httpTrigger`. |
| **direction** | N.v.t.| Vereist: moet worden ingesteld op `in`. |
| **name** | N.v.t.| Vereist: de naam van de variabele die wordt gebruikt in de functiecode voor de aanvraag of de hoofdtekst van de aanvraag. |
| <a name="http-auth"></a>**authLevel** |  **authLevel** |Hiermee bepaalt u wat sleutels, indien van toepassing, aanwezig zijn op de aanvraag moeten om de functie aanroepen. Het autorisatieniveau kan een van de volgende waarden zijn: <ul><li><code>anonymous</code>&mdash;Er zijn geen API-sleutel is vereist.</li><li><code>function</code>&mdash;Een functie-specifieke API-sleutel is vereist. Dit is de standaardwaarde als niets wordt opgegeven.</li><li><code>admin</code>&mdash;De hoofdsleutel is vereist.</li></ul> Zie de sectie voor meer informatie over [sleutels voor de verificatieregel](#authorization-keys). |
| **methods** |**Methoden** | Een matrix met de HTTP-methoden waarop de functie reageert. Indien niet opgegeven, wordt de functie reageert op alle HTTP-methoden. Zie [aanpassen van het http-eindpunt](#customize-the-http-endpoint). |
| **route** | **Route** | Hiermee definieert u de Routesjabloon beheren waarop aanvraag-URL's die uw functie reageert. De standaardwaarde als niets wordt opgegeven is `<functionname>`. Zie voor meer informatie, [aanpassen van het http-eindpunt](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Alleen ondersteund voor de versie 1.x-runtime._<br/><br/>Hiermee wordt de HTTP-trigger om te fungeren als een [webhook](https://en.wikipedia.org/wiki/Webhook) ontvanger voor de opgegeven provider. Stel de `methods` eigenschap als u deze eigenschap is ingesteld. Het webhooktype kan een van de volgende waarden zijn:<ul><li><code>genericJson</code>&mdash;Een algemene webhook-eindpunt zonder logica voor een specifieke provider. Deze instelling aanvragen beperkt tot alleen die via HTTP POST en met de `application/json` type inhoud.</li><li><code>github</code>&mdash;De functie reageert op [GitHub webhooks](https://developer.github.com/webhooks/). Gebruik niet de _authLevel_ eigenschap met GitHub webhooks. Zie de sectie GitHub webhooks verderop in dit artikel voor meer informatie.</li><li><code>slack</code>&mdash;De functie reageert op [webhooks Slack](https://api.slack.com/outgoing-webhooks). Gebruik niet de _authLevel_ eigenschap met de Slack webhooks. Zie de sectie Slack webhooks verderop in dit artikel voor meer informatie.</li></ul>|

## <a name="trigger---usage"></a>Trigger - gebruik

Voor C# en F# functies, kunt u het type van de invoer voor een trigger declareren `HttpRequest` of een aangepast type. Als u ervoor kiest `HttpRequest`, krijgt u volledige toegang tot het request-object. Voor een type aangepaste probeert de runtime parseren van de hoofdtekst van de JSON-aanvraag voor het instellen van eigenschappen van het object.

De Functions-runtime biedt voor JavaScript-functies, de hoofdtekst van de aanvraag in plaats van het request-object. Zie voor meer informatie de [JavaScript trigger voorbeeld](#trigger---javascript-example).

### <a name="customize-the-http-endpoint"></a>Aanpassen van het HTTP-eindpunt

Wanneer u een functie voor een HTTP-trigger, maakt is de functie standaard beschikbaar met een route van het formulier:

    http://<yourapp>.azurewebsites.net/api/<funcname>

U kunt deze route met de optionele `route` eigenschap van de HTTP-trigger invoer van de binding. Als u bijvoorbeeld de volgende *function.json* -bestand definieert een `route` eigenschap voor een HTTP-trigger:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Met behulp van deze configuratie de functie is nu beschikbaar met de volgende route in plaats van de oorspronkelijke route.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Hiermee wordt de functiecode voor de ondersteuning van twee parameters in het adres _categorie_ en _id_. U kunt een [Web API Route beperking](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) met de parameters. De volgende C#-functiecode maakt gebruik van beide parameters.

```csharp
public static Task<IActionResult> Run(HttpRequest req, string category, int? id, ILogger log)
{
    if (id == null)
    {
        return (ActionResult)new OkObjectResult($"All {category} items were requested.");
    }
    else
    {
        return (ActionResult)new OkObjectResult($"{category} item with id = {id} has been requested.");
    }
    
    // -----
    log.LogInformation($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
}
```

Hier volgt een Node.js-functiecode die gebruikmaakt van de Routeparameters die.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
}
```

Standaard alle functie-routes worden voorafgegaan door *api*. U kunt ook aanpassen of verwijderen van het voorvoegsel via de `http.routePrefix` eigenschap in uw [host.json](functions-host-json.md) bestand. Het volgende voorbeeld verwijdert u de *api* route voorvoegsel met behulp van een lege tekenreeks voor het voorvoegsel in de *host.json* bestand.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="working-with-client-identities"></a>Werken met client-id 's

Als uw functie-app [App Service-verificatie / autorisatie](../app-service/overview-authentication-authorization.md), vindt u informatie over geverifieerde clients vanuit uw code. Deze informatie is beschikbaar als [aanvraagheaders geïnjecteerd door het platform](../app-service/app-service-authentication-how-to.md#access-user-claims). 

U kunt deze informatie ook lezen uit het binden van gegevens. Deze mogelijkheid is alleen beschikbaar voor de Functions-runtime 2.x. Het is ook momenteel alleen beschikbaar voor .NET-talen.

In .NET-talen, deze informatie is beschikbaar als een [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Het ClaimsPrincipal is beschikbaar als onderdeel van de context van de aanvraag zoals wordt weergegeven in het volgende voorbeeld:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
}
```

Het ClaimsPrincipal kan ook gewoon worden opgenomen als een extra parameter in de functiehandtekening:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}

```

### <a name="authorization-keys"></a>Sleutels voor de verificatieregel

Functions kunt u sleutels gebruiken voor het moeilijker voor toegang tot uw HTTP-eindpunten voor de functie tijdens de ontwikkeling.  Een standaard HTTP-trigger mogelijk dat die een API-sleutel worden gebruikt in de aanvraag. 

> [!IMPORTANT]
> Terwijl de sleutels kunnen helpen uw HTTP-eindpunten, onleesbaar maakt tijdens de ontwikkeling, zijn ze niet bedoeld als een manier voor het beveiligen van een HTTP-trigger in de productieomgeving. Zie voor meer informatie, [beveiligen van een HTTP-eindpunt in de productieomgeving](#secure-an-http-endpoint-in-production).

> [!NOTE]
> In de Functions-runtime 1.x kunnen webhook providers sleutels gebruiken voor het toestaan van aanvragen in verschillende manieren, afhankelijk van wat de provider ondersteunt. Dit wordt behandeld [Webhooks en sleutels](#webhooks-and-keys). De runtime versie 2.x bevat geen ingebouwde ondersteuning voor de webhook-providers.

Er zijn twee soorten sleutels:

* **Hostsleutels**: Deze sleutels worden gedeeld door alle functies in de functie-app. Wanneer gebruikt als een API-sleutel, geven deze toegang tot een functie binnen de functie-app.
* **Functietoetsen**: Deze sleutels gelden alleen voor de specifieke functies waarin ze zijn gedefinieerd. Wanneer gebruikt als een API-sleutel, geven alleen toegang tot die functie.

Elke sleutel is met de naam van verwijzing en er is een standaard-sleutel (met de naam 'standaard') op het niveau van de functie en de host. Functietoetsen hebben voorrang op de hostsleutels. Wanneer er twee sleutels zijn gedefinieerd met dezelfde naam, wordt de sleutel van de functie altijd gebruikt.

Elke functie-app heeft ook een speciale **hoofdsleutel**. Deze sleutel is de hostsleutel van een met de naam `_master`, waarmee u beheerderstoegang tot de runtime-API's. Deze sleutel kan niet worden ingetrokken. Tijdens het instellen van een autorisatie van `admin`, aanvragen moeten de hoofdsleutel; gebruiken een andere toets Autorisatiefout leidt.

> [!CAUTION]  
> Vanwege de verhoogde machtigingen in uw functie-app met de hoofdsleutel is verleend, moet u niet met derden delen van deze sleutel of deze in native clienttoepassingen te distribueren. Wees voorzichtig bij het kiezen van het autorisatieniveau van de beheerder.

### <a name="obtaining-keys"></a>Het verkrijgen van sleutels

Sleutels worden opgeslagen als onderdeel van uw functie-app in Azure en in rust worden versleuteld. Als u uw sleutels, nieuwe te maken of sleutels Ga naar de nieuwe waarden, gaat u naar een van uw HTTP-geactiveerde functies in de [Azure-portal](https://portal.azure.com) en selecteer **beheren**.

![Functie-sleutels in de portal beheren.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Er is geen ondersteunde API om via een programma te verkrijgen functietoetsen.

### <a name="api-key-authorization"></a>Autorisatiebeleid API

De meeste HTTP-trigger sjablonen vereisen een API-sleutel in de aanvraag. Dus lijkt de HTTP-aanvraag normaal op de volgende URL:

    https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>

De sleutel kan worden opgenomen in een queryreeks-variabele met de naam `code`, zoals hierboven. Kan ook worden opgenomen in een `x-functions-key` HTTP-header. De waarde van de sleutel kan zijn een functie-sleutel gedefinieerd voor de functie of een willekeurige toets host.

U kunt anonieme aanvragen, waarvoor geen sleutels. U kunt ook vereisen dat de hoofdsleutel worden gebruikt. U het standaardniveau voor autorisatie wijzigen met behulp van de `authLevel` eigenschap in de JSON van de binding. Zie voor meer informatie, [Trigger - configuratie](#trigger---configuration).

> [!NOTE]
> Wanneer u functies lokaal uitvoert, is autorisatie, ongeacht de instelling voor het opgegeven verificatietype uitgeschakeld. Nadat deze is gepubliceerd naar Azure, de `authLevel` instelling in de trigger wordt afgedwongen.



### <a name="secure-an-http-endpoint-in-production"></a>Beveiligen van een HTTP-eindpunt in de productieomgeving

Als u wilt uw functie-eindpunten in de productieomgeving volledig te beveiligen, moet u overwegen implementatie van een van de volgende opties voor functie-app-beveiliging:

* Inschakelen van App Service-verificatie / autorisatie voor uw functie-app. Het App Service-platform kunt Azure Active Directory (AAD) en diverse externe id-providers gebruiken om clients te verifiëren. U kunt deze gebruiken voor het implementeren van aangepaste regels voor uw functies en u kunt werken met de gebruikersinformatie uit uw functiecode aan te geven. Zie voor meer informatie, [verificatie en autorisatie in Azure App Service](../app-service/overview-authentication-authorization.md) en [werken met client-id's](#working-with-client-identities).

* Azure API Management (APIM) gebruiken om aanvragen te verifiëren. APIM biedt een verscheidenheid aan opties voor API-beveiliging voor binnenkomende aanvragen. Zie voor meer informatie, [API Management-verificatiebeleid](../api-management/api-management-authentication-policies.md). Met APIM op locatie, kunt u uw functie-app voor het accepteren van aanvragen van het IP-adres van de APIM-instantie te configureren. Zie voor meer informatie, [IP-adresbeperkingen](ip-addresses.md#ip-address-restrictions).

* Uw functie-app implementeren naar een Azure App Service Environment (ASE). As-omgeving biedt een toegewezen hosting omgeving waarin u kunt uw functies worden uitgevoerd. As-omgeving kunt u configureren van één front-gateway die u gebruiken kunt om alle inkomende aanvragen te verifiëren. Zie voor meer informatie, [configureren van een Web Application Firewall (WAF) voor App Service Environment](../app-service/environment/app-service-app-service-environment-web-application-firewall.md).

Wanneer u een van deze functie op app-niveau beveiligingsmethoden, moet u de verificatie van de HTTP-geactiveerde functie op instellen `anonymous`.

### <a name="webhooks"></a>Webhooks

> [!NOTE]
> Webhook-modus is alleen beschikbaar voor versie 1.x van de Functions-runtime. Deze wijziging is doorgevoerd ter verbetering van de prestaties van HTTP-triggers in versie 2.x.

In versie 1.x, webhook sjablonen bieden aanvullende validatie voor de webhook-nettoladingen. In versie 2.x, de basis HTTP-trigger nog werkt en is de aanbevolen aanpak voor webhooks. 

#### <a name="github-webhooks"></a>GitHub webhooks

Om te reageren op GitHub webhooks, eerst uw functie maken met een HTTP-Trigger en stel de **webHookType** eigenschap `github`. Kopieer de URL en API-sleutel in de **webhook toevoegen** pagina van uw GitHub-opslagplaats. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

#### <a name="slack-webhooks"></a>Slack-webhooks

De Slack-webhook genereert een token voor u in plaats van waarin u kunt opgeven, dus u een sleutel functiespecifieke met het token van Slack configureren moet. Zie [sleutels voor de verificatieregel](#authorization-keys).

### <a name="webhooks-and-keys"></a>Webhooks en sleutels

Autorisatie van de Webhook wordt verwerkt door het onderdeel van de ontvanger webhook, onderdeel van de HTTP-trigger, en het mechanisme is afhankelijk van het webhooktype. Elke methode zijn afhankelijk van een sleutel. De functie-sleutel met de naam 'standaard' wordt standaard gebruikt. Voor het gebruik van een andere productcode, de webhook-provider voor het verzenden van de naam van de sleutel met de aanvraag in een van de volgende manieren te configureren:

* **Querytekenreeks**: De provider geeft de naam van de sleutel in de `clientid` query-tekenreeksparameter, zoals `https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?clientid=<KEY_NAME>`.
* **Aanvraagkoptekst**: De provider geeft de naam van de sleutel in de `x-functions-clientid` header.

## <a name="trigger---limits"></a>Trigger - limieten

De lengte van de HTTP-aanvraag is beperkt tot 100 MB (104,857,600 bytes) en de URL-lengte is beperkt tot 4 KB (4096 bytes). Deze limieten zijn opgegeven door de `httpRuntime` element van de runtime van [Web.config-bestand](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Als een functie die wordt gebruikt niet de HTTP-trigger binnen ongeveer 2,5 minuten, de time-out van gateway wordt voltooid en retourneert een HTTP 502-fout. De functie blijft actief, maar kan niet naar een HTTP-antwoord retourneren. Voor langlopende functies, wordt aangeraden dat u gaat u als volgt de asynchrone patronen en retourneert een locatie waar u de status van de aanvraag kunt pingen. Zie voor informatie over hoe lang een functie kunt uitvoeren, [schaal en hosting - verbruiksabonnement](functions-scale.md#consumption-plan).

## <a name="trigger---hostjson-properties"></a>Trigger - eigenschappen voor host.json

De [host.json](functions-host-json.md) bestand bevat instellingen die het HTTP-trigger gedrag beheren.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Output

Gebruik de HTTP-Uitvoerbinding om te reageren op de afzender van HTTP-aanvraag. Deze binding vereist een HTTP-trigger en Hiermee kunt u het antwoord dat is gekoppeld aan de aanvraag van de trigger aanpassen. Als een HTTP-Uitvoerbinding is niet opgegeven, wordt een HTTP-trigger HTTP 200 OK geretourneerd met een lege hoofdtekst in functies 1.x of HTTP 204 geen inhoud met een lege hoofdtekst in functies 2.x.

## <a name="output---configuration"></a>Uitvoer - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand. Voor C#-klassebibliotheken, zijn er geen kenmerkeigenschappen die overeenkomen met deze *function.json* eigenschappen.

|Eigenschap  |Description  |
|---------|---------|
| **type** |Moet worden ingesteld op `http`. |
| **direction** | Moet worden ingesteld op `out`. |
|**name** | De naam van de variabele in functiecode gebruikt voor het antwoord of `$return` de geretourneerde waarde gebruiken. |

## <a name="output---usage"></a>Uitvoer - gebruik

Gebruik de taal-standaard antwoord patronen voor het verzenden van een HTTP-antwoord. In C# of C#-script, moet u de functie retourtype `IActionResult` of `Task<IActionResult>`. In C#, is een kenmerk van de geretourneerde waarde niet vereist.

Bijvoorbeeld antwoorden, Zie de [trigger voorbeeld](#trigger---example).

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
