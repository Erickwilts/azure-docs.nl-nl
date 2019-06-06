---
title: Azure Automation-taakgegevens doorsturen naar Azure Monitor-logboeken
description: In dit artikel laat zien hoe u met het verzenden van taakstatus en runbook-taakstromen naar Logboeken van Azure Monitor om meer inzicht en beheer te bieden.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/05/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e0f2d3491db24ecbb49c189232dbc7f698e09fb1
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430781"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-azure-monitor-logs"></a>Taakstatus en taakstromen van Automation doorsturen naar de logboeken van Azure Monitor

Automation kunt runbook en taakstromen van een taak verzenden naar uw Log Analytics-werkruimte. Dit proces wordt geen gebruikgemaakt van het koppelen van de werkruimte en is volledig onafhankelijk. Taaklogboeken en taakstromen zijn zichtbaar in de Azure portal of met PowerShell, voor afzonderlijke taken en deze kunt u eenvoudige onderzoek uitvoeren. Met Azure Monitor-Logboeken kunt u nu:

* Inzicht verwerven in uw Automation-taken.
* De trigger is een e-mailadres of de waarschuwing op basis van uw runbook job status (voor bijvoorbeeld mislukte of onderbroken).
* Geavanceerde query's voor verschillende taakstromen schrijven.
* Taken voor verschillende Automation-accounts aan elkaar te relateren.
* Uw taakgeschiedenis in de loop van de tijd visualiseren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor implementatie

Als u wilt beginnen met het verzenden van uw Automation-Logboeken in Logboeken van Azure Monitor, hebt u het volgende nodig:

* De nieuwste versie van [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).
* Een Log Analytics-werkruimte. Zie voor meer informatie, [aan de slag met Azure Monitor logboeken](../log-analytics/log-analytics-get-started.md). 
* De ResourceId voor uw Azure Automation-account.

Zoek de ResourceId voor uw Azure Automation-account:

```powershell-interactive
# Find the ResourceId for the Automation Account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Als u zoekt de ResourceId voor uw Log Analytics-werkruimte, voer de volgende PowerShell:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Als u meer dan één Automation-accounts hebben, of als werkruimten in de uitvoer van de voorgaande opdrachten, vinden de *naam* u wilt configureren en kopieer de waarde voor *ResourceId*.

Als u wilt zoeken de *naam* van uw Automation-account, selecteert u in Azure portal uw Automation-account van de **Automation-account** blade en selecteer **alle instellingen** . Selecteer op de blade **Alle instellingen** onder **Accountinstellingen** de optie **Eigenschappen**.  Noteer de waarden die op de blade **Eigenschappen** worden weergegeven.<br> ![Eigenschappen van Automation-Account](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-azure-monitor-logs"></a>Integratie met Azure Monitor logboeken instellen

1. Start op uw computer **Windows PowerShell** uit de **Start** scherm.
2. Voer de volgende PowerShell uit en bewerkt u de waarde voor de `[your resource id]` en `[resource id of the log analytics workspace]` met de waarden uit de vorige stap.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Na dit script is uitgevoerd, duurt een uur voordat u begint met de records worden weergegeven in Azure Monitor-logboeken van de nieuwe JobLogs of JobStreams wordt geschreven.

Als u wilt zien van de logboeken, moet u de volgende query uitvoeren in log analytics zoeken in Logboeken: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Configuratie controleren

Om te bevestigen dat uw Automation-account logboeken naar uw Log Analytics-werkruimte verzendt, controleert u of diagnostische gegevens correct zijn geconfigureerd op het Automation-account met behulp van de volgende PowerShell:

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Zorg ervoor dat in de uitvoer:

* Onder *logboeken*, de waarde voor *ingeschakeld* is *waar*.
* De waarde van *WorkspaceId* is ingesteld op de ResourceId van uw Log Analytics-werkruimte.

## <a name="azure-monitor-log-records"></a>Azure Monitor log-records

Diagnostische gegevens van Azure Automation worden twee typen records gemaakt in Azure Monitor-logboeken en zijn gelabeld als **AzureDiagnostics**. De volgende query's uit de bijgewerkte querytaal naar Logboeken van Azure Monitor gebruiken. Bezoek voor meer informatie over algemene query's tussen de verouderde query-taal en de nieuwe querytaal van Azure Kusto [verouderde naar de nieuwe querytaal van Azure Kusto-referentiemateriaal](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>Taaklogboeken

| Eigenschap | Description |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Nieuwe<br>-Gemaakt<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>-Voltooid |
| Category | Classificatie van het type gegevens. Voor Automation is de waarde JobLogs. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Hoe Azure Monitor-logboeken verzameld voor de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Hiermee wordt resultaatstatus van de runbooktaak beschreven. Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account resource-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Taakstromen
| Eigenschap | Description |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| StreamType_s |Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| JobId_g |De GUID die de id van de runbooktaak is. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Wordt uitgevoerd |
| Category | Classificatie van het type gegevens. Voor Automation is de waarde JobStreams. |
| OperationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak. |
| Resource | Naam van het Automation-account |
| SourceSystem | Hoe Azure Monitor-logboeken verzameld voor de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResultDescription |Bevat de uitvoerstroom van het runbook. |
| CorrelationId |De GUID die de correlatie-id van de runbooktaak is. |
| ResourceId |Hiermee geeft u de Azure Automation-account resource-id van het runbook. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-azure-monitor-logs"></a>Automation-Logboeken in Azure Monitor logboeken weergeven

Nu dat u uw Automation-taaklogboeken verzenden naar Azure Monitor-logboeken eerst, laten we zien wat u kunt doen met deze logboeken in Logboeken van Azure Monitor.

Als u wilt zien van de logboeken, voer de volgende query uit: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Een e-mailbericht verzenden wanneer een runbook-taak is mislukt of wordt onderbroken
Een van de grote klanten wordt gevraagd de mogelijkheid voor het verzenden van een e-mailadres of een SMS-bericht wanneer er iets met een runbook-taak misgaat.   

Voor het maken van een waarschuwingsregel, begint u met het maken van een zoeken in Logboeken voor de runbook-Taakrecords die de waarschuwing moet worden aangeroepen. Klik op de **waarschuwing** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics-werkruimte **logboeken bekijken**.
2. Maak een zoekquery logboek voor de waarschuwing door te zoeken in de volgende typen in het queryveld: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")`  U kunt ook de RunbookName groeperen met behulp van: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Als u logboeken van meer dan één Automation-account of abonnement aan uw werkruimte, kunt u uw waarschuwingen op basis van abonnement en de Automation-account kunt groeperen. Naam van het Automation-account kan worden gevonden in het veld Resource in het zoekvak van JobLogs.
3. Om te openen de **maken regel** scherm, klikt u op **+ nieuwe waarschuwingsregel** aan de bovenkant van de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [waarschuwingen voor activiteitenlogboeken in Azure](../azure-monitor/platform/alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Alle taken hebt voltooid met fouten zoeken
U kunt naast de waarschuwingen op fouten, wanneer een runbook-taak een niet-afsluitfout heeft vinden. In dergelijke gevallen PowerShell een foutstroom produceert, maar de niet-afsluitfouten niet tot gevolg dat de taak onderbreken of als mislukt.    

1. Klik in uw Log Analytics-werkruimte op **logboeken**.
2. Typ in het queryveld `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` en klik vervolgens op de **zoeken** knop.

### <a name="view-job-streams-for-a-job"></a>Taakstromen weergeven voor een taak
Wanneer u een taak foutopsporing, kunt u ook de taakstromen kan bekijken. De volgende query geeft alle stromen voor een enkele taak met GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Status van de historische taak weergeven
Ten slotte kunt u voor het visualiseren van uw Taakgeschiedenis na verloop van tijd. U kunt deze query gebruiken om te zoeken naar de status van uw taken na verloop van tijd.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Log Analytics historische taak Statusgrafiek](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Diagnostische instellingen verwijderen

Als u wilt verwijderen van de diagnostische instelling van het Automation-Account, voer de volgende opdrachten:

```powershell-interactive
$automationAccountId = "[resource id of your automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="summary"></a>Samenvatting

Uw Automation-taak de status en stream-gegevens verzenden naar Azure Monitor-Logboeken, krijgt u inzicht in de status van uw Automation-taken op:
+ Instellen van waarschuwingen om u te waarschuwen wanneer er een probleem.
+ Met behulp van aangepaste weergaven en zoekopdrachten naar uw runbookresultaten te visualiseren, gerelateerde de status van de runbook-taak en andere KPI's of metrische gegevens.  

Logboeken in Azure Monitor biedt tevens grotere operationele zichtbaarheid aan uw Automation-taken en kunt u incidenten sneller.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van verschillende zoekquery's en bekijk de Automation-taaklogboeken met Azure Monitor-Logboeken, [zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md).
* Zie voor meer informatie over het maken en ophalen van de uitvoer en foutberichten van runbooks, [Runbook-uitvoer en berichten](automation-runbook-output-and-messages.md).
* Zie [Runbooktaken bijhouden](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details.
* Zie voor meer informatie over Azure Monitor-logboeken en -verzameling gegevensbronnen, [verzamelen van Azure storage-gegevens in Azure Monitor-Logboeken overzicht](../azure-monitor/platform/collect-azure-metrics-logs.md).

