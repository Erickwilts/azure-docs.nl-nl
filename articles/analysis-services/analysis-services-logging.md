---
title: Diagnostische logboekregistratie voor Azure Analysis Services | Microsoft Docs
description: Meer informatie over het instellen van de registratie in diagnoselogboek voor Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2303d385d3d688050a8d82c07e78a68588f41e88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66142602"
---
# <a name="setup-diagnostic-logging"></a>Registratie in diagnoselogboek instellen

Een belangrijk onderdeel van een Analysis Services-oplossing wordt bewaakt door het uitvoeren van uw servers. Met [diagnostische logboeken van Azure-resource](../azure-monitor/platform/diagnostic-logs-overview.md), u kunt controleren en logboeken te verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), ze te streamen [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en ze te exporteren [Azure Logboeken bewaken](../azure-monitor/azure-monitor-log-hub.md).

![Diagnostische logboekregistratie naar opslag, Event Hubs of Azure Monitor-Logboeken](./media/analysis-services-logging/aas-logging-overview.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="whats-logged"></a>Wat aangemeld?

U kunt selecteren **Engine**, **Service**, en **metrische gegevens** categorieën.

### <a name="engine"></a>Engine

Selecteren **Engine** registreert alle [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). U kunt geen afzonderlijke gebeurtenissen selecteren. 

|XEvent-categorieën |Gebeurtenisnaam  |
|---------|---------|
|Beveiligingscontrole    |   Audit-aanmelding      |
|Beveiligingscontrole    |   Afmelden controleren      |
|Beveiligingscontrole    |   Audit-Server wordt gestart en gestopt      |
|Van voortgangsrapporten     |   Voortgang rapport Begin      |
|Van voortgangsrapporten     |   Einde van het voortgangsrapport      |
|Van voortgangsrapporten     |   Voortgang rapport huidige      |
|Query's     |  Begin van de query       |
|Query's     |   Einde van de query      |
|Opdrachten     |  Begin van de opdracht       |
|Opdrachten     |  Einde van de opdracht       |
|Fouten en waarschuwingen     |   Fout      |
|Ontdekken     |   Einde detecteren      |
|Melding     |    Melding     |
|Sessie     |  Sessie initialiseren       |
|Vergrendelingen    |  Impasse       |
|Verwerking van query 's     |   VertiPaq SE-Query Begin      |
|Verwerking van query 's     |   Einde van VertiPaq SE-Query      |
|Verwerking van query 's     |   VertiPaq SE-Query Cache overeenkomst      |
|Verwerking van query 's     |   Directe Query Begin      |
|Verwerking van query 's     |  Directe Query End       |

### <a name="service"></a>Service

|Naam van bewerking  |Treedt op wanneer  |
|---------|---------|
|ResumeServer     |    Hervatten van een server     |
|SuspendServer    |   Onderbreken van een server      |
|DeleteServer     |    Een server verwijderen     |
|RestartServer    |     Gebruiker opnieuw wordt opgestart een server via SSMS of PowerShell    |
|GetServerLogFiles    |    Gebruiker exporteert serverlogboek via PowerShell     |
|ExportModel     |   Gebruiker Hiermee exporteert u een model in de portal met behulp van Open in Visual Studio     |

### <a name="all-metrics"></a>Alle metrische gegevens

De metrische categorie registreert hetzelfde [metrische servergegevens](analysis-services-monitor.md#server-metrics) weergegeven in de metrische gegevens.

## <a name="setup-diagnostics-logging"></a>Diagnostische logboekregistratie instellen

### <a name="azure-portal"></a>Azure Portal

1. In [Azure-portal](https://portal.azure.com) >-server, klikt u op **diagnostische logboeken** in de navigatiebalk aan de linkerkant en klik vervolgens op **diagnostische gegevens inschakelen**.

    ![Schakel registratie in diagnoselogboek voor Azure Cosmos DB in Azure portal](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. In **diagnostische instellingen**, geeft u de volgende opties: 

    * **Naam**. Voer een naam voor de logboeken om te maken.

    * **Archiveren naar een opslagaccount**. Als u wilt deze optie gebruikt, moet u een bestaand opslagaccount verbinden. Zie [een opslagaccount maken](../storage/common/storage-create-storage-account.md). Volg de instructies voor het maken van een Resource Manager, account voor algemeen gebruik, en selecteer vervolgens uw storage-account door te retourneren aan deze pagina in de portal. Het duurt een paar minuten voor de zojuist gemaakte storage-accounts worden weergegeven in de vervolgkeuzelijst.
    * **Stream naar een event hub**. Als u wilt deze optie gebruikt, moet u een bestaande Event Hub-naamruimte en event hub te verbinden. Zie voor meer informatie, [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](../event-hubs/event-hubs-create.md). Ga vervolgens terug naar deze pagina in de portal voor het selecteren van de naam van de Event Hub-naamruimte en het beleid.
    * **Verzenden naar Azure Monitor (Log Analytics-werkruimte)** . Als u wilt deze optie gebruikt, gebruikt u een bestaande werkruimte of [Maak een nieuwe werkruimte](../azure-monitor/learn/quick-create-workspace.md) resource in de portal. Zie voor meer informatie over het weergeven van uw logboeken [weergave-logboeken in Log Analytics-werkruimte](#view-logs-in-log-analytics-workspace) in dit artikel.

    * **Engine**. Selecteer deze optie om aan te melden xEvents. Als u naar een opslagaccount archiveren bent, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaarperiode is verlopen.
    * **Service**. Selecteer deze optie om aan te melden op gebeurtenissen van de service. Als u naar een opslagaccount archiveren wordt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaarperiode is verlopen.
    * **Metrische gegevens**. Selecteer deze optie voor het opslaan van uitgebreide gegevens in [metrische gegevens](analysis-services-monitor.md#server-metrics). Als u naar een opslagaccount archiveren wordt, kunt u de bewaarperiode voor de diagnostische logboeken. Logboeken zijn autodeleted nadat de bewaarperiode is verlopen.

3. Klik op **Opslaan**.

    Als u een foutbericht ontvangt met de tekst ' kan niet bijwerken van diagnostische gegevens voor \<Werkruimtenaam >. Het abonnement \<abonnements-id > is niet geregistreerd voor het gebruik van microsoft.insights. " Ga als volgt de [oplossen Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instructies voor het registreren van het account, voer deze procedure.

    Als u wijzigen hoe uw logboeken met diagnostische gegevens in de toekomst op elk gewenst moment worden opgeslagen wilt, kunt u terugkeren naar deze pagina om instellingen te wijzigen.

### <a name="powershell"></a>PowerShell

Hier volgen de basisopdrachten voor u aan de slag. Als u wilt dat Stapsgewijze instructies over het instellen van logboekregistratie naar een opslagaccount met behulp van PowerShell, raadpleegt u de zelfstudie verderop in dit artikel.

Om in te schakelen metrische en diagnostische gegevens logboekregistratie met behulp van PowerShell, gebruikt u de volgende opdrachten:

- Om in te schakelen opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Het opslagaccount-ID is de resource-ID voor het opslagaccount waar u om de logboeken te verzenden.

- Als u wilt inschakelen voor streaming van diagnostische logboeken naar een event hub, gebruikt u deze opdracht:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   De regel-ID van Azure Service Bus is een tekenreeks zijn met deze indeling:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Als u wilt inschakelen verzenden van diagnostische logboeken naar Log Analytics-werkruimte, moet u deze opdracht gebruiken:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- U vindt de resource-ID van uw Log Analytics-werkruimte met behulp van de volgende opdracht uit:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

### <a name="rest-api"></a>REST-API

Meer informatie over het [diagnostische instellingen wijzigen met behulp van de Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/). 

### <a name="resource-manager-template"></a>Resource Manager-sjabloon

Meer informatie over het [diagnostische instellingen bij het maken van resource inschakelen met behulp van Resource Manager-sjabloon](../azure-monitor/platform/diagnostic-logs-stream-template.md). 

## <a name="manage-your-logs"></a>Uw logboeken beheren

Logboeken zijn doorgaans beschikbaar binnen een paar uur na het instellen van logboekregistratie. Het is aan u om uw logboeken in uw opslagaccount te beheren:

* Gebruik standaardmethoden van Azure voor toegangsbeheer om uw logboeken te beveiligen door het aantal gebruikers te beperken dat toegang heeft tot de logboeken.
* Verwijder de logboeken die u niet meer in uw opslagaccount wilt bewaren.
* Zorg ervoor dat een bewaarperiode voor instellen, zodat de oude logboeken worden verwijderd uit uw storage-account.

## <a name="view-logs-in-log-analytics-workspace"></a>Logboeken in Log Analytics-werkruimte weergeven

Metrische gegevens en servergebeurtenissen zijn geïntegreerd met xEvents in uw Log Analytics-werkruimteresource voor side-by-side-analyse. Log Analytics-werkruimte kan ook worden geconfigureerd voor het ontvangen van gebeurtenissen van andere Azure-services bieden een holistische weergave van diagnostische gegevens voor logboekregistratie voor uw architectuur.

Voor de diagnostische gegevens in Log Analytics-werkruimte opent **logboeken** in het menu links.

![Opties voor het doorzoeken van logboekbestanden in de Azure-portal](./media/analysis-services-logging/aas-logging-open-log-search.png)

Vouw in de opbouwfunctie voor query's, **LogManagement** > **AzureDiagnostics**. AzureDiagnostics bevat-Engine en servicegebeurtenissen. U ziet dat een query is gemaakt op het begeven. De EventClass\_s veld bevat xEvent-namen, die bekend als u xEvents hebt gebruikt voor on-premises logboekregistratie kunnen zoeken. Klik op **EventClass\_s** of een van de gebeurtenisnamen en Log Analytics-werkruimte blijft met het samenstellen van een query. Zorg ervoor dat u uw query's voor hergebruik.

### <a name="example-query"></a>Voorbeeld van een query
Deze query berekent en retourneert CPU voor elke query end/vernieuwen end-gebeurtenis voor een model-database en de server:

```Kusto
let window =  AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and ServerName_s =~"MyServerName" and DatabaseName_s == "Adventure Works Localhost" ;
window
| where OperationName has "QueryEnd" or (OperationName has "CommandEnd" and EventSubclass_s == 38)
| where extract(@"([^,]*)", 1,Duration_s, typeof(long)) > 0
| extend DurationMs=extract(@"([^,]*)", 1,Duration_s, typeof(long))
| extend Engine_CPUTime=extract(@"([^,]*)", 1,CPUTime_s, typeof(long))
| project  StartTime_t,EndTime_t,ServerName_s,OperationName,RootActivityId_g ,TextData_s,DatabaseName_s,ApplicationName_s,Duration_s,EffectiveUsername_s,User_s,EventSubclass_s,DurationMs,Engine_CPUTime
| join kind=leftouter (
window
    | where OperationName == "ProgressReportEnd" or (OperationName == "VertiPaqSEQueryEnd" and EventSubclass_s  != 10) or OperationName == "DiscoverEnd" or (OperationName has "CommandEnd" and EventSubclass_s != 38)
    | summarize sum_Engine_CPUTime = sum(extract(@"([^,]*)", 1,CPUTime_s, typeof(long))) by RootActivityId_g
    ) on RootActivityId_g
| extend totalCPU = sum_Engine_CPUTime + Engine_CPUTime

```


Er zijn honderden query's die u kunt gebruiken. Zie voor meer informatie over query's, [aan de slag met Azure Monitor logboeken-query's](../azure-monitor/log-query/get-started-queries.md).


## <a name="turn-on-logging-by-using-powershell"></a>Logboekregistratie inschakelen met behulp van PowerShell

In deze korte zelfstudie maakt u een opslagaccount in hetzelfde abonnement en dezelfde resourcegroep als uw Analysis Services-server. Vervolgens gebruikt u Set-AzDiagnosticSetting om in te schakelen Diagnostische logboekregistratie, het verzenden van uitvoer naar het nieuwe opslagaccount.

### <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u de volgende bronnen:

* Een bestaande Azure Analysis Services-server. Zie voor instructies over het maken van een serverresource [maken van een server in Azure portal](analysis-services-create-server.md), of [maken van een Azure Analysis Services-server met behulp van PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Verbinding maken met uw abonnementen

Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:  

```powershell
Connect-AzAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. Azure PowerShell haalt alle abonnementen op die zijn gekoppeld aan dit account en gebruikt standaard het eerste abonnement.

Als u meerdere abonnementen hebt, moet u wellicht specifiek opgeven welk abonnement is gebruikt voor het maken van uw Azure Sleutelkluis. Typ het volgende als u de abonnementen voor uw account wilt zien:

```powershell
Get-AzSubscription
```

Klik, als u het abonnement dat is gekoppeld aan het Azure Analysis Services-account dat u zich aanmeldt, typt u:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, is het belangrijk om op te geven van het abonnement.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Een nieuw opslagaccount voor uw logboeken maken

U kunt een bestaand opslagaccount gebruiken voor uw Logboeken, indien deze zich in hetzelfde abonnement als uw server. Voor deze zelfstudie maakt u een nieuw opslagaccount maken speciaal voor Analysis Services-Logboeken. Om het eenvoudig maken, u details van het opslagaccount opslaat in een variabele met de naam **sa**.

U kunt ook dezelfde resourcegroep gebruiken als het account dat met uw Analysis Services-server. Vervang de waarden voor `awsales_resgroup`, `awsaleslogs`, en `West Central US` door uw eigen waarden:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>De server-serviceaccount voor uw logboeken identificeren

Stel de naam van het aan een variabele met de naam **account**, waar ResourceName is de naam van het account.

```powershell
$account = Get-AzResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Logboekregistratie inschakelen

Als logboekregistratie wilt inschakelen, gebruikt u de cmdlet Set-AzDiagnosticSetting samen met de variabelen voor de nieuwe storage-account, server-account en de categorie. Voer de volgende opdracht, instellen van de **-ingeschakeld** markering **$true**:

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

De uitvoer zou er ongeveer uit moeten zien zoals in dit voorbeeld:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Deze uitvoer bevestigt dat logboekregistratie nu is ingeschakeld voor de server, gegevens opslaat in de storage-account.

U kunt ook bewaarbeleid instellen voor uw Logboeken, zodat oudere logboeken automatisch worden verwijderd. Bijvoorbeeld, stel retentiebeleid in met behulp **- RetentionEnabled** markering **$true**, en stel **- RetentionInDays** parameter **90**. Logboeken die ouder zijn dan 90 dagen worden automatisch verwijderd.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Diagnostische logboekregistratie van Azure-resource](../azure-monitor/platform/diagnostic-logs-overview.md).

Zie [Set AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) in de PowerShell-help.
