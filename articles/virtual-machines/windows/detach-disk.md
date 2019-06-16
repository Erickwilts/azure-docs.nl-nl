---
title: Loskoppelen van een gegevensschijf van een Windows-VM - Azure | Microsoft Docs
description: Een gegevensschijf van een virtuele machine in Azure met behulp van de Resource Manager-implementatiemodel ontkoppelen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 21b9ec78c4b4ca3a0f96bdd84be1d11e9d7257d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64681857"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Een gegevensschijf van een Windows virtuele machine loskoppelen

Wanneer u een gegevensschijf die is gekoppeld aan een virtuele machine niet meer nodig hebt, kunt u deze eenvoudig loskoppelen. Hiermee wordt de schijf van de virtuele machine verwijderd, maar niet verwijderd uit de opslag.

> [!WARNING]
> Als u een schijf loskoppelen wordt deze niet automatisch verwijderd. Als u bent geabonneerd op Premium-opslag, blijft u opslag in rekening gebracht voor de schijf. Zie voor meer informatie, [prijzen en facturering bij het gebruik van Premium Storage](disks-types.md#billing).

Als u de bestaande gegevens op de schijf opnieuw wilt gebruiken, kunt u de schijf opnieuw koppelen aan dezelfde of een andere virtuele machine.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="detach-a-data-disk-using-powershell"></a>Loskoppelen van een gegevensschijf met behulp van PowerShell

U kunt *hot* verwijderen van een gegevensschijf met behulp van PowerShell, maar zorg ervoor dat er niets is actief met behulp van de schijf voordat u deze loskoppelen van de virtuele machine.

In dit voorbeeld verwijderen we de schijf met de naam **myDisk** van de virtuele machine **myVM** in de **myResourceGroup** resourcegroep. Eerst verwijdert u de schijf met de [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk) cmdlet. En u de status van de virtuele machine bijwerken met behulp van de [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) cmdlet om het proces van het verwijderen van de gegevensschijf te voltooien.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.

## <a name="detach-a-data-disk-using-the-portal"></a>Een gegevensschijf ontkoppelen via de portal

1. Selecteer in het menu links **virtuele Machines**.
2. Selecteer de virtuele machine waarvoor de gegevensschijf die u wilt loskoppelen en klikt u op **stoppen** voor het toewijzen van de virtuele machine ongedaan maken.
3. Selecteer in het deelvenster met de virtuele machine **schijven**.
4. Aan de bovenkant van de **schijven** venster **bewerken**.
5. In de **schijven** in het deelvenster aan de rechterkant van de gegevensschijf die u wilt loskoppelen, klikt u op de ![afbeelding van de knop ontkoppelen](./media/detach-disk/detach.png) knop loskoppelen.
5. Nadat de schijf is verwijderd, klikt u op **opslaan** boven aan het deelvenster.
6. Klik in het deelvenster virtuele machine **overzicht** en klik vervolgens op de **Start** knop aan de bovenkant van het deelvenster met de virtuele machine opnieuw opstarten.

De schijf blijft in de opslag, maar is niet meer gekoppeld aan een virtuele machine.

## <a name="next-steps"></a>Volgende stappen

Als u de gegevensschijf opnieuw gebruiken wilt, kunt u zojuist hebt [koppelen aan een andere virtuele machine](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)