---
title: 'Een virtueel netwerk koppelen aan een circuit-ExpressRoute: Azure Portal | Microsoft Docs'
description: Een VNet verbinden met een Azure ExpressRoute-Circuit. Procedures.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 1545e2e7eba23b0bedbfe297c292fc94e1659ae3
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775286"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Een virtueel netwerk verbinden met een ExpressRoute-circuit met behulp van de portal
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure-CLI](howto-linkvnet-cli.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-linkvnet-classic.md)
> 

Dit artikel helpt u bij het maken van een verbinding voor een virtueel netwerk koppelen aan een Azure ExpressRoute-circuit met behulp van de Azure-portal. De virtuele netwerken die u verbinding met uw Azure ExpressRoute-circuit maken kunnen in hetzelfde abonnement, of ze kunnen deel uitmaken van een ander abonnement.

## <a name="before-you-begin"></a>Voordat u begint

* Controleer de [vereisten](expressroute-prerequisites.md), [routeringsvereisten](expressroute-routing.md), en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.

* U moet een actief ExpressRoute-circuit hebben.
  * Volg de instructies voor [maken van een ExpressRoute-circuit](expressroute-howto-circuit-portal-resource-manager.md) en laat het circuit inschakelen door de connectiviteitsprovider.
  * Zorg ervoor dat u Azure private peering is geconfigureerd voor uw circuit hebt. Zie de [configureren routering](expressroute-howto-routing-portal-resource-manager.md) artikel voor routeringsinstructies.
  * Zorg ervoor dat de persoonlijke Azure-peering is geconfigureerd en van de BGP-peering tussen uw netwerk en Microsoft is, zodat u end-to-end-connectiviteit kunt inschakelen.
  * Zorg ervoor dat u hebt een virtueel netwerk en een virtuele netwerkgateway gemaakt en volledig is ingericht. Volg de instructies voor [een virtuele netwerkgateway maken voor ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Het GatewayType 'ExpressRoute', niet VPN maakt gebruik van een virtuele netwerkgateway voor ExpressRoute.

* U kunt maximaal 10 virtuele netwerken koppelen aan een standard ExpressRoute-circuit. Alle virtuele netwerken moeten zich in dezelfde geopolitieke regio bij het gebruik van een standard ExpressRoute-circuit.

* Een enkel VNet kan worden gekoppeld aan maximaal vier ExpressRoute-circuits. De procedure hieronder gebruiken om te maken van een nieuw verbindingsobject voor elk ExpressRoute-circuit dat u verbinding maakt. De ExpressRoute-circuits kunnen zich in hetzelfde abonnement, verschillende abonnementen of een combinatie van beide.

* U kunt een virtueel netwerk buiten de geopolitieke regio van het ExpressRoute-circuit koppelen of een groter aantal virtuele netwerken verbinden met uw ExpressRoute-circuit als u de premium-invoegtoepassing voor ExpressRoute hebt ingeschakeld. Controleer de [Veelgestelde vragen over](expressroute-faqs.md) voor meer informatie over de premium-invoegtoepassing.

* U kunt [een video bekijken](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) voordat u begint voor een beter begrip van de stappen.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Een VNet verbinden met een circuit - hetzelfde abonnement

> [!NOTE]
> BGP-configuratie-informatie wordt niet weergegeven als de laag-3-provider uw peerings geconfigureerd. Als uw circuit heeft de ingerichte status, moet u mogelijk zijn om verbindingen te maken.
>

### <a name="to-create-a-connection"></a>Een verbinding te maken

1. Zorg ervoor dat uw ExpressRoute-circuit en de persoonlijke Azure-peering is is geconfigureerd. Volg de instructies in [maken van een ExpressRoute-circuit](expressroute-howto-circuit-arm.md) en [configureren routering](expressroute-howto-routing-arm.md). Uw ExpressRoute-circuit moet er uitzien zoals in de volgende afbeelding:

   ![Schermafbeelding van de ExpressRoute-circuit](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
2. U kunt nu beginnen een verbinding voor het koppelen van uw virtuele netwerkgateway voor uw ExpressRoute-circuit wordt ingericht. Klik op **verbinding** > **toevoegen** openen de **verbinding toevoegen** pagina en configureer vervolgens de waarden.

   ![Verbindingen schermopname toevoegen](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
3. Nadat de verbinding is geconfigureerd, wordt de informatie voor de verbinding weergegeven in het verbindingsobject.

   ![Schermafbeelding van de verbinding-object](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Een VNet verbinden met een circuit - ander abonnement

U kunt een ExpressRoute-circuit delen voor meerdere abonnementen. De afbeelding hieronder ziet een eenvoudige schematische van hoe delen werkt voor ExpressRoute-circuits voor meerdere abonnementen.

![Abonnementoverschrijdende connectiviteit](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Elk van de kleinere clouds binnen de grote cloud wordt gebruikt voor abonnementen die deel uitmaken van verschillende afdelingen binnen een organisatie.
- Elk van de afdelingen binnen de organisatie kan hun eigen abonnement gebruiken voor het implementeren van hun services, maar ze kunnen een enkel ExpressRoute-circuit terugverbinding maken met uw on-premises netwerk delen.
- Eén afdeling (in dit voor beeld: IT) kan eigenaar zijn van het ExpressRoute-circuit. Andere abonnementen binnen de organisatie kunnen gebruiken voor de ExpressRoute-circuit en de machtigingen die zijn gekoppeld aan het circuit, met inbegrip van abonnementen die zijn gekoppeld aan andere Azure Active Directory-tenants en de Enterprise Agreement-inschrijvingen.

  > [!NOTE]
  > Connectiviteit en de bandbreedte kosten in rekening gebracht voor het toegewezen circuit wordt toegepast op de eigenaar van het ExpressRoute-circuit. Alle virtuele netwerken delen de dezelfde bandbreedte.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Beheer - over circuit eigenaren en circuitgebruikers

De circuiteigenaar is een geautoriseerde gebruiker van de kracht van de resource van ExpressRoute-circuit. De circuiteigenaar van het kunt autorisaties maken die kunnen worden ingewisseld door 'circuitgebruikers'. Circuitgebruikers zijn eigenaren van virtuele netwerkgateways die zich niet binnen hetzelfde abonnement bevinden als het ExpressRoute-circuit. Circuitgebruikers kunnen autorisaties willen (één autorisatie per virtueel netwerk) inwisselen.

De circuiteigenaar van het heeft de mogelijkheid om te wijzigen en autorisaties op elk gewenst moment intrekken. Intrekken van de resultaten van een autorisatie in alle koppeling verbindingen wordt verwijderd uit het abonnement waarvan toegang is ingetrokken.

### <a name="circuit-owner-operations"></a>Circuit eigenaar bewerkingen

**Maken van een verbinding-autorisatieregels**

De circuiteigenaar van het maakt een autorisatie. Dit resulteert in het maken van een autorisatiesleutel die door de gebruiker van een circuit kan worden gebruikt om hun virtuele netwerkgateways aan ExpressRoute-circuit. Een autorisatie is geldig voor slechts één verbinding.

> [!NOTE]
> Voor elke verbinding is een afzonderlijke autorisatie vereist.
>

1. Klik op de pagina ExpressRoute **autorisaties** en typ vervolgens een **naam** voor de autorisatie en klik op **opslaan**.

   ![Autorisaties](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Wanneer de configuratie is opgeslagen, Kopieer de **Resource-ID** en de **Autorisatiesleutel**.

   ![Autorisatiesleutel](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Autorisatie om een verbinding verwijderen**

U kunt een verbinding verwijderen door het selecteren van de **verwijderen** pictogram op de pagina voor de verbinding.

### <a name="circuit-user-operations"></a>Circuit gebruikersbewerkingen

De gebruiker circuit moet de resource-ID en een autorisatiesleutel van de circuiteigenaar van het.

**Een verbinding-autorisatie inwisselen**

1. Klik op de **+ nieuw** knop.

   ![Klik op Nieuw](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Zoeken naar **'Verbinding'** op Marketplace, selecteert u deze en klikt u op **maken**.

   ![Zoeken naar verbinding](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. Zorg ervoor dat de **verbindingstype** is ingesteld op 'ExpressRoute'.
4. Vul de details en klik vervolgens op **OK** op de pagina van de basisprincipes.

   ![Grondbeginselen van pagina](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. In de **instellingen** pagina, selecteer de **gateway van virtueel netwerk** en controleer de **autorisatie inwisselen** selectievakje.
6. Voer de **autorisatiesleutel** en de **URI van Peercircuit** en geef een naam op voor de verbinding. Klik op **OK**. De **URI van het peer circuit** is de resource-id van het ExpressRoute-circuit (dat u kunt vinden onder het instellingen venster Eigenschappen van het ExpressRoute-circuit).

   ![De pagina Instellingen](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. Lees de informatie in de **samenvatting** pagina en klik op **OK**.

**Vrij te geven van autorisatie om een verbinding**

U kunt een autorisatie vrijgeven door het verwijderen van de verbinding die het ExpressRoute-circuit aan het virtuele netwerk is gekoppeld.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Een verbinding als u wilt ontkoppelen van een VNet verwijderen

U kunt een verbinding verwijderen en uw VNet aan een ExpressRoute-circuit ontkoppelen door het selecteren van de **verwijderen** pictogram op de pagina voor de verbinding.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
