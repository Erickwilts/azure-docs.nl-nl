---
title: De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory | Microsoft Docs
description: De toegang tot de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b6524a757d885e95637cb05480838db8ac37259
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67701933"
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory

Het is gemakkelijk om te verwijderen van een gebruiker of een groep van toegewezen toegang op een van uw bedrijfstoepassingen in Azure Active Directory (Azure AD). U moet de juiste machtigingen voor het beheren van de enterprise-app. En u moet globale beheerder voor de map.

> [!NOTE]
> Voor Microsoft Applications (zoals Office 365-apps), moet u PowerShell gebruiken om gebruikers met een enterprise-app te verwijderen.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-in-the-azure-portal"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app in Azure portal?

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
1. Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
1. Op de **Azure Active Directory - *directoryname***  pagina (dat wil zeggen, de Azure AD-pagina voor de map die u beheert), selecteer **bedrijfstoepassingen**.
1. Op de **bedrijfstoepassingen - alle toepassingen** pagina, ziet u een lijst van de apps die u kunt beheren. Selecteer een app.
1. Op de ***appname*** overzichtspagina (dat wil zeggen, de pagina met de naam van de geselecteerde app in de titel), selecteer **gebruikers en groepen**.
1. Op de ***appname*** **-gebruiker & groepstoewijzing** pagina, selecteer een van de meer gebruikers of groepen en selecteer vervolgens de **verwijderen** opdracht. Controleer of uw beslissing bij de prompt.

## <a name="how-do-i-remove-a-user-or-group-assignment-to-an-enterprise-app-using-powershell"></a>Hoe verwijder ik een gebruiker of groepstoewijzing aan een enterprise-app met behulp van PowerShell?

1. Open een opdrachtprompt met verhoogde bevoegdheid Windows PowerShell.

   > [!NOTE]
   > U moet de AzureAD-module installeren (Gebruik de opdracht `Install-Module -Name AzureAD`). Als u hierom wordt gevraagd om een NuGet-module of de nieuwe Azure Active Directory V2 PowerShell-module te installeren, typt u Y en druk op ENTER.

1. Voer `Connect-AzureAD` en meld u aan met een globale beheerdersaccount voor de gebruiker.
1. Het volgende script gebruiken om te verwijderen van een gebruiker en de rol van een toepassing:

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ```

## <a name="next-steps"></a>Volgende stappen

- [Zie alle Mijn groepen](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
- [Gebruikersaanmeldingen voor een zakelijke app uitschakelen](disable-user-sign-in-portal.md)
- [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)
