---
title: Trigger voor Azure Functions opwarm
description: Meer informatie over het gebruik van de opwarm-trigger in Azure Functions.
documentationcenter: na
author: alexkarcher-msft
manager: gwallace
keywords: Azure functions, functies, gebeurtenis verwerking, opwarm, koude start, Premium, dynamische compute, serverloze architectuur
ms.service: azure-functions
ms.topic: reference
ms.date: 11/08/2019
ms.author: alkarche
ms.openlocfilehash: a947ff11fbbe418af84ff49033cba3857bb8447f
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74925184"
---
# <a name="azure-functions-warm-up-trigger"></a>Opwarmende trigger Azure Functions

In dit artikel wordt uitgelegd hoe u kunt werken met de trigger opwarm in Azure Functions. De trigger opwarm wordt alleen ondersteund voor functie-apps die worden uitgevoerd in een [Premium-abonnement](functions-premium-plan.md). Er wordt een opwarm-trigger aangeroepen wanneer een exemplaar wordt toegevoegd om een actieve functie-app te schalen. U kunt een opwarm-trigger gebruiken om vooraf aangepaste afhankelijkheden te laden tijdens het [voorbereidings proces](./functions-premium-plan.md#pre-warmed-instances) , zodat uw functies gereed zijn om het verwerken van aanvragen onmiddellijk te starten. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Pakketten-functions 2. x en hoger

Het pakket [micro soft. Azure. webjobs. Extensions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet, versie **3.0.5 of hoger** is vereist. De bron code voor het pakket bevindt zich in de GitHub-opslag plaats [Azure-webjobs-SDK-Extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) . 

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Trigger

Met de trigger opwarm kunt u een functie definiëren die wordt uitgevoerd op een instantie wanneer deze wordt toegevoegd aan uw actieve app. U kunt een opwarm-functie gebruiken om verbindingen te openen, afhankelijkheden te laden of andere aangepaste logica uit te voeren voordat uw app verkeer ontvangt. 

De trigger opwarm is bedoeld voor het maken van gedeelde afhankelijkheden die worden gebruikt door de andere functies in uw app. [Zie hier voor voor beelden van gedeelde afhankelijkheden](./manage-connections.md#client-code-examples).

Houd er rekening mee dat de trigger opwarm alleen wordt aangeroepen tijdens opschalen, niet tijdens het opnieuw opstarten of andere niet-schaal bare opstart bewerkingen. U moet ervoor zorgen dat uw logica alle vereiste afhankelijkheden kan laden zonder gebruik te maken van de opwarm-trigger. Luie lading is een goed patroon om dit te doen.

## <a name="trigger---example"></a>Trigger - voorbeeld

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

In het volgende voor beeld ziet u een [ C# functie](functions-dotnet-class-library.md) die wordt uitgevoerd op elk nieuw exemplaar wanneer deze wordt toegevoegd aan uw app. Een retour waarde-kenmerk is niet vereist.


* De functie moet een naam hebben ```warmup``` (niet hoofdletter gevoelig) en er mag slechts één opwarm-functie per app zijn.
* Als u opwarm als een .NET-klassen bibliotheek functie wilt gebruiken, moet u een pakket verwijzing naar **micro soft. Azure. webjobs. Extensions > = 3.0.5**
    * ```<PackageReference Include="Microsoft.Azure.WebJobs.Extensions" Version="3.0.5" />```


In de tijdelijke aanduiding opmerkingen wordt aangegeven waar in de toepassing gedeelde afhankelijkheden moeten worden gedeclareerd en geïnitialiseerd. 
Meer [informatie over gedeelde afhankelijkheden vindt u hier](./manage-connections.md#client-code-examples).

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;
 
namespace WarmupSample
{

    //Declare shared dependencies here

    public static class Warmup
    {
        [FunctionName("Warmup")]
        public static void Run([WarmupTrigger()] WarmupContext context,
            ILogger log)
        {
            //Initialize shared dependencies here
            
            log.LogInformation("Function App instance is warm 🌞🌞🌞");
        }
    }
}
```
# <a name="c-scripttabcsharp-script"></a>[C#Schriften](#tab/csharp-script)


In het volgende voor beeld ziet u een opwarm-trigger in een *Function. json* -bestand en een [ C# script-functie](functions-reference-csharp.md) die wordt uitgevoerd op elk nieuw exemplaar wanneer het wordt toegevoegd aan uw app.

De functie moet de naam ```warmup``` (niet hoofdletter gevoelig) hebben en er mag slechts één opwarm-functie per app zijn.

Hier volgt de *function.json* bestand:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is C# de script code die wordt gekoppeld aan `HttpRequest`:

```cs
public static void Run(ILogger log)
{
    log.LogInformation("Function App instance is warm 🌞🌞🌞");  
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

In het volgende voor beeld ziet u een opwarm-trigger in een *Function. json* -bestand en een [Java script-functie](functions-reference-node.md) die wordt uitgevoerd op elk nieuw exemplaar wanneer het wordt toegevoegd aan uw app.

De functie moet een naam hebben ```warmup``` (niet hoofdletter gevoelig) en er mag slechts één opwarm-functie per app zijn.

Hier volgt de *function.json* bestand:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de JavaScript-code:

```javascript
module.exports = async function (context, warmupContext) {
    context.log('Function App instance is warm 🌞🌞🌞');
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

In het volgende voor beeld ziet u een opwarm-trigger in een *Function. json* -bestand en een [python-functie](functions-reference-python.md) die wordt uitgevoerd op elk nieuw exemplaar wanneer het wordt toegevoegd aan uw app.

De functie moet een naam hebben ```warmup``` (niet hoofdletter gevoelig) en er mag slechts één opwarm-functie per app zijn.

Hier volgt de *function.json* bestand:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

De [configuratie](#trigger---configuration) sectie wordt uitgelegd dat deze eigenschappen.

Dit is de python-code:

```python
import logging
import azure.functions as func


def main(warmupContext: func.Context) -> None:
    logging.info('Function App instance is warm 🌞🌞🌞')
```

# <a name="javatabjava"></a>[Java](#tab/java)

In het volgende voor beeld ziet u een opwarm-trigger in een *Function. json* -bestand en een [Java-functie](functions-reference-java.md) die wordt uitgevoerd op elk nieuw exemplaar wanneer het wordt toegevoegd aan uw app.

De functie moet een naam hebben ```warmup``` (niet hoofdletter gevoelig) en er mag slechts één opwarm-functie per app zijn.

Hier volgt de *function.json* bestand:

```json
{
    "bindings": [
        {
            "type": "warmupTrigger",
            "direction": "in",
            "name": "warmupContext"
        }
    ]
}
```

Dit is de Java-code:

```java
@FunctionName("Warmup")
public void run( ExecutionContext context) {
       context.getLogger().info("Function App instance is warm 🌞🌞🌞");
}
```

---

## <a name="trigger---attributes"></a>Trigger - kenmerken

In [ C# class-bibliotheken](functions-dotnet-class-library.md)is het kenmerk `WarmupTrigger` beschikbaar voor het configureren van de functie.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

In dit voor beeld ziet u hoe u het kenmerk [opwarm](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions/Extensions/Warmup/Trigger/WarmupTriggerAttribute.cs) gebruikt.

Houd er rekening mee dat uw functie moet worden aangeroepen ```Warmup``` en dat er slechts één opwarm-functie per app kan zijn.

```csharp
 [FunctionName("Warmup")]
        public static void Run(
            [WarmupTrigger()] WarmupContext context, ILogger log)
        {
            ...
        }
```

Zie voor een volledig voor beeld het [voor beeld](#trigger---example)van de trigger.

# <a name="c-scripttabcsharp-script"></a>[C#Schriften](#tab/csharp-script)

Kenmerken worden niet ondersteund door C# het script.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Kenmerken worden niet ondersteund door Java script.

# <a name="pythontabpython"></a>[Python](#tab/python)

Kenmerken worden niet ondersteund door python.

# <a name="javatabjava"></a>[Java](#tab/java)

De trigger opwarm wordt niet ondersteund in Java als een kenmerk.

---

## <a name="trigger---configuration"></a>Trigger - configuratie

De volgende tabel beschrijft de binding configuratie-eigenschappen die u instelt in de *function.json* bestand en de `WarmupTrigger` kenmerk.

|de eigenschap Function.JSON | De kenmerkeigenschap |Beschrijving|
|---------|---------|----------------------|
| **type** | n.v.t.| Vereist: moet worden ingesteld op `warmupTrigger`. |
| **direction** | n.v.t.| Vereist: moet worden ingesteld op `in`. |
| **De naam** | n.v.t.| Vereist: de naam van de variabele die wordt gebruikt in de functie code.|

## <a name="trigger---usage"></a>Trigger - gebruik

Er wordt geen aanvullende informatie verstrekt aan een door opwarm geactiveerde functie wanneer deze wordt aangeroepen.

## <a name="trigger---limits"></a>Trigger-limieten

* De trigger opwarm is alleen beschikbaar voor apps die worden uitgevoerd op het [Premium-abonnement](./functions-premium-plan.md).
* De trigger opwarm wordt alleen aangeroepen tijdens het schalen van bewerkingen, niet tijdens het opnieuw opstarten of andere niet-geschaalde opstart handelingen. U moet ervoor zorgen dat uw logica alle vereiste afhankelijkheden kan laden zonder gebruik te maken van de opwarm-trigger. Luie lading is een goed patroon om dit te doen.
* De trigger opwarm kan niet worden aangeroepen als er al een exemplaar wordt uitgevoerd.
* Er kan slechts één opwarm-trigger functie per functie-app zijn.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Azure functions-triggers en bindingen](functions-triggers-bindings.md)
