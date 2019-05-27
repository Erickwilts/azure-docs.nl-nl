---
title: Machtigingen - Azure Active Directory en beschrijvingen van de rol Administrator | Microsoft Docs
description: Een beheerdersrol kunt gebruikers toevoegen, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren.
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/15/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1107a6df92bf577cd60b9ad31627219da8e1a388
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956540"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Rol beheerdersmachtigingen in Azure Active Directory

Met Azure Active Directory (Azure AD), kunt u beperkte beheerders voor het beheren van identiteit taken in minder bevoegdheden rollen opgeven. Beheerders kunnen worden toegewezen voor dergelijke doeleinden als toevoegen of wijzigen, gebruikers, beheerdersrollen toewijzen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren en beheren van domeinnamen. De standaardmachtigingen van de gebruiker kunnen alleen in de gebruikersinstellingen worden gewijzigd in Azure AD.

De globale beheerder heeft toegang tot alle beheerfuncties. Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. Alleen globale beheerders en beheerders met bevoorrechte rol kunt beheerdersrollen delegeren. U wordt aangeraden dat u deze rol aan slechts een paar mensen in uw bedrijf toewijzen om het risico voor uw bedrijf.


## <a name="assign-or-remove-administrator-roles"></a>Toewijzen of verwijderen van beheerdersrollen

Zie voor meer informatie over beheerdersrollen toewijzen aan een gebruiker in Azure Active Directory, [weergeven en toewijzen beheerdersrollen in Azure Active Directory](directory-manage-roles-portal.md).

## <a name="available-roles"></a>Beschikbare rollen

De volgende beheerdersrollen zijn beschikbaar:

* **[Toepassingsbeheerder](#application-administrator)**: Gebruikers in deze rol kunnen maken en beheren van alle aspecten van zakelijke toepassingen, registratie en instellingen van de toepassingsproxy. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Gebruikers die zijn toegewezen aan deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de toepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Toepassingsontwikkelaar](#application-developer)**: Gebruikers in deze rol kunnen toepassingsregistraties maken wanneer de 'Gebruikers kunnen toepassingen registreren' is ingesteld op Nee. Deze rol geeft ook het recht om in te stemmen uit eigen naam als de 'Gebruikers toestemming kunnen geven voor apps die toegang tot bedrijfsgegevens in hun naam' is ingesteld op Nee. Gebruikers die zijn toegewezen aan deze rol worden toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

* **[Authentication-beheerder](#authentication-administrator)**: Gebruikers met deze rol kunnen instellen of referenties in niet-wachtwoord opnieuw instellen. Verificatie-beheerders kunnen vereisen dat gebruikers kunnen opnieuw worden geregistreerd op basis van bestaande niet-wachtwoordreferenties (bijvoorbeeld, MFA of FIDO) en intrekken **MFA herinneren op het apparaat**, waarin wordt gevraagd voor MFA op de volgende aanmelding van gebruikers die zijn niet-beheerders of alleen de volgende rollen toegewezen:
  * Verificatiebeheerder
  * Adreslijstlezers
  * Afzender van gastuitnodigingen
  * Berichtencentrum-lezer
  * Rapportenlezer

  De beheerdersrol voor de verificatie is momenteel in openbare preview. Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

  <b>Belangrijke</b>: Gebruikers met deze rol kunnen referenties wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van de referenties van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:

  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de verificatie-beheerders. Via dit pad beheerder verificatie mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

* **[B2C-Gebruikerbeheerder stroom](#b2c-user-flow-administrator)**: Gebruikers met deze rol kunnen maken en beheren van B2C Gebruikersstromen (ook wel 'ingebouwde' beleidsregels) in Azure Portal. Door het maken of bewerken van de gebruikersstromen, kunnen deze gebruikers wijzigen de CSS-html/javascript-inhoud van de gebruikerservaring, MFA-vereisten per gebruikersstroom wijzigen, claims in het token wijzigen en aanpassen van de sessie-instellingen voor alle beleidsregels in de tenant. Aan de andere kant, deze rol niet de mogelijkheid om gebruikersgegevens te bekijken, of wijzigingen aanbrengen in de kenmerken die zijn opgenomen in de tenantschema. Wijzigingen in Identity-Ervaringsframework (ook wel aangepast) beleid ook is buiten het bereik van deze rol.

* **[Stroom B2C-kenmerk Gebruikerbeheerder](#b2c-user-flow-attribute-administrator)**: Gebruikers met deze rol toevoegen of verwijderen van aangepaste kenmerken die beschikbaar zijn voor alle gebruikersstromen in de tenant. Als zodanig kunnen gebruikers met deze rol wijzigen of nieuwe elementen toevoegen aan het schema van de eindgebruiker en van invloed zijn op het gedrag van alle gebruikersstromen en wijzigingen indirect leiden naar welke gegevens kan worden gevraagd van eindgebruikers en uiteindelijk worden verzonden als claims voor toepassingen. Deze rol kan gebruikersstromen niet bewerken.

* **[B2C IEF sleutelset beheerder](#b2c-ief-keyset-administrator)**:    Gebruiker maken en beheren van voor beleidssleutels en geheimen voor tokenversleuteling, token handtekening en versleuteling/ontsleuteling claim. Nieuwe sleutels toevoegt aan bestaande sleutelcontainers, kan deze beperkte beheerder rollover geheimen behoefte zonder gevolgen voor bestaande toepassingen. Deze gebruiker ziet de volledige inhoud van deze geheimen en hun verloopdatum zelfs nadat het is gemaakt.
    
  <b>Belangrijk:</b> dit is een gevoelige rol. De beheerdersrol sleutelset moet zorgvuldig worden gecontroleerd en toegewezen zorgvuldig tijdens de testfase vóór productie- en productie.

* **[B2C IEF beleid beheerder](#b2c-ief-policy-administrator)**: Gebruikers in deze rol hebben de mogelijkheid om te maken, lezen, bijwerken, en verwijderen van alle aangepaste beleidsregels in Azure AD B2C en daarom hebt volledige controle over de Identiteitservaring-Framework in de relevante Azure AD B2C-tenant. Door het bewerken van beleid, kan deze gebruiker tot stand brengen direct Federatie met externe id-providers, wijzigen van het directory-schema, alle gebruikersgerichte inhoud (HTML, CSS, JavaScript) wijzigt, wijzigen van de vereisten voor het voltooien van verificatie, maken van nieuwe gebruikers, verzenden gebruikersgegevens met externe systemen, met inbegrip van volledige migraties en bewerken van alle gebruikersgegevens, met inbegrip van tijdgevoelige velden, zoals wachtwoorden en telefoonnummers. Deze rol kan echter wijzigen van de versleutelingssleutels of bewerken van de geheimen die worden gebruikt voor Federatie in de tenant.

  <b>Belangrijk:</b> De beheerder van B2 IEF beleid is een uiterst gevoelige rol die moet worden toegewezen in een zeer beperkte mate voor tenants in de productieomgeving. Activiteiten op basis van deze gebruikers moeten worden nauw gecontroleerd, met name voor tenants in de productieomgeving.

* **[Factureringsbeheerder](#billing-administrator)**: Doet aankopen, beheert abonnementen, beheert ondersteuningstickets en controleert de status van de service.

* **[Beheerder van de cloudtoepassing](#cloud-application-administrator)**: Gebruikers in deze rol hebben dezelfde machtigingen als de rol beheerder van de toepassing, met uitzondering van de mogelijkheid voor het beheren van de toepassingsproxy. Deze rol hebben de mogelijkheid om te maken en beheren van alle aspecten van bedrijfstoepassingen en registratie. Deze rol hebben ook de mogelijkheid om in te stemmen voor gedelegeerde machtigingen en Toepassingsmachtigingen met uitzondering van Microsoft Graph en Azure AD Graph. Gebruikers die zijn toegewezen aan deze rol zijn niet toegevoegd als eigenaars bij het maken van nieuwe toepassingsregistraties of zakelijke toepassingen.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van referenties voor toepassingen. Deze rol toegewezen gebruikers kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om u te imiteren identiteit van de toepassing. Als de identiteit van de toepassing heeft toegang gekregen tot Azure Active Directory, zoals de mogelijkheid om te maken of bijwerken van de gebruiker of andere objecten, kunnen een gebruiker die is toegewezen aan deze rol kan deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om te imiteren identiteit van de toepassing mogelijk misbruik van bevoegdheden via wat de gebruiker via hun roltoewijzingen in Azure AD doen kan. Het is belangrijk om te begrijpen dat een gebruiker toewijzen aan de rol beheerder van de Cloudtoepassing geeft ze de mogelijkheid om te imiteren identiteit van een toepassing.

* **[Cloud-Apparaatbeheerder](#cloud-device-administrator)**: Gebruikers in deze rol kunnen inschakelen, uitschakelen, en apparaten verwijderen in Azure AD en Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal lezen. De rol verleent machtigingen voor het beheren van andere eigenschappen op het apparaat.

* **[Beheerder voor naleving](#compliance-administrator)**: Gebruikers met deze rol hebben machtigingen voor het beheren van functies met betrekking tot naleving in de compliancecentrum Microsoft 365, Microsoft 365-beheercentrum, Azure, en Office 365-beveiliging en compliance. Gebruikers kunnen ook alle functies in de Exchange-beheercentrum, Teams en Skype voor bedrijven-beheercentrum beheren en maken van ondersteuningstickets voor Azure en Microsoft 365. Meer informatie vindt u op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  In | Kan doen
  ----- | ----------
  [Microsoft 365 compliancecentrum](https://protection.office.com) | Beveiligen en beheren van gegevens van uw organisatie via Microsoft 365-services<br>Naleving-waarschuwingen beheren
  [Voor naleving](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Bijhouden, toewijzen en controleer of de activiteiten van de naleving van regelgeving van uw organisatie
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Gegevensbeheer beheren<br>Wettelijke informatie en gegevens onderzoek uitvoeren<br>Aanvraag voor het onderwerp van gegevens beheren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle Intune-controlegegevens weergeven
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren<br>Kunt maken en wijzigen van de beleidsregels voor bestanden en governance-acties voor toestaan<br> De ingebouwde rapporten onder beheer van de gegevens kunt weergeven

<!--* **[Compliance Data Administrator](#compliance-data-administrator)**: Users with this role have permissions to protect and track data in the Microsoft 365 compliance center, Microsoft 365 admin center, and Azure. Users can also manage all features within the Exchange admin center, Compliance Manager, and Teams & Skype for Business admin center and create support tickets for Azure and Microsoft 365.

  In | Can do
  ----- | ----------
  [Microsoft 365 compliance center](https://protection.office.com) | Monitor compliance-related policies across Microsoft 365 services<br>Manage compliance alerts
  [Compliance Manager](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | Track, assign, and verify your organization's regulatory compliance activities
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Manage data governance<br>Perform legal and data investigation<br>Manage Data Subject Request
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | View all Intune audit data
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Has read-only permissions and can manage alerts<br>Can create and modify file policies and allow file governance actions<br> Can view all the built-in reports under Data Management
-->
* **[Beheerder van voorwaardelijke toegang](#conditional-access-administrator)**: Gebruikers met deze rol kunnen Azure Active Directory-instellingen voor voorwaardelijke toegang beheren.
  > [!NOTE]
  > Voor het implementeren van Exchange ActiveSync-beleid voor voorwaardelijke toegang in Azure, moet de gebruiker ook een globale beheerder zijn.
  
* **[Klanten-Lockbox toegang goedkeurder](#customer-lockbox-access-approver)**: Beheert [aanvragen van klanten-Lockbox](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) in uw organisatie. Ze kunnen ontvangen van e-mailmeldingen voor aanvragen van klanten-Lockbox goedkeuren en weigeren van de Microsoft 365-beheercentrum. Ze kunnen ook de functie voor klanten-Lockbox inschakelen of uitschakelen. Alleen globale beheerders kunnen de wachtwoorden van personen die zijn toegewezen aan deze rol opnieuw instellen.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Apparaatbeheerders](#device-administrators)**: Deze functie is beschikbaar voor toewijzing alleen als een lokale beheerder in [apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze hebben niet de mogelijkheid voor het beheren van apparaatobjecten in Azure Active Directory. 

* **[Adreslijstlezers](#directory-readers)**: Dit is een rol die moet worden toegewezen aan alleen oudere toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Niet toewijzen aan gebruikers.

* **[Directory-Accounts voor synchronisatie](#directory-synchronization-accounts)**: Gebruik geen. Deze rol is wordt automatisch toegewezen aan de Azure AD Connect-service en niet bedoeld of ondersteund voor ander gebruik.

* **[Schrijvers van mappen](#directory-writers)**: Dit is een verouderde rol die moet worden toegewezen aan toepassingen die geen ondersteuning voor de [toestemming geven Framework](../develop/quickstart-v1-integrate-apps-with-azure-ad.md). Deze moet niet worden toegewezen aan alle gebruikers.

* **[Dynamics 365-beheerder / CRM-beheerder](#crm-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Dynamics 365 Online, wanneer de service aanwezig is, evenals de mogelijkheid ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [de rol admin gebruiken voor het beheren van uw tenant](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Dynamics 365-servicebeheerder'. Het 'Dynamics 365-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Exchange-beheerder](#exchange-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft CRM Online, wanneer de service aanwezig is. Heeft ook de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Exchange Service Administrator". Het 'Exchange-beheerder' is in de [Azure-portal](https://portal.azure.com). Het 'Exchange Online-beheerder' is in de [Exchange-beheercentrum](https://go.microsoft.com/fwlink/p/?LinkID=529144). 

* **[Externe id-Provider beheerder](#external-identity-provider-administrator)**: Deze beheerder beheert federatie tussen tenants van Azure Active Directory en externe id-providers. Met deze rol kunnen gebruikers nieuwe id-providers toevoegen en configureren van alle beschikbare instellingen (bijvoorbeeld verificatiepad, service-id,-sleutelcontainers toegewezen). Deze gebruiker kan de tenant te vertrouwen verificaties van externe id-providers kunt inschakelen. De resulterende impact op de ervaringen van eindgebruikers, is afhankelijk van het type tenant:
  * Azure Active Directory-tenants voor werknemers en partners: Het toevoegen van een federatieve (bijvoorbeeld met Gmail) is direct van invloed op alle Gast uitnodigingen nog niet ingewisseld. Zie [Google toe te voegen als een id-provider voor B2B-gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).
  * Azure Active Directory B2C-tenants: Het toevoegen van een federatieve (bijvoorbeeld met Facebook of met een andere Azure Active Directory) is niet onmiddellijk invloed op het stromen van de eindgebruiker totdat de id-provider is toegevoegd als een optie in een gebruikersstroom (ook wel het ingebouwde beleid). Zie [configureren van een Microsoft-account als id-provider](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) voor een voorbeeld. Als u wilt wijzigen gebruikersstromen, zijn de beperkte rol van 'B2C gebruiker Flow beheerder' is vereist.

* **[Globale beheerder / Company Administrator](#company-administrator)**: Gebruikers met deze rol hebben toegang tot alle beheerfuncties in Azure Active Directory, evenals de services die gebruikmaken van Azure Active Directory-identiteiten, zoals Microsoft 365 security center, Microsoft 365 compliancecentrum, Exchange Online, SharePoint Online, en Skype voor bedrijven Online. De persoon die zich aanmeldt voor de Azure Active Directory-tenant wordt globale beheerder. Alleen globale beheerders kunnen andere beheerdersrollen toewijzen. Er is meer dan één globale beheerder in uw bedrijf. Globale beheerders kunnen het wachtwoord voor elke gebruiker en alle andere beheerders opnieuw instellen.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als "Company Administrator". Het 'Globale beheerder' is in de [Azure-portal](https://portal.azure.com).
  >
  >

* **[Gastuitnodiging](#guest-inviter)**: Gebruikers in deze rol kunnen uitnodigingen voor Azure Active Directory B2B Gast beheren wanneer de **leden kunnen uitnodigen** gebruikersinstelling is ingesteld op Nee. Meer informatie over B2B-samenwerking bij [over Azure AD B2B-samenwerking](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Deze omvatten geen andere machtigingen.

* **[Information Protection-beheerder](#information-protection-administrator)**: Gebruikers met deze rol hebben alle machtigingen in de Azure Information Protection-service. Deze rol kan labels voor de Azure Information Protection-beleid configureren, beveiligingssjablonen beheren en beveiliging activeren. Deze rol verleent alle machtigingen in Identity Protection Center, Privileged Identity Management, Monitor Office 365-servicestatus of Office 365 Centrum voor beveiliging en naleving.

* **[Intune-beheerder](#intune-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Intune Online, wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid voor het beheren van gebruikers en apparaten om te koppelen van beleid, evenals groepen maken en beheren. Meer informatie op [rollen gebaseerd toegangsbeheer (RBAC) met Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Intune-servicebeheerder '. Het 'Intune-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Licentiebeheerder](#license-administrator)**: Gebruikers in deze rol kunnen toevoegen, verwijderen en update licentie toewijzingen aan gebruikers, groepen (met Groepslicenties) en de gebruikslocatie van gebruikers beheren. De rol heeft niet de mogelijkheid om te kopen of beheren van abonnementen, maken of beheren van groepen, of maken of beheren van gebruikers buiten de gebruikslocatie verlenen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Berichtencentrum-lezer](#message-center-reader)**: Gebruikers in deze rol kunnen controleren, meldingen en de gezondheid van advies-updates in [Office 365-berichtencentrum](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie op de geconfigureerde services zoals Exchange, Intune en Microsoft Teams. Berichtencentrum-lezer wekelijkse e-mailbericht verwerkingen van berichten, updates, ontvangen en message center berichten in Office 365 kunnen delen. In Azure AD, wordt gebruikers die zijn toegewezen aan deze rol alleen alleen-lezen toegang hebben op Azure AD-services, zoals gebruikers en groepen. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Laag1-ondersteuning voor partner](#partner-tier1-support)**: Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Laag2-ondersteuning voor partner](#partner-tier2-support)**: Gebruik geen. Deze rol is afgeschaft en wordt verwijderd uit Azure AD in de toekomst. Deze rol is bedoeld voor gebruik door een klein aantal wederverkoop partners van Microsoft en is niet bedoeld voor algemeen gebruik.

* **[Helpdeskbeheerder (wachtwoord)](#helpdesk-administrator)**: Gebruikers met deze rol kunnen wachtwoorden wijzigen, vernieuwingstokens ongeldig te maken, serviceaanvragen beheren en servicestatus controleren. Ongeldig vernieuwingstoken zorgt ervoor dat de gebruiker zich opnieuw aanmelden. Helpdesk-beheerders kunnen wachtwoorden opnieuw instellen en vernieuwen van tokens van andere gebruikers die niet-beheerders en alleen de volgende rollen toegewezen ongeldig te maken:
  * Adreslijstlezers
  * Afzender van gastuitnodigingen
  * Helpdeskbeheerder
  * Berichtencentrum-lezer
  * Rapportenlezer
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan de Helpdesk-medewerkers. Via dit pad die een Helpdesk-beheerder kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.


  > [!NOTE]
  > Beheerdersmachtigingen overdragen via subsets van gebruikers en beleidsregels toepassen op een subset van gebruikers er mogelijk is met [Beheereenheden (preview)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).


  > [!NOTE]
  > Deze rol heette vroeger Microsoft Azure "wachtwoordbeheerder" in [Azure-portal](https://portal.azure.com/). We zijn de naam ervan wijzigen in 'Helpdesk-beheerder' zodat deze overeenkomen met de naam ervan in Azure AD PowerShell, Azure AD Graph API en Microsoft Graph API. Voor een korte periode wordt we de naam gewijzigd in '(wachtwoord) Helpdeskbeheerder' in Azure-portal voordat de wijziging door te 'Helpdesk-beheerder'.


* **[Power BI Administrator](#power-bi-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Online Power BI, wanneer de service aanwezig is, evenals de mogelijkheid om ondersteuningstickets te beheren en de servicestatus te controleren. Meer informatie op [inzicht in de Power BI-beheerdersrol](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Power BI-servicebeheerder '. ' Power BI-beheerder ' is in de [Azure-portal](https://portal.azure.com).

* **[Authentication-beheerder in beschermde modus](#privileged-authentication-administrator)**: Gebruikers met deze rol kunnen instellen of referenties voor alle gebruikers, met inbegrip van globale beheerders niet-wachtwoord opnieuw instellen. Bevoegde verificatie-beheerders kunnen afdwingen dat gebruikers kunnen opnieuw worden geregistreerd op basis van bestaande niet-wachtwoord referentie (bijvoorbeeld MFA, FIDO) en intrekken 'MFA herinneren op het apparaat', dat u wordt gevraagd voor MFA op de volgende aanmelding van alle gebruikers. Beheerders met bevoorrechte verificatie kunt doen:
  * Afdwingen dat gebruikers opnieuw registreren op basis van bestaande niet-wachtwoord referentie (bijvoorbeeld MFA, FIDO)
  * Intrekken 'MFA herinneren op het apparaat', dat u wordt gevraagd voor MFA op de volgende aanmelding

* **[Rol van beheerder in beschermde modus](#privileged-role-administrator)**: Gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory, evenals in Azure AD Privileged Identity Management beheren. Bovendien kan deze rol beheer van alle aspecten van Privileged Identity Management.

  <b>Belangrijke</b>: Deze rol hebben de mogelijkheid voor het beheren van toewijzingen voor alle Azure AD-rollen, met inbegrip van de rol globale beheerder. Deze rol bevat geen andere bevoegde mogelijkheden in Azure AD, zoals het maken of bijwerken van gebruikers. Echter kunnen gebruikers zijn toegewezen aan deze rol verlenen zichzelf of andere aanvullende bevoegdheden door aanvullende rollen toewijzen.

* **[Lezer-rapporten](#reports-reader)**: Gebruikers met deze rol kunnen reporting-gebruiksgegevens weergeven en het dashboard rapporten in Microsoft 365-beheercentrum en de acceptatie-context pack in Power BI. Bovendien de rol biedt toegang tot aanmelden-rapporten en -activiteit in Azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. De gebruiker die is toegewezen aan de rol Rapportenlezer toegang alleen relevante gebruik en acceptatie metrische gegevens. Ze geen geen admin-machtigingen voor het configureren van instellingen of toegang tot die het beheercentrums productspecifieke zoals Exchange. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Beveiligingsbeheerder](#security-administrator)**: Gebruikers met deze rol hebben machtigingen voor het beheren van beveiligingsfuncties in de Microsoft 365 security center, Azure Active Directory Identity Protection, Azure Information Protection, en Office 365-beveiliging en compliance. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | Beveiligingsbeleid controleren in Microsoft 365-services<br>Beveiligingsrisico's en waarschuwingen beheren<br>Rapporten weergeven
  Identity Protection Center | Alle machtigingen van de rol van Beveiligingslezer<br>Bovendien de mogelijkheid om uit te voeren van alle Identity Protection Center-bewerkingen, met uitzondering van wachtwoorden opnieuw instellen
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Alle machtigingen van de rol van Beveiligingslezer<br>**Kan geen** instellingen of Azure AD-roltoewijzingen beheren
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid beheren<br>Weergeven, onderzoeken en direct reageren op bedreigingen<br>Rapporten weergeven
  Azure Advanced Threat Protection | Bewaken van en reageren op verdachte activiteit
  Windows Defender ATP en EDR | Rollen toewijzen<br>Computergroepen beheren<br>Detectie van bedreigingen eindpunt en geautomatiseerd herstel configureren<br>Weergeven, onderzoeken en reageren op waarschuwingen
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Weergaven gebruiker, apparaat, inschrijving, configuratie en informatie over toepassingen<br>Kan geen wijzigingen aanbrengen aan Intune
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Beheerders toevoegen, beleidsregels en instellingen, logboeken te uploaden en beheeracties uitvoeren toevoegen
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Kan weergeven beveiligingsbeleid, security-status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen
  [Office 365-servicestatus](https://docs.microsoft.com/office365/enterprise/view-service-health) | Bekijk de status van Office 365-services

<!--* **[Security operator](#security-operator)**: Users with this role can manage alerts and have global read-only access on security-related feature, including all information in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, as well as the ability to read Azure Active Directory sign-in reports and audit logs, and in Office 365 Security & Compliance Center.

  In | Can do
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | All permissions of the Security Reader role<br>View, investigate, and respond to security threats alerts
  Identity Protection Center | All permissions of the Security Reader role<br>Additionally, the ability to perform all Identity Protection Center operations except for resetting passwords
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | All permissions of the Security Reader role
  [Office 365 Security & Compliance Center](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  Windows Defender ATP and EDR | All permissions of the Security Reader role<br>View, investigate, and respond to security alerts
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | All permissions of the Security Reader role
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | All permissions of the Security Reader role
  [Office 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health) | View the health of Office 365 services
-->
* **[Beveiligingslezer](#security-reader)**: Gebruikers met deze rol hebben globale alleen-lezen toegang op functies met betrekking tot beveiliging, met inbegrip van alle gegevens in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management, evenals de mogelijkheid om te lezen Azure Active Directory-aanmeldingsrapporten en controlelogboeken, en in Office 365-beveiligings- en Compliancecentrum. Meer informatie over Office 365-machtigingen is beschikbaar op [machtigingen in het Office 365-beveiligings- en Nalevingscentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  In | Kan doen
  --- | ---
  [Microsoft 365 security center](https://protection.office.com) | Beveiligingsbeleid voor Microsoft 365-services weergeven<br>Weergave beveiligingsrisico's en waarschuwingen<br>Rapporten weergeven
  Identity Protection Center | Alle beveiligingsrapporten en informatie over de instellingen voor beveiligingsfuncties lezen<br><ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevensverlies<li>Anti-malware<li>Geavanceerde beveiliging tegen bedreigingen<li>Anti-phishing<li>Mailflow regels
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Is alleen-lezen toegang tot alle gegevens in Azure AD PIM opgehaald: Beleid en rapporten voor Azure AD-roltoewijzingen wordt beveiliging beoordeelt en toegang tot gegevens en rapporten voor scenario's behalve Azure AD-roltoewijzing in de toekomst te lezen.<br>**Kan geen** aanmelden voor Azure AD PIM of wijzigingen aanbrengen. In de PIM-portal of via PowerShell, kan iemand zich in deze rol aanvullende rollen (bijvoorbeeld: globale beheerder of beheerder met bevoorrechte rol), activeren als de gebruiker in aanmerking komt voor deze.
  [Office 365-beveiliging en compliance](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid bekijken<br>Weergeven en bedreigingen te onderzoeken<br>Rapporten weergeven
  Windows Defender ATP en EDR | Weergeven en onderzoeken van waarschuwingen
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Weergaven gebruiker, apparaat, inschrijving, configuratie en informatie over toepassingen. Kan geen wijzigingen aanbrengen aan Intune.
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alleen-lezen machtigingen heeft en kunnen waarschuwingen beheren
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven
  [Office 365-servicestatus](https://docs.microsoft.com/office365/enterprise/view-service-health) | Bekijk de status van Office 365-services

* **[Beheerder serviceondersteuning](#service-support-administrator)**: Gebruikers met deze rol kunnen ondersteuningsaanvragen openen met Microsoft Azure en Office 365-services, weergaven en het servicedashboard en berichtencentrum weergeven in de [Azure-portal](https://portal.azure.com) en [Microsoft 365-beheercentrum](https://admin.microsoft.com). Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als ' beheerder serviceondersteuning. " Het is 'Servicebeheerder' de [Azure-portal](https://portal.azure.com), wordt de [Microsoft 365-beheercentrum](https://admin.microsoft.com), en de Intune-portal.

* **[SharePoint-beheerder](#sharepoint-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft SharePoint Online, wanneer de service aanwezig is, evenals de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren. Meer informatie op [over Office 365-beheerdersrollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, deze rol aangeduid als "SharePoint-servicebeheerder." Het 'SharePoint-beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Skype voor bedrijven / Lync beheerder](#lync-service-administrator)**: Gebruikers met deze rol hebben algemene machtigingen in Microsoft Skype voor bedrijven, wanneer de service aanwezig is, evenals beheren van de kenmerken van de Skype-specifieke gebruiker in Azure Active Directory. Deze rol hebben bovendien de mogelijkheid ondersteuningstickets beheren en servicestatus controleren en de toegang tot de Teams en Skype voor bedrijven-beheercentrum. Het account moet ook een licentie hebben voor Teams of Teams PowerShell-cmdlets kan niet worden uitgevoerd. Meer informatie op [over de Skype voor bedrijven-beheerdersrol](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) en Teams informatie over licenties op [Skype voor bedrijven en Microsoft Teams-Add-on-licentieverlening](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als 'Lync-servicebeheerder'. Het 'Skype voor bedrijven-beheerder' is in de [Azure-portal](https://portal.azure.com/).

* **[Beheerder teams](#teams-service-administrator)**: Gebruikers in deze rol kunnen alle aspecten van de Microsoft Teams-werkbelasting via de Microsoft Teams en Skype voor bedrijven-beheercentrum en de bijbehorende PowerShell-modules beheren. Dit omvat onder andere gebieden, alle beheerprogramma's met betrekking tot de telefoon, chatberichten, vergaderingen en teams zelf. Deze rol hebben bovendien de mogelijkheid om te maken en beheren van alle Office 365-groepen, ondersteuningstickets beheren en servicestatus controleren.
  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API en Azure AD PowerShell, wordt deze rol aangeduid als ' Teams Service Administrator ". Het 'Teams beheerder' is in de [Azure-portal](https://portal.azure.com).

* **[Communicatie-beheerder teams](#teams-communications-administrator)**: Gebruikers in deze rol kunnen aspecten van de Microsoft Teams-workload met betrekking tot de spraak- en TAPI beheren. Dit omvat de beheerhulpprogramma's voor de toewijzing van telefoon, spraak- en voldoen aan beleidsregels en volledige toegang tot de aanroep analytics toolset.

* **[Communicatie-ondersteuningstechnicus teams](#teams-communications-support-engineer)**: Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen bekijken aanroep van de volledige gegevens voor alle deelnemers die betrokken zijn. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **[Communicatie ondersteuning voor gespecialiseerde teams](#teams-communications-support-specialist)**: Gebruikers in deze rol kunnen problemen met communicatie binnen Microsoft Teams en Skype voor bedrijven met behulp van de aanroep van de gebruiker het oplossen van hulpprogramma's in de Microsoft Teams en Skype voor bedrijven-beheercentrum. Gebruikers in deze rol kunnen alleen gebruikersgegevens weergeven in de aanroep voor de specifieke gebruiker dat ze hebt opgezocht. Deze rol heeft geen toegang tot weergeven, maken of ondersteuningstickets beheren.

* **Gebruikersbeheerder**: Gebruikers met deze rol kunnen gebruikers, maken en beheren van alle aspecten van gebruikers met enkele beperkingen (Zie hieronder) en wachtwoordverloopbeleid kunnen bijwerken. Gebruikers met deze rol kunnen bovendien maken en beheren van alle groepen. Deze rol omvat ook de mogelijkheid om te maken en beheren van gebruikersweergaven, ondersteuningstickets beheren en servicestatus controleren.

  | | |
  | --- | --- |
  |Algemene machtigingen|<p>Gebruikers en groepen maken</p><p>Gebruikersweergaven maken en beheren</p><p>Office-ondersteuningstickets beheren<p>Wachtwoordverloopbeleid bijwerken|
  |<p>Op alle gebruikers, met inbegrip van alle beheerders</p>|<p>Licenties beheren</p><p>Eigenschappen van alle gebruikers, behalve de User Principal Name beheren</p>
  |Alleen op gebruikers die niet-beheerders of beperkte beheerdersrollen in het volgende:<ul><li>Adreslijstlezers<li>Afzender van gastuitnodigingen<li>Helpdeskbeheerder<li>Berichtencentrum-lezer<li>Rapportenlezer<li>Gebruikerbeheerder|<p>Verwijderen en herstellen</p><p>Uitschakelen en inschakelen</p><p>Ongeldig vernieuwingstokens</p><p>Eigenschappen van alle gebruikers met inbegrip van de User Principal Name beheren</p><p>Wachtwoord opnieuw instellen</p><p>Apparaatsleutels (FIDO) bijwerken</p>
  
  <b>Belangrijke</b>: Gebruikers met deze rol kunnen wachtwoorden wijzigen voor gebruikers die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie binnen en buiten Azure Active Directory. Wijzigen van het wachtwoord van een gebruiker kan betekenen dat de mogelijkheid om te wordt ervan uitgegaan dat de identiteit en de machtigingen van die gebruiker. Bijvoorbeeld:
  * Registratie van toepassingen en zakelijke toepassing eigenaren, die de referenties van waarvan ze eigenaar apps kunnen beheren. Deze apps kunnen machtigingen in Azure AD privileged en ergens anders niet worden toegekend aan beheerders. Via dit pad voor de beheerder van een gebruiker kan mogelijk wordt ervan uitgegaan dat de identiteit van de eigenaar van een toepassing en vervolgens de identiteit aannemen van een bevoegde toepassing door bij te werken van de referenties voor de toepassing.
  * Azure-abonnementseigenaren, die mogelijk toegang heeft tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure.
  * Beveiligingsgroepen en Office 365-groep eigenaren, die het lidmaatschap kunnen beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke gegevens of essentiële configuratie in Azure AD en elders.
  * Beheerders in de andere services buiten Azure AD, zoals Exchange Online, Office-beveiliging en Compliancecentrum en HR-systemen.
  * Niet-beheerders, zoals leidinggevenden, juridische afdeling en werknemers van human resources die mogelijk toegang heeft tot gevoelige of persoonlijke informatie.

## <a name="role-permissions"></a>Machtigingen van de rol
De volgende tabellen beschrijven de specifieke machtigingen in Azure Active Directory die aan elke rol. Sommige functies mogelijk extra machtigingen in Microsoft-services buiten Azure Active Directory.

### <a name="application-administrator"></a>Toepassingsbeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Werk de eigenschap applications.audience bij in Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Werk de eigenschap applications.authentication bij in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Werk de eigenschap applications.credentials bij in Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Werk de eigenschap applications.owners bij in Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Werk de eigenschap applications.permissions bij in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/read | Lees de appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="application-developer"></a>Toepassingsontwikkelaar
Kan toepassingsregistraties onafhankelijk van de 'gebruikers kunnen toepassingen registreren' maken instelling.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Maak toepassingen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Maak appRoleAssignments in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Maak oAuth2PermissionGrants in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Maak servicePrincipals in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |

### <a name="authentication-administrator"></a>Verificatiebeheerder
Toegestaan wilt weergeven, instellen en opnieuw instellen van verificatie methode-informatie voor elke gebruiker die geen beheerder.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | De eigenschappen van sterke verificatie bijwerken, bijvoorbeeld MFA-referentiegegevens. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="b2c-user-flow-administrator"></a>B2C User Flow Administrator
Maken en beheren van alle aspecten van de gebruikersstromen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Lees en gebruikersstromen configureren in Azure Active Directory B2C. |

### <a name="b2c-user-flow-attribute-administrator"></a>B2C User Flow Attribute Administrator
Maken en beheren van het kenmerkschema beschikbaar voor alle gebruikersstromen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Lees en gebruikerskenmerken configureren in Azure Active Directory B2C. |

### <a name="b2c-ief-keyset-administrator"></a>B2C IEF Keyset Administrator
Geheimen voor Federatie en versleuteling in de Identity-Ervaringsframework beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Lees en configureren van sleutel sets in Azure Active Directory B2C. |

### <a name="b2c-ief-policy-administrator"></a>B2C IEF Policy Administrator
Maken en beheren van framework vertrouwensbeleid in de Identiteitservaring-Framework.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Lees en aangepaste beleidsregels configureren in Azure Active Directory B2C. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kan algemene taken met betrekking tot facturering uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | Werk de basiseigenschappen voor een organisatie bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics-beheerder
Openen en beheren van bureaubladbeheerhulpprogramma's en services, met inbegrip van Intune.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van Desktop Analytics beheren. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-application-administrator"></a>Cloudtoepassingsbeheerder
Kan alle aspecten van app-registraties en bedrijfsapps maken en beheren, behalve App Proxy.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Werk de eigenschap applications.audience bij in Azure Active Directory. |
| microsoft.aad.directory/applications/authentication/update | Werk de eigenschap applications.authentication bij in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/update | Werk de basiseigenschappen voor toepassingen bij in Azure Active Directory. |
| microsoft.aad.directory/applications/create | Maak toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/credentials/update | Werk de eigenschap applications.credentials bij in Azure Active Directory. |
| microsoft.aad.directory/applications/delete | Verwijder toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/update | Werk de eigenschap applications.owners bij in Azure Active Directory. |
| microsoft.aad.directory/applications/permissions/update | Werk de eigenschap applications.permissions bij in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Werk de eigenschap policies.applicationConfiguration bij in Azure Active Directory. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Lees de eigenschap policies.applicationConfiguration in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/delete | Verwijder servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="cloud-device-administrator"></a>Cloud-apparaatbeheerder
Volledige toegang om apparaten te beheren in Azure AD.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Verwijder apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/disable | Schakel apparaten uit in Azure Active Directory. |
| microsoft.aad.directory/devices/enable | Apparaten in Azure Active Directory inschakelen. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="company-administrator"></a>Bedrijfsbeheerder
Kan alle aspecten beheren van Azure AD en Microsoft-services die Azure AD-identiteiten gebruiken.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Alle resources maken en verwijderen en de standaardeigenschappen in microsoft.aad.cloudAppSecurity lezen en bijwerken. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Maak en verwijder administrativeUnits en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/applications/allProperties/allTasks | Maak en verwijder toepassingen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Maak en verwijder appRoleAssignments en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Maak en verwijder contactpersonen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Maak en verwijder contracten en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/devices/allProperties/allTasks | Maak en verwijder apparaten en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Maak en verwijder directoryRoles en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Maak en verwijder directoryRoleTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/domains/allProperties/allTasks | Maak en verwijder domeinen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groups/allProperties/allTasks | Maak en verwijder groepen en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Maak en verwijder groupSettings en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Maak en verwijder groupSettingTemplates en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Maak en verwijder loginTenantBranding en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Maak en verwijder oAuth2PermissionGrants en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/organization/allProperties/allTasks | Maak en verwijder een organisatie en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/policies/allProperties/allTasks | Maak en verwijder beleid en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Maak en verwijder roleAssignments en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Maak en verwijder roleDefinitions en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Maak en verwijder scopedRoleMemberships en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/serviceAction/activateService | Mag de serviceactie Activateservice uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Mag de serviceactie Disabledirectoryfeature uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Mag de serviceactie Enabledirectoryfeature uitvoeren in Azure Active Directory |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Mag de serviceactie Getavailableextentionproperties uitvoeren in Azure Active Directory |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Maak en verwijder servicePrincipals en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Maak en verwijder subscribedSkus en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/users/allProperties/allTasks | Maak en verwijder gebruikers en lees alle eigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |
| microsoft.aad.identityProtection/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.identityProtection en werk deze bij. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Alle resources lezen in microsoft.azure.advancedThreatProtection. |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.commerce.billing/allEntities/allTasks | Beheer alle aspecten van Office 365-facturering. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| microsoft.office365.complianceManager/allEntities/allTasks | Beheer alle aspecten van Office 365 Compliancebeheer |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Alle aspecten van Desktop Analytics beheren. |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.lockbox/allEntities/allTasks | Beheer alle aspecten van Office 365 Klanten-lockbox |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read | securityMessages lezen in microsoft.office365.messageCenter. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Beheer alle aspecten van Office 365 Protection Center. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Alle resources maken en verwijderen, de standaardeigenschappen in microsoft.office365.securityComplianceCenter lezen en bijwerken. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Alle resources lezen in microsoft.windows.defenderAdvancedThreatProtection. |

### <a name="compliance-administrator"></a>Beheerder voor naleving
Kan nalevingsconfiguratie en -rapporten lezen en beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.complianceManager/allEntities/allTasks | Beheer alle aspecten van Office 365 Compliancebeheer |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="conditional-access-administrator"></a>Voorwaardelijke toegang beheerder
Kan de mogelijkheden van voorwaardelijke toegang beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Lees de eigenschap policies.conditionalAccess in Azure Active Directory. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | Werk de eigenschap policies.conditionalAccess bij in Azure Active Directory. |

### <a name="crm-service-administrator"></a>CRM-servicebeheerder
Kan alle aspecten van het Dynamics 365-product beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Beheer alle aspecten van Dynamics 365. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="customer-lockbox-access-approver"></a>Toegangsfiatteur voor Klanten-lockbox
Kan Microsoft-ondersteuningsaanvragen voor toegang tot bedrijfsgegevens van klanten goedkeuren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.lockbox/allEntities/allTasks | Beheer alle aspecten van Office 365 Klanten-lockbox |

### <a name="device-administrators"></a>Apparaatbeheerders
Gebruikers die zijn toegewezen aan deze rol worden toegevoegd aan de groep lokale beheerders op Azure AD join-apparaten.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen voor groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen voor groupSettingTemplates in Azure Active Directory. |

### <a name="directory-readers"></a>Adreslijstlezers
Basic directory-informatie kan worden gelezen. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Lees de basiseigenschappen van administrativeUnits in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/members/read | Lees de eigenschap administrativeUnits.members in Azure Active Directory. |
| microsoft.aad.directory/applications/basic/read | Lees de basiseigenschappen voor toepassingen in Azure Active Directory. |
| microsoft.aad.directory/applications/owners/read | Lees de eigenschap applications.owners in Azure Active Directory. |
| microsoft.aad.directory/applications/policies/read | Lees de eigenschap applications.policies in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/read | Lees de basiseigenschappen voor contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/memberOf/read | Lees de eigenschap contacts.memberOf in Azure Active Directory. |
| microsoft.aad.directory/contracts/basic/read | Lees de basiseigenschappen voor contracten in Azure Active Directory. |
| microsoft.aad.directory/devices/basic/read | Lees de basiseigenschappen voor apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/memberOf/read | Lees de eigenschap devices.memberOf in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/read | Lees de eigenschap devices.registeredOwners in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/read | Lees de eigenschap devices.registeredUsers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/basic/read | Lees de basiseigenschappen voor directoryRoles in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Lees de eigenschap directoryRoles.eligibleMembers in Azure Active Directory. |
| microsoft.aad.directory/directoryRoles/members/read | Lees de eigenschap directoryRoles.members in Azure Active Directory. |
| microsoft.aad.directory/domains/basic/read | Lees de basiseigenschappen voor domeinen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Lees de eigenschap groups.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/read | Lees de basiseigenschappen voor groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/memberOf/read | Lees de eigenschap groups.memberOf in Azure Active Directory. |
| microsoft.aad.directory/groups/members/read | Lees de eigenschap groups.members in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/read | Lees de eigenschap groups.owners in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/read | Lees de eigenschap groups.settings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen voor groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen voor groupSettingTemplates in Azure Active Directory. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Lees de basiseigenschappen voor oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/organization/basic/read | Lees de basiseigenschappen voor een organisatie in Azure Active Directory. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Lees de eigenschap organization.trustedCAsForPasswordlessAuth in Azure Active Directory. |
| microsoft.aad.directory/roleAssignments/basic/read | De basiseigenschappen voor roltoewijzingen lezen in Azure Active Directory. |
| microsoft.aad.directory/roleDefinitions/basic/read | De basiseigenschappen voor roldefinities lezen in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lees de eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lees de eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen voor servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lees de eigenschap servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lees de eigenschap servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lees de eigenschap servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lees de eigenschap servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lees de eigenschap servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/subscribedSkus/basic/read | Lees de basiseigenschappen voor subscribedSkus in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/read | Lees de eigenschap users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lees de eigenschap users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lees de eigenschap users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lees de eigenschap users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lees de eigenschap users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lees de eigenschap users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lees de eigenschap users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lees de eigenschap users.registeredDevices in Azure Active Directory. |

### <a name="directory-synchronization-accounts"></a>Synchronisatie van Active Directory-Accounts
Alleen gebruikt door Azure AD Connect-service.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Werk de eigenschap organization.dirSync bij in Azure Active Directory. |
| microsoft.aad.directory/policies/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/read | Lees de eigen basiseigenschappen voor beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/read | Lees de eigenschap policies.owners in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Werk de eigenschap policies.owners bij in Azure Active Directory. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Lees de eigenschap policies.policiesAppliedTo in Azure Active Directory. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Lees de eigenschap servicePrincipals.appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Werk de eigenschap servicePrincipals.appRoleAssignedTo bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Lees de eigenschap servicePrincipals.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Werk de eigenschap servicePrincipals.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/audience/update | De eigenschap servicePrincipals.audience bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/authentication/update | De eigenschap servicePrincipals.authentication bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/read | Lees de basiseigenschappen voor servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/basic/update | Werk de basiseigenschappen voor servicePrincipals bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/create | Maak servicePrincipals in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/credentials/update | De eigenschap servicePrincipals.credentials bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Lees de eigenschap servicePrincipals.memberOf in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Lees de eigenschap servicePrincipals.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/read | Lees de eigenschap servicePrincipals.owners in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/owners/update | Werk de eigenschap servicePrincipals.owners bij in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Lees de eigenschap servicePrincipals.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/permissions/update | De eigenschap servicePrincipals.permissions bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/read | Lees de eigenschap servicePrincipals.policies in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directorySync/allEntities/allTasks | Voer alle acties uit in Azure AD Connect. |

### <a name="directory-writers"></a>Schrijvers van mappen
Kan lezen en schrijven van basic directory-informatie. Voor het verlenen van toegang tot toepassingen, niet is bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/basic/update | Werk de basiseigenschappen voor groupSettings bij in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/create | Maak groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettings/delete | Verwijder groupSettings in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |

### <a name="exchange-service-administrator"></a>Exchange Service-beheerder
Kan alle aspecten van het product Exchange beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.exchange/allEntities/allTasks | Beheer alle aspecten van Exchange Online. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="external-identity-provider-administrator"></a>Externe id-Provider beheerder
Id-providers voor gebruik in directe federatie configureren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Lees en id-providers configureren in Azure Active Directory B2C. |

### <a name="guest-inviter"></a>Afzender van gastuitnodigingen
Kan onafhankelijk van de instelling 'leden kunnen gasten uitnodigen' gastgebruikers uitnodigen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Lees de eigenschap users.appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/users/basic/read | Lees de basiseigenschappen voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/directReports/read | Lees de eigenschap users.directReports in Azure Active Directory. |
| microsoft.aad.directory/users/inviteGuest | Nodig gastgebruikers uit in Azure Active Directory. |
| microsoft.aad.directory/users/manager/read | Lees de eigenschap users.manager in Azure Active Directory. |
| microsoft.aad.directory/users/memberOf/read | Lees de eigenschap users.memberOf in Azure Active Directory. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Lees de eigenschap users.oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/users/ownedDevices/read | Lees de eigenschap users.ownedDevices in Azure Active Directory. |
| microsoft.aad.directory/users/ownedObjects/read | Lees de eigenschap users.ownedObjects in Azure Active Directory. |
| microsoft.aad.directory/users/registeredDevices/read | Lees de eigenschap users.registeredDevices in Azure Active Directory. |

### <a name="helpdesk-administrator"></a>Helpdeskbeheerder
Kan wachtwoorden voor niet-beheerders en Helpdesk-medewerkers opnieuw instellen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="information-protection-administrator"></a>Information Protection-beheerder
Kan alle aspecten van het product Azure Information Protection beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/devices/basic/update | Werk de basiseigenschappen voor apparaten bij in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/devices/create | Maak apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/delete | Verwijder apparaten in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredOwners/update | Werk de eigenschap devices.registeredOwners bij in Azure Active Directory. |
| microsoft.aad.directory/devices/registeredUsers/update | Werk de eigenschap devices.registeredUsers bij in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |

### <a name="license-administrator"></a>Licentiebeheerder
Kan productlicenties voor gebruikers en groepen beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/usageLocation/update | Werk de eigenschap users.usageLocation bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="lync-service-administrator"></a>Lync-servicebeheerder
Kan alle aspecten van het product Skype voor Bedrijven beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | Beheer alle aspecten van Skype voor bedrijven Online. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in het Office 365-berichtencentrum lezen. 

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Gebruik geen - niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/domains/allTasks | Maak en verwijder domeinen en lees alle standaardeigenschappen in Azure Active Directory en werk deze bij. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/organization/basic/update | Werk de basiseigenschappen voor een organisatie bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="power-bi-service-administrator"></a>Power BI-servicebeheerder
Kan alle aspecten van het Power BI-product beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Beheer alle aspecten van Power BI. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="privileged-authentication-administrator"></a>Bevoorrechte verificatiebeheerder
Is gemachtigd voor het weergeven, instellen en opnieuw instellen van de gegevens voor de verificatiemethode van elke gebruiker (beheerder of niet-beheerder).

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | De eigenschappen van sterke verificatie bijwerken, bijvoorbeeld MFA-referentiegegevens. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kan roltoewijzingen in Azure AD en alle aspecten van Privileged Identity Management beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | Werk directoryRoles bij in Azure Active Directory. |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.privilegedIdentityManagement en werk deze bij. |

### <a name="reports-reader"></a>Rapportenlezer
Kan rapporten met betrekking tot aanmeldingen en controles lezen.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligingsgegevens en -rapporten lezen en configuratie beheren in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | Werk de eigenschap applications.policies bij in Azure Active Directory. |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/policies/basic/update | Werk de basiseigenschappen voor beleid bij in Azure Active Directory. |
| microsoft.aad.directory/policies/create | Maak beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/delete | Verwijder beleid in Azure Active Directory. |
| microsoft.aad.directory/policies/owners/update | Werk de eigenschap policies.owners bij in Azure Active Directory. |
| microsoft.aad.directory/policies/tenantDefault/update | De eigenschap policies.tenantDefault bijwerken in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/policies/update | Werk de eigenschap servicePrincipals.policies bij in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lees alle resources in microsoft.aad.identityProtection. |
| microsoft.aad.identityProtection/allEntities/update | Werk alle resources bij in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.protectionCenter/allEntities/update | Werk alle resources bij in microsoft.office365.protectionCenter. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="security-reader"></a>Beveiligingslezer
Kan beveiligingsgegevens en -rapporten lezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | De eigenschap devices.bitLockerRecoveryKeyss lezen in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.aad.identityProtection/allEntities/read | Lees alle resources in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.protectionCenter/allEntities/read | Lees alle aspecten van Office 365 Protection Center. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="service-support-administrator"></a>Beheerder serviceondersteuning
Kan gegevens over de servicestatus lezen en ondersteuningstickets beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="sharepoint-service-administrator"></a>SharePoint-servicebeheerder
Kan alle aspecten van de SharePoint-service beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.sharepoint/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.office365.sharepoint en werk deze bij. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="teams-communications-administrator"></a>Teams-communicatiebeheerder
Kan de aanroep- en vergaderfuncties van de service Microsoft Teams beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="teams-communications-support-engineer"></a>Ondersteuningstechnicus voor Teams-communicatie
Kan communicatieproblemen in Teams oplossen met geavanceerde hulpprogramma's.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="teams-communications-support-specialist"></a>Ondersteuningsspecialist voor Teams-communicatie
Kan communicatieproblemen in Teams oplossen met basishulpprogramma's.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |

### <a name="teams-service-administrator"></a>Teams-servicebeheerder
Kan de service Microsoft Teams beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie de bovenstaande beschrijving van de functie voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | De eigenschap groups.unified bijwerken in Azure Active Directory. |
| microsoft.aad.directory/groups/unified/basic/update | De basiseigenschappen van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/create | Office 365-groepen maken. |
| microsoft.aad.directory/groups/unified/delete | Office 365-groepen verwijderen. |
| microsoft.aad.directory/groups/unified/members/update | Lidmaatschap van Office 365-groepen bijwerken. |
| microsoft.aad.directory/groups/unified/owners/update | Eigendom van Office 365-groepen bijwerken. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="user-administrator"></a>Gebruikerbeheerder
Kan alle aspecten van gebruikers en groepen beheren, inclusief het opnieuw instellen van wachtwoorden voor bepaalde beheerders.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Maak appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/delete | Verwijder appRoleAssignments in Azure Active Directory. |
| microsoft.aad.directory/appRoleAssignments/update | Werk appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Werk de eigenschap groups.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/groups/basic/update | Werk de basiseigenschappen voor groepen bij in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. Maker wordt toegevoegd als de eigenaar van de eerste en het gemaakte object in mindering gebracht op de maker van 250 gemaakte objecten quotum. |
| microsoft.aad.directory/groups/delete | Verwijder groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/hiddenMembers/read | Lees de eigenschap groups.hiddenMembers in Azure Active Directory. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/groups/restore | Herstel groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/settings/update | Werk de eigenschap groups.settings bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/create | Maak gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Bijwerken van wachtwoorden voor alle gebruikers in Azure Active Directory. Zie de onlinedocumentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

## <a name="role-template-ids"></a>Rolsjabloon-id

Rolsjabloon-id worden voornamelijk door gebruikers van Graph API of PowerShell gebruikt.

Graph displayName | Weergavenaam van Azure portal | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Toepassingsbeheerder | Toepassingsbeheerder | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Toepassingsontwikkelaar | Toepassingsontwikkelaar | CF1C38E5-3621-4004-A7CB-879624DCED7C
Verificatiebeheerder | Verificatiebeheerder | c4e39bd9-1100-46d3-8c65-fb160da0071f
Factureringsbeheerder | Factureringsbeheerder | b0f54661-2d74-4c50-afa3-1ec803f12efe
Desktop Analytics-beheerder | Desktop Analytics-beheerder | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Cloudtoepassingsbeheerder | Cloudtoepassingsbeheerder | 158c047a-c907-4556-b7ef-446551a6b5f7
Cloud-apparaatbeheerder | Cloudapparaatbeheerder | 7698a772-787b-4ac8-901f-60d6b08affd2
Bedrijfsbeheerder | Globale beheerder | 62e90394-69f5-4237-9190-012177145e10
Beheerder voor naleving | Beheerder voor naleving | 17315797-102d-40b4-93e0-432062caca18
Voorwaardelijke toegang beheerder | Beheerder voor voorwaardelijke toegang | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM-servicebeheerder | Dynamics 365-beheerder | 44367163-eba1-44c3-98af-f5787879f96a
Toegangsfiatteur voor Klanten-lockbox | Klanten-Lockbox toegang goedkeurder | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Apparaatbeheerders | Apparaatbeheerders | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Toevoegen | Toevoegen | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Apparaatbeheerders | Device Manager-services | 2b499bcd-da44-4968-8aec-78e1674fa64d
Gebruikers van apparaten | Gebruikers van apparaten | d405c6df-0af8-4e3b-95e4-4d06e542189e
Adreslijstlezers | Adreslijstlezers | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Synchronisatie van Active Directory-Accounts | Adreslijstsynchronisatieaccounts | d29b2b05-8046-44ba-8758-1e26182fcf32
Schrijvers van mappen | Adreslijstschrijvers | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Service-beheerder | Exchange-beheerder | 29232cdf-9323-42fd-ade2-1d097af3e4de
Afzender van gastuitnodigingen | Afzender van gastuitnodigingen | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Helpdeskbeheerder | Wachtwoordbeheerder | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Information Protection-beheerder | Information Protection-beheerder | 7495fdc4-34c4-4d15-a289-98788ce399fd
Intune-servicebeheerder | Intune-beheerder | 3a2c62db-5318-420d-8d74-23affee5d9d5
Licentiebeheerder | Licentiebeheerder | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync-servicebeheerder | Skype voor Bedrijven-beheerder | 75941009-915a-4869-abe7-691bff18279e
Berichtencentrum-lezer | Berichtencentrum-lezer | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Laag1-ondersteuning voor partner | Laag1-ondersteuning voor partner | 4ba39ca4-527c-499a-b93d-d9b492c50246
Laag2-ondersteuning voor partner | Laag2-ondersteuning voor partner | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Power BI-servicebeheerder | Power BI-beheerder | a9ea8996-122f-4c74-9520-8edcd192826c
Bevoorrechte verificatiebeheerder | Bevoorrechte verificatiebeheerder | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Beheerder met bevoorrechte rol | Beheerder voor bevoorrechte rollen | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapportenlezer | Rapportenlezer | 4a5d8f65-41da-4de4-8968-e035b65339cf
Beveiligingsbeheerder | Beveiligingsbeheerder | 194ae4cb-b126-40b2-bd5b-6091b380977d
Beveiligingslezer | Beveiligingslezer | 5d6b6bb7-de71-4623-b4af-96380a352509
Beheerder serviceondersteuning | Servicebeheerder | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint-servicebeheerder | SharePoint-beheerder | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Teams-communicatiebeheerder | Teams-communicatiebeheerder | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Ondersteuningstechnicus voor Teams-communicatie | Ondersteuningstechnicus voor Teams-communicatie | f70938a0-fc10-4177-9e90-2178f8765737
Ondersteuningsspecialist voor Teams-communicatie | Ondersteuningsspecialist voor Teams-communicatie | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Teams-servicebeheerder | Teams-servicebeheerder | 69091246-20e8-4a56-aa4d-066075b2a7a8
Gebruiker | Gebruiker | a0b1b346-4d3e-4e8b-98f8-753987be4970
Beheerder van gebruikersaccounts | Gebruikersbeheerder | fe930be7-5e62-47db-91af-98c3a49a38b1
Werkplekapparaat toevoegen | Werkplekapparaat toevoegen | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen moeten niet worden gebruikt. Ze zijn afgeschaft en wordt verwijderd uit Azure AD in de toekomst.

* Ad-hoclicentiebeheerder
* Toevoegen
* Apparaatbeheerders
* Gebruikers van apparaten
* Maker van geverifieerde e-mailgebruiker
* Postvakbeheerder
* Werkplekapparaat toevoegen

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het toewijzen van een gebruiker als een beheerder van een Azure-abonnement, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md)
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
