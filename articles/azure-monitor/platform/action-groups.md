---
title: Actiegroepen maken en beheren in Azure Portal
description: Meer informatie over het maken en beheren van actiegroepen in de Azure Portal.
author: dkamstra
ms.topic: conceptual
ms.date: 07/28/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 72caeb60fc058b88158979d211a0bc38985975c7
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968854"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Actiegroepen maken en beheren in Azure Portal
Een actiegroep is een verzameling voorkeuren voor meldingen die zijn gedefinieerd door de eigenaar van een Azure-abonnement. Azure Monitor- en Service Health-waarschuwingen gebruiken actiegroepen om gebruikers te laten weten dat een waarschuwing is geactiveerd. Verschillende waarschuwingen kunnen dezelfde actiegroep of verschillende actiegroepen gebruiken, afhankelijk van de wensen van de gebruiker. 

In dit artikel wordt beschreven hoe u actiegroepen maakt en beheert in de Azure Portal.

Elke actie bestaat uit de volgende eigenschappen:

* **Type**: de melding of actie die is uitgevoerd. Voorbeelden hiervan zijn het verzenden van een spraakoproep, SMS, e-mail; of het activeren van verschillende typen geautomatiseerde acties. Zie typen verderop in dit artikel.
* **Naam**: een unieke id in de actiegroep.
* **Details**: de bijbehorende details die per *type* verschillen.

Zie voor meer informatie over het gebruik van Azure Resource Manager-sjablonen voor het configureren van actiegroepen de [actiegroep Resource Manager-sjablonen](./action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Een actiegroep maken met behulp van de Azure Portal

1. Zoek in het [Azure Portal](https://portal.azure.com) naar en selecteer **monitor**. In het deelvenster **monitor** worden al uw bewakingsinstellingen en -gegevens in één weergave samengevoegd.

1. Selecteer **waarschuwingen** en selecteer vervolgens **acties beheren**.

    ![Knop acties beheren](./media/action-groups/manage-action-groups.png)
    
1. Selecteer **actiegroep toevoegen** en vul de relevante velden in de wizard in.

    ![De opdracht ' actiegroep toevoegen '](./media/action-groups/add-action-group.PNG)

### <a name="configure-basic-action-group-settings"></a>Basisinstellingen voor de actiegroep configureren

Onder **Project Details**:

Selecteer het **abonnement** en de **resource groep** waarin de actiegroep wordt opgeslagen.

Geef onder **Exemplaardetails** het volgende op:

1. Voer een naam in voor de **actiegroep**.

1. Voer een **weergavenaam** in. De weergavenaam wordt gebruikt in plaats van de naam van een volledige actiegroep wanneer er meldingen worden verzonden met behulp van deze groep.

      ![Het dialoogvenster actiegroep toevoegen](./media/action-groups/action-group-1-basics.png)


### <a name="configure-notifications"></a>Meldingen configureren

1. Klik op de knop **volgende: meldingen >** om naar het tabblad **meldingen** te gaan of selecteer het tabblad **meldingen** boven aan het scherm.

1. Definieer een lijst met meldingen die moeten worden verzonden wanneer een waarschuwing wordt geactiveerd. Geef het volgende op voor elke melding:

    a. **Meldings type**: Selecteer het type melding dat u wilt verzenden. De beschikbare opties zijn:
      * Rol e-mail Azure Resource Manager: een e-mail verzenden naar gebruikers die zijn toegewezen aan bepaalde ARM-rollen op abonnements niveau.
      * E-mail/SMS/push/Voice: verzend dit type meldingen naar specifieke ontvangers.
    
    b. **Naam**: Voer een unieke naam in voor de melding.

    c. **Details**: Voer een e-mailadres, telefoon nummer, enzovoort in op basis van het geselecteerde meldingstype.
    
    d. **Algemeen waarschuwingsschema**: u kunt ervoor kiezen om het [algemene waarschuwingsschema](./alerts-common-schema.md)in te schakelen. Dit biedt het voor deel van het gebruik van een single Extensible en Unified alert Payload in alle waarschuwings Services in Azure Monitor.

    ![Het tabblad meldingen](./media/action-groups/action-group-2-notifications.png)
    
### <a name="configure-actions"></a>Acties configureren

1. Klik op de knop **volgende: acties >** om naar het tabblad **acties** te gaan of selecteer het tabblad **acties** boven aan het scherm.

1. Definieer een lijst met acties die moeten worden geactiveerd wanneer een waarschuwing wordt geactiveerd. Geef het volgende op voor elke actie:

    a. **Actie type**: Selecteer Automation-Runbook, Azure function, ITSM, Logic app, beveiligde webhook, of webhook.
    
    b. **Naam**: Voer een unieke naam in voor de actie.

    c. **Details**: Voer een webhook-URI, een Azure-app, ITSM-verbinding of een Automation-runbook in op basis van het actietype. Geef voor ITSM-actie ook **werk item** en andere velden op die nodig zijn voor het ITSM-hulpprogramma.
    
    d. **Algemeen waarschuwings schema**: u kunt ervoor kiezen om het [algemene waarschuwings schema](./alerts-common-schema.md)in te scha kelen. Dit biedt het voor deel van het gebruik van een single Extensible en Unified alert Payload in alle waarschuwings-Services in Azure Monitor.
    
    ![Het tabblad acties](./media/action-groups/action-group-3-actions.png)

### <a name="create-the-action-group"></a>De actiegroep maken

1. U kunt indien gewenst de instellingen voor **tags** bekijken. Hiermee kunt u sleutel-waardeparen koppelen aan de actiegroep voor uw categorisatie en is deze een functie die beschikbaar is voor elke Azure-resource.

    ![Het tabblad Labels](./media/action-groups/action-group-4-tags.png)
    
1. Klik op **Controleren + maken** om de instellingen te controleren. Er wordt een snelle validatie uitgevoerd van uw invoer om ervoor te zorgen dat alle vereiste velden zijn geselecteerd. Als er problemen zijn, worden deze hier vermeld. Zodra u de instellingen hebt gecontroleerd, klikt u op **Maken** om de actiegroep in te richten.
    
    ![Het tabblad controleren en maken](./media/action-groups/action-group-5-review.png)

> [!NOTE]
> Wanneer u een actie configureert om een persoon op de hoogte te stellen per e-mail of SMS, ontvangt de gebruiker een bevestiging dat deze is toegevoegd aan de actiegroep.

## <a name="manage-your-action-groups"></a>Uw actiegroepen beheren

Nadat u een actiegroep hebt gemaakt, kunt u **actiegroepen** weergeven door **acties beheren** te selecteren op de pagina **waarschuwingssignalen** in het deelvenster **monitor** . Selecteer de actiegroep die u wilt beheren:

* Acties toevoegen, bewerken of verwijderen.
* De actiegroep verwijderen.

## <a name="action-specific-information"></a>Actie-specifieke informatie

> [!NOTE]
> Zie [limieten voor abonnements Services voor controle](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits) op numerieke limieten voor elk van de onderstaande items.  

### <a name="automation-runbook"></a>Automation-runbook
Raadpleeg de [service limieten](../../azure-resource-manager/management/azure-subscription-service-limits.md) van het Azure-abonnement voor limieten voor nettoladingen van het Runbook.

Mogelijk hebt u een beperkt aantal Runbook-acties in een actiegroep. 

### <a name="azure-app-push-notifications"></a>Push meldingen van Azure-app
Mogelijk hebt u een beperkt aantal Azure-app-acties in een actiegroep.

### <a name="email"></a>E-mail
E-mails worden verzonden vanaf de volgende e-mail adressen. Controleren of uw e-mailfiltering op de juiste wijze is geconfigureerd
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

Mogelijk hebt u een beperkt aantal e-mailacties in een acti groep. Zie het artikel [informatie over de frequentie beperking](./alerts-rate-limiting.md) .

### <a name="email-azure-resource-manager-role"></a>E-mailadres voor Azure Resource Manager-rol
E-mail verzenden naar de leden van de rol van het abonnement. E-mail wordt alleen verzonden naar **Azure AD-gebruikers** leden van de rol. Er wordt geen e-mail verzonden naar Azure AD-groepen of service-principals.

Een e-mailmelding wordt alleen verzonden naar het *primaire e-mailadres*.

Als u geen meldingen ontvangt op uw *primaire e-mail*, kunt u de volgende stappen uitvoeren:

1. Ga in Azure Portal naar *Active Directory*.
2. Klik op alle gebruikers (in het linkerdeel venster). er wordt een lijst met gebruikers weergegeven (in het rechterdeel venster).
3. Selecteer de gebruiker voor wie u de *primaire e-mail* gegevens wilt controleren.

  :::image type="content" source="media/action-groups/active-directory-user-profile.png" alt-text="Voorbeeld voor het controleren van het gebruikers profiel."border="true":::

4. Klik in het gebruikersprofiel onder contactgegevens op het tabblad E-mail op de knop *bewerken* bovenaan en voeg uw *primaire e-mail* toe en klik bovenaan op de knop *Opslaan* .

  :::image type="content" source="media/action-groups/active-directory-add-primary-email.png" alt-text="Voorbeeld voor het toevoegen van een primair e-mail bericht."border="true":::

Mogelijk hebt u een beperkt aantal e-mailacties in een actiegroep. Zie het artikel [informatie over de frequentie beperking](./alerts-rate-limiting.md) .

### <a name="function"></a>Functie
Hiermee wordt een bestaand HTTP trigger-eindpunt aangeroepen in [Azure functions](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app).

Mogelijk hebt u een beperkt aantal functie-acties in een actiegroep.

### <a name="itsm"></a>ITSM
Voor de actie ITSM is een ITSM-verbinding vereist. Meer informatie over het maken van een [ITSM-verbinding](./itsmc-overview.md).

Mogelijk hebt u een beperkt aantal ITSM-acties in een actiegroep. 

### <a name="logic-app"></a>Logische apps
Mogelijk hebt u een beperkt aantal logische app-acties in een actiegroep.

### <a name="secure-webhook"></a>Beveiligde webhook

> [!NOTE]
> Het gebruik van de webhook-actie vereist dat het eindpunt van de doel-webhook ófwel geen details vereist van de waarschuwing om correct te laten functioneren, ófwel dat de waarschuwingscontext gegevens kan parseren die worden verstrekt als onderdeel van de POST-bewerking. Als het webhook-eindpunt de informatie over de context van de waarschuwing niet zelf kan verwerken, kunt u een oplossing gebruiken zoals een actie van een [logische app](./action-groups-logic-app.md) voor een aangepaste manipulatie van de waarschuwingscontextinformatie die overeenkomt met de verwachte gegevens indeling van de webhook.

Met de actiegroepen-webhook kunt u gebruik maken van Azure Active Directory om de verbinding tussen uw actiegroep en de beveiligde web-API (webhook-eindpunt) te beveiligen. De algemene werkstroom voor het gebruik van deze functionaliteit wordt hieronder beschreven. Zie [overzicht van Microsoft Identity platform (v 2.0)](../../active-directory/develop/v2-overview.md)voor een overzicht van Azure AD-toepassingen en-service-principals.

1. Maak een Azure AD-toepassing voor uw beveiligde web-API. Zie de [beveiligde web-API: app-registratie](../../active-directory/develop/scenario-protected-web-api-app-registration.md).
    - Configureer uw beveiligde API om te worden [aangeroepen door een daemon-app](../../active-directory/develop/scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).
    
2. Schakel actiegroepen in voor het gebruik van uw Azure AD-toepassing.

    > [!NOTE]
    > U moet lid zijn van de [rol Azure AD-toepassings beheerder](../../active-directory/roles/permissions-reference.md#available-roles) om dit script uit te voeren.
    
    - Wijzig de Connect-AzureAD aanroep van het Powershell-script om uw Azure AD-Tenant-ID te gebruiken.
    - Wijzig de variabele van het Powershell-script $myAzureADApplicationObjectId om de object-ID van uw Azure AD-toepassing te gebruiken.
    - Voer het gewijzigde script uit.
    
3. Configureer de actiegroep beveiligde webhooks.
    - Kopieer de waarde $myApp. ObjectId uit het script en voer deze in het veld toepassingsobject-ID in de actiedefinitie van webhook in.
    
    ![Actie beveiligde webhook](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>Powershell-script voor beveiligde webhook

```PowerShell
Connect-AzureAD -TenantId "<uw Azure AD tenant ID>"
    
# Dit is het ObjectId van de Azure AD Application
$myAzureADApplicationObjectId = "<het Object Id van de Azure AD Application>"
    
# Dit is het Azure AD AppId van de Actiegroepen
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# Dit is de naam van de nieuwe rol die we toevoegen aan de Azure AD Applicatie
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Creëer een applicatierol met de gegeven naam en beschrijving
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Lees mijn Azure AD-applicatie, bijbehorende rollen en servicePrincipal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "Rollen voorafgaand aan het toevoegen van de nieuwe App Role:"
Write-Host $myAppRoles

# Creëer de rol als die nog niet bestaat
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "Deze rol is al gedefinieerd.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Voeg de nieuwe rol toe aan de Azure AD Applicatie
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Maak de Service Principal aan als die nog niet bestaat
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "De Service principal is al gedefinieerd.`n"
}
else
{
    # Maak een service principal voor de actiegroepen Azure AD applicatie en voeg die toe aan de rol
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId

Write-Host "Mijn Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "Mijn Azure AD Application's rollen:"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>Sms
Zie de [informatie over het beperken](./alerts-rate-limiting.md) van de frequentie en het [gedrag van SMS-berichten](./alerts-sms-behavior.md) voor aanvullende belangrijke informatie. 

Mogelijk hebt u een beperkt aantal SMS-acties in een actiegroep.

> [!NOTE]
> Als de gebruikersinterface van de Azure Portal actiegroep niet toestaat dat u uw land-/regiocode selecteert, wordt SMS niet ondersteund voor uw land/regio.  Als uw land-/regiocode niet beschikbaar is, kunt u stemmen om uw land/regio toe te voegen op [deze stempagina](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice). In afwachting van een tijdelijke oplossing is het aan te bieden dat uw actiegroep een webhook aanroept van een SMS-provider van derden met ondersteuning in uw land/regio.  

Prijzen voor ondersteunde landen/regio's worden vermeld op de [pagina met Azure monitor prijzen](https://azure.microsoft.com/pricing/details/monitor/).

**Lijst met landen waarin SMS-meldingen worden ondersteund**

| Landnummer | Naam van land/regio |
|:---|:---|
| 61 | Australië |
| 43 | Oostenrijk |
| 32 | België |
| 55 | Brazilië |
| 1 |Canada |
| 56 | Chili |
| 86 | China |
| 420 | Tsjechische Republiek |
| 45 | Denemarken |
| 372 | Estland |
| 358 | Finland |
| 33 | Frankrijk |
| 49 | Duitsland |
| 852 | Hongkong |
| 91 | India |
| 353 | Ierland |
| 972 | Israël |
| 39 | Italië |
| 81 | Japan |
| 352 | Luxemburg |
| 60 | Maleisië |
| 52 | Mexico |
| 31 | Nederland |
| 64 | Nieuw-Zeeland |
| 47 | Noorwegen |
| 351 | Portugal |
| 1 | Puerto Rico |
| 40 | Roemenië |
| 65 | Singapore |
| 27 | Zuid-Afrika |
| 82 | Zuid-Korea |
| 34 | Spanje |
| 41 | Zwitserland |
| 886 | Taiwan |
| 44 | Verenigd Koninkrijk |
| 1 | Verenigde Staten |

### <a name="voice"></a>Spraak
Zie het artikel [informatie over de frequentiebeperking](./alerts-rate-limiting.md) voor meer belangrijk gedrag.

Mogelijk hebt u een beperkt aantal spraakacties in een actiegroep.

> [!NOTE]
> Als de gebruikersinterface van de Azure Portal actiegroep niet toestaat dat u uw land-/regiocode selecteert, worden telefoongesprekken niet ondersteund voor uw land/regio. Als uw land-/regiocode niet beschikbaar is, kunt u stemmen om uw land/regio toe te voegen op [deze feedbackpagina](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice). In de tussentijd is het een goed gebruik om uw actiegroep een webhook te laten aanroepen naar een aanbieder van een telefoonoproep van derden met ondersteuning voor uw land/regio.  
> De landcode die nu wordt ondersteund in Azure Portal actiegroep voor spraakmeldingen is +1 (Verenigde Staten). 

Prijzen voor ondersteunde landen/regio's worden vermeld op de [pagina met Azure Monitor-prijzen](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="webhook"></a>Webhook

> [!NOTE]
> Het gebruik van de webhook-actie vereist dat het eindpunt van de doel-webhook ófwel geen details vereist van de waarschuwing om correct te laten functioneren, ófwel dat de waarschuwingscontext gegevens kan parseren die worden verstrekt als onderdeel van de POST-bewerking. 

> De gebruiker moet de **eigenaar** zijn van de service-principal van de webhook om ervoor te zorgen dat de beveiliging niet wordt geschonden. Elke Azure-klant heeft toegang tot alle objectId's via de portal, zonder de eigenaar te controleren, maar iedereen kan de beveiligde webhook toevoegen aan hun eigen actiegroep voor Azure monitor-waarschuwingsmeldingen die de beveiliging schenden.

> Als het webhook-eindpunt de informatie over de context van de waarschuwing niet zelf kan verwerken, kunt u een oplossing gebruiken zoals een actie van een [logische app](./action-groups-logic-app.md) voor een aangepaste manipulatie van de waarschuwings context informatie die overeenkomt met de verwachte gegevensindeling van de webhook.

Webhooks worden verwerkt aan de hand van de volgende regels:
- Voor een webhook-aanroep is maximaal drie keer geprobeerd.
- Er wordt opnieuw geprobeerd de aanroep uit te voeren als er geen antwoord wordt ontvangen binnen de time-outperiode of als een van de volgende HTTP-status codes wordt geretourneerd: 408, 429, 503 of 504.
- Bij de eerste aanroep wordt 10 seconden gewacht op een antwoord.
- De tweede en derde keer wachten 30 seconden op een reactie.
- Nadat de drie pogingen tot het aanroepen van de webhook zijn mislukt, wordt het eindpunt gedurende 15 minuten door de actiegroep aangeroepen.

IP-adresbereiken van bron:

 - 13.66.60.119/32
 - 13.66.143.220/30
 - 13.66.202.14/32
 - 13.66.248.225/32
 - 13.66.249.211/32
 - 13.67.10.124/30
 - 13.69.109.132/30
 - 13.71.199.112/30
 - 13.77.53.216/30
 - 13.77.172.102/32
 - 13.77.183.209/32
 - 13.78.109.156/30
 - 13.84.49.247/32
 - 13.84.51.172/32
 - 13.84.52.58/32
 - 13.86.221.220/30
 - 13.106.38.142/32
 - 13.106.38.148/32
 - 13.106.54.3/32
 - 13.106.54.19/32
 - 13.106.57.181/32
 - 13.106.57.196/31
 - 20.38.149.132/30
 - 20.42.64.36/30
 - 20.43.121.124/30
 - 20.44.17.220/30
 - 20.45.123.236/30
 - 20.72.27.152/30
 - 20.150.172.228/30
 - 20.192.238.124/30
 - 20.193.202.4/30
 - 40.68.195.137/32
 - 40.68.201.58/32
 - 40.68.201.65/32
 - 40.68.201.206/32
 - 40.68.201.211/32
 - 40.68.204.18/32
 - 40.115.37.106/32
 - 40.121.219.215/32
 - 40.121.221.62/32
 - 40.121.222.201/32
 - 40.121.223.186/32
 - 51.104.9.100/30
 - 52.183.20.244/32
 - 52.183.31.0/32
 - 52.183.94.59/32
 - 52.184.145.166/32
 - 191.233.50.4/30
 - 191.233.207.64/26
 - 2603:1000:4:402::178/125
 - 2603:1000:104:402::178/125
 - 2603:1010:6:402::178/125
 - 2603:1010:101:402::178/125
 - 2603:1010:304:402::178/125
 - 2603:1010:404:402::178/125
 - 2603:1020:5:402::178/125
 - 2603:1020:206:402::178/125
 - 2603:1020:305:402::178/125
 - 2603:1020:405:402::178/125
 - 2603:1020:605:402::178/125
 - 2603:1020:705:402::178/125
 - 2603:1020:805:402::178/125
 - 2603:1020:905:402::178/125
 - 2603:1020: A04:402:: 178/125
 - 2603:1020: B04:402:: 178/125
 - 2603:1020: C04:402:: 178/125
 - 2603:1020: D04:402:: 178/125
 - 2603:1020: E04:402:: 178/125
 - 2603:1020: F04:402:: 178/125
 - 2603:1020:1004:800:: F8/125
 - 2603:1020:1104:400::178/125
 - 2603:1030: f:400:: 978/125
 - 2603:1030:10:402::178/125
 - 2603:1030:104:402::178/125
 - 2603:1030:107:400:: F0/125
 - 2603:1030:210:402::178/125
 - 2603:1030:40b: 400:: 978/125
 - 2603:1030:40c: 402:: 178/125
 - 2603:1030:504:802:: F8/125
 - 2603:1030:608:402::178/125
 - 2603:1030:807:402::178/125
 - 2603:1030: A07:402:: 8f8/125
 - 2603:1030: B04:402:: 178/125
 - 2603:1030: C06:400:: 978/125
 - 2603:1030: F05:402:: 178/125
 - 2603:1030:1005:402::178/125
 - 2603:1040:5:402::178/125
 - 2603:1040:207:402::178/125
 - 2603:1040:407:402::178/125
 - 2603:1040:606:402::178/125
 - 2603:1040:806:402::178/125
 - 2603:1040:904:402::178/125
 - 2603:1040: A06:402:: 178/125
 - 2603:1040: B04:402:: 178/125
 - 2603:1040: C06:402:: 178/125
 - 2603:1040: D04:800:: F8/125
 - 2603:1040: F05:402:: 178/125
 - 2603:1040:1104:400::178/125
 - 2603:1050:6:402::178/125
 - 2603:1050:403:400:: 1f8/125

Als u updates wilt ontvangen over wijzigingen in deze IP-adressen, raden we u aan een Service Health-waarschuwing te configureren, waarmee wordt gecontroleerd op informatieve meldingen over de service voor de actiegroep.

Mogelijk hebt u een beperkt aantal webhook-acties in een actiegroep.

Regelmatige updates van bron-IP-adressen kunnen veel tijd in beslag nemen in webhooks. Het gebruik van de **service tag** voor *ActionGroup* helpt bij het handmatig beperken van de complexiteit van regelmatige updates van IP-adressen. Het bereik van de bron-IP-adressen die hierboven worden gedeeld, wordt automatisch beheerd door Microsoft, dat is opgenomen in het **service label**.

#### <a name="service-tag"></a>Servicetag
Een servicetag vertegenwoordigt een groep IP-adres voorvoegsels van een bepaalde Azure-service. Microsoft beheert de adres voorvoegsels die zijn opgenomen in het service label en werkt de servicetag automatisch bij met gewijzigde adressen, zodat de complexiteit van regelmatige updates voor netwerkbeveiligingsregels voor een ActionGroup wordt geminimaliseerd.

1. Zoek in Azure Portal onder Azure Services naar *netwerkbeveiligingsgroep*.
2. Klik op **toevoegen** en maak een netwerkbeveiligingsgroep.

   1. Voeg de naam van de resource groep toe en voer vervolgens Details van het *exemplaar* in.
   1. Klik op **Controleren + maken** en klik vervolgens op *Maken*.
   
   :::image type="content" source="media/action-groups/action-group-create-security-group.png" alt-text="Voorbeeld voor het maken van een netwerkbeveiligingsgroep."border="true":::

3. Ga naar de resourcegroep en klik vervolgens op de *netwerkbeveiligingsgroep* die u hebt gemaakt.

    1. Selecteer *inkomende beveiligingsregels*.
    1. Klik op **Toevoegen**.
    
    :::image type="content" source="media/action-groups/action-group-add-service-tag.png" alt-text="Voorbeeld voor het toevoegen van een servicetag."border="true":::

4. Er wordt een nieuw venster geopend in het rechterdeel venster.
    1.  Bron **selecteren: servicetag**
    1.  Bron service label: **ActionGroup**
    1.  Klik op **Toevoegen**.
    
    :::image type="content" source="media/action-groups/action-group-service-tag.png" alt-text="Voorbeeld voor het toevoegen van een servicetag."border="true":::

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [gedrag van SMS-waarschuwingen](./alerts-sms-behavior.md).  
* Krijg [inzicht in het webhook-schema van waarschuwingen voor activiteiten logboeken](./activity-log-alerts-webhook.md).  
* Meer informatie over [ITSM-connector](./itsmc-overview.md).
* Meer informatie over de [frequentie limiet](./alerts-rate-limiting.md) voor waarschuwingen.
* Een [overzicht van waarschuwingen voor het activiteitenlogboek](./alerts-overview.md) en meer informatie over het ontvangen van waarschuwingen.  
* Meer informatie over het [configureren van waarschuwingen wanneer een service status melding wordt geplaatst](../../service-health/alerts-activity-log-service-notifications-portal.md).
