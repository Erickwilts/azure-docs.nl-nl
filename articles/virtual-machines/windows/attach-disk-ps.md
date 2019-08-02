---
title: Een gegevens schijf koppelen aan een virtuele Windows-machine in azure met behulp van Power shell | Microsoft Docs
description: Een nieuwe of bestaande gegevens schijf koppelen aan een virtuele Windows-machine met behulp van Power shell met het Resource Manager-implementatie model.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 615eedc66d1c4ac931067ffccdace5d161b18384
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699884"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Een gegevens schijf koppelen aan een virtuele Windows-machine met Power shell

Dit artikel laat u zien hoe u met behulp van Power shell zowel nieuwe als bestaande schijven kunt koppelen aan een virtuele Windows-machine. 

Lees eerst de volgende tips:

* De grootte van de virtuele machine bepaalt hoeveel gegevens schijven u kunt bijvoegen. Zie voor meer informatie, [grootten voor virtuele machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Als u Premium Ssd's wilt gebruiken, moet u een [VM-type met Premium-opslag capaciteit](sizes-memory.md)hebben, zoals de virtuele machine van de DS-serie of GS-serie.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Een lege gegevens schijf toevoegen aan een virtuele machine

In dit voor beeld ziet u hoe u een lege gegevens schijf toevoegt aan een bestaande virtuele machine.

### <a name="using-managed-disks"></a>Beheerde schijven gebruiken

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

### <a name="using-managed-disks-in-an-availability-zone"></a>Beheerde schijven in een beschikbaarheids zone gebruiken

Als u een schijf in een beschikbaarheids zone wilt maken, gebruikt u [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) met de `-Zone` para meter. In het volgende voor beeld wordt een schijf in zone *1*gemaakt.

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

### <a name="initialize-the-disk"></a>De schijf initialiseren

Nadat u een lege schijf hebt toegevoegd, moet u deze initialiseren. Als u de schijf wilt initialiseren, kunt u zich aanmelden bij een virtuele machine en schijf beheer gebruiken. Als u [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) en een certificaat op de VM hebt ingeschakeld toen u het maakte, kunt u externe Power shell gebruiken om de schijf te initialiseren. U kunt ook een aangepaste script extensie gebruiken:

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

Het script bestand kan code bevatten voor het initialiseren van de schijven, bijvoorbeeld:

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

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Een bestaande gegevens schijf koppelen aan een virtuele machine

U kunt een bestaande beheerde schijf koppelen aan een virtuele machine als een gegevens schijf.

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

Maak een [moment opname](snapshot-copy-managed-disk.md).
