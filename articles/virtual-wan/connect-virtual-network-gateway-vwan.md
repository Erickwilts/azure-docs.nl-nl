---
title: Een virtuele netwerk gateway verbinden met een virtueel WAN van Azure
description: Dit artikel helpt u bij het verbinden van een virtuele Azure-netwerk gateway aan een Azure Virtual WAN VPN-gateway
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/19/2020
ms.author: cherylmc
ms.openlocfilehash: ca5880f76ffd3a85d4b3cec8e01f58ae5c024a58
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84749699"
---
# <a name="connect-a-vpn-gateway-virtual-network-gateway-to-virtual-wan"></a>Een VPN Gateway (virtuele netwerk gateway) verbinden met een virtueel WAN

Dit artikel helpt u bij het instellen van de connectiviteit vanuit een Azure-VPN Gateway (virtuele netwerk gateway) naar een virtueel WAN van Azure (VPN-gateway). Het maken van een verbinding van een VPN Gateway (virtuele netwerk gateway) naar een virtueel WAN (VPN-gateway) is vergelijkbaar met het instellen van de verbinding met een virtueel WAN vanuit filiaal sites van filialen.

Om mogelijke Verwar ring tussen twee functies tot een minimum te beperken, gaan we de gateway voor de naam van de functie waarnaar we verwijzen. Bijvoorbeeld VPN Gateway virtuele netwerk gateway en virtuele WAN-gateway.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, moet u de volgende resources maken:

Azure Virtual WAN

* [Maak een virtueel WAN](virtual-wan-site-to-site-portal.md#openvwan).
* [Maak een hub](virtual-wan-site-to-site-portal.md#hub). De virtuele hub bevat de virtuele WAN-VPN-gateway.

Azure Virtual Network

* Maak een virtueel netwerk zonder virtuele netwerk gateways. Controleer of geen van de subnetten van uw on-premises netwerken overlapt met de virtuele netwerken waarmee u verbinding wilt maken. Als u een virtueel netwerk in de Azure Portal wilt maken, raadpleegt u [Quick](../virtual-network/quick-create-portal.md)start.

## <a name="1-create-an-azure-virtual-network-gateway"></a><a name="vnetgw"></a>1. een virtuele Azure-netwerk gateway maken

Maak een VPN Gateway virtuele netwerk gateway voor uw virtuele netwerk in de modus actief-actief voor het virtuele netwerk. Wanneer u de gateway maakt, kunt u bestaande open bare IP-adressen gebruiken voor de twee exemplaren van de gateway of u kunt nieuwe open bare Ip's maken. U gebruikt deze open bare IP-adressen bij het instellen van de virtuele WAN-sites. Zie [actieve verbindingen configureren](../vpn-gateway/vpn-gateway-activeactive-rm-powershell.md#aagateway)voor meer informatie over de modus actief-actief.

### <a name="active-active-mode-setting"></a><a name="active-active"></a>Instelling van de modus actief-actief

![actief-actief](./media/connect-virtual-network-gateway-vwan/active.png "actief-actief")

### <a name="bgp-setting"></a><a name="BGP"></a>BGP-instelling

De BGP-ASN kan niet 65515 zijn. 66515 wordt gebruikt door virtuele WAN van Azure.

![BGP](./media/connect-virtual-network-gateway-vwan/bgp.png "BGP")

### <a name="public-ip-addresses"></a><a name="pip"></a>Open bare IP-adressen

Wanneer de gateway is gemaakt, gaat u naar de pagina **Eigenschappen** . De eigenschappen en configuratie-instellingen zijn vergelijkbaar met het volgende voor beeld. Let op de twee open bare IP-adressen die worden gebruikt voor de gateway.

![eigenschappen](./media/connect-virtual-network-gateway-vwan/publicip.png "properties")

## <a name="2-create-virtual-wan-vpn-sites"></a><a name="vwansite"></a>2. virtuele WAN-sites maken

Als u virtuele WAN-sites wilt maken, navigeert u naar uw virtuele WAN en selecteert u onder **connectiviteit**de optie **VPN-sites**. In deze sectie maakt u twee virtuele WAN-VPN-sites die overeenkomen met de virtuele netwerk gateways die u in de vorige sectie hebt gemaakt.

1. Selecteer **+ site maken**.
2. Typ op de pagina **VPN-sites maken** de volgende waarden:

   * **Regio** -(dezelfde regio als de Azure VPN gateway virtuele netwerk gateway)
   * **Leverancier van apparaat** : Voer de leverancier van het apparaat in (een wille keurige naam)
   * **Persoonlijke adres ruimte** : (Voer een waarde in of laat het veld leeg wanneer BGP is ingeschakeld)
   * **Border Gateway Protocol** -(ingesteld om **in te SCHA kelen** als BGP is ingeschakeld voor de Azure VPN gateway virtuele netwerk gateway)
   * **Verbinding maken met hubs** (Selecteer de hub die u hebt gemaakt in de vereisten van de vervolg keuzelijst)
3. Voer onder **koppelingen**de volgende waarden in:

   * **Provider naam** : Voer een naam in voor de koppeling en een provider naam (een wille keurige naam)
   * **Snelheid** snelheid (wille keurig getal)
   * **IP-adres** : Voer het IP-adres in (hetzelfde als het eerste open bare IP-adres dat wordt weer gegeven onder de (VPN gateway) eigenschappen van de virtuele netwerk gateway)
   * **BGP-adres** en **ASN** -BGP-adres en ASN. Deze moeten hetzelfde zijn als een van de BGP-peer-IP-adressen en ASN van de VPN Gateway virtuele netwerk gateway die u in [stap 1](#vnetgw)hebt geconfigureerd.
4. Controleer en selecteer **bevestigen** om de site te maken.
5. Herhaal de vorige stappen om de tweede site te maken zodat deze overeenkomt met het tweede exemplaar van de VPN Gateway virtuele netwerk gateway. U behoudt dezelfde instellingen, met uitzonde ring van het gebruik van het tweede open bare IP-adres en het tweede BGP-peer-IP-adres van VPN Gateway configuratie.
6. U hebt nu twee sites ingericht en kunt door gaan met de volgende sectie om de configuratie bestanden te downloaden.

## <a name="3-download-the-vpn-configuration-files"></a><a name="downloadconfig"></a>3. de VPN-configuratie bestanden downloaden

In deze sectie downloadt u het VPN-configuratie bestand voor elk van de sites die u hebt gemaakt in de vorige sectie.

1. Selecteer boven aan de pagina virtuele WAN **-VPN-sites** de **site**en selecteer vervolgens **site-naar-site-VPN-configuratie downloaden**. Azure maakt een configuratie bestand met de instellingen.

   ![configuratie bestand downloaden](./media/connect-virtual-network-gateway-vwan/download.png "downloaden")
2. Down load en open het configuratie bestand.
3. Herhaal deze stappen voor de tweede site. Zodra u beide configuratie bestanden hebt geopend, kunt u door gaan naar de volgende sectie.

## <a name="4-create-the-local-network-gateways"></a><a name="createlocalgateways"></a>4. de lokale netwerk gateways maken

In deze sectie maakt u twee Azure VPN Gateway lokale netwerk gateways. De configuratie bestanden uit de vorige stap bevatten de configuratie-instellingen van de gateway. Gebruik deze instellingen om de Azure VPN Gateway lokale netwerk gateways te maken en te configureren.

1. Maak de lokale netwerk gateway met behulp van deze instellingen. Voor informatie over het maken van een VPN Gateway lokale netwerk gateway raadpleegt u het VPN Gateway artikel [een lokale netwerk gateway maken](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway).

   * **IP-adres** : gebruik het IP-adres van de Instance0 dat wordt weer gegeven voor *gatewayconfiguration* uit het configuratie bestand.
   * **BGP** : als de verbinding via BGP wordt uitgevoerd, selecteert u **BGP-instellingen configureren** en voert u het ASN ' 65515 ' in. Voer het IP-adres van de BGP-peer in. Gebruik ' Instance0 BgpPeeringAddresses ' voor *gatewayconfiguration* van het configuratie bestand.
   * Het **abonnement, de resource groep en de locatie** zijn hetzelfde als voor de virtuele WAN-hub.
2. Bekijk en maak de lokale netwerk gateway. Uw lokale netwerk gateway moet er ongeveer uitzien als in dit voor beeld.

   ![configuratie bestand downloaden](./media/connect-virtual-network-gateway-vwan/lng1.png "instance0")
3. Herhaal deze stappen voor het maken van een andere lokale netwerk gateway, maar deze keer gebruikt u de ' Exemplaar1-waarden in plaats van ' Instance0-waarden uit het configuratie bestand.

   ![configuratie bestand downloaden](./media/connect-virtual-network-gateway-vwan/lng2.png "exemplaar1")

## <a name="5-create-connections"></a><a name="createlocalgateways"></a>5. verbindingen maken

In deze sectie maakt u een verbinding tussen de VPN Gateway lokale netwerk gateways en de gateway van het virtuele netwerk. Zie [Configure a Connection (een verbinding configureren](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#CreateConnection)) voor stappen voor het maken van een VPN gateway verbinding.

1. Navigeer in de portal naar de gateway van het virtuele netwerk en klik op **verbindingen**. Klik bovenaan de pagina Verbindingen op **+ Toevoegen** om de pagina **Verbinding toevoegen** te openen.
2. Configureer op de pagina **verbinding toevoegen** de volgende waarden voor de verbinding:

   * **Naam:** de naam van de verbinding.
   * **Verbindings type:** **Site-naar-site (IPSec)** selecteren
   * **Virtuele netwerkgateway:** hiervoor geldt een vaste waarde, omdat u verbinding maakt vanaf deze gateway.
   * **Lokale netwerk gateway:** Deze verbinding maakt verbinding tussen de gateway van het virtuele netwerk en de lokale netwerk gateway. Kies een van de lokale netwerk gateways die u eerder hebt gemaakt.
   * **Gedeelde sleutel:** Voer een gedeelde sleutel in.
   * **IKE-protocol:** Kies het IKE-protocol.
   * **BGP:** Kies **BGP inschakelen** als de verbinding zich boven de BGP bevindt.
3. Klik op **OK** om uw verbinding te maken.
4. U kunt de verbinding bekijken op de pagina **Verbindingen** van de virtuele netwerkgateway.

   ![Verbinding](./media/connect-virtual-network-gateway-vwan/connect.png "verbinding")
5. Herhaal de voor gaande stappen om een tweede verbinding te maken. Selecteer voor de tweede verbinding de andere lokale netwerk gateway die u hebt gemaakt.

## <a name="6-test-connections"></a><a name="test"></a>6. verbindingen testen

U kunt de connectiviteit testen door twee virtuele machines te maken, één aan de zijkant van de VPN Gateway virtuele netwerk gateway en één in een virtueel netwerk voor het virtuele WAN. vervolgens pingt u de twee virtuele machines.

1. Maak een virtuele machine in het virtuele netwerk (test1-VNet) voor Azure VPN Gateway (test1-VNG). Maak de virtuele machine niet in de GatewaySubnet.
2. Maak een ander virtueel netwerk om verbinding te maken met het virtuele WAN. Maak een virtuele machine in een subnet van dit virtuele netwerk. Dit virtuele netwerk kan geen virtuele netwerk gateways bevatten. U kunt snel een virtueel netwerk maken met behulp van de Power shell-stappen in het artikel [site-naar-site-verbinding](virtual-wan-site-to-site-portal.md#vnet) . Zorg ervoor dat u de waarden wijzigt voordat u de cmdlets uitvoert.
3. Verbind het VNet met de virtuele WAN-hub. Op de pagina voor uw virtuele WAN selecteert u **virtuele netwerk verbindingen**en vervolgens **+ verbinding toevoegen**. Vul de volgende velden in op de pagina **Verbinding toevoegen**:

    * **Verbindingsnaam** - voer een naam in voor uw verbinding.
    * **Hubs** - selecteer de hub die u wilt koppelen aan deze verbinding.
    * **Abonnement** - controleer of het abonnement klopt.
    * **Virtueel netwerk** - selecteer het virtuele netwerk dat met deze hub wilt verbinden. Het virtuele netwerk mag geen bestaande virtuele netwerkgateway hebben.
4. Klik op **OK** om de virtuele netwerk verbinding te maken.
5. Connectiviteit is nu ingesteld tussen de Vm's. U moet een virtuele machine van de andere kunnen pingen, tenzij er firewalls of andere beleids regels zijn die de communicatie blok keren.

## <a name="next-steps"></a>Volgende stappen

Zie [Configure a Custom IPSec Policy for Virtual WAN](virtual-wan-custom-ipsec-portal.md)(Engelstalig) voor de stappen voor het configureren van een aangepast IPSec-beleid.
Zie [informatie over Azure Virtual WAN](virtual-wan-about.md) en de [Veelgestelde vragen over Azure Virtual WAN](virtual-wan-faq.md)voor meer informatie over Virtual WAN.