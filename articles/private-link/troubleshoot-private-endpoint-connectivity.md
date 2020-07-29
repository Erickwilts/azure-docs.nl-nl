---
title: Verbindingsproblemen met het Azure-privé-eindpunt oplossen
description: Stapsgewijze richt lijnen voor het vaststellen van de connectiviteit van privé-eind punten
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: fcc482e6231bbd925fd500a37989052765dede58
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77538531"
---
# <a name="troubleshoot-azure-private-endpoint-connectivity-problems"></a>Verbindingsproblemen met het Azure-privé-eindpunt oplossen

In dit artikel vindt u stapsgewijze instructies voor het valideren en diagnosticeren van uw connectiviteits instellingen voor Azure private endpoint.

Persoonlijk Azure-eind punt is een netwerk interface die u privé en veilig verbindt met een privé koppelings service. Met deze oplossing kunt u uw werk belastingen in azure beveiligen door particuliere verbinding te bieden met uw Azure-service resources vanuit uw virtuele netwerk. Deze oplossing brengt deze services effectief naar uw virtuele netwerk.

Hier volgen de verbindings scenario's die beschikbaar zijn met een persoonlijk eind punt:

- Virtueel netwerk uit dezelfde regio
- Regionaal peered virtuele netwerken
- Wereld wijd gekoppelde virtuele netwerken
- On-premises klant via VPN-of Azure ExpressRoute-circuits

## <a name="diagnose-connectivity-problems"></a>Verbindings problemen vaststellen 

Bekijk deze stappen om te controleren of alle gebruikelijke configuraties naar verwachting problemen oplossen met de installatie van uw persoonlijke eind punt.

1. Bekijk de configuratie van het persoonlijke eind punt door te bladeren door de resource.

    a. Ga naar het **persoonlijke koppelings centrum**.

      ![Persoonlijk koppelings centrum](./media/private-endpoint-tsg/private-link-center.png)

    b. Selecteer **privé-eind punten**in het linkerdeel venster.
    
      ![Privé-eindpunten](./media/private-endpoint-tsg/private-endpoints.png)

    c. Filter en selecteer het persoonlijke eind punt dat u wilt diagnosticeren.

    d. Controleer de informatie over het virtuele netwerk en de DNS.
     - Controleer of de verbindings status is **goedgekeurd**.
     - Controleer of de virtuele machine verbinding heeft met het virtuele netwerk dat als host fungeert voor de persoonlijke eind punten.
     - Controleer of de FQDN-gegevens (kopie) en het persoonlijke IP-adres zijn toegewezen.
    
       ![Virtuele netwerk en DNS-configuratie](./media/private-endpoint-tsg/vnet-dns-configuration.png)
    
1. Gebruik [Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) om te zien of gegevens stromen.

    a. Selecteer op de resource van het persoonlijke eind punt de optie **monitor**.
     - Selecteer **gegevens in** of **uit de gegevens**. 
     - Controleer of de gegevens stroomt wanneer u verbinding probeert te maken met het persoonlijke eind punt. Er wordt een vertraging van ongeveer 10 minuten verwacht.
    
       ![Telemetrie van privé-eind punt controleren](./media/private-endpoint-tsg/private-endpoint-monitor.png)

1.  Gebruik **VM-verbinding oplossen van problemen** met Azure Network Watcher.

    a. Selecteer de client-VM.

    b. Selecteer **verbinding oplossen**en selecteer vervolgens het tabblad **uitgaande verbindingen** .
    
      ![Uitgaande verbindingen Network Watcher-testen](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c. Selecteer **Network Watcher gebruiken voor gedetailleerde verbindings tracering**.
    
      ![Network Watcher verbindings problemen oplossen](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d. Selecteer **testen op FQDN**.
     - Plak de FQDN van de persoonlijke eindpunt resource.
     - Geef een poort op. Gebruik normaal gesp roken 443 voor Azure Storage of Azure Cosmos DB en 1336 voor SQL.

    e. Selecteer **testen**en valideer de test resultaten.
    
      ![Network Watcher-test resultaten](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
1. De DNS-omzetting van de test resultaten moet hetzelfde persoonlijke IP-adres hebben dat aan het persoonlijke eind punt is toegewezen.

    a. Als de DNS-instellingen onjuist zijn, voert u de volgende stappen uit:
     - Als u een privé zone gebruikt: 
       - Zorg ervoor dat het virtuele client-VM-netwerk is gekoppeld aan de privé zone.
       - Controleer of de privé-DNS-zone record bestaat. Als deze niet bestaat, maakt u deze.
     - Als u aangepaste DNS gebruikt:
       - Controleer uw aangepaste DNS-instellingen en controleer of de DNS-configuratie juist is.
       Zie [overzicht van privé-eind punten: DNS-configuratie](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#dns-configuration)voor meer informatie.

    b. Als de verbinding mislukt als gevolg van netwerk beveiligings groepen (Nsg's) of door de gebruiker gedefinieerde routes:
     - Controleer de uitgaande regels voor NSG en maak de juiste regels voor uitgaande verbindingen om verkeer toe te staan.
    
       ![NSG uitgaande regels](./media/private-endpoint-tsg/nsg-outbound-rules.png)

1. Als de verbinding resultaten heeft gevalideerd, kan het connectiviteits probleem te maken hebben met andere aspecten zoals geheimen, tokens en wacht woorden op de toepassingslaag.
   - In dit geval controleert u de configuratie van de persoonlijke koppelings resource die aan het persoonlijke eind punt is gekoppeld. Zie voor meer informatie de Azure-oplossing voor het [oplossen van problemen met persoonlijke koppelingen](troubleshoot-private-link-connectivity.md).

1. Neem contact op met het [ondersteunings](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) team van Azure als uw probleem nog steeds niet is opgelost en er nog een probleem is met de verbinding.

## <a name="next-steps"></a>Volgende stappen

 * [Een persoonlijk eind punt maken op het bijgewerkte subnet (Azure Portal)](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)
 * [Probleemoplossings gids voor persoonlijke koppelingen van Azure](troubleshoot-private-link-connectivity.md)
