---
title: Extensie van virtuele machine voor Azure Network Watcher agent voor Linux | Microsoft Docs
description: Implementeer de Network Watcher-agent op de virtuele Linux-machine met behulp van een extensie voor virtuele machines.
services: virtual-machines-linux
documentationcenter: ''
author: gurudennis
manager: amku
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: b59e4c570032bdd3341dc7d519f23f4cd86984c7
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70084446"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Extensie van virtuele machine voor Network Watcher agent voor Linux

## <a name="overview"></a>Overzicht

[Azure Network Watcher](/azure/network-watcher/) is een bewakings-, diagnose-en analyse service voor netwerk prestaties waarmee u Azure-netwerken kunt bewaken. De extensie van de Network Watcher agent virtuele machine (VM) is een vereiste voor een aantal van de Network Watcher functies op Azure-Vm's, zoals het vastleggen van netwerk verkeer op aanvraag en andere geavanceerde functies.

In dit artikel vindt u meer informatie over de ondersteunde platforms en implementatie opties voor de Network Watcher agent VM-extensie voor Linux. Installatie van de agent verstoort niet of vereist een herstart van de virtuele machine. U kunt de uitbrei ding implementeren op virtuele machines die u implementeert. Als de virtuele machine wordt geïmplementeerd door een Azure-service, raadpleegt u de documentatie voor de service om te bepalen of de installatie van uitbrei dingen in de virtuele machine is toegestaan.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De uitbrei ding voor de Network Watcher-agent kan worden geconfigureerd voor de volgende Linux-distributies:

| Distributie | Version |
|---|---|
| Ubuntu | 12+ |
| Debian | 7 en 8 |
| Red Hat | 6 en 7 |
| Oracle Linux | 6,8+ en 7 |
| SUSE Linux Enterprise Server | 11 en 12 |
| OpenSUSE-Schrikkel | 42.3 + |
| CentOS | 6.5+ en 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>Internetconnectiviteit

Voor sommige functies van de Network Watcher-agent moet een virtuele machine zijn verbonden met internet. Zonder de mogelijkheid om uitgaande verbindingen tot stand te brengen, kunnen sommige van de functies van de Network Watcher-agent defect zijn of niet meer beschikbaar zijn. Zie de[Network Watcher-documentatie](/azure/network-watcher/)voor meer informatie over Network Watcher functionaliteit waarvoor de agent is vereist.

## <a name="extension-schema"></a>Extensieschema

De volgende JSON toont het schema voor de uitbrei ding van de Network Watcher agent. De uitbrei ding vereist geen door de gebruiker opgegeven instellingen, of ondersteuning. De uitbrei ding is afhankelijk van de standaard configuratie.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| publisher | Microsoft.Azure.NetworkWatcher |
| Type | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Sjabloonimplementatie

U kunt Azure VM-extensies implementeren met een Azure Resource Manager sjabloon. Als u de uitbrei ding voor de Network Watcher-agent wilt implementeren, gebruikt u het vorige JSON-schema in uw sjabloon.

## <a name="azure-classic-cli-deployment"></a>Implementatie van klassieke Azure-CLI

In het volgende voor beeld wordt de VM-extensie Network Watcher agent geïmplementeerd op een bestaande virtuele machine die is geïmplementeerd via het klassieke implementatie model:

```azurecli
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-deployment"></a>Azure CLI-implementatie

In het volgende voor beeld wordt de VM-extensie Network Watcher agent geïmplementeerd op een bestaande VM die is geïmplementeerd via Resource Manager:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Probleemoplossing en ondersteuning

### <a name="troubleshooting"></a>Problemen oplossen

U kunt gegevens ophalen over de status van uitbreidings implementaties met behulp van de Azure Portal of Azure CLI.

In het volgende voor beeld ziet u de implementatie status van uitbrei dingen voor een VM die is geïmplementeerd via het klassieke implementatie model met behulp van de klassieke CLI van Azure:

```azurecli
azure config mode asm
azure vm extension get myVM1
```
Uitvoer voor uitvoering van extensie wordt vastgelegd in bestanden die zijn gevonden in de volgende map:

```
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
```

In het volgende voor beeld ziet u de implementatie status van de NetworkWatcherAgentLinux-extensie voor een VM die is geïmplementeerd via Resource Manager met behulp van Azure CLI:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Ondersteuning

Als u op elk moment meer hulp nodig hebt, raadpleegt u de [documentatie van Network Watcher](/azure/network-watcher/)of neemt u contact op met de Azure-experts op [MSDN Azure en stack overflow forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteunings site van Azure](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning verkrijgen**. Zie de [Microsoft Azure Veelgestelde vragen over ondersteuning](https://azure.microsoft.com/support/faq/)voor meer informatie over het gebruik van Azure-ondersteuning.
