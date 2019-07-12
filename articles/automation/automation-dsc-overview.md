---
title: Overzicht van Azure Automation-status
description: Een overzicht van Azure Automation State Configuration (DSC), de voorwaarden en bekende problemen
keywords: PowerShell dsc, configuratie van gewenste status, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: a5d4657f87b0a6cbae0699c5a2f95773ff55f633
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798439"
---
# <a name="azure-automation-state-configuration-overview"></a>Overzicht van Azure Automation-status

Configuratie van de status van Azure Automation is een Azure-service waarmee u te schrijven, beheren en compileren PowerShell Desired State Configuration (DSC) [configuraties](/powershell/dsc/configurations), importeren [DSC-Resources](/powershell/dsc/resources), en configuraties toewijzen aan doelknooppunten, allemaal in de cloud.

## <a name="why-use-azure-automation-state-configuration"></a>Waarom Azure Automation State Configuration gebruiken

Statusconfiguratie van Azure Automation biedt verschillende voordelen boven het gebruik van DSC buiten Azure.

### <a name="built-in-pull-server"></a>Ingebouwde pull-server

Configuratie van de status van Azure Automation biedt een DSC-pull-server die vergelijkbaar is met de [Windows functie DSC-Service](/powershell/dsc/pullserver) zodat doelknooppunten automatisch configuraties ontvangen, voldoen aan de gewenste status en rapporteren over hun naleving van beleid. De ingebouwde pull-server in Azure Automation elimineert de noodzaak te instellen en onderhouden van uw eigen pull-server. Azure Automation kunt zich richten op virtuele of fysieke Windows- of Linux-systemen, in de cloud of on-premises.

### <a name="management-of-all-your-dsc-artifacts"></a>Beheer van alle DSC-artefacten

Statusconfiguratie van Azure Automation biedt dezelfde beheerlaag [PowerShell Desired State Configuration](/powershell/dsc/overview) als Azure Automation voor PowerShell-scripts biedt.

Vanuit Azure portal of PowerShell, kunt u alle uw DSC-configuraties, resources en doelknooppunten kunt beheren.

![Schermafbeelding van de pagina Azure Automation](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-azure-monitor-logs"></a>Rapportage gegevens importeren in Azure Monitor-Logboeken

Gedetailleerde status rapportagegegevens verzenden knooppunten die worden beheerd met Azure Automation State Configuration naar de ingebouwde pull-server. U kunt Azure Automation State Configuration voor het verzenden van deze gegevens aan uw Log Analytics-werkruimte configureren. Zie voor meer informatie over het verzenden van gegevens over de toepassingsstatus State Configuration naar uw Log Analytics-werkruimte, [doorsturen Azure Automation State Configuration rapportagegegevens naar Logboeken van Azure Monitor](automation-dsc-diagnostics.md).

## <a name="prerequisites"></a>Vereisten

Houd rekening met de volgende vereisten bij het gebruik van Azure Automation State Configuration (DSC).

### <a name="operating-system-requirements"></a>Vereisten voor besturingssysteem

Voor de knooppunten waarop Windows wordt uitgevoerd, zijn de volgende versies worden ondersteund:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

Voor de knooppunten waarop Linux wordt uitgevoerd, zijn de volgende distributies/versies worden ondersteund:

De DSC-Linux-extensie biedt ondersteuning voor alle Linux-distributies [goedgekeurd op Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) , met uitzondering:

Distributie | Version
-|-
Debian  | Alle versies
Ubuntu  | 18.04

### <a name="dsc-requirements"></a>DSC-vereisten

Voor alle Windows-knooppunten die worden uitgevoerd in Azure, [WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure) wordt geïnstalleerd tijdens onboarding.  Knooppunten met Windows Server 2012 en Windows 7, [WinRM wordt ingeschakeld](https://docs.microsoft.com/powershell/dsc/troubleshooting/troubleshooting#winrm-dependency).

Voor alle Linux-knooppunten die worden uitgevoerd in Azure, [PowerShell DSC voor Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) wordt geïnstalleerd tijdens onboarding.

### <a name="network-planning"></a>Particuliere netwerken configureren

Als uw knooppunten zich in een particulier netwerk bevinden, zijn de volgende poort en URL's vereist voor State Configuration (DSC) om te communiceren met Automation:

* Poort: Alleen TCP 443 is vereist voor uitgaande toegang tot internet.
* Algemene URL: *.azure-automation.net
* Algemene URL van de VS (overheid) Virginia: *.azure-automation.us
* Agent-service: https://\<workspaceId\>.agentsvc.azure-automation.net

Op deze manier verbinding met het netwerk voor het beheerde knooppunt om te communiceren met Azure Automation.
Als u DSC-resources die communicatie tussen knooppunten, zoals de [WaitFor * resources](https://docs.microsoft.com/powershell/dsc/reference/resources/windows/waitForAllResource), moet u ook verkeer tussen knooppunten toe te staan.
Zie de documentatie voor elke DSC-resource om te begrijpen die netwerkvereisten.

#### <a name="proxy-support"></a>Proxy-ondersteuning

Proxy-ondersteuning voor de DSC-agent is beschikbaar in Windows versie 1809 en hoger.
Als u wilt configureren met deze optie, stel de waarde voor **ProxyURL** en **ProxyCredential** in de [metaconfiguration script](automation-dsc-onboarding.md#generating-dsc-metaconfigurations) gebruikt voor het registreren van de knooppunten.
Proxy is niet beschikbaar in DSC voor eerdere versies van Windows.

Voor Linux-knooppunten, de DSC-agent biedt ondersteuning voor proxy en de variabele gebruikt om te bepalen van de url gaan gebruiken.

#### <a name="azure-state-configuration-network-ranges-and-namespace"></a>Bereik van Azure staat configuratie van netwerken en de naamruimte

Het is raadzaam om de adressen die worden vermeld bij het definiëren van uitzonderingen te gebruiken. Voor IP-adressen die u kunt downloaden de [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Dit bestand wordt wekelijks bijgewerkt, en heeft de bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken.

Als u een Automation-account dat gedefinieerd voor een specifieke regio hebt, kunt u communicatie met dat regionaal datacenter kunt beperken. De volgende tabel bevat de DNS-record voor elke regio:

| **Regio** | **DNS-record** |
| --- | --- |
| US - west-centraal | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| US - zuid-centraal |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| US - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Canada - midden |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa -west |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Europa - noord |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Azië - zuidoost |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| India - centraal |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan - oost |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australië - zuidoost |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Verenigd Koninkrijk Zuid | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| VS (overheid) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Voor een lijst met IP-adressen in plaats van regionamen regio, downloadt u de [Azure Datacenter IP-adres](https://www.microsoft.com/download/details.aspx?id=41653) XML-bestand van het Microsoft Download Center.

> [!NOTE]
> Het Azure Datacenter IP-adres XML-bestand bevat de IP-adresbereiken die worden gebruikt in de Microsoft Azure-datacenters. Het bestand bevat compute, SQL en storage.
>
>Wekelijks wordt een bijgewerkt bestand geplaatst. Het bestand weerspiegelt bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacenters voor ten minste één week.
>
> Er is een goed idee om te downloaden van het nieuwe XML-bestand elke week. Werk vervolgens uw site services die worden uitgevoerd in Azure correct kan worden geïdentificeerd. Gebruikers van Azure ExpressRoute Houd er rekening mee dat dit bestand wordt gebruikt voor het bijwerken van de aankondiging Border Gateway Protocol (BGP) van Azure-ruimte in de eerste week van elke maand.

## <a name="introduction-video"></a>Introductievideo

Wilt u liever kijken dan lezen? Bekijk de volgende video van mei 2015 wanneer Azure Automation State Configuration voor het eerst werd aangekondigd hebben.

> [!NOTE]
> Hoewel de concepten en de levenscyclus van die in deze video worden besproken correct zijn, Azure Automation State Configuration heeft veel vooruitgang geboekt met sinds deze video is opgenomen. Het is nu algemeen beschikbaar, heeft een veel uitgebreidere gebruikersinterface in Azure portal en biedt ondersteuning voor veel extra mogelijkheden.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Voor meer informatie over hoe voor onboarding knooppunten, Zie [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
