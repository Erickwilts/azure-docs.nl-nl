---
title: Eeuwige-integratie in Durable Functions-Azure
description: Meer informatie over het implementeren van eeuwige-integratie met behulp van de Durable Functions-extensie voor Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 352fd16d98e6f376e230d2112a9b94b66ccc1b5a
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69542728"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Eeuwige-integraties in Durable Functions (Azure Functions)

*Eeuwige* -indelingen zijn Orchestrator-functies die nooit eindigen. Ze zijn handig wanneer u [Durable functions](durable-functions-overview.md) wilt gebruiken voor aggregators en elk scenario waarvoor een oneindige lus is vereist.

## <a name="orchestration-history"></a>Orchestration-geschiedenis

Zoals beschreven in [controle punten en herhalingen](durable-functions-checkpointing-and-replay.md), houdt het duurzame taak raamwerk de geschiedenis bij van elke functie indeling. Deze geschiedenis neemt voortdurend toe zolang de Orchestrator-functie blijft werken om nieuwe werkzaamheden te plannen. Als de Orchestrator-functie een oneindige lus doorloopt en voortdurend schema's werken, kan deze geschiedenis kritiek toenemen en aanzienlijke prestatie problemen veroorzaken. Het *eeuwige-Orchestration* -concept is ontworpen om dit soort problemen te verhelpen voor toepassingen die oneindige lussen nodig hebben.

## <a name="resetting-and-restarting"></a>Opnieuw instellen en opnieuw starten

In plaats van een oneindige lus te gebruiken, wordt de status van Orchestrator-functies opnieuw ingesteld door de [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) -methode aan te roepen. Deze methode heeft één JSON-Serializable-para meter, die de nieuwe invoer wordt voor de volgende Orchestrator-functie generatie.

Wanneer `ContinueAsNew` wordt aangeroepen, in het exemplaar een bericht naar zichzelf voordat het wordt afgesloten. Het bericht start het exemplaar opnieuw met de nieuwe invoer waarde. Dezelfde exemplaar-ID wordt bewaard, maar de geschiedenis van de Orchestrator-functie wordt in feite afgekapt.

> [!NOTE]
> Het duurzame taak raamwerk houdt dezelfde exemplaar-ID bij, maar maakt intern een nieuwe *uitvoerings-id* voor de Orchestrator-functie die `ContinueAsNew`opnieuw wordt ingesteld door. Deze uitvoerings-ID wordt over het algemeen niet extern weer gegeven, maar het kan nuttig zijn om te weten wanneer de uitvoering van een fout in de Orchestration is opgetreden.

## <a name="periodic-work-example"></a>Voor beeld van periodieke werk

Een use-case voor eeuwige-indelingen is code die voor onbepaalde tijd periodiek moet worden uitgevoerd.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup", null);

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>Java script (alleen functies 2. x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

Het verschil tussen dit voor beeld en een door een timer geactiveerde functie is dat hier niet op basis van een schema wordt geactiveerd. Zo kan een CRON-schema waarmee een functie elk uur wordt uitgevoerd, worden uitgevoerd op 1:00, 2:00, 3:00 enz. Dit kan mogelijk worden uitgevoerd in overlappende problemen. Als het opschonen echter 30 minuten duurt, wordt het gepland op 1:00, 2:30, 4:00 enzovoort. er is geen kans op overlap ping.

## <a name="exit-from-an-eternal-orchestration"></a>Afsluiten vanuit een eeuwige-indeling

Als een Orchestrator-functie uiteindelijk moet worden voltooid, hoeft u *niet* te bellen `ContinueAsNew` en kunt u de functie niet afsluiten.

Als een Orchestrator-functie zich in een oneindige lus bevindt en moet worden gestopt, gebruikt u de methode [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) om deze te stoppen. Zie [instance Management](durable-functions-instance-management.md)(Engelstalig) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het implementeren van Singleton-indelingen](durable-functions-singletons.md)
