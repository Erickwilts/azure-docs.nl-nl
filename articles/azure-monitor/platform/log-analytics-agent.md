---
title: Verzamelen van gegevens van een met Azure Log Analytics-agent | Microsoft Docs
description: In dit onderwerp vindt u informatie over het verzamelen van gegevens en controleren van computers die worden gehost in Azure, on-premises of andere cloudomgeving met Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: magoedte
ms.openlocfilehash: 081d65f60eab4e2412a5dd14c3a63a18598e3b8a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67146321"
---
# <a name="collect-log-data-with-the-log-analytics-agent"></a>Verzamelen van logboekgegevens met de Log Analytics-agent

De Azure Log Analytics-agent, voorheen aangeduid als de Microsoft Monitoring Agent (MMA) of OMS-Linux-agent is ontwikkeld voor het beheer van uitgebreide in on-premises machines computers bewaakt door [System Center Operations Manager ](https://docs.microsoft.com/system-center/scom/), en virtuele machines in elke cloud. De Windows- en Linux-agents koppelen aan een Azure-Monitor en opslaan van verzamelde logboekgegevens uit verschillende bronnen in uw Log Analytics-werkruimte, evenals een unieke Logboeken of de metrische gegevens zoals gedefinieerd in een oplossing voor bewaking. 

Dit artikel bevat een gedetailleerd overzicht van de agent, system en netwerkvereisten en de verschillende implementatiemethoden.   

## <a name="overview"></a>Overzicht

![Log Analytics-agent communicatie diagram](./media/log-analytics-agent/log-analytics-agent-01.png)

Voordat u analyseren en uitvoeren van de verzamelde gegevens, moet u eerst installeren en verbinden-agents voor alle machines die u wilt gegevens verzenden naar de service Azure Monitor. U kunt agents installeren op uw Azure-VM's met behulp van de Azure Log Analytics VM-extensie voor Windows en Linux, en voor computers in een hybride omgeving met behulp van setup, vanaf de opdrachtregel, of met Desired State Configuration (DSC) in Azure Automation. 

De agent voor Linux en Windows uitgaand naar de Azure Monitor-service communiceert via TCP-poort 443, en als de computer verbinding maakt via een firewall of proxyserver om te communiceren via Internet, raadpleegt u onderstaande voorwaarden om te begrijpen van de netwerkconfiguratie Vereist. Als uw IT-beveiligingsbeleid niet computers op het netwerk verbinding maken met Internet toestaat, kunt u instellen een [Log Analytics gateway](gateway.md) en configureer vervolgens de agent verbinding maken via de gateway naar de logboeken van Azure Monitor. De agent vervolgens configuratie-informatie ontvangen en verzenden van gegevens die worden verzameld, afhankelijk van welke gegevens u regels voor het verzamelen en oplossingen voor de controle die u hebt ingeschakeld in uw werkruimte. 

Als u een computer met System Center Operations Manager 2012 R2 of hoger worden bewaakt, kan zijn met de Azure Monitor-service voor het verzamelen van gegevens en door te sturen naar de service en nog steeds worden bewaakt door multihomed [Operations Manager](../../azure-monitor/platform/om-agents.md). Met Linux-computers, de agent bevat geen een onderdeel van health-service als de Windows-agent wordt en gegevens worden verzameld en verwerkt door een beheerserver voor zijn rekening. Omdat de Linux-computers worden verschillende manieren worden bewaakt met Operations Manager, ze niet configuratie ontvangen of rechtstreeks gegevens te verzamelen en doorsturen via de beheergroep, zoals een Windows-agent beheerde systeem heeft. Als gevolg hiervan in dit scenario wordt niet ondersteund voor Linux-computers die rapporteren aan Operations Manager en moet u de Linux-computer te configureren [rapporteren aan een Operations Manager-beheergroep](../platform/agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group) en een Log Analytics-werkruimte in twee stappen.

De Windows-agent kan rapporteren dat maximaal vier Log Analytics-werkruimten, terwijl de Linux-agent biedt alleen ondersteuning voor rapportage aan één werkruimte.  

De agent voor Linux en Windows wordt niet alleen voor het verbinden met Azure Monitor, ondersteunt ook Azure Automation voor het hosten van de functie Hybrid Runbook worker en andere services zoals [bijhouden](../../automation/change-tracking.md), [updatebeheer](../../automation/automation-update-management.md), en [Azure Security Center](../../security-center/security-center-intro.md). Zie voor meer informatie over de Hybrid Runbook Worker-rol, [Azure Automation Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Ondersteunde Windows-besturingssystemen
De volgende versies van het Windows-besturingssysteem worden officieel ondersteund voor de Windows-agent:

* Windows Server 2019
* Windows Server 2008 R2, 2012, 2012 R2, 2016, versie 1709 en 1803
* Windows 7 SP1 en hoger

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen
In deze sectie bevat informatie over de ondersteunde Linux-distributies.    

Beginnen met versies die na augustus 2018 wordt uitgebracht, maken we de volgende wijzigingen aan ons ondersteuningsmodel:  

* Alleen de server-versies worden ondersteund, een niet-client.  
* Nieuwe versies van [goedgekeurd voor Azure Linux-distributies](../../virtual-machines/linux/endorsed-distros.md) altijd worden ondersteund.  
* Alle secundaire versies worden ondersteund voor elke primaire versie weergegeven.
* De versies die zijn geslaagd voor de datum van de fabrikant einde van ondersteuning worden niet ondersteund.  
* Nieuwe versies van AMI worden niet ondersteund.  
* Alleen versies met SSL 1.x standaard worden ondersteund.

>[!NOTE]
>Als u een distributie of een versie die wordt momenteel niet ondersteund en wordt niet afgestemd op onze ondersteuningsmodel gebruikt, raden wij dat u deze opslagplaats splitst, waarmee wordt bevestigd dat Microsoft-ondersteuning geen hulp bij gesplitste agent versies geeft.

* Amazon Linux 2017.09 (x 64)
* CentOS Linux 6 (x86/x64) en 7 (x 64)  
* Oracle Linux 6 en 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) en 7 (x 64)
* Debian GNU/Linux 8 en 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) en 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x 64)

>[!NOTE]
>OpenSSL 1.1.0 wordt alleen ondersteund op x86_x64 platforms (64-bits) en OpenSSL ouder dan 1.x wordt niet ondersteund op elk platform.
>

### <a name="agent-prerequisites"></a>Vereisten voor clientagents

De volgende tabel ziet u de pakketten zijn vereist voor de ondersteunde Linux-distributies die de agent worden geïnstalleerd op.

|Vereist pakket |Description |Minimale versie |
|-----------------|------------|----------------|
|Glibc |    GNU C-bibliotheek | 2.5-12 
|Openssl    | OpenSSL-bibliotheken | 1.0.x of 1.1.x |
|Curl | cURL webclient | 7.15.5 |
|Python-ctypes | | 
|PAM | Pluggable Authentication Modules | | 

>[!NOTE]
>Rsyslog of syslog-ng het volgende zijn vereist voor het verzamelen van syslog-berichten. De standaard syslog-daemon op versie 5 van Red Hat Enterprise Linux, CentOS en Oracle Linux-versie (sysklog) wordt niet ondersteund voor de verzameling van syslog. Voor het verzamelen van syslog-gegevens in deze versie van deze distributies, moet de rsyslog-daemon worden geïnstalleerd en geconfigureerd ter vervanging van sysklog.

## <a name="tls-12-protocol"></a>TLS 1.2-protocol
Als u wilt controleren of de beveiliging van gegevens die onderweg zijn naar Azure Monitor-Logboeken, we raden u aan de agent configureren voor het gebruik van ten minste Transport Layer Security (TLS) 1.2. Oudere versies van TLS/Secure Sockets Layer (SSL) kwetsbaar zijn gevonden en hoewel ze op dit moment nog steeds werken om toe te staan achterwaartse compatibiliteit, zijn ze onderling **niet aanbevolen**.  Raadpleeg voor meer informatie, [verzenden van gegevens veilig gebruik TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Netwerkvereisten voor de firewall
Gegevens van de onderstaande lijst de proxy- en firewallinstellingen configuratie-informatie nodig is voor de agent voor Linux en Windows om te communiceren met Azure Monitor-Logboeken.  

|Agentresource|Poorten |Richting |HTTPS-controle overslaan|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Poort 443 |Uitgaande|Ja |  
|*.oms.opinsights.azure.com |Poort 443 |Uitgaande|Ja |  
|*.blob.core.windows.net |Poort 443 |Uitgaande|Ja |  
|*.azure-automation.net |Poort 443 |Uitgaande|Ja |  

Zie voor firewall-informatie is vereist voor Azure Government, [beheer van Azure Government](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Als u van plan bent de Hybrid Runbook Worker voor Azure Automation gebruiken om te verbinden en registreren bij de service Automation runbooks gebruiken in uw omgeving, moet er toegang tot het poortnummer en de URL's die worden beschreven in [het netwerk configureren voor de Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md#network-planning). 

De Windows- en Linux-agent ondersteunt communiceren via een proxyserver of de Log Analytics-gateway naar Azure Monitor met behulp van het HTTPS-protocol.  Zowel eenvoudige als anonieme verificatie (gebruikersnaam en wachtwoord) worden ondersteund.  Voor de Windows-agent rechtstreeks verbonden met de service, de configuratie van de proxy is opgegeven tijdens de installatie of [na de implementatie](agent-manage.md#update-proxy-settings) via het Configuratiescherm of met PowerShell.  

Voor de Linux-agent de proxyserver wordt opgegeven tijdens de installatie of [na de installatie](agent-manage.md#update-proxy-settings) door het wijzigen van het configuratiebestand proxy.conf.  Waarde voor de Linux-agent proxyconfiguratie heeft de volgende syntaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Als uw proxy-server niet vereist is om te verifiëren, moet de Linux-agent nog steeds bieden een pseudo-gebruiker en wachtwoord. Dit kan een gebruikersnaam of het wachtwoord zijn.

|Eigenschap| Description |
|--------|-------------|
|Protocol | https |
|user | Optioneel de gebruikersnaam voor proxy-verificatie |
|wachtwoord | Optionele wachtwoord voor proxy-verificatie |
|proxyhost | Adres of FQDN-naam van de proxy-server/Log Analytics-gateway |
|poort | Optionele poortnummer voor de proxy-server/Log Analytics-gateway |

Bijvoorbeeld: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Als u speciale tekens zoals "\@" in uw wachtwoord, ontvangt u een proxy-verbindingsfout omdat de waarde is niet juist geparseerd.  U kunt dit probleem omzeilen, codeert u het wachtwoord in de URL met behulp van een hulpprogramma zoals [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Agent installeren en configureren 
Computers in uw Azure-abonnement of een hybride omgeving rechtstreeks met Azure Monitor Logboeken verbinding kan worden bewerkstelligd met verschillende methoden, afhankelijk van uw vereisten. De volgende tabel ziet u elke methode om te bepalen welke het beste werkt in uw organisatie.

|Bron | Methode | Description|
|-------|-------------|-------------|
|Azure VM| -Log Analytics VM-extensie voor [Windows](../../virtual-machines/extensions/oms-windows.md) of [Linux](../../virtual-machines/extensions/oms-linux.md) met behulp van Azure CLI of met een Azure Resource Manager-sjabloon<br>- [Handmatig vanuit de Azure-portal](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json). | De extensie voor de Log Analytics-agent geïnstalleerd op virtuele machines van Azure en deze heeft geregistreerd in een bestaande werkruimte van Azure Monitor.|
| Hybride Windows-computer|- [Handmatige installatie](agent-windows.md)<br>- [Azure Automation DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager-sjabloon met Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |De Microsoft Monitoring agent installeren vanaf de opdrachtregel of met behulp van een geautomatiseerde methode, zoals Azure Automation DSC, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), of met een Azure Resource Manager-sjabloon als u Microsoft hebt geïmplementeerd Azure Stack in uw datacenter.| 
| Hybride Linux-computer| [Handmatige installatie](../../azure-monitor/learn/quick-collect-linux-computer.md)|Installeer de agent voor Linux aanroepen van een wrapper-scripts die worden gehost op GitHub. | 
| System Center Operations Manager|[Operations Manager integreert met Log Analytics](../../azure-monitor/platform/om-agents.md) | Configureren van de integratie tussen Operations Manager en Azure Monitor logboeken naar doorsturen van Windows-computers die rapporteren aan een beheergroep die gegevens worden verzameld.|  

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [gegevensbronnen](../../azure-monitor/platform/agent-data-sources.md) om te begrijpen van de gegevensbronnen die beschikbaar zijn voor het verzamelen van gegevens van uw Windows- of Linux-systeem. 

* Meer informatie over [query's bijgehouden](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren. 

* Meer informatie over [bewakingsoplossingen](../../azure-monitor/insights/solutions.md) die functionaliteit toevoegen aan Azure Monitor en ook gegevens verzamelen in Log Analytics-werkruimte.
