---
title: Een Recovery Services-kluis geconfigureerd voor de Azure Site Recovery-service verwijderen
description: Meer informatie over het verwijderen van een Recovery Services-kluis geconfigureerd voor Azure Site Recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajani-janaki-ram
ms.openlocfilehash: 981b78345a0d9ea589e9c39ddaa2e253f1dd343f
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412843"
---
# <a name="delete-a-site-recovery-services-vault"></a>Een Site Recovery Services-kluis verwijderen

Afhankelijkheden kunnen voorkomen dat u een Azure Site Recovery-kluis verwijderen. De acties die u moet uitvoeren, afhankelijk van het scenario voor Site Recovery. Als u wilt verwijderen van een kluis die wordt gebruikt in Azure Backup, Zie [verwijderen van een back-upkluis in Azure](../backup/backup-azure-delete-vault.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-a-site-recovery-vault"></a>Een Site Recovery-kluis verwijderen 
Als de kluis verwijderen, gaat u als volgt de aanbevolen stappen voor uw scenario.
### <a name="azure-vms-to-azure"></a>Azure-VM's naar Azure

1. Verwijder alle beveiligde VM's met de volgende stappen in [Schakel de beveiliging voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure).
2. Verwijder de kluis.

### <a name="vmware-vms-to-azure"></a>VMware-VM’s naar Azure

1. Verwijder alle beveiligde VM's met de volgende stappen in [Schakel de beveiliging voor een VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. Alle replicatiebeleidsregels voor verwijderen met de volgende stappen in [een replicatiebeleid verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy).

3. Verwijzingen naar vCenter verwijderen met de volgende stappen in [een vCenter-server verwijderen](vmware-azure-manage-vcenter.md#delete-a-vcenter-server).

4. De configuratieserver verwijderen met de volgende stappen in [uit bedrijf nemen van een configuratieserver](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Verwijder de kluis.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Virtuele Hyper-V-machines (met VMM) naar Azure
1. Verwijder alle beveiligde VM's met de volgende stappen in[Schakel de beveiliging voor een Hyper-V-VM (met VMM)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. Ontkoppelen en verwijderen van alle replicatiebeleidsregels voor door te bladeren naar de kluis -> **Site Recovery-infrastructuur** -> **voor System Center VMM** -> **replicatie Beleid**

3.  Verwijzingen naar de VMM-servers verwijderen met de volgende stappen in [registratie ongedaan maken van een VMM-server verbonden](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Verwijder de kluis.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Hyper-V-machines (zonder Virtual Machine Manager) naar Azure
1. Verwijder alle beveiligde VM's met de volgende stappen in [Schakel de beveiliging voor een Hyper-V virtuele machine (Hyper-V naar Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Ontkoppelen en verwijderen van alle replicatiebeleidsregels voor door te bladeren naar de kluis -> **Site Recovery-infrastructuur** -> **voor Hyper-V-Sites** -> **replicatiebeleid**

3. Verwijzingen naar Hyper-V-servers verwijderen met de volgende stappen in [registratie ongedaan maken van een Hyper-V-host](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site).

4. De Hyper-V-site verwijderen.

5. Verwijder de kluis.


## <a name="use-powershell-to-force-delete-the-vault"></a>Gebruik PowerShell om af te dwingen de kluis verwijderen 

> [!Important]
> Als u het product test en niet betrokken zijn bij verlies van gegevens, gebruikt u de force-delete-methode aan de kluis en alle bijbehorende afhankelijkheden snel verwijderd.
> De PowerShell-opdracht wordt de inhoud van de kluis verwijderd en is **niet ongedaan worden gemaakt**.

Als de Site Recovery-kluis verwijderen, zelfs als er beveiligde items, gebruikt u deze opdrachten:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

Meer informatie over [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault), en [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault).
