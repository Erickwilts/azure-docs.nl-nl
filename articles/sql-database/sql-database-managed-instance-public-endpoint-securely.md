---
title: Open bare eind punten van beveiligd beheerd exemplaar-Azure SQL Database beheerd exemplaar | Microsoft Docs
description: Open bare eind punten veilig gebruiken in azure met een beheerd exemplaar
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/08/2019
ms.openlocfilehash: c7f57a636e95bb137dd4285b8f9ce8343b27d2a0
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567373"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Een Azure SQL Database beheerd exemplaar veilig met open bare eind punten gebruiken

Azure SQL Database beheerde instanties kunnen gebruikers connectiviteit bieden via [open bare eind punten](../virtual-network/virtual-network-service-endpoints-overview.md). In dit artikel wordt uitgelegd hoe u deze configuratie beter kunt beveiligen.

## <a name="scenarios"></a>Scenario's

Een SQL Database Managed instance biedt een persoonlijk eind punt om connectiviteit binnen het virtuele netwerk toe te staan. De standaard optie is om maximale isolatie te bieden. Er zijn echter scenario's waarin u een open bare eindpunt verbinding moet opgeven:

- Het beheerde exemplaar moet worden geïntegreerd met PaaS-aanbiedingen (platform-as-a-Service) met meerdere tenants.
- U hebt een hogere door Voer van gegevens uitwisseling nodig dan mogelijk is wanneer u een VPN gebruikt.
- Met het bedrijfs beleid wordt PaaS in bedrijfs netwerken niet toegestaan.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Een beheerd exemplaar implementeren voor toegang tot open bare eind punten

Hoewel dit niet verplicht is, is het algemene implementatie model voor een beheerd exemplaar met open bare eindpunt toegang het exemplaar te maken in een speciaal geïsoleerd virtueel netwerk. In deze configuratie wordt het virtuele netwerk alleen gebruikt voor isolatie van virtuele clusters. Het maakt niet uit of de IP-adres ruimte van het beheerde exemplaar overlapt met de IP-adres ruimte van een bedrijfs netwerk.

## <a name="secure-data-in-motion"></a>Gegevens in beweging beveiligen

Gegevens verkeer van beheerde exemplaren wordt altijd versleuteld als het client stuur programma versleuteling ondersteunt. Gegevens die worden verzonden tussen het beheerde exemplaar en andere virtuele machines van Azure of Azure-Services, verlaten nooit de backbone van Azure. Als er een verbinding is tussen het beheerde exemplaar en een on-premises netwerk, raden we u aan Azure ExpressRoute te gebruiken met micro soft-peering. ExpressRoute helpt u bij het verplaatsen van gegevens via het open bare Internet te voor komen. Voor privé-communicatie met een beheerd exemplaar kan alleen privé-peering worden gebruikt.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Binnenkomende en uitgaande verbindingen vergren delen

In het volgende diagram ziet u de aanbevolen beveiligings configuraties:

![Beveiligings configuraties voor het vergren delen van binnenkomende en uitgaande verbindingen](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Een beheerd exemplaar heeft een [toegewezen openbaar eindpunt adres](sql-database-managed-instance-find-management-endpoint-ip-address.md). Stel in de uitgaande firewall aan de client zijde en in de regels voor de netwerk beveiligings groep het IP-adres van dit open bare eind punt in om de uitgaande connectiviteit te beperken.

Om ervoor te zorgen dat verkeer naar het beheerde exemplaar afkomstig is van betrouw bare bronnen, raden we aan om verbinding te maken vanuit bronnen met bekende IP-adressen. Gebruik een netwerk beveiligings groep om de toegang tot het open bare eind punt van het beheerde exemplaar op poort 3342 te beperken.

Wanneer clients een verbinding vanuit een on-premises netwerk moeten initiëren, moet u ervoor zorgen dat het adres van de bron wordt omgezet in een bekende set IP-adressen. Als u dit niet kunt doen (bijvoorbeeld een mobiel personeel dat een typisch scenario is), raden we u aan om [punt-naar-site-VPN-verbindingen en een persoonlijk eind punt](sql-database-managed-instance-configure-p2s.md)te gebruiken.

Als er verbindingen worden gestart vanuit Azure, raden we aan dat verkeer afkomstig is van een bekend toegewezen [virtueel IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) (bijvoorbeeld een virtuele machine). Als u het beheer van virtuele IP-adressen (VIP) eenvoudiger wilt maken, kunt u gebruikmaken van [open bare IP-adres voorvoegsels](../virtual-network/public-ip-address-prefix.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configureren van een openbaar eind punt voor het beheren van instanties: [Openbaar eind punt configureren](sql-database-managed-instance-public-endpoint-configure.md)
