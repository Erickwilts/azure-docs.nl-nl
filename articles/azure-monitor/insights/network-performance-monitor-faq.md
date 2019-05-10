---
title: Veelgestelde vragen - oplossing in Azure Network Performance Monitor | Microsoft Docs
description: In dit artikel bevat de antwoorden op veelgestelde vragen over Network Performance Monitor in Azure. Netwerk Netwerkprestatiemeter (NPM) helpt u de prestaties van uw netwerken bijna in realtime controleren en het opsporen en zoek op het netwerk knelpunten.
services: log-analytics
documentationcenter: ''
author: vinynigam
manager: agummadi
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/12/2018
ms.author: vinynigam
ms.openlocfilehash: d573b7ad9edac6b1502744b61e85cba3402a6f68
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232668"
---
# <a name="network-performance-monitor-solution-faq"></a>Veelgestelde vragen over het netwerk Prestatiemeter-oplossing

![Network Performance Monitor symbool](media/network-performance-monitor-faq/npm-symbol.png)

In dit artikel bevat de antwoorden op veelgestelde vragen over (Netwerkprestatiemeter) in Azure

[Netwerkprestatiemeter](/azure/networking/network-monitoring-overview) is een cloud-gebaseerde [hybride netwerkbewaking](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md) oplossing waarmee u netwerkprestaties tussen verschillende punten in uw netwerkinfrastructuur te bewaken. Ook kunt u verbinding met het netwerk te bewaken [service en toepassing eindpunten](../../azure-monitor/insights/network-performance-monitor-service-connectivity.md) en [de prestaties van Azure ExpressRoute controleren](../../azure-monitor/insights/network-performance-monitor-expressroute.md). 

Network Performance Monitor detecteert netwerkproblemen, zoals verkeer blackholing, routering fouten en problemen die controlemethoden conventionele netwerk niet kan detecteren. De oplossing genereert waarschuwingen en waarschuwt u als een drempelwaarde voor een netwerkverbinding wordt overschreden. Bovendien worden problemen met de netwerkprestaties tijdig gedetecteerd en wordt de oorzaak van het probleem op een bepaald netwerksegment of apparaat opgespoord. 

Meer informatie over de verschillende mogelijkheden ondersteund door [Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) is online beschikbaar.

## <a name="set-up-and-configure-agents"></a>Instellen en configureren van agents

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>Wat zijn de vereisten voor het beheerplatform voor de knooppunten die moeten worden gebruikt voor het bewaken via NPM?
Hieronder ziet u de Platformvereisten voor NPM van verschillende mogelijkheden:

- De Prestatiemeter en Connectiviteitsmonitor voor Service-mogelijkheden van NPM ondersteuning voor zowel Windows server en Windows-desktops/client-besturingssystemen. Windows server OS-versies die ondersteund zijn 2008 SP1 of hoger. Windows-desktops/client-versies die ondersteund zijn voor Windows 10, Windows 8.1, Windows 8 en Windows 7. 
- NPM van ExpressRoute-bewaking mogelijkheid ondersteunt alleen Windows server (2008 SP1 of hoger) besturingssysteem.

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>Kan ik Linux-machines als de bewaking van knooppunten in NPM gebruiken?
De mogelijkheid voor het bewaken van netwerken met behulp van op basis van Linux-knooppunten is momenteel in preview. Contact opnemen met uw Account Manager voor meer informatie. Linux-agents bewakingsmogelijkheid alleen voor de NPM van Prestatiemeter functionaliteit bieden, en zijn niet beschikbaar voor de Connectiviteitsmonitor voor Service- en ExpressRoute-bewaking mogelijkheden

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>Wat zijn de vereisten van de grootte van de knooppunten moet worden gebruikt voor het bewaken via NPM?
Voor het uitvoeren van de NPM-oplossing op knooppunt VM's voor het bewaken van netwerken, moet de knooppunten ten minste 500 MB geheugen en één kern. U hoeft niet te gebruiken van afzonderlijke knooppunten voor het uitvoeren van NPM. De oplossing kunt uitvoeren op de knooppunten waarop andere werkbelastingen die erop worden uitgevoerd. De oplossing heeft de mogelijkheid om te stoppen van het bewakingsproces als hierbij meer dan 5% CPU.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>Voor het gebruik van NPM, moet ik verbinding maken met mijn knooppunten als agent toewijzen of via System Center Operations Manager?
Zowel de Prestatiemeter en de Connectiviteitsmonitor voor Service-mogelijkheden ondersteunen knooppunten [verbonden als Direct Agents](../../azure-monitor/platform/agent-windows.md) en [verbonden zijn via Operations Manager](../../azure-monitor/platform/om-agents.md).

Voor ExpressRoute-bewaking functionaliteit, die moeten de Azure-knooppunten worden verbonden als Direct Agents alleen. Azure knooppunten die zijn verbonden via Operations Manager worden niet ondersteund. Voor on-premises knooppunten, worden de knooppunten verbonden als Direct Agents en via Operations Manager ondersteund voor het bewaken van een ExpressRoute-circuit.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>Welk protocol TCP-en ICMP zo worden gekozen voor het bewaken van?
Als u uw netwerk met behulp van Windows server-gebaseerde knooppunten worden bewaakt, raden wij aan u TCP als protocol voor bewaking omdat het biedt betere nauwkeurigheid. 

ICMP wordt aanbevolen voor Windows-desktops /-client op basis van een besturingssysteem knooppunten. Dit platform does'nt TCP-gegevens worden verzonden via onbewerkte sockets, dat gebruikmaakt van NPM voor het detecteren van de topologie van netwerk toestaan.

U kunt meer informatie krijgen over de relatieve voordelen van elk protocol [hier](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md#choose-the-protocol).

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>Hoe configureer ik een knooppunt om te controleren met behulp van TCP-protocol ondersteunen?
Voor het knooppunt voor de ondersteuning van bewaking met TCP-protocol: 
* Zorg ervoor dat het platform knooppunt Windows Server (2008 SP1 of hoger).
* Voer [EnableRules.ps1](https://aka.ms/npmpowershellscript) Powershell-script op het knooppunt. Zie [instructies](../../azure-monitor/insights/network-performance-monitor.md#configure-log-analytics-agents-for-monitoring) voor meer informatie.


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>Hoe kan ik de TCP-poort wordt gebruikt door NPM voor het bewaken van wijzigen?
U kunt de TCP-poort die wordt gebruikt door NPM voor bewaking, door het uitvoeren van de [EnableRules.ps1](https://aka.ms/npmpowershellscript) script. U moet het poortnummer dat u wilt gebruiken als een parameter invoeren. Bijvoorbeeld, als u TCP op poort 8060, voert u `EnableRules.ps1 8060`. Zorg ervoor dat u op alle knooppunten die wordt gebruikt voor het bewaken van de TCP-poort gebruiken.

Het script configureert alleen Windows Firewall lokaal. Als u hebt de firewall of een Netwerkbeveiligingsgroep (NSG) regels, ervoor zorgen dat het verkeer dat bestemd is voor de TCP-poort die wordt gebruikt door NPM dat.

### <a name="how-many-agents-should-i-use"></a>Het aantal agents moet ik gebruiken?
U moet ten minste één agent gebruiken voor elk subnet dat u wilt bewaken.

### <a name="what-is-the-maximum-number-of-agents-i-can-use-or-i-see-error--youve-reached-your-configuration-limit"></a>Wat is het maximum aantal agents dat ik kan gebruiken of ik Zie-fout '... u hebt uw limiet configuratie worden bereikt"?
NPM beperkt het aantal IP-adressen aan 5000 IP-adressen per werkruimte. Als een knooppunt zowel IPv4 als IPv6-adressen heeft, wordt dit meegeteld als 2 IP-adressen voor dat knooppunt. Daarom kan bepaalt deze limiet van 5000 IP-adressen de bovengrens voor het aantal agents. U kunt de inactieve agents verwijderen uit knooppunten tabblad in NPM >> configureren. NPM onderhoudt ook geschiedenis van alle de IP-adressen die ooit zijn toegewezen aan de virtuele machine die als host fungeert voor de agent en elk wordt geteld als afzonderlijke IP-adres een bijdrage levert aan die limiet van 5000 IP-adressen. Aan vrij te maken van IP-adressen voor uw werkruimte, kunt u de pagina knooppunten te verwijderen van de IP-adressen die zich niet in gebruik.

## <a name="monitoring"></a>Bewaking

### <a name="how-are-loss-and-latency-calculated"></a>Hoe worden beschermd tegen verlies en latentie berekend
Bron agents verzenden ofwel TCP SYN aanvragen (als TCP wordt geselecteerd als het protocol voor het bewaken van) of ICMP-echoaanvragen (als ICMP wordt geselecteerd als het protocol voor het bewaken van) naar de doel-IP met regelmatige intervallen om ervoor te zorgen dat alle paden tussen de bron-doel-IP combinatie worden besproken. Het percentage ontvangen pakketten en de retourtijd pakket wordt voor het berekenen van de verlies en latentie van elk pad gemeten. Deze gegevens worden samengevoegd via het polling-interval en de paden om op te halen van de geaggregeerde waarden van verlies en latentie voor de combinatie van IP voor de specifieke polling-interval.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>Met welke frequentie de bronagent verzendt pakketten naar de bestemming voor het bewaken van?
Voor Prestatiemeter- en ExpressRoute-bewaking mogelijkheden, de bron-pakketten verzendt om de vijf seconden en de netwerkmetingen registreert. Deze gegevens worden samengevoegd tijdens een pollinginterval van 3 minuten voor het berekenen van de gemiddelde en piekuren waarden van verlies en latentie. Voor de Connectiviteitsmonitor voor Service-functionaliteit, wordt de frequentie van het verzenden van de pakketten voor netwerkmeting bepaald door de frequentie die is ingevoerd door de gebruiker voor de specifieke test tijdens het configureren van de test.

### <a name="how-many-packets-are-sent-for-monitoring"></a>Hoeveel pakketten worden verzonden voor het bewaken van?
Het aantal pakketten dat is verzonden door de bronagent naar doel in een polling kan worden aangepast en wordt bepaald door ons eigen algoritme, die kan verschillen voor verschillende netwerktopologieën. Meer het aantal netwerkpaden tussen de bron-doel-IP-combinatie, meer is het aantal pakketten die worden verzonden. Het systeem zorgt ervoor dat alle paden tussen de bron-doel-IP-combinatie zijn gedekt.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>Hoe detecteert NPM netwerktopologie tussen bron en bestemming?
NPM maakt gebruik van een eigen algoritme op basis van Traceroute voor het detecteren van alle paden en hops tussen bron en bestemming.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>Biedt NPM Routering en switch niveau info 
Hoewel NPM alle mogelijke routes tussen de bronagent en de bestemming detecteert, biedt het geen zichtbaarheid waarin route is gemaakt door de pakketten worden verzonden door uw specifieke workloads. Aan de hand van de oplossing kunt u de paden en het onderliggende netwerkhops, die meer latentie dan u verwacht wilt toevoegen.

### <a name="why-are-some-of-the-paths-unhealthy"></a>Waarom zijn sommige van de paden niet in orde?
Verschillende netwerkpaden kunnen bestaan tussen de bron en doel-IP-adressen en elk pad kan een andere waarde van het verlies en latentie. NPM markeert deze paden niet in orde (aangegeven met kleur rood) waarvoor de waarden van gegevensverlies en/of latentie is groter dan de respectieve drempelwaarde die is ingesteld in de configuratie van de bewaking.

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>Wat een hop in rood in de toewijzing van de topologie van netwerk overheen?
Als een hop rood is, geeft aan dat het deel van ten minste één pad op niet in orde uitmaakt. NPM markeert alleen de paden is beschadigd, het heeft niet de status van elk pad scheiden. U kunt voor het identificeren van de lastige hops, bekijken van de hop-by-hop '-latentie en scheiden van de computer die bij het toevoegen van meer dan de verwachte latentie.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>Hoe werkt het lokaliseren van fouten in Prestatiemeter?
NPM gebruikt een mechanisme voor probabilistic fault-kansen toewijzen aan elke netwerkpad, netwerksegment, en wanneer ze een deel van de samenstellende netwerkhops op basis van het aantal paden niet in orde zijn. Zodra de netwerksegmenten en hops deel uit van meer aantal paden niet in orde, wordt de fout-de kans dat is gekoppeld aan deze verhoogt. Dit algoritme werkt het beste als u veel knooppunten met NPM-agent die met elkaar verbonden hebben als dit de gegevenspunten voor het berekenen van de fout-kansen toeneemt.

### <a name="how-can-i-create-alerts-in-npm"></a>Hoe kan ik waarschuwingen in NPM maken?
Raadpleeg [sectie in de documentatie van waarschuwingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor#alerts) voor stapsgewijze instructies.

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>NPM kunt bewaken routers en servers als afzonderlijke apparaten?
NPM wordt alleen de IP-adres en host-naam van onderliggende netwerkhops (switches, routers, servers, enzovoort) tussen de bron en doel-IP-adressen. Het identificeert ook de latentie tussen deze geïdentificeerde hops. Het bewaakt deze onderliggende hops niet afzonderlijk.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>Kan de NPM worden gebruikt voor het bewaken van de netwerkverbinding tussen Azure en AWS?
Ja. Raadpleeg het artikel [Monitor Azure, AWS en on-premises netwerken met behulp van NPM](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/) voor meer informatie.

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>Is het ExpressRoute-bandbreedtegebruik binnenkomende of uitgaande?
Gebruik van netwerkbandbreedte is het totaal van binnenkomende en uitgaande bandbreedte. Het wordt uitgedrukt in Bits per seconde.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>Vind we informatie over de binnenkomende en uitgaande bandbreedte voor de ExpressRoute?
Binnenkomende en uitgaande waarden voor zowel primaire als secundaire bandbreedte kunnen worden vastgelegd.

Voor peering niveau informatie gebruikt u de hieronder genoemde query in zoeken in Logboeken

    NetworkMonitoring 
    | where SubType == "ExpressRoutePeeringUtilization"
    | project CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
  
Gebruik voor circuit niveau informatie, de hieronder genoemde query 

    NetworkMonitoring 
    | where SubType == "ExpressRouteCircuitUtilization"
    | project CircuitName,PrimaryBytesInPerSecond, PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>Welke regio's worden ondersteund voor de NPM-Prestatiemeter?
NPM kunt Controleer de connectiviteit tussen netwerken in een deel van de hele wereld, uit een werkruimte die wordt gehost in een van de [ondersteunde regio's](../../azure-monitor/insights/network-performance-monitor.md#supported-regions)

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>Welke regio's worden ondersteund voor de Connectiviteitsmonitor voor de NPM-Service?
NPM kunt connectiviteit controleren naar services in een deel van de hele wereld, uit een werkruimte die wordt gehost in een van de [ondersteunde regio's](../../azure-monitor/insights/network-performance-monitor.md#supported-regions)

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>Welke regio's worden ondersteund voor ExpressRoute-bewaking van NPM?
NPM kunt bewaken van uw ExpressRoute-circuits die zich in een Azure-regio. Voor Onboarding van NPM, moet u een Log Analytics-werkruimte die moet worden gehost in een van de [ondersteunde regio's](/azure/expressroute/how-to-npm)

## <a name="troubleshoot"></a>Problemen oplossen

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>Waarom zijn een aantal de hops gemarkeerd als niet-geïdentificeerde in de weergave van de topologie van netwerk?
Een aangepaste versie van traceroute NPM gebruikt voor het detecteren van de topologie van de bronagent naar de bestemming. Een niet-geïdentificeerde hop vertegenwoordigt de netwerk-hop reageert niet op de bronagent traceroute aanvraag. Als drie opeenvolgende netwerkhops niet op traceroute van de agent reageren, wordt de oplossing de niet-reagerende hops als niet-geïdentificeerde markeert en probeert niet meer hops detecteren.

Een hop reageert niet op een traceroute in een of meer van de onderstaande scenario's:

* De routers zijn geconfigureerd om hun identiteit niet weer te geven.
* De netwerkapparaten zijn niet ICMP_TTL_EXCEEDED verkeer wordt toegestaan.
* Een firewall wordt geblokkeerd door het antwoord ICMP_TTL_EXCEEDED van het netwerkapparaat.

### <a name="why-does-my-link-show-unhealthy-but-the-topology-does-not"></a>Waarom wordt Mijn koppeling weergeven die niet in orde, maar de topologie niet 
NPM bewaakt end-to-end-verlies en latentie topologie met verschillende intervallen. Verlies en latentie worden gemeten om de vijf seconden en samengevoegd om de drie minuten (voor Prestatiemeter en Express Route-Monitor) terwijl topologie wordt berekend met behulp van traceroute om de tien minuten. Bijvoorbeeld, tussen de 3:44 en 4:04 topologie kan worden bijgewerkt drie keer (3:44, 3:54, 4:04), maar verlies en latentie over zeven keer worden bijgewerkt (3:44, 3:47, 3:50, 3:53, 3:56 van 3:59, 4:02). De topologie die is gegenereerd op 3:54 wordt weergegeven voor het verlies en latentie die wordt berekend op 3:56 van 3:59 en 4:02. Stel dat u ontvangt een waarschuwing dat uw circuit ER niet in orde werd op 3:59. U aanmelden bij NPM en probeer het instellen van de tijd van de topologie op 3:59. NPM, worden de topologie die is gegenereerd op 3:54 weergegeven. Voor meer informatie over de laatste bekende topologie van uw netwerk, vergelijkt de velden TimeProcessed (tijd op welke verlies en latentie is berekend) en TracerouteCompletedTime (tijd op welke topologie is berekend). 

### <a name="what-is-the-difference-between-the-fields-e2emedianlatency-and-avghoplatencylist-in-the-networkmonitoring-table"></a>Wat is het verschil tussen de velden E2EMedianLatency en AvgHopLatencyList in de tabel NetworkMonitoring
E2EMedianLatency is de latentie bijgewerkt om de drie minuten nadat elke 10 minuten op basis van traceroute aggregeren van de resultaten van de TCP-ping-tests, terwijl AvgHopLatencyList is bijgewerkt. Voor meer informatie over de precieze tijd waarop E2EMedianLatency is berekend, moet u het veld TimeProcessed gebruiken. Voor meer informatie over de precieze tijd bij welke traceroute voltooid en bijgewerkte AvgHopLatencyList, gebruikt u het veld TracerouteCompletedTime

### <a name="why-does-hop-by-hop-latency-numbers-differ-from-hoplatencyvalues"></a>Waarom verschilt hop-by-hop '-latentie van HopLatencyValues 
HopLatencyValues zijn bron naar eindpunt.
Bijvoorbeeld: Hops - A, B, C. AvgHopLatency - 10,15,20. Dit betekent dat de bron naar een latentie = 10, bron voor B latentie = 15 en bron voor C latentie is 20. Gebruikersinterface wordt A-B hop-latentie als 5 in de topologie berekend

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>De oplossing toont 100% verlies, maar er een verbinding tussen de bron en bestemming is
Dit kan gebeuren als de firewall van de host of de tussenliggende firewall (netwerkfirewall of Azure NSG) de communicatie tussen de bronagent en de bestemming via de poort die wordt gebruikt blokkeren is voor het bewaken van NPM (standaard de poort is 8084, tenzij de de klant is gewijzigd. dit).

* Om te controleren of de communicatie over de vereiste poort niet door de hostfirewall wordt geblokkeerd, raadpleegt u de status van de bron- als doelknooppunten uit de volgende weergave: Network Performance Monitor -> configuratieserver -> knooppunten. 
  Als ze niet in orde zijn, instructies en neemt u corrigerende maatregelen. Als de knooppunten in orde zijn, verplaatst naar de stap b. hieronder.
* Gebruik om te controleren of een tussenliggende firewall of Azure NSG de communicatie over de vereiste poort niet blokkeert, de derde partij PsPing hulpprogramma via de onderstaande instructies volgen:
  * psping hulpprogramma is beschikbaar als download [hier](https://technet.microsoft.com/sysinternals/psping.aspx) 
  * Voer de volgende opdracht van het bronknooppunt.
    * psping -n 15 \<doelknooppunt IPAddress\>: portNumber standaard NPM 8084 poort gebruikt. In geval u hebt dit expliciet gewijzigd met behulp van het script EnableRules.ps1, voer het aangepaste poortnummer in dat u gebruikt). Dit is een ping van de machine van Azure naar on-premises
* Controleer of de pings geslaagd zijn. Als dat niet het geval is, klikt u vervolgens wordt aangegeven dat het verkeer op deze poort wordt geblokkeerd door een tussenliggende firewall of Azure NSG.
* Voer nu de opdracht van doelknooppunt om IP-adres van de gegevensbron.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>Er is verlies van knooppunt A naar B, maar niet vanaf het knooppunt B naar A. Waarom?
Als de netwerkpaden tussen A naar B van de netwerkpaden tussen B naar A afwijken kunnen, kunnen verschillende waarden voor verlies en latentie worden waargenomen.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>Waarom wordt mijn ExpressRoute-circuits en -peeringverbindingen niet gedetecteerd?
NPM detecteert nu ExpressRoute-circuits en -peeringverbindingen in alle abonnementen waartoe de gebruiker toegang heeft. Alle abonnementen waar uw Express Route-resources zijn gekoppeld en schakel de bewaking voor elke gedetecteerde bron kiezen. NPM zoekt connection-objecten bij het detecteren van een privé-peering, dus controleer geven als een VNET is gekoppeld aan uw peering.

### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>De functie ER Monitor heeft een diagnostisch bericht 'Verkeer wordt niet doorgegeven via een circuit'. Wat houdt dat in?

Er is een scenario waarin er is een goede verbinding tussen de on-premises en Azure-knooppunten, maar het verkeer gaat niet via de ExpressRoute-circuit dat is geconfigureerd om te worden bewaakt door NPM. 

Dit kan gebeuren als:

* Het circuit ER is niet actief.
* De routefilters worden geconfigureerd in zodanig dat ze prioriteit aan de andere routes (zoals een VPN-verbinding of een ander ExpressRoute-circuit geven) via het bedoelde ExpressRoute-circuit. 
* De on-premises en Azure-knooppunten gekozen voor het bewaken van het ExpressRoute-circuit in de configuratie van de bewaking, geen verbinding met elkaar via het bedoelde ExpressRoute-circuit. Zorg ervoor dat u hebt ervoor gekozen juiste knooppunten die zijn verbonden met elkaar via de ExpressRoute-circuit dat u van plan bent om te controleren.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>Tijdens het configureren van bewaking van mijn ExpressRoute-circuit, worden de Azure-knooppunten niet gedetecteerd.
Dit kan gebeuren als de Azure-knooppunten zijn verbonden via Operations Manager. De mogelijkheid van de ExpressRoute-bewaking biedt ondersteuning voor alleen die Azure knooppunten die zijn verbonden als directe Agents.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>Ik kan niet detecteren door ExpressRoute-circuits in de OMS-portal
Hoewel NPM voor zowel in de Azure-portal, evenals de OMS-portal, werkt de detectie van het circuit in de functie ExpressRoute Monitor alleen via de Azure-portal. Zodra de circuits zijn gedetecteerd via Azure portal, kunt u de mogelijkheid vervolgens gebruiken in een van de twee portals. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>In de Connectiviteitsmonitor voor Service-capaciteit, worden de reactietijd van service, wegvallend netwerk, evenals latentie weergegeven als NA
Dit kan gebeuren als een of meer ingesteld op true is:

* De service niet actief is.
* Het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service is niet actief.
* Het doel is opgegeven in de Testconfiguratie van de is onjuist.
* Het knooppunt beschikt niet over een netwerkverbinding.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>De reactietijd van een geldige service wordt weergegeven in de Connectiviteitsmonitor voor Service-capaciteit, maar wegvallend netwerk, evenals de latentie worden weergegeven als NA
 Dit kan gebeuren als een of meer ingesteld op true is:

* Als het knooppunt dat wordt gebruikt voor het controleren van de netwerkverbinding met de service een Windows client-computer is, de doelservice ICMP-aanvragen blokkeert of een netwerkfirewall ICMP-aanvragen die afkomstig uit het knooppunt zijn blokkeert.
* Het selectievakje uitvoeren netwerk metingen is leeg in de Testconfiguratie.

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>In de Connectiviteitsmonitor voor Service-capaciteit, de reactietijd van de service is NA maar wegvallend netwerk, evenals de latentie geldig zijn
Dit kan gebeuren als de doelservice niet een web-App is, maar de test is geconfigureerd als een Webtest. De Testconfiguratie van de bewerken en de test kiezen als het netwerk in plaats van Web.

## <a name="miscellaneous"></a>Overig

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>Is er een prestatie-impact op het knooppunt wordt gebruikt voor het bewaken van?
NPM-proces is geconfigureerd om te stoppen als deze gebruikmaakt van meer dan 5% van de host CPU-resources. Dit is om ervoor te zorgen dat u blijven de knooppunten voor hun gebruikelijke werkbelastingen gebruiken kunt zonder gevolgen voor prestaties.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>NPM firewall-regels voor het bewaken van bewerken?
NPM maakt alleen een lokale Windows Firewall-regel op de knooppunten waarop de Powershell-script EnableRules.ps1 wordt uitgevoerd, zodat de agents TCP-verbindingen met elkaar maken op de opgegeven poort. De oplossing biedt een firewall of een Netwerkbeveiligingsgroep (NSG) regels niet worden gewijzigd.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>Hoe kan ik de status van de knooppunten die wordt gebruikt voor het bewaken van controleren?
U kunt de status van de knooppunten die wordt gebruikt voor het bewaken van de volgende weergave bekijken: Network Performance Monitor -> configuratieserver -> knooppunten. Als een knooppunt niet in orde is, kunt u de Foutdetails weergeven en de voorgestelde actie ondernemen.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>Kan NPM latentie rapporteren in microseconden
NPM rondt af naar de latentie van getallen in de gebruikersinterface en in milliseconden. Dezelfde gegevens worden opgeslagen op een hogere granulariteit (soms maximaal vier cijfers achter de komma).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Network Performance Monitor door te verwijzen naar [oplossing in Azure Network Performance Monitor](../../azure-monitor/insights/network-performance-monitor.md).
