---
title: Hybrid Runbook Worker voor Azure Automation in Windows
description: In dit artikel bevat informatie over het installeren van een Azure Automation Hybrid Runbook Worker die u gebruiken kunt om uit te voeren van runbooks op Windows gebaseerde computers in uw lokale datacenter of cloudomgeving.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cc3307a4f32d77b9b8d259ac846c4db1c1ae4a99
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002519"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Een Windows Hybrid Runbook Worker implementeren

U kunt de functie Hybrid Runbook Worker van Azure Automation gebruiken voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van deze lokale resources. Runbooks worden opgeslagen en beheerd in Azure Automation en vervolgens geleverd aan een of meer opgegeven computers. In dit artikel wordt beschreven hoe u de Hybrid Runbook Worker installeren op een Windows-computer.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>De Windows Hybrid Runbook Worker installeren

Als u wilt installeren en configureren van een Windows Hybrid Runbook Worker, kunt u twee methoden. Gebruik een Automation-runbook is de aanbevolen methode voor het volledig automatiseren van het configureren van een Windows-computer. De tweede methode is een stapsgewijze procedure voor het handmatig installeren en configureren van de rol te volgen.

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden voor de Hybrid Runbook Worker-functie met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten.

De minimale vereisten voor een Windows Hybrid Runbook Worker zijn:

* WindowsServer 2012 of hoger.
* Windows PowerShell 5.1 of hoger ([downloaden van WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)).
* .NET framework 4.6.2 of hoger.
* Twee cores.
* 4 GB aan RAM-geheugen.
* Poort 443 (uitgaand).

Als u meer netwerkvereisten voor de Hybrid Runbook Worker, Zie [configureren van uw netwerk](automation-hybrid-runbook-worker.md#network-planning).

Zie voor meer informatie over de servers voorbereiden voor beheer met DSC, [Onboarding van machines voor beheer met Azure Automation DSC](automation-dsc-onboarding.md).
Als u inschakelt de [oplossing Update Management](../operations-management-suite/oms-solution-update-management.md), een Windows-computer die verbonden met uw Azure Log Analytics-werkruimte wordt automatisch geconfigureerd als Hybrid Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing. Echter niet is geregistreerd bij Hybrid Worker-groepen al gedefinieerd in uw Automation-account. 

De computer kan worden toegevoegd aan een Hybrid Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor zowel de oplossing en het lidmaatschap van de Hybrid Runbook Worker gebruikt. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.

Nadat u een runbook worker implementeren, controleren [runbooks uitvoeren op een Hybrid Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks om processen in uw on-premises datacenter of andere cloudomgeving te automatiseren.

### <a name="automated-deployment"></a>Geautomatiseerde implementatie

Voer de volgende stappen uit voor het automatiseren van de installatie en configuratie van de Windows Hybrid Worker-rol:

1. Download het script New-OnPremiseHybridWorker.ps1 uit de [PowerShell Gallery](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) rechtstreeks vanaf de computer waarop de Hybrid Runbook Worker-rol of vanaf een andere computer in uw omgeving. Kopieer het script naar de werknemer.

   Het script New-OnPremiseHybridWorker.ps1 moet de volgende parameters tijdens de uitvoering:

   * *AutomationAccountName* (verplicht): De naam van uw Automation-account.
   * *AAResourceGroupName* (verplicht): De naam van de resourcegroep die is gekoppeld aan uw Automation-account.
   * *OMSResourceGroupName* (optioneel): De naam van de resourcegroep voor de Log Analytics-werkruimte. Als deze resourcegroep niet opgegeven is, *AAResourceGroupName* wordt gebruikt.
   * *HybridGroupName* (verplicht): De naam van een Hybrid Runbook Worker-groep die u opgeeft als een doel voor de runbooks die ondersteuning bieden voor dit scenario.
   * *Abonnements-id* (verplicht): De Azure-abonnement-ID die uw Automation-account.
   * *WorkspaceName* (optioneel): De naam van de Log Analytics-werkruimte. Als u een Log Analytics-werkruimte hebt, wordt het script maakt en configureert een.

   > [!NOTE]
   > Bij het inschakelen van oplossingen worden slechts bepaalde regio's ondersteund voor het koppelen van een Log Analytics-werkruimte aan een Automation-Account.
   >
   > Zie voor een lijst van de ondersteunde toewijzingsparen, [regiotoewijzing voor Automation-Account en de Log Analytics-werkruimte](how-to/region-mappings.md).

2. Open op uw computer **Windows PowerShell** uit de **Start** scherm in de beheerdersmodus.
3. Blader naar de map waarin het script dat u hebt gedownload vanaf de opdrachtregel-shell PowerShell. Wijzig de waarden voor de parameters *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, en *- WorkspaceName*. Voer het script.

     > [!NOTE]
     > U wordt gevraagd om te verifiëren met Azure, nadat u het script hebt uitgevoerd. U *moet* Meld u aan met een account dat lid is van de rol Abonnementsbeheerders en CO-beheerder van het abonnement.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

4. U wordt gevraagd om akkoord gaan met NuGet installeren en u wordt gevraagd om te verifiëren met uw Azure-referenties.

5. Nadat het script is voltooid, de **Hybrid Worker-groepen** pagina ziet u de nieuwe groep en het aantal leden. Als het een bestaande groep, wordt het aantal leden wordt verhoogd. U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** pagina en selecteer de **Hybrid Workers** tegel. Op de **Hybrid Workers** pagina, ziet u elk lid van de groep die wordt vermeld.

### <a name="manual-deployment"></a>Handmatige implementatie

De eerste twee stappen eenmaal uitvoeren voor uw Automation-omgeving en herhaal vervolgens de resterende stappen voor elke worker-computer.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

#### <a name="1-create-a-log-analytics-workspace"></a>1. Een Log Analytics-werkruimte maken

Als u nog een Log Analytics-werkruimte hebt, maakt u een met behulp van de instructies op de [beheren van uw werkruimte](../azure-monitor/platform/manage-access.md). Als u nog niet hebt, kunt u een bestaande werkruimte gebruiken.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. De oplossing Automation toevoegen aan de Log Analytics-werkruimte

De oplossing Automation Azure Monitor-Logboeken wordt functionaliteit toegevoegd voor Azure Automation, inclusief ondersteuning voor Hybrid Runbook Worker. Wanneer u de oplossing aan uw werkruimte toevoegt, worden automatisch worker-onderdelen op de agentcomputer die u in de volgende stap installeren wilt pushes.

Om toe te voegen de **Automation** Azure Monitor oplossing aan uw werkruimte, voer de volgende PowerShell-Logboeken.

```powershell-interactive
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true
```

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. De Microsoft Monitoring Agent installeren

De Microsoft Monitoring Agent verbonden computers met Azure Monitor-Logboeken. Wanneer u de agent op uw on-premises computer installeren en deze verbinden met uw werkruimte, wordt automatisch de onderdelen die vereist voor Hybrid Runbook Worker zijn gedownload.

Volg de instructies op voor het installeren van de agent op de lokale computer, [verbinding maken met Windows-computers naar Logboeken van Azure Monitor](../log-analytics/log-analytics-windows-agent.md). U kunt dit proces voor meerdere computers meerdere werknemers toevoegen aan uw omgeving herhalen.

Wanneer de agent is met Azure Monitor-logboeken verbonden, wordt deze weergegeven op de **verbonden bronnen** tabblad van de met log analytics **instellingen** pagina. U kunt controleren of de agent correct de Automation-oplossing heeft gedownload wanneer er een map met de naam **AzureAutomationFiles** in C:\Program Files\Microsoft Monitoring Agent\Agent. Als u wilt controleren welke versie van de Hybrid Runbook Worker, kunt u bladeren naar C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ en noteer de \\ *versie* submap.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. De runbookomgeving installeren en verbinding maken met Azure Automation

Wanneer u een agent naar Logboeken van Azure Monitor toevoegt, wordt de oplossing voor automatisering pushes omlaag het **HybridRegistration** PowerShell-module bevat de **Add-HybridRunbookWorker** cmdlet. Met deze cmdlet kunt u de runbookomgeving op de computer installeren en registreren met Azure Automation.

Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdrachten om de module te importeren:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Voer vervolgens de **Add-HybridRunbookWorker** cmdlet met behulp van de volgende syntaxis:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Krijgt u de informatie die nodig zijn voor deze cmdlet uit de **sleutels beheren** pagina in de Azure portal. Deze pagina openen door te selecteren de **sleutels** optie uit de **instellingen** pagina in uw Automation-account.

![Pagina 'Sleutels beheren'](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** is de naam van de Hybrid Runbook Worker-groep. Als deze groep al in het Automation-account bestaat, wordt de huidige computer toegevoegd aan het. Als deze groep niet bestaat, wordt deze toegevoegd.
* **Eindpunt** is de **URL** vermelding op de **sleutels beheren** pagina.
* **Token** is de **primaire TOEGANGSSLEUTEL** vermelding op de **sleutels beheren** pagina.

Voor het ontvangen van gedetailleerde informatie over de installatie, gebruikt u de **-uitgebreide** overschakelen met **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. PowerShell-modules installeren

Runbooks kunt u elk van de activiteiten en gedefinieerd in de modules die zijn geïnstalleerd in uw omgeving Azure Automation-cmdlets. Deze modules worden niet automatisch geïmplementeerd naar on-premises computers, zodat u ze handmatig moet installeren. De uitzondering hierop is de Azure-module, dat wordt standaard geïnstalleerd en biedt toegang tot de cmdlets voor alle Azure-services en activiteiten voor Azure Automation.

Omdat het primaire doel van de functie Hybrid Runbook Worker is om lokale resources te beheren, moet u waarschijnlijk voor het installeren van de modules die ondersteuning bieden voor deze resources. Zie voor meer informatie over het installeren van Windows PowerShell-modules [Modules installeren](/powershell/developer/windows-powershell). 

Modules die zijn geïnstalleerd moeten zich op een locatie waarnaar wordt verwezen door de **PSModulePath** omgevingsvariabele zodat ze automatisch van de hybrid worker importeren kan. Zie voor meer informatie, [wijzigen van het installatiepad PSModulePath](/powershell/developer/windows-powershell).

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het configureren van uw runbooks om processen in uw on-premises datacenter of andere cloudomgeving te automatiseren, [runbooks uitvoeren op een Hybrid Runbook Worker](automation-hrw-run-runbooks.md).
* Zie voor instructies over het verwijderen van Hybrid Runbook Workers [verwijderen Azure Automation Hybrid Runbook Workers](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Zie voor informatie over het oplossen van de Hybrid Runbook Workers, [probleemoplossing Windows Hybrid Runbook Workers](troubleshoot/hybrid-runbook-worker.md#windows)
* Zie voor aanvullende stappen over het oplossen van problemen met updatebeheer [updatebeheer: probleemoplossing](troubleshoot/update-management.md).
