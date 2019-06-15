---
title: Het configureren van beheerde identiteiten voor Azure-resources op een virtuele-machineschaalset instellen met behulp van PowerShell
description: Stapsgewijze instructies voor het configureren van een systeem en de gebruiker toegewezen beheerde identiteiten op een virtuele-machineschaalset instellen met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4917720af2396b68ccd36cc0410c9acbbba2d9b2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60304558"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>Configureren van beheerde identiteiten voor Azure-resources op virtuele-machineschaalsets met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van PowerShell, u hoe u de beheerde identiteit voor bewerkingen van de Azure-resources op een virtuele-machineschaalset uitvoeren:
- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit op een virtuele-machineschaalset
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit op een virtuele-machineschaalset

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een door het systeem toegewezen en beheerd gebruiker toegewezen identiteit](overview.md#how-does-it-work)** .
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) instellen voor een virtuele-machineschaalset maken en inschakelen en verwijderen door het systeem toegewezen beheerd en/of de gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele-machineschaalset.
- Installeer [de meest recente versie van Azure PowerShell](/powershell/azure/install-az-ps) als u dat nog niet gedaan hebt. 

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een systeem toegewezen beheerde identiteit met Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een schaalset voor virtuele Azure-machine inschakelen

Een virtuele-machineschaalset met de ingeschakeld door het systeem toegewezen beheerde identiteit maken:

1. Raadpleeg *voorbeeld 1* in de [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) cmdlet-verwijzing artikel te maken van een virtuele-machineschaalset instellen met een door het systeem toegewezen beheerde identiteit.  De parameter toevoegen `-IdentityType SystemAssigned` naar de `New-AzVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```
> [!NOTE]
> U mag de beheerde identiteiten (optioneel) inrichten voor Azure-resources virtuele-uitbreiding machineschaalset, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [stoppen met het gebruik van de VM-extensie en start met behulp van het eindpunt van de Azure-IMDS voor verificatie](howto-migrate-vm-extension.md).


## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>De beheerde inschakelen door het systeem toegewezen identiteit op een bestaande schaalset voor virtuele machine van Azure

Als u nodig hebt om in te schakelen van een systeem toegewezen beheerde identiteit op een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de [ `Get-AzVmss` ](/powershell/module/az.compute/get-azvmss) cmdlet. Gebruik vervolgens om in te schakelen op een beheerde identiteit voor het systeem is toegewezen, de `-IdentityType` overschakelen op de [Update AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

> [!NOTE]
> U mag de beheerde identiteiten (optioneel) inrichten voor Azure-resources virtuele-uitbreiding machineschaalset, maar wordt binnenkort worden afgeschaft. Het is raadzaam om het eindpunt van Azure Instance Metadata identiteit gebruiken voor verificatie. Zie voor meer informatie, [migreren van de VM-extensie naar Azure IMDS eindpunt voor de verificatie](howto-migrate-vm-extension.md).

### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>De door het systeem toegewezen beheerde identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u een virtuele-machineschaalset die niet meer nodig is op het systeem toegewezen beheerde identiteit maar nog steeds moet gebruiker toegewezen identiteiten op beheerde hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

2. Voer de volgende cmdlet uit:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Als u een virtuele-machineschaalset die beheerde identiteit systeem toegewezen niet meer nodig hebt en er geen beheerde gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Door een gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een VM-schaalset met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een schaalset voor virtuele Azure-machine

Het maken van een nieuwe virtuele-machineschaalset met een gebruiker toegewezen beheerde identiteit wordt momenteel niet ondersteund via PowerShell. Zie de volgende sectie over het toevoegen van een beheerde identiteit voor de gebruiker toegewezen aan een bestaande virtuele-machineschaalset. Controleer later op updates.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine

Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Connect-AzAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de `Get-AzVM` cmdlet. Als u wilt een beheerde identiteit voor de gebruiker toegewezen aan de virtuele-machineschaalset toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet. Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` door uw eigen waarden.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit verwijderen uit een schaalset voor virtuele Azure-machine

Als uw virtuele-machineschaalset meerdere gebruiker toegewezen beheerde identiteiten heeft, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VIRTUAL MACHINE SCALE SET NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY NAME>` is van de gebruiker toegewezen beheerde identiteit van de naam van eigenschap, die op de virtuele-machineschaalset moet blijven. Deze informatie kan worden gevonden in de sectie van de identiteit van de VM-schaalset met behulp van `az vmss show`:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Als uw virtuele-machineschaalset beschikt niet over een systeem toegewezen beheerd wilt identiteits- en u alle beheerde identiteiten die door de gebruiker toegewezen van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Als uw virtuele-machineschaalset beide door het systeem is toegewezen en gebruiker toegewezen beheerde identiteiten, kunt u alle de gebruiker toegewezen beheerde identiteiten verwijderen door over te schakelen voor het gebruik van alleen het systeem toegewezen beheerde identiteit.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















