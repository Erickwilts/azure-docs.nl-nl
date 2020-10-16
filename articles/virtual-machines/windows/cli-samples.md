---
title: Azure CLI-voor beelden Windows
description: Azure CLI-voor beelden Windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 6c4b707a3cebc1bcdad7c9e14a96d82a8dda2371
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89318791"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Azure CLI-voorbeelden voor virtuele Windows-machines

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI voor het implementeren van virtuele Windows-machines.

| Script | Beschrijving |
|---|---|
|**Virtuele machines maken**||
| [Een virtuele machine maken](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele Windows-machine met een minimale configuratie. |
| [Een volledig geconfigureerde virtuele machine maken](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een resource groep, een virtuele machine en alle gerelateerde resources.|
| [Highly Available Virtual Machines maken](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Maakt verschillende virtuele machines in een configuratie met hoge Beschik baarheid en taak verdeling. |
| [Een VM maken en een configuratiescript uitvoeren](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de aangepaste scriptextensie voor Azure om IIS te installeren. |
| [Een virtuele machine maken en een DSC-configuratie uitvoeren](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine en gebruikt u de Desired State Configuration-extensie (DSC) voor Azure om IIS te installeren. |
|**Opslag beheren**||
| [Een beheerde schijf maken op basis van een VHD](../scripts/virtual-machines-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een beheerde schijf van een speciale VHD als een besturingssysteem schijf of van een gegevens-VHD als gegevens schijf.  |
| [Een beheerde schijf maken op basis van een momentopname](../scripts/virtual-machines-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u beheerde schijf op basis van een momentopname. |
| [Een beheerde schijf kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee wordt de beheerde schijf gekopieerd naar hetzelfde of een ander abonnement, maar in dezelfde regio als de bovenliggende beheerde schijf. 
| [Een momentopname als VHD exporteren naar een opslagaccount](../scripts/virtual-machines-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporteert een beheerde moment opname als VHD naar een opslag account in een andere regio. |
| [De VHD van een beheerde schijf exporteren naar een opslagaccount](../scripts/virtual-machines-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Exporteert de onderliggende VHD van een beheerde schijf naar een opslag account in een andere regio. |
| [Een momentopname kopiëren naar hetzelfde of een ander abonnement](../scripts/virtual-machines-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | De moment opname wordt gekopieerd naar hetzelfde of een ander abonnement, maar in dezelfde regio als de bovenliggende moment opname. |
|**Netwerken voor virtuele machines instellen**||
| [Netwerkverkeer tussen virtuele machines beveiligen](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Maakt twee virtuele machines, alle gerelateerde resources en een interne en externe netwerk beveiligings groepen (NSG). |
|**Virtuele machines beveiligen**||
| [Een virtuele machine en de bijbehorende gegevensschijven versleutelen](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Maakt een Azure Key Vault, versleutelings sleutel en Service-Principal en versleutelt vervolgens een virtuele machine. |
|**Virtuele machines bewaken**||
| [VM bewaken met Azure-bewaking](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Hiermee maakt u een virtuele machine, installeert u de Log Analytics-agent en registreert u de VM in een Log Analytics-werk ruimte.  |
| | |
