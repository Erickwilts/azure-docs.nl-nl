---
title: Beheerde schijven naar een abonnement kopiëren - PowerShell-voorbeeld
description: Voorbeeld van Azure PowerShell-script - beheerde schijven kopiëren of verplaatsen naar hetzelfde of een ander abonnement
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.subservice: disks
ms.topic: sample
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: e682a1546297e7715a00c7c174ad9a17021e6029
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89320459"
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a>Met PowerShell beheerde schijven kopiëren naar hetzelfde of een ander abonnement

Met dit script maakt u een kopie van een bestaande beheerde schijf in hetzelfde abonnement of in een ander abonnement. De nieuwe schijf wordt gemaakt in dezelfde regio als de bovenliggende beheerde schijf.   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om een nieuwe beheerde schijf te maken in het doelabonnement met gebruikmaking van de id van de beheerde bronschijf. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Hiermee wordt de schijfconfiguratie gemaakt die voor het maken van de schijf wordt gebruikt. De opdracht bevat de resource-id van de bovenliggende schijf en de locatie, die dezelfde is als de locatie van de bovenliggende schijf.  |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Hiermee maakt u een schijf die de schijfconfiguratie, de schijfnaam en de naam van de resourcegroep als parameters gebruikt. |


## <a name="next-steps"></a>Volgende stappen

[Een virtuele machine maken op basis van een beheerde schijf](./virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/).

U kunt extra PowerShell-scriptvoorbeelden voor virtuele machines vinden in de [Azure-documentatie voor Windows-VM's](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
