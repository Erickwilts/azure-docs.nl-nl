---
title: Fout domeinen in virtuele-machine schaal sets van Azure beheren | Microsoft Docs
description: Meer informatie over het kiezen van het juiste aantal Fd's tijdens het maken van een schaalset voor virtuele machines.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: drewm
ms.openlocfilehash: d6207a1a77ff00ff36fe6b3b748c3f8f7279b62d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639955"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Het juiste aantal fout domeinen voor de schaalset van de virtuele machine kiezen
Virtuele-machine schaal sets worden standaard gemaakt met vijf fout domeinen in azure-regio's zonder zones. Voor de regio's die ondersteuning bieden voor zonegebonden-implementatie van schaal sets voor virtuele machines en deze optie is geselecteerd, is de standaard waarde van het aantal fout domeinen 1 voor elk van de zones. FD = 1 in dit geval impliceert dat de VM-exemplaren die deel uitmaken van de schaalset, worden verdeeld over veel racks op basis van de beste inspanningen.

U kunt ook overwegen het aantal fout domeinen met schaal sets af te stemmen met het aantal Managed Disks fout domeinen. Deze uitlijning kan helpen voor komen dat quorum verloren gaat als een volledig Managed Disks fout domein wordt uitgeschakeld. Het aantal FD kan worden ingesteld op minder dan of gelijk aan het aantal Managed Disks fout domeinen dat in elk van de regio's beschikbaar is. Raadpleeg dit [document](../virtual-machines/windows/manage-availability.md) voor meer informatie over het aantal Managed disks fout domeinen per regio.

## <a name="rest-api"></a>REST-API
U kunt de eigenschap `properties.platformFaultDomainCount` instellen op 1, 2 of 3 (de standaard waarde is 5 als deze niet is opgegeven). Raadpleeg [hier](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)de documentatie voor rest API.

## <a name="azure-cli"></a>Azure-CLI
U kunt de para meter `--platform-fault-domain-count` instellen op 1, 2 of 3 (de standaard waarde is 5 als deze niet is opgegeven). Raadpleeg [hier](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)de documentatie voor Azure cli.

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [functies voor Beschik baarheid en redundantie](../virtual-machines/windows/availability.md) voor Azure-omgevingen.
