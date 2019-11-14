---
title: OKD implementeren in azure
description: Implementeer OKD in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: joraio
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/15/2019
ms.author: haroldw
ms.openlocfilehash: 6ecae09b0efbc0e35e591b31dbb54fb49ce101b3
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74035406"
---
# <a name="deploy-okd-in-azure"></a>OKD implementeren in azure

U kunt een van de twee manieren gebruiken om OKD (voorheen openstaande oorsprong) in azure te implementeren:

- U kunt alle benodigde onderdelen van Azure Infrastructure hand matig implementeren en vervolgens de [OKD-documentatie](https://docs.okd.io)volgen.
- U kunt ook een bestaande [Resource Manager-sjabloon](https://github.com/Microsoft/openshift-origin) gebruiken die de implementatie van het OKD-cluster vereenvoudigt.

## <a name="deploy-using-the-okd-template"></a>Implementeren met behulp van de OKD-sjabloon

Als u wilt implementeren met behulp van de Resource Manager-sjabloon, gebruikt u een parameter bestand voor het opgeven van de invoer parameters. Als u de implementatie verder wilt aanpassen, splitst u de GitHub-opslag plaats en wijzigt u de juiste items.

Enkele algemene aanpassings opties zijn, maar zijn niet beperkt tot:

- Bastion VM-grootte (variabele in azuredeploy. json)
- Naamgevings regels (variabelen in azuredeploy. json)
- Details van open Shift-cluster, gewijzigd via een hosts-bestand (deployOpenShift.sh)

Voor de [OKD-sjabloon](https://github.com/Microsoft/openshift-origin) zijn meerdere vertakkingen beschikbaar voor verschillende versies van OKD.  Op basis van uw behoeften kunt u rechtstreeks vanuit de opslag plaats implementeren of u kunt het opslag plaats opsplitsen en aangepaste wijzigingen aanbrengen voordat u implementeert.

Gebruik de `appId` waarde van de service-principal die u eerder hebt gemaakt voor de para meter `aadClientId`.

Hier volgt een voor beeld van een bestand met para meters met de naam azuredeploy. para meters. json met alle vereiste invoer.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "masterVmSize": {
            "value": "Standard_E2s_v3"
        },
        "infraVmSize": {
            "value": "Standard_E2s_v3"
        },
        "nodeVmSize": {
            "value": "Standard_E2s_v3"
        },
        "storageKind": {
            "value": "managed"
        },
        "openshiftClusterPrefix": {
            "value": "mycluster"
        },
        "masterInstanceCount": {
            "value": 3
        },
        "infraInstanceCount": {
            "value": 2
        },
        "nodeInstanceCount": {
            "value": 2
        },
        "dataDiskSize": {
            "value": 128
        },
        "adminUsername": {
            "value": "clusteradmin"
        },
        "openshiftPassword": {
            "value": "{Strong Password}"
        },
        "sshPublicKey": {
            "value": "{SSH Public Key}"
        },
        "enableMetrics": {
            "value": "true"
        },
        "enableLogging": {
            "value": "false"
        },
        "keyVaultResourceGroup": {
            "value": "keyvaultrg"
        },
        "keyVaultName": {
            "value": "keyvault"
        },
        "keyVaultSecret": {
            "value": "keysecret"
        },
        "enableAzure": {
            "value": "true"
        },
        "aadClientId": {
            "value": "11111111-abcd-1234-efgh-111111111111"
        },
        "aadClientSecret": {
            "value": "{Strong Password}"
        },
        "defaultSubDomainType": {
            "value": "nipio"
        }
    }
}
```

Vervang de para meters door uw specifieke informatie.

Verschillende releases hebben mogelijk verschillende para meters. Controleer de benodigde para meters voor de vertakking die u gebruikt.

### <a name="deploy-using-azure-cli"></a>Implementeren met behulp van Azure CLI


> [!NOTE] 
> Voor de volgende opdracht is Azure CLI 2.0.8 of hoger vereist. U kunt de CLI-versie controleren met de opdracht `az --version`. Zie [Azure cli installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)voor informatie over het bijwerken van de CLI-versie.

In het volgende voor beeld worden het OKD-cluster en alle gerelateerde resources geïmplementeerd in een resource groep met de naam openshiftrg, met een implementatie naam van myOpenShiftCluster. Er wordt rechtstreeks vanuit de GitHub-opslag plaats naar de sjabloon verwezen tijdens het gebruik van een lokaal bestand met para meters met de naam azuredeploy. para meters. json.

```azurecli 
az group deployment create -g openshiftrg --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --parameters @./azuredeploy.parameters.json
```

Het volt ooien van de implementatie duurt ten minste 30 minuten, op basis van het totale aantal geïmplementeerde knoop punten. Wanneer de implementatie is voltooid, wordt de URL van de open Shift-console en de DNS-naam van de open Shift-Master afgedrukt op de Terminal. U kunt ook de sectie outputs van de implementatie bekijken vanuit het Azure Portal.

```json
{
  "OpenShift Console Url": "http://openshiftlb.cloudapp.azure.com/console",
  "OpenShift Master SSH": "ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com"
}
```

Als u de opdracht regel niet wilt laten wachten tot de implementatie is voltooid, voegt u `--no-wait` toe als een van de opties voor de groeps implementatie. De uitvoer van de implementatie kan worden opgehaald uit de Azure Portal in de implementatie sectie voor de resource groep.

## <a name="connect-to-the-okd-cluster"></a>Verbinding maken met het OKD-cluster

Wanneer de implementatie is voltooid, kunt u met behulp van de `OpenShift Console Url`verbinding maken met de open Shift-console met uw browser. U kunt ook SSHen naar de OKD-Master. Hieronder volgt een voor beeld waarin de uitvoer van de implementatie wordt gebruikt:

```bash
$ ssh -p 2200 clusteradmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de opdracht [AZ Group delete](/cli/azure/group) om de resource groep, open Shift-cluster en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-container-platform-3x-post-deployment.md)
- [Problemen met de openshift-implementatie oplossen](./openshift-container-platform-3x-troubleshooting.md)
- [Aan de slag met OKD](https://docs.okd.io)
