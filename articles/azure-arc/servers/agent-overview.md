---
title: Overzicht van de verbonden computer Windows-agent
description: Dit artikel bevat een gedetailleerd overzicht van de beschik bare Azure Arc-servers agent, die ondersteuning biedt voor het bewaken van virtuele machines die worden gehost in hybride omgevingen.
ms.date: 09/30/2020
ms.topic: conceptual
ms.openlocfilehash: 248604884cf1b7592b382a3490aab60102e12faf
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979152"
---
# <a name="overview-of-azure-arc-enabled-servers-agent"></a>Overzicht van de agent voor servers met Azure Arc ingeschakeld

Met de met Azure Arc ingeschakelde servers verbonden machine-agent kunt u uw Windows-en Linux-machines beheren die buiten Azure worden gehost op uw bedrijfs netwerk of een andere Cloud provider. Dit artikel bevat een gedetailleerd overzicht van de agent-, systeem-en netwerk vereisten en de verschillende implementatie methoden.

>[!NOTE]
>Met ingang van de algemene versie van Azure Arc-servers in september 2020, worden alle voorlopige versies van de met Azure verbonden machine agent (agents met versies lager dan 1,0) **verouderd** op **2 februari 2021**.  Met deze periode kunt u een upgrade uitvoeren naar versie 1,0 of hoger voordat de vooraf vrijgegeven agents niet meer kunnen communiceren met de service Azure Arc enabled servers.

## <a name="agent-component-details"></a>Details van agent onderdeel

Het pakket met de Azure Connected machine agent bevat verschillende logische onderdelen, die samen worden gebundeld.

* Met de HIMDS (Hybrid instance meta data service) wordt de verbinding met Azure en de Azure-identiteit van de verbonden machine beheerd.

* De gast configuratie agent biedt In-Guest beleid en gast configuratie functionaliteit, zoals het controleren of de computer voldoet aan het vereiste beleid.

    Let op het volgende gedrag met Azure Policy [gast configuratie](../../governance/policy/concepts/guest-configuration.md) voor een niet-verbonden computer:

    * Een toewijzing van een gast configuratie beleid met een doel voor niet-verbonden computers wordt niet beïnvloed.
    * De gast toewijzing wordt gedurende 14 dagen lokaal opgeslagen. In de periode van 14 dagen worden beleids toewijzingen opnieuw toegepast als de verbonden machine agent opnieuw verbinding maakt met de service.
    * Toewijzingen worden na 14 dagen verwijderd en worden na de periode van 14 dagen niet opnieuw aan de computer toegewezen.

* De extensie agent beheert VM-extensies, met inbegrip van installeren, verwijderen en bijwerken. Uitbrei dingen worden gedownload van Azure en gekopieerd naar de `%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads` map in Windows, en voor Linux naar `/opt/GC_Ext/downloads` . In Windows wordt de uitbrei ding geïnstalleerd op het volgende pad `%SystemDrive%\Packages\Plugins\<extension>` en op Linux wordt de extensie geïnstalleerd `/var/lib/waagent/<extension>` .

## <a name="download-agents"></a>Agents downloaden

U kunt het Azure Connected machine agent-pakket voor Windows en Linux downloaden van de hieronder vermelde locaties.

* [Windows agent Windows Installer-pakket](https://aka.ms/AzureConnectedMachineAgent) van het micro soft Download centrum.

* Linux-agent pakket wordt gedistribueerd vanuit de [pakket opslagplaats](https://packages.microsoft.com/) van micro soft met behulp van de voorkeurs indeling van het pakket voor de distributie (. RPM of. DEB).

De Azure Connected machine-agent voor Windows en Linux kan hand matig worden bijgewerkt naar de meest recente versie of automatisch afhankelijk van uw vereisten. Klik [hier](manage-agent.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

### <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende versies van het Windows-en Linux-besturings systeem worden officieel ondersteund voor de Azure Connected machine agent:

- Windows Server 2012 R2 en hoger (inclusief Windows Server Core)
- Ubuntu 16,04 en 18,04 LTS (x64)
- CentOS Linux 7 (x64)
- SUSE Linux Enterprise Server (SLES) 15 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64)
- Amazon Linux 2 (x64)

### <a name="required-permissions"></a>Vereiste machtigingen

* Voor de onboarding van machines bent u lid van de rol **Azure Connected machine** .

* Als u een machine wilt lezen, wijzigen, onboarden en verwijderen, bent u lid van de **Azure Connected machine resource Administrator** -rol. 

### <a name="azure-subscription-and-service-limits"></a>Limieten voor het Azure-abonnement en de Azure-service

Voordat u uw computers configureert met servers voor Azure-Arc, controleert u de limieten voor het Azure Resource Manager- [abonnement](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) en de beperkingen van de [resource groep](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) om het aantal machines te plannen dat moet worden verbonden.

### <a name="transport-layer-security-12-protocol"></a>Transport Layer Security 1,2-protocol

Om te zorgen voor de beveiliging van gegevens die onderweg zijn naar Azure, raden we u aan om de machine te configureren voor het gebruik van Transport Layer Security (TLS) 1,2. Er zijn oudere versies van TLS/Secure Sockets Layer (SSL) gevonden die kwetsbaar zijn en terwijl ze nog steeds werken om achterwaartse compatibiliteit mogelijk te maken, worden ze **niet aanbevolen**.

|Platform/taal | Ondersteuning | Meer informatie |
| --- | --- | --- |
|Linux | Linux-distributies zijn vaak afhankelijk van [openssl](https://www.openssl.org) voor TLS 1,2-ondersteuning. | Controleer de [openssl wijzigingen logboek](https://www.openssl.org/news/changelog.html) om te bevestigen dat uw versie van openssl wordt ondersteund.|
| Windows Server 2012 R2 en hoger | Wordt ondersteund en is standaard ingeschakeld. | Om te bevestigen dat u nog steeds de [standaard instellingen](/windows-server/security/tls/tls-registry-settings)gebruikt.|

### <a name="networking-configuration"></a>Netwerkconfiguratie

De verbonden machine agent voor Linux en Windows communiceert veilig uitgaande naar Azure Arc via TCP-poort 443. Als de computer verbinding maakt via een firewall of proxy server om via internet te communiceren, raadpleegt u de vereisten hieronder om inzicht te krijgen in de vereisten voor de netwerk configuratie.

Als de uitgaande connectiviteit wordt beperkt door uw firewall of proxy server, controleert u of de Url's die hieronder worden weer gegeven niet zijn geblokkeerd. Als u alleen de IP-bereiken of domein namen toestaat die vereist zijn om de agent te laten communiceren met de-service, moet u ook toegang tot de volgende service tags en Url's toestaan.

Service Tags:

* AzureActiveDirectory
* AzureTrafficManager
* AzureResourceManager
* AzureArcInfrastructure

Adres

| Agentresource | Beschrijving |
|---------|---------|
|`management.azure.com`|Azure Resource Manager|
|`login.windows.net`|Azure Active Directory|
|`dc.services.visualstudio.com`|Application Insights|
|`*.guestconfiguration.azure.com` |Gastconfiguratie|
|`*.his.arc.azure.com`|Hybride identiteits service|

Voor preview-agents (versie 0,11 en lager) hebt u ook toegang tot de volgende Url's nodig:

| Agentresource | Beschrijving |
|---------|---------|
|`agentserviceapi.azure-automation.net`|Gastconfiguratie|
|`*-agentservice-prod-1.azure-automation.net`|Gastconfiguratie|

Zie voor een lijst met IP-adressen voor elke servicetag/regio het JSON-bestand- [Azure IP-bereiken en de service Tags – open bare Cloud](https://www.microsoft.com/download/details.aspx?id=56519). Micro soft publiceert wekelijkse updates met elke Azure-service en de IP-bereiken die worden gebruikt. Bekijk [service Tags](../../virtual-network/security-overview.md#service-tags)voor meer informatie.

De Url's in de vorige tabel zijn vereist naast de IP-adres bereik gegevens van de service label, omdat de meeste services momenteel geen servicetag registratie hebben. Zo kunnen de IP-adressen worden gewijzigd. Als IP-adresbereiken vereist zijn voor de configuratie van de firewall, moet de **Cloud** -servicetag worden gebruikt om toegang tot alle Azure-Services toe te staan. Schakel de beveiligings controle of inspectie van deze Url's niet uit, en sta ze toe als andere Internet verkeer.

### <a name="register-azure-resource-providers"></a>Azure-resourceproviders registreren

Servers voor Azure Arc zijn afhankelijk van de volgende Azure-resourceproviders in uw abonnement om deze service te kunnen gebruiken:

* **Microsoft.HybridCompute**
* **Microsoft.GuestConfiguration**

Als ze niet zijn geregistreerd, kunt u ze registreren met de volgende opdrachten:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

Azure CLI:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

U kunt de resource providers ook registreren in de Azure Portal door de stappen onder [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal)te volgen.

## <a name="installation-and-configuration"></a>Installatie en configuratie

Het koppelen van machines in uw hybride omgeving met Azure kan worden uitgevoerd met behulp van verschillende methoden, afhankelijk van uw vereisten. In de volgende tabel ziet u elke methode om te bepalen welke functie het beste werkt voor uw organisatie.

> [!IMPORTANT]
> De verbonden machine agent kan niet worden geïnstalleerd op een virtuele machine van Azure Windows. Als u dit probeert, wordt dit door de installatie gedetecteerd en teruggezet.

| Methode | Beschrijving |
|--------|-------------|
| Interactief | Installeer de agent hand matig op een enkele of een beperkt aantal machines Volg de stappen in [computers verbinden met Azure Portal](onboard-portal.md).<br> Vanuit het Azure Portal kunt u een script genereren en uitvoeren op de computer om de installatie-en configuratie stappen van de agent te automatiseren.|
| Op schaal | Installeer en configureer de agent voor meerdere machines die de [Connect-computers volgen met behulp van een Service-Principal](onboard-service-principal.md).<br> Met deze methode maakt u een Service-Principal om machines niet-interactief te verbinden.|
| Op schaal | Installeer en configureer de agent voor meerdere machines [met behulp van Windows Power shell DSC](onboard-dsc.md).<br> Deze methode maakt gebruik van een Service-Principal om machines niet-interactief met Power shell DSC te verbinden. |

## <a name="connected-machine-agent-technical-overview"></a>Technisch overzicht van verbonden machine agent

### <a name="windows-agent-installation-details"></a>Details van de installatie van Windows agent

De verbonden machine agent voor Windows kan worden geïnstalleerd met behulp van een van de volgende drie methoden:

* Dubbel klik op het bestand `AzureConnectedMachineAgent.msi` .
* Hand matig door het Windows Installer-pakket uit te voeren `AzureConnectedMachineAgent.msi` vanuit de opdracht shell.
* Vanuit een Power shell-sessie met behulp van een script methode.

Na de installatie van de verbonden machine-agent voor Windows, worden de volgende aanvullende configuratie wijzigingen van het hele systeem toegepast.

* De volgende installatie mappen worden tijdens de installatie gemaakt.

    |Map |Beschrijving |
    |-------|------------|
    |%ProgramFiles%\AzureConnectedMachineAgent |Standaardpad met de agent ondersteunings bestanden.|
    |%ProgramData%\AzureConnectedMachineAgent |Bevat de configuratie bestanden voor de agent.|
    |%ProgramData%\AzureConnectedMachineAgent\Tokens |Bevat de verkregen tokens.|
    |%ProgramData%\AzureConnectedMachineAgent\Config |Bevat het configuratie bestand van de agent waarin de `agentconfig.json` registratie gegevens van de service worden vastgelegd.|
    |%SystemDrive%\Program Files\ArcConnectedMachineAgent\ExtensionService\GC | Installatiepad met de bestanden van de gast configuratie agent. |
    |%ProgramData%\GuestConfig |Bevat het beleid (toegepast) van Azure.|
    |%SystemDrive%\AzureConnectedMachineAgent\ExtensionService\downloads | Uitbrei dingen worden uit Azure gedownload en hier gekopieerd.|

* De volgende Windows-Services worden tijdens de installatie van de agent gemaakt op de doel machine.

    |Servicenaam |Weergavenaam |Procesnaam |Beschrijving |
    |-------------|-------------|-------------|------------|
    |himds |Azure Hybrid Instance Metadata Service |himds.exe |Deze service implementeert de Azure instance meta data service (IMDS) voor het beheren van de verbinding met Azure en de Azure-identiteit van de verbonden machine.|
    |DscService |Gast configuratie service |dsc_service.exe |De code basis die voor desired state Configuration (DSC v2) in azure wordt gebruikt om In-Guest beleid te implementeren.|

* De volgende omgevings variabelen worden tijdens de installatie van de agent gemaakt.

    |Naam |Standaardwaarde |Beschrijving |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Er zijn verschillende logboek bestanden beschikbaar voor het oplossen van problemen. Deze worden beschreven in de volgende tabel.

    |Logboek |Beschrijving |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Registreert gegevens van de agents (HIMDS) en de interactie met Azure.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Bevat de uitvoer van de azcmagent-hulp programma-opdrachten wanneer het argument uitgebreid (-v) wordt gebruikt.|
    |%ProgramData%\GuestConfig\ gc_agent_logs \ gc_agent. log |Registreert gegevens van de DSC-service activiteit,<br> met name de connectiviteit tussen de HIMDS-service en Azure Policy.|
    |% Program Data% \GuestConfig\gc_agent_logs\gc_agent_telemetry.txt |Registreert gegevens over de telemetrie van DSC-service en uitgebreide logboek registratie.|
    |%SystemDrive%\ProgramData\GuestConfig\ ext_mgr_logs|Registreert gegevens over het onderdeel van de extensie agent.|
    |%SystemDrive%\ProgramData\GuestConfig\ extension_logs\<Extension>|Registreert gegevens van de geïnstalleerde uitbrei ding.|

* De lokale beveiligings groep **hybride agent extensie toepassingen** wordt gemaakt.

* Tijdens het verwijderen van de agent worden de volgende artefacten niet verwijderd.

    * %ProgramFiles%\AzureConnectedMachineAgent\Logs
    * %ProgramData%\AzureConnectedMachineAgent en submappen
    * %ProgramData%\GuestConfig

### <a name="linux-agent-installation-details"></a>Details van installatie van Linux-agent

De verbonden machine agent voor Linux is opgenomen in de voorkeurs pakket indeling voor de distributie (. RPM of. DEB) die wordt gehost in de micro soft [package-opslag plaats](https://packages.microsoft.com/). De agent wordt geïnstalleerd en geconfigureerd met de shell-script bundel [Install_linux_azcmagent. sh](https://aka.ms/azcmagent).

Na de installatie van de verbonden machine agent voor Linux worden de volgende aanvullende configuratie wijzigingen van het hele systeem toegepast.

* De volgende installatie mappen worden tijdens de installatie gemaakt.

    |Map |Beschrijving |
    |-------|------------|
    |/var/opt/azcmagent/ |Standaardpad met de agent ondersteunings bestanden.|
    |/opt/azcmagent/ |
    |/opt/GC_Ext | Installatiepad met de bestanden van de gast configuratie agent.|
    |/opt/DSC/ |
    |/var/opt/azcmagent/tokens |Bevat de verkregen tokens.|
    |/var/lib/GuestConfig |Bevat het beleid (toegepast) van Azure.|
    |/opt/GC_Ext/downloads|Uitbrei dingen worden uit Azure gedownload en hier gekopieerd.|

* De volgende daemons worden tijdens de installatie van de agent gemaakt op de doel machine.

    |Servicenaam |Weergavenaam |Procesnaam |Beschrijving |
    |-------------|-------------|-------------|------------|
    |himdsd. service |Azure Hybrid Instance Metadata Service |/opt/azcmagent/bin/himds |Deze service implementeert de Azure instance meta data service (IMDS) voor het beheren van de verbinding met Azure en de Azure-identiteit van de verbonden machine.|
    |dscd. service |Gast configuratie service |/opt/DSC/dsc_linux_service |Dit is de configuratie code van de desired state Configuration (DSC v2) die in azure wordt gebruikt om In-Guest beleid te implementeren.|

* Er zijn verschillende logboek bestanden beschikbaar voor het oplossen van problemen. Deze worden beschreven in de volgende tabel.

    |Logboek |Beschrijving |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Registreert gegevens van de agents (HIMDS) en de interactie met Azure.|
    |/var/opt/azcmagent/log/azcmagent.log |Bevat de uitvoer van de azcmagent-hulp programma-opdrachten wanneer het argument uitgebreid (-v) wordt gebruikt.|
    |/opt/logs/dsc.log |Registreert gegevens van de DSC-service activiteit,<br> met name de connectiviteit tussen de himds-service en Azure Policy.|
    |/opt/logs/dsc.telemetry.txt |Registreert gegevens over de telemetrie van DSC-service en uitgebreide logboek registratie.|
    |/var/lib/GuestConfig/ext_mgr_logs |Registreert gegevens over het onderdeel van de extensie agent.|
    |/var/lib/GuestConfig/extension_logs|Registreert gegevens van de geïnstalleerde uitbrei ding.|

* De volgende omgevings variabelen worden tijdens de installatie van de agent gemaakt. Deze variabelen worden ingesteld in `/lib/systemd/system.conf.d/azcmagent.conf` .

    |Naam |Standaardwaarde |Beschrijving |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |http://localhost:40342/metadata/identity/oauth2/token ||
    |IMDS_ENDPOINT |http://localhost:40342 ||

* Tijdens het verwijderen van de agent worden de volgende artefacten niet verwijderd.

    * /var/opt/azcmagent
    * /opt/logs

## <a name="next-steps"></a>Volgende stappen

* Als u wilt beginnen met de evaluatie van Azure Arc-servers, volgt u het artikel een [verbinding maken tussen hybride computers en Azure via de Azure Portal](onboard-portal.md).

* Informatie over probleem oplossing vindt u in de [hand leiding problemen met verbonden machine agent oplossen](troubleshoot-agent-onboard.md).
