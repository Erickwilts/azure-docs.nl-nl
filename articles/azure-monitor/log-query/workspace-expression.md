---
title: Workspace() expressie in de query voor Azure Monitor | Microsoft Docs
description: De werkruimte-expressie wordt gebruikt in een query voor Azure Monitor voor het ophalen van gegevens uit een specifieke werkruimte in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.openlocfilehash: b4bc652d54150b72cc64898464b3511a860bf011
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60785673"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>de expressie Workspace() in Azure Monitor log-query

De `workspace` expressie in een Azure Monitor-query wordt gebruikt voor het ophalen van gegevens uit een specifieke werkruimte in dezelfde resourcegroep, een andere resourcegroep of een ander abonnement. Dit is handig om op te nemen van logboekgegevens in een Application Insights-query en om gegevens te doorzoeken in meerdere werkruimten in een logboekquery.


## <a name="syntax"></a>Syntaxis

`workspace(`*Identifier*`)`

## <a name="arguments"></a>Argumenten

- *Id*: Hiermee geeft u de werkruimte met behulp van een van de indelingen in de onderstaande tabel.

| ID | Description | Voorbeeld
|:---|:---|:---|
| Resourcenaam | Menselijke leesbare naam van de werkruimte (AKA "naam van onderdeel") | Workspace("contosoretail") |
| Gekwalificeerde naam | Volledige naam van de werkruimte in de vorm: "resourceGroup-subscriptionName/componentName" | workspace('Contoso/ContosoResource/ContosoWorkspace') |
| Id | GUID van de werkruimte | workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure-Resource-ID | ID voor de Azure-resource | workspace("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Opmerkingen

* U moet leestoegang hebben tot de werkruimte.
* Een bijbehorende expressie is `app` waarmee u query's uitvoeren voor Application Insights-toepassingen.

## <a name="examples"></a>Voorbeelden

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Volgende stappen

- Zie de [app expressie](app-expression.md) om te verwijzen naar een Application Insights-app.
- Meer informatie over hoe u [Azure Monitor gegevens](log-query-overview.md) is opgeslagen.
- Toegang tot volledige documentatie voor de [Kusto-querytaal](/azure/kusto/query/).
