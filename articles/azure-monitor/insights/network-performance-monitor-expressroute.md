---
title: Network Performance Monitor-oplossing in Azure Log Analytics | Microsoft Docs
description: De ExpressRoute-bewaking-mogelijkheid in Network Performance Monitor gebruiken voor het bewaken van end-to-end-connectiviteit en -prestaties tussen uw filialen en Azure, via Azure ExpressRoute.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: absha
ms.openlocfilehash: 7f9c0d905a7b2bc81063e59229d78a1200894d47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65963923"
---
# <a name="expressroute-monitor"></a>ExpressRoute-bewaking

Kunt u de mogelijkheid van Azure ExpressRoute Monitor [Network Performance Monitor](network-performance-monitor.md) voor het bewaken van end-to-end-connectiviteit en -prestaties tussen uw filialen en Azure, via Azure ExpressRoute. Belangrijkste voordelen zijn: 

- Automatische detectie van ExpressRoute-circuits die zijn gekoppeld aan uw abonnement.
- Het bijhouden van bandbreedtegebruik, -verlies en latentie in het circuit, peering en Azure Virtual Network-niveau voor ExpressRoute.
- Detectie van de netwerktopologie van uw ExpressRoute-circuits.

![ExpressRoute-bewaking](media/network-performance-monitor-expressroute/expressroute-intro.png)

## <a name="configuration"></a>Configuratie 
Open de configuratie voor Network Performance Monitor te openen de [Network Performance Monitor oplossing](network-performance-monitor.md) en selecteer **configureren**.

### <a name="configure-network-security-group-rules"></a>Configureer regels voor netwerkbeveiligingsgroepen 
Voor de servers in Azure die worden gebruikt voor het bewaken via Network Performance Monitor, network security group (NSG) regels voor TCP-verkeer op de poort die wordt gebruikt door Network Performance Monitor voor synthetische transacties te configureren. De standaardpoort is 8084. Deze configuratie kan de Log Analytics-agent geïnstalleerd op Azure Virtual machines om te communiceren met een on-premises monitoring agent. 

Zie voor meer informatie over nsg's [Netwerkbeveiligingsgroepen](../../virtual-network/manage-network-security-group.md). 

>[!NOTE]
> Voordat u met deze stap doorgaat, installeert u de on-premises server-agent en de Azure-server-agent en voer het script EnableRules.ps1 PowerShell. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute-peering verbindingen detecteren 
 
1. Selecteer de **ExpressRoute-Peerings** weergeven.
2. Selecteer **nu detecteren** voor het detecteren van alle ExpressRoute particuliere peerings die zijn verbonden met de virtuele netwerken in het Azure-abonnement is gekoppeld met dit Azure Log Analytics-werkruimte.

    >[!NOTE]
    > De oplossing wordt momenteel alleen ExpressRoute privé-peerings. 

    >[!NOTE]
    > Alleen worden persoonlijke peerings verbonden met de virtuele netwerken die zijn gekoppeld aan het abonnement is gekoppeld met deze Log Analytics-werkruimte gedetecteerd. Als ExpressRoute is verbonden met virtuele netwerken buiten het abonnement dat is gekoppeld aan deze werkruimte, moet u een Log Analytics-werkruimte maken in deze abonnementen. Network Performance Monitor vervolgens gebruiken voor het bewaken van deze peerings. 

    ![Configuratie van ExpressRoute](media/network-performance-monitor-expressroute/expressroute-configure.png)
 
   Nadat de detectie voltooid is, worden de gedetecteerde privé-peeringverbindingen in een tabel weergegeven. De bewaking voor deze peerings is in eerste instantie een uitgeschakelde status. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Schakel de bewaking van de ExpressRoute-peering-verbindingen 

1. Selecteer de persoonlijke peering verbinding die u wilt bewaken.
2. Selecteer in het deelvenster aan de rechterkant de **deze Peering controleren** selectievakje. 
3. Als u van plan bent de gezondheid van gebeurtenissen voor deze verbinding maken, selecteert u **statuscontrole inschakelen voor deze peering**. 
4. Kies voorwaarden controleren. U kunt aangepaste drempelwaarden voor het genereren van de gebeurtenis status instellen door te voeren drempelwaarden. Wanneer de waarde van de voorwaarde hoger dan de geselecteerde drempelwaarde voor de peering-verbinding gaat, wordt een statusgebeurtenis wordt gegenereerd. 
5. Selecteer **Agents toevoegen** de bewakingsagents die u gebruiken wilt voor het bewaken van deze peering verbinding kiezen. Zorg ervoor dat u agents aan beide uiteinden van de verbinding toevoegen. U moet ten minste één agent in het virtuele netwerk is verbonden met deze peering. Ook moet u ten minste één on-premises agent verbonden met deze peering. 
6. Selecteer **opslaan** aan de configuratie op te slaan. 

   ![Configuratie van de ExpressRoute-bewaking](media/network-performance-monitor-expressroute/expressroute-configure-discovery.png)


Nadat u de regels en de select-waarden en de agents hebt ingeschakeld, wacht u 30 tot 60 minuten voor de waarden voor het vullen van en de **ExpressRoute-controle** tegels worden weergegeven. Wanneer u de bewaking tegels ziet, zijn nu uw ExpressRoute-circuits en verbindingsresources bewaakt door Network Performance Monitor. 

>[!NOTE]
> Deze functie werkt op betrouwbare wijze op werkruimten die zijn bijgewerkt naar de nieuwe querytaal.

## <a name="walkthrough"></a>Walkthrough 

Het dashboard Network Performance Monitor geeft een overzicht van de status van ExpressRoute-circuits en -peeringverbindingen. 

![Network Performance Monitor-dashboard](media/network-performance-monitor-expressroute/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Lijst met circuits 

Een lijst van alle bewaakte ExpressRoute-circuits wilt bekijken, selecteert u de tegel van ExpressRoute-circuits. U kunt een circuit selecteren en de status, trendgrafieken voor pakketverlies en bandbreedtegebruik latentie weergeven. De grafieken zijn interactief. U kunt een aangepaste tijdvenster voor het uitzetten van de grafieken selecteren. Sleep de muis over een gebied in het diagram inzoomen en Zie fijnmazig gegevenspunten. 

![Lijst met ExpressRoute-circuits](media/network-performance-monitor-expressroute/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Trends van gegevensverlies, latentie en doorvoer 

De bandbreedte-gebruik, latentie en verlies grafieken, zijn interactief. U kunt inzoomen op een gedeelte van deze grafieken met behulp van besturingselementen van de muis. U kunt ook de bandbreedte, latentie en van verliesgegevens voor andere intervallen zien. In de linkerbovenhoek onder de **acties** knop, selecteer **datum/tijd**. 

![ExpressRoute-latentie](media/network-performance-monitor-expressroute/expressroute-latency.png) 

### <a name="peerings-list"></a>Lijst met Peerings 

Als u wilt maken van een lijst van alle verbindingen met virtuele netwerken via persoonlijke peering, selecteer de **privé-Peerings** tegel op het dashboard. Hier kunt u een virtuele netwerkverbinding en de status, trendgrafieken voor pakketverlies en bandbreedtegebruik latentie weergeven. 

![ExpressRoute-peerings](media/network-performance-monitor-expressroute/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Circuit-topologie 

Als u wilt weergeven van circuit topologie, selecteer de **topologie** tegel. Hiermee gaat u naar de Topologieweergave van de geselecteerde circuit of peering. Diagram van de topologie biedt de latentie voor elk segment in het netwerk, en elke laag-3-hop wordt vertegenwoordigd door een knooppunt in het diagram. Selecteren van een hop geeft meer informatie over de hop. Om te vergroten het niveau van de zichtbaarheid van on-premises hops opnemen, verplaats de schuifregelaar onder **FILTERS**. Verplaatsen van de schuifregelaar naar de linker- of toeneemt of verkleint u het aantal hops in de grafiek topologie. De latentie voor elk segment is zichtbaar, waardoor voor snellere isolatie van segmenten van hoge latentie in uw netwerk.

![Topologie van ExpressRoute](media/network-performance-monitor-expressroute/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Van de gedetailleerde Topologieweergave van een circuit 

Deze weergave toont virtuele netwerkverbindingen. 

![ExpressRoute-verbindingen voor virtueel netwerk](media/network-performance-monitor-expressroute/expressroute-vnet.png)
 
## <a name="diagnostics"></a>Diagnostiek 

Network Performance Monitor kunt u verschillende circuit Verbindingsproblemen vaststellen. Aantal van de problemen die u kunt zien, worden hieronder vermeld.

U kunt het overzicht van de melding en waarschuwingen instellen voor deze via **LogAnalytics**. Op de **NPM Diagnostics** pagina, ziet u beschrijvingen voor elk bericht diagnostics is geactiveerd.

| Melding Code (Logboeken) | Description |
| --- | --- |
| 5501 | Kan niet door secundaire verbinding van het ExpressRoute-circuit stromen |
| 5502 | Kan niet door primaire verbinding van het ExpressRoute-circuit stromen |
| 5503 | Er is geen circuit gevonden voor het abonnement is gekoppeld aan de werkruimte | 
| 5508 | Kan niet bepalen of verkeer wordt doorgegeven via een circuit voor het pad |
| 5510 | Het verkeer wordt niet doorgegeven via het beoogde circuit | 
| 5511 | Het verkeer wordt niet doorgegeven via het beoogde virtuele netwerk | 

**Circuit is niet beschikbaar.** Network Performance Monitor ontvangt u een melding zodra de verbinding tussen uw on-premises bronnen en virtuele netwerken van Azure verbroken is. Deze melding kunt u proactieve maatregelen voordat u escalaties van de gebruiker ontvangt en minder uitvaltijd.

![ExpressRoute-circuit is niet actief](media/network-performance-monitor-expressroute/expressroute-circuit-down.png)
 

**Verkeer niet via het beoogde circuit.** Network Performance Monitor ontvangt u een melding wanneer wordt niet via het beoogde ExpressRoute-circuit verkeersstromen. Dit probleem kan zich voordoen als het circuit niet actief is en het verkeer wordt doorgestuurd via de back-route. Dit kan ook gebeuren als er een probleem met de routering. Deze informatie helpt u proactief configuratieproblemen met in uw routeringsbeleid beheren en zorg ervoor dat de meest optimale en veilige route wordt gebruikt. 

 

**Verkeersstroom niet naar primaire circuit.** Network Performance Monitor waarschuwt u als via het secundaire ExpressRoute-circuit verkeersstromen. Zelfs als u wordt niet elke verbindingsproblemen in dit geval, proactief ondervindt kunt problemen met het primaire circuit u beter voorbereid. 

 
![ExpressRoute-netwerkverkeer](media/network-performance-monitor-expressroute/expressroute-traffic-flow.png)


**Afnemen als gevolg van piekgebruik.** U kunt de trend voor het gebruik van bandbreedte met de trend in replicatielatentie om te bepalen of de afname van de Azure-workload is te wijten aan een piek in het gebruik van de bandbreedte of niet correleren. Vervolgens kunt u acties dienovereenkomstig uitvoeren.

![ExpressRoute-bandbreedtegebruik](media/network-performance-monitor-expressroute/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Volgende stappen
[Zoeken in logboeken](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde gegevens prestatierecords weer te geven.
