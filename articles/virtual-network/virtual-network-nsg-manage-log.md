---
title: Network security group gebeurtenis en een regel voor teller diagnostische logboeken in Azure
titlesuffix: Azure Virtual Network
description: Informatie over het inschakelen van de gebeurtenis en een regel voor teller diagnostische logboeken voor een Azure-netwerkbeveiligingsgroep.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: kumud
ms.openlocfilehash: b3225d8d2f9eb7ccd0f4087d93cd9c1d940783d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64714688"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Diagnostische logboekregistratie voor een netwerkbeveiligingsgroep

Een netwerkbeveiligingsgroep (NSG) bevat regels die verkeer naar een subnet van een virtueel netwerk, de netwerkinterface of beide toestaan of weigeren. Wanneer u Diagnostische logboekregistratie voor een NSG inschakelt, kunt u zich aanmelden met de volgende categorieën van informatie:

* **Gebeurtenis:** Vermeldingen worden geregistreerd voor welke NSG regels worden toegepast op virtuele machines, op basis van MAC-adres. De status van deze regels worden elke 60 seconden worden verzameld.
* **Teller voor regel:** Bevat vermeldingen voor het aantal keren dat elke NSG-regel is toegepast om te weigeren of verkeer toestaan.

Logboeken met diagnostische gegevens zijn alleen beschikbaar voor nsg's die zijn geïmplementeerd via het Azure Resource Manager-implementatiemodel. U kunt Diagnostische logboekregistratie voor Netwerkbeveiligingsgroepen die zijn geïmplementeerd via het klassieke implementatiemodel niet inschakelen. Zie voor een beter begrip van de twee modellen, [inzicht krijgen in Azure-implementatiemodellen](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Registratie in diagnoselogboek afzonderlijk is ingeschakeld voor *elke* NSG die u wenst te verzamelen van diagnostische gegevens voor. Als u geïnteresseerd bent in operationeel is, of activiteit, in plaats daarvan registreert, bekijk hoe Azure [activiteitenregistratie](../azure-monitor/platform/activity-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Logboekregistratie inschakelen

U kunt de [Azure Portal](#azure-portal), [PowerShell](#powershell), of de [Azure CLI](#azure-cli) Diagnostische logboekregistratie inschakelen.

### <a name="azure-portal"></a>Azure-portal

1. Meld u aan bij de [portal](https://portal.azure.com).
2. Selecteer **alle services**, typt u *netwerkbeveiligingsgroepen*. Wanneer **Netwerkbeveiligingsgroepen** worden weergegeven in de lijst met zoekresultaten, selecteert u deze.
3. Selecteer de gewenste logboekregistratie inschakelen voor NSG.
4. Onder **bewaking**, selecteer **diagnoselogboeken**, en selecteer vervolgens **diagnostische gegevens inschakelen**, zoals wordt weergegeven in de volgende afbeelding:

   ![Diagnostische gegevens inschakelen](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. Onder **diagnostische instellingen**, invoeren, of Selecteer de volgende informatie en selecteer vervolgens **opslaan**:

    | Instelling                                                                                     | Waarde                                                          |
    | ---------                                                                                   |---------                                                       |
    | Name                                                                                        | De naam van uw keuze.  Bijvoorbeeld: *myNsgDiagnostics*      |
    | **Archiveren naar een opslagaccount**, **Stream naar een event hub**, en **verzenden naar Log Analytics** | U kunt zo veel bestemmingen als u kiest. Zie voor meer informatie over elk [melden bestemmingen](#log-destinations).                                                                                                                                           |
    | LOG                                                                                         | Selecteer een van beide of beide logboekcategorieën. Zie voor meer informatie over de gegevens die zijn geregistreerd voor elke categorie, [categorieën zich](#log-categories).                                                                                                                                             |
6. Logboeken bekijken en analyseren. Zie voor meer informatie, [weergeven en logboeken analyseren](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/powershell), of door te voeren PowerShell vanaf uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u PowerShell vanaf uw computer uitvoeren, moet u de Azure PowerShell-module, versie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` op uw computer, de geïnstalleerde versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook om uit te voeren `Connect-AzAccount` zich aanmeldt bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions).

Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u de Id van een bestaande NSG. Als u een bestaande NSG hebt, kunt u maken met [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

Ophalen van de netwerkbeveiligingsgroep die u wilt om in te schakelen Diagnostische logboekregistratie voor met [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup). Bijvoorbeeld, om op te halen van een NSG met de naam *myNsg* die zich in een resourcegroep met de naam *myResourceGroup*, voer de volgende opdracht:

```azurepowershell-interactive
$Nsg=Get-AzNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

U kunt Logboeken met diagnostische gegevens schrijven naar drie typen van de bestemming. Zie voor meer informatie, [melden bestemmingen](#log-destinations). In dit artikel, logboeken zijn verzonden naar de *Log Analytics* bestemming, als voorbeeld. Ophalen van een bestaande Log Analytics-werkruimte met [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace). Bijvoorbeeld, om op te halen van een bestaande werkruimte met de naam *myWorkspace* in een resourcegroep met de naam *myWorkspaces*, voer de volgende opdracht:

```azurepowershell-interactive
$Oms=Get-AzOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Als u geen een bestaande werkruimte hebt, kunt u maken met [New-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace).

Er zijn twee categorieën van logboekregistratie die kunt u Logboeken voor inschakelen. Zie voor meer informatie, [categorieën zich](#log-categories). Diagnostische logboekregistratie inschakelen voor de NSG met [Set AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting). Het volgende voorbeeld registreert gebeurtenis zowel teller categoriegegevens aan de werkruimte voor een Netwerkbeveiligingsgroep, met behulp van de id's voor de NSG en de werkruimte die u eerder hebt opgehaald:

```azurepowershell-interactive
Set-AzDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Als u alleen gegevens voor één categorie of de andere, in plaats van beide vastleggen wilt, voegt u toe de `-Categories` optie naar de vorige opdracht, gevolgd door *NetworkSecurityGroupEvent* of *NetworkSecurityGroupRuleCounter*. Als u zich wilt aanmelden met een andere [bestemming](#log-destinations) dan een Log Analytics-werkruimte, gebruikt u de juiste parameters voor een Azure [opslagaccount](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Event Hub](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Logboeken bekijken en analyseren. Zie voor meer informatie, [weergeven en logboeken analyseren](#view-and-analyze-logs).

### <a name="azure-cli"></a>Azure-CLI

U kunt de opdrachten die volgen in uitvoeren de [Azure Cloud Shell](https://shell.azure.com/bash), of door de Azure CLI uitvoert op uw computer. De Azure Cloud Shell is een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Als u de CLI vanaf uw computer uitvoert, moet u versie 2.0.38 of hoger. Voer `az --version` op uw computer, de geïnstalleerde versie te vinden. Als u upgraden wilt, raadpleegt u [Azure CLI installeren](/cli/azure/install-azure-cli?view=azure-cli-latest). Als u de CLI lokaal uitvoert, moet u ook om uit te voeren `az login` zich aanmeldt bij Azure met een account met de [benodigde machtigingen](virtual-network-network-interface.md#permissions).

Als u wilt vastleggen van diagnostische gegevens inschakelen, moet u de Id van een bestaande NSG. Als u een bestaande NSG hebt, kunt u maken met [az network nsg maken](/cli/azure/network/nsg#az-network-nsg-create).

Ophalen van de netwerkbeveiligingsgroep die u wilt om in te schakelen Diagnostische logboekregistratie voor met [az network nsg show](/cli/azure/network/nsg#az-network-nsg-show). Bijvoorbeeld, om op te halen van een NSG met de naam *myNsg* die zich in een resourcegroep met de naam *myResourceGroup*, voer de volgende opdracht:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

U kunt Logboeken met diagnostische gegevens schrijven naar drie typen van de bestemming. Zie voor meer informatie, [melden bestemmingen](#log-destinations). In dit artikel, logboeken zijn verzonden naar de *Log Analytics* bestemming, als voorbeeld. Zie voor meer informatie, [categorieën zich](#log-categories).

Diagnostische logboekregistratie inschakelen voor de NSG met [az monitor diagnostic-settings maken](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Het volgende voorbeeld registreert gebeurtenis zowel teller categoriegegevens aan een bestaande werkruimte met de naam *myWorkspace*, waar zich bevindt in een resourcegroep met de naam *myWorkspaces*, en de ID van de NSG die u hebt opgehaald eerder:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Als u geen een bestaande werkruimte hebt, kunt u maken met behulp van één de [Azure-portal](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [PowerShell](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace). Er zijn twee categorieën van logboekregistratie die kunt u Logboeken voor inschakelen.

Als u wilt dat alleen om gegevens voor één categorie of het andere te registreren, moet u de categorie die u niet wilt dat om gegevens te registreren in de vorige opdracht verwijderen. Als u zich wilt aanmelden met een andere [bestemming](#log-destinations) dan een Log Analytics-werkruimte, gebruikt u de juiste parameters voor een Azure [opslagaccount](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Event Hub](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Logboeken bekijken en analyseren. Zie voor meer informatie, [weergeven en logboeken analyseren](#view-and-analyze-logs).

## <a name="log-destinations"></a>Logboek bestemmingen

Diagnostische gegevens kan zijn:
- [Naar een Azure Storage-account geschreven](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), voor controle of handmatige controle. U kunt de bewaartijd (in dagen) met behulp van de instellingen voor resourcediagnose opgeven.
- [Gestreamd naar een Event hub](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor opname van een service van derden of aangepaste analytics-oplossing, zoals Power BI.
- [Naar Azure Monitor-Logboeken geschreven](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Logboekcategorieën

Gegevens in JSON-indeling is geschreven voor de volgende logboekcategorieën:

### <a name="event"></a>Gebeurtenis

Het gebeurtenislogboek bevat informatie over welke NSG-regels worden toegepast op virtuele machines, op basis van MAC-adres. De volgende gegevens wordt voor elke gebeurtenis geregistreerd. In het volgende voorbeeld worden de gegevens worden geregistreerd voor een virtuele machine met het IP-adres 192.168.1.4 en een MAC-adres van 00-0D-3A-92-6A-7C:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":"[PRIORITY-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>Regel teller

Het logboek van de teller regel bevat informatie over elke regel toegepast op resources. De voorbeeldgegevens van het volgende wordt geregistreerd telkens wanneer die een regel wordt toegepast. In het volgende voorbeeld worden de gegevens worden geregistreerd voor een virtuele machine met het IP-adres 192.168.1.4 en een MAC-adres van 00-0D-3A-92-6A-7C:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> De bron-IP-adres voor de communicatie wordt niet geregistreerd. U kunt inschakelen [NSG-stroomlogboeken](../network-watcher/network-watcher-nsg-flow-logging-portal.md) voor een NSG echter die registreert alle van de regel iteminformatie, evenals de bron-IP-adres dat de communicatie wordt gestart. NSG-stroomlogboekgegevens worden naar een Azure Storage-account geschreven. U kunt de gegevens analyseren met de [traffic analytics](../network-watcher/traffic-analytics.md) mogelijkheden van Azure Network Watcher.

## <a name="view-and-analyze-logs"></a>Logboeken bekijken en analyseren

Zie voor informatie over het weergeven van diagnostische logboekgegevens, [diagnostische logboeken van Azure-overzicht](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u diagnostische gegevens verzenden:
- **Logboeken in Azure Monitor**: U kunt de [network security group analytics](../azure-monitor/insights/azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-azure-monitor
) oplossing voor het verbeterde inzicht te krijgen. De oplossing biedt visualisaties voor NSG-regels die verkeer per MAC-adres van de netwerkinterface in een virtuele machine toestaan of weigeren.
- **Azure Storage-account**: Gegevens worden geschreven naar een bestand PT1H.json. U vindt de:
  - Gebeurtenislogboek in het volgende pad: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
  - Logboek voor regel prestatiemeteritems in het volgende pad: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [activiteitenregistratie](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), voorheen bekend als de controle- of operationele Logboeken. Activiteit-logboekregistratie is standaard ingeschakeld voor Netwerkbeveiligingsgroepen die zijn gemaakt via een Azure-implementatiemodel. Om te bepalen welke bewerkingen zijn voltooid op nsg's in het activiteitenlogboek, zoeken naar gegevens die bestaan uit de volgende resourcetypen:
  - Microsoft.ClassicNetwork/networkSecurityGroups
  - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
  - Microsoft.Network/networkSecurityGroups
  - Microsoft.Network/networkSecurityGroups/securityRules
- Zie voor meer informatie over diagnostische gegevens, om op te nemen van de bron-IP-adres voor elke stroom [NSG-stroomlogboeken](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).