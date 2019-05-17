---
title: Identiteit voor Azure-app maken met PowerShell | Microsoft Docs
description: Hierin wordt beschreven hoe u met Azure PowerShell een Azure Active Directory-toepassing en service-principal maakt en toegang tot resources verleent via toegangsbeheer op basis van rollen. Het laat zien hoe u de toepassing verifieert met een certificaat.
services: active-directory
documentationcenter: na
author: rwike77
manager: CelesteDG
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/24/2018
ms.author: ryanwi
ms.reviewer: tomfitz
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6d6de5186b1906d56b5a43317d9c36ad1cc6aad
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540400"
---
# <a name="how-to-use-azure-powershell-to-create-a-service-principal-with-a-certificate"></a>Procedure: Azure PowerShell gebruiken om een service-principal met een certificaat te maken

Wanneer u een app of een script hebt waarvoor toegang tot resources vereist is, kunt u een identiteit voor de app instellen en de app met eigen referenties verifiëren. Deze identiteit staat bekend als een service-principal. Hiermee kunt u het volgende doen:

* Andere machtigingen aan de identiteit van de app toekennen dan uw eigen machtigingen. Deze machtigingen worden meestal beperkt tot de functies die met de app moeten kunnen worden uitgevoerd.
* Een certificaat voor verificatie gebruiken bij het uitvoeren van een onbewaakt script.

> [!IMPORTANT]
> In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor de identiteit van uw toepassingen. Als uw code wordt uitgevoerd op een service die ondersteuning biedt voor beheerde identiteiten en toegang tot bronnen die ondersteuning bieden voor verificatie van Azure Active Directory (Azure AD), zijn beheerde identiteiten een betere optie voor u. Zie voor meer informatie over beheerde identiteiten voor een Azure-resources, met inbegrip van welke services momenteel, [wat is beheerde identiteiten voor Azure-resources?](../managed-identities-azure-resources/overview.md).

In dit artikel leest u hoe een service-principal maakt die zichzelf verifieert met een certificaat. Zie [Een Azure-service-principal maken met Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps) voor het instellen van een service-principal met een wachtwoord.

Voor dit artikel hebt u de [meest recente versie](/powershell/azure/install-az-ps) van PowerShell nodig.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt in dit artikel hebt voltooid, moet u voldoende machtigingen hebben in uw Azure AD en Azure-abonnement. Specifiek, moet u een app maken in de Azure AD en de service-principal toewijzen aan een rol zijn.

De eenvoudigste manier om te controleren of uw account over de juiste machtigingen beschikt, verloopt via de portal. Zie [Check required permission](howto-create-service-principal-portal.md#required-permissions) (Vereiste machtiging controleren).

## <a name="create-service-principal-with-self-signed-certificate"></a>Service-principal met een zelfondertekend certificaat maken

In het volgende voorbeeld wordt een eenvoudig scenario behandeld. Hierbij [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal) een service-principal maken met een zelfondertekend certificaat en maakt gebruik van [New-AzureRmRoleAssignment](/powershell/module/az.resources/new-azroleassignment) om toe te wijzen de [Inzender](../../role-based-access-control/built-in-roles.md#contributor) de rol aan de service-principal. De roltoewijzing is afgestemd op uw huidige, geselecteerde Azure-abonnement. Selecteer een ander abonnement, gebruikt u [Set AzContext](/powershell/module/Az.Accounts/Set-AzContext).

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" `
  -Subject "CN=exampleappScriptCert" `
  -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzADServicePrincipal -DisplayName exampleapp `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
Sleep 20
New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Het voorbeeld van de inactieve modus ingeschakeld gedurende 20 seconden om toe te staan totdat de nieuwe service principal doorgeven in Azure AD. Als uw script niet lang genoeg wachten, ziet u een foutbericht weergegeven: "Principal {ID} bestaat niet in de map {DIR-ID}." U kunt deze fout oplossen, wacht u even voert u de **New-AzRoleAssignment** opdracht opnieuw uit.

U kunt de roltoewijzing beperken tot een specifieke resourcegroep met behulp van de parameter **ResourceGroupName**. Of tot een specifieke resource met behulp van de parameters **ResourceType** en **ResourceName**. 

Als u **niet beschikt over Windows 10 of Windows Server 2016**, moet u het [programma voor het genereren van zelfondertekende certificaten](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) downloaden van Microsoft Script Center. Pak de inhoud uit en importeer de cmdlet die u nodig hebt.

```powershell
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```

Vervang de volgende twee regels in het script om het certificaat te genereren.

```powershell
New-SelfSignedCertificateEx -StoreLocation CurrentUser `
  -Subject "CN=exampleapp" `
  -KeySpec "Exchange" `
  -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Certificaat opgeven via een automatisch PowerShell-script

Wanneer u zich aanmeldt als een service-principal, moet u de tenant-ID van de map voor uw AD-app opgeven. Er is een exemplaar van Azure AD voor een tenant.

```powershell
$TenantId = (Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
$ApplicationId = (Get-AzADApplication -DisplayNameStartWith exampleapp).ApplicationId

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -eq "CN=exampleappScriptCert" }).Thumbprint
 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Service-principal met certificaat van certificeringsinstantie maken

In het volgende voorbeeld wordt een service-principal gemaakt met behulp van een certificaat dat is uitgegeven door een certificeringsinstantie. De toewijzing is afgestemd op het opgegeven Azure-abonnement. De service-principal wordt toegevoegd aan de rol [Inzender](../../role-based-access-control/built-in-roles.md#contributor). Als er een fout optreedt bij het toewijzen van de rol, wordt het opnieuw geprobeerd.

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Connect-AzAccount
 Import-Module Az.Resources
 Set-AzContext -Subscription $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Certificaat opgeven via een automatisch PowerShell-script
Wanneer u zich aanmeldt als een service-principal, moet u de tenant-ID van de map voor uw AD-app opgeven. Er is een exemplaar van Azure AD voor een tenant.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object `
  -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 `
  -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Connect-AzAccount -ServicePrincipal `
  -CertificateThumbprint $Thumbprint `
  -ApplicationId $ApplicationId `
  -TenantId $TenantId
```

De toepassings-id en tenant-id zijn geen gevoelige gegevens, dus u kunt deze rechtstreeks in uw script insluiten. Als u de tenant-id wilt ophalen, gebruikt u:

```powershell
(Get-AzSubscription -SubscriptionName "Contoso Default").TenantId
```

Als u de toepassings-id wilt ophalen, gebruikt u:

```powershell
(Get-AzADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Referenties wijzigen

Als u wilt wijzigen van de referenties voor een AD-app, hetzij vanwege een inbreuk op de beveiliging of een verlopen van referenties, gebruikt u de [Remove-AzADAppCredential](/powershell/module/az.resources/remove-azadappcredential) en [New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential) cmdlets.

Als u alle referenties voor een toepassing wilt verwijderen, gebruikt u:

```powershell
Get-AzADApplication -DisplayName exampleapp | Remove-AzADAppCredential
```

Als u een waarde voor een certificaat wilt toevoegen, maakt u een zelfondertekend certificaat zoals aangegeven in dit artikel. Vervolgens gebruikt u:

```powershell
Get-AzADApplication -DisplayName exampleapp | New-AzADAppCredential `
  -CertValue $keyValue `
  -EndDate $cert.NotAfter `
  -StartDate $cert.NotBefore
```

## <a name="debug"></a>Fouten opsporen

Bij het maken van een service-principal kunnen de volgende foutmeldingen worden weergegeven:

* **Authentication_Unauthorized** (Verificatie geweigerd) of **No subscription found in the context.** (Geen abonnement gevonden in de context.) -U deze fout wordt weergegeven wanneer u uw account beschikt niet over de [vereiste machtigingen](#required-permissions) op de Azure AD om een app te registreren. Normaal gesproken ziet u deze fout wanneer alleen beheerder gebruikers in uw Azure Active Directory-apps kunnen registreren, en uw account niet een beheerder. Vraag uw beheerder om u de rol van beheerder toe te wijzen, of in te stellen dat gebruikers apps mogen registreren.

* Uw account **'heeft geen autorisatie om uit te voeren actie 'Microsoft.Authorization/roleAssignments/write' over scope '/ subscriptions / {guid}'.'**  -U deze fout wordt weergegeven wanneer u uw account beschikt niet over voldoende machtigingen voor een rol toewijzen aan een identiteit. Vraag de abonnementsbeheerder om u toe te voegen aan de rol Administrator voor gebruikerstoegang.

## <a name="next-steps"></a>Volgende stappen

* Zie [Een Azure-service-principal maken met Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps) voor het instellen van een service-principal met een wachtwoord.
* Zie de [ontwikkelaarshandleiding voor autorisatie met de Azure Resource Manager-API](../../azure-resource-manager/resource-manager-api-authentication.md) voor gedetailleerde stappen voor het integreren van een toepassing in Azure voor het beheren van resources.
* Zie [Toepassingsobjecten en service-principalobjecten](app-objects-and-service-principals.md) voor meer informatie over toepassingen en service-principals.
* Zie voor meer informatie over Azure AD-verificatie, [Verificatiescenario's voor Azure AD](authentication-scenarios.md).
