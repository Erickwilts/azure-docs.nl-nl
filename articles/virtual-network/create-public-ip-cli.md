---
title: Een openbaar IP-adres maken-Azure CLI
description: Meer informatie over het maken van een openbaar IP-adres met behulp van Azure CLI
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: d868a2d9bc88be7faea161779c35110f13e2b2ac
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2020
ms.locfileid: "91939050"
---
# <a name="quickstart-create-a-public-ip-address-using-azure-cli"></a>Snelstartgids: een openbaar IP-adres maken met behulp van Azure CLI

In dit artikel wordt beschreven hoe u een open bare IP-adres bron maakt met behulp van Azure CLI. Zie [open bare IP-adressen](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses)voor meer informatie over de resources waaraan dit kan worden gekoppeld, het verschil tussen de basis-en standaard-SKU en andere gerelateerde informatie.  In dit voor beeld richten we zich alleen op IPv4-adressen. Zie voor meer informatie over IPv6-adressen [IPv6 voor Azure VNet](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).

## <a name="prerequisites"></a>Vereisten

- Azure CLI lokaal geïnstalleerd of Azure Cloud Shell

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Maak een resource groep met [AZ Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) named **myResourceGroup** in de **eastus2** -locatie.

```azurecli-interactive
  az group create \
    --name myResourceGroup \
    --location eastus2
```
---
# <a name="standard-sku---using-zones"></a>[**Standaard-SKU-zones gebruiken**](#tab/option-create-public-ip-standard-zones)

>[!NOTE]
>De volgende opdracht werkt voor API-versie 2020-08-01 of hoger.  Raadpleeg [resource providers en-typen](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)voor meer informatie over de API-versie die momenteel wordt gebruikt.

Gebruik [AZ Network Public-IP Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) om een standaard zone-REDUNDANT openbaar IP-adres te maken met de naam **myStandardZRPublicIP** in **myResourceGroup**.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardZRPublicIP \
    --sku Standard
    --zone 1,2,3
```
> [!IMPORTANT]
> Voor versies van de API ouder dan 2020-08-01 voert u de bovenstaande opdracht uit zonder een zone parameter op te geven om een zone-redundant IP-adres te maken. 
>

Gebruik de volgende opdracht om een openbaar IP-adres voor de standaard zonegebonden te maken in Zone 2 met de naam **myStandardZonalPublicIP** in **myResourceGroup**:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myStandardZonalPublicIP \
    --sku Standard \
    --zone 2
```

Houd er rekening mee dat de bovenstaande opties voor zones alleen geldige selecties in regio's met [Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)zijn.

# <a name="standard-sku---no-zones"></a>[**Standaard-SKU-geen zones**](#tab/option-create-public-ip-standard)

>[!NOTE]
>De volgende opdracht werkt voor API-versie 2020-08-01 of hoger.  Raadpleeg [resource providers en-typen](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)voor meer informatie over de API-versie die momenteel wordt gebruikt.

Gebruik [AZ Network Public-IP Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) om een standaard openbaar IP-adres te maken als een niet-zonegebonden resource met de naam **myStandardPublicIP** in **myResourceGroup**.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardPublicIP \
    --sku Standard
```
Deze selectie is geldig in alle regio's en is de standaard selectie voor standaard open bare IP-adressen in regio's zonder [Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones).

# <a name="basic-sku"></a>[**Basis-SKU**](#tab/option-create-public-ip-basic)

Gebruik [AZ Network Public-IP Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) om een eenvoudig, statisch openbaar IP-adres te maken met de naam **myBasicPublicIP** in **myResourceGroup**.  Algemene open bare Ip's hebben niet het concept van beschikbaarheids zones.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myBasicPublicIP \
    --sku Basic \
    --allocation-method Static
```
Als het IP-adres acceptabel is om na verloop van tijd te wijzigen, kan de **dynamische** IP-toewijzing worden geselecteerd door de toewijzings methode te wijzigen in dynamisch.

---

## <a name="additional-information"></a>Aanvullende informatie 

Zie [open bare IP-adressen beheren](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address)voor meer informatie over de afzonderlijke variabelen die hierboven worden vermeld.

## <a name="next-steps"></a>Volgende stappen
- Een [openbaar IP-adres koppelen aan een virtuele machine](https://docs.microsoft.com/azure/virtual-network/associate-public-ip-address-vm#azure-portal).
- Meer informatie over [open bare IP-adressen](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) in Azure.
- Meer informatie over alle [open bare IP-adres instellingen](virtual-network-public-ip-address.md#create-a-public-ip-address).
