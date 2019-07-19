---
title: Taak verdeling en regels voor uitgaande verbindingen configureren met behulp van Azure CLI
titlesuffix: Azure Load Balancer
description: In dit artikel laat zien hoe het configureren van load balancing en uitgaande regels in een Standard Load Balancer met de Azure CLI.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: allensu
ms.openlocfilehash: 837df78ea76451c7dc5e16efde0e90b780b6ee50
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275709"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-cli"></a>Configureren van taakverdeling en regels voor uitgaand verkeer in de standaardversie van Load Balancer met behulp van Azure CLI

In deze Quick Start laat zien hoe u regels voor uitgaand verkeer in Standard Load Balancer configureren met behulp van Azure CLI.  

Wanneer u klaar bent, de Load Balancer-resource bevat twee front-ends en regels die zijn gekoppeld aan deze: één voor binnenkomende en uitgaande.  Elke front-end bevat een verwijzing naar een openbaar IP-adres en deze scenario maakt gebruik van een ander openbaar IP-adres voor inkomend en uitgaand.   De load balancer-regel biedt alleen binnenkomende load balancing en de uitgaande regel bepaalt de uitgaande NAT opgegeven voor de virtuele machine.  In deze Quick Start worden twee afzonderlijke back-endservers gebruikt, één voor inkomend en één voor uitgaand, om de mogelijkheid te illustreren en flexibiliteit te bieden voor dit scenario.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep maken met [az group create](https://docs.microsoft.com/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Het volgende voorbeeld wordt een resourcegroep met de naam *myresourcegroupoutbound* in de *eastus2* locatie:

```azurecli-interactive
  az group create \
    --name myresourcegroupoutbound \
    --location eastus2
```
## <a name="create-virtual-network"></a>Virtueel netwerk maken
Maak een virtueel netwerk met de naam *myvnetoutbound* met een subnet met de naam *mysubnetoutbound* in de *myresourcegroupoutbound* met behulp van [az network vnet Maak](https://docs.microsoft.com/cli/azure/network/vnet).

```azurecli-interactive
  az network vnet create \
    --resource-group myresourcegroupoutbound \
    --name myvnetoutbound \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mysubnetoutbound
    --subnet-prefix 192.168.0.0/24
```

## <a name="create-inbound-public-ip-address"></a>Inkomende openbare IP-adres maken 

Om toegang te krijgen tot uw web-app op internet, hebt u een openbaar IP-adres nodig voor de load balancer. Een Standard Load Balancer biedt alleen ondersteuning voor standaard, openbare IP-adressen. Gebruik [az network public-ip maken](https://docs.microsoft.com/cli/azure/network/public-ip) te maken van een Standard openbaar IP-adres met de naam *mypublicipinbound* in *myresourcegroupoutbound*.

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipinbound --sku standard
```

## <a name="create-outbound-public-ip-address"></a>Uitgaande openbare IP-adres maken 

Maak een standaard IP-adres voor het gebruik van Load Balancer frontend uitgaande configuratie [az network public-ip maken](https://docs.microsoft.com/cli/azure/network/public-ip).

```azurecli-interactive
  az network public-ip create --resource-group myresourcegroupoutbound --name mypublicipoutbound --sku standard
```

## <a name="create-azure-load-balancer"></a>Azure Load Balancer maken

In deze sectie wordt beschreven hoe u de volgende onderdelen van de load balancer kunt maken en configureren:
  - Een front-end IP-adres dat het binnenkomende netwerkverkeer op de load balancer ontvangt.
  - Een back-end-groep waarbij het frontend-IP-netwerk verkeer met gelijke taak verdeling verzendt.
  - Een back-end-groep voor uitgaande verbindingen. 
  - een statustest die bepaalt de status van de back-end-VM-exemplaren.
  - Een load balancer inkomende regel waarmee wordt gedefinieerd hoe verkeer wordt gedistribueerd naar de virtuele machines.
  - Een load balancer uitgaande regel waarmee wordt gedefinieerd hoe verkeer wordt gedistribueerd naar de VM's.

### <a name="create-load-balancer"></a>Load Balancer maken

Maak een Load Balancer met het inkomende IP-adres met [AZ Network lb Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) named *lb* dat een inkomende frontend-IP-configuratie bevat en een back- *bepoolinbound* die is gekoppeld aan het open bare IP-adres  *mypublicipinbound* die u in de vorige stap hebt gemaakt.

```azurecli-interactive
  az network lb create \
    --resource-group myresourcegroupoutbound \
    --name lb \
    --sku standard \
    --backend-pool-name bepoolinbound \
    --frontend-ip-name myfrontendinbound \
    --location eastus2 \
    --public-ip-address mypublicipinbound   
  ```

### <a name="create-outbound-pool"></a>Uitgaande pool maken

Maak een extra back-end-adres groep om de uitgaande connectiviteit voor een groep Vm's te definiëren met [AZ Network lb address-pool Create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) met de naam *bepooloutbound*.  Het maken van een afzonderlijke uitgaande groep biedt maximale flexibiliteit, maar u kunt deze stap overs Laan en ook alleen de inkomende *bepoolinbound* gebruiken.

```azurecli-interactive
  az network lb address-pool create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name bepooloutbound
```

### <a name="create-outbound-frontend-ip"></a>Uitgaande frontend-IP-adres maken
Maken van de uitgaande frontend-IP-configuratie voor de Load Balancer met [az network lb frontend-ip maken](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) met en uitgaande front-end-IP-configuratie met de naam *myfrontendoutbound* dat wil zeggen dat is gekoppeld aan het openbare IP-adres *mypublicipoutbound*

```azurecli-interactive
  az network lb frontend-ip create \
    --resource-group myresourcegroupoutbound \
    --name myfrontendoutbound \
    --lb-name lb \
    --public-ip-address mypublicipoutbound 
  ```

### <a name="create-health-probe"></a>Statustest maken

Een statuscontrole controleert alle exemplaren van de virtuele machines om ervoor te zorgen dat deze netwerkverkeer kunnen verzenden. Het exemplaar van een virtuele machine met mislukte testcontroles wordt uit de load balancer verwijderd totdat deze weer online komt en een testcontrole bepaalt of deze in orde is. Maak met [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) een statustest om de status van de virtuele machines te bewaken. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myresourcegroupoutbound \
    --lb-name lb \
    --name http \
    --protocol http \
    --port 80 \
    --path /  
```

### <a name="create-load-balancing-rule"></a>Regel voor taakverdeling maken

Een load balancer-regel definieert de front-end-IP-configuratie voor het binnenkomende verkeer en de back endadresgroep voor het ontvangen van het verkeer, samen met de gewenste bron- en doelserver. Maak een load balancer-regel *myinboundlbrule* met [az network lb-regel maken](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) voor het luisteren naar poort 80 in de front-endpool *myfrontendinbound* en verzenden netwerkverkeer met Load Balancing naar de back-endadresgroep *bepool* ook van poort 80. 

>[!NOTE]
>Deze load-balancingregel wordt automatische uitgaande (S) NAT uitgeschakeld als gevolg van deze regel met de--uitschakelen uitgaande snat-parameter. Uitgaande NAT wordt alleen geleverd door de regel voor uitgaande.

```azurecli-interactive
az network lb rule create \
--resource-group myresourcegroupoutbound \
--lb-name lb \
--name inboundlbrule \
--protocol tcp \
--frontend-port 80 \
--backend-port 80 \
--probe http \
--frontend-ip-name myfrontendinbound \
--backend-pool-name bepoolinbound \
--disable-outbound-snat
```

### <a name="create-outbound-rule"></a>Uitgaande regel maken

Een uitgaande regel definieert het front-end openbare IP-adres, vertegenwoordigd door de front-end *myfrontendoutbound*, die wordt gebruikt voor alle uitgaande NAT-verkeer, evenals de back-endpool waarop deze regel van toepassing is.  Maakt een uitgaande regel *myoutboundrule* voor uitgaand netwerkverkeer vertaling van alle virtuele machines (NIC IP-configuraties) in *bepool* back-endpool.  De onderstaande opdracht ook de uitgaande time-out voor inactiviteit wordt gewijzigd van 4 tot 15 minuten en 10000 SNAT poorten in plaats van 1024 toegewezen.  Beoordeling [regels voor uitgaand verkeer](https://aka.ms/lboutboundrules) voor meer informatie.

```azurecli-interactive
az network lb outbound-rule create \
 --resource-group myresourcegroupoutbound \
 --lb-name lb \
 --name outboundrule \
 --frontend-ip-configs myfrontendoutbound \
 --protocol All \
 --idle-timeout 15 \
 --outbound-ports 10000 \
 --address-pool bepooloutbound
```

Als u geen afzonderlijke uitgaande pool wilt gebruiken, kunt u in plaats daarvan het argument adres groep wijzigen in de voor gaande opdracht om *bepoolinbound* op te geven.  We raden u aan om afzonderlijke groepen te gebruiken voor flexibiliteit en lees baarheid van de resulterende configuratie.

U kunt nu door gaan met het toevoegen van uw VM aan de back- *bepoolinbound* __en__ *bepooloutbound* door de IP-configuratie van de betreffende NIC-bronnen bij te werken met [AZ Network NIC IP-config address-pool add](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest).

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep, de load balancer en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurecli-interactive 
  az group delete --name myresourcegroupoutbound
```

## <a name="next-steps"></a>Volgende stappen
In dit artikel standaardversie van Load Balancer gemaakt, zowel binnenkomend verkeersregels voor taakverdeling, geconfigureerd en statustest geconfigureerd voor de virtuele machines in de back-endpool. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
