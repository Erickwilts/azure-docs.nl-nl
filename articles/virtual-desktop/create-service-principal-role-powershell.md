---
title: Windows virtuele bureaublad Preview-service-principals en roltoewijzingen maken met PowerShell - Azure
description: Het service-principals maken en toewijzen van rollen met PowerShell in Windows Virtual Desktop Preview.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 04/12/2019
ms.author: helohr
ms.openlocfilehash: d3357cec426585ba8550301dfa703f583a930ad0
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236942"
---
# <a name="tutorial-create-service-principals-and-role-assignments-with-powershell"></a>Zelfstudie: Service-principals en roltoewijzingen maken met PowerShell

Service-principals zijn identiteiten die u in Azure Active Directory maken kunt voor het toewijzen van rollen en machtigingen voor een specifiek doel. In Windows Virtual Desktop Preview, kunt u een service principal te maken:

- Specifieke virtuele Windows-bureaublad-beheertaken automatiseren
- Gebruiken als de referenties in plaats van gebruikers MFA is vereist bij het uitvoeren van een Windows virtuele bureaublad Azure Resource Manager-sjabloon

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een service-principal maken in Azure Active Directory
> * Een roltoewijzing maken in een virtuele Windows-bureaublad
> * Aanmelden bij virtuele Windows-bureaublad met de service-principal

## <a name="prerequisites"></a>Vereisten

Voordat u service-principals en roltoewijzingen maken kunt, moet u drie dingen doen:

1. Installeer de module AzureAD. Voor het installeren van de module PowerShell ook uitvoeren als beheerder en voer de volgende cmdlet uit:

    ```powershell
    Install-Module AzureAD
    ```

2. Voer de volgende cmdlets met de waarden in de aanhalingstekens vervangen door de waarden die relevant zijn voor uw sessie. Als u zojuist hebt gemaakt de tenant van uw virtuele Windows-bureaublad van de [een tenant maken in de zelfstudie voor virtuele Windows-bureaublad](./tenant-setup-azure-active-directory.md), gebruikt u 'Standaard-Tenant groeperen' als de naam van uw tenant.

    ```powershell
    $myTenantGroupName = "<my-tenant-group-name>"
    $myTenantName = "<my-tenant-name>"
    ```

3. Volg alle instructies in dit artikel in dezelfde PowerShell-sessie. U werkt deze mogelijk niet als u het venster sluiten en terug naar het later opnieuw.

## <a name="create-a-service-principal-in-azure-active-directory"></a>Een service-principal maken in Azure Active Directory

Nadat u de vereisten hebt voldaan in de PowerShell-sessie, voer de volgende PowerShell-cmdlets voor het maken van een multitenant service principal in Azure.

```powershell
Import-Module AzureAD
$aadContext = Connect-AzureAD
$svcPrincipal = New-AzureADApplication -AvailableToOtherTenants $true -DisplayName "Windows Virtual Desktop Svc Principal"
$svcPrincipalCreds = New-AzureADApplicationPasswordCredential -ObjectId $svcPrincipal.ObjectId
```

## <a name="create-a-role-assignment-in-windows-virtual-desktop-preview"></a>Een roltoewijzing maken in Windows Virtual Desktop Preview

Nu dat u een service principal gemaakt hebt, kunt u het aanmelden bij virtuele Windows-bureaublad. Zorg ervoor dat u zich aanmelden met een account met machtigingen voor het maken van de roltoewijzing.

Eerste, [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

Voer de volgende PowerShell-cmdlets voor de verbinding met virtuele Windows-bureaublad en een roltoewijzing voor de service-principal maken.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
Set-RdsContext -TenantGroupName $myTenantGroupName
New-RdsRoleAssignment -RoleDefinitionName "RDS Owner" -ApplicationId $svcPrincipal.AppId -TenantGroupName $myTenantGroupName -TenantName $myTenantName
```

## <a name="sign-in-with-the-service-principal"></a>Meld u aan met de service-principal

Na het maken van een roltoewijzing voor de service principal, moet u controleren of dat de service-principal kan zich aanmelden bij virtuele Windows-bureaublad door de volgende cmdlet:

```powershell
$creds = New-Object System.Management.Automation.PSCredential($svcPrincipal.AppId, (ConvertTo-SecureString $svcPrincipalCreds.Value -AsPlainText -Force))
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com" -Credential $creds -ServicePrincipal -AadTenantId $aadContext.TenantId.Guid
```

Nadat u zich hebt aangemeld, zorg ervoor dat alles werkt door te testen van een paar Windows virtuele bureaublad PowerShell-cmdlets met de service-principal.

## <a name="view-your-credentials-in-powershell"></a>Uw referenties in PowerShell weergeven

Voordat u de PowerShell-sessie beëindigen, moet u uw referenties weergeven en schrijf ze op voor toekomstig gebruik. Het wachtwoord is vooral belangrijk omdat u niet meer ophalen nadat u deze PowerShell-sessie sluit.

Dit zijn de drie referenties die te noteren en de cmdlets die u moet uitvoeren om op te halen ze:

- Wachtwoord:

    ```powershell
    $svcPrincipalCreds.Value
    ```

- Tenant-id:

    ```powershell
    $aadContext.TenantId.Guid
    ```

- Toepassings-id:

    ```powershell
    $svcPrincipal.AppId
    ```

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt gemaakt van de service-principal en een rol in uw tenant virtuele Windows-bureaublad toegewezen, kunt u deze kunt gebruiken om een host-pool te maken. Voor meer informatie over pools host, verder met de zelfstudie voor het maken van een host van toepassingen in virtuele Windows-bureaublad.

 > [!div class="nextstepaction"]
 > [Zelfstudie voor virtuele Windows-bureaublad host pool](./create-host-pools-azure-marketplace.md)
