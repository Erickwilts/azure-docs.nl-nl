---
title: 'Snelstart: Een load balancer van het type Basic maken - Azure CLI'
titlesuffix: Azure Load Balancer
description: In deze snelstart vindt u meer informatie over het maken van een openbare load balancer met Azure CLI
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
tags: azure-resource-manager
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.custom: mvc
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2019
ms.author: kumud
ms.openlocfilehash: a947b5983f4a7d58878fc15785cfbcbcbba7f226
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66156209"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-azure-cli"></a>Snelstart: Een load balancer maken om taken van VM's te verdelen met behulp van Azure CLI

Hierin leest u over het maken van een Azure Load Balancer om internetverkeer te verdelen tussen virtuele machines in Azure. U test de load balancer door twee virtuele machines (VM's) te implementeren waarop een Ubuntu-server wordt uitgevoerd en waartussen de taken van een web-app worden verdeeld.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep maken met [az group create](https://docs.microsoft.com/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupLB* gemaakt op de locatie *VS Oost*:

```azurecli-interactive
  az group create \
    --name myResourceGroupLB \
    --location eastus
```

## <a name="create-a-public-ip-address"></a>Een openbaar IP-adres maken

Om toegang te krijgen tot uw web-app op internet, hebt u een openbaar IP-adres nodig voor de load balancer. Gebruik [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip) om in *myResourceGroupLB* een openbaar IP-adres te maken met de naam *myPublicIP*.

```azurecli-interactive
  az network public-ip create --resource-group myResourceGroupLB --name myPublicIP
```

## <a name="create-azure-load-balancer"></a>Azure-load balancer maken

In deze sectie wordt beschreven hoe u de volgende onderdelen van de load balancer kunt maken en configureren:
  - een front-end IP-pool die het binnenkomende netwerkverkeer op de load balancer ontvangt.
  - een back-end IP-pool waar de front-endpool het netwerkverkeer op de load balancer heen stuurt.
  - een statustest die de status van de back-end-VM-exemplaren vaststelt.
  - een load balancer-regel die bepaalt hoe het verkeer over de VM's wordt verdeeld.

### <a name="create-the-load-balancer"></a>Load balancer maken

Maak met [az network lb create](https://docs.microsoft.com/cli/azure/network/lb?view=azure-cli-latest) een openbare Azure-load balancer met de naam **myLoadBalancer** die een front-endgroep bevat met de naam **myFrontEndPool** en een back-endgroep met de naam **myBackEndPool** die wordt gekoppeld aan het openbare IP-adres **myPublicIP** dat u in de vorige stap hebt gemaakt.

```azurecli-interactive
  az network lb create \
    --resource-group myResourceGroupLB \
    --name myLoadBalancer \
    --public-ip-address myPublicIP \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool       
  ```

### <a name="create-the-health-probe"></a>Statustest maken

Een statuscontrole controleert alle exemplaren van de virtuele machines om ervoor te zorgen dat deze netwerkverkeer kunnen verzenden. Het exemplaar van een virtuele machine met mislukte testcontroles wordt uit de load balancer verwijderd totdat deze weer online komt en een testcontrole bepaalt of deze in orde is. Maak met [az network lb probe create](https://docs.microsoft.com/cli/azure/network/lb/probe?view=azure-cli-latest) een statustest om de status van de virtuele machines te bewaken. 

```azurecli-interactive
  az network lb probe create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80   
```

### <a name="create-the-load-balancer-rule"></a>Load balancer-regel maken

Een load balancer-regel definieert de front-end-IP-configuratie voor het binnenkomende verkeer en de back-end-IP-pool om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maak met [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest) de regel *myLoadBalancerRuleWeb* voor het luisteren naar poort 80 in de front-endpool *myFrontEndPool* en het verzenden van netwerkverkeer met evenredige taakverdeling naar de back-endadresgroep *myBackEndPool* waarbij ook van poort 80 gebruik wordt gemaakt. 

```azurecli-interactive
  az network lb rule create \
    --resource-group myResourceGroupLB \
    --lb-name myLoadBalancer \
    --name myHTTPRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe  
```

## <a name="configure-virtual-network"></a>Virtueel netwerk configureren

Voordat u enkele VM's implementeert en uw load balancer test, maakt u de ondersteunende virtuele-netwerkbronnen.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak met [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) in *myResourceGroup* een virtueel netwerk met de naam *myVnet* met een subnet met de naam *mySubnet*.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroupLB \
    --location eastus \
    --name myVnet \
    --subnet-name mySubnet
```
###  <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken
Maak een netwerkbeveiligingsgroep om binnenkomende verbindingen met uw virtuele netwerk te definiëren.

```azurecli-interactive
  az network nsg create \
    --resource-group myResourceGroupLB \
    --name myNetworkSecurityGroup
```

### <a name="create-a-network-security-group-rule"></a>Regel voor netwerkbeveiligingsgroep maken

Maak een regel voor de netwerkbeveiligingsgroep om binnenkomende verbindingen via poort 80 toe te staan.

```azurecli-interactive
  az network nsg rule create \
    --resource-group myResourceGroupLB \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleHTTP \
    --protocol tcp \
    --direction inbound \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 80 \
    --access allow \
    --priority 200
```
### <a name="create-nics"></a>NIC's maken

Maak met [az network nic create](/cli/azure/network/nic#az-network-nic-create) drie netwerkinterfaces en koppel deze aan het openbare IP-adres en de netwerkbeveiligingsgroep. 

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupLB \
    --name myNic$i \
    --vnet-name myVnet \
    --subnet mySubnet \
    --network-security-group myNetworkSecurityGroup \
    --lb-name myLoadBalancer \
    --lb-address-pools myBackEndPool
done
```


## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u drie virtuele machines die worden gebruikt als back-endservers voor de load balancer. Installeer tevens NGINX op de virtuele machines om te controleren of de load balancer is gemaakt.

### <a name="create-an-availability-set"></a>Beschikbaarheidsset maken

Een beschikbaarheidsset maken met [az vm availabilityset create](/cli/azure/network/nic)

 ```azurecli-interactive
  az vm availability-set create \
    --resource-group myResourceGroupLB \
    --name myAvailabilitySet
```

### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

U kunt een cloud-init-configuratiebestand maken om NGINX te installeren en een 'Hallo wereld' Node.js-app uit te voeren op een virtuele Linux-machine. Maak in uw huidige shell een bestand met de naam cloud-init.txt en plak de volgende configuratie in de shell. Zorg ervoor dat u het hele cloud-init-bestand correct kopieert, met name de eerste regel:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
``` 
 
Maak de virtuele machines met [az vm create](/cli/azure/vm#az-vm-create).

 ```azurecli-interactive
for i in `seq 1 2`; do
  az vm create \
    --resource-group myResourceGroupLB \
    --name myVM$i \
    --availability-set myAvailabilitySet \
    --nics myNic$i \
    --image UbuntuLTS \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
    --no-wait
    done
```
Het kan enkele minuten duren voordat de VM's zijn geïmplementeerd.

## <a name="test-the-load-balancer"></a>Load balancer testen

Gebruik [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) om het openbare IP-adres van de load balancer op te halen. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurecli-interactive
  az network public-ip show \
    --resource-group myResourceGroupLB \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
``` 
![Load balancer testen](./media/load-balancer-get-started-internet-arm-cli/running-nodejs-app.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az-group-delete) gebruiken om de resourcegroep, de load balancer en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurecli-interactive 
  az group delete --name myResourceGroupLB
```


## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u een eenvoudige load balancer gemaakt, VM's aan de load balancer gekoppeld, een regel voor het load balancer-verkeer geconfigureerd, een statustest gemaakt en vervolgens de load balancer getest. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-basic-internal-portal.md)
