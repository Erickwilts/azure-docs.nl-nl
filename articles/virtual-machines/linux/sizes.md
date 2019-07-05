---
title: Linux VM-grootten in Azure | Microsoft Docs
description: Geeft een lijst van de verschillende grootten beschikbaar voor virtuele Linux-machines in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/07/2019
ms.author: jonbeck
ms.openlocfilehash: 8f01cb939ea5369812a2e39aa0e07fe2a409a1d5
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542617"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Grootten voor virtuele Linux-machines in Azure
Dit artikel beschrijft de beschikbare grootten en opties voor de virtuele machines van Azure die kunt u uw Linux-apps en workloads uit te voeren. Het biedt ook overwegingen voor de implementatie moet letten wanneer u van plan bent om deze resources te gebruiken. In dit artikel is ook beschikbaar voor [Windows virtuele machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Type                     | Grootten           |    Description       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Algemeen doel](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, Av2, DC  | Evenwichtige CPU-geheugenverhouding. Ideaal voor testen en ontwikkelen, kleine tot middelgrote databases en webservers met weinig of gemiddeld verkeer. |
| [Geoptimaliseerde rekenkracht](sizes-compute.md)        | Fsv2           | Hoge CPU-geheugenverhouding. Goed voor webservers met gemiddeld verkeer, netwerkapparatuur, batchprocessen en toepassingsservers.        |
| [Geoptimaliseerd geheugen](sizes-memory.md)         | Esv3, Ev3, Mv2, M, DSv2, Dv2  | Hoge geheugen-naar-CPU-verhouding. Uiterst geschikt voor relationele-databaseservers, middelgrote tot grote caches en analysefuncties in het geheugen.                 |
| [Geoptimaliseerde opslag](sizes-storage.md)        | Lsv2                | Snelle doorvoer van schijfgegevens en i/o-ideaal voor Big Data, SQL, NoSQL-databases, gegevensopslag en grote transactionele databases.  |
| [GPU](sizes-gpu.md)            | NC, NCv2, NCv3, ND, NDv2 (Preview), NV, NVv3 (Preview) | Gespecialiseerde virtuele machines bedoeld voor intensieve grafische rendering en videobewerking, evenals model trainings- en inferentietaken (ND) met deep learning. Beschikbaar met één of meerdere GPU's.       |
| [Krachtig rekenvermogen](sizes-hpc.md) | HB, HC,  H | Onze snelste en krachtigste CPU-virtuele machines met optionele netwerkinterfaces (RDMA) voor hoge doorvoer. |

<br>

- Zie voor meer informatie over de prijzen van de verschillende grootten [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Zie voor de beschikbaarheid van VM-grootten in Azure-regio's, [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).
- Zie voor algemene limieten op Azure Virtual machines [Azure-abonnement en Servicelimieten, quotums en beperkingen](../../azure-subscription-service-limits.md).
- Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.


## <a name="rest-api"></a>REST-API

Zie de volgende onderwerpen voor meer informatie over het gebruik van de REST-API voor query's voor VM-grootten:

- [Grootten van de lijst met beschikbare virtuele machine voor het vergroten of verkleinen](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Lijst met beschikbare VM-grootten voor een abonnement](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Grootte van de lijst met beschikbare virtuele machine in een beschikbaarheidsset](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Meer informatie over hoe u [Azure compute units (ACU)](acu.md) kunt u de prestaties van Azure-SKU's met elkaar vergelijken.

## <a name="benchmark-scores"></a>Benchmarkscores

Meer informatie over compute-prestaties voor virtuele Linux-machines met behulp van de [CoreMark benchmarkscores](compute-benchmark-scores.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende VM-grootten die beschikbaar zijn:
- [Algemeen doel](sizes-general.md)
- [Geoptimaliseerde rekenkracht](sizes-compute.md)
- [Geoptimaliseerd geheugen](sizes-memory.md)
- [Geoptimaliseerde opslag](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Krachtig rekenvermogen](sizes-hpc.md)
- Controleer de [vorige generatie](sizes-previous-gen.md) -pagina voor A Standard, Dv1 (D1-4 en D11-14 v1), en A8-A11-serie



