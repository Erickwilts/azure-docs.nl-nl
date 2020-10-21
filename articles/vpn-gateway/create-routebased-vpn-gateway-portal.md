---
title: 'Een op een route gebaseerde VPN-gateway maken: Portal'
titleSuffix: Azure VPN Gateway
description: Gebruik de Azure Portal om snel een op route gebaseerde Azure VPN-gateway te maken voor een VPN-verbinding met uw on-premises netwerk of om virtuele netwerken te verbinden.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: e24473380fe4d0837518f1616df52621089ff2a5
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2020
ms.locfileid: "92328978"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Een op een route gebaseerde VPN-gateway maken met behulp van de Azure Portal

Dit artikel helpt u bij het snel maken van een op route gebaseerde Azure VPN-gateway met behulp van de Azure Portal.  Een VPN-gateway wordt gebruikt bij het maken van een VPN-verbinding met uw on-premises netwerk. U kunt ook een VPN-gateway gebruiken om verbinding te maken met VNets. 

In de stappen in dit artikel wordt een VNet, een subnet, een gateway-subnet en een op route gebaseerde VPN-gateway (virtuele netwerk gateway) gemaakt. Nadat het maken van de gateway is voltooid, kunt u verbindingen maken. Voor deze stappen is een Azure-abonnement vereist. Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Een virtueel netwerk maken

[!INCLUDE [Cross-premises addresses](../../includes/vpn-gateway-cross-premises.md)]

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="configure-and-create-the-gateway"></a><a name="gwvalues"></a>De gateway configureren en maken

In deze stap maakt u de virtuele netwerkgateway VNet. Het maken van een gateway duurt vaak 45 minuten of langer, afhankelijk van de geselecteerde gateway-SKU.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [Create a gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

>[!NOTE]
>De basis gateway-SKU biedt geen ondersteuning voor IKEv2-of RADIUS-verificatie. Als u van plan bent Mac-clients verbinding te laten maken met uw virtuele netwerk, moet u de basis-SKU niet gebruiken.

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>De VPN-gateway weer geven

1. Nadat de gateway is gemaakt, gaat u naar VNet1 in de portal. De VPN-gateway wordt op de overzichts pagina weer gegeven als een verbonden apparaat.

   ![Verbonden apparaten](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Verbonden apparaten")

2. Klik in de lijst met apparaten op **VNet1GW** om meer informatie weer te geven.

   ![VPN-gateway weer geven](./media/create-routebased-vpn-gateway-portal/view-gateway.png "VPN-gateway weer geven")

## <a name="next-steps"></a>Volgende stappen

Zodra de gateway is gemaakt, kunt u een verbinding maken tussen uw virtuele netwerk en een ander VNet. U kunt ook een verbinding maken tussen uw virtuele netwerk en een on-premises locatie.

> [!div class="nextstepaction"]
> [Een site-naar-site-verbinding maken](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Een punt-naar-site-verbinding maken](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Een verbinding met een ander VNet maken](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
