---
title: Veel toepassingen om een Azure-sleutelkluis - Azure Key Vault toegang te verlenen tot | Microsoft Docs
description: Leer hoe u toestemming te verlenen voor veel toepassingen toegang tot een key vault
services: key-vault
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: b1d0b0948e089d41f460ac2a54150ee51333f87c
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "64721996"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Verschillende toepassingen toegang verlenen tot een key vault

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Beleid voor toegangsbeheer kan worden gebruikt om verschillende toepassingen toegang verlenen tot een key vault. Een beleid voor toegangsbeheer kan maximaal 1024 toepassingen ondersteunen en is als volgt geconfigureerd:

1. Een Azure Active Directory-beveiligingsgroep maken. 
2. Voeg alle van de toepassingen die de service-principals aan de beveiligingsgroep die is gekoppeld.
3. De beveiliging groepstoegang verlenen tot uw Key Vault.

## <a name="prerequisites"></a>Vereisten

Hier volgen de vereisten:
* [Installeer Azure PowerShell](/powershell/azure/overview).
* [Installeer de Azure Active Directory V2 PowerShell-module](https://www.powershellgallery.com/packages/AzureAD).
* Machtigingen voor groepen in de Azure Active Directory-tenant maken/bewerken. Als u geen machtigingen hebt, moet u mogelijk contact op met uw Azure Active Directory-beheerder. Zie [over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md) voor meer informatie over Key Vault-beleid toegangsmachtigingen.

## <a name="granting-key-vault-access-to-applications"></a>Key Vault toegang verlenen tot toepassingen

Voer de volgende opdrachten uit in PowerShell:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Als u een andere set machtigingen aan een groep van toepassingen te verlenen wilt, maakt u een afzonderlijke Azure Active Directory-beveiligingsgroep voor deze toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [uw key vault beveiligen](key-vault-secure-your-key-vault.md).
