---
title: Beheren van routering in een klassiek Virtueelnetwerk van Azure - CLI - | Microsoft Docs
description: Meer informatie over het beheren van routering in vnet's met de Azure CLI in het klassieke implementatiemodel
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: e1b8bb3544a08b60564ceb5bd7e1666214059e09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60743918"
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Beheren van Routering en gebruik van virtuele apparaten zijn met de Azure CLI (klassiek)

> [!div class="op_single_selector"]
> * [PowerShell](tutorial-create-route-table-powershell.md)
> * [Azure-CLI](tutorial-create-route-table-cli.md)
> * [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
> * [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [beheren van Routering en het gebruik van virtuele apparaten in het Resource Manager-implementatiemodel](tutorial-create-route-table-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het voorbeeld van Azure CLI-opdrachten onderstaande verwachten dat een eenvoudige omgeving al gemaakt op basis van bovenstaand scenario. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in [maken van een VNet (klassiek) met de Azure CLI](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor het front-end-subnet maken
Voor het maken van de routetabel en route die nodig zijn voor de front-end-subnet op basis van de bovenstaande scenario, de volgende stappen uit te voeren.

1. Voer de volgende opdracht uit om over te schakelen naar de klassieke modus:

    ```azurecli
    azure config mode asm
    ```

    Uitvoer:

        info:    New mode is asm

2. Voer de volgende opdracht om te maken van een routetabel voor de front-end-subnet:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Uitvoer:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Parameters:
   
   * **-l (of --locatie)** . Azure-regio waar de nieuwe Netwerkbeveiligingsgroep wordt gemaakt. In ons scenario *westus*.
   * **-n (of --naam)** . Naam voor de nieuwe Netwerkbeveiligingsgroep. In ons scenario *NSG-FrontEnd*.
3. Voer de volgende opdracht om te maken van een route in de routetabel voor het verzenden van al het verkeer dat bestemd is voor de back-end-subnet (192.168.2.0/24) naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Uitvoer:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Parameters:
   
   * **-r (of--route-table-naam)** . De naam van de routetabel waarin de route wordt toegevoegd. In ons scenario *UDR-FrontEnd*.
   * **-a (of --adresvoorvoegsel)** . Het adresvoorvoegsel voor het subnet waarin de pakketten zijn bestemd is voor. In ons scenario *192.168.2.0/24*.
   * **-t (of--volgende hoptype)** . Type object verkeer wordt verzonden naar. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, of *geen*.
   * **-p (of--volgende-hop-ip-adres**). IP-adres van volgende hop. In ons scenario *192.168.0.4*.
4. Voer de volgende opdracht om te koppelen van de routetabel gemaakt met de **FrontEnd** subnet:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Uitvoer:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Parameters:
   
   * **-t (of--vnet naam)** . Naam van de VNet waar het subnet zich bevindt. In ons scenario *TestVNet*.
   * **-n (of--subnet-name**. De naam van het subnet in dat de routetabel wordt toegevoegd aan. In ons scenario *FrontEnd*.

## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor het back-end-subnet maken
Voor het maken van de routetabel en route die nodig zijn voor de back-end-subnet op basis van het scenario, moet u de volgende stappen uitvoeren:

1. Voer de volgende opdracht om te maken van een routetabel voor de back-end-subnet:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. Voer de volgende opdracht om te maken van een route in de routetabel voor het verzenden van al het verkeer dat bestemd is voor de front-end-subnet (192.168.1.0/24) naar de **FW1** VM (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Voer de volgende opdracht om te koppelen van de routetabel, met de **back-end** subnet:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

