---
title: Oplossen van fouten onboarding updatebeheer, wijzigingen bijhouden en inventaris
description: Meer informatie over het oplossen van fouten met de updatebeheer, wijzigingen bijhouden en inventaris oplossingen onboarding
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 8867912d98897a695c1e59ebd4177301230281bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399759"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Problemen oplossen bij het onboarding-oplossingen

Fouten kunnen optreden tijdens het onboarding-oplossingen zoals updatebeheer of wijzigingen bijhouden en inventaris. In dit artikel beschrijft de verschillende fouten die optreden en hoe ze op te lossen.

## <a name="general-errors"></a>Algemene fouten

### <a name="missing-write-permissions"></a>Scenario: Onboarding is mislukt met het bericht: de oplossing kan niet worden ingeschakeld

#### <a name="issue"></a>Probleem

U ontvangen een van de volgende berichten wanneer u te onboarden een virtuele machine naar een oplossing probeert:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt door onjuiste of ontbrekende machtigingen op de virtuele machine, de werkruimte, of voor de gebruiker.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat u hebt juiste machtigingen voor de Onboarding van de virtuele machine. Controleer de [machtigingen die nodig zijn voor Onboarding van machines](../automation-role-based-access-control.md#onboarding) en probeer zorgen voor onboarding de oplossing voor het opnieuw. Als u de foutmelding `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, zorg ervoor dat u hebt de `Microsoft.OperationalInsights/workspaces/read` toestemming om te kunnen vinden als de virtuele machine toegevoegd aan een werkruimte is.

### <a name="diagnostic-logging"></a>Scenario: Onboarding is mislukt met het bericht - Automation-Account configureren voor registratie in diagnoselogboek is mislukt

#### <a name="issue"></a>Probleem

U ontvangt het volgende bericht wanneer u te onboarden een virtuele machine naar een oplossing probeert:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt als de prijscategorie komt niet overeen met het factureringsmodel van het abonnement. Zie voor meer informatie, [gebruik en geschatte kosten in Azure Monitor bewaken](http://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Oplossing

Handmatig maken van uw Log Analytics-werkruimte en herhaal het onboarding-proces om te selecteren van de werkruimte hebt gemaakt.

### <a name="computer-group-query-format-error"></a>Scenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de opgeslagen zoekopdracht computer groepsquery gebruikt om u te richten op de oplossing is niet juist opgemaakt. 

#### <a name="cause"></a>Oorzaak

U hebt de query gewijzigd, of door het systeem zijn gewijzigd.

#### <a name="resolution"></a>Oplossing

U kunt de query voor deze oplossing en de oplossing, wordt de query reonboard verwijderen. De query worden gevonden in uw werkruimte onder **opgeslagen zoekopdrachten**. De naam van de query is **MicrosoftDefaultComputerGroup**, en de categorie van de query is de naam van de oplossing die is gekoppeld aan deze query. Als meerdere oplossingen zijn ingeschakeld, de **MicrosoftDefaultComputerGroup** meerdere keren weergegeven onder **opgeslagen zoekopdrachten**.

### <a name="policy-violation"></a>Scenario: PolicyViolation

#### <a name="issue"></a>Probleem

Deze foutcode betekent dat de implementatie is mislukt vanwege schending van een of meer beleidsregels.

#### <a name="cause"></a>Oorzaak 

Een beleid is dat wordt geblokkeerd door de bewerking niet voltooien.

#### <a name="resolution"></a>Oplossing

De oplossing implementeren, moet u rekening houden met het wijzigen van het opgegeven beleid. Er zijn veel verschillende soorten beleid die kunnen worden gedefinieerd, is de specifieke wijzigingen nodig zijn afhankelijk van het beleid dat is geschonden. Bijvoorbeeld, als er een beleid is gedefinieerd in een resourcegroep die geen toestemming hebben voor het wijzigen van de inhoud van bepaalde typen resources in die resourcegroep, kunt u bijvoorbeeld doen van de volgende:

* Verwijder het beleid kan worden overgeslagen.
* Probeer te onboarden naar een andere resourcegroep.
* Wijzig het beleid door, bijvoorbeeld:
  * Opnieuw die gericht is op het beleid op een specifieke bron (bijvoorbeeld bij een specifieke Automation-account).
  * Wijzigen van de set van resources dat beleid is geconfigureerd om te weigeren.

Controleer de meldingen in de rechterbovenhoek van de Azure-portal of Ga naar de resourcegroep waarin uw automation-account en selecteer **implementaties** onder **instellingen** om de mislukte weer te geven de implementatie. Voor meer informatie over Azure Policy gaat u naar: [Overzicht van Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="unlink"></a>Scenario: Fouten opgetreden bij het ontkoppelen van een werkruimte

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het ontkoppelen van een werkruimte:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer u hebt nog steeds oplossingen active in uw Log Analytics-werkruimte die afhankelijk zijn van uw Automation-Account en Log Analytics-werkruimte wordt gekoppeld.

### <a name="resolution"></a>Oplossing

U kunt dit oplossen moet u de volgende oplossingen verwijderen uit uw werkruimte als u deze gebruikt:

* Updatebeheer
* Tracering wijzigen
* VM's starten/stoppen buiten kantooruren

Wanneer u de oplossingen verwijdert kunt u uw werkruimte ontkoppelen. Het is belangrijk om alle bestaande artefacten van deze oplossingen ook vanuit uw werkruimte en het Automation-Account op te schonen.  

* Updatebeheer
  * Update-implementaties (schema's) van uw Automation-Account verwijderen
* VM's starten/stoppen buiten kantooruren
  * Verwijderen van alle vergrendelingen op oplossingsonderdelen in uw Automation-Account onder **instellingen** > **vergrendelingen**.
  * Voor extra stappen voor het verwijderen van de VM's starten/stoppen buiten kantooruren oplossing ziet, [verwijderen van de VM starten/stoppen buiten kantooruren oplossing](../automation-solution-vm-management.md##remove-the-solution).

## <a name="mma-extension-failures"></a>MMA-extensies oplossen

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bij het implementeren van een oplossing voor een verscheidenheid aan gerelateerde resources worden geïmplementeerd. Een van deze resources is de Microsoft Monitoring Agent-extensie of Log Analytics-agent voor Linux. Dit zijn geïnstalleerd door de virtuele machine-Gastagent die verantwoordelijk is voor communicatie met de geconfigureerde Log Analytics-werkruimte, ten behoeve van hoger coördinatie van het downloaden van de binaire bestanden extensies voor virtuele machines en andere bestanden die de oplossing voor u onboarding afhankelijk van wanneer de uitvoering wordt gestart.
U doorgaans eerst op de hoogte van de MMA of Log Analytics-agent voor Linux-installatiefouten vanaf een melding wordt weergegeven in Notification hubs. Te klikken op de melding dat biedt meer informatie over de specifieke fout. Navigatie naar de resource resourcegroepen, en vervolgens naar het element implementaties binnen het bevat ook informatie over de fouten bij de implementatie die zijn opgetreden.
Installatie van de MMA of Log Analytics-agent voor Linux kan om verschillende redenen mislukken en de stappen voor het oplossen van deze fouten zijn afhankelijk van het probleem. Specifieke stappen volgt.

De volgende sectie worden verschillende problemen beschreven die u kunt tijdens het onboarding die ertoe leiden dat een fout opgetreden bij de implementatie van de MMA-extensie.

### <a name="webclient-exception"></a>Scenario: Er is een uitzondering opgetreden tijdens een WebClient-aanvraag

De MMA-extensie op de virtuele machine is kan niet communiceren met externe bronnen en de implementatie mislukt.

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten die worden geretourneerd:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Oorzaak

Er zijn enkele mogelijke oorzaken voor deze fout:

* Er is een proxy is geconfigureerd in de virtuele machine, waarmee alleen bepaalde poorten.

* Een firewallinstelling is toegang tot de vereiste poorten en -adressen geblokkeerd.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat u de juiste poorten hebt en adressen voor communicatie openen. Zie voor een lijst met poorten en -adressen, [plannen van uw netwerk](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Scenario: De installatie is mislukt vanwege een tijdelijke omgevingsproblemen

De installatie van de Microsoft Monitoring Agent-extensie is tijdens de implementatie is mislukt vanwege een andere installatie of blokkeren van de installatie van de actie

#### <a name="issue"></a>Probleem

Hier volgen enkele voorbeelden van foutberichten kunnen worden geretourneerd:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Oorzaak

Er zijn enkele mogelijke oorzaken voor deze fout:

* Een andere installatie wordt uitgevoerd
* Het systeem wordt opnieuw opstarten tijdens de sjabloonimplementatie van de geactiveerd

#### <a name="resolution"></a>Oplossing

Deze fout is een tijdelijke fout in de natuur. De implementatie voor het installeren van de extensie opnieuw probeert.

### <a name="installation-timeout"></a>Scenario: Time-out voor installatie

De installatie van de MMA-extensie is niet voltooid vanwege een time-out.

#### <a name="issue"></a>Probleem

Het volgende voorbeeld heeft een foutbericht die kan worden geretourneerd:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op omdat de virtuele machine wordt zwaar belast tijdens de installatie.

### <a name="resolution"></a>Oplossing

Poging tot de MMA-extensie installeren wanneer de virtuele machine minder belast wordt.

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
