---
title: Azure Service Fabric - bewaking hebt ingesteld met Azure Monitor-Logboeken | Microsoft Docs
description: Meer informatie over het instellen van Azure Monitor-logboeken voor het visualiseren en analyseren van gebeurtenissen voor het bewaken van uw Azure Service Fabric-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: c8f7198b59a0fe7ed6775736f8b97f5b5a262640
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306867"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Azure Monitor-logboeken voor een cluster instellen

Logboeken in Azure Monitor is onze aanbeveling om te controleren op gebeurtenissen op cluster. U kunt instellen als er een Log Analytics-werkruimte via Azure Resource Manager, PowerShell of Azure Marketplace. Als u een bijgewerkte Resource Manager-sjabloon van uw implementatie voor toekomstig gebruik onderhoudt, moet u dezelfde sjabloon gebruiken voor het instellen van uw omgeving Azure Monitor-Logboeken. Implementatie via de Marketplace is het gemakkelijker als u al een cluster geïmplementeerd met diagnostische gegevens zijn ingeschakeld. Als u geen toegang op abonnementsniveau in het account waarnaar u implementeert, implementeert u met behulp van PowerShell of de Resource Manager-sjabloon.

> [!NOTE]
> Als u logboeken van Azure Monitor voor het bewaken van uw cluster instelt, moet u diagnostische gegevens zijn ingeschakeld om gebeurtenissen van cluster- of platform-niveau weer te geven. Raadpleeg [over het instellen van diagnostische gegevens in de Windows-clusters](service-fabric-diagnostics-event-aggregation-wad.md) en [over het instellen van diagnostische gegevens in Linux-clusters](service-fabric-diagnostics-oms-syslog.md) voor meer informatie

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Een Log Analytics-werkruimte implementeren met behulp van Azure Marketplace

Als u een Log Analytics-werkruimte toevoegen wilt nadat u een cluster hebt geïmplementeerd, gaat u naar Azure Marketplace in de portal en zoek naar **Service Fabric-analyse**. Dit is een aangepaste oplossing voor Service Fabric-implementaties met gegevens die specifiek zijn voor Service Fabric. In dit proces maakt u de oplossing (het dashboard om inzichten weer te geven) en de werkruimte (de aggregatie van de clustergegevens van het onderliggende).

1. Selecteer **nieuw** in het navigatiemenu links. 

2. Zoeken naar **Service Fabric-analyse**. Selecteer de resource die wordt weergegeven.

3. Selecteer **Maken**.

    ![Service Fabric-analyse in Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Selecteer in het venster van het maken van Service Fabric-analyse **een werkruimte selecteren** voor de **OMS-werkruimte** veld, en vervolgens **Maak een nieuwe werkruimte**. Vul de vereiste gegevens. De enige vereiste hier is dat het abonnement voor de Service Fabric-cluster en de werkruimte hetzelfde is. Wanneer de gegevens zijn geverifieerd, wordt uw werkruimte begint met de implementatie. De implementatie duurt slechts enkele minuten.

5. Wanneer u klaar bent, selecteert u **maken** opnieuw aan de onderkant van het Service Fabric-analyse maken van het venster. Zorg ervoor dat de nieuwe werkruimte weergegeven onder **OMS-werkruimte**. Deze actie worden de oplossing toegevoegd aan de werkruimte die u hebt gemaakt.

Als u Windows gebruikt, gaat u verder met de volgende stappen voor het verbinding maken met Azure Monitor-logboeken naar het opslagaccount waar uw Clustergebeurtenissen worden opgeslagen. 

>[!NOTE]
>De Service Fabric-analyse-oplossing wordt alleen ondersteund voor Windows-clusters. Voor Linux-clusters, lees dan het artikel op [over het instellen van Azure Monitor-logboeken voor Linux-clusters](service-fabric-diagnostics-oms-syslog.md).  

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Verbinding maken met de Log Analytics-werkruimte in uw cluster 

1. De werkruimte moet worden verbonden met de diagnostische gegevens die afkomstig zijn van uw cluster. Ga naar de resourcegroep waarin u de Service Fabric-analyse-oplossing hebt gemaakt. Selecteer **ServiceFabric\<Naam_werkruimte\>**  en Ga naar de overzichtspagina. U kunt daar Oplossingsinstellingen, werkruimte-instellingen wijzigen en toegang tot de Log Analytics-werkruimte.

2. In het navigatiemenu links, onder **gegevensbronnen voor werkruimte**, selecteer **logboeken voor opslagaccounts**.

3. Op de **logboeken voor Opslagaccounts** weergeeft, schakelt **toevoegen** aan de bovenkant van uw cluster logboeken toevoegen aan de werkruimte.

4. Selecteer **opslagaccount** het juiste account hebt gemaakt in uw cluster toe te voegen. Als u de standaardnaam gebruikt, wordt het opslagaccount is **sfdg\<resourceGroupName\>** . U kunt dit ook bevestigen met de Azure Resource Manager-sjabloon gebruikt voor het implementeren van uw cluster, door het controleren van de waarde die wordt gebruikt voor **applicationDiagnosticsStorageAccountName**. Als de naam niet wordt weergegeven, schuif naar beneden en selecteer **meer laden**. Selecteer de naam van het opslagaccount.

5. Het gegevenstype opgeven. Stel deze in op **Service Fabric-gebeurtenissen**.

6. Zorg ervoor dat de bron wordt automatisch ingesteld op **WADServiceFabric\*EventTable**.

7. Selecteer **OK** verbinding maken met uw werkruimte van uw cluster Logboeken.

    ![Logboeken voor opslagaccounts toevoegen aan Azure Monitor-Logboeken](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Het account wordt nu weergegeven als onderdeel van uw storage-account in gegevensbronnen van uw werkruimte registreert.

U kunt de Service Fabric-analyse-oplossing hebt toegevoegd in een Log Analytics-werkruimte die nu correct met het platform van uw cluster en de logboektabel die toepassing verbonden is. U kunt aanvullende bronnen toevoegen aan de werkruimte op dezelfde manier.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Azure Monitor-logboeken met Azure Resource Manager implementeren

Wanneer u een cluster met behulp van Resource Manager-sjabloon implementeert, de sjabloon is, wordt een nieuwe Log Analytics-werkruimte maakt, voegt u de Service Fabric-oplossing toe aan de werkruimte en geconfigureerd voor het lezen van gegevens van de juiste opslagtabellen.

U kunt gebruiken en wijzigen [deze voorbeeldsjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) om te voldoen aan uw vereisten. Deze sjabloon doet het volgende

* Hiermee maakt u een Service Fabric-cluster 5 knooppunten
* Hiermee maakt u een Log Analytics-werkruimte en een Service Fabric-oplossing
* Hiermee configureert u de Log Analytics-agent voor het verzamelen en verzenden van 2 voorbeeld-prestatiemeteritems in de werkruimte
* Hiermee configureert u WAD voor het verzamelen van Service Fabric en stuurt deze naar Azure storage-tabellen (WADServiceFabric * EventTable)
* Hiermee configureert u de Log Analytics-werkruimte als u wilt de gebeurtenissen kan lezen uit deze tabellen


U kunt de sjabloon als de upgrade van een Resource Manager met uw cluster implementeren met behulp van de `New-AzResourceGroupDeployment` API in de Azure PowerShell-module. Een van de voorbeeldopdracht zou zijn:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Resource Manager detecteert dat met deze opdracht een update voor een bestaande resource is. Alleen de wijzigingen tussen de sjabloon die de bestaande implementatie te stimuleren en de nieuwe sjabloon die worden verwerkt.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Implementeren van Azure Monitor-logboeken met Azure PowerShell

U kunt uw log analytics-resource via PowerShell ook implementeren met behulp van de `New-AzOperationalInsightsWorkspace` opdracht. Als u wilt deze methode gebruikt, zorg ervoor dat u hebt geïnstalleerd [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-Az-ps). Gebruik dit script voor een nieuwe Log Analytics-werkruimte maken en de Service Fabric-oplossing aan toevoegen: 

```powershell

$SubID = "<subscription ID>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Sign in to Azure and access the correct subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Wanneer u klaar bent, volg de stappen in de voorgaande sectie logboeken van Azure Monitor verbinden met de juiste storage-account.

U kunt ook andere oplossingen toevoegen of andere wijzigingen aanbrengen in uw Log Analytics-werkruimte met behulp van PowerShell. Zie voor meer informatie, [beheren Azure Monitor registreert met behulp van PowerShell](../azure-monitor/platform/powershell-workspace-configuration.md).

## <a name="next-steps"></a>Volgende stappen
* [De Log Analytics-agent implementeren](service-fabric-diagnostics-oms-agent.md) naar uw knooppunten om te verzamelen prestatiemeteritems en verzamelen van Logboeken voor uw containers en docker-statistieken
* Familiarized ophalen met de [zoeken en uitvoeren van query's](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van Azure Monitor-Logboeken
* [Weergaveontwerper aangepaste weergaven maken in Azure Monitor-Logboeken gebruiken](../azure-monitor/platform/view-designer.md)
