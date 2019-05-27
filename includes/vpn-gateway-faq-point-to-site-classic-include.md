---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/06/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0c0ad6ea5a687d066c78533b45a7f531561661bf
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66157678"
---
Deze veelgestelde vragen zijn van toepassing op P2S-verbindingen die gebruikmaken van het klassieke implementatiemodel.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Welke clientbesturingssystemen kan ik met punt-naar-site gebruiken?

De volgende clientbesturingssystemen worden ondersteund:

* Windows 7 (32-bits en 64-bits)
* Windows Server 2008 R2 (alleen 64-bits)
* Windows 8 (32-bits en 64-bits)
* Windows 8.1 (32-bits en 64-bits)
* Windows Server 2012 (alleen 64-bits)
* Windows Server 2012 R2 (alleen 64-bits)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-that-supports-sstp-for-point-to-site"></a>Kan ik voor punt-naar-site elke VPN-softwareclient gebruiken die SSTP ondersteunt?

Nee. Ondersteuning is beperkt tot de vermelde versies van het Windows-besturingssysteem.

### <a name="how-many-vpn-client-endpoints-can-exist-in-my-point-to-site-configuration"></a>Hoeveel VPN-clienteindpunten kan mijn punt-naar-site-configuratie hebben?

Er kunnen maximaal 128 VPN-clients tegelijk met een virtueel netwerk worden verbonden.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basis-CA voor een punt-naar-site-verbinding gebruiken?

Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog maximaal twintig basiscertificaten uploaden.

### <a name="can-i-traverse-proxies-and-firewalls-by-using-point-to-site"></a>Kan ik met punt-naar-site proxy's en firewalls passeren?

Ja. SSTP (Secure Socket Tunneling Protocol) wordt gebruikt om firewalls te passeren. Deze tunnel wordt weergegeven als een HTTPS-verbinding.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-site opnieuw start, wordt de VPN-verbinding dan automatisch opnieuw tot stand gebracht?

Standaard wordt de VPN-verbinding niet automatisch op de clientcomputer hersteld.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt punt-naar-site ondersteuning voor automatisch opnieuw verbinding maken en DDNS op de VPN-clients?

Nee. Automatisch opnieuw verbinding maken en DDNS worden momenteel niet ondersteund in VPN’s met punt-naar-site-verbinding.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-for-the-same-virtual-network"></a>Kunnen site-naar-site- en punt-naar-site-configuraties in hetzelfde virtuele netwerk worden gebruikt?

Ja. Beide oplossingen werken als de gateway een op RouteBased-type VPN heeft. Voor het klassieke implementatiemodel hebt u een dynamische gateway nodig. Punt-naar-site wordt niet ondersteund voor VPN-gateways met statische routering of gateways die gebruikmaken van de cmdlet **-VpnType PolicyBased**.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-site-client configureren om verbinding te maken met meerdere virtuele netwerken tegelijk?

Ja. De virtuele netwerken kunnen echter geen overlappende IP-voorvoegsels hebben en de punt-naar-site-adresruimten tussen de virtuele netwerken mogen elkaar niet overlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via site-naar-site- of punt-naar-site-verbindingen?

Het is moeilijk om de exacte doorvoer van de VPN-tunnels te onderhouden. IPSec en SSTP zijn cryptografisch zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw locatie en het internet.
