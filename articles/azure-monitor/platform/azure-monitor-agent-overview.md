---
title: Overzicht van Azure Monitor-agent
description: Overzicht van de Azure Monitor-agent (AMA), waarmee bewakings gegevens worden verzameld van het gast besturingssysteem van virtuele machines.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/10/2020
ms.openlocfilehash: e38d59ff1eb31dd5fc3ecf6b7df6b12504141d5e
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88082969"
---
# <a name="azure-monitor-agent-overview-preview"></a>Overzicht van Azure Monitor-agent (preview)
De Azure Monitor-agent (AMA) verzamelt bewakings gegevens van het gast besturingssysteem van virtuele machines en levert deze aan Azure Monitor. In deze artikelen vindt u een overzicht van de Azure Monitor-agent, inclusief hoe u deze kunt installeren en hoe u gegevens verzameling kunt configureren.

## <a name="relationship-to-other-agents"></a>Relatie met andere agents
De Azure Monitor-agent vervangt de volgende agents die momenteel worden gebruikt door Azure Monitor om gast gegevens van virtuele machines te verzamelen:

- [Log Analytics agent](log-analytics-agent.md) : verzendt gegevens naar log Analytics-werk ruimte en ondersteunt Azure monitor voor VM's-en bewakings oplossingen.
- [Diagnostische extensie](diagnostics-extension-overview.md) : verzendt gegevens naar Azure monitor meet waarden (alleen Windows), Azure Event Hubs en Azure Storage.
- [Telegrafa-agent](collect-custom-metrics-linux-telegraf.md) : Hiermee worden gegevens verzonden naar Azure monitor metrieken (alleen voor Linux).

Naast het consolideren van deze functionaliteit in één agent biedt de Azure Monitor-agent de volgende voor delen ten opzichte van de bestaande agents:

- Bewakings bereik. Configureer verzameling centraal voor verschillende gegevens sets van verschillende sets virtuele machines.  
- Linux multi-multihoming: gegevens van Linux-Vm's naar meerdere werk ruimten verzenden.
- Windows-gebeurtenis filtering: XPATH-query's gebruiken om te filteren welke Windows-gebeurtenissen worden verzameld.
- Verbeterd extensie beheer: Azure Monitor-agent gebruikt een nieuwe methode voor het verwerken van uitbreid baarheid die transparanter en beter kan worden beheerd dan Management Packs en Linux-invoeg toepassingen in de huidige Log Analytics agents.

### <a name="changes-in-data-collection"></a>Wijzigingen in gegevens verzameling
De methoden voor het definiëren van het verzamelen van gegevens voor de bestaande agents zijn verschillend van elkaar en elk een probleem met Azure Monitor agent.

- De configuratie van Log Analytics-agent wordt opgehaald uit een Log Analytics-werk ruimte. Dit is eenvoudig te configureren, maar moeilijk om onafhankelijke definities te definiëren voor verschillende virtuele machines. Er kunnen alleen gegevens worden verzonden naar een Log Analytics-werk ruimte.

- Diagnostische uitbrei ding heeft een configuratie voor elke virtuele machine. Zo kunt u eenvoudig onafhankelijke definities definiëren voor verschillende virtuele machines, maar moeilijk te beheren. Er kunnen alleen gegevens worden verzonden naar Azure Monitor metrieken, Azure Event Hubs of Azure Storage. Voor Linux-agents is de open source-telegrafie agent vereist om gegevens te verzenden naar Azure Monitor meet waarden.

Azure Monitor agent gebruikt [gegevens verzamelings regels (DCR)](data-collection-rule-overview.md) om gegevens te configureren die van elke agent moeten worden verzameld. Regels voor het verzamelen van gegevens zorgen ervoor dat de verzamelings instellingen op schaal kunnen worden beheerd, terwijl er nog steeds unieke, scoped configuraties voor subsets van machines worden ingeschakeld. Ze zijn onafhankelijk van de werk ruimte en onafhankelijk van de virtuele machine, waardoor ze eenmaal kunnen worden gedefinieerd en opnieuw worden gebruikt tussen computers en omgevingen. Zie [gegevens verzameling configureren voor de Azure monitor-agent (preview)](data-collection-rule-azure-monitor-agent.md).



## <a name="current-limitations"></a>Huidige beperkingen
De volgende beperkingen zijn van toepassing tijdens de open bare preview van de Azure Monitor-agent:

- De Azure Monitor-agent biedt geen ondersteuning voor oplossingen en inzichten zoals Azure Monitor voor VM's en Azure Security Center. Het enige scenario dat momenteel wordt ondersteund, is het verzamelen van gegevens met behulp van de regels voor gegevens verzameling die u configureert. 
- Regels voor het verzamelen van gegevens moeten in dezelfde regio worden gemaakt als de Log Analytics werk ruimte die als bestemming wordt gebruikt.
- Er worden momenteel alleen virtuele Azure-machines ondersteund. On-premises virtuele machines, virtuele-machine schaal sets, Arc voor servers, Azure Kubernetes-service en andere compute-resource typen worden momenteel niet ondersteund.
- De virtuele machine moet toegang hebben tot de volgende HTTPS-eind punten:
  - *.ods.opinsights.azure.com
  - *. ingest.monitor.azure.com
  - *. control.monitor.azure.com


## <a name="coexistence-with-other-agents"></a>Samen werking met andere agents
De Azure Monitor-agent kan naast de bestaande agents worden gebruikt, zodat u de bestaande functionaliteit ervan kunt blijven gebruiken tijdens de evaluatie of migratie. Dit is met name belang rijk vanwege de beperkingen in de open bare Preview bij de ondersteuning van bestaande oplossingen. Wees voorzichtig met het verzamelen van dubbele gegevens, omdat dit ertoe kan leiden dat query resultaten worden gescheefd en dat er extra kosten in rekening worden gebracht voor gegevens opname en-retentie.

Azure Monitor voor VM's gebruikt bijvoorbeeld de Log Analytics-agent om prestatie gegevens naar een Log Analytics-werk ruimte te verzenden. Mogelijk hebt u de werk ruimte ook geconfigureerd voor het verzamelen van Windows-gebeurtenissen en syslog-gebeurtenissen van agents. Als u de Azure Monitor Agent installeert en een regel voor het verzamelen van gegevens voor dezelfde gebeurtenissen en prestatie gegevens maakt, worden er dubbele gegevens geretourneerd.


## <a name="costs"></a>Kosten
Er zijn geen kosten verbonden aan Azure Monitor-agent, maar mogelijk worden er kosten in rekening gebracht voor de gegevens die zijn opgenomen. Zie [Azure monitor prijzen](https://azure.microsoft.com/pricing/details/monitor/) voor meer informatie over log Analytics gegevens verzameling en-retentie en voor de metriek van de klant.

## <a name="data-sources-and-destinations"></a>Gegevens bronnen en doelen
De volgende tabel bevat de gegevens typen die u op dit moment kunt verzamelen met de Azure Monitor-agent met behulp van regels voor het verzamelen van gegevens en waar u deze gegevens kunt verzenden. Zie [wat wordt bewaakt door Azure monitor?](../monitor-reference.md) voor een lijst met inzichten, oplossingen en andere oplossingen die gebruikmaken van de Azure monitor-agent voor het verzamelen van andere soorten gegevens.


De Azure Monitor-agent verzendt gegevens naar Azure Monitor metrieken of een Log Analytics-werk ruimte die Azure Monitor logboeken ondersteunt.

| Gegevensbron | Bestemmingen | Beschrijving |
|:---|:---|:---|
| Prestaties        | Azure Monitor metrische gegevens<br>Log Analytics-werkruimte | Numerieke waarden meten de prestaties van verschillende aspecten van het besturings systeem en de werk belastingen. |
| Windows-gebeurtenis logboeken | Log Analytics-werkruimte | Gegevens die worden verzonden naar het Windows-systeem voor gebeurtenis registratie. |
| Syslog             | Log Analytics-werkruimte | Informatie die wordt verzonden naar het systeem voor het registreren van Linux-gebeurtenissen. |


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
De volgende besturings systemen worden momenteel ondersteund door de Azure Monitor-agent.

### <a name="windows"></a>Windows 
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012
  - Windows Server 2012 R2

### <a name="linux"></a>Linux
  - CentOS 6<sup>,</sup>7
  - Debian 9, 10
  - Oracle Linux 6<sup>1</sup>, 7
  - RHEL 6<sup>, 7, 8</sup>
  - SLES 11, 12, 15
  - Ubuntu 14,04 LTS, 16,04 LTS, 18,04 LTS

> [!IMPORTANT]
> <sup>1</sup> Voor deze distributies om syslog-gegevens te verzenden, moet u rsyslog verwijderen en syslog-ng installeren.


## <a name="security"></a>Beveiliging
Voor de Azure Monitor-agent zijn geen sleutels vereist, maar hiervoor is een door het [systeem toegewezen beheerde identiteit](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#system-assigned-managed-identity)vereist. U moet een door het systeem toegewezen beheerde identiteit hebben ingeschakeld op elke virtuele machine voordat u de agent implementeert.


## <a name="install-the-azure-monitor-agent"></a>De Azure Monitor-agent installeren
De Azure Monitor-agent wordt geïmplementeerd als een [Azure VM-extensie](../../virtual-machines/extensions/overview.md) met de details in de volgende tabel. 

| Eigenschap | Windows | Linux |
|:---|:---|:---|
| Uitgever | Micro soft. Azure. monitor  | Micro soft. Azure. monitor |
| Type      | AzureMonitorWindowsAgent | AzureMonitorLinuxAgent  |
| TypeHandlerVersion  | 1.0 | 0.9 |

Installeer de Azure Monitor agent met behulp van een van de methoden om virtuele-machine agents te installeren, inclusief het volgende met behulp van Power shell of CLI. U kunt de agent ook installeren en gegevens verzameling configureren op virtuele machines in uw Azure-abonnement met behulp van de portal met de procedure die wordt beschreven in [gegevens verzameling configureren voor de Azure monitor-agent (preview)](data-collection-rule-azure-monitor-agent.md#create-using-the-azure-portal).

### <a name="windows"></a>Windows

# <a name="cli"></a>[CLI](#tab/CLI1)

```azurecli
az vm extension set --name AzureMonitorWindowsAgent --publisher Microsoft.Azure.Monitor --version 1.0 --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell1)

```powershell
Set-AzVMExtension -Name AMAWindows -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -Version 1.0 -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---


### <a name="linux"></a>Linux

# <a name="cli"></a>[CLI](#tab/CLI2)

```azurecli
az vm extension set --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --version 0.9 --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell2)

```powershell
Set-AzVMExtension -Name AMALinux -ExtensionType AzureMonitorLinuxAgent -Publisher Microsoft.Azure.Monitor -Version 0.9 -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---

## <a name="next-steps"></a>Volgende stappen

- [Maak een gegevensverzamelings regel](data-collection-rule-azure-monitor-agent.md) voor het verzamelen van gegevens van de agent en verzend deze naar Azure monitor.
