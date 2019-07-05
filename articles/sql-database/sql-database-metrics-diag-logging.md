---
title: Metrische gegevens van Azure SQL-Database en diagnostische logboekregistratie | Microsoft Docs
description: Informatie over het inschakelen van diagnostische gegevens in Azure SQL Database voor het opslaan van informatie over het gebruik van resources en Uitvoeringsstatistieken query.
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 05/21/2019
ms.openlocfilehash: 6824a7151a0c007d6fe4ba021f274886a3cf0dcb
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447825"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Metrische gegevens van Azure SQL-Database en logboekregistratie van diagnostische gegevens

In dit onderwerp leert u hoe u logboekregistratie van diagnostische gegevens telemetrie voor Azure SQL Database via Azure portal, PowerShell, Azure CLI, Azure Monitor REST API en Azure Resource Manager-sjabloon configureren. Deze diagnostische gegevens kan worden gebruikt om te meten van gebruik van resources en Uitvoeringsstatistieken query.

Individuele databases, gepoolde databases in elastische pools en databases van de instantie in een beheerd exemplaar kunnen stream metrische gegevens en diagnostische logboeken voor prestatiebewaking van eenvoudiger. U kunt een database om te verzenden, Resourcegebruik, werkrollen en sessies en connectiviteit met een van de volgende Azure-resources configureren:

- **Azure SQL Analytics**: aan de intelligente bewaking van uw Azure SQL-databases met rapporten, waarschuwingen en risicobeperking aanbevelingen.
- **Azure Event Hubs**: SQL Database-telemetrie integreren met uw aangepaste bewakingsoplossingen of actieve pijplijnen.
- **Azure Storage**: voor het archiveren van grote hoeveelheden telemetriegegevens voor een fractie van de prijs.

    ![Architectuur](./media/sql-database-metrics-diag-logging/architecture.png)

Zie voor meer informatie over de metrische gegevens en logboekbestanden categorieën die door de verschillende Azure-services worden ondersteund:

- [Overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Overzicht van diagnostische logboeken van Azure](../azure-monitor/platform/diagnostic-logs-overview.md)

Dit artikel bevat richtlijnen om u te helpen u bij het inschakelen van diagnostische gegevens telemetrie voor Azure SQL-databases, elastische pools en beheerde exemplaren. Ook kunt u informatie over het configureren van Azure SQL Analytics als een hulpprogramma voor bewaking voor het weergeven van database-telemetrie voor diagnostische gegevens.

## <a name="enable-logging-of-diagnostics-telemetry"></a>Logboekregistratie van diagnostische gegevens telemetrie inschakelen

U kunt inschakelen en metrische gegevens en logboekregistratie van diagnostische gegevens telemetrie beheren met behulp van een van de volgende methoden:

- Azure Portal
- PowerShell
- Azure-CLI
- Azure Monitor REST API
- Azure Resource Manager-sjabloon

Wanneer u metrische gegevens en logboekregistratie van diagnostische gegevens inschakelt, moet u de Azure-resource-doel voor het verzamelen van de diagnostische gegevens telemetrie opgeven. Beschikbare opties zijn onder andere:

- Azure SQL-analyse
- Azure Event Hubs
- Azure Storage

U kunt inrichten van een nieuwe Azure-resource of Selecteer een bestaande resource. Nadat u een resource met behulp van de **diagnostische instellingen** optie, Geef op welke gegevens te verzamelen.

## <a name="supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases"></a>Registratie in diagnoselogboek ondersteund voor Azure SQL-databases en databases van exemplaar

De metrische gegevens en diagnostische gegevens van logboekregistratie voor SQL-databases inschakelen: deze zijn niet standaard ingeschakeld.

U kunt Azure SQL-databases en databases exemplaar instellen om de volgende diagnostische gegevens telemetrie te verzamelen:

| Telemetrie voor databases bewaken | Individuele databases en gepoolde database-ondersteuning | Ondersteuning voor Instance-database |
| :------------------- | ----- | ----- |
| [Basismetriek](#basic-metrics): Bevat DTU/CPU-percentage, DTU/CPU-limiet, fysieke logboek schrijven gegevens gelezen percentage, percentage geslaagd/mislukt/geblokkeerd door firewallverbindingen, sessies percentage, percentage van de werknemers, opslag, opslagpercentage en XTP-opslagpercentage. | Ja | Nee |
| [QueryStoreRuntimeStatistics](#query-store-runtime-statistics): Bevat informatie over de query duur statistieken en de query-runtime-statistieken, zoals CPU-gebruik. | Ja | Ja |
| [QueryStoreWaitStatistics](#query-store-wait-statistics): Bevat informatie over de query-wait-statistieken (wat uw query's gewacht op) zoals CPU, het logboek en VERGRENDELEN zijn. | Ja | Ja |
| [Fouten](#errors-dataset): Bevat informatie over SQL-fouten op een database. | Ja | Ja |
| [DatabaseWaitStatistics](#database-wait-statistics-dataset): Bevat informatie over hoe lang een database besteed aan het wachten op andere wacht typen. | Ja | Nee |
| [Time-outs](#time-outs-dataset): Bevat informatie over time-outs voor een database. | Ja | Nee |
| [Blokken](#blockings-dataset): Bevat informatie over het blokkeren van gebeurtenissen voor een database. | Ja | Nee |
| [Deadlocks](#deadlocks-dataset): Bevat informatie over gebeurtenissen impasse op een database. | Ja | Nee |
| [AutomaticTuning](#automatic-tuning-dataset): Bevat informatie over aanbevelingen voor automatische afstemming voor een database. | Ja | Nee |
| [SQLInsights](#intelligent-insights-dataset): Intelligent Insights in de prestaties voor een database bevat. Zie voor meer informatie, [Intelligent Insights](sql-database-intelligent-insights.md). | Ja | Ja |

> [!IMPORTANT]
> Elastische pools en beheerde exemplaren hebben hun eigen afzonderlijke diagnostische gegevens telemetrie van databases die ze bevatten. Dit is belangrijk te weten als diagnostische gegevens telemetrie wordt afzonderlijk geconfigureerd voor elk van deze resources, zoals hieronder beschreven.

> [!NOTE]
> Logboeken voor beveiligingscontrole en SQLSecurityAuditEvents kunnen niet worden ingeschakeld vanuit de database-instellingen voor diagnostische gegevens (Hoewel weergegeven op het scherm). Inschakelen van controle logboekstreaming [controle voor uw database instellen](sql-database-auditing.md#subheading-2), en [controle van Logboeken in Logboeken van Azure Monitor en Azure Event Hubs](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).

## <a name="azure-portal"></a>Azure Portal

U kunt de **diagnostische instellingen** menu voor elk één, gegroepeerd, of database in Azure portal voor het configureren van diagnostische gegevens telemetrie streaming-instantie. Bovendien diagnostische telemetrie kan ook afzonderlijk worden geconfigureerd voor database-containers: elastische pools en beheerde exemplaren. U kunt de volgende bestemmingen om de telemetrie van Azure diagnostics streamen instellen: Logboeken in Azure Storage, Azure Event Hubs en Azure Monitor.

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Streaming van diagnostische gegevens telemetrie voor elastische pools configureren

   ![Elastische pool-pictogram](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

U kunt een elastische pool-resource instellen om de volgende diagnostische gegevens telemetrie te verzamelen:

| Resource | Bewaking van telemetrie |
| :------------------- | ------------------- |
| **Elastische pool** | [Basismetriek](sql-database-metrics-diag-logging.md#basic-metrics) bevat eDTU/CPU-percentage, eDTU/CPU-limiet, fysieke gegevens gelezen percentage, logboek schrijven percentage, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, limiet voor opslag en XTP-opslagpercentage. |

Als u wilt configureren met het streamen van diagnostische gegevens telemetrie voor elastische pools en databases in elastische pools, moet u afzonderlijk configureren **beide** van de volgende:

- Inschakelen van diagnostische gegevens telemetrie voor een elastische pool, streaming **en**
- Streaming van diagnostische gegevens telemetrie voor elke database in een elastische pool inschakelen

Dit is omdat de elastische pool is een databasecontainer met een eigen telemetrie wordt gescheiden van een afzonderlijke database-telemetrie.

Als u wilt inschakelen voor streaming van diagnostische gegevens telemetrie voor een elastische pool-resource, de volgende stappen uit:

1. Ga naar de **elastische pool** resource in Azure portal.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** bewerken van een vorige instelling.

   ![Diagnostische gegevens voor elastische pools inschakelen](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

1. Voer een instellingnaam in voor uw eigen referentie.
1. Selecteer een doelresource voor de streaminggegevens van diagnostische gegevens: **Archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**.
1. Selecteer voor log analytics, **configureren** en een nieuwe werkruimte maken door te selecteren **+ nieuwe werkruimte maken**, of Selecteer een bestaande werkruimte.
1. Schakel het selectievakje voor telemetrie voor elastische Pools diagnostische gegevens: **Basic** metrische gegevens.
   ![Diagnostische gegevens voor elastische pools configureren](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)
1. Selecteer **Opslaan**.
1. Configureer bovendien streaming van diagnostische gegevens telemetrie voor elke database in de elastische groep die u wilt bewaken door de volgende stappen wordt beschreven in de volgende sectie.

> [!IMPORTANT]
> Naast het configureren van diagnostische gegevens telemetrie voor een elastische pool, moet u ook configureren diagnostische gegevens telemetrie voor elke database in een elastische pool, zoals hieronder beschreven.

### <a name="configure-streaming-of-diagnostics-telemetry-for-single-database-or-database-in-elastic-pool"></a>Streaming van diagnostische gegevens telemetrie voor één database of een database in de elastische groep configureren

   ![Pictogram van een SQL-Database](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

Als u wilt inschakelen voor streaming van diagnostische gegevens telemetrie voor enkel of gegroepeerde databases, de volgende stappen uit:

1. Ga naar Azure **SQL-database** resource.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** bewerken van een vorige instelling.
   - U kunt maximaal drie parallelle verbindingen met diagnostische telemetrie van stroom maken.
   - Selecteer **+ diagnostische instelling toevoegen** parallelle streaming van diagnostische gegevens naar meerdere resources configureren.

   ![Inschakelen van diagnostische gegevens voor één, gepoold of databases-exemplaar](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)
1. Voer een instellingnaam in voor uw eigen referentie.
1. Selecteer een doelresource voor de streaminggegevens van diagnostische gegevens: **Archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**.
1. Voor de standaard, op basis van gebeurtenissen bewakingservaring, selecteert u de volgende selectievakjes uit voor databasetelemetrie diagnostische gegevens over log: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics**, **fouten** , **DatabaseWaitStatistics**, **time-outs**, **blokken**, en **impassen**.
1. Voor een geavanceerde, op basis van een-minuut bewakingservaring, schakel het selectievakje voor **Basic** metrische gegevens.
   ![Configureren van diagnostische gegevens voor één, gepoold of databases-exemplaar](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)
1. Selecteer **Opslaan**.
1. Herhaal deze stappen voor elke database die u wilt bewaken.

> [!NOTE]
> Logboeken voor beveiligingscontrole en SQLSecurityAuditEvents kunnen niet worden ingeschakeld vanuit de database-instellingen voor diagnostische gegevens (hoewel wordt weergegeven op het scherm). Inschakelen van controle logboekstreaming [controle voor uw database instellen](sql-database-auditing.md#subheading-2), en [controle van Logboeken in Logboeken van Azure Monitor en Azure Event Hubs](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/SQL-Audit-logs-in-Azure-Log-Analytics-and-Azure-Event-Hubs/ba-p/386242).
> [!TIP]
> Herhaal deze stappen voor elke Azure SQL-Database die u wilt bewaken.

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instances"></a>Configureer streaming van diagnostische gegevens telemetrie voor beheerde exemplaren

   ![Pictogram voor beheerd exemplaar](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

U kunt de resource van een beheerd exemplaar voor het verzamelen van de volgende diagnostische gegevens telemetrie instellen:

| Resource | Bewaking van telemetrie |
| :------------------- | ------------------- |
| **Beheerd exemplaar** | [ResourceUsageStats](#resource-usage-stats-for-managed-instance) bevat het aantal vCores, gemiddelde CPU-percentage, i/o-aanvragen, bytes gelezen of weggeschreven, gereserveerde opslagruimte en opslagruimte gebruikt. |

Als u wilt configureren met het streamen van diagnostische gegevens telemetrie voor beheerd exemplaar en exemplaar databases, moet u afzonderlijk configureren **beide** van de volgende:

- Inschakelen van diagnostische gegevens telemetrie voor het beheerde exemplaar streaming **en**
- Streaming van diagnostische gegevens telemetrie voor elke Exemplaardatabase inschakelen

Dit is omdat het beheerde exemplaar is een databasecontainer met een eigen telemetrie, los van de database-telemetrie van een afzonderlijk exemplaar.

Als u wilt inschakelen voor streaming van diagnostische gegevens telemetrie voor de resource van een beheerd exemplaar, de volgende stappen uit:

1. Ga naar de **beheerd exemplaar** resource in Azure portal.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** bewerken van een vorige instelling.

   ![Diagnostische gegevens voor het beheerde exemplaar inschakelen](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-enable.png)

1. Voer een instellingnaam in voor uw eigen referentie.
1. Selecteer een doelresource voor de streaminggegevens van diagnostische gegevens: **Archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**.
1. Selecteer voor log analytics, **configureren** en een nieuwe werkruimte maken door te selecteren **+ nieuwe werkruimte maken**, of gebruik een bestaande werkruimte.
1. Schakel het selectievakje bijvoorbeeld diagnostische gegevens telemetrie: **ResourceUsageStats**.
   ![Configureren van diagnostische gegevens voor het beheerde exemplaar](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-mi-selection.png)
1. Selecteer **Opslaan**.
1. Configureer bovendien streaming van diagnostische gegevens telemetrie voor elke Exemplaardatabase binnen het beheerde exemplaar dat u controleren wilt met de volgende stappen wordt beschreven in de volgende sectie.

> [!IMPORTANT]
> Naast het configureren van diagnostische gegevens telemetrie voor een beheerd exemplaar, moet u ook diagnostische gegevens telemetrie voor elke Exemplaardatabase configureren zoals hieronder beschreven.

### <a name="configure-streaming-of-diagnostics-telemetry-for-instance-databases"></a>Streaming van diagnostische gegevens telemetrie bijvoorbeeld databases configureren

   ![Exemplaar in de database in het beheerde exemplaar pictogram](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

Als u wilt inschakelen voor streaming van diagnostische gegevens telemetrie bijvoorbeeld databases, de volgende stappen uit:

1. Ga naar **Exemplaardatabase** resource binnen het beheerde exemplaar.
1. Selecteer **diagnostische instellingen**.
1. Selecteer **diagnostische gegevens inschakelen** als er geen vorige instellingen bestaat, of selecteer **instelling bewerken** bewerken van een vorige instelling.
   - U kunt maximaal drie (3) parallelle verbindingen met stream diagnostische gegevens telemetrie kunt maken.
   - Selecteer **+ diagnostische instelling toevoegen** parallelle streaming van diagnostische gegevens naar meerdere resources configureren.

   ![Diagnostische gegevens over bijvoorbeeld databases inschakelen](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

1. Voer een instellingnaam in voor uw eigen referentie.
1. Selecteer een doelresource voor de streaminggegevens van diagnostische gegevens: **Archiveren naar opslagaccount**, **Stream naar een event hub**, of **verzenden naar Log Analytics**.
1. Schakel de selectievakjes voor databasetelemetrie diagnostische gegevens: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** en **fouten**.
   ![Diagnostische gegevens over bijvoorbeeld databases configureren](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)
1. Selecteer **Opslaan**.
1. Herhaal deze stappen voor elke Exemplaardatabase die u wilt bewaken.

> [!TIP]
> Herhaal deze stappen voor elke Exemplaardatabase die u wilt bewaken.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

U kunt metrische gegevens en logboekregistratie van diagnostische gegevens inschakelen met behulp van PowerShell.

- Om in te schakelen opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   De storage-account-ID is de resource-ID voor het doelopslagaccount.

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

### <a name="to-configure-multiple-azure-resources"></a>Het configureren van meerdere Azure-resources

Ter ondersteuning van meerdere abonnementen, gebruikt u de PowerShell-script uit [inschakelen Azure resource metrische gegevens vastleggen met behulp van PowerShell](https://blogs.technet.microsoft.com/msoms/20../../enable-azure-resource-metrics-logging-using-powershell/).

De werkruimte resource-ID opgeven \<$WSID\> als parameter bij het uitvoeren van het script `Enable-AzureRMDiagnostics.ps1` voor het verzenden van diagnostische gegevens uit meerdere bronnen naar de werkruimte.

- Om op te halen van de werkruimte-ID \<$WSID\> van de bestemming voor de diagnostische gegevens, het volgende script gebruiken:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

   Vervang \<subID\> met de abonnements-ID, \<RG_NAME\> met de naam van de resourcegroep, en \<WS_NAME\> met de naam van de werkruimte.

### <a name="azure-cli"></a>Azure-CLI

U kunt metrische gegevens en logboekregistratie van diagnostische gegevens inschakelen met behulp van de Azure CLI.

> [!NOTE]
> Scripts voor het inschakelen van logboekregistratie van diagnostische gegevens worden ondersteund voor Azure CLI v1.0. Houd er rekening mee dat CLI versie 2.0 niet ondersteund op dit moment wordt.

- Als u wilt inschakelen in de opslag van diagnostische logboeken in een opslagaccount, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   De storage-account-ID is de resource-ID voor het doelopslagaccount.

- Als u wilt inschakelen voor het streamen van diagnostische logboeken naar een event hub, gebruikt u deze opdracht:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   De regel-ID van Service Bus is een tekenreeks zijn met deze indeling:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Als u wilt inschakelen voor het verzenden van diagnostische logboeken naar Log Analytics-werkruimte, moet u deze opdracht gebruiken:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

U kunt deze parameters voor het inschakelen van meerdere uitvoeropties combineren.

### <a name="rest-api"></a>REST-API

Meer informatie over hoe u [diagnostische instellingen wijzigen met behulp van de Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).

### <a name="resource-manager-template"></a>Resource Manager-sjabloon

Meer informatie over hoe u [diagnostische instellingen bij het maken van resource inschakelen met behulp van Resource Manager-sjabloon](../azure-monitor/platform/diagnostic-logs-stream-template.md).

## <a name="stream-into-azure-sql-analytics"></a>Stream in Azure SQL-analyse

Azure SQL Analytics is een cloudoplossing waarmee de prestaties van Azure SQL-databases, elastische pools en beheerde exemplaren op schaal en voor meerdere abonnementen worden gecontroleerd. Kunt u het verzamelen en visualiseren van Azure SQL Database-maatstaven voor prestaties en het beschikt over ingebouwde intelligentie voor het oplossen van prestaties.

![Overzicht van Azure SQL Analytics](../azure-monitor/insights/media/azure-sql/azure-sql-sol-overview.png)

De logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database kunnen worden gestreamd naar Azure SQL Analytics met behulp van de ingebouwde **verzenden naar Log Analytics** optie in het tabblad instellingen van diagnostische gegevens in de portal. U kunt ook log analytics inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="installation-overview"></a>Installatie-overzicht

U kunt een vloot SQL-Database met Azure SQL Analytics controleren. Voer de volgende stappen uit:

1. Maak een Azure SQL Analytics-oplossing op Azure Marketplace.
2. Maak een werkruimte voor bewaking in de oplossing.
3. Configureer databases naar stream diagnostische gegevens telemetrie in de werkruimte.

Als u van elastische pools gebruikmaakt of instanties die worden beheerd, moet u ook het configureren van diagnostische gegevens telemetrie streaming van deze resources.

### <a name="create-azure-sql-analytics-resource"></a>Azure SQL Analytics-resource maken

1. Zoeken naar Azure SQL Analytics in Azure Marketplace en selecteer deze.

   ![Zoeken naar Azure SQL Analytics in portal](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)

2. Selecteer **maken** op de scherm overzicht van de oplossing.

3. Vul het formulier Azure SQL-analyse met de aanvullende informatie die is vereist: naam van de werkruimte, abonnement, resourcegroep, locatie en prijscategorie.

   ![Configureren van Azure SQL Analytics in portal](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. Selecteer **OK** om te bevestigen en selecteer vervolgens **maken**.

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Databases naar Logboeken voor records metrische en diagnostische gegevens configureren

De eenvoudigste manier om te configureren waarin de metrische gegevens voor het registreren van databases is met behulp van de Azure-portal. Zoals eerder beschreven, gaat u naar de resource van uw SQL-Database in Azure portal en selecteer **diagnostische instellingen**.

Als u van elastische pools gebruikmaakt of instanties die worden beheerd, moet u ook de diagnostische instellingen configureren in de volgende bronnen voor het inschakelen van de telemetrie van Azure diagnostics streamen naar de werkruimte.

### <a name="use-the-sql-analytics-solution"></a>De oplossing SQL Analytics gebruiken

U kunt SQL-analyse als een hiërarchische dashboard gebruiken om uw SQL Database-resources weer te geven. Zie voor meer informatie over het gebruik van de oplossing SQL Analytics, [SQL-Database controleren met behulp van de oplossing SQL Analytics](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Streamen in Event Hubs

U kunt de logboeken van de metrische gegevens en diagnostische gegevens van de SQL-Database streamen naar Event Hubs met behulp van de ingebouwde **Stream naar een event hub** optie in Azure portal. U kunt de regel-ID van Service Bus ook inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>Wat te doen met metrische en diagnostische gegevens in Event Hubs-Logboeken

Nadat u de geselecteerde gegevens worden gestreamd naar Event Hubs, bent u een stapje dichter bij het inschakelen van geavanceerde bewakingsscenario's. Eventhubs fungeert als de voordeur van een gebeurtenispijplijn. Nadat gegevens zijn verzameld in een event hub, kan worden omgezet en opgeslagen met behulp van een realtime-analyseprovider of een opslagadapter. Eventhubs worden losgekoppeld van de productie van een stroom gebeurtenissen van het gebruik van deze gebeurtenissen. Op deze manier kunnen gebeurtenisconsumers toegang tot de gebeurtenissen op hun eigen planning. Zie voor meer informatie over Event Hubs:

- [Wat zijn Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

U kunt gestreamde metrische gegevens in Event Hubs te gebruiken:

- **Servicestatus weergeven door het streamen van gegevens naar Power BI hot pad**. Met behulp van Event Hubs, Stream Analytics en Power BI, kunt u eenvoudig uw metrische gegevens en diagnostische gegevens in bijna realtime inzichten in uw Azure-services te transformeren. Zie voor een overzicht van het instellen van een event hub, gegevens verwerken met Stream Analytics, en gebruik Power BI als uitvoer, [Stream Analytics en Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

- **Stream logboeken naar derden logboekregistratie en telemetrie stromen**. Met behulp van Event Hubs streamen, krijgt u uw logboeken metrische en diagnostische gegevens in verschillende oplossingen van derden controleren en log analytics.

- **Een aangepaste Telemetrie-en logboekregistratie platform**. Doet u al beschikken over een platform op maat gemaakte telemetrie of van plan bent te maken van een? De zeer schaalbare publish-subscribe aard van Event Hubs kunt u logboeken met diagnostische gegevens flexibel opnemen. Zie [Dan Rosanova van handleiding voor het gebruik van Event Hubs in een wereldwijde schaal telemetrie-platform](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Stream naar Storage

U kunt SQL-Database metrische gegevens opslaan en diagnostische logboeken in Azure Storage met behulp van de ingebouwde **archiveren naar een opslagaccount** optie in Azure portal. U kunt ook Storage inschakelen met behulp van een diagnostische instelling via PowerShell-cmdlets, de Azure CLI of de Azure Monitor REST API.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Registreert het schema van de metrische en diagnostische gegevens in het storage-account

Na het instellen van metrische gegevens en diagnostische gegevens over het verzamelen van Logboeken, wordt een storage-container gemaakt in het opslagaccount dat u hebt geselecteerd tijdens de eerste rijen met gegevens beschikbaar zijn. De structuur van de blobs is:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Of eenvoudiger gezegd:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Bijvoorbeeld, kan een blobnaam voor eenvoudige metrische gegevens zijn:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Een blobnaam voor het opslaan van gegevens van een elastische pool ziet eruit zoals:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

## <a name="data-retention-policy-and-pricing"></a>Bewaarbeleid voor gegevens en prijzen

Als u Event Hubs of een Storage-account selecteert, kunt u een bewaarbeleid opgeven. Dit beleid verwijdert de gegevens die ouder is dan een geselecteerde periode. Als u Log Analytics opgeeft, is het bewaarbeleid afhankelijk van de geselecteerde prijscategorie. In dit geval kunt opgegeven gratis eenheden van de gegevensopname gratis bewaking van meerdere databases per maand. Een verbruik van diagnostische gegevens telemetrie dat gratis eenheden overschrijdt mogelijk kosten in rekening. Let erop dat actieve databases met zwaardere werkbelastingen meer gegevens dan niet-actieve databases opnemen. Zie voor meer informatie, [prijzen voor Log analytics](https://azure.microsoft.com/pricing/details/monitor/).

Als u van Azure SQL Analytics gebruikmaakt, kunt u uw gegevensverbruik voor opname in de oplossing bewaken door het selecteren van **OMS-werkruimte** in het navigatiemenu van Azure SQL Analytics en vervolgens de optie **gebruik** en **geschatte kosten**.

## <a name="metrics-and-logs-available"></a>Metrische gegevens en logboeken beschikbaar

Bewaking telemetrie beschikbaar voor Azure SQL Database, wordt elastische pools en beheerd exemplaar hieronder beschreven. Verzamelde bewaking telemetrie in SQL-analyse kan worden gebruikt voor uw eigen aangepaste analyse en de toepassing ontwikkeling met [logboeken-query's van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) taal.

## <a name="basic-metrics"></a>Basismetriek

Raadpleeg de volgende tabellen voor meer informatie over algemene metrische gegevens per resource.

> [!NOTE]
> Basismetriek optie was voorheen bekend als alle metrische gegevens. De wijzigingen zijn aangebracht aan de naamgeving alleen is en er is geen wijziging in de gecontroleerde metrische gegevens. Deze wijziging is gestart om toe te staan voor de introductie van de categorieën voor aanvullende metrische gegevens in de toekomst.

### <a name="basic-metrics-for-elastic-pools"></a>Algemene metrische gegevens voor elastische pools

|**Resource**|**Metrische gegevens**|
|---|---|
|Elastische pool|eDTU-percentage, eDTU gebruikt, eDTU-limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, limiet voor opslag, XTP-opslagpercentage |

### <a name="basic-metrics-for-azure-sql-databases"></a>Algemene metrische gegevens voor Azure SQL-Databases

|**Resource**|**Metrische gegevens**|
|---|---|
|Azure SQL-database|DTU-percentage, DTU gebruikt, DTU limiet, CPU-percentage, fysieke gegevens lezen percentage logboek schrijven percentage, geslaagd/mislukt/geblokkeerd door firewallverbindingen, sessies percentage, percentage van de werknemers, opslag, opslagpercentage, percentage van XTP-opslag, en impassen |

## <a name="basic-logs"></a>Basic-Logboeken

Details van telemetrie beschikbaar voor alle logboeken worden beschreven in de onderstaande tabellen. Raadpleeg [ondersteund registratie in diagnoselogboek](#supported-diagnostic-logging-for-azure-sql-databases-and-instance-databases) om te begrijpen welke logboeken worden ondersteund voor een bepaalde database smaak - Azure SQL-één, gegroepeerd, of database-exemplaar.

### <a name="resource-usage-stats-for-managed-instance"></a>Statistieken voor het gebruik van resource voor het beheerde exemplaar

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure|
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: ResourceUsageStats |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: MANAGEDINSTANCES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van het beheerde exemplaar |
|ResourceId|Resource-URI |
|SKU_s|Beheerd exemplaar product-SKU |
|virtual_core_count_s|Het aantal vCores beschikbaar |
|avg_cpu_percent_s|Gemiddelde CPU-percentage |
|reserved_storage_mb_s|Gereserveerde capaciteit op het beheerde exemplaar |
|storage_space_used_mb_s|Gebruikte opslag op het beheerde exemplaar |
|io_requests_s|Aantal IOPS |
|io_bytes_read_s|Gelezen IOP's bytes |
|io_bytes_written_s|Aantal geschreven bytes van IOPS |

### <a name="query-store-runtime-statistics"></a>Query Store runtime-statistieken

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: QueryStoreRuntimeStatistics |
|OperationName|Naam van de bewerking. Altijd: QueryStoreRuntimeStatisticsEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|query_hash_s|Queryhash |
|query_plan_hash_s|Queryhash-plan |
|statement_sql_handle_s|Ingang voor de instructie sql |
|interval_start_time_d|Datetimeoffset van het interval op te starten in aantal tikken vanaf 1900-1-1 |
|interval_end_time_d|Einde datetimeoffset van het aantal tikken vanaf 1900-1-1-interval |
|logical_io_writes_d|Totaal aantal logische i/o-schrijfbewerkingen |
|max_logical_io_writes_d|Maximumaantal logische i/o-schrijfbewerkingen per uitvoering |
|physical_io_reads_d|Totale aantal fysieke i/o-leesbewerkingen |
|max_physical_io_reads_d|Maximumaantal logische i/o-leesbewerkingen per uitvoering |
|logical_io_reads_d|Totaal aantal logische i/o-leesbewerkingen |
|max_logical_io_reads_d|Maximumaantal logische i/o-leesbewerkingen per uitvoering |
|execution_type_d|Uitvoeringstype |
|count_executions_d|Aantal uitvoeringen van de query |
|cpu_time_d|Totale CPU-tijd gebruikt door de query in microseconden |
|max_cpu_time_d|Maximaal CPU-tijd consumenten door één uitvoering in microseconden |
|dop_d|Som van de mate van parallelle uitvoering |
|max_dop_d|Maximale graad van parallelle uitvoering gebruikt voor één uitvoering |
|rowcount_d|Totaal aantal rijen dat wordt geretourneerd |
|max_rowcount_d|Maximumaantal rijen dat wordt geretourneerd in één uitvoering |
|query_max_used_memory_d|Totale hoeveelheid geheugen die wordt gebruikt in KB |
|max_query_max_used_memory_d|Maximale hoeveelheid geheugen die wordt gebruikt door één uitvoering in KB |
|duration_d|Totale uitvoeringstijd in microseconden |
|max_duration_d|Maximale uitvoeringstijd van één uitvoering |
|num_physical_io_reads_d|Totale aantal fysieke leesbewerkingen |
|max_num_physical_io_reads_d|Maximumaantal fysieke leesbewerkingen per uitvoering |
|log_bytes_used_d|Totale hoeveelheid log bytes dat wordt gebruikt |
|max_log_bytes_used_d|Maximale hoeveelheid log bytes dat wordt gebruikt per uitvoering |
|query_id_d|ID van de query in Query Store |
|plan_id_d|ID van het plan in Query Store |

Meer informatie over [gegevens voor Query Store runtime-statistieken](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Query Store wacht statistieken

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: QueryStoreWaitStatistics |
|OperationName|Naam van de bewerking. Altijd: QueryStoreWaitStatisticsEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|wait_category_s|Categorie van de wachttijd |
|is_parameterizable_s|De query is worden gebruikt als parameter |
|statement_type_s|Type van de instructie |
|statement_key_hash_s|Instructie sleutel-hash |
|exec_type_d|Type van de uitvoering van |
|total_query_wait_time_ms_d|De totale wachttijd van de query op de specifieke wait-categorie |
|max_query_wait_time_ms_d|Maximale wachttijd van de query in de afzonderlijke worden uitgevoerd op de specifieke wait-categorie |
|query_param_type_d|0 |
|query_hash_s|Queryhash in Query Store |
|query_plan_hash_s|Queryhash-plan in Query Store |
|statement_sql_handle_s|Ingang voor de instructie in Query Store |
|interval_start_time_d|Datetimeoffset van het interval op te starten in aantal tikken vanaf 1900-1-1 |
|interval_end_time_d|Einde datetimeoffset van het aantal tikken vanaf 1900-1-1-interval |
|count_executions_d|Aantal uitvoeringen van de query |
|query_id_d|ID van de query in Query Store |
|plan_id_d|ID van het plan in Query Store |

Meer informatie over [Query Store wacht statistiekgegevens](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Gegevensset voor fouten

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQ |
|Category|De naam van de categorie. Altijd: Fouten |
|OperationName|Naam van de bewerking. Altijd: ErrorEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|Bericht|Foutbericht in tekst zonder opmaak |
|user_defined_b|Is de gebruiker gedefinieerde fout bit |
|error_number_d|Foutcode |
|Severity|Ernst van de fout |
|state_d|Status van de fout |
|query_hash_s|Queryhash van de mislukte query, indien beschikbaar |
|query_plan_hash_s|Query-plan hash van de mislukte query, indien beschikbaar |

Meer informatie over [SQL Server-foutberichten](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Database wacht statistieken gegevensset

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: DatabaseWaitStatistics |
|OperationName|Naam van de bewerking. Altijd: DatabaseWaitStatisticsEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|wait_type_s|Naam van het type wait |
|start_utc_date_t [UTC]|Gemeten periode begintijd |
|end_utc_date_t [UTC]|Gemeten periode eindtijd |
|delta_max_wait_time_ms_d|Maximum aantal keer per uitvoering gewacht |
|delta_signal_wait_time_ms_d|Totaal aantal signalen wachttijd |
|delta_wait_time_ms_d|De totale wachttijd in de periode |
|delta_waiting_tasks_count_d|Aantal taken wachten |

Meer informatie over [wacht statistieken van de database](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Time-outs gegevensset

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: Time-outs |
|OperationName|Naam van de bewerking. Altijd: TimeoutEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|error_state_d|Status van foutcode |
|query_hash_s|Query-hash, indien beschikbaar |
|query_plan_hash_s|Query uitvoeren op hash van de planning, indien beschikbaar |

### <a name="blockings-dataset"></a>Blockings gegevensset

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: blokken |
|OperationName|Naam van de bewerking. Altijd: BlockEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|lock_mode_s|LOCK-modus gebruikt door de query |
|resource_owner_type_s|Eigenaar van de vergrendeling |
|blocked_process_filtered_s|Proces rapport XML geblokkeerd |
|duration_d|De duur van de vergrendeling in microseconden |

### <a name="deadlocks-dataset"></a>Impassen gegevensset

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC] |Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: Deadlocks |
|OperationName|Naam van de bewerking. Altijd: DeadlockEvent |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|deadlock_xml_s|Impasse rapport XML |

### <a name="automatic-tuning-dataset"></a>Automatische afstemming gegevensset

|Eigenschap|Description|
|---|---|
|TenantId|Uw tenant-ID |
|SourceSystem|Altijd: Azure |
|TimeGenerated [UTC]|Tijdstempel wanneer het logboek is vastgelegd |
|Type|Altijd: AzureDiagnostics |
|ResourceProvider|De naam van de resourceprovider. Altijd: MICROSOFT.SQL |
|Category|De naam van de categorie. Altijd: AutomaticTuning |
|Resource|Naam van de resource |
|ResourceType|De naam van het resourcetype. Altijd: SERVERS/DATABASES |
|SubscriptionId|Abonnement-GUID voor de database |
|ResourceGroup|Naam van de resourcegroep voor de database |
|LogicalServerName_s|Naam van de server voor de database |
|LogicalDatabaseName_s|Naam van de database |
|ElasticPoolName_s|Naam van de elastische pool voor de database, indien van toepassing |
|DatabaseName_s|Naam van de database |
|ResourceId|Resource-URI |
|RecommendationHash_s|De unieke hash van de aanbeveling voor automatisch afstemmen |
|OptionName_s|Bewerking voor automatisch afstemmen |
|Schema_s|Database-schema |
|Table_s|Tabel beïnvloed |
|IndexName_s|Indexnaam |
|IndexColumns_s|Kolomnaam |
|IncludedColumns_s|Opgenomen kolommen |
|EstimatedImpact_s|Geschatte invloed van automatisch afstemmen aanbeveling JSON |
|Event_s|Type gebeurtenis voor automatisch afstemmen |
|Timestamp_t|De laatst bijgewerkte timestamp |

### <a name="intelligent-insights-dataset"></a>Intelligent Insights-gegevensset

Meer informatie over de [Intelligent Insights-logboekindeling](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het inschakelen van logboekregistratie en om te begrijpen van de metrische gegevens en meld u categorieën die worden ondersteund door de verschillende Azure-services, Zie:

- [Overzicht van metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
- [Overzicht van diagnostische logboeken van Azure](../azure-monitor/platform/diagnostic-logs-overview.md)

Voor meer informatie over Event Hubs, lezen:

- [Wat is Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Aan de slag met Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
