---
title: Een gegevensschijf koppelen aan een Windows-VM in Azure met behulp van PowerShell | Microsoft Docs
description: Hoe u een nieuwe of bestaande gegevensschijf koppelen aan een Windows-VM met behulp van PowerShell met het Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 6a20dac0f89390f1229c7a71793814dc9f9397c1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66727852"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Een gegevensschijf koppelen aan een Windows-VM met PowerShell

In dit artikel wordt beschreven hoe u zowel nieuwe als bestaande schijven koppelt aan een virtuele machine van Windows met behulp van PowerShell. 

Bekijk eerst de volgende tips:

* De grootte van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie, [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Voor het gebruik van premium SSD's, moet u een [VM-schijftype voor premium-opslag ingeschakeld](sizes-memory.md), zoals de DS-reeks of GS-serie virtuele machine.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Een lege gegevensschijf toevoegen aan een virtuele machine

In dit voorbeeld laat zien hoe een lege gegevensschijf toevoegen aan een bestaande virtuele machine.

### <a name="using-managed-disks"></a>Beheerde schijven

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Beheerde schijven gebruiken in een Beschikbaarheidszone

Gebruik voor het maken van een schijf in een Beschikbaarheidszone [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) met de `-Zone` parameter. Het volgende voorbeeld wordt een schijf in de zone *1*.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2'
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="initialize-the-disk"></a>Initialiseer de schijf

Nadat u een lege schijf toevoegt, moet u voor het initialiseren. Voor het initialiseren van de schijf, kunt u zich aanmelden bij een virtuele machine en Gebruik Schijfbeheer. Als u ingeschakeld [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) en een certificaat op de virtuele machine hebt gemaakt, kunt u externe PowerShell gebruiken voor het initialiseren van de schijf. U kunt ook een aangepaste scriptextensie gebruiken:

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

Het scriptbestand kan code voor het initialiseren van de schijven, bijvoorbeeld bevatten:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Een bestaande gegevensschijf koppelen aan een virtuele machine

U kunt een bestaande beheerde schijf koppelen aan een virtuele machine als gegevensschijf.

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Volgende stappen

Maak een [momentopname](snapshot-copy-managed-disk.md).
