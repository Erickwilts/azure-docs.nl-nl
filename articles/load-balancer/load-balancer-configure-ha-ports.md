---
title: Poorten voor hoge beschikbaarheid voor Azure Load Balancer configureren
titlesuffix: Azure Load Balancer
description: Informatie over het gebruik van poorten voor hoge beschikbaarheid voor intern verkeer op alle poorten voor taakverdeling
services: load-balancer
documentationcenter: na
author: rdhillon
manager: narayan
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: ec43b79109181457f8ef8e214e296969db5dcb26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66122349"
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Poorten voor hoge beschikbaarheid configureren voor een interne load balancer

Dit artikel bevat een voorbeeld van een implementatie van poorten voor hoge beschikbaarheid op een interne load balancer. Zie voor meer informatie over specifieke configuraties voor virtuele netwerkapparaten (NVA's), de bijbehorende provider-websites.

>[!NOTE]
>Azure Load Balancer ondersteunt twee verschillende typen: Basic en Standard. Dit artikel worden besproken Standard Load Balancer. Zie voor meer informatie over de Basic Load Balancer [overzicht van Load Balancer](load-balancer-overview.md).

De afbeelding ziet u de volgende configuratie van de voorbeeld-implementatie in dit artikel beschreven:

- De NVA's worden geïmplementeerd in de groep back-end van een interne load balancer achter de configuratie van de poorten voor hoge beschikbaarheid. 
- De gebruiker gedefinieerde route (UDR) toegepast op de routes DMZ-subnet al het verkeer naar de NVA's door de volgende hop als de interne load balancer virtueel IP-adres. 
- De interne load balancer verdeelt het verkeer naar een van de actieve NVA's op basis van de load balancer-algoritme.
- De NVA verwerkt het verkeer en deze doorstuurt naar het oorspronkelijke doel in het back-end-subnet.
- Het geretourneerde pad kan dezelfde route overnemen als een bijbehorende UDR is geconfigureerd in de back-end-subnet. 

![Voorbeeld van een implementatie poorten voor hoge beschikbaarheid](./media/load-balancer-configure-ha-ports/haports.png)

## <a name="configure-high-availability-ports"></a>Poorten voor hoge beschikbaarheid configureren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het configureren van poorten voor hoge beschikbaarheid, instellen van een interne load balancer met de NVA's in de back-endpool. Instellen van een bijbehorende load balancer health test-configuratie voor het detecteren van NVA-status en de load balancer-regel met de poorten voor hoge beschikbaarheid. De algemene load balancer-gerelateerde configuratie wordt beschreven in [aan de slag](load-balancer-get-started-ilb-arm-portal.md). In dit artikel ziet u de configuratie van de poorten voor hoge beschikbaarheid.

De configuratie in feite omvat het instellen van de front-endpoort en de waarde van de back-end-poort naar **0**. Stel de protocolwaarde op **alle**. In dit artikel wordt beschreven hoe u poorten voor hoge beschikbaarheid configureren met behulp van de Azure portal, PowerShell en Azure CLI.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Een load balancer-regel van poorten voor hoge beschikbaarheid configureren met de Azure-portal

Voor het configureren van poorten voor hoge beschikbaarheid met behulp van Azure portal, selecteert u de **HA-poorten in** selectievakje. Als u selecteert, worden de gerelateerde configuratie voor poort en protocol wordt automatisch gevuld. 

![Configuratie van de poorten voor hoge beschikbaarheid via Azure portal](./media/load-balancer-configure-ha-ports/haports-portal.png)

### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Regel voor een load-balancing van poorten voor hoge beschikbaarheid via het Resource Manager-sjabloon configureren

U kunt poorten voor hoge beschikbaarheid configureren met behulp van de API-versie 2017-08-01 voor Microsoft.Network/loadBalancers in de Load Balancer-resource. De volgende JSON-fragment ziet u de wijzigingen in de load balancer-configuratie voor de poorten voor hoge beschikbaarheid via de REST-API:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>Een load balancer-regel van poorten voor hoge beschikbaarheid configureren met PowerShell

Gebruik de volgende opdracht op de poorten voor hoge beschikbaarheid load balancer-regel maken terwijl u de interne load balancer met PowerShell maken:

```powershell
lbrule = New-AzLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli"></a>Een load balancer-regel van poorten voor hoge beschikbaarheid configureren met Azure CLI

In stap 4 van [maken van een interne load balancer-set](load-balancer-get-started-ilb-arm-cli.md), gebruik de volgende opdracht om te maken van de poorten voor hoge beschikbaarheid load balancer-regel:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [poorten voor hoge beschikbaarheid](load-balancer-ha-ports-overview.md).