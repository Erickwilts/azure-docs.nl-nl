---
title: VM's starten/stoppen buiten kantooruren oplossing
description: Deze oplossing voor het beheer van virtuele machine wordt gestart en stopt met uw virtuele machines van Azure Resource Manager volgens een planning en proactief bewaakt vanuit Azure Monitor-Logboeken.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2269eac0790e61dbf0ce893bbb737cb22d58d497
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002481"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>VM's starten/stoppen buiten kantooruren oplossing in Azure Automation

De VM's starten/stoppen buiten kantooruren oplossing wordt gestart en gestopt van uw Azure virtual machines op de gebruiker gedefinieerde schema's, biedt inzichten via Azure Monitor-logboeken en optioneel e-mailberichten worden verzonden via [actiegroepen](../azure-monitor/platform/action-groups.md). Deze biedt ondersteuning voor zowel Azure Resource Manager en klassieke virtuele machines voor de meeste scenario's.

> [!NOTE]
> De virtuele machines starten/stoppen buiten kantooruren oplossing is getest met de Azure-modules die worden geïmporteerd in uw Automation-Account wanneer u de oplossing implementeert. De oplossing werkt niet op dit moment met een nieuwere versie van de Azure-module. Dit is alleen van invloed op de Automation-Account dat u gebruikt om uit te voeren van de VM's starten/stoppen buiten kantooruren oplossing. U kunt nog steeds nieuwere versies van de Azure-module gebruiken in uw andere Automation-Accounts, zoals beschreven in [het bijwerken van Azure PowerShell-modules in Azure Automation](automation-update-azure-modules.md)

Deze oplossing biedt een automatiseringsoptie gedecentraliseerde van de lage kosten voor gebruikers die hun VM-kosten optimaliseren. Met deze oplossing kunt u het volgende doen:

- Plannen van virtuele machines starten en stoppen.
- Plannen van virtuele machines starten en stoppen in oplopende volgorde met behulp van Azure-Tags (niet ondersteund voor klassieke VM's).
- AUTOSTOP VM's op basis van lage CPU-gebruik.

De volgende zijn beperkingen aan de huidige oplossing:

- Deze oplossing VM's in andere regio's worden beheerd, maar kan alleen worden gebruikt in hetzelfde abonnement als uw Azure Automation-account.
- Deze oplossing is beschikbaar in Azure en AzureGov in elke regio die ondersteuning biedt voor een Log Analytics-werkruimte, een Azure Automation-account en waarschuwingen. AzureGov regio's bieden op dit moment geen ondersteuning van e-mailfunctionaliteit.

> [!NOTE]
> Als u van de oplossing voor klassieke virtuele machines gebruikmaakt, worden alle virtuele machines verwerkt sequentieel worden verwerkt per cloudservice. Virtuele machines worden in verschillende cloudservices nog steeds parallel verwerkt.
>
> Abonnementen voor Azure Cloud Solution Provider (Azure CSP) ondersteunen alleen de Azure Resource Manager-model, niet - Azure Resource Manager-services zijn niet beschikbaar in het programma. Als de oplossing starten/stoppen wordt uitgevoerd krijgt u mogelijk fouten omdat u cmdlets voor het beheren van klassieke resources. Zie voor meer informatie over de CSP, [beschikbare services in CSP-abonnementen](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services#comments). Als u een CSP-abonnement, moet u wijzigt de [ **External_EnableClassicVMs** ](#variables) variabele **False** na de implementatie.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

De runbooks voor deze oplossing werkt met een [uitvoeren als-account](automation-create-runas-account.md). Uitvoeren als-account is de aanbevolen verificatiemethode omdat deze verificatie via certificaten gebruikt in plaats van een wachtwoord dat mogelijk verlopen of regelmatig wordt gewijzigd.

Het verdient aanbeveling een afzonderlijk Automation-Account gebruiken voor het starten/stoppen van VM-oplossing. Dit is omdat de versies van de Azure-module worden regelmatig bijgewerkt en de bijbehorende parameters kunnen worden gewijzigd. De oplossing starten/stoppen van VM is niet bijgewerkt op de frequentie die dezelfde zodat deze werkt niet met een nieuwere versie van de cmdlets die worden gebruikt. Het verdient aanbeveling voor het testen van module-updates in een test Automation-Account voordat u ze importeert in uw productieomgeving Automation-Account.

### <a name="permissions-needed-to-deploy"></a>Machtigingen die nodig zijn om te implementeren

Er zijn bepaalde machtigingen die een gebruiker hebben moet tot het starten/stoppen van VM's uit uur oplossing implementeren. Deze machtigingen zijn verschillend als een vooraf gemaakte Automation-Account en de Log Analytics-werkruimte gebruiken of nieuwe maken tijdens de implementatie. Als u een bijdrager voor het abonnement en een globale beheerder in uw Azure Active Directory-tenant bent, hoeft u niet om de volgende machtigingen te configureren. Als u geen deze rechten hebben of moet een aangepaste rol configureren, raadpleegt u de machtigingen die vereist zijn hieronder.

#### <a name="pre-existing-automation-account-and-log-analytics-account"></a>Bestaande Automation-Account en de Log Analytics-account

Het starten/stoppen van VM's uit uur oplossing implementeren in een Automation-Account en de Log Analytics is de gebruiker die de oplossing implementeren op de volgende machtigingen vereist de **resourcegroep**. Zie voor meer informatie over rollen, [aangepaste rollen voor Azure-resources](../role-based-access-control/custom-roles.md).

| Machtiging | Scope|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Resourcegroep |
| Microsoft.Automation/automationAccounts/variables/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/schedules/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/runbooks/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/connections/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/certificates/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/modules/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/modules/read | Resourcegroep |
| Microsoft.automation/automationAccounts/jobSchedules/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/jobs/write | Resourcegroep |
| Microsoft.Automation/automationAccounts/jobs/read | Resourcegroep |
| Microsoft.OperationsManagement/solutions/write | Resourcegroep |
| Microsoft.OperationalInsights/workspaces/* | Resourcegroep |
| Microsoft.Insights/diagnosticSettings/write | Resourcegroep |
| Microsoft.Insights/ActionGroups/WriteMicrosoft.Insights/ActionGroups/read | Resourcegroep |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroep |
| Microsoft.Resources/deployments/* | Resourcegroep |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Nieuw Automation-Account en een nieuwe Log Analytics-werkruimte

Als u wilt implementeren, het starten/stoppen van VM's buiten kantooruren moet oplossing voor een nieuwe Automation-Account en de Log Analytics-werkruimte de gebruiker die de oplossing implementeert de machtigingen die zijn gedefinieerd in de vorige sectie, evenals de volgende machtigingen:

- CO-beheerder van abonnement: dit is alleen nodig voor het maken van het klassieke uitvoeren als-Account
- Deel uitmaken van de [Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) **toepassingsontwikkelaar** rol. Zie voor meer informatie over het configureren van uitvoeren als-Accounts [machtigingen voor het configureren van uitvoeren als-accounts](manage-runas-account.md#permissions).
- Inzender voor het abonnement of de volgende machtigingen.

| Machtiging |Scope|
| --- | --- |
| Microsoft.Authorization/Operations/read | Abonnement|
| Microsoft.Authorization/permissions/read |Abonnement|
| Microsoft.Authorization/roleAssignments/read | Abonnement |
| Microsoft.Authorization/roleAssignments/write | Abonnement |
| Microsoft.Authorization/roleAssignments/delete | Abonnement |
| Microsoft.Automation/automationAccounts/connections/read | Resourcegroep |
| Microsoft.Automation/automationAccounts/certificates/read | Resourcegroep |
| Microsoft.Automation/automationAccounts/write | Resourcegroep |
| Microsoft.OperationalInsights/workspaces/write | Resourcegroep |

## <a name="deploy-the-solution"></a>De oplossing implementeren

De volgende stappen uitvoeren om de VM's starten/stoppen buiten kantooruren oplossing toevoegen aan uw Automation-account en configureer vervolgens de variabelen voor het aanpassen van de oplossing.

1. Selecteer in een Automation-Account **starten/stoppen van VM** onder **gerelateerde Resources**. Hier kunt u **meer informatie over en inschakelen van de oplossing**. Als u al een oplossing starten/stoppen van virtuele machine is geïmplementeerd, kunt u dit selecteren door te klikken op **beheer van de oplossing** en kunt vinden in de lijst.

   ![Inschakelen van automation-account](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > U kunt het ook maken vanaf elke locatie in de Azure-portal door te klikken op **een resource maken**. Typ in de Marketplace-pagina, een trefwoord zoals **Start** of **starten/stoppen**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. U kunt ook Typ trefwoorden in een of meer van de volledige naam van de oplossing en druk op Enter. Selecteer **VM's starten/stoppen buiten kantooruren** uit de lijst met zoekresultaten.

2. In de **VM's starten/stoppen buiten kantooruren** pagina voor de geselecteerde oplossing, Controleer de samenvattingsinformatie en klik vervolgens op **maken**.

   ![Azure Portal](media/automation-solution-vm-management/azure-portal-01.png)

3. De **oplossing toevoegen** pagina wordt weergegeven. U wordt gevraagd de oplossing te configureren voordat u deze in uw Automation-abonnement kunt importeren.

   ![Oplossing toevoegen voor VM beheren pagina](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. Op de **oplossing toevoegen** weergeeft, schakelt **werkruimte**. Selecteer een Log Analytics-werkruimte die gekoppeld aan hetzelfde Azure-abonnement dat het Automation-account in. Als u een werkruimte hebt, selecteert u **nieuwe werkruimte maken**. Op de **Log Analytics-werkruimte** pagina, voert u de volgende stappen uit:
   - Geef een naam voor de nieuwe **Log Analytics-werkruimte**, zoals 'ContosoLAWorkspace'.
   - Selecteer een **abonnement** om te koppelen aan door in de vervolgkeuzelijst te selecteren of de geselecteerde standaardwaarde niet geschikt is.
   - Voor **resourcegroep**, u kunt maken van een nieuwe resourcegroep of Selecteer een bestaande resourcegroep.
   - Selecteer een **locatie**. Op dit moment de enige beschikbare locaties zijn **Australië-Zuidoost**, **Canada-centraal**, **centraal-India**, **VS-Oost**, **Japan (Oost)**, **Zuidoost-Azië**, **UK-Zuid**, **West-Europa**, en **VS-West 2**.
   - Selecteer een **prijscategorie**. Kies de **Per GB (zelfstandig)** optie. Azure Monitor-logboeken heeft bijgewerkt [prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) en de Per GB-laag is de enige optie.

   > [!NOTE]
   > Bij het inschakelen van oplossingen worden slechts bepaalde regio's ondersteund voor het koppelen van een Log Analytics-werkruimte aan een Automation-Account.
   >
   > Zie voor een lijst van de ondersteunde toewijzingsparen, [regiotoewijzing voor Automation-Account en de Log Analytics-werkruimte](how-to/region-mappings.md).

5. Na het opgeven van de vereiste gegevens op de **Log Analytics-werkruimte** pagina, klikt u op **maken**. U kunt de voortgang bijhouden onder **meldingen** in het menu dat gaat u terug naar de **oplossing toevoegen** pagina wanneer u klaar bent.
6. Op de **oplossing toevoegen** weergeeft, schakelt **Automation-account**. Als u een nieuwe Log Analytics-werkruimte maakt, kunt u een nieuw Automation-account worden gekoppeld aan het maken of Selecteer een bestaand Automation-Account die nog niet is gekoppeld aan een Log Analytics-werkruimte. Selecteer een bestaand Automation-Account of klik op **maken van een Automation-account**, en klik op de **Automation-account toevoegen** pagina, geef de volgende informatie:
   - Voer in het veld **Naam** de naam van het Automation-account in.

     Alle andere opties worden automatisch ingevuld op basis van de geselecteerde Log Analytics-werkruimte. Deze opties worden niet gewijzigd. Een Uitvoeren als-account voor Azure is de standaardmethode voor verificatie voor de runbooks die zijn opgenomen in deze oplossing. Nadat u op **OK**, worden de configuratieopties gevalideerd en het Automation-account wordt gemaakt. U kunt de voortgang bijhouden onder **Meldingen** in het menu.

7. Ten slotte op de **oplossing toevoegen** weergeeft, schakelt **configuratie**. De **Parameters** pagina wordt weergegeven.

   ![Pagina van de parameters voor oplossing](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Hier kunt u wordt gevraagd naar:
   - Geef de **ResourceGroup namen als doel**. Deze waarden zijn de namen van resourcegroepen met virtuele machines worden beheerd door deze oplossing. U kunt meer dan één naam invoeren en scheiden met een komma (waarden zijn niet hoofdlettergevoelig). Jokertekens worden ondersteund als de bewerking moet worden gericht op VM's in alle resourcegroepen in het abonnement. Deze waarde wordt opgeslagen in de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** variabelen.
   - Geef de **uitsluitingslijst VM (tekenreeks)**. Deze waarde is de naam van een of meer virtuele machines van de doelresourcegroep. U kunt meer dan één naam invoeren en scheiden met een komma (waarden zijn niet hoofdlettergevoelig). Gebruik een jokerteken wordt ondersteund. Deze waarde wordt opgeslagen in de **External_ExcludeVMNames** variabele.
   - Selecteer een **planning**. Deze waarde is een periodieke datum en tijd voor het starten en stoppen van de virtuele machines in de doel-resourcegroepen. Het schema is standaard geconfigureerd voor 30 minuten vanaf nu. Selecteren van een andere regio is niet beschikbaar. Zie voor meer informatie over het configureren van de planning voor de tijdzone van uw specifieke na het configureren van de oplossing [wijzigen van de planning voor opstarten en afsluiten](#modify-the-startup-and-shutdown-schedules).
   - Voor het ontvangen van **e-mailmeldingen** uit een actiegroep, accepteer de standaardwaarde van **Ja** en geef een geldig e-mailadres. Als u selecteert **Nee** , maar besluit dat op een later tijdstip dat u wilt ontvangen van e-mailmeldingen, u kunt bijwerken de [actiegroep](../azure-monitor/platform/action-groups.md) die is gemaakt met geldige e-mailadressen gescheiden door een komma. U moet ook de volgende regels voor waarschuwingen inschakelen:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > De standaardwaarde voor **ResourceGroup doelnamen** is een **&ast;**. Dit is bedoeld voor alle virtuele machines in een abonnement. Als u niet wilt dat de oplossing is gericht op alle virtuele machines in uw abonnement wordt deze waarde moet worden bijgewerkt naar een lijst met namen van resourcegroepen voordat u de schema's inschakelt.

8. Nadat u de oorspronkelijke instellingen vereist voor de oplossing hebt geconfigureerd, klikt u op **OK** sluiten de **Parameters** pagina en selecteer **maken**. Nadat u alle instellingen worden gevalideerd, wordt de oplossing wordt geïmplementeerd op uw abonnement. Dit proces duurt enkele seconden om te voltooien en u kunt de voortgang bijhouden onder **meldingen** in het menu.

> [!NOTE]
> Als u een Azure Cloud Solution Provider (Azure CSP)-abonnement hebt na de implementatie is voltooid in uw Automation-Account, gaat u naar **variabelen** onder **gedeelde bronnen** en stel de [ **External_EnableClassicVMs** ](#variables) variabele **False**. Hierdoor wordt voorkomen dat de oplossing van het zoeken naar klassieke VM-resources.

## <a name="scenarios"></a>Scenario's

De oplossing bevat drie verschillende scenario's. Deze scenario's zijn:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Scenario 1: VM's starten/stoppen volgens een schema

Dit scenario is de standaardconfiguratie wanneer u eerst de oplossing implementeert. U kunt bijvoorbeeld configureren om te stoppen alle virtuele machines in een abonnement als u werk in de avonduren is gepland, en ze te starten in de ochtend wanneer u in het bent. Wanneer u de schema's configureren **geplande StartVM** en **geplande StopVM** tijdens de implementatie van deze starten en stoppen van gerichte VM's. Configureren van deze oplossing als u wilt stoppen alleen virtuele machines wordt ondersteund, Zie [wijzigen van de planning voor opstarten en afsluiten](#modify-the-startup-and-shutdown-schedules) voor informatie over het configureren van een aangepaste planning.

> [!NOTE]
> De tijdzone is uw huidige tijdzone bij het configureren van de parameter van de tijd plannen. Het is echter opgeslagen in UTC-notatie in Azure Automation. U hoeft niet te doen een tijdzoneconversie als dit wordt afgehandeld tijdens de implementatie.

U bepalen welke VM's binnen het bereik zijn door het configureren van de volgende variabelen: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, and **External_ExcludeVMNames**.

U kunt inschakelen die gericht is op de actie op basis van een abonnement en resourcegroep of die zijn gericht op een specifieke lijst met virtuele machines, maar niet beide.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Doel van de acties starten en stoppen op basis van een groep en de resourcegroep

1. Configureer de **External_Stop_ResourceGroupNames** en **External_ExcludeVMNames** variabelen om op te geven van de doel-VM's.
2. Inschakelen en werk de **geplande StartVM** en **geplande StopVM** schema's.
3. Voer de **ScheduledStartStop_Parent** runbook met de parameter ACTION is ingesteld op **start** en whatIf ingesteld op **waar** om uw wijzigingen te bekijken.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Doel van de actie starten en stoppen van VM-lijst

1. Voer de **ScheduledStartStop_Parent** runbook met de parameter ACTION is ingesteld op **start**, toevoegen van een door komma's gescheiden lijst met virtuele machines in de *VMList* parameter en stel vervolgens de Parameter WHATIF **waar**. Bekijk uw wijzigingen.
1. Configureer de **External_ExcludeVMNames** parameter met een door komma's gescheiden lijst met virtuele machines (VM1, VM2, VM3).
1. In dit scenario wordt niet voldoen aan de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupnames** variabelen. Voor dit scenario moet u uw eigen planning Automation maken. Zie voor meer informatie, [een runbook in Azure Automation plannen](../automation/automation-schedules.md).

> [!NOTE]
> De waarde voor **ResourceGroup doelnamen** wordt opgeslagen als de waarde voor beide **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames**. Voor meer granulatie, kunt u elk van deze variabelen om de doelgroep van verschillende resourcegroepen te wijzigen. Gebruik voor de startactie, **External_Start_ResourceGroupNames**, en voor actie bij stoppen, gebruikt u **External_Stop_ResourceGroupNames**. Virtuele machines worden automatisch toegevoegd aan het begin en stoppen van schema's.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Scenario 2: VM's starten/stoppen in de reeks met behulp van tags

In een omgeving met twee of meer onderdelen op meerdere virtuele machines ondersteunen van een gedistribueerde werkbelasting, is de volgorde waarin de onderdelen zijn gestart en gestopt ondersteunende in volgorde belangrijk. U kunt dit scenario voltooien met de volgende stappen uit:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Doel van de acties starten en stoppen op basis van een groep en de resourcegroep

1. Voeg een **klikvolgorde** en een **sequencestop** code met een positief geheel getal op virtuele machines die zijn gericht in **External_Start_ResourceGroupNames** en  **External_Stop_ResourceGroupNames** variabelen. Het starten en stoppen acties worden uitgevoerd in oplopende volgorde. Zie voor meer informatie over het taggen van een virtuele machine [taggen van een Windows-Machine in Azure](../virtual-machines/windows/tag.md) en [taggen van een virtuele Linux-Machine in Azure](../virtual-machines/linux/tag.md).
1. Wijzigen van de schema's **geordende StartVM** en **geordende StopVM** op de datum en tijd die voldoen aan uw vereisten en de planning inschakelt.
1. Voer de **SequencedStartStop_Parent** runbook met de parameter ACTION is ingesteld op **start** en whatIf ingesteld op **waar** om uw wijzigingen te bekijken.
1. Voorbeeld van de actie en breng eventueel benodigde wijzigingen voordat u implementeert op basis van de productie-VM's. Wanneer gereed, handmatig uitvoeren van het runbook met de parameter is ingesteld op **False**, of laat de Automation-planning **geordende StartVM** en **geordende StopVM** uitvoeren automatisch na de voorgeschreven planning.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Doel van de actie starten en stoppen van VM-lijst

1. Voeg een **klikvolgorde** en een **sequencestop** code met een positief geheel getal op virtuele machines die u van plan bent om toe te voegen aan de **VMList** parameter.
1. Voer de **SequencedStartStop_Parent** runbook met de parameter ACTION is ingesteld op **start**, toevoegen van een door komma's gescheiden lijst met virtuele machines in de *VMList* parameter en stel vervolgens de Parameter WHATIF **waar**. Bekijk uw wijzigingen.
1. Configureer de **External_ExcludeVMNames** parameter met een door komma's gescheiden lijst met virtuele machines (VM1, VM2, VM3).
1. In dit scenario wordt niet voldoen aan de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupnames** variabelen. Voor dit scenario moet u uw eigen planning Automation maken. Zie voor meer informatie, [een runbook in Azure Automation plannen](../automation/automation-schedules.md).
1. Voorbeeld van de actie en breng eventueel benodigde wijzigingen voordat u implementeert op basis van de productie-VM's. Wanneer gereed, handmatig uitvoeren van de controle-en-diagnostics/monitoring-actie-groupsrunbook met de parameter is ingesteld op **False**, of laat de Automation-planning **geordende StartVM** en **Geordende StopVM** automatisch na de voorgeschreven planning worden uitgevoerd.

### <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a>Scenario 3: Starten/stoppen automatisch op basis van CPU-gebruik

Deze oplossing kunt de kosten van het uitvoeren van virtuele machines in uw abonnement op Azure-VM's die niet worden gebruikt tijdens perioden van niet-piekuren, zoals na het uur, evalueren en automatisch afgesloten als processorgebruik is dan x % beheren.

De oplossing is standaard, vooraf geconfigureerd voor het evalueren van het percentage CPU-metrische gegevens om te zien of Gemiddeld gebruik 5 procent of minder. In dit scenario wordt bepaald door de volgende variabelen en kan worden gewijzigd als de standaardwaarden niet voldoen aan uw vereisten:

- External_AutoStop_MetricName
- External_AutoStop_Threshold
- External_AutoStop_TimeAggregationOperator
- External_AutoStop_TimeWindow

U kunt inschakelen die gericht is op de actie op basis van een abonnement en resourcegroep of die zijn gericht op een specifieke lijst met virtuele machines, maar niet beide.

#### <a name="target-the-stop-action-against-a-subscription-and-resource-group"></a>Doel van de actie bij stoppen op basis van een groep en de resourcegroep

1. Configureer de **External_Stop_ResourceGroupNames** en **External_ExcludeVMNames** variabelen om op te geven van de doel-VM's.
1. Inschakelen en werk de **Schedule_AutoStop_CreateAlert_Parent** planning.
1. Voer de **AutoStop_CreateAlert_Parent** runbook met de parameter ACTION is ingesteld op **start** en whatIf ingesteld op **waar** om uw wijzigingen te bekijken.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>Doel van de actie starten en stoppen van VM-lijst

1. Voer de **AutoStop_CreateAlert_Parent** runbook met de parameter ACTION is ingesteld op **start**, toevoegen van een door komma's gescheiden lijst met virtuele machines in de *VMList* parameter en stel vervolgens de Parameter WHATIF **waar**. Bekijk uw wijzigingen.
1. Configureer de **External_ExcludeVMNames** parameter met een door komma's gescheiden lijst met virtuele machines (VM1, VM2, VM3).
1. In dit scenario wordt niet voldoen aan de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupnames** variabelen. Voor dit scenario moet u uw eigen planning Automation maken. Zie voor meer informatie, [een runbook in Azure Automation plannen](../automation/automation-schedules.md).

Nu dat u een schema hebt voor het stoppen van VM's op basis van CPU-gebruik, moet u inschakelen op een van de volgende schema's te starten.

- Doel starten actie van het abonnement en resourcegroep. Zie de stappen in [Scenario 1](#scenario-1-startstop-vms-on-a-schedule) voor testen en in te schakelen **geplande StartVM** schema's.
- Doel actie per abonnement, resourcegroep en tag start. Zie de stappen in [Scenario 2](#scenario-2-startstop-vms-in-sequence-by-using-tags) voor testen en in te schakelen **geordende StartVM** schema's.

## <a name="solution-components"></a>Oplossingsonderdelen

Deze oplossing bevat vooraf geconfigureerde runbooks, schema's en integratie met Azure Monitor-Logboeken, zodat u kunt het opstarten en afsluiten van uw virtuele machines op basis van uw zakelijke behoeften aanpassen.

### <a name="runbooks"></a>Runbooks

De volgende tabel bevat de runbooks die zijn geïmplementeerd in uw Automation-account door deze oplossing. Geen wijzigingen aanbrengen in de runbook-code. In plaats daarvan Schrijf uw eigen runbook voor de nieuwe functionaliteit.

> [!IMPORTANT]
> Voer niet rechtstreeks uit een runbook met 'onderliggende' toegevoegd aan de naam.

Alle bovenliggende runbooks bevatten de _WhatIf_ parameter. Als de waarde **waar**, _WhatIf_ ondersteunt met gedetailleerde informatie over het exacte probleem moet het runbook worden uitgevoerd wanneer uitvoeren zonder de _WhatIf_ parameter en valideert de juiste virtuele machines worden het doel. Een runbook worden alleen de gedefinieerde acties uitgevoerd wanneer de _WhatIf_ parameter is ingesteld op **False**.

|Runbook | Parameters | Description|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Met de naam van het bovenliggende runbook. Dit runbook maakt waarschuwingen op basis van per-resource voor het scenario AutoStop.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: De waarde True of False  | Hiermee of Azure waarschuwingsregels op virtuele machines in de doelgroepen voor abonnement of resourcegroep bijgewerkt. <br> VMList: Door komma's gescheiden lijst met virtuele machines. Bijvoorbeeld, _vm1, vm2, vm3_.<br> *WhatIf* valideert de runbooklogica zonder uit te voeren.|
|AutoStop_Disable | geen | Hiermee schakelt u AutoStop waarschuwingen en standaardschema.|
|AutoStop_StopVM_Child | WebHookData | Met de naam van het bovenliggende runbook. Waarschuwingsregels aanroepen met dit runbook als u wilt stoppen van de virtuele machine.|
|Bootstrap_Main | geen | Één keer gebruikt voor het instellen van de bootstrap-configuraties, zoals webhookURI, die doorgaans niet toegankelijk vanuit Azure Resource Manager. Dit runbook wordt automatisch verwijderd na de implementatie is voltooid.|
|ScheduledStartStop_Child | VM-naam <br> Actie: Starten of stoppen <br> ResourceGroupName | Met de naam van het bovenliggende runbook. Een actie starten of stoppen voor de geplande stoppen wordt uitgevoerd.|
|ScheduledStartStop_Parent | Actie: Starten of stoppen <br>VMList <br> WhatIf: De waarde True of False | Deze instelling is van invloed op alle virtuele machines in het abonnement. Bewerk de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** gericht alleen uitvoeren op deze resourcegroepen. U kunt ook specifieke virtuele machines uitsluiten door het bijwerken van de **External_ExcludeVMNames** variabele.<br> VMList: Door komma's gescheiden lijst met virtuele machines. Bijvoorbeeld, _vm1, vm2, vm3_.<br> _WhatIf_ valideert de runbooklogica zonder uit te voeren.|
|SequencedStartStop_Parent | Actie: Starten of stoppen <br> WhatIf: De waarde True of False<br>VMList| Met de naam-tags maken **klikvolgorde** en **sequencestop** op elke virtuele machine waarvoor u te sequence starten/stoppen-activiteit wenst. De namen van deze tags zijn hoofdlettergevoelig. De waarde van de tag moet een positief geheel getal zijn (1, 2, 3) die overeenkomt met de volgorde waarin u wilt starten of stoppen. <br> VMList: Door komma's gescheiden lijst met virtuele machines. Bijvoorbeeld, _vm1, vm2, vm3_. <br> _WhatIf_ valideert de runbooklogica zonder uit te voeren. <br> **Opmerking**: VMs must be within resource groups defined as External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames, and External_ExcludeVMNames in Azure Automation variables. Ze beschikken over de juiste tags voor acties die moeten worden van kracht.|

### <a name="variables"></a>Variabelen

De volgende tabel bevat de variabelen die in uw Automation-account gemaakt. Alleen Wijzig variabelen die worden voorafgegaan door **externe**. Variabelen wijzigen voorafgegaan door **intern** zorgt ervoor dat ongewenste effecten.

|Variabele | Description|
|---------|------------|
|External_AutoStop_Condition | De conditionele operator vereist voor het configureren van de voorwaarde voordat een waarschuwing wordt geactiveerd. Acceptabele waarden zijn **groter dan**, **GreaterThanOrEqual**, **LessThan**, en **LessThanOrEqual**.|
|External_AutoStop_Description | De waarschuwing op de virtuele machine stoppen als het CPU-percentage hoger is dan de drempelwaarde.|
|External_AutoStop_MetricName | De naam van de metrische gegevens voor prestaties waarvoor de Azure-waarschuwingsregel is om te worden geconfigureerd.|
|External_AutoStop_Threshold | De drempelwaarde voor de Azure waarschuwingsregel opgegeven in de variabele _External_AutoStop_MetricName_. Percentagewaarden kunnen variëren van 1 tot 100.|
|External_AutoStop_TimeAggregationOperator | De tijd aggregatieoperator, die wordt toegepast op de grootte van het geselecteerde venster om de voorwaarde te evalueren. Acceptabele waarden zijn **gemiddelde**, **Minimum**, **maximale**, **totale**, en **laatste**.|
|External_AutoStop_TimeWindow | De grootte van het venster waarin Azure geselecteerde metrische gegevens analyseert voor het activeren van een waarschuwing. Deze parameter accepteert invoer in timespan-indeling. Mogelijke waarden zijn van 5 minuten tot zes uur.|
|External_EnableClassicVMs| Hiermee geeft u op of de klassieke virtuele machines door de oplossing zijn gericht. De standaardwaarde is True. Dit moet worden ingesteld op False voor CSP-abonnementen.|
|External_ExcludeVMNames | Geef de namen van de virtuele machine moeten worden uitgesloten, namen scheiden met behulp van een door komma's zonder spaties. Dit is beperkt tot 140 VM's. Als u meer dan 140 VM's aan deze lijst met door komma's gescheiden toevoegt, virtuele machines die zijn ingesteld om te worden uitgesloten mogelijk per ongeluk gestart of gestopt.|
|External_Start_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen, waarden scheiden met behulp van een door komma's, gericht voor begin acties.|
|External_Stop_ResourceGroupNames | Hiermee geeft u een of meer resourcegroepen, waarden scheiden met behulp van een door komma's, gericht voor stop-acties.|
|Internal_AutomationAccountName | Hiermee geeft u de naam van het Automation-account.|
|Internal_AutoSnooze_WebhookUri | Hiermee geeft u de dat webhook URI met de naam voor het scenario AutoStop.|
|Internal_AzureSubscriptionId | Hiermee geeft u de Azure-abonnement-ID.|
|Internal_ResourceGroupName | Hiermee geeft u de Resourcegroepnaam van Automation-account.|

In alle scenario's, de **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**, en **External_ExcludeVMNames** variabelen die nodig zijn die zijn gericht op virtuele machines, met uitzondering van het leveren van een door komma's gescheiden lijst met virtuele machines voor de **AutoStop_CreateAlert_Parent**, **SequencedStartStop_Parent**, en  **ScheduledStartStop_Parent** runbooks. Dat wil zeggen, moeten uw virtuele machines zich bevinden in de resourcegroepen doel om te worden gestart en stop acties moeten plaatsvinden. De logische werkt die vergelijkbaar is met Azure policy in die u kunt richten op het abonnement of resourcegroep groep en er acties overgenomen door de nieuwe virtuele machines. Deze methode zo voorkomt u dat voor het onderhouden van een aparte schema voor elke virtuele machine en het beheren van wordt gestart en gestopt in de schaalset.

### <a name="schedules"></a>Planningen

De volgende tabel geeft een lijst van elk van de standaardschema's in uw Automation-account hebt gemaakt. U kunt deze wijzigen of maken van uw eigen aangepaste schema's. Standaard alle van de schema's zijn uitgeschakeld, behalve voor **Scheduled_StartVM** en **Scheduled_StopVM**.

U moet alle schema's is niet inschakelen omdat dit mogelijk overlappende schema-acties maken. Het is raadzaam om te bepalen welke optimalisaties die u wilt uitvoeren en aanpassen. Zie de voorbeeldscenario's in de overzichtssectie voor verdere uitleg.

|Schemanaam | Frequentie | Description|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | Om de 8 uur | Wordt uitgevoerd het AutoStop_CreateAlert_Parent runbook om de 8 uur, wat op zijn beurt voorkomt dat de waarden op basis van een virtuele machine in External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames en External_ExcludeVMNames in Azure Automation-variabelen. U kunt ook kunt u een door komma's gescheiden lijst met virtuele machines met behulp van de parameter VMList.|
|Scheduled_StopVM | Door de gebruiker gedefinieerde dagelijks | Het runbook Scheduled_Parent uitvoert met een parameter van _stoppen_ elke dag om de opgegeven tijd. Alle virtuele machines die voldoen aan de regels die zijn gedefinieerd door asset variabelen automatisch gestopt. Inschakelen van het bijbehorende schema **geplande StartVM**.|
|Scheduled_StartVM | Door de gebruiker gedefinieerde dagelijks | Het runbook Scheduled_Parent uitvoert met een parameter van _Start_ elke dag om de opgegeven tijd. Alle virtuele machines die voldoen aan de regels die zijn gedefinieerd door de betreffende variabelen wordt automatisch gestart. Inschakelen van het bijbehorende schema **geplande StopVM**.|
|Sequenced-StopVM | 1:00 uur (UTC), elke vrijdag | Het runbook Sequenced_Parent uitvoert met een parameter van _stoppen_ elke vrijdag op het opgegeven tijdstip. Sequentieel worden verwerkt (oplopend) stopt alle virtuele machines met een code van **SequenceStop** gedefinieerd door de betreffende variabelen. Zie de sectie Runbooks voor meer informatie over tagwaarden en variabelen voor de asset. Inschakelen van het bijbehorende schema **geordende StartVM**.|
|Sequenced-StartVM | 1:00 uur (UTC), elke maandag | Het runbook Sequenced_Parent uitvoert met een parameter van _Start_ elke maandag op het opgegeven tijdstip. Sequentieel worden verwerkt alle virtuele machines (aflopend) begint met een code van **klikvolgorde** gedefinieerd door de betreffende variabelen. Zie de sectie Runbooks voor meer informatie over tagwaarden en variabelen voor de asset. Inschakelen van het bijbehorende schema **geordende StopVM**.|

## <a name="azure-monitor-logs-records"></a>Azure Monitor-logboeken records

Automation worden twee typen records gemaakt in de Log Analytics-werkruimte: taak-logboeken en stromen van de taak.

### <a name="job-logs"></a>Taaklogboeken

|Eigenschap | Description|
|----------|----------|
|Caller |  Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
|Category | Classificatie van het type gegevens. Voor Automation is de waarde JobLogs.|
|CorrelationId | De GUID die de correlatie-ID van de runbooktaak is.|
|JobId | De GUID die de ID van de runbooktaak is.|
|operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak.|
|resourceId | Hiermee wordt het resourcetype in Azure opgegeven. Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
|ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
|ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren. Voor Automation is de waarde Azure Automation.|
|ResourceType | Hiermee wordt het resourcetype in Azure opgegeven. Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
|resultType | De status van de runbooktaak. Mogelijke waarden zijn:<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>- Geslaagd|
|resultDescription | Hiermee wordt resultaatstatus van de runbooktaak beschreven. Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid|
|RunbookName | Hiermee wordt de naam van het runbook opgegeven.|
|SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven. Voor Automation is de waarde OpsManager|
|StreamType | Hiermee wordt het type gebeurtenis opgegeven. Mogelijke waarden zijn:<br>- Uitgebreid<br>- Uitvoer<br>- Fout<br>- Waarschuwing|
|SubscriptionId | Hiermee wordt de abonnements-id van de taak opgegeven.
|Time | Datum en tijd van uitvoering van de runbooktaak.|

### <a name="job-streams"></a>Taakstromen

|Eigenschap | Description|
|----------|----------|
|Caller |  Wie de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken.|
|Category | Classificatie van het type gegevens. Voor Automation is de waarde JobStreams.|
|JobId | De GUID die de ID van de runbooktaak is.|
|operationName | Hiermee wordt het type bewerking opgegeven dat in Azure wordt uitgevoerd. Voor Automation is is de waarde van taak.|
|ResourceGroup | Hiermee wordt de resourcegroepnaam van de runbooktaak opgegeven.|
|resourceId | Hiermee geeft u de resource-ID in Azure. Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
|ResourceProvider | Hiermee wordt de Azure-service opgegeven waarmee de resources worden geleverd die u kunt implementeren en beheren. Voor Automation is de waarde Azure Automation.|
|ResourceType | Hiermee wordt het resourcetype in Azure opgegeven. Voor Automation is de waarde het Automation-account dat is gekoppeld aan het runbook.|
|resultType | Het resultaat van de runbooktaak op het moment dat de gebeurtenis werd gegenereerd. Er is een mogelijke waarde:<br>- Wordt uitgevoerd|
|resultDescription | Bevat de uitvoerstroom van het runbook.|
|RunbookName | De naam van het runbook.|
|SourceSystem | Hiermee wordt het bronsysteem voor de verzonden gegevens opgegeven. Voor Automation is de waarde OpsManager.|
|StreamType | Het type taakstroom. Mogelijke waarden zijn:<br>-Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid|
|Time | Datum en tijd van uitvoering van de runbooktaak.|

Wanneer u een zoekopdracht in Logboeken waarmee categorierecords met uitvoert **JobLogs** of **JobStreams**, kunt u de **JobLogs** of **JobStreams**weergave die een set tegels samenvatting van de updates die zijn geretourneerd door de zoekopdracht weergeeft.

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

De volgende tabel bevat voorbeeldzoekopdrachten in logboeken voor taakrecords die worden verzameld met deze oplossing.

|Query | Description|
|----------|----------|
|Taken zoeken voor runbook ScheduledStartStop_Parent die met succes voltooid | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Taken zoeken voor runbook SequencedStartStop_Parent die met succes voltooid | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="viewing-the-solution"></a>De oplossing bekijken

Voor toegang tot de oplossing, gaat u naar uw Automation-Account selecteren **werkruimte** onder **gerelateerde RESOURCES**. Selecteer op de pagina log analytics **oplossingen** onder **algemene**. Op de **oplossingen** pagina, selecteert u de oplossing **Start-Stop-VM [workspace]** in de lijst.

Als u de oplossing wordt weergegeven de **Start-Stop-VM [workspace]** oplossingenpagina. Hier kunt u belangrijke details zoals controleren de **StartStopVM** tegel. Zoals in uw Log Analytics-werkruimte, worden deze tegel toont een telling en een grafische weergave van de runbooktaken voor de oplossing die zijn gestart en hebt voltooid.

![De pagina van de oplossing Update Management Automation](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Hier kunt kunt u verder analyse uitvoeren van de Taakrecords door te klikken op de tegel ring. Dashboard van de oplossing toont Taakgeschiedenis en vooraf gedefinieerde log zoekquery's. Overschakelen naar de geavanceerde log analytics-portal om te zoeken, is afhankelijk van uw zoekquery's.

## <a name="configure-email-notifications"></a>E-mailmeldingen configureren

Voor het e-mailmeldingen wordt gewijzigd nadat de oplossing is geïmplementeerd, actiegroep die is gemaakt tijdens de implementatie te wijzigen.  

> [!NOTE]
> Abonnementen in de Azure Government-Cloud bieden geen ondersteuning voor de e-mailfunctionaliteit van deze oplossing.

In de Azure-portal, gaat u naar Monitor actiegroepen ->. Selecteer de actiegroep met de titel **StartStop_VM_Notication**.

![De pagina van de oplossing Update Management Automation](media/automation-solution-vm-management/azure-monitor.png)

Op de **StartStop_VM_Notification** pagina, klikt u op **details bewerken** onder **Details**. Hiermee opent u de **e-mailadres/SMS/Push/stem** pagina. Het e-mailadres bijwerken en klik op **OK** uw wijzigingen op te slaan.

![De pagina van de oplossing Update Management Automation](media/automation-solution-vm-management/change-email.png)

U kunt ook toevoegen aanvullende acties aan de actiegroep, voor meer informatie over actiegroepen, Zie [actiegroepen](../azure-monitor/platform/action-groups.md)

Hier volgt een voorbeeld van de e-mailbericht wordt verzonden wanneer de oplossing wordt afgesloten virtuele machines.

![De pagina van de oplossing Update Management Automation](media/automation-solution-vm-management/email.png)

## <a name="add-exclude-vms"></a>VM's toevoegen/uitsluiten

De oplossing biedt de mogelijkheid om toe te voegen van virtuele machines voor het doel zijn van de oplossing of specifiek machines uitsluiten van de oplossing.

### <a name="add-a-vm"></a>Een virtuele machine toevoegen

Er zijn een aantal opties die u gebruiken kunt om ervoor te zorgen dat een virtuele machine wordt opgenomen in de oplossing starten/stoppen wanneer deze wordt uitgevoerd.

* Elk van de bovenliggende [runbooks](#runbooks) van de oplossing hebt u een **VMList** parameter. U kunt een door komma's gescheiden lijst met namen van de virtuele machine voor deze parameter doorgeven bij het plannen van de juiste bovenliggend runbook voor uw situatie en deze VM's worden opgenomen wanneer de oplossing wordt uitgevoerd.

* Om te selecteren van meerdere virtuele machines, stel de **External_Start_ResourceGroupNames** en **External_Stop_ResourceGroupNames** met namen voor de resource met de virtuele machines die u wilt starten of stoppen. U kunt deze waarde ook instellen op `*`, hebben de oplossing uitvoeren op alle resourcegroepen in het abonnement.

### <a name="exclude-a-vm"></a>Uitsluiten van een virtuele machine

Als u wilt uitsluiten van een virtuele machine van de oplossing, u kunt deze toevoegen aan de **External_ExcludeVMNames** variabele. Deze variabele is een door komma's gescheiden lijst van specifieke virtuele machines moeten worden uitgesloten van de oplossing starten/stoppen. Deze lijst is beperkt tot 140 VM's. Als u meer dan 140 VM's aan deze lijst met door komma's gescheiden toevoegt, virtuele machines die zijn ingesteld om te worden uitgesloten mogelijk per ongeluk gestart of gestopt.

## <a name="modify-the-startup-and-shutdown-schedules"></a>De planning voor opstarten en afsluiten wijzigen

Beheren van de planning voor opstarten en afsluiten in deze oplossing volgt u dezelfde stappen zoals wordt beschreven in [een runbook in Azure Automation plannen](automation-schedules.md). Er moet een afzonderlijke schema te starten en stoppen van VM's.

Configureren van de oplossing als u wilt stoppen alleen virtuele machines op een bepaalde periode wordt ondersteund. In dit scenario maakt u alleen een **stoppen** plannen en er geen bijbehorende **Start** geplande. Hiervoor doet u het volgende:

1. Zorg ervoor dat u hebt toegevoegd de resourcegroepen voor de virtuele machines af te sluiten in de **External_Stop_ResourceGroupNames** variabele.
2. Maak uw eigen planning voor de tijd die u wilt afsluiten van de virtuele machines.
3. Navigeer naar de **ScheduledStartStop_Parent** runbook en klik op **planning**. Hiermee kunt u om te selecteren van de planning die u in de vorige stap hebt gemaakt.
4. Selecteer **Parameters en uitvoerinstellingen** en stel de parameter actie voor 'Stop'.
5. Klik op **OK** om uw wijzigingen op te slaan.

## <a name="update-the-solution"></a>De oplossing bijwerken

Als u een eerdere versie van deze oplossing hebt geïmplementeerd, moet u eerst verwijderen het uit uw account voordat u implementeert een bijgewerkte versie. Volg de stappen voor het [verwijderen van de oplossing](#remove-the-solution) en volg de stappen hierboven om te [implementeren van de oplossing](#deploy-the-solution).

## <a name="remove-the-solution"></a>De oplossing verwijderen

Als u besluit dat u niet meer nodig hebt om de oplossing te gebruiken, kunt u het verwijderen van het Automation-account. Als u de oplossing verwijdert, verwijdert alleen de runbooks. De planningen of variabelen die zijn gemaakt toen de oplossing is toegevoegd wordt niet verwijderd. Deze assets u handmatig verwijderen moet als u deze niet worden gebruikt met andere runbooks.

Als u wilt verwijderen van de oplossing, moet u de volgende stappen uitvoeren:

1. In uw Automation-account onder **gerelateerde resources**, selecteer **gekoppelde werkruimte**.
1. Selecteer **gaat u naar werkruimte**.
1. Onder **algemene**, selecteer **oplossingen**. 
1. Op de **oplossingen** pagina, selecteert u de oplossing **Start-Stop-VM [Workspace]**. Op de **VMManagementSolution [Workspace]** pagina, in het menu Selecteer **verwijderen**.<br><br> ![VM-Mgmt-oplossing verwijderen](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. In de **oplossing verwijderen** venster bevestigen dat u wilt verwijderen van de oplossing.
1. Hoewel de informatie wordt gecontroleerd en de oplossing wordt verwijderd, u kunt de voortgang bijhouden onder **meldingen** in het menu. U keert terug naar de **oplossingen** pagina nadat het proces voor het verwijderen van de oplossing is gestart.

Het Automation-account en de Log Analytics-werkruimte worden niet verwijderd als onderdeel van dit proces. Als u niet behouden van de Log Analytics-werkruimte wilt, moet u deze handmatig te verwijderen. Dit kan worden bereikt vanaf de Azure-portal:

1. Selecteer in de Azure portal startscherm **Log Analytics-werkruimten**.
1. Op de **Log Analytics-werkruimten** pagina, selecteert u de werkruimte.
1. Selecteer **verwijderen** in het menu op de pagina van de werkruimte instellingen.

Als u niet behouden van de onderdelen van Azure Automation-account wilt, kunt u elk handmatig verwijderen. Zie voor een lijst van runbooks, variabelen en schema's die zijn gemaakt door de oplossing, de [oplossingsonderdelen](#solution-components).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van verschillende zoekquery's en bekijk de Automation-taaklogboeken met Azure Monitor-Logboeken, [zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md).
- Zie [Runbooktaken bijhouden](automation-runbook-execution.md) voor meer informatie over runbookuitvoering, het bewaken van runbooktaken en andere technische details.
- Zie voor meer informatie over Azure Monitor-logboeken en -verzameling gegevensbronnen, [verzamelen van Azure storage-gegevens in Azure Monitor-Logboeken overzicht](../azure-monitor/platform/collect-azure-metrics-logs.md).
