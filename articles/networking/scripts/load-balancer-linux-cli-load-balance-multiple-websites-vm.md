---
title: Azure CLI-voorbeeldscript - taakverdeling instellen voor meerdere websites met de Azure CLI | Microsoft Docs
description: Azure CLI-voorbeeldscript - taakverdeling instellen voor meerdere websites op de virtuele machine
services: load-balancer
documentationcenter: load-balancer
author: KumudD
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: e3dc9476d188382db31b03b37b2a23affc61aed3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60564881"
---
# <a name="load-balance-multiple-websites"></a>Taakverdeling instellen voor meerdere websites

Met dit voorbeeldscript maakt een virtueel netwerk met twee virtuele machines (VM) die lid van een beschikbaarheidsset zijn. Een load balancer stuurt verkeer voor twee afzonderlijke IP-adressen door naar de twee virtuele machines. Nadat het script is uitgevoerd, kunt u webserversoftware implementeren op de virtuele machines en meerdere websites hosten, elk met een eigen IP-adres.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het maken van een resourcegroep, een virtueel netwerk, een load balancer en alle gerelateerde resources. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Hiermee maakt u een virtueel Azure-netwerk en -subnet. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) | Hiermee maakt u een openbaar IP-adres met een statisch IP-adres en een bijbehorende DNS-naam. |
| [az network lb create](https://docs.microsoft.com/cli/azure/network/lb) | Hiermee maakt u een Azure-load balancer. |
| [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe) | Hiermee maakt u een load balancer-test. Een load balancer-test wordt gebruikt voor het bewaken van elke virtuele machine in de load balancer-set. Als een virtuele machine niet meer toegankelijk is, wordt verkeer niet meer naar die virtuele machine doorgestuurd. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Hiermee maakt u een load balancer-regel. In dit voorbeeld wordt een regel gemaakt voor poort 80. Wanneer HTTP-verkeer bij de load balancer aankomt, wordt dat doorgestuurd naar poort 80 van een van de virtuele machines in de load balancer-set. |
| [az network lb frontend-ip create](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip) | Hiermee maakt u een front-end-IP-adres voor de load balancer. |
| [az network lb address-pool create](https://docs.microsoft.com/cli/azure/network/lb/address-pool) | Hiermee maakt u een back-endadresgroep. |
| [az network nic create](https://docs.microsoft.com/cli/azure/network/nic) | Hiermee maakt u een netwerkkaart die gekoppeld wordt aan het virtuele netwerk en subnet. |
| [az vm availability-set create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Hiermee maakt u een beschikbaarheidsset. Beschikbaarheidssets waarborgen de uptime van toepassingen door de virtuele machines te verdelen over fysieke resources, zodat als er een fout optreedt, niet de hele set wordt getroffen. |
| [az network nic ip-config create](https://docs.microsoft.com/cli/azure/network/nic/ip-config) | Hiermee maakt u een IP-configuratie. De functie Microsoft.Network/AllowMultipleIpConfigurationsPerNic moet zijn ingeschakeld voor uw abonnement. Er kan maar één configuratie worden aangewezen als de primaire IP-configuratie per NIC, met behulp van de vlag --make-primary. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Hiermee maakt u de virtuele machine en verbindt u deze met de netwerkkaart, het virtuele netwerk, het subnet en de netwerkbeveiligingsgroep. Met deze opdracht geeft u ook de installatiekopie van de virtuele machine op die moet worden gebruikt, samen met beheerdersreferenties.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Hiermee verwijdert u een resourcegroep met inbegrip van alle geneste resources. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Aanvullende CLI-voorbeeldscripts voor netwerken vindt u in de [Documentatie met een overzicht van Azure-netwerken](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
