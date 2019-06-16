---
title: Instellingen voor met behulp van PowerShell - Azure Active Directory configureren | Microsoft Docs
description: Beheren hoe de instellingen voor groepen met behulp van Azure Active Directory-cmdlets
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/26/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5ccc4ef6c095eacd29590504d46756ead856574
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058619"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlets voor het configureren van groepsinstellingen
In dit artikel bevat instructies voor het gebruik van Azure Active Directory (Azure AD) PowerShell-cmdlets voor groepen maken en bijwerken. Deze inhoud geldt alleen voor Office 365-groepen (ook wel gecombineerde groepen). 

> [!IMPORTANT]
> Sommige instellingen voor nodig een Azure Active Directory Premium P1-licentie. Zie voor meer informatie de [sjablooninstellingen](#template-settings) tabel.

Voor meer informatie over hoe u om te voorkomen dat gebruikers die geen beheerder van het maken van beveiligingsgroepen instellen `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` zoals beschreven in [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Office 365-groepen instellingen worden geconfigureerd met behulp van een object-instellingen en een SettingsTemplate-object. In eerste instantie wordt er geen instellingenobjecten in uw directory omdat uw directory is geconfigureerd met de standaardinstellingen. Als u wilt de standaardinstellingen wijzigen, moet u een nieuwe instellingenobject met behulp van een sjabloon instellingen maken. Instellingen voor sjablonen zijn gedefinieerd door Microsoft. Er zijn verschillende sjablonen met verschillende instellingen. Office 365-groep om instellingen te configureren voor uw directory, moet u de sjabloon met de naam 'Group.Unified' gebruiken. Gebruik de sjabloon met de naam 'Group.Unified.Guest' voor informatie over het configureren van instellingen voor Office 365-groep op één groep. Deze sjabloon wordt gebruikt voor het beheren van toegang voor gasten voor een Office 365-groep. 

De cmdlets zijn onderdeel van de Azure Active Directory PowerShell V2-module. Voor instructies over het downloaden en installeren van de module op uw computer, Zie het artikel [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/azuread/). U kunt de release van versie 2 van de module op basis van installeren [de PowerShell gallery](https://www.powershellgallery.com/packages/AzureAD/).



## <a name="create-settings-at-the-directory-level"></a>Instellingen op het niveau van de map maken
Deze stappen maakt u instellingen op het niveau van de map die van toepassing op alle Office 365-groepen in de map zijn. De cmdlet Get-AzureADDirectorySettingTemplate is alleen beschikbaar in de [Preview van Azure AD PowerShell-module voor Graph](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. In de DirectorySettings-cmdlets, moet u de ID van de SettingsTemplate die u wilt gebruiken. Als u deze ID niet weet, wordt de lijst van alle instellingen voor sjablonen in deze cmdlet geretourneerd:
  
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
   Deze cmdlet-aanroep retourneert alle sjablonen die beschikbaar zijn:
  
   ```powershell
   Id                                   DisplayName         Description
   --                                   -----------         -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
   16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
   ```
2. Als u wilt toevoegen een URL van de richtlijn gebruik, moet u eerst het SettingsTemplate-object dat de waarde van de URL in de gebruik richtlijn; definieert ophalen dat wil zeggen, de sjabloon Group.Unified:
  
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Maak vervolgens een nieuwe instellingenobject op basis van die sjabloon:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Werk vervolgens de waarde van de richtlijn gebruik:
  
   ```powershell
   $Setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
   ```  
5. De instelling vervolgens toepassen:
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. U kunt de waarden met behulp van lezen:

   ```powershell
   $Setting.Values
   ```  
## <a name="update-settings-at-the-directory-level"></a>Update-instellingen op het niveau van de map
Voor het bijwerken van de waarde voor UsageGuideLinesUrl in de sjabloon instelling, gewoon Bewerk de URL in stap 4 hierboven, voer vervolgens stap 5 om in te stellen de nieuwe waarde.

Als u wilt verwijderen van de waarde van UsageGuideLinesUrl, bewerk de URL om te worden van een lege tekenreeks met behulp van stap 4 hierboven:

   ```powershell
   $Setting["UsageGuidelinesUrl"] = ""
   ```  
Voer stap 5 om in te stellen de nieuwe waarde.

## <a name="template-settings"></a>Sjablooninstellingen
Hier vindt u de instellingen die zijn gedefinieerd in de Group.Unified SettingsTemplate. Tenzij anders aangegeven, wordt met deze functies een Azure Active Directory Premium P1-licentie nodig. 

| **Instelling** | **Beschrijving** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Type: Boolean<li>Standaard: True |De vlag die aangeeft of Office 365-groep maken in de map is toegestaan door niet-beheerders. Deze instelling is niet vereist voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GroupCreationAllowedGroupId<li>Type: String<li>Standaard: ' " |GUID van de beveiligingsgroep waarvan de leden kunnen Office 365-groepen maken, zelfs wanneer EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Type: String<li>Standaard: ' " |Een koppeling naar de groepsgebruik. |
|  <ul><li>ClassificationDescriptions<li>Type: String<li>Standaard: ' " | Een door komma's gescheiden lijst met beschrijvingen van de classificatie. De waarde van ClassificationDescriptions is alleen geldig in deze indeling:<br>$setting[“ClassificationDescriptions”] ="Classification:Description,Classification:Description"<br>waar classificatie komt overeen met de tekenreeksen in de ClassificationList.|
|  <ul><li>DefaultClassification<li>Type: String<li>Standaard: ' " | De classificatie die moet worden gebruikt als de Standaardclassificatie voor een groep als niets is opgegeven.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Type: String<li>Standaard: ' " | Tekenreeks met een maximale lengte van 64 tekens die de naamconventie die is geconfigureerd voor Office 365-groepen definieert. Zie voor meer informatie, [afdwingen van een naamgevingsbeleid voor Office 365-groepen](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Type: String<li>Standaard: ' " | Door komma's gescheiden tekenreeks van zinnen die gebruikers niet mogen gebruiken in namen of aliassen. Zie voor meer informatie, [afdwingen van een naamgevingsbeleid voor Office 365-groepen](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Type: Boolean<li>Standaard: "False" | Gebruik geen
|  <ul><li>AllowGuestsToBeGroupOwner<li>Type: Boolean<li>Standaard: False | Booleaanse waarde waarmee wordt aangegeven of een gastgebruiker moet een eigenaar van de groepen kan worden. |
|  <ul><li>AllowGuestsToAccessGroups<li>Type: Boolean<li>Standaard: True | Booleaanse waarde waarmee wordt aangegeven of een gastgebruiker toegang tot inhoud voor Office 365-groepen kan hebben.  Deze instelling is niet vereist voor een Azure Active Directory Premium P1-licentie.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Type: String<li>Standaard: ' " | De url van een koppeling naar de richtlijnen voor het gebruik van Gast. |
|  <ul><li>AllowToAddGuests<li>Type: Boolean<li>Standaard: True | Een Booleaanse waarde die aangeeft of gasten toevoegen aan deze map is toegestaan of niet.|
|  <ul><li>ClassificationList<li>Type: String<li>Standaard: ' " |Een door komma's gescheiden lijst van geldige classificatiewaarden die kunnen worden toegepast op Office 365-groepen. |

## <a name="example-configure-guest-policy-for-groups-at-the-directory-level"></a>Voorbeeld: Gast-beleid voor groepen configureren op het niveau van de map
1. Alle sjablonen van de instelling ophalen:
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
2. Als u wilt instellen Gast voor groepen op het niveau van de map, moet u Group.Unified sjabloon
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. Maak vervolgens een nieuwe instellingenobject op basis van die sjabloon:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Werk vervolgens AllowToAddGuests instelling
   ```powershell
   $Setting["AllowToAddGuests"] = $False
   ```  
5. De instelling vervolgens toepassen:
  
   ```powershell
   Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
   ```
6. U kunt de waarden met behulp van lezen:

   ```powershell
   $Setting.Values
   ```   

## <a name="read-settings-at-the-directory-level"></a>Instellingen op het niveau van de map lezen

Als u bekend bent met de naam van de instelling die u wilt ophalen, kunt u de volgende cmdlet voor het ophalen van de huidige waarde van de instellingen. In dit voorbeeld zijn we ophalen van de waarde voor de instelling met de naam "UsageGuidelinesUrl." 

   ```powershell
   (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
   ```
Deze stappen lezen instellingen op het niveau van de map die van toepassing op alle Office-groepen in de map zijn.

1. Alle bestaande directoryinstellingen lezen:
   ```powershell
   Get-AzureADDirectorySetting -All $True
   ```
   Deze cmdlet retourneert een lijst van alle directoryinstellingen:
   ```powershell
   Id                                   DisplayName   TemplateId                           Values
   --                                   -----------   ----------                           ------
   c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
   ```

2. Lees alle instellingen voor een specifieke groep:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
   ```

3. Lees alle waarden in de map instellingen van een specifieke map instellingen-object met behulp van ID-GUID-instellingen:
   ```powershell
   (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
   ```
   Deze cmdlet retourneert de namen en waarden in dit instellingenobject voor deze specifieke groep:
   ```powershell
   Name                          Value
   ----                          -----
   ClassificationDescriptions
   DefaultClassification
   PrefixSuffixNamingRequirement
   CustomBlockedWordsList        
   AllowGuestsToBeGroupOwner     False 
   AllowGuestsToAccessGroups     True
   GuestUsageGuidelinesUrl
   GroupCreationAllowedGroupId
   AllowToAddGuests              True
   UsageGuidelinesUrl            https://guideline.example.com
   ClassificationList
   EnableGroupCreation           True
   ```

## <a name="remove-settings-at-the-directory-level"></a>Instellingen op het niveau van de map verwijderen
Deze stap worden de instellingen op het niveau van de map die van toepassing op alle Office-groepen in de map verwijderd.
   ```powershell
   Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
   ```

## <a name="create-settings-for-a-specific-group"></a>Instellingen voor een specifieke groep maken

1. Zoeken naar de instellingen-sjabloon met de naam "Groups.Unified.Guest"
   ```powershell
   Get-AzureADDirectorySettingTemplate
  
   Id                                   DisplayName            Description
   --                                   -----------            -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
   ```
2. Het ophalen van de sjabloonobject voor de sjabloon Groups.Unified.Guest:
   ```powershell
   $Template1 = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
   ```
3. Maak een nieuw instellingenobject van de sjabloon:
   ```powershell
   $SettingCopy = $Template1.CreateDirectorySetting()
   ```

4. De instelling ingesteld op de vereiste waarde:
   ```powershell
   $SettingCopy["AllowToAddGuests"]=$False
   ```
5. De ID van de groep die u wilt toepassen van deze instelling niet ophalen:
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
6. Maak de nieuwe instelling voor de vereiste groep in de map:
   ```powershell
   New-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -DirectorySetting $SettingCopy
   ```
7. Als u wilt controleren of de instellingen, moet u deze opdracht uitvoeren:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="update-settings-for-a-specific-group"></a>Instellingen voor een specifieke groep bijwerken
1. Haal de ID van de groep waarvan de instelling u wilt bijwerken:
   ```powershell
   $groupID= (Get-AzureADGroup -SearchString "YourGroupName").ObjectId
   ```
2. De instelling van de groep op te halen:
   ```powershell
   $Setting = Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
3. Werk de instelling van de groep als u nodig, bijvoorbeeld hebt
   ```powershell
   $Setting["AllowToAddGuests"] = $True
   ```
4. Haal vervolgens de ID van de instelling voor deze specifieke groep:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups
   ```
   U ontvangt een antwoord ongeveer als volgt uit:
   ```powershell
   Id                                   DisplayName            TemplateId                             Values
   --                                   -----------            -----------                            ----------
   2dbee4ca-c3b6-4f0d-9610-d15569639e1a Group.Unified.Guest    08d542b9-071f-4e16-94b0-74abb372e3d9   {class SettingValue {...
   ```
5. U kunt vervolgens de nieuwe waarde instellen voor deze instelling:
   ```powershell
   Set-AzureADObjectSetting -TargetType Groups -TargetObjectId $groupID -Id 2dbee4ca-c3b6-4f0d-9610-d15569639e1a -DirectorySetting $Setting
   ```
6. De waarde van de instelling om te controleren of dat deze correct is bijgewerkt, kunt u lezen:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId $groupID -TargetType Groups | fl Values
   ```

## <a name="cmdlet-syntax-reference"></a>Naslaginformatie over de syntaxis van cmdlets
U vindt meer Azure Active Directory PowerShell-documentatie op [Azure Active Directory-Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Meer lezen

* [Managing access to resources with Azure Active Directory groups](../fundamentals/active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
