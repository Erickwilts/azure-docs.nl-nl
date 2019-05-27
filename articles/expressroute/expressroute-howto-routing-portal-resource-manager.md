---
title: 'Configureren voor een circuit - ExpressRoute-peering: Azure | Microsoft Docs'
description: In dit artikel worden de stappen voor het maken en inrichten van ExpressRoute particuliere en Microsoft-peering. In dit artikel ook ziet u hoe u de status bijwerken of verwijderen van peerings voor een circuit.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: f6061710fb15d4183bd42a82c4bd269a69fc9be2
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964437"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Peering voor een ExpressRoute-circuit maken en wijzigen

Dit artikel helpt u bij het maken en beheren van routeringsconfiguratie voor een Azure Resource Manager (ARM) ExpressRoute-circuit via Azure portal. U kunt ook controleren op de status, bijwerken of verwijderen en inrichting van peerings voor een ExpressRoute-circuit ongedaan maken. Als u wilt een andere methode gebruiken om te werken met uw circuit, selecteert u een artikel in de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure-CLI](howto-routing-cli.md)
> * [Video - Privépeering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - openbare peering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-routing-classic.md)
> 

U kunt Azure privé- en Microsoft-peering voor een ExpressRoute-circuit configureren (openbare Azure-peering is afgeschaft voor nieuwe circuits). U kunt peerings configureren in elke gewenste volgorde. U moet er echter wel voor zorgen dat u de configuratie van elke peering een voor een voltooit. Zie voor meer informatie over routering domeinen en peerings [circuits en-peerings](expressroute-circuit-peerings.md).

## <a name="configuration-prerequisites"></a>Configuratievereisten

* Zorg dat u de pagina met [vereisten](expressroute-prerequisites.md), de pagina over [routeringsvereisten](expressroute-routing.md) en de pagina over [werkstromen](expressroute-workflows.md) hebt gelezen voordat u begint met de configuratie.
* U moet een actief ExpressRoute-circuit hebben. Volg de instructies voor het [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider voordat u verder gaat. Als u wilt configureren van peering (s), moet het ExpressRoute-circuit zich in een status ingericht en zijn ingeschakeld. 
* Als u van plan bent een gedeelde sleutel/MD5-hash wilt gebruiken, moet u dit aan beide zijden van de tunnel te gebruiken en het aantal alfanumerieke tekens bevatten tot een maximum van 25. Speciale tekens worden niet ondersteund. 

Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met serviceproviders die services met Laag-2-connectiviteit aanbieden. Als u een serviceprovider die beheerde laag-3-services (meestal een IPVPN, zoals MPLS), uw connectiviteitsprovider configureert en beheert routering voor u. 

> [!IMPORTANT]
> Op dit moment bieden we nog geen peerings aan die door serviceproviders worden geconfigureerd via de beheerportal van de service. Deze mogelijkheid zal binnenkort worden ingeschakeld. Neem contact op met uw serviceprovider voordat u BGP-peerings configureert.
> 
> 

## <a name="msft"></a>Microsoft-peering

In deze sectie helpt u bij het maken, ophalen, bijwerken en verwijderen van de configuratie voor de Microsoft-peering voor een ExpressRoute-circuit.

> [!IMPORTANT]
> Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd vóór 1 augustus 2017 hebben alle service-voorvoegsels geadverteerd via de Microsoft-peering, zelfs als routefilters zijn niet gedefinieerd. Microsoft-peering van ExpressRoute-circuits die zijn geconfigureerd op of na 1 augustus 2017 hebben geen voorvoegsels geadverteerd totdat een routefilter is gekoppeld aan het circuit. Zie voor meer informatie, [een routefilter voor Microsoft-peering configureren](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft-peering maken

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen van Microsoft-peering voor u vragen. In dat geval hoeft u de instructies die worden vermeld in de volgende secties. Echter, als uw connectiviteitsprovider niet wordt beheerd routering voor u, na het maken van uw circuit, doorgaan met de volgende stappen.

   ![lijst met Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer Microsoft-peering voor het circuit. Zorg ervoor dat u over de volgende informatie beschikt voordat u verder gaat.

   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn waarvan u eigenaar bent en dat is geregistreerd in een RIR/IRR. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt. Voor zowel de primaire en secundaire koppelingen moet u de dezelfde VLAN-ID.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * Geadverteerde voorvoegsels: U kunt een lijst van alle voorvoegsels die u van plan bent om te adverteren via de BGP-sessie moet opgeven. Alleen openbare IP-adresvoorvoegsels worden geaccepteerd. Als u van plan bent om een set voorvoegsels te verzenden, kunt u een door komma's gescheiden lijst verzenden. Deze voorvoegsels moeten voor u zijn geregistreerd in een RIR/IRR.
   * **Optioneel:** klant-ASN: Als u voorvoegsels adverteert die niet zijn geregistreerd op de AS-nummer peering, kunt u het AS-nummer waaraan ze zijn geregistreerd.
   * Naam van Routeringsregister: U kunt het RIR / IRR op basis waarvan het AS-nummer en de voorvoegsels zijn geregistreerd.
   * **Optioneel:** een MD5-hash, als u ervoor kiest een te gebruiken.
3. U kunt selecteren om de peering die u configureren, wilt zoals wordt weergegeven in het volgende voorbeeld. Selecteer de rij voor Microsoft-peering.

   ![Selecteer de rij voor Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Configureer Microsoft-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

   ![Microsoft-peering configureren](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven.

   Als uw circuit opgehaald voor een 'validatie vereist' status (zoals weergegeven in de afbeelding), moet u een ondersteuningsticket om aan te tonen dat u eigenaar bent van de voorvoegsels aan ons ondersteuningsteam openen.

   ![Configuratie van de Microsoft-peering opslaan](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

   U kunt een ondersteuningsticket rechtstreeks vanuit de portal, zoals wordt weergegeven in het volgende voorbeeld:

   ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Nadat de configuratie is geaccepteerd, ziet u iets die vergelijkbaar is met de volgende afbeelding:

   ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="getmsft"></a>Details van Microsoft-peering weergeven

Hier vindt u de eigenschappen van Microsoft-peering door de peering te selecteren.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="updatemsft"></a>Configuratie van Microsoft-peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="deletemsft"></a>Microsoft-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram te selecteren zoals wordt weergegeven in de volgende afbeelding:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="private"></a>Persoonlijke Azure-peering

In deze sectie helpt u bij het maken, ophalen, bijwerken en verwijderen van de Azure private peering configuratie voor een ExpressRoute-circuit.

### <a name="to-create-azure-private-peering"></a>Persoonlijke Azure-peering maken

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen persoonlijke Azure-peering voor u vragen. In dat geval hoeft u de instructies die worden vermeld in de volgende secties. Echter, als uw connectiviteitsprovider niet wordt beheerd routering voor u, na het maken van uw circuit, doorgaan met de volgende stappen.

   ![lijst](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer persoonlijke Azure-peering voor het circuit. Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:

   * Een /30-subnet voor de primaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.
   * Een /30-subnet voor de secundaire koppeling. Het subnet moet deel uitmaken van een adresruimte gereserveerd voor virtuele netwerken niet. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt. Voor zowel de primaire en secundaire koppelingen moet u de dezelfde VLAN-ID.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken. U kunt een persoonlijke AS-nummer voor deze peering, met uitzondering van het nummer van 65515 tot 65520, liggen.
   * **Optioneel:** een MD5-hash, als u ervoor kiest een te gebruiken.
3. Selecteer de rij voor persoonlijke Azure-peering, zoals wordt weergegeven in het volgende voorbeeld:

   ![persoonlijk](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Configureer persoonlijke Azure-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

   ![Configureer persoonlijke Azure-peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven. Nadat de configuratie is geaccepteerd, ziet er ongeveer uitzien als in het volgende voorbeeld:

   ![Sla persoonlijke peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="getprivate"></a>Details van een Azure private peering weergeven

U kunt de eigenschappen van persoonlijke Azure-peering weergeven door de peering te selecteren.

![privé-peering weergeven](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="updateprivate"></a>Configuratie van een Azure private peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![privé-peering bijwerken](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="deleteprivate"></a>Persoonlijke Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram te selecteren zoals wordt weergegeven in de volgende afbeelding:

> [!WARNING]
> U moet ervoor zorgen dat alle virtuele netwerken en globale bereiken ExpressRoute-verbindingen worden verwijderd voordat u het volgende voorbeeld uitvoert. 
> 
> 

![privé-peering verwijderen](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="public"></a>Openbare Azure-peering

In deze sectie helpt u bij het maken, ophalen, bijwerken en verwijderen van de Azure openbare peering configuratie voor een ExpressRoute-circuit.

> [!Note]
> Openbare Azure-peering is afgeschaft voor nieuwe circuits. Zie voor meer informatie, [ExpressRoute-peering](expressroute-circuit-peerings.md).
>

### <a name="to-create-azure-public-peering"></a>Openbare Azure-peering maken

1. Configureer het ExpressRoute-circuit. Zorg dat het circuit volledig is ingericht door de connectiviteitsprovider voordat u verder gaat. Als uw connectiviteitsprovider beheerde laag-3-services biedt, kunt u uw connectiviteitsprovider om in te schakelen openbare Azure-peering voor u vragen. In dat geval hoeft u de instructies in de volgende secties niet te volgen. Als uw connectiviteitsprovider niet wordt beheerd routering voor u, na het maken van uw circuit blijven echter de configuratie met behulp van de volgende stappen.

   ![lijst met openbare peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Configureer openbare Azure-peering voor het circuit. Zorg ervoor dat u de volgende items hebt voordat u verdergaat met de volgende stappen:

   * Een /30-subnet voor de primaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt. 
   * Een /30-subnet voor de secundaire koppeling. Dit moet een geldig openbaar IPv4-voorvoegsel zijn. In dit subnet wordt u het eerste bruikbaar IP-adres toewijzen aan uw router, zoals Microsoft het tweede bruikbaar IP-adres voor de router gebruikt.
   * Een geldige VLAN-id waarop u deze peering wilt instellen. Controleer of er geen andere peering in het circuit is die dezelfde VLAN-id gebruikt. Voor zowel de primaire en secundaire koppelingen moet u de dezelfde VLAN-ID.
   * AS-nummer voor peering. U kunt 2-bytes en 4-bytes AS-nummers gebruiken.
   * **Optioneel:** een MD5-hash, als u ervoor kiest een te gebruiken.
3. Selecteer het Azure rij openbare peering, zoals wordt weergegeven in de volgende afbeelding:

   ![Selecteer de rij voor openbare peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Configureer openbare Azure-peering. De volgende afbeelding toont een configuratievoorbeeld van een:

   ![Configureer openbare Azure-peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Sla de configuratie op wanneer u alle parameters hebt opgegeven. Nadat de configuratie is geaccepteerd, ziet er ongeveer uitzien als in het volgende voorbeeld:

   ![Openbare peering-configuratie op te slaan](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="getpublic"></a>Details van een Azure openbare peering weergeven

U kunt de eigenschappen van openbare Azure-peering weergeven door de peering te selecteren.

![openbare peering eigenschappen weergeven](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="updatepublic"></a>Azure-configuratie in openbare peering bijwerken

U kunt de rij voor peering selecteren en de eigenschappen van de peering wijzigen.

![Selecteer de rij voor openbare peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="deletepublic"></a>Openbare Azure-peering verwijderen

U kunt de peeringconfiguratie verwijderen door het verwijderingspictogram te selecteren, zoals wordt weergegeven in het volgende voorbeeld:

![openbare peering verwijderen](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="next-steps"></a>Volgende stappen

Volgende stap, [een VNet koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-portal-resource-manager.md)
* Voor meer informatie over ExpressRoute-werkstromen raadpleegt u [ExpressRoute workflows](expressroute-workflows.md) (ExpressRoute-werkstromen).
* Voor meer informatie over circuitpeering raadpleegt u [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (ExpressRoute-circuits en -routeringsdomeinen).
* Bekijk het [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Virtual Network-overzicht) voor meer informatie over het gebruik van virtuele netwerken.
