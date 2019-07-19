---
title: Azure Functions trigger voor Cosmos DB verbindings beleid
description: Meer informatie over het configureren van het verbindings beleid dat wordt gebruikt door Azure Functions trigger voor Cosmos DB
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 359b6a905e64046aad62b70ae53b993c86884ad2
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335625"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-functions-trigger-for-cosmos-db"></a>Het verbindings beleid configureren dat wordt gebruikt door Azure Functions trigger voor Cosmos DB

In dit artikel wordt beschreven hoe u het verbindings beleid kunt configureren wanneer u de Azure Functions trigger gebruikt voor Cosmos DB om verbinding te maken met uw Azure Cosmos-account.

## <a name="why-is-the-connection-policy-important"></a>Waarom is het verbindings beleid belang rijk?

Er zijn twee verbindings modi: directe modus en gateway modus. Zie het artikel over [prestatie tips](./performance-tips.md#networking) voor meer informatie over deze verbindings modi. Standaard wordt **Gateway** gebruikt om alle verbindingen te maken op de Azure functions trigger voor Cosmos db. Het is echter mogelijk niet de beste optie voor prestatie gerichte scenario's.

## <a name="changing-the-connection-mode-and-protocol"></a>De verbindings modus en het protocol wijzigen

Er zijn twee belang rijke configuratie-instellingen beschikbaar voor het configureren van het beleid voor client verbindingen: de **verbindings modus** en het **verbindings protocol**. U kunt de standaard verbindings modus en het protocol dat wordt gebruikt door de Azure Functions trigger voor Cosmos DB en alle [Azure Cosmos DB bindingen](../azure-functions/functions-bindings-cosmosdb-v2.md#output)) wijzigen. Als u de standaard instellingen wilt wijzigen, moet u het `host.json` bestand in uw Azure functions project of Azure functions app zoeken en de volgende [extra instelling](../azure-functions/functions-bindings-cosmosdb-v2.md#hostjson-settings)toevoegen:

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Waar `connectionMode` moet de gewenste verbindings modus (direct of gateway) en `protocol` het gewenste verbindings Protocol (TCP of https) hebben. 

Als uw Azure functions project werkt met Azure functions v1-runtime, heeft de configuratie een geringe naam verschil. u moet in `documentDB` plaats van `cosmosDB`het volgende gebruiken:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Wanneer u werkt met Azure Functions hosting abonnement voor het verbruiks abonnement, heeft elk exemplaar een limiet voor de hoeveelheid socket verbindingen die het kan onderhouden. Wanneer u werkt met de modus direct/TCP, worden er meer verbindingen gemaakt en kunnen de [limieten voor verbruiks abonnementen](../azure-functions/manage-connections.md#connection-limit)worden bereikt. in dat geval kunt u de gateway modus gebruiken of uw Azure Functions in [app service modus](../azure-functions/functions-scale.md#app-service-plan)uitvoeren.

## <a name="next-steps"></a>Volgende stappen

* [Verbindings limieten in Azure Functions](../azure-functions/manage-connections.md#connection-limit)
* [Tips voor Azure Cosmos DB prestaties](./performance-tips.md)
* [Codevoorbeelden](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
