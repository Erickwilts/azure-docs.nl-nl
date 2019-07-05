---
title: Azure Automation Hybrid Runbook Worker
description: In dit artikel bevat informatie over het installeren en gebruiken van Hybrid Runbook Worker, dat is een functie van Azure Automation kunt u runbooks uitvoeren op virtuele machines in uw lokale datacenter of cloudprovider.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 04/05/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fceeed47ee77207e00ebfc619226ecbb5956bc3d
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478518"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Resources in uw datacentrum en de cloud automatiseren met behulp van Hybrid Runbook Worker

Runbooks in Azure Automation mogelijk geen toegang tot resources in andere clouds of in uw on-premises-omgeving omdat ze worden uitgevoerd op het Azure-cloud-platform. U kunt de functie Hybrid Runbook Worker van Azure Automation gebruiken voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van deze lokale resources. Runbooks worden opgeslagen en beheerd in Azure Automation en vervolgens geleverd aan een of meer toegewezen computers.

De volgende afbeelding ziet u deze functionaliteit:

![Overzicht van Hybrid Runbook Worker](media/automation-hybrid-runbook-worker/automation.png)

Elke Hybrid Runbook Worker is lid van een Hybrid Runbook Worker-groep die u opgeeft wanneer u de agent installeert. Een groep kan een afzonderlijke agent bevatten, maar u kunt meerdere agents installeren in een groep voor hoge beschikbaarheid.

Wanneer u een runbook op een Hybrid Runbook Worker starten, geeft u de groep die op wordt uitgevoerd. Elke werknemer in de groep pollt de Azure Automation om te zien of alle taken die beschikbaar zijn. Als een taak beschikbaar is, wordt het door de eerste worker is om op te halen van de taak duurt. De verwerkingstijd van de wachtrij van taken, is afhankelijk van de Hybrid worker-hardwareprofiel en laden. U kunt een bepaalde worker niet opgeven. Hybrid Runbook Workers delen niet veel van de limieten die Azure sandboxes. Ze geen de grenzen van schijfruimte, geheugen of netwerk-sockets. Hybrid Runbook Workers wordt alleen beperkt door de bronnen op de Hybrid Runbook Worker zelf. Bovendien Hybrid Runbook Workers niet delen de 180 minuten [evenredige deel](automation-runbook-execution.md#fair-share) tijdslimiet die Azure sandboxes uitvoeren. Zie voor meer informatie over de Servicelimieten voor Azure-sandboxes geladen en Hybrid Runbook Workers, de taak [limieten](../azure-subscription-service-limits.md#automation-limits) pagina.

## <a name="install-a-hybrid-runbook-worker"></a>Een hybride Runbook Worker installeren

Het proces voor het installeren van een Hybrid Runbook Worker is afhankelijk van het besturingssysteem. De volgende tabel bevat koppelingen naar de methoden die u voor de installatie gebruiken kunt.

Als u wilt installeren en configureren van een Windows Hybrid Runbook Worker, kunt u twee methoden. Gebruik een Automation-runbook is de aanbevolen methode voor het volledig automatiseren van het configureren van een Windows-computer. De tweede methode is een stapsgewijze procedure voor het handmatig installeren en configureren van de rol te volgen. Voor Linux-machines, moet u een Python-script voor het installeren van de agent op de machine uitvoeren.

|OS  |Implementatietypen  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[Handmatig](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden voor de Hybrid Runbook Worker-functie met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten. Voor meer informatie over onboarding voor beheer met DSC, Zie [Onboarding van machines voor beheer met Azure Automation DSC](automation-dsc-onboarding.md).
>
>Als u inschakelt de [oplossing Update Management](automation-update-management.md), elke computer die verbonden met uw Azure Log Analytics-werkruimte wordt automatisch geconfigureerd als Hybrid Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing. De computer is echter niet geregistreerd bij Hybrid Worker-groepen al gedefinieerd in uw Automation-account. De computer kan worden toegevoegd aan een Hybrid Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor zowel de oplossing en het lidmaatschap van de Hybrid Runbook Worker gebruikt. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van Hybrid Runbook Worker.

Controleer de [informatie voor het plannen van uw netwerk](#network-planning) voordat u begint met het implementeren van een Hybrid Runbook Worker. Nadat u de werknemer implementeren, controleren [runbooks uitvoeren op een Hybrid Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks om processen in uw on-premises datacenter of andere cloudomgeving te automatiseren.

De computer kan worden toegevoegd aan een Hybrid Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor zowel de oplossing en het lidmaatschap van de Hybrid Runbook Worker gebruikt. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.
## <a name="remove-a-hybrid-runbook-worker"></a>Een Hybrid Runbook Worker verwijderen

U kunt een of meer Hybrid Runbook Workers verwijderen uit een groep of u kunt de groep verwijderen, afhankelijk van uw vereisten. Als u wilt een Hybrid Runbook Worker verwijderen uit een on-premises computer, gebruikt u de volgende stappen uit:

1. In de Azure-portal, gaat u naar uw Automation-account.
2. Onder **Accountinstellingen**, selecteer **sleutels** en noteer de waarden voor **URL** en **primaire toegangssleutel**. U hebt deze informatie nodig voor de volgende stap.

### <a name="windows"></a>Windows

Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdracht uit. Gebruik de **-uitgebreide** overschakelen voor een gedetailleerd logboek van de procedure voor het verwijderen.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>
```

Als verlopen machines uit de Hybrid Worker-groep verwijderen, gebruik het optionele `machineName` parameter.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="linux"></a>Linux

U kunt de opdracht `ls /var/opt/microsoft/omsagent` op Hybrid Runbook Worker om op te halen van de werkruimte-id. Er is een map in de map waarin de naam van de map de werkruimte-id is.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Deze code wordt de Microsoft Monitoring Agent niet verwijderd van de computer, alleen de functionaliteit en de configuratie van de Hybrid Runbook Worker-rol.

## <a name="remove-a-hybrid-worker-group"></a>Een Hybrid Worker-groep verwijderen

Als u wilt een groep verwijderen, moet u eerst de Hybrid Runbook Worker verwijderen vanaf elke computer die deel uitmaakt van de groep met behulp van de procedure eerder weergegeven. Gebruik vervolgens de volgende stappen uit de groep te verwijderen:

1. Open het Automation-account in Azure portal.
2. Onder **procesautomatisering**, selecteer **Hybrid worker-groepen**. Selecteer de groep die u wilt verwijderen. De eigenschappenpagina voor de groep die wordt weergegeven.

   ![De pagina Eigenschappen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Selecteer op de eigenschappenpagina voor de geselecteerde groep **verwijderen**. Een bericht gevraagd deze actie te bevestigen. Selecteer **Ja** als u zeker dat u wilt doorgaan.

   ![Bevestigingsbericht](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Dit proces kan enkele seconden om te voltooien duren. U kunt de voortgang bijhouden onder **Meldingen** in het menu.

## <a name="network-planning"></a>Het netwerk configureren

### <a name="hybrid-worker-role"></a>Hybride werkrol

De Hybrid Runbook Worker verbinding maken met en registreren met Azure Automation, moet de toegang tot het poortnummer en de URL's die worden beschreven in deze sectie hebben. Deze toegang is boven aan de [poorten en URL's die zijn vereist voor Microsoft Monitoring Agent](../azure-monitor/platform/agent-windows.md) verbinding maken met Azure Monitor-Logboeken.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Als u een proxyserver voor communicatie tussen de agent en de Azure Automation-service gebruikt, moet u controleren of de juiste resources toegankelijk zijn. De time-out voor aanvragen van de Hybrid Runbook Worker en het Automation-services is 30 seconden. De aanvraag mislukt na 3 pogingen. Als u een firewall gebruikt voor toegang tot internet te beperken, moet u uw firewall zodanig toegang configureren. Als u de Log Analytics-gateway als een proxy gebruikt, controleert u of dat deze is geconfigureerd voor hybrid workers. Zie voor instructies over hoe u dit doet, [de Log Analytics-gateway configureren voor Automation Hybrid Workers](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway).

De volgende poort en URL's zijn vereist voor de Hybrid Runbook Worker-rol om te communiceren met Automation:

* Poort: Alleen TCP 443 is vereist voor uitgaande toegang tot internet.
* Algemene URL: *.azure-automation.net
* Algemene URL van de VS (overheid) Virginia: *.azure-automation.us
* Agent-service: https://\<workspaceId\>.agentsvc.azure-automation.net

Het verdient aanbeveling om de adressen die worden vermeld bij het definiëren van uitzonderingen te gebruiken. Voor IP-adressen die u kunt downloaden de [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Dit bestand wordt wekelijks bijgewerkt, en heeft de bereiken momenteel zijn geïmplementeerd en eventuele toekomstige wijzigingen in de IP-adresbereiken.

Als u een Automation-account dat gedefinieerd voor een specifieke regio hebt, kunt u communicatie met dat regionaal datacenter kunt beperken. De volgende tabel bevat de DNS-record voor elke regio:

| **Regio** | **DNS-record** |
| --- | --- |
| US - west-centraal | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| US - zuid-centraal |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| US - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| US - west 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |
| Canada - midden |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa -west |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Europa - noord |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Azië - zuidoost |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| India - centraal |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan - oost |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australië - oost |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
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

### <a name="update-management"></a>Updatebeheer

De volgende adressen worden boven op de standard-adressen en poorten die vereist voor de Hybrid Runbook Worker, vereist voor het beheer van updates. Communicatie met deze adressen wordt gedaan via poort 443.

|Azure Public  |Azure Government  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="next-steps"></a>Volgende stappen

* Zie voor informatie over het configureren van uw runbooks om processen in uw on-premises datacenter of andere cloudomgeving te automatiseren, [runbooks uitvoeren op een Hybrid Runbook Worker](automation-hrw-run-runbooks.md).
* Zie voor informatie over het oplossen van de Hybrid Runbook Workers, [probleemoplossing Hybrid Runbook Workers](troubleshoot/hybrid-runbook-worker.md#general)

