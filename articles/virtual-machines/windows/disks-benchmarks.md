---
title: Uw toepassing op Azure Disk-opslag - beheerde schijven benchmarking
description: Meer informatie over het proces van benchmarks van uw toepassing in Azure.
services: virtual-machines-windows,storage
author: roygara
ms.author: rogarana
ms.date: 01/11/2019
ms.topic: article
ms.service: virtual-machines-windows
ms.tgt_pltfrm: windows
ms.subservice: disks
ms.openlocfilehash: 8db1fb3c9b3ed551cd668cf14105eb8bfb486251
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60679736"
---
# <a name="benchmarking-a-disk"></a>Een diskette benchmarking

Benchmarking is het proces van het simuleren van verschillende workloads op uw toepassing en de prestaties van toepassingen voor elke werkbelasting te meten. Met behulp van de stappen in de [ontwerpen voor hoge prestaties artikel](premium-storage-performance.md), u hebt de prestatie-eisen voor toepassingen verzameld. Door het uitvoeren van hulpprogramma's voor benchmarking op de virtuele machines die als host fungeert voor de toepassing, kunt u de prestatieniveaus die binnen uw toepassing met Premium Storage bereiken kunt bepalen. In dit artikel bieden wij u voorbeelden van benchmarks van een Standard DS14-virtuele machine ingericht met Azure Premium Storage-schijven.

We hebben respectievelijk algemene hulpprogramma's voor benchmarking Iometer en FIO, voor Windows en Linux gebruikt. Deze hulpprogramma's starten meerdere threads simuleren van een productie-achtige werkbelasting en meten van prestaties van het systeem. U kunt ook een parameters, zoals het block-grootte en wachtrij diepte, die u normaal gesproken niet voor een toepassing wijzigen configureren met behulp van de hulpprogramma's. Dit biedt u meer flexibiliteit om de maximale prestaties op een grote schaal VM ingericht met premium-schijven voor verschillende soorten werkbelastingen van toepassingen te stimuleren. Voor meer informatie over elke benchmarking hulpprogramma gaat u naar [Iometer](http://www.iometer.org/) en [FIO](http://freecode.com/projects/fio).

Volg de onderstaande voorbeelden, een standaard DS14-virtuele machine maken en 11 Premium-opslagschijven koppelen aan de virtuele machine. 11 schijven, 10 schijven configureren met opslaan in cache als 'Geen' en deze in een volume naam NoCacheWrites stripe. Opslaan in cache als 'Alleen-lezen' op de resterende schijf configureren en maken van een volume naam CacheReads met deze schijf. Met deze instellingen, bent u ziet de maximale prestaties voor lezen en schrijven van een Standard DS14-virtuele machine. Voor gedetailleerde stappen over het maken van een DS14-VM met premium SSD's, gaat u naar [ontwerpen voor hoge prestaties](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Volgende stappen

Volg de stappen in onze ontwerp voor hoge prestaties artikel. In deze maakt u een controlelijst die vergelijkbaar is met uw bestaande toepassing voor het model. Met behulp van hulpprogramma's voor Benchmarking kunt u de werkbelastingen te simuleren en meten van prestaties van de toepassing prototype. Op deze manier kunt u bepalen welke schijf aanbieding kan overeenkomen met of groter zijn dan de prestatievereisten van uw toepassing. Vervolgens kunt u dezelfde richtlijnen als voor uw productietoepassing implementeren.

> [!div class="nextstepaction"]
> Zie het artikel op [ontwerpen voor hoge prestaties](premium-storage-performance.md) beginnen.