---
title: Beschrijvingen en machtigingen van beheerdersrol-Azure Active Directory | Microsoft Docs
description: Een beheerdersrol kan gebruikers toevoegen, beheerders rollen toewijzen, gebruikers wachtwoorden opnieuw instellen, gebruikers licenties beheren of domeinen beheren.
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/04/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e139b274ab8a1f7d91d46ec56171b84db4f5025e
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812826"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Machtigingen voor beheerdersrol in Azure Active Directory

Met Azure Active Directory (Azure AD) kunt u beperkte beheerders aanwijzen voor het beheren van identiteits taken in functies met minder bevoegdheden. Beheerders kunnen voor dergelijke doel einden worden toegewezen om gebruikers toe te voegen of te wijzigen, beheerders rollen toe te wijzen, gebruikers wachtwoorden opnieuw in te stellen, gebruikers licenties te beheren en domein namen te beheren. De standaard gebruikers machtigingen kunnen alleen worden gewijzigd in gebruikers instellingen in azure AD.

## <a name="limit-the-use-of-global-administrator"></a>Het gebruik van de globale beheerder beperken

Gebruikers die zijn toegewezen aan de rol van globale beheerder kunnen elke beheer instelling in uw Azure AD-organisatie lezen en wijzigen. De persoon die zich aanmeldt voor een Azure-abonnement krijgt standaard de rol van globale beheerder voor de Azure AD-organisatie toegewezen. Alleen globale beheerders en bevoegde beheerdersrol kunnen beheerders rollen delegeren. Om het risico voor uw bedrijf te verminderen, wordt u aangeraden deze rol toe te wijzen aan de minste mogelijke personen in uw organisatie.

## <a name="best-practices"></a>Aanbevolen procedures

Als best practice wordt u aangeraden deze rol toe te wijzen aan minder dan 5 personen in uw organisatie. Als u meer dan vijf gebruikers hebt toegewezen aan de rol van globale beheerder in uw organisatie, zijn dit een aantal manieren om het gebruik te verminderen.

### <a name="find-the-role-you-need"></a>Zoek de gewenste rol

Als u de functie die u nodig hebt uit een lijst met veel rollen wilt vinden, kunt u met Azure AD kortere lijsten maken op basis van Rolgroepen. Bekijk het nieuwe **type** filter voor [Azure AD-rollen en-beheerders](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) om alleen de rollen in het geselecteerde type weer te geven.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>Er bestaat nu een rol die niet bestaat tijdens het toewijzen van de rol globale beheerder

Het is mogelijk dat een rol of rollen aan Azure AD zijn toegevoegd met meer gedetailleerde machtigingen die geen optie waren wanneer u een aantal gebruikers hebt uitgebreid naar de globale beheerder. Na verloop van tijd worden er extra rollen geïmplementeerd die taken uitvoeren die alleen de rol van globale beheerder kunnen uitvoeren. U kunt deze weer geven in de volgende [beschik bare rollen](#available-roles).

## <a name="assign-or-remove-administrator-roles"></a>Beheerders rollen toewijzen of verwijderen

Zie [beheerders rollen weer geven en toewijzen in azure Active Directory](directory-manage-roles-portal.md)voor meer informatie over het toewijzen van beheerders rollen aan een gebruiker in azure Active Directory.

## <a name="available-roles"></a>Beschik bare rollen

De volgende beheerders rollen zijn beschikbaar:

* **[Toepassings beheerder](#application-administrator)** : Gebruikers met deze rol kunnen alle aspecten van bedrijfs toepassingen, toepassings registraties en toepassings proxy-instellingen maken en beheren. Deze rol verleent ook de mogelijkheid om toestemming te geven aan gedelegeerde machtigingen en toepassings machtigingen, met uitzonde ring van Microsoft Graph en Azure AD Graph. Gebruikers die aan deze rol zijn toegewezen, worden niet toegevoegd als eigen aren bij het maken van nieuwe toepassings registraties of zakelijke toepassingen.

  <b>Belang rijk</b>: Deze rol biedt de mogelijkheid om toepassings referenties te beheren. Gebruikers aan wie deze rol is toegewezen, kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om de identiteit van de toepassing te imiteren. Als de identiteit van de toepassing toegang heeft gekregen tot Azure Active Directory, zoals de mogelijkheid om gebruikers of andere objecten te maken of bij te werken, kan een gebruiker die is toegewezen aan deze rol deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om de identiteit van de toepassing te imiteren, kan een uitbrei ding van bevoegdheden hebben ten opzichte van wat de gebruiker kan doen via hun roltoewijzingen in azure AD. Het is belang rijk om te begrijpen dat het toewijzen van een gebruiker aan de rol toepassings beheerder hen de mogelijkheid biedt om de identiteit van een toepassing te imiteren.

* **[Toepassings ontwikkelaar](#application-developer)** : Gebruikers met deze rol kunnen toepassings registraties maken wanneer de instelling ' gebruikers kunnen toepassingen registreren ' is ingesteld op Nee. Deze rol verleent ook toestemming om de toestemming te geven aan de hand van een eigen naam wanneer de instelling ' gebruikers kunnen toestemming geven voor het openen van Bedrijfs gegevens namens hun naam ' is ingesteld op Nee. Gebruikers die aan deze rol zijn toegewezen, worden toegevoegd als eigen aren bij het maken van nieuwe toepassings registraties of zakelijke toepassingen.

* **[Verificatie beheerder](#authentication-administrator)** : Gebruikers met deze rol kunnen referenties voor niet-wacht woord instellen of opnieuw instellen en kunnen wacht woorden voor alle gebruikers bijwerken. Authenticatie beheerders kunnen vereisen dat gebruikers zich opnieuw registreren bij bestaande referenties zonder wacht woord (bijvoorbeeld MFA of FIDO) en **MFA op het apparaat**intrekken, waarbij wordt gevraagd om MFA te volgen bij de volgende aanmelding van gebruikers die niet-beheerders zijn of de volgende rollen zijn alleen toegewezen:
  * Verificatiebeheerder
  * Adreslijstlezers
  * Afzender van gastuitnodigingen
  * Berichtencentrum-lezer
  * Rapportenlezer

  De rol authenticatie beheerder bevindt zich momenteel in de open bare preview. Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

  <b>Belang rijk</b>: Gebruikers met deze rol kunnen referenties wijzigen voor personen die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie binnen en buiten Azure Active Directory. Het wijzigen van de referenties van een gebruiker kan betekenen dat de identiteit en machtigingen van de gebruiker worden aangenomen. Bijvoorbeeld:

  * Toepassings registratie en eigen aren van bedrijfs toepassingen, die referenties kunnen beheren van apps waarvan ze eigenaar zijn. Deze apps hebben mogelijk privileged-machtigingen in azure AD en elders niet verleend aan verificatie beheerders. Via dit pad kan een verificatie beheerder mogelijk de identiteit van een toepassings eigenaar aannemen en vervolgens de identiteit van een bevoorrechte toepassing verder aannemen door de referenties voor de toepassing bij te werken.
  * Eigen aars van Azure-abonnementen, die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie in Azure.
  * Groeps eigenaren van beveiligings groep en Office 365, wie groepslid maatschap kan beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke informatie of kritieke configuratie in azure AD en elders.
  * Beheerders in andere services buiten Azure AD, zoals Exchange Online, Office Security and Compliance Center en Human Resources Systems.
  * Niet-beheerders als leidinggevenden, juridisch adviseur en Human Resources-werk nemers die mogelijk toegang tot gevoelige of persoonlijke informatie hebben.

* **[Azure Information Protection beheerder](#azure-information-protection-administrator)** : Gebruikers met deze rol hebben alle machtigingen in de Azure Information Protection-Service. Deze rol staat het configureren van labels toe voor het Azure Information Protection beleid, het beheren van beveiligings sjablonen en het activeren van de beveiliging. Deze rol verleent geen enkele machtiging in Identity Protection Center, Privileged Identity Management, Bewaak Office 365 Service Health of Office 365 Beveiligings-en compliancecentrum.

* **[Beheerder van B2C-gebruikers stroom](#b2c-user-flow-administrator)** : Gebruikers met deze rol kunnen B2C-Gebruikersstromen (ook wel ' ingebouwd beleid) maken en beheren in azure Portal. Door gebruikers stromen te maken of te bewerken, kunnen deze gebruikers de HTML/CSS/java script-inhoud van de gebruikers ervaring wijzigen, de MFA-vereisten per gebruikers stroom wijzigen, claims wijzigen in het token en sessie-instellingen aanpassen voor alle beleids regels in de Tenant. Aan de andere kant omvat deze rol niet de mogelijkheid om gebruikers gegevens te controleren of wijzigingen aan te brengen in de kenmerken die zijn opgenomen in het Tenant schema. Wijzigingen in het beleid voor identiteits ervaring (ook wel aangepast) ligt ook buiten het bereik van deze rol.

* **[Beheerder van B2C-gebruikers stroom kenmerk](#b2c-user-flow-attribute-administrator)** : Gebruikers met deze rol kunnen aangepaste kenmerken toevoegen of verwijderen die beschikbaar zijn voor alle gebruikers stromen in de Tenant. Als zodanig kunnen gebruikers met deze rol nieuwe elementen wijzigen of toevoegen aan het eind gebruikers schema en invloed hebben op het gedrag van alle gebruikers stromen en indirect als gevolg van wijzigingen in welke gegevens kunnen worden gesteld aan eind gebruikers en uiteindelijk als claims naar toepassingen worden verzonden. Deze rol kan geen gebruikers stromen bewerken.

* **[B2C IEF sleutelsetcursor](#b2c-ief-keyset-administrator)** :    Gebruiker kan beleids sleutels en geheimen maken en beheren voor token versleuteling, token handtekeningen en claim versleuteling/ontsleuteling. Door nieuwe sleutels aan bestaande sleutel containers toe te voegen, kan deze beperkte beheerder geheimen naar behoefte overzetten zonder dat dit van invloed is op bestaande toepassingen. Deze gebruiker kan de volledige inhoud van deze geheimen en de verval datums bekijken, zelfs na het maken ervan.
    
  <b>Belang rijk:</b> dit is een gevoelige rol. De rol sleutelsetcursor moet zorgvuldig worden gecontroleerd en worden toegewezen tijdens de preproductie en productie.

* **[B2C IEF-beleids beheerder](#b2c-ief-policy-administrator)** : Gebruikers met deze rol kunnen alle aangepaste beleids regels maken, lezen, bijwerken en verwijderen in Azure AD B2C en hebben daarom volledige controle over het Framework voor identiteits ervaring in de relevante Azure AD B2C Tenant. Door beleids regels te bewerken, kan deze gebruiker directe Federatie tot stand brengen met externe ID-providers, het Directory-schema wijzigen, alle gebruikers gerichte inhoud wijzigen (HTML, CSS, java script), de vereisten wijzigen voor het volt ooien van een verificatie, het maken van nieuwe gebruikers, verzenden gebruikers gegevens naar externe systemen met inbegrip van volledige migraties en bewerk alle gebruikers gegevens, inclusief gevoelige velden zoals wacht woorden en telefoon nummers. Deze rol kan de versleutelings sleutels daarentegen niet wijzigen of de geheimen bewerken die worden gebruikt voor Federatie in de Tenant.

  <b>Belangrijk:</b> De B2 IEF-beleids beheerder is een zeer gevoelige rol die zeer beperkt moet worden toegewezen aan tenants in de productie omgeving. Activiteiten door deze gebruikers moeten nauw keurig worden gecontroleerd, met name voor tenants in de productie omgeving.

* **[Facturerings beheerder](#billing-administrator)** : Doet aankopen, beheert abonnementen, beheert ondersteuningstickets en controleert de status van de service.

* **[Cloud toepassings beheerder](#cloud-application-administrator)** : Gebruikers met deze rol hebben dezelfde machtigingen als de rol toepassings beheerder, met uitzonde ring van de mogelijkheid om toepassings proxy te beheren. Met deze rol kunnen alle aspecten van bedrijfs toepassingen en toepassings registraties worden gemaakt en beheerd. Deze rol verleent ook de mogelijkheid om toestemming te geven aan gedelegeerde machtigingen en toepassings machtigingen, met uitzonde ring van Microsoft Graph en Azure AD Graph. Gebruikers die aan deze rol zijn toegewezen, worden niet toegevoegd als eigen aren bij het maken van nieuwe toepassings registraties of zakelijke toepassingen.

  <b>Belang rijk</b>: Deze rol biedt de mogelijkheid om toepassings referenties te beheren. Gebruikers aan wie deze rol is toegewezen, kunnen referenties toevoegen aan een toepassing en deze referenties gebruiken om de identiteit van de toepassing te imiteren. Als de identiteit van de toepassing toegang heeft gekregen tot Azure Active Directory, zoals de mogelijkheid om gebruikers of andere objecten te maken of bij te werken, kan een gebruiker die is toegewezen aan deze rol deze acties uitvoeren tijdens het imiteren van de toepassing. Deze mogelijkheid om de identiteit van de toepassing te imiteren, kan een uitbrei ding van bevoegdheden hebben ten opzichte van wat de gebruiker kan doen via hun roltoewijzingen in azure AD. Het is belang rijk om te begrijpen dat het toewijzen van een gebruiker aan de rol van de Cloud toepassings beheerder de mogelijkheid biedt om de identiteit van een toepassing te imiteren.

* **[Beheerder van Cloud apparaat](#cloud-device-administrator)** : Gebruikers met deze rol kunnen apparaten in azure AD inschakelen, uitschakelen en verwijderen en Windows 10 BitLocker-sleutels (indien aanwezig) in de Azure Portal lezen. De rol verleent geen machtigingen voor het beheren van andere eigenschappen op het apparaat.

* **[Beheerder voor naleving](#compliance-administrator)** : Gebruikers met deze rol hebben machtigingen voor het beheren van aan naleving gerelateerde functies in het Microsoft 365 compliance Center, Microsoft 365 beheer centrum, Azure en Office 365 Beveiligings-en compliancecentrum. Gebruikers kunnen ook alle functies in het Exchange-beheer centrum en teams beheren & het beheer centrum van Skype voor bedrijven en ondersteunings tickets maken voor Azure en Microsoft 365. Meer informatie vindt u op de [Office 365-beheerders rollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  In | Kan doen
  ----- | ----------
  [Nalevings centrum Microsoft 365](https://protection.office.com) | De gegevens van uw organisatie beveiligen en beheren in Microsoft 365 Services<br>Nalevings waarschuwingen beheren
  [Nalevings beheerder](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | De regelgeving voor naleving van uw organisatie bijhouden, toewijzen en verifiëren
  [Office 365 Beveiligings-en compliancecentrum](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beheer van gegevens beheren<br>Juridisch en gegevens onderzoek uitvoeren<br>Aanvraag voor gegevens onderwerp beheren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle intune-controle gegevens weer geven
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Heeft alleen-lezen-machtigingen en kan waarschuwingen beheren<br>Kan bestands beleid maken en wijzigen en bestandsbeheer acties toestaan<br> Kan alle ingebouwde rapporten weer geven onder Gegevensbeheer

* **[Beheerder van nalevings gegevens](#compliance-data-administrator)** : Gebruikers met deze rol hebben machtigingen om gegevens te beveiligen en bij te houden in het Microsoft 365 compliance Center, Microsoft 365 beheer centrum en Azure. Gebruikers kunnen ook alle functies in het Exchange-beheer centrum, Compliance Manager en teams beheren & het beheer centrum van Skype voor bedrijven en ondersteunings tickets maken voor Azure en Microsoft 365.

  In | Kan doen
  ----- | ----------
  [Nalevings centrum Microsoft 365](https://protection.office.com) | Nalevings beleid bewaken over Microsoft 365 Services<br>Nalevings waarschuwingen beheren
  [Nalevings beheerder](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | De regelgeving voor naleving van uw organisatie bijhouden, toewijzen en verifiëren
  [Office 365 Beveiligings-en compliancecentrum](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beheer van gegevens beheren<br>Juridisch en gegevens onderzoek uitvoeren<br>Aanvraag voor gegevens onderwerp beheren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle intune-controle gegevens weer geven
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Heeft alleen-lezen-machtigingen en kan waarschuwingen beheren<br>Kan bestands beleid maken en wijzigen en bestandsbeheer acties toestaan<br> Kan alle ingebouwde rapporten weer geven onder Gegevensbeheer

* **[Beheerder van voorwaardelijke toegang](#conditional-access-administrator)** : Gebruikers met deze rol kunnen Azure Active Directory instellingen voor voorwaardelijke toegang beheren.
  > [!NOTE]
  > Als u beleid voor voorwaardelijke toegang van Exchange ActiveSync wilt implementeren in azure, moet de gebruiker ook een globale beheerder zijn.
  
* **[Klanten-lockbox-fiatteur voor toegang](#customer-lockbox-access-approver)** : Beheert [klanten-lockbox-aanvragen](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests) in uw organisatie. Ze ontvangen e-mail meldingen voor Klanten-lockbox aanvragen en kunnen aanvragen goed keuren en weigeren vanuit het Microsoft 365-beheer centrum. Ze kunnen ook de Klanten-lockbox functie in-of uitschakelen. Alleen globale beheerders kunnen de wacht woorden van personen die aan deze rol zijn toegewezen, opnieuw instellen.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[Beheerder van Desktop Analytics](#desktop-analytics-administrator)** : Gebruikers met deze rol kunnen de bureau blad Analytics en de aanpassing van Office-&-beleids Services beheren. Voor desktop Analytics is dit onder andere de mogelijkheid om inventarisatie van assets te bekijken, implementatie plannen te maken, implementatie en status weer te geven. Voor Office Customization & Policy service kunnen gebruikers met deze rol Office-beleid beheren.

* **[Apparaatbeheer](#device-administrators)** : Deze rol is alleen beschikbaar voor toewijzing als extra lokale beheerder in [Apparaatinstellingen](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Gebruikers met deze rol worden lokale computerbeheerders op alle Windows 10-apparaten die zijn toegevoegd aan Azure Active Directory. Ze kunnen geen apparaten objecten in Azure Active Directory beheren. 

* **[Lezers van mappen](#directory-readers)** : Dit is een rol die alleen moet worden toegewezen aan verouderde toepassingen die het toestemmings [raamwerk](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)niet ondersteunen. Wijs het niet toe aan gebruikers.

* **[Directory-synchronisatie accounts](#directory-synchronization-accounts)** : Niet gebruiken. Deze rol wordt automatisch toegewezen aan de Azure AD Connect-service en is niet bedoeld of wordt niet ondersteund voor andere gebruik.

* **[Schrijvers van mappen](#directory-writers)** : Dit is een verouderde rol die moet worden toegewezen aan toepassingen die het toestemmings [raamwerk](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)niet ondersteunen. Het mag niet worden toegewezen aan gebruikers.

* **[Dynamics 365-beheerder/CRM-beheerder](#crm-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in micro soft Dynamics 365 online, wanneer de service aanwezig is, evenals de mogelijkheid om ondersteunings tickets te beheren en de service status te controleren. Meer informatie over [het gebruik van de service beheerdersrol voor het beheren van uw Tenant](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant).
  > [!NOTE] 
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als ' Dynamics 365 service Administrator '. Het is "Dynamics 365-beheerder" in de [Azure Portal](https://portal.azure.com).

* **[Exchange-beheerder](#exchange-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft CRM Online, wanneer de service aanwezig is. Daarnaast beschikt u over de mogelijkheid om alle Office 365-groepen te maken en beheren, ondersteunings tickets te beheren en de service status te controleren. Meer informatie [over Office 365-beheerders rollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als Exchange service-beheerder. Het is ' Exchange Administrator ' in de [Azure Portal](https://portal.azure.com). Het is ' Exchange Online beheerder ' in het [Exchange-beheer centrum](https://go.microsoft.com/fwlink/p/?LinkID=529144). 

* **[Beheerder van externe ID-provider](#external-identity-provider-administrator)** : Deze beheerder beheert Federatie tussen Azure Active Directory tenants en externe ID-providers. Met deze rol kunnen gebruikers nieuwe id-providers toevoegen en alle beschik bare instellingen configureren (bijvoorbeeld het pad Authentication, Service-id, toegewezen sleutel containers). Deze gebruiker kan de Tenant in staat stellen verificaties van externe ID-providers te vertrouwen. De impact op de ervaring van de eind gebruiker is afhankelijk van het type Tenant:
  * Azure Active Directory tenants voor werk nemers en partners: De toevoeging van een Federatie (bijvoorbeeld met Gmail) heeft onmiddellijk invloed op alle uitnodigingen van gasten die nog niet zijn ingewisseld. Zie [Google toevoegen als een id-provider voor B2B-gast gebruikers](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).
  * Azure Active Directory B2C-tenants: De toevoeging van een Federatie (bijvoorbeeld met Facebook of met een andere Azure Active Directory) is niet onmiddellijk van invloed op eind gebruikers, totdat de ID-provider is toegevoegd als een optie in een gebruikers stroom (ook wel ingebouwd beleid). Zie [Configure a Microsoft-account als een id-provider](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) voor een voor beeld. Als u de gebruikers stromen wilt wijzigen, is de beperkte rol ' B2C User flow Administrator ' vereist.

* **[Beheerder van globale beheerder/bedrijf](#company-administrator)** : Gebruikers met deze rol hebben toegang tot alle beheer functies in Azure Active Directory en services die gebruikmaken van Azure Active Directory identiteiten, zoals Microsoft 365 Security Center, Microsoft 365 compliance Center, Exchange Online, share point online en Skype voor bedrijven online. De persoon die zich aanmeldt voor de Azure Active Directory Tenant, wordt een globale beheerder. Alleen globale beheerders kunnen andere beheerders rollen toewijzen. Uw bedrijf kan meer dan één globale beheerder zijn. Globale beheerders kunnen het wacht woord voor elke gebruiker en alle andere beheerders opnieuw instellen.

  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als ' bedrijfs beheerder '. Het is "globale beheerder" in de [Azure Portal](https://portal.azure.com).
  >
  >

* **[Gast uitnodiging](#guest-inviter)** : Gebruikers met deze rol kunnen uitnodigingen van Azure Active Directory B2B-gast gebruiker beheren wanneer de leden de gebruikers instelling **kunnen uitnodigen** is ingesteld op Nee. Meer informatie over B2B-samen werking bij de [samen werking met Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Het bevat geen andere machtigingen.

* **[Helpdesk beheerder](#helpdesk-administrator)** : Gebruikers met deze rol kunnen wacht woorden wijzigen, tokens voor vernieuwen ongeldig maken, service aanvragen beheren en de service status controleren. Wanneer een vernieuwings token ongeldig is, wordt de gebruiker gedwongen zich opnieuw aan te melden. Helpdesk beheerders kunnen wacht woorden opnieuw instellen en vernieuwings tokens van andere gebruikers die niet-beheerders zijn, alleen toewijzen aan de volgende rollen:
  * Adreslijstlezers
  * Afzender van gastuitnodigingen
  * Helpdeskbeheerder
  * Berichtencentrum-lezer
  * Rapportenlezer
  
  <b>Belang rijk</b>: Gebruikers met deze rol kunnen wacht woorden wijzigen voor personen die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie binnen en buiten Azure Active Directory. Het wijzigen van het wacht woord van een gebruiker kan betekenen dat de identiteit en machtigingen van de gebruiker worden aangenomen. Bijvoorbeeld:
  * Toepassings registratie en eigen aren van bedrijfs toepassingen, die referenties kunnen beheren van apps waarvan ze eigenaar zijn. Deze apps hebben mogelijk privileged-machtigingen in azure AD en elders niet verleend aan helpdesk beheerders. Via dit pad kan een helpdesk beheerder de identiteit van een toepassings eigenaar aannemen en vervolgens de identiteit van een bevoorrechte toepassing verder aannemen door de referenties voor de toepassing bij te werken.
  * Eigen aars van Azure-abonnementen, die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie in Azure.
  * Groeps eigenaren van beveiligings groep en Office 365, wie groepslid maatschap kan beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke informatie of kritieke configuratie in azure AD en elders.
  * Beheerders in andere services buiten Azure AD, zoals Exchange Online, Office Security and Compliance Center en Human Resources Systems.
  * Niet-beheerders als leidinggevenden, juridisch adviseur en Human Resources-werk nemers die mogelijk toegang tot gevoelige of persoonlijke informatie hebben.


  > [!NOTE]
  > Het overdragen van beheerders machtigingen via subsets van gebruikers en het Toep assen van beleid op een subset van gebruikers is mogelijk met [beheer eenheden (preview)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).


  > [!NOTE]
  > Deze rol heette voorheen ' Wachtwoord beheerder ' in [Azure Portal](https://portal.azure.com/). We hebben de naam gewijzigd in "helpdesk beheerder" zodat deze overeenkomt met de naam in azure AD Power shell, Azure AD Graph API en Microsoft Graph-API.

* **[Intune-beheerder](#intune-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen binnen Microsoft Intune online, wanneer de service aanwezig is. Daarnaast bevat deze rol de mogelijkheid om gebruikers en apparaten te beheren om het beleid te koppelen, en om groepen te maken en te beheren. Meer informatie over op [rollen gebaseerd beheer beheer (RBAC) met Microsoft intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als intune-service beheerder. Het is de ' intune-beheerder ' in de [Azure Portal](https://portal.azure.com).
  
 * **[Kaizala beheerder](#kaizala-administrator)** : Gebruikers met deze rol hebben algemene machtigingen voor het beheren van instellingen in micro soft Kaizala, wanneer de service aanwezig is, en de mogelijkheid om ondersteunings tickets te beheren en de service status te controleren.
Daarnaast heeft de gebruiker toegang tot rapporten met betrekking tot de acceptatie & gebruik van Kaizala door organisatie leden en zakelijke rapporten die zijn gegenereerd met behulp van de Kaizala-acties. 

* **[Licentie beheerder](#license-administrator)** : Gebruikers met deze rol kunnen licentie toewijzingen toevoegen, verwijderen en bijwerken voor gebruikers, groepen (met behulp van groeps licenties) en de gebruiks locatie van gebruikers beheren. De rol biedt geen mogelijkheid om abonnementen te kopen of te beheren, groepen te maken of te beheren, of gebruikers te maken of te beheren buiten de gebruiks locatie. Deze rol heeft geen toegang voor het weer geven, maken of beheren van ondersteunings tickets.

* **[Privacy-lezer van Message Center](#message-center-privacy-reader)** : Gebruikers met deze rol kunnen alle meldingen in het berichten centrum bewaken, inclusief gegevens privacy-berichten. Berichten centrum privacy lezers ontvangen e-mail meldingen met inbegrip van de privacy van gegevens en ze kunnen zich afmelden met behulp van de voor keuren voor berichten centrum. Alleen de globale beheerder en de privacy-lezer van het berichten centrum kunnen gegevens privacy-berichten lezen. Daarnaast bevat deze rol de mogelijkheid om groepen, domeinen en abonnementen weer te geven. Deze rol heeft geen machtiging om service aanvragen weer te geven, te maken of te beheren.

* **[Message Center-lezer](#message-center-reader)** : Gebruikers met deze rol kunnen meldingen en advies status updates in [Office 365 Message Center](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) voor hun organisatie controleren op geconfigureerde services zoals Exchange, intune en micro soft teams. Berichten centrum-lezers ontvangen wekelijkse e-mail samenvattingen van berichten, updates en kunnen berichten centrum berichten delen in Office 365. In azure AD hebben gebruikers die aan deze rol zijn toegewezen alleen alleen-lezen toegang tot Azure AD-services zoals gebruikers en groepen. Deze rol heeft geen toegang voor het weer geven, maken of beheren van ondersteunings tickets.

* **[Ondersteuning voor partner Tier1](#partner-tier1-support)** : Niet gebruiken. Deze rol is afgeschaft en wordt in de toekomst verwijderd uit Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal micro soft-verkoop partners en is niet bedoeld voor algemeen gebruik.

* **[Ondersteuning voor partner Tier2](#partner-tier2-support)** : Niet gebruiken. Deze rol is afgeschaft en wordt in de toekomst verwijderd uit Azure AD. Deze rol is bedoeld voor gebruik door een klein aantal micro soft-verkoop partners en is niet bedoeld voor algemeen gebruik.

* **[Wachtwoord beheerder](#password-administrator)** : Gebruikers met deze rol hebben beperkte mogelijkheden voor het beheren van wacht woorden. Deze rol biedt geen mogelijkheid om service aanvragen te beheren of de service status te controleren. Wachtwoord beheerders kunnen wacht woorden van andere gebruikers die niet-beheerders zijn of alleen leden van de volgende rollen opnieuw instellen:
  * Adreslijstlezers
  * Afzender van gastuitnodigingen
  * Wachtwoordbeheerder

* **[Power bi beheerder](#power-bi-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in Microsoft Online Power BI, wanneer de service aanwezig is, evenals de mogelijkheid om ondersteuningstickets te beheren en de servicestatus te controleren. Meer informatie over [de rol van Power bi-beheerder](https://docs.microsoft.com/power-bi/service-admin-role).
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als Power BI service beheerder. Het is ' Power BI Administrator ' in de [Azure Portal](https://portal.azure.com).

* **[Beheerder](#privileged-authentication-administrator)** voor geprivilegieerde authenticatie: Gebruikers met deze rol kunnen referenties voor niet-wacht woord instellen of opnieuw instellen voor alle gebruikers, met inbegrip van globale beheerders, en kunnen wacht woorden voor alle gebruikers bijwerken. Bevoegde authenticatie beheerders kunnen gebruikers dwingen om zich opnieuw te registreren bij bestaande niet-wachtwoord referenties (bijvoorbeeld MFA, FIDO) en ' MFA onthouden op het apparaat ' in te trekken bij de volgende aanmelding van alle gebruikers. Beheerders met geprivilegieerde verificatie kunnen:
  * Gebruikers dwingen om zich opnieuw te registreren bij bestaande niet-wachtwoord referenties (bijv. MFA, FIDO)
  * ' MFA op het apparaat onthouden ' intrekken, bij de volgende aanmelding vragen om MFA

* **[Beheerder van geprivilegieerde rol](#privileged-role-administrator)** : Gebruikers met deze rol kunnen roltoewijzingen in Azure Active Directory beheren, en in Azure AD Privileged Identity Management. Daarnaast kunt u met deze rol alle aspecten van Privileged Identity Management en administratieve eenheden beheren.

  <b>Belang rijk</b>: Deze rol biedt de mogelijkheid om toewijzingen te beheren voor alle Azure AD-rollen, inclusief de rol van globale beheerder. Deze rol bevat geen andere geprivilegieerde mogelijkheden in azure AD, zoals het maken of bijwerken van gebruikers. Gebruikers die aan deze rol zijn toegewezen, kunnen zich echter zelf of anderen extra bevoegdheid verlenen door extra rollen toe te wijzen.

* **[Rapport lezer](#reports-reader)** : Gebruikers met deze rol kunnen gegevens over gebruiks rapportage en het dash board rapporten weer geven in Microsoft 365 beheer centrum en het pakket voor de acceptatie context in Power BI. Daarnaast biedt de rol toegang tot aanmeldings rapporten en activiteiten in azure AD en gegevens die zijn geretourneerd door de Microsoft Graph rapportage-API. Een gebruiker die is toegewezen aan de rol van de rapport lezer heeft alleen toegang tot de metrische gegevens over het gebruik en de juiste aanneming. Ze hebben geen beheerders machtigingen om instellingen te configureren of om toegang te krijgen tot de productspecifieke beheer centra zoals Exchange. Deze rol heeft geen toegang voor het weer geven, maken of beheren van ondersteunings tickets.

* **[Zoek beheerder](#search-administrator)** : Gebruikers met deze rol hebben volledige toegang tot alle micro soft Search-beheer functies in het Microsoft 365-beheer centrum. Beheerders van zoeken kunnen de rollen Zoek beheerders en zoek editor delegeren naar gebruikers, en inhoud maken en beheren, zoals blad wijzers, Q & als en locaties. Daarnaast kunnen deze gebruikers het berichten centrum bekijken, service status bewaken en service aanvragen maken.

* **[Zoek editor](#search-editor)** : Gebruikers met deze rol kunnen inhoud voor micro soft Search maken, beheren en verwijderen in het beheer centrum van Microsoft 365, waaronder blad wijzers, Q & als en locaties.

* **[Beveiligings beheerder](#security-administrator)** : Gebruikers met deze rol hebben machtigingen voor het beheren van beveiligings functies in het Microsoft 365 Security Center, Azure Active Directory Identity Protection, Azure Information Protection en Office 365 Beveiligings-en compliancecentrum. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365 beveiligings-en compliancecentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).
  
  In | Kan doen
  --- | ---
  [Microsoft 365 Security Center](https://protection.office.com) | Beveiligings beleid bewaken over Microsoft 365 Services<br>Beveiligings Risico's en-waarschuwingen beheren<br>Rapporten weer geven
  Identity Protection Center | Alle machtigingen van de rol beveiligings lezer<br>Daarnaast is de mogelijkheid om alle bewerkingen voor identiteits beveiliging uit te voeren, met uitzonde ring van het opnieuw instellen van wacht woorden
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Alle machtigingen van de rol beveiligings lezer<br>Azure AD-roltoewijzingen of-instellingen **kunnen niet worden** beheerd
  [Office 365 Beveiligings-en compliancecentrum](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligings beleid beheren<br>Beveiligings Risico's weer geven, onderzoeken en hierop reageren<br>Rapporten weer geven
  Azure Advanced Threat Protection | Verdachte beveiligings activiteit bewaken en erop reageren
  Windows Defender ATP en EDR | Rollen toewijzen<br>Computer groepen beheren<br>Detectie van de Endpoint Threat en automatisch herstel configureren<br>Waarschuwingen weer geven, onderzoeken en erop reageren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Gebruikers-, apparaat-, registratie-, configuratie-en toepassings gegevens weer geven<br>Kan geen wijzigingen aanbrengen in intune
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Beheerders toevoegen, beleids regels en instellingen toevoegen, logboeken uploaden en beheer acties uitvoeren
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Kan beveiligings beleid weer geven, beveiligings statussen bekijken, beveiligings beleid bewerken, waarschuwingen en aanbevelingen weer geven, waarschuwingen en aanbevelingen negeren
  [Office 365-service status](https://docs.microsoft.com/office365/enterprise/view-service-health) | De status van Office 365-services weer geven

* **[Beveiligings operator](#security-operator)** : Gebruikers met deze rol kunnen waarschuwingen beheren en algemene alleen-lezen toegang hebben voor de functie met betrekking tot beveiliging, inclusief alle informatie in Microsoft 365 Security Center, Azure Active Directory, identiteits beveiliging, Privileged Identity Management en Office 365 Beveiligings-en compliancecentrum. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365 beveiligings-en compliancecentrum](https://docs.microsoft.com/office365/securitycompliance/permissions-in-the-security-and-compliance-center).

  In | Kan doen
  --- | ---
  [Microsoft 365 Security Center](https://protection.office.com) | Alle machtigingen van de rol beveiligings lezer<br>Waarschuwingen voor beveiligings Risico's weer geven, onderzoeken en hierop reageren
  Identity Protection Center | Alle machtigingen van de rol beveiligings lezer<br>Daarnaast is de mogelijkheid om alle bewerkingen voor identiteits beveiliging uit te voeren, met uitzonde ring van het opnieuw instellen van wacht woorden
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Alle machtigingen van de rol beveiligings lezer
  [Office 365 Beveiligings-en compliancecentrum](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Alle machtigingen van de rol beveiligings lezer<br>Beveiligings waarschuwingen weer geven, onderzoeken en hierop reageren
  Windows Defender ATP en EDR | Alle machtigingen van de rol beveiligings lezer<br>Beveiligings waarschuwingen weer geven, onderzoeken en hierop reageren
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Alle machtigingen van de rol beveiligings lezer
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Alle machtigingen van de rol beveiligings lezer
  [Office 365-service status](https://docs.microsoft.com/office365/enterprise/view-service-health) | De status van Office 365-services weer geven
<!--* **[Security Operator](#security-operator)**: Users with this role can manage alerts and have global read-only access on security-related feature, including all information in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management.-->

* **[Beveiligings lezer](#security-reader)** : Gebruikers met deze rol hebben algemene alleen-lezen toegang voor de functie met betrekking tot beveiliging, met inbegrip van alle informatie in Microsoft 365 Security Center, Azure Active Directory, identiteits beveiliging, Privileged Identity Management, en de mogelijkheid om Azure Active te lezen Active Directory-aanmeldings rapporten en controle logboeken, en in Office 365 Beveiligings-en compliancecentrum. Meer informatie over machtigingen voor Office 365 is beschikbaar op [machtigingen in de Office 365 beveiligings-en compliancecentrum](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

  In | Kan doen
  --- | ---
  [Microsoft 365 Security Center](https://protection.office.com) | Beveiligings beleid weer geven in Microsoft 365 Services<br>Beveiligings Risico's en-waarschuwingen weer geven<br>Rapporten weer geven
  Identity Protection Center | Alle beveiligings rapporten en instellingen voor beveiligings functies lezen<br><ul><li>Anti-spam<li>Versleuteling<li>Preventie van gegevens verlies<li>Anti-malware<li>Geavanceerde beveiliging tegen bedreigingen<li>Anti-phishing<li>Mailstroom regels
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Heeft alleen-lezen toegang tot alle informatie die wordt weer gegeven in azure AD PIM: Beleids regels en rapporten voor Azure AD-roltoewijzingen, beveiligings beoordelingen en in de toekomst Lees toegang tot beleids gegevens en rapporten voor scenario's naast Azure AD-roltoewijzing.<br>U **kunt zich niet** aanmelden voor Azure AD PIM of wijzigingen aanbrengen. In de PIM-portal of via Power shell kan iemand met deze rol aanvullende rollen activeren (bijvoorbeeld globale beheerder of beheerdersrol), als de gebruiker hiervoor in aanmerking komt.
  [Office 365 Beveiligings-en compliancecentrum](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | Beveiligingsbeleid bekijken<br>Beveiligings Risico's weer geven en onderzoeken<br>Rapporten weer geven
  Windows Defender ATP en EDR | Waarschuwingen weer geven en onderzoeken
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | Gebruikers-, apparaat-, registratie-, configuratie-en toepassings gegevens weer geven. Kan geen wijzigingen aanbrengen in intune.
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | Heeft alleen-lezen-machtigingen en kan waarschuwingen beheren
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | Kan aanbevelingen en waarschuwingen weer geven, beveiligings beleid weer geven, beveiligings status weer geven, maar kan geen wijzigingen aanbrengen
  [Office 365-service status](https://docs.microsoft.com/office365/enterprise/view-service-health) | De status van Office 365-services weer geven

* **[Ondersteunings beheerder voor service](#service-support-administrator)** : Gebruikers met deze rol kunnen ondersteunings aanvragen openen met micro soft voor Azure en Office 365-Services, en het service dashboard en berichten centrum weer geven in het [Azure Portal](https://portal.azure.com) en [Microsoft 365 beheer centrum](https://admin.microsoft.com). Meer informatie [over Office 365-beheerders rollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als ' service ondersteunings beheerder '. Het is ' service beheerder ' in de [Azure Portal](https://portal.azure.com), het [Microsoft 365 beheer centrum](https://admin.microsoft.com)en de intune-Portal.

* **[Share point-beheerder](#sharepoint-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in micro soft share point online, wanneer de service aanwezig is, evenals de mogelijkheid om alle Office 365-groepen te maken en beheren, ondersteunings tickets te beheren en de service status te controleren. Meer informatie [over Office 365-beheerders rollen](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als share point-service beheerder. Het is share point-beheerder in de [Azure Portal](https://portal.azure.com).

* **[Skype voor bedrijven/Lync-beheerder](#lync-service-administrator)** : Gebruikers met deze rol hebben algemene machtigingen in micro soft Skype voor bedrijven, wanneer de service aanwezig is, en het beheren van Skype-specifieke gebruikers kenmerken in Azure Active Directory. Daarnaast verleent deze rol de mogelijkheid om ondersteunings tickets te beheren en de service status te controleren en om toegang te krijgen tot de teams en het beheer centrum van Skype voor bedrijven. Het account moet ook een licentie hebben voor teams of Power shell-cmdlets kunnen niet worden uitgevoerd. Meer informatie over de licentie gegevens voor [de rol van Skype voor bedrijven-beheerder](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5) en teams in [Skype voor bedrijven en micro soft teams-invoeg toepassingen](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als Lync-service beheerder. Het is "Skype voor bedrijven-beheerder" in de [Azure Portal](https://portal.azure.com/).

* **[Team beheerder](#teams-service-administrator)** : Gebruikers met deze rol kunnen alle aspecten van de werk belasting van micro soft teams beheren via micro soft teams & het beheer centrum van Skype voor bedrijven en de respectieve Power shell-modules. Dit omvat onder andere alle beheer hulpprogramma's die betrekking hebben op telefonie, berichten, vergaderingen en de teams zelf. Deze rol verleent daarnaast de mogelijkheid om alle Office 365-groepen te maken en beheren, ondersteunings tickets te beheren en de service status te controleren.
  > [!NOTE]
  > In Microsoft Graph-API, Azure AD Graph API en Azure AD Power shell wordt deze rol aangeduid als teams service beheerder. Het is ' teams beheerder ' in de [Azure Portal](https://portal.azure.com).

* **[Communicatie beheerder teams](#teams-communications-administrator)** : Gebruikers met deze rol kunnen aspecten van de werk belasting van micro soft teams beheren die betrekking hebben op spraak & telefonie. Dit omvat de beheer hulpprogramma's voor telefoon nummer toewijzing, spraak-en Vergader beleid en volledige toegang tot de Call Analytics-hulp programmaset.

* **[Team communicatie](#teams-communications-support-engineer)** -ondersteunings technicus: Gebruikers met deze rol kunnen communicatie problemen in micro soft-teams oplossen & Skype voor bedrijven met behulp van de hulp middelen voor het oplossen van problemen met gebruikers aanroepen in het micro soft teams & Skype voor bedrijven-beheer centrum. Gebruikers met deze rol kunnen volledige informatie over de oproep record voor alle betrokken deel nemers weer geven. Deze rol heeft geen toegang voor het weer geven, maken of beheren van ondersteunings tickets.

* **[Team communicatie support specialist](#teams-communications-support-specialist)** : Gebruikers met deze rol kunnen communicatie problemen in micro soft-teams oplossen & Skype voor bedrijven met behulp van de hulp middelen voor het oplossen van problemen met gebruikers aanroepen in het micro soft teams & Skype voor bedrijven-beheer centrum. Gebruikers met deze rol kunnen alleen gebruikers details weer geven in de aanroep voor de specifieke gebruiker die ze hebben gezocht. Deze rol heeft geen toegang voor het weer geven, maken of beheren van ondersteunings tickets.

* **[Gebruikers beheerder](#user-administrator)** : Gebruikers met deze rol kunnen gebruikers maken en alle aspecten van gebruikers met enkele beperkingen beheren (zie hieronder) en het verloop beleid voor wacht woorden kan bijwerken. Daarnaast kunnen gebruikers met deze rol alle groepen maken en beheren. Deze rol omvat ook de mogelijkheid om gebruikers weergaven te maken en beheren, ondersteunings tickets te beheren en de service status te controleren.

  | | |
  | --- | --- |
  |Algemene machtigingen|<p>Gebruikers en groepen maken</p><p>Gebruikers weergaven maken en beheren</p><p>Office-ondersteunings tickets beheren<p>Verloop beleid voor wacht woorden bijwerken|
  |<p>Voor alle gebruikers, inclusief alle beheerders</p>|<p>Licenties beheren</p><p>Alle gebruikers eigenschappen beheren, met uitzonde ring van Principal-naam van gebruiker</p>
  |Alleen voor gebruikers die niet-beheerders zijn of in een van de volgende beperkte beheerders rollen:<ul><li>Adreslijstlezers<li>Afzender van gastuitnodigingen<li>Helpdeskbeheerder<li>Berichtencentrum-lezer<li>Rapportenlezer<li>Gebruikerbeheerder|<p>Verwijderen en herstellen</p><p>Uitschakelen en inschakelen</p><p>Vernieuwings tokens ongeldig maken</p><p>Alle gebruikers eigenschappen beheren, met inbegrip van Principal-naam van gebruiker</p><p>Wachtwoord opnieuw instellen</p><p>Apparaatinstellingen bijwerken (FIDO)</p>
  
  <b>Belang rijk</b>: Gebruikers met deze rol kunnen wacht woorden wijzigen voor personen die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie binnen en buiten Azure Active Directory. Het wijzigen van het wacht woord van een gebruiker kan betekenen dat de identiteit en machtigingen van de gebruiker worden aangenomen. Bijvoorbeeld:
  * Toepassings registratie en eigen aren van bedrijfs toepassingen, die referenties kunnen beheren van apps waarvan ze eigenaar zijn. Deze apps hebben mogelijk privileged-machtigingen in azure AD en andere personen die niet aan gebruikers beheerders zijn toegekend. Via dit pad kan een gebruikers beheerder mogelijk de identiteit van een toepassings eigenaar aannemen en vervolgens de identiteit van een bevoorrechte toepassing verder aannemen door de referenties voor de toepassing bij te werken.
  * Eigen aars van Azure-abonnementen, die mogelijk toegang hebben tot gevoelige of persoonlijke informatie of een kritieke configuratie in Azure.
  * Groeps eigenaren van beveiligings groep en Office 365, wie groepslid maatschap kan beheren. Deze groepen kunnen toegang verlenen tot gevoelige of persoonlijke informatie of kritieke configuratie in azure AD en elders.
  * Beheerders in andere services buiten Azure AD, zoals Exchange Online, Office Security and Compliance Center en Human Resources Systems.
  * Niet-beheerders als leidinggevenden, juridisch adviseur en Human Resources-werk nemers die mogelijk toegang tot gevoelige of persoonlijke informatie hebben.

## <a name="role-permissions"></a>Rolmachtigingen
In de volgende tabellen worden de specifieke machtigingen in Azure Active Directory beschreven die aan elke rol worden gegeven. Sommige rollen hebben mogelijk extra machtigingen in micro soft-services buiten Azure Active Directory.

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
Kan toepassings registraties maken onafhankelijk van de instelling gebruikers kunnen toepassingen registreren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Maak toepassingen in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Maak appRoleAssignments in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Maak oAuth2PermissionGrants in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Maak servicePrincipals in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |

### <a name="authentication-administrator"></a>Verificatiebeheerder
Voor het weer geven, instellen en opnieuw instellen van verificatie methode-informatie voor een niet-beheerders gebruiker.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/strongAuthentication/update | De eigenschappen van sterke verificatie bijwerken, bijvoorbeeld MFA-referentiegegevens. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.aad.directory/users/password/update | Update wacht woorden voor alle gebruikers in de Office 365-organisatie. Raadpleeg de online documentatie voor meer informatie. |

### <a name="azure-information-protection-administrator"></a>Azure Information Protection beheerder
Kan alle aspecten van de Azure Information Protection-Service beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="b2c-user-flow-administrator"></a>B2C User Flow Administrator
Alle aspecten van gebruikers stromen maken en beheren.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Gebruikers stromen lezen en configureren in Azure Active Directory B2C. |

### <a name="b2c-user-flow-attribute-administrator"></a>B2C User Flow Attribute Administrator
Het kenmerk schema maken en beheren dat beschikbaar is voor alle gebruikers stromen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Gebruikers kenmerken lezen en configureren in Azure Active Directory B2C. |

### <a name="b2c-ief-keyset-administrator"></a>B2C IEF Keyset Administrator
Geheimen voor Federatie en versleuteling beheren in het Framework voor identiteits ervaring.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Lees en configureer sleutel sets in Azure Active Directory B2C. |

### <a name="b2c-ief-policy-administrator"></a>B2C IEF Policy Administrator
Beleid voor vertrouwens relaties maken en beheren in het Framework voor identiteits ervaring.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Aangepaste beleids regels lezen en configureren in Azure Active Directory B2C. |

### <a name="billing-administrator"></a>Factureringsbeheerder
Kan algemene taken met betrekking tot facturering uitvoeren, zoals betalingsgegevens bijwerken.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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

### <a name="compliance-data-administrator"></a>Beheerder van nalevings gegevens
Hiermee wordt inhoud voor naleving gemaakt en beheerd.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security lezen en configureren. |
| microsoft.azure.informationProtection/allEntities/allTasks | Beheer alle aspecten van Azure Information Protection. |
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
Kan mogelijkheden voor voorwaardelijke toegang beheren.

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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.lockbox/allEntities/allTasks | Beheer alle aspecten van Office 365 Klanten-lockbox |

### <a name="desktop-analytics-administrator"></a>Desktop Analytics-beheerder
Kan de bureau blad Analytics en de aanpassing van Office-&-beleids Services beheren. Voor desktop Analytics is dit onder andere de mogelijkheid om inventarisatie van assets te bekijken, implementatie plannen te maken, implementatie en status weer te geven. Voor Office Customization & Policy service kunnen gebruikers met deze rol Office-beleid beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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

### <a name="device-administrators"></a>Apparaatbeheerders
Gebruikers die aan deze rol zijn toegewezen, worden toegevoegd aan de lokale groep Administrators op apparaten die lid zijn van Azure AD.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Lees de basiseigenschappen voor groupSettings in Azure Active Directory. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Lees de basiseigenschappen voor groupSettingTemplates in Azure Active Directory. |

### <a name="directory-readers"></a>Adreslijstlezers
Kan basis informatie over de Directory lezen. Voor het verlenen van toegang tot toepassingen, niet bedoeld voor gebruikers.

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

### <a name="directory-synchronization-accounts"></a>Directory-synchronisatie accounts
Wordt alleen gebruikt door Azure AD Connect service.

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
Kan basis informatie over de Directory lezen & schrijven. Voor het verlenen van toegang tot toepassingen, niet bedoeld voor gebruikers.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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

### <a name="external-identity-provider-administrator"></a>Beheerder van externe ID-provider
Configureer id-providers voor gebruik in directe Federatie.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Id-providers lezen en configureren in Azure Active Directory B2C. |

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
| microsoft.aad.directory/users/password/update | Wacht woorden bijwerken voor alle gebruikers in Azure Active Directory. Raadpleeg de online documentatie voor meer informatie. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="intune-service-administrator"></a>Intune-servicebeheerder
Kan alle aspecten van het product Intune beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
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

### <a name="kaizala-administrator"></a>Kaizala-beheerder
Kan instellingen voor micro soft Kaizala beheren.  

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >  
  
| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | Lees het Office 365-beheer centrum. |

### <a name="license-administrator"></a>Licentiebeheerder
Kan product licenties voor gebruikers en groepen beheren.

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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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

### <a name="message-center-privacy-reader"></a>Berichten centrum-privacy-lezer
Kan berichten centrum berichten, gegevens privacy-berichten, groepen, domeinen en abonnementen lezen.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |
| microsoft.office365.messageCenter/securityMessages/read | securityMessages lezen in microsoft.office365.messageCenter. |

### <a name="message-center-reader"></a>Berichtencentrum-lezer
Kan berichten en updates voor hun organisatie alleen in het Office 365-berichtencentrum lezen. 

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |

### <a name="partner-tier1-support"></a>Laag1-ondersteuning voor partner
Niet gebruiken-niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Werk de basiseigenschappen voor contactpersonen bij in Azure Active Directory. |
| microsoft.aad.directory/contacts/create | Maak contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/contacts/delete | Verwijder contactpersonen in Azure Active Directory. |
| microsoft.aad.directory/groups/create | Maak groepen in Azure Active Directory. |
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
| microsoft.aad.directory/groups/members/update | Werk de eigenschap groups.members bij in Azure Active Directory. |
| microsoft.aad.directory/groups/owners/update | Werk de eigenschap groups.owners bij in Azure Active Directory. |
| microsoft.aad.directory/users/appRoleAssignments/update | Werk de eigenschap users.appRoleAssignments bij in Azure Active Directory. |
| microsoft.aad.directory/users/assignLicense | Beheer licenties voor gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/basic/update | Werk de basiseigenschappen voor gebruikers bij in Azure Active Directory. |
| microsoft.aad.directory/users/delete | Verwijder gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Maak alle vernieuwingstokens voor gebruikers ongeldig in Azure Active Directory. |
| microsoft.aad.directory/users/manager/update | Werk de eigenschap users.manager bij in Azure Active Directory. |
| microsoft.aad.directory/users/password/update | Wacht woorden bijwerken voor alle gebruikers in Azure Active Directory. Raadpleeg de online documentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="partner-tier2-support"></a>Laag2-ondersteuning voor partner
Niet gebruiken-niet bedoeld voor algemeen gebruik.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
| microsoft.aad.directory/users/password/update | Wacht woorden bijwerken voor alle gebruikers in Azure Active Directory. Raadpleeg de online documentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

### <a name="password-administrator"></a>Wachtwoordbeheerder
Kan wacht woorden voor niet-beheerders en wachtwoord beheerders opnieuw instellen.

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/users/password/update | Wacht woorden bijwerken voor alle gebruikers in Azure Active Directory. Raadpleeg de online documentatie voor meer informatie. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |

### <a name="power-bi-service-administrator"></a>Power BI-servicebeheerder
Kan alle aspecten van het Power BI-product beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
| microsoft.aad.directory/users/password/update | Update wacht woorden voor alle gebruikers in de Office 365-organisatie. Raadpleeg de online documentatie voor meer informatie. |
### <a name="privileged-role-administrator"></a>Beheerder met bevoorrechte rol
Kan roltoewijzingen in azure AD en alle aspecten van Privileged Identity Management beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | Maak en verwijder alle resources en lees de standaardeigenschappen in microsoft.aad.privilegedIdentityManagement en werk deze bij. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/allTasks | Lees en configureer de eigenschap servicePrincipals. appRoleAssignedTo in Azure Active Directory. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Lees en configureer de eigenschap servicePrincipals. oAuth2PermissionGrants in Azure Active Directory. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Beheer eenheden maken en beheren (inclusief leden) |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Roltoewijzingen maken en beheren. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Roldefinities maken en beheren. |

### <a name="reports-reader"></a>Rapportenlezer
Kan rapporten met betrekking tot aanmeldingen en controles lezen.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in auditLogs in Azure Active Directory. |
| microsoft.aad.directory/signInReports/allProperties/read | Alle eigenschappen (inclusief bevoegde eigenschappen) lezen in signInReports in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="search-administrator"></a>Beheerder zoeken
Kan alle aspecten van instellingen voor micro soft Search maken en beheren.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |
| microsoft.office365.search/allEntities/allProperties/allTasks | Maak en verwijder alle resources en Lees alle eigenschappen in micro soft. office365. Search en werk deze bij. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |

### <a name="search-editor"></a>Zoek editor
Kan de redactionele inhoud maken en beheren, zoals blad wijzers, Q en as, locaties, floorplan.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | Berichten lezen in microsoft.office365.messageCenter. |
| microsoft.office365.search/content/allProperties/allTasks | Maak en verwijder inhoud en Lees alle eigenschappen in micro soft. office365. Search en werk deze bij. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |

### <a name="security-administrator"></a>Beveiligingsbeheerder
Kan beveiligings gegevens en-rapporten lezen en configuratie beheren in azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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

### <a name="security-operator"></a>Beveiligings operator
Maakt en beheert beveiligings gebeurtenissen.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
  >
  >

| **Acties** | **Beschrijving** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security lezen en configureren. |
| microsoft.aad.identityProtection/allEntities/read | Lees alle resources in microsoft.aad.identityProtection. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | Lees alle resources in microsoft.aad.privilegedIdentityManagement. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Lees en configureer Azure AD Advanced Threat Protection. |
| microsoft.intune/allEntities/allTasks | Beheer alle aspecten van Intune. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | Beveiligings-en compliancecentrum lezen en configureren. |
| microsoft.office365.usageReports/allEntities/read | Lees Office 365-gebruiksrapporten. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Lees en configureer Windows Defender Advanced Threat Protection. |

### <a name="security-reader"></a>Beveiligingslezer
Kan beveiligingsgegevens en -rapporten lezen in Azure AD en Office 365.

  > [!NOTE]
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
  > Deze rol heeft aanvullende machtigingen buiten Azure Active Directory. Zie beschrijving van rol hierboven voor meer informatie.
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
| microsoft.aad.directory/groups/createAsOwner | Maak groepen in Azure Active Directory. De Maker wordt toegevoegd als de eerste eigenaar en het gemaakte object telt op het quotum van 250 gemaakte objecten van de maker. |
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
| microsoft.aad.directory/users/password/update | Wacht woorden bijwerken voor alle gebruikers in Azure Active Directory. Raadpleeg de online documentatie voor meer informatie. |
| microsoft.aad.directory/users/restore | Herstel verwijderde gebruikers in Azure Active Directory. |
| microsoft.aad.directory/users/userPrincipalName/update | Werk de eigenschap users.userPrincipalName bij in Azure Active Directory. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Lees en configureer Azure Service Health. |
| microsoft.azure.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Azure-ondersteuning. |
| microsoft.office365.webPortal/allEntities/basic/read | De basiseigenschappen van alle resources lezen in microsoft.office365.webPortal. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Lees en configureer de Office 365-servicestatus. |
| microsoft.office365.supportTickets/allEntities/allTasks | Maak en beheer tickets voor Office 365-ondersteuning. |

## <a name="role-template-ids"></a>Id van de functie sjabloon

Rollen sjabloon-Id's worden voornamelijk gebruikt door Graph API-of Power shell-gebruikers.

Grafiek weergave naam | Weergave naam Azure Portal | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
Toepassingsbeheerder | Toepassingsbeheerder | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
Toepassingsontwikkelaar | Toepassingsontwikkelaar | CF1C38E5-3621-4004-A7CB-879624DCED7C
Verificatiebeheerder | Verificatiebeheerder | c4e39bd9-1100-46d3-8c65-fb160da0071f
Azure Information Protection beheerder | Azure Information Protection beheerder | 7495fdc4-34c4-4d15-a289-98788ce399fd
Beheerder van B2C-gebruikers stroom | Beheerder van B2C-gebruikers stroom | 6e591065-9bad-43ed-90f3-e9424366d2f0
B2C User Flow Attribute Administrator | B2C User Flow Attribute Administrator | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
B2C IEF Keyset Administrator | B2C IEF Keyset Administrator | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C IEF Policy Administrator | B2C IEF Policy Administrator | 3edaf663-341e-4475-9f94-5c398ef6c070
Factureringsbeheerder | Factureringsbeheerder | b0f54661-2d74-4c50-afa3-1ec803f12efe
Cloudtoepassingsbeheerder | Cloudtoepassingsbeheerder | 158c047a-c907-4556-b7ef-446551a6b5f7
Cloud-apparaatbeheerder | Cloudapparaatbeheerder | 7698a772-787b-4ac8-901f-60d6b08affd2
Bedrijfsbeheerder | Globale beheerder | 62e90394-69f5-4237-9190-012177145e10
Beheerder voor naleving | Beheerder voor naleving | 17315797-102d-40b4-93e0-432062caca18
Beheerder van nalevings gegevens | Beheerder van nalevings gegevens | e6d1a23a-da11-4be4-9570-befc86d067a7
Voorwaardelijke toegang beheerder | Beheerder voor voorwaardelijke toegang | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM-servicebeheerder | Dynamics 365-beheerder | 44367163-eba1-44c3-98af-f5787879f96a
Toegangsfiatteur voor Klanten-lockbox | Klanten-lockbox Access-fiatteur | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
Desktop Analytics-beheerder | Desktop Analytics-beheerder | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
Apparaatbeheerders | Beheerders van apparaten | 9f06204d-73c1-4d4c-880a-6edb90606fd8
Apparaat toevoegen | Apparaat toevoegen | 9c094953-4995-41c8-84c8-3ebb9b32c93f
Apparaatbeheerders | Apparaatbeheer | 2b499bcd-da44-4968-8aec-78e1674fa64d
Gebruikers van het apparaat | Gebruikers van het apparaat | d405c6df-0af8-4e3b-95e4-4d06e542189e
Adreslijstlezers | Adreslijstlezers | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
Directory-synchronisatie accounts | Adreslijstsynchronisatieaccounts | d29b2b05-8046-44ba-8758-1e26182fcf32
Schrijvers van mappen | Adreslijstschrijvers | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange Service-beheerder | Exchange-beheerder | 29232cdf-9323-42fd-ade2-1d097af3e4de
Beheerder van externe ID-provider | Beheerder van externe ID-provider | be2f45a1-457d-42af-a067-6ec1fa63bc45
Afzender van gastuitnodigingen | Afzender van gastuitnodigingen | 95e79109-95c0-4d8e-aee3-d01accf2d47b
Helpdeskbeheerder | Wachtwoordbeheerder | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Intune-servicebeheerder | Intune-beheerder | 3a2c62db-5318-420d-8d74-23affee5d9d5
Kaizala-beheerder | Kaizala-beheerder | 74ef975b-6605-40af-a5d2-b9539d836353
Licentiebeheerder | Licentiebeheerder | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync-servicebeheerder | Skype voor Bedrijven-beheerder | 75941009-915a-4869-abe7-691bff18279e
Berichten centrum-privacy-lezer | Berichten centrum-privacy-lezer | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
Berichtencentrum-lezer | Berichtencentrum-lezer | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
Laag1-ondersteuning voor partner | Laag1-ondersteuning voor partner | 4ba39ca4-527c-499a-b93d-d9b492c50246
Laag2-ondersteuning voor partner | Laag2-ondersteuning voor partner | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
Wachtwoordbeheerder | Wachtwoordbeheerder | 966707d0-3269-4727-9be2-8c3a10f19b9d
Power BI-servicebeheerder | Power BI-beheerder | a9ea8996-122f-4c74-9520-8edcd192826c
Bevoorrechte verificatiebeheerder | Bevoorrechte verificatiebeheerder | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
Beheerder met bevoorrechte rol | Beheerder voor bevoorrechte rollen | e8611ab8-c189-46e8-94e1-60213ab1f814
Rapportenlezer | Rapportenlezer | 4a5d8f65-41da-4de4-8968-e035b65339cf
Beheerder zoeken | Beheerder zoeken | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
Zoek editor | Zoek editor | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
Beveiligingsbeheerder | Beveiligingsbeheerder | 194ae4cb-b126-40b2-bd5b-6091b380977d
Beveiligings operator | Beveiligings operator | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f
Beveiligingslezer | Beveiligingslezer | 5d6b6bb7-de71-4623-b4af-96380a352509
Beheerder serviceondersteuning | Servicebeheerder | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint-servicebeheerder | Share point-beheerder | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Teams-communicatiebeheerder | Teams-communicatiebeheerder | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Ondersteuningstechnicus voor Teams-communicatie | Ondersteuningstechnicus voor Teams-communicatie | f70938a0-fc10-4177-9e90-2178f8765737
Ondersteuningsspecialist voor Teams-communicatie | Ondersteuningsspecialist voor Teams-communicatie | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Teams-servicebeheerder | Teams-servicebeheerder | 69091246-20e8-4a56-aa4d-066075b2a7a8
Gebruiker | Gebruiker | a0b1b346-4d3e-4e8b-98f8-753987be4970
Beheerder van gebruikersaccounts | Gebruikersbeheerder | fe930be7-5e62-47db-91af-98c3a49a38b1
Werkplekapparaat toevoegen | Werkplek apparaat toevoegen | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>Afgeschafte functies

De volgende rollen mogen niet worden gebruikt. Ze zijn afgeschaft en worden in de toekomst verwijderd uit Azure AD.

* Ad-hoclicentiebeheerder
* Apparaat toevoegen
* Apparaatbeheerders
* Gebruikers van het apparaat
* Maker van geverifieerde e-mailgebruiker
* Postvakbeheerder
* Werkplekapparaat toevoegen

## <a name="next-steps"></a>Volgende stappen

* Zie [toegang beheren met RBAC en de Azure Portal](../../role-based-access-control/role-assignments-portal.md) voor meer informatie over het toewijzen van een gebruiker als beheerder van een Azure-abonnement.
* Als u meer wilt weten over hoe de toegang tot resources wordt beheerd in Microsoft Azure, ziet u [Inzicht krijgen in toegang tot resources in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Zie [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](../fundamentals/active-directory-how-subscriptions-associated-directory.md) voor meer informatie over hoe Azure Active Directory aan uw Azure-abonnement wordt gekoppeld
