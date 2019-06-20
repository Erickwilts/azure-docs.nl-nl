---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/24/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 2d84a905cba503119f1b6e0f0a1a7cbbf91b3a1f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175900"
---
1. Klik in de portal aan de linkerkant op **Een resource maken** en typ in het zoekvak: 'virtuele netwerkgateway'. Klik op het zoekresultaat **Gateway van het virtuele netwerk**. Op de **gateway van virtueel netwerk** pagina, klikt u op **maken** aan de onderkant van de pagina. Hiermee opent u de pagina **Gateway van het virtuele netwerk maken**.
2. Vul op de pagina **Virtuele netwerkgateway maken** de waarden in voor uw virtuele netwerkgateway.

   ![Velden van de pagina Virtuele netwerkgateway maken](./media/vpn-gateway-add-gw-p2s-rm-portal-include/p2sgw.png "Velden van de pagina Virtuele netwerkgateway maken")
3. Vul op de pagina **Virtuele netwerkgateway maken** de waarden in voor de gateway van het virtuele netwerk.

   - **Naam**: Naam van uw gateway. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. Het is de naam van het gateway-object dat u maakt.
   - **Gatewaytype**: Selecteer **VPN**. VPN-gateways maken gebruik van een gateway van het virtuele netwerk van het type **VPN**. 
   - **VPN-type**: Selecteer het VPN-type dat is opgegeven voor uw configuratie. De meeste configuraties vereisen een op route gebaseerd VPN-type.
   - **SKU**: Selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.

     Selecteer **Modus actief-actief inschakelen** alleen als u een gatewayconfiguratie van het type actief-actief maakt. Anders selecteert u deze instelling niet.
   - **Locatie**: Mogelijk moet u schuiven om locatie te zien. Wijzig het veld **Locatie** om naar de locatie van het virtuele netwerk te verwijzen. Bijvoorbeeld: US - west. Als de locatie niet verwijst naar de regio waarin het virtuele netwerk zich bevindt, wordt het virtuele netwerk niet weergegeven in de vervolgkeuzelijst als u in de volgende stap een virtueel netwerk kiest.
   - **Virtueel netwerk**: Kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **Virtueel netwerk** om de pagina Een virtueel netwerk kiezen te openen. Selecteer het VNet. Als u uw VNet niet ziet, moet u controleren of het veld Locatie verwijst naar de regio waarin het virtuele netwerk zich bevindt.
   - **Adresbereik gatewaysubnet**: Deze instelling wordt alleen weergegeven als u de een gatewaysubnet voor het virtuele netwerk niet op de eerder hebt gemaakt. Als u al een geldig gatewaysubnet hebt gemaakt, wordt deze instelling niet weergegeven.
   - **Openbaar IP-adres**: Deze instelling bepaalt u het openbare IP-adres-object, dat gekoppeld aan de VPN-gateway wordt. Het openbare IP-adres wordt dynamisch toegewezen aan dit object wanneer de VPN-gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

     - Laat **Nieuwe maken** geselecteerd.
     - Typ in het tekstvak een **naam** voor uw openbare IP-adres.

4. Selecteer **ASN van BGP configureren** niet, tenzij deze instelling voor uw configuratie specifiek vereist is. Als u deze instelling wel nodig hebt, is de ASN standaard 65515. U kunt deze waarde wijzigen.
5. Controleer de instellingen. Als u wilt dat uw gateway op het dashboard wordt weergegeven, kunt u onderaan de pagina **Vastmaken aan dashboard** selecteren.
6. Klik op **Maken** om de VPN-gateway aan te maken. De instellingen worden gevalideerd en op het dashboard wordt de tegel 'De gateway van het virtuele netwerk implementeren' weergegeven. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

Nadat de gateway is gemaakt, gaat u na welk IP-adres eraan is toegewezen. Bekijk hiervoor het virtuele netwerk in de portal. De gateway wordt weergegeven als verbonden apparaat. U kunt op het verbonden apparaat klikken (uw gateway van het virtuele netwerk) om meer informatie te laten weergeven.