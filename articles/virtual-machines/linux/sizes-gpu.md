---
title: Azure Linux VM-grootten - versnelde compute | Microsoft Docs
description: Een lijst met de verschillende GPU geoptimaliseerd grootten beschikbaar voor virtuele Linux-machines in Azure. Bevat informatie over het aantal vcpu's, gegevensschijven en NIC's, evenals opslag en doorvoer netwerkbandbreedte voor grootten die in deze reeks.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 64cbcd375840d78916810abf9ccb8478ef9a1359
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708835"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU-geoptimaliseerde VM-grootten

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>Ondersteunde distributies en stuurprogramma 's

Als u wilt profiteren van de GPU-mogelijkheden van Azure VM's voor N-serie waarop Linux wordt uitgevoerd, moeten de NVIDIA GPU-stuurprogramma's worden geïnstalleerd. De [NVIDIA GPU-stuurprogramma extensie](../extensions/hpccompute-gpu-linux.md) juiste NVIDIA CUDA- of GRID stuurprogramma's worden geïnstalleerd op een VM uit de N-serie. Installeren of beheren van de extensie met de Azure portal of hulpprogramma's, zoals de Azure CLI of Azure Resource Manager-sjablonen. Zie de [NVIDIA GPU-stuurprogramma extensie documentatie](../extensions/hpccompute-gpu-linux.md) voor ondersteunde distributies en de implementatiestappen. Raadpleeg voor algemene informatie over VM-extensies [virtuele machine van Azure-extensies en functies](../extensions/overview.md).

Als u kiest voor het handmatig installeren van NVIDIA GPU-stuurprogramma's, Zie [setup van de N-serie GPU-stuurprogramma voor Linux](n-series-driver-setup.md) voor ondersteunde distributies en stuurprogramma's voor de installatie-en verificatiestappen.


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* U mag niet X installeren-server of andere systemen die gebruikmaken van de `Nouveau` stuurprogramma op Ubuntu Netwerkcontroller-VM's. Voordat u NVIDIA GPU-stuurprogramma's installeert, moet u om uit te schakelen de `Nouveau` stuurprogramma.  

## <a name="other-sizes"></a>Andere grootten
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- [Vorige generaties](sizes-previous-gen.md)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.