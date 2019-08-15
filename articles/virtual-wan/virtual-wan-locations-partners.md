---
title: Locaties van virtuele WAN-partners van Azure | Microsoft Docs
description: Dit artikel bevat een lijst met virtuele WAN-partners en hub-locaties van Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: d9f0bb7d3817b943b71715eb080dd0b060efa025
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990336"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Virtuele WAN-partners en virtuele-hub-locaties

In dit artikel vindt u informatie over regio's en partners die worden ondersteund voor virtuele WANs voor connectiviteit in virtuele hub.

Azure Virtual WAN is een netwerkservice die via Azure voor een geoptimaliseerde en geautomatiseerde verbinding tussen filialen zorgt. Met Virtual WAN kunt u apparaten in filialen verbinden en configureren, zodat deze met Azure kunnen communiceren. Dit kan hand matig worden gedaan of door gebruik te maken van provider apparaten via een virtuele WAN-partner. Door gebruik te maken van partner apparaten kunt u gebruiks gemak, vereenvoudiging van connectiviteit en configuratie beheer gebruiken.

De connectiviteit van het on-premises apparaat wordt op een geautomatiseerde manier ingesteld voor de virtuele hub. Een virtuele hub is een virtueel netwerk dat door micro soft wordt beheerd. De hub bevat verschillende service-eindpunten die verbindingen vanaf uw on-premises netwerk (vpnsite) mogelijk maken. U kunt slechts één hub per regio hebben.

## <a name="automation"></a>Automatisering van connectiviteits partners

Apparaten die verbinding maken met Azure Virtual WAN hebben ingebouwde automatisering om verbinding te maken. Dit wordt meestal ingesteld in de gebruikers interface van het apparaat (of gelijkwaardig), waarmee de connectiviteit en het configuratie beheer tussen het VPN-vertakkings apparaat worden ingesteld op een Azure Virtual hub VPN-eind punt (VPN-gateway).

De volgende automatisering op hoog niveau is ingesteld in de console van het apparaat en het beheer centrum:

* Juiste machtigingen voor het apparaat om toegang te krijgen tot de Azure Virtual WAN-resource groep
* Een vertakkings apparaat uploaden naar een virtueel WAN van Azure
* Automatisch downloaden van Azure-verbindings gegevens
* Configuratie van een on-premises vertakkings apparaat 

Sommige connectiviteits partners kunnen de automatisering uitbreiden om de virtuele Azure-hub VNet en VPN Gateway op te nemen. Als u meer wilt weten over Automation, raadpleegt u [Automation configureren: WAN-partners](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>Connectiviteit via partners

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

De volgende partners zijn voor de nabije toekomst zou op onze route kaart: Arista, F5 Networks, Fortinet, Silver-piek, Velocloud, versa, nuage Nokia.

## <a name="locations"></a>Maplocaties

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie de [Veelgestelde vragen over virtuele WAN](virtual-wan-faq.md)voor meer informatie over Virtual WAN.

* Zie voor meer informatie over het automatiseren van de connectiviteit met Azure Virtual WAN [: virtuele WAN-partners-How to Automatiseer](virtual-wan-configure-automation-providers.md).
