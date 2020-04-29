---
title: IPv6 toevoegen aan een IPv4-toepassing in het virtuele Azure-netwerk-Azure CLI
titlesuffix: Azure Virtual Network
description: In dit artikel wordt beschreven hoe u IPv6-adressen implementeert voor een bestaande toepassing in een virtueel Azure-netwerk met behulp van Azure CLI.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: f3f9b32ea55f0ceebf08b22ccc7e2ceec0b6227e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80420793"
---
# <a name="add-ipv6-to-an-ipv4-application-in-azure-virtual-network---azure-cli"></a>IPv6 toevoegen aan een IPv4-toepassing in het virtuele Azure-netwerk-Azure CLI

Dit artikel laat u zien hoe u IPv6-adressen kunt toevoegen aan een toepassing die gebruikmaakt van een openbaar IP-adres in een Azure-netwerk voor een Standard Load Balancer met behulp van Azure CLI. De in-place upgrade bevat een virtueel netwerk en subnet, een Standard Load Balancer met IPv4 + IPV6-front-end configuraties, Vm's met Nic's met een IPv4 + IPv6-configuratie, netwerk beveiligings groep en open bare Ip's.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om Azure CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) voor installatie- of upgrade-informatie.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een Standard Load Balancer hebt geïmplementeerd zoals beschreven in [Quick Start: een Standard Load Balancer-Azure cli maken](../load-balancer/quickstart-load-balancer-standard-public-cli.md).

## <a name="create-ipv6-addresses"></a>IPv6-adressen maken

Maak een openbaar IPv6-adres met behulp van [AZ Network Public-IP Create](/cli/azure/network/public-ip) voor uw Standard Load Balancer. In het volgende voor beeld wordt een openbaar IP-adres voor IPv6 gemaakt met de naam *PublicIP_v6* in de resource groep *myResourceGroupSLB* :

```azurecli
  
az network public-ip create \
--name PublicIP_v6 \
--resource-group MyResourceGroupSLB \
--location EastUS \
--sku Standard \
--allocation-method static \
--version IPv6
```

## <a name="configure-ipv6-load-balancer-frontend"></a>IPv6 load balancer-front-end configureren

Configureer de load balancer met het nieuwe IPv6 IP-adres met [AZ Network lb frontend-IP Create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create) als volgt:

```azurecli
az network lb frontend-ip create \
--lb-name myLoadBalancer \
--name dsLbFrontEnd_v6 \
--resource-group MyResourceGroupSLB \
--public-ip-address PublicIP_v6
```

## <a name="configure-ipv6-load-balancer-backend-pool"></a>IPv6-load balancer back-end-pool configureren

Maak de back-end-groep voor Nic's met IPv6-adressen met behulp van [AZ Network lb address-pool Create](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) als volgt:

```azurecli
az network lb address-pool create \
--lb-name myLoadBalancer \
--name dsLbBackEndPool_v6 \
--resource-group MyResourceGroupSLB
```

## <a name="configure-ipv6-load-balancer-rules"></a>IPv6-load balancer regels configureren

Maak IPv6-load balancer regels met [AZ Network lb Rule Create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create).

```azurecli
az network lb rule create \
--lb-name myLoadBalancer \
--name dsLBrule_v6 \
--resource-group MyResourceGroupSLB \
--frontend-ip-name dsLbFrontEnd_v6 \
--protocol Tcp \
--frontend-port 80 \
--backend-port 80 \
--backend-pool-name dsLbBackEndPool_v6
```

## <a name="add-ipv6-address-ranges"></a>IPv6-adresbereiken toevoegen

Voeg als volgt IPv6-adresbereiken toe aan het virtuele netwerk en het subnet dat als host fungeert voor de load balancer:

```azurecli
az network vnet update \
--name myVnet  `
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/16"  "ace:cab:deca::/48"

az network vnet subnet update \
--vnet-name myVnet \
--name mySubnet \
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/24"  "ace:cab:deca:deed::/64"  
```

## <a name="add-ipv6-configuration-to-nics"></a>IPv6-configuratie toevoegen aan Nic's

Configureer de VM Nic's met een IPv6-adres met [AZ Network NIC IP-config Create](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create) als volgt:

```azurecli
az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name myNicVM1 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC2 \
--nic-name myNicVM2 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

az network nic ip-config create \
--name dsIp6Config_NIC3 \
--nic-name myNicVM3 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Virtueel IPv6-netwerk met dubbele stack in Azure Portal weer geven
U kunt het virtuele IPv6-netwerk met dubbele stack als volgt weer geven in Azure Portal:
1. Voer in de zoek balk van de portal *myVnet*in.
2. Wanneer **myVnet** wordt weer gegeven in de zoek resultaten, selecteert u dit. Hiermee opent u de **overzichts** pagina van het virtuele netwerk met dubbele stack met de naam *myVNet*. Het virtuele netwerk met dubbele stack toont de drie Nic's met zowel IPv4-als IPv6-configuraties die zich bevinden in het dubbele stack-subnet met de naam *mySubnet*.

  ![Virtueel IPv6-netwerk met dubbele stack in azure](./media/ipv6-add-to-existing-vnet-powershell/ipv6-dual-stack-vnet.png)


## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) gebruiken om de resourcegroep, de VM en alle gerelateerde resources te verwijderen wanneer u ze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyAzureResourceGroupSLB
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een bestaande Standard Load Balancer bijgewerkt met een IPv4-frontend-IP-configuratie naar een configuratie met dubbele stack (IPv4 en IPv6). U hebt ook IPv6-configuraties toegevoegd aan de Nic's van de virtuele machines in de back-endadresgroep. Zie [Wat is IPv6 voor azure Virtual Network?](ipv6-overview.md) voor meer informatie over IPv6-ondersteuning in azure Virtual Networks.
