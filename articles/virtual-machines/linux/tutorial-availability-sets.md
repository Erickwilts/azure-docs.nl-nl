---
title: 'Zelfstudie: Hoge beschikbaarheid voor virtuele Linux-machines in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe u de Azure CLI gebruikt om maximaal beschikbare virtuele machines in beschikbaarheidssets te implementeren
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 08/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1eea6bf06c6245cf5a13cdd33879cf31469f6042
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708563"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-the-azure-cli"></a>Zelfstudie: Virtuele machines met hoge beschikbaarheid maken en implementeren met de Azure CLI

In deze zelfstudie leert u hoe u de beschikbaarheid en betrouwbaarheid van uw Virtual Machine-oplossingen op Azure kunt verhogen met behulp van beschikbaarheidssets. Beschikbaarheidssets zorgen ervoor dat de VM's die u op Azure implementeert, verdeeld worden over meerdere geïsoleerde hardwareclusters. Dit zorgt ervoor dat als er zich binnen Azure een hardware- of softwarestoring voordoet, er slechts een subset van uw VM's wordt beïnvloed en dat uw totale oplossing beschikbaar en operationeel blijft.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="availability-set-overview"></a>Overzicht beschikbaarheidsset

Een beschikbaarheidsset is een logische groeperingsmogelijkheid die u in Azure kunt gebruiken om ervoor te zorgen dat de VM-resources die u erin plaatst, van elkaar worden geïsoleerd wanneer ze in een Azure-datacenter worden geïmplementeerd. Azure zorgt ervoor dat de VM's die u in een beschikbaarheidsset plaatst, op meerdere fysieke servers, rekenrekken, opslageenheden en netwerkswitches worden uitgevoerd. Als er zich een hardware- of softwarestoring in Azure voordoet, wordt slechts een subset van uw VM's getroffen en blijft uw totale toepassing actief en beschikbaar voor uw klanten. Beschikbaarheidssets zijn essentieel wanneer u betrouwbare cloudoplossingen wilt bouwen.

Laten we eens kijken naar een typische VM-oplossing met vier front-end webservers en twee back-end VM's die een database hosten. Met Azure wilt u twee beschikbaarheidssets definiëren voordat u uw VM's implementeert: een beschikbaarheidsset voor de ‘web’-laag en een beschikbaarheidsset voor de ‘database’-laag. Bij het maken van een nieuwe VM kunt u vervolgens de beschikbaarheidsset opgeven als parameter voor de opdracht az vm create, en zorgt Azure er automatisch voor dat de VM's die u binnen de beschikbare set maakt, worden geïsoleerd over meerdere fysieke hardwareresources. Als er een probleem is met de fysieke hardware waarop een van uw webserver- of databaseserver-VM's draait, weet u dat de andere instanties van uw webserver en database-VM's actief blijven, omdat ze worden uitgevoerd op andere hardware.

Gebruik beschikbaarheidssets wanneer u betrouwbare VM-oplossingen wilt implementeren in Azure.


## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

U kunt een beschikbaarheidsset maken met behulp van [az vm availability-set create](/cli/azure/vm/availability-set). In dit voorbeeld is het aantal update- en foutdomeinen ingesteld op *2* voor de beschikbaarheidsset met de naam *myAvailabilitySet* in de resourcegroep *ResourceGroupAvailability*.

Maak eerst een resourcegroep met [az group create](/cli/azure/group#az-group-create) en maak daarna de beschikbaarheidsset:

```azurecli-interactive
az group create --name myResourceGroupAvailability --location eastus

az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Met beschikbaarheidssets kunt u bronnen isoleren tussen foutdomeinen en domeinen bijwerken. Een **foutdomein** vertegenwoordigt een geïsoleerde verzameling van server- plus netwerk- en opslagresources. In het voorgaande voorbeeld wordt de beschikbaarheidsset verdeeld over minstens twee foutdomeinen wanneer de VM's worden geïmplementeerd. De beschikbaarheidsset is ook verdeeld over twee **updatedomeinen**. Twee updatedomeinen zorgen ervoor dat wanneer er software-updates worden uitgevoerd op Azure, de VM-resources worden geïsoleerd, zodat wordt voorkomen dat alle software onder de VM tegelijk wordt bijgewerkt.


## <a name="create-vms-inside-an-availability-set"></a>VM's maken in een beschikbaarheidsset

VM's moeten worden gemaakt binnen de beschikbaarheidsset om ervoor te zorgen dat ze correct over de hardware worden verdeeld. U kunt een bestaande VM niet toevoegen aan een beschikbaarheidsset nadat deze is gemaakt.

Wanneer er een VM is gemaakt met [az vm create](/cli/azure/vm), gebruikt u de parameter `--availability-set` om de naam van de beschikbaarheidsset op te geven.

```azurecli-interactive
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --vnet-name myVnet \
     --subnet mySubnet \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys
done
```

De beschikbaarheidsset bevat nu twee virtuele machines. Omdat ze zich in dezelfde beschikbaarheidsset bevinden, zorgt Azure ervoor dat de virtuele machines en alle bijbehorende resources (inclusief gegevensschijven) worden gedistribueerd over geïsoleerde fysieke hardware. Door deze verdeling is de beschikbaarheid van de algehele VM-oplossing veel hoger.

U kunt de verdeling van de beschikbaarheidsset bekijken in de portal door te gaan naar Resourcegroepen > myResourceGroupAvailability > myAvailabilitySet. De VM's zijn verdeeld over de twee fout- en updatedomeinen, zoals wordt weergegeven in het volgende voorbeeld:

![Beschikbaarheidsset in de portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Beschikbare VM-grootten controleren

Extra VM's kunnen later worden toegevoegd aan de beschikbaarheidsset, als VM-grootten beschikbaar zijn op de hardware. Gebruik [az vm availability-set list-sizes](/cli/azure/vm/availability-set#az-vm-availability-set-list-sizes) om een lijst weer te geven met alle beschikbare grootten voor de beschikbaarheidsset op de hardwarecluster:

```azurecli-interactive
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren

Ga naar de volgende zelfstudie voor meer informatie over virtuele-machineschaalsets.

> [!div class="nextstepaction"]
> [Een virtuele-machineschaalset maken](tutorial-create-vmss.md)
