---
title: Aangepaste rollen maken in azure AD-op rollen gebaseerd toegangs beheer | Microsoft Docs
description: Aangepaste Azure AD-rollen maken en toewijzen met resource bereik op Azure Active Directory resources.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e172a023cc9156f435b4f40b2262ee44128c138e
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84732001"
---
# <a name="create-and-assign-a-custom-role-in-azure-active-directory"></a>Een aangepaste rol maken en toewijzen in Azure Active Directory

In dit artikel wordt beschreven hoe u nieuwe aangepaste rollen maakt in Azure Active Directory (Azure AD). Zie [overzicht van aangepaste functies](roles-custom-overview.md)voor de basis beginselen van aangepaste rollen. De rol kan alleen worden toegewezen op het bereik van de directory-niveau of een bron bereik voor app-registratie.

U kunt aangepaste rollen maken op het tabblad [rollen en beheerders](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) op de overzichts pagina van Azure AD.

## <a name="create-a-role-in-the-azure-portal"></a>Een rol maken in de Azure Portal

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Een nieuwe aangepaste rol maken voor het verlenen van toegang voor het beheren van app-registraties

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com)   met privileged Role Administrator of Global Administrator Permissions in de Azure AD-organisatie.
1. Selecteer **Azure Active Directory**  >  **rollen en beheerders**  >  **nieuwe aangepaste rol**.

   ![Rollen maken of bewerken op de pagina rollen en beheerders](./media/roles-create-custom/new-custom-role.png)

1. Geef op het tabblad **basis beginselen** een naam en beschrijving op voor de rol en klik vervolgens op **volgende**.

   ![Geef een naam en beschrijving op voor een aangepaste rol op het tabblad basis beginselen](./media/roles-create-custom/basics-tab.png)

1. Selecteer op het tabblad **machtigingen** de machtigingen die nodig zijn voor het beheren van basis eigenschappen en referentie-eigenschappen van app-registraties. Zie [subtypen voor toepassings registratie en machtigingen in azure Active Directory](./roles-custom-available-permissions.md)voor een gedetailleerde beschrijving van elke machtiging.
   1. Voer eerst "referenties" in de zoek balk in en selecteer de `microsoft.directory/applications/credentials/update` machtiging.

      ![De machtigingen voor een aangepaste rol selecteren op het tabblad Machtigingen](./media/roles-create-custom/permissions-tab.png)

   1. Voer vervolgens ' Basic ' in de zoek balk in, selecteer de `microsoft.directory/applications/basic/update` machtiging en klik vervolgens op **volgende**.
1. Controleer op het tabblad **controleren en maken** de machtigingen en selecteer **maken**.

Uw aangepaste rol wordt weer gegeven in de lijst met beschik bare rollen die moeten worden toegewezen.

## <a name="create-a-role-using-powershell"></a>Een rol maken met Power shell

### <a name="prepare-powershell"></a>Power shell voorbereiden

Eerst moet u [de Azure ad preview Power shell-module downloaden](https://www.powershellgallery.com/packages/AzureADPreview).

Als u de Azure AD Power shell-module wilt installeren, gebruikt u de volgende opdrachten:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Als u wilt controleren of de module gereed is voor gebruik, gebruikt u de volgende opdracht:

``` PowerShell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.2.31     azuread                      {Add-AzureADAdministrati...}
```

### <a name="create-the-custom-role"></a>De aangepaste rol maken

Maak een nieuwe rol met behulp van het volgende Power shell-script:

``` PowerShell
# Basic role information
$displayName = "Application Support Administrator"
$description = "Can manage basic aspects of application registrations."
$templateId = (New-Guid).Guid
 
# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$rolePermissions = @{'allowedResourceActions'= $allowedResourceAction}
 
# Create new custom admin role
$customAdmin = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>De aangepaste rol toewijzen met behulp van Azure AD Power shell

Wijs de rol toe met behulp van het onderstaande Power shell-script:

``` PowerShell
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'cburl@f128.info'"
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Application Support Administrator'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'f/128 Filter Photos'"
$resourceScope = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope $resourceScope -RoleDefinitionId $roleDefinition.Id -PrincipalId $user.objectId
```

## <a name="create-a-role-with-graph-api"></a>Een rol maken met Graph API

1. De roldefinitie maken.

    HTTP-aanvraag voor het maken van een aangepaste roldefinitie.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
    ```

    Hoofdtekst

    ``` HTTP
   {
       "description": "Can manage basic aspects of application registrations.",
       "displayName": "Application Support Administrator",
       "isEnabled": true,
       "templateId": "<GUID>",
       "rolePermissions": [
           {
               "allowedResourceActions": [
                   "microsoft.directory/applications/basic/update",
                   "microsoft.directory/applications/credentials/update"
               ]
           }
       ]
   }
    ```

  > [!Note]
  > De "ontbrekende templateid": "GUID" is een optionele para meter die wordt verzonden in de hoofd tekst, afhankelijk van de vereiste. Als u een vereiste hebt voor het maken van meerdere verschillende aangepaste rollen met algemene para meters, kunt u het beste een sjabloon maken en een ontbrekende templateid definiëren. U kunt vooraf een ontbrekende templateid genereren met behulp van de Power shell-cmdlet (New-GUID). GPT. 

1. Maak de roltoewijzing.

    HTTP-aanvraag voor het maken van een aangepaste roldefinitie.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
    ```

    Hoofdtekst

    ``` HTTP
   {
       "principalId":"<GUID OF USER>",
       "roleDefinitionId":"<GUID OF ROLE DEFINITION>",
       "resourceScope":"/<GUID OF APPLICATION REGISTRATION>"
   }
    ```


## <a name="assign-a-custom-role-scoped-to-a-resource"></a>Een aangepast rollen bereik toewijzen aan een resource

Net als ingebouwde rollen worden aangepaste rollen standaard toegewezen op basis van het standaard organisatie bereik om toegangs machtigingen te verlenen voor alle app-registraties in uw organisatie. Maar in tegens telling tot ingebouwde rollen kunnen aangepaste rollen ook worden toegewezen in het bereik van één Azure AD-resource. Hierdoor kunt u de gebruiker toestemming geven om referenties en basis eigenschappen van één app bij te werken zonder dat u een tweede aangepaste rol hoeft te maken.

1. Meld u aan bij het [Azure AD-beheer centrum](https://aad.portal.azure.com) met toepassings ontwikkelaars machtigingen in de Azure AD-organisatie.
1. Selecteer **App-registraties**.
1. Selecteer de app-registratie waaraan u toegang wilt verlenen. Mogelijk moet u **alle toepassingen** selecteren om de volledige lijst van app-registraties in uw Azure AD-organisatie weer te geven.

    ![Selecteer de app-registratie als een resource bereik voor een roltoewijzing](./media/roles-create-custom/appreg-all-apps.png)

1. Selecteer in de app-registratie **rollen en beheerders**. Als u er nog geen hebt gemaakt, vindt u de instructies in de [voor gaande procedure](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations).

1. Selecteer de rol voor het openen van de pagina **toewijzingen** .
1. Selecteer **toewijzing toevoegen** om een gebruiker toe te voegen. Aan de gebruiker worden alleen machtigingen verleend via de geselecteerde app-registratie.

## <a name="next-steps"></a>Volgende stappen

- U kunt graag delen met ons op het [forum van Azure AD-beheerders](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
- Zie [beheerders rollen toewijzen](directory-assign-admin-roles.md)voor meer informatie over functies en de toewijzing van beheerdersrol.
- Zie voor standaard gebruikers machtigingen een [vergelijking van de standaard machtigingen voor gast-en gebruikers rechten](../fundamentals/users-default-permissions.md).
