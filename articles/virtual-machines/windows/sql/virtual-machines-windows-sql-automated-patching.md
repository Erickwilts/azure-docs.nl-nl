---
title: Automatische patches voor SQL Server-VM's (Resource Manager) | Microsoft Docs
description: De functie automatisch patchen voor SQL Server Virtual Machines die worden uitgevoerd in Azure met behulp van Resource Manager uitgelegd.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/07/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 4f0d681c93ab7ac7fef941892a95282a2fd59b89
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075724"
---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automated Patching voor SQL Server in virtuele machines van Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klassiek](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)

Geautomatiseerde Patching het vastleggen van een onderhoudsvenster voor een virtuele Machine van Azure met SQL Server. Geautomatiseerde updates kunnen alleen worden geïnstalleerd tijdens dit onderhoudsvenster. In SQL Server zorgt deze beperking ervoor dat systeemupdates en eventueel benodigd opnieuw opstarten plaatsvinden op het meest geschikte tijdstip voor de database. 

> [!IMPORTANT]
> Er worden alleen Windows-updates geïnstalleerd die zijn gemarkeerd als **Belangrijk**. Andere SQL Server-updates, zoals cumulatieve updates, moeten handmatig worden geïnstalleerd. 

Geautomatiseerd patchen is afhankelijk van de [extensie voor de SQL Server IaaS-agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van geautomatiseerde Patching, houd rekening met de volgende vereisten:

**Besturingssysteem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versie**:

* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell**:

* [Installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview) als u van plan bent automatisch patchen configureren met PowerShell.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!NOTE]
> Geautomatiseerde Patching, is afhankelijk van de SQL Server IaaS Agent-extensie. Huidige galerie met installatiekopieën van de SQL-machines met deze extensie standaard toegevoegd. Zie voor meer informatie, [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Instellingen
De volgende tabel beschrijft de opties die kunnen worden geconfigureerd voor het automatisch patchen. De werkelijke configuratiestappen variëren afhankelijk van of u de Azure portal of Azure Windows PowerShell-opdrachten gebruiken.

| Instelling | Mogelijke waarden | Description |
| --- | --- | --- |
| **Automatisch patch toepassen** |In-of uitschakelen (uitgeschakeld) |Hiermee schakelt automatisch patchen voor een Azure-machine of. |
| **Onderhoudsplanning** |Elke dag, maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag |Het schema voor het downloaden en installeren van Windows, SQL Server en Microsoft-updates voor uw virtuele machine. |
| **Begintijd van onderhoud** |0-24 |De lokale tijd voor het bijwerken van de virtuele machine start. |
| **Duur van het onderhoudsvenster** |30-180 |Het aantal minuten dat is toegestaan om uit te voeren van het downloaden en installeren van updates. |
| **Patch-categorie** |Belangrijk | De categorie van de Windows-updates downloaden en installeren.|

## <a name="configuration-in-the-portal"></a>Configuratie in de Portal
De Azure-portal kunt u automatisch patchen tijdens het inrichten of voor bestaande VM's configureren.

### <a name="new-vms"></a>Nieuwe virtuele machines
De Azure portal gebruiken voor het automatisch patchen te configureren wanneer u een nieuwe SQL Server-Machine in het Resource Manager-implementatiemodel maakt.

In de **SQL Server-instellingen** tabblad **-configuratie wijzigen** onder **Automated patching**. De volgende schermafbeelding van Azure portal bevat de **SQL automatisch patchen** blade.

![SQL automatisch patchen in Azure portal](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Zie de volledige onderwerp op voor de context, [inrichten van een virtuele machine van SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande VM 's

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Voor bestaande SQL Server virtuele machines, opent u uw [SQL-resource voor virtuele machines](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) en selecteer **Patching** onder **instellingen**. 

![Automatisch patchen van SQL voor bestaande VM 's](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)


Wanneer u klaar bent, klikt u op de **OK** knop aan de onderkant van de **SQL Server-configuratie** blade uw wijzigingen op te slaan.

Als u automatisch patchen voor het eerst inschakelen wilt, configureert Azure de SQL Server IaaS Agent op de achtergrond. Gedurende deze tijd de Azure-portal mogelijk niet weergegeven dat automatisch patchen is geconfigureerd. Wacht enkele minuten voor de agent moet worden geïnstalleerd of geconfigureerd. Daarna geeft de Azure portal de nieuwe instellingen.

## <a name="configuration-with-powershell"></a>Configuratie met PowerShell
Na het inrichten van uw SQL-VM door PowerShell te gebruiken voor het configureren van automatisch patchen.

In het volgende voorbeeld wordt PowerShell gebruikt voor het configureren van automatisch patchen op een bestaande SQL Server-VM. De **New-AzVMSqlServerAutoPatchingConfig** opdracht configureert u een nieuw onderhoudsvenster voor automatische updates.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = New-AzVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"
s Set-AzVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

> [!IMPORTANT]
> Als de extensie niet al is geïnstalleerd, opnieuw installeren van de extensie de SQL Server-service.

Op basis van dit voorbeeld, beschrijft de volgende tabel de praktische gevolgen zijn voor de doel-Azure-VM:

| Parameter | Effect |
| --- | --- |
| **DayOfWeek** |Patches elke donderdag geïnstalleerd. |
| **MaintenanceWindowStartingHour** |Begin updates om 11:00 uur. |
| **MaintenanceWindowsDuration** |Patches moeten worden geïnstalleerd binnen 120 minuten. Op basis van de begintijd, moeten ze voltooien door 13:00 uur. |
| **PatchCategory** |De enige mogelijke instellen voor deze parameter is **belangrijk**. Hiermee installeert u Windows update is gemarkeerd als belangrijk; het wordt niet geïnstalleerd op alle SQL Server-updates die niet zijn opgenomen in deze categorie. |

Het kan enkele minuten om te installeren en configureren van de SQL Server IaaS Agent duren.

Als u wilt uitschakelen automatisch patchen, Voer hetzelfde script zonder de **-inschakelen** parameter voor de **New-AzVMSqlServerAutoPatchingConfig**. Het ontbreken van de **-inschakelen** parameter geeft u de opdracht uit om de functie uit te schakelen.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere beschikbare automatiseringstaken [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

