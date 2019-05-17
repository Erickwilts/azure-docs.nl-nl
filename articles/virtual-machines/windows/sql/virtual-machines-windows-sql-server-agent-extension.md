---
title: Automatiseer beheertaken op SQL-VM's (Resource Manager) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u voor het beheren van de SQL Server agent-extensie, welke specifieke SQL Server-beheertaken worden geautomatiseerd. Het gaat hierbij om automatische back-up, automatisch patchen en integratie van Azure Sleutelkluis.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 2943a1501e1d81ff4884c21b5aa3861e16523bbb
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827697"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>Automatiseer beheertaken op Azure Virtual Machines met de SQL Server Agent-extensie (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassiek](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

De SQL Server IaaS Agent-extensie (SqlIaasExtension) wordt uitgevoerd op virtuele Azure-machines beheertaken te automatiseren. Dit artikel bevat een overzicht van de services die worden ondersteund door de extensie, evenals de instructies voor installatie, status en verwijdering.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Voor de klassieke versie van dit artikel, raadpleegt u [SQL Server Agent-extensie voor SQL Server-VM's klassieke](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Ondersteunde services
De SQL Server IaaS Agent-extensie ondersteunt de volgende beheertaken:

| Beheer-functie | Description |
| --- | --- |
| **Automatische back-up van SQL** |Automatiseert de planning van de back-ups voor alle databases voor het standaardexemplaar of een [juist is geïnstalleerd](virtual-machines-windows-sql-server-iaas-faq.md#administration) benoemd exemplaar van SQL Server op de virtuele machine. Zie voor meer informatie, [automatische back-up voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisch patchen van SQL** |Hiermee configureert u een onderhoudsvenster waarin belangrijke updates voor Windows met uw virtuele machine plaatsvinden kunnen, zodat u dat updates tijdens piektijden voor uw workload voorkomen kunt. Zie voor meer informatie, [automatische patches voor SQL Server in Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Integratie van Azure Key Vault** |Hiermee kunt u automatisch installeren en configureren van Azure Key Vault op uw SQL Server-VM. Zie voor meer informatie, [configureren van Azure Key Vault-integratie voor SQL Server op Azure Virtual machines (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Eenmaal geïnstalleerd en wordt uitgevoerd, beschikbaar de SQL Server IaaS Agent-extensie deze beheerfuncties van SQL Server in het venster van de virtuele machine in Azure portal en via Azure PowerShell voor SQL Server-marketplace-installatiekopieën en via Azure PowerShell voor handmatige installaties van de extensie. 

## <a name="prerequisites"></a>Vereisten
Vereisten voor het gebruik van de SQL Server IaaS Agent-extensie op de virtuele machine:

**Besturingssysteem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-versies**:

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**:

* [Downloaden en configureren van de meest recente Azure PowerShell-opdrachten](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> Op dit moment de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) wordt niet ondersteund voor FCI van SQL Server op Azure. Het is raadzaam dat u de extensie van virtuele machines die deel uitmaken van een FCI verwijderen. De functies die worden ondersteund door de extensie zijn niet beschikbaar voor de SQL-VM's nadat de agent is verwijderd.

## <a name="installation"></a>Installatie
De SQL Server IaaS Agent-extensie wordt automatisch geïnstalleerd wanneer u een van de galerie met installatiekopieën van SQL Server virtuele machine inricht. De SQL IaaS-extensie biedt beheerbaarheid voor één exemplaar van de SQL Server-VM. Als er een standaardexemplaar, klikt u vervolgens de extensie werkt met het standaardexemplaar en er geen ondersteuning voor het beheren van andere exemplaren. Als er geen standaard-exemplaar, maar slechts één benoemd exemplaar, wordt deze het benoemde exemplaar beheren. Als er geen standaardexemplaar is en er meerdere genoemde instanties zijn, mislukt de extensie te installeren. 



Als u moet de extensie handmatig installeren op een van deze SQL Server-VM's, gebruikt u de volgende PowerShell-opdracht:

```powershell
Set-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension" -Version "2.0" -Location "East US 2"
```

> [!WARNING]
> Als de extensie niet al is geïnstalleerd, opnieuw installeren van de extensie de SQL Server-service. Bijwerken van de SQL IaaS-extensie heeft echter niet start de SQL Server-service. 

> [!NOTE]
> Hoewel het mogelijk voor het installeren van de SQL Server IaaS Agent-extensie voor aangepaste SQL Server-installatiekopieën, de functionaliteit is momenteel beperkt tot [wijzigen van het licentietype](virtual-machines-windows-sql-ahb.md). Andere functies van de SQL IaaS-extensie werkt alleen op [installatiekopieën van SQL Server-VM-galerie](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms) (betalen per gebruik of bring-your-own-license).

### <a name="use-a-single-named-instance"></a>Een enkele benoemd exemplaar
De SQL IaaS-extensie werkt met een benoemd exemplaar op een SQL Server-installatiekopie als het standaardexemplaar correct wordt verwijderd, en als de IaaS-extensie is geïnstalleerd.

Voor het gebruik van een benoemd exemplaar van SQL Server, het volgende doen:
   1. Implementeer een SQL Server-VM vanuit de marketplace. 
   1. De IaaS-extensie verwijderen vanuit de [Azure-portal](https://portal.azure.com).
   1. Verwijder de SQL-Server volledig in de SQL Server-VM.
   1. Installeer SQL Server in een benoemd exemplaar in de SQL Server-VM. 
   1. Installeer de IaaS-extensie van de Azure-portal.  

## <a name="status"></a>Status
Er is één manier om te controleren of de extensie is geïnstalleerd om de agentstatus in Azure portal weer te geven. Selecteer **alle instellingen** in het venster van de virtuele machine en klik vervolgens op **extensies**. U ziet de **SqlIaasExtension** extensie is die weergegeven.

![SQL Server IaaS Agent-extensie in Azure portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

U kunt ook de **Get-AzVMSqlServerExtension** Azure PowerShell-cmdlet.

    Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

De vorige opdracht wordt bevestigd dat de agent is geïnstalleerd en biedt algemene statusinformatie. U kunt ook specifieke statusinformatie over automatische back-up en patchen met de volgende opdrachten ophalen.

    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Verwijdering
In de Azure-portal, kunt u de extensie verwijderen door te klikken op het beletselteken op de **extensies** venster van de eigenschappen van uw virtuele machine. Klik vervolgens op **Verwijderen**.

![Verwijderen van de SQL Server IaaS Agent-extensie in Azure portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

U kunt ook de **Remove-AzVMSqlServerExtension** PowerShell-cmdlet.

    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"

## <a name="next-steps"></a>Volgende stappen
Beginnen met een van de services die worden ondersteund door de extensie. Zie voor meer informatie de artikelen waarnaar wordt verwezen in de [ondersteunde services](#supported-services) sectie van dit artikel.

Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual Machines [SQL Server op Azure Virtual Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).

