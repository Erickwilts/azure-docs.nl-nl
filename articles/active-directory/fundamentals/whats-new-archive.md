---
title: Archief voor de wat is er nieuw? -Azure Active Directory | Microsoft Docs
description: Wat is nieuw opmerkingen bij de release in het overzicht van de sectie van deze inhoudsset bevat zes maanden van de activiteit van. Na zes maanden worden de items verwijderd uit het belangrijkste artikel en in dit archiefartikel plaatsen.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68be46b406e7a5caaabbc0726a6aece0fd0423ce
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472188"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Archief voor de wat is er nieuw? in Azure Active Directory

De primaire [wat is er nieuwe opmerkingen bij de release](whats-new.md) artikel bevat de laatste zes maanden van gegevens, hoewel dit artikel de oudere gegevens bevat.

Wat is nieuw opmerkingen bij de release bieden u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies
- Plannen voor wijzigingen

---

## <a name="novemberdecember-2018"></a>November/December 2018

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>Gebruikers verwijderd uit synchronisatie bereik niet langer switch naar alleen-cloud-accounts

**Type:** Vast  
**Categorie van de service:** Gebruikersbeheer  
**Product-mogelijkheid:** Directory

>[!Important]
>We hebben op basis van en inzicht in uw frustraties vanwege deze oplossing. Dus hebben we hersteld deze wijziging tot aan het moment dat we kunnen de oplossing voor u eenvoudiger te implementeren in uw organisatie.

We hebben een bug waarin de vlag DirSyncEnabled van een gebruiker zou worden ten onrechte overgeschakeld naar de vaste **False** wanneer het Active Directory Domain Services (AD DS)-object is uitgesloten van synchronisatie-bereik en klikt u vervolgens verplaatst naar de Prullenbak in Azure AD op de volgende synchronisatiecyclus. Als gevolg van deze oplossing wordt als de gebruiker is uitgesloten van het bereik van gegevenssynchronisatie en daarna hersteld op basis van Azure AD Recycle Bin account van de gebruiker blijft als die zijn gesynchroniseerd vanaf on-premises AD, zoals verwacht en kan niet worden beheerd in de cloud, omdat de bron van bronrecords authority (SoA) blijft on-premises AD.

Voordat u deze oplossing is het een probleem wanneer de vlag DirSyncEnabled is overgeschakeld naar de waarde False. Het heeft de verkeerde indruk dat deze accounts zijn geconverteerd naar alleen-cloud-objecten en dat de accounts in de cloud kunnen worden beheerd. De accounts bewaard echter nog steeds hun SoA als on-premises en alle gesynchroniseerde eigenschappen (kenmerken) die afkomstig zijn van on-premises AD. Dit probleem veroorzaakt meerdere problemen in Azure AD en andere cloudwerkbelastingen (zoals Exchange Online) die u verwacht dat deze accounts behandelen als gesynchroniseerd uit Active Directory, maar zijn nu gedraagt, zoals accounts alleen in de cloud.

Op dit moment is de enige manier om echt een gesynchroniseerd van AD-account converteren naar alleen-cloud-account door het uitschakelen van DirSync op tenantniveau, waarvan een back endbewerking om over te dragen van de SoA wordt geactiveerd. Dit type SoA wijziging vereist (maar is niet beperkt tot) het opschonen van alle de on-premises gerelateerde kenmerken (zoals LastDirSyncTime en de kenmerken van) en een signaal verzenden naar andere cloudworkloads naar de respectieve object wordt omgezet in een alleen-cloud-account te hebben .

Deze oplossing is als gevolg daarvan wordt voorkomen dat directe updates op het kenmerk ImmutableID van een gebruiker gesynchroniseerd uit Active Directory dat in sommige scenario's in het verleden vereist zijn. Standaard de ImmutableID van een object in Azure AD, zoals de naam al aangeeft, is bedoeld om onveranderbaar zijn. Nieuwe functies die zijn geïmplementeerd in Azure AD Connect Health en Azure AD Connect-synchronisatie-client zijn beschikbaar voor dergelijke scenario's:

- **Grootschalige ImmutableID-update voor een groot aantal gebruikers in een gefaseerde benadering**
  
  Bijvoorbeeld, moet u een langdurige migratie van AD DS tussen forests. Oplossing: Gebruik Azure AD Connect **Bronanker configureren** en wanneer de gebruiker worden gemigreerd, kopieert u de bestaande ImmutableID-waarden van Azure AD naar ms-DS-consistentie-Guid-kenmerk van de lokale AD DS-gebruiker van het nieuwe forest. Zie voor meer informatie, [met behulp van ms-DS-ConsistencyGuid as sourceAnchor](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor).

- **Grootschalige ImmutableID updates voor veel gebruikers in één keer**

  Bijvoorbeeld bij het implementeren van Azure AD Connect u een fout maakt, en nu moet u het kenmerk SourceAnchor wijzigen. Oplossing: Uitschakelen van DirSync op het tenantniveau van de en schakel alle ongeldige ImmutableID-waarden. Zie voor meer informatie, [uitschakelen voor adreslijstsynchronisatie van Office 365](/office365/enterprise/turn-off-directory-synchronization).

- **Rematch on-premises gebruiker met een bestaande gebruiker in Azure AD** bijvoorbeeld een gebruiker die opnieuw worden gemaakt in AD DS is een duplicaat in Azure AD-account in plaats van het met een bestaande Azure AD-account (zwevende object) rematching genereert. Oplossing: Azure AD Connect Health gebruiken in Azure portal opnieuw toewijzen van anker/ImmutableID van de bron. Zie voor meer informatie, [zwevende object scenario](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario).

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>Belangrijke wijziging: Updates voor de controle en het schema van de logboeken voor aanmelden via Azure Monitor

**Type:** Gewijzigde functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

We zijn momenteel log-streams voor zowel de controle en meld u via Azure Monitor, publiceren, zodat u de logboekbestanden naadloos met uw SIEM-hulpprogramma's of met Log Analytics integreren kunt. Op basis van uw feedback en ter voorbereiding van de aankondiging van de algemene beschikbaarheid van deze functie, doorvoeren we de volgende wijzigingen om onze schema. Deze wijzigingen in het schema en de bijbehorende documentatie-updates wordt uitgevoerd op de eerste week van januari.

#### <a name="new-fields-in-the-audit-schema"></a>Nieuwe velden in het schema controleren
We voegen een nieuwe toe **bewerkingstype** veld voor het type bewerking uitgevoerd op de resource. Bijvoorbeeld, **toevoegen**, **Update**, of **verwijderen**.

#### <a name="changed-fields-in-the-audit-schema"></a>Gewijzigde velden in het schema controleren
De volgende velden wijzigt in het controle-schema:

|Veldnaam|Wat is gewijzigd|Oude waarden|Nieuwe waarden|
|----------|------------|----------|----------|
|Category|Dit is de **servicenaam** veld. Het is nu de **Audit categorieën** veld. **Servicenaam** is gewijzigd in de **loggedByService** veld.|<ul><li>Account inrichten</li><li>Hoofddirectory</li><li>Self-service voor wachtwoord opnieuw instellen</li></ul>|<ul><li>Gebruikersbeheer</li><li>Groepsbeheer</li><li>App-beheer</li></ul>|
|targetResources|Bevat **TargetResourceType** op het hoogste niveau.|&nbsp;|<ul><li>Beleid</li><li>App</li><li>Gebruiker</li><li>Groep</li></ul>|
|loggedByService|Hier wordt de naam van de service die het auditlogboek gegenereerd.|Null|<ul><li>Account inrichten</li><li>Hoofddirectory</li><li>Self-service voor wachtwoord opnieuw instellen</li></ul>|
|Resultaat|Geeft het resultaat van de auditlogboeken. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li></ul>|<ul><li>Geslaagd</li><li>Fout</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>Gewijzigde velden in het schema voor aanmelden
De volgende velden wilt in het schema voor aanmelden wijzigen:

|Veldnaam|Wat is gewijzigd|Oude waarden|Nieuwe waarden|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|Dit is de **conditionalaccessPolicies** veld. Het is nu de **appliedConditionalAccessPolicies** veld.|Geen wijziging|Geen wijziging|
|conditionalAccessStatus|Geeft het resultaat van de beleidsstatus van voorwaardelijke toegang bij het aanmelden. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Geslaagd</li><li>Fout</li><li>Niet toegepast</li><li>Uitgeschakeld</li></ul>|
|appliedConditionalAccessPolicies: resultaat|Geeft het resultaat van de afzonderlijke voorwaardelijke toegang Beleidsstatus bij het aanmelden. Voorheen was dit is geïnventariseerd, maar laten we nu zien de werkelijke waarde.|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Geslaagd</li><li>Fout</li><li>Niet toegepast</li><li>Uitgeschakeld</li></ul>|

Zie voor meer informatie over het schema [interpreteren van de Azure AD-auditlogboeken schema beschikbaar zijn in Azure Monitor (preview)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>Identity Protection verbeteringen in de bewaakte machine learning-model en de risico-engine van score

**Type:** Gewijzigde functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Risicoscores

Verbeteringen in de Identity Protection-gerelateerde gebruiker en meld u risico-evaluatie-engine kunnen helpen om gebruiker risico nauwkeurigheid en dekking te verbeteren. Beheerders kunnen u ziet dat risiconiveau van de gebruiker niet meer rechtstreeks is gekoppeld aan het risiconiveau van specifieke detecties, en of er een toename van het aantal en het niveau van riskante aanmelding-gebeurtenissen.

Detecties van risico's worden nu geëvalueerd door de bewaakte machine learning-model, welke gebruikers risico's berekent met behulp van een patroon van detecties en extra functies van de aanmeldingen van de gebruiker. Op basis van dit model, kan de beheerder van de gebruikers met een hoog risicoscores, zoeken, zelfs als detecties die zijn gekoppeld aan deze gebruiker van laag of Gemiddeld risico zijn. 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>Beheerders kunnen hun eigen wachtwoord met behulp van de Microsoft Authenticator-app (preview-versie) in opnieuw instellen

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

Azure AD-beheerders kunnen nu opnieuw instellen voor hun eigen wachtwoord met behulp van de Microsoft Authenticator-app-meldingen of een code van een mobiele verificator-app of de hardware-token. Om hun eigen wachtwoord opnieuw instellen, is beheerders nu mogelijk gebruik van twee van de volgende methoden:

- Microsoft Authenticator-app-melding

- Andere mobiele verificator-app / Hardware token code

- Email

- Telefonische oproep

- Sms-bericht

Zie voor meer informatie over het gebruik van de Microsoft Authenticator-app opnieuw instellen van wachtwoorden [Azure AD Self-service voor wachtwoord opnieuw instellen - mobiele app en SSPR (Preview)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr-preview)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>Nieuwe Cloud-Apparaatbeheerder voor Azure AD-rol (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** Toegangsbeheer

Beheerders kunnen gebruikers toewijzen aan de nieuwe rol in de Cloud-Apparaatbeheerder cloud apparaat beheerderstaken uit te voeren. Gebruikers die zijn toegewezen de beheerdersrol van Cloud-apparaat kunnen inschakelen, uitschakelen en verwijderen van apparaten in Azure AD, samen met kunnen Windows 10-BitLocker-sleutels (indien aanwezig) in Azure portal worden gelezen.

Zie voor meer informatie over rollen en machtigingen, [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>Uw apparaten beheren met de tijdstempel van de nieuwe activiteit in Azure AD (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** Levenscyclus van Apparaatbeheer

We realiseren ons dat na verloop van tijd moet u vernieuwen en is voor uw organisatie apparaten buiten gebruik stellen in Azure AD, om te voorkomen dat verouderde apparaten in uw omgeving. Om te helpen met dit proces, werkt nu Azure AD uw apparaten met een nieuwe activiteit timestamp, helpen u bij het beheren van de levenscyclus van uw apparaat.

Zie voor meer informatie over het verkrijgen en gebruiken van deze timestamp [How To: De verouderde apparaten beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>Beheerders kunnen vereisen dat gebruikers de gebruiksvoorwaarden op elk apparaat accepteren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 
Beheerders kunnen nu inschakelen op de **vereisen dat gebruikers accepteren op elk apparaat** optie uw gebruikers moeten accepteer de gebruiksvoorwaarden op elk apparaat dat wordt gebruikt op uw tenant.

Zie voor meer informatie de [Per apparaat en de gebruiksvoorwaarden gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van een terugkerend schema configureren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer
 

Beheerders kunnen nu inschakelen op de **verlopen toestemmingen** optie om te maken van een gebruiksrechtovereenkomst verlopen voor alle gebruikers op basis van de opgegeven terugkerende planning. Het schema mag per jaar, bi per jaar, kwartaal of per maand. Nadat de gebruiksvoorwaarden is verlopen, moeten gebruikers accepteren.

Zie voor meer informatie de [toevoegen van de voorwaarden van het gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>Beheerders kunnen een gebruiksrechtovereenkomst verloopt op basis van de planning van elke gebruiker configureren

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

Beheerders kunnen nu opgeven voor een duur van die gebruiker moet de gebruiksvoorwaarden accepteren. Beheerders kunnen bijvoorbeeld opgeven dat een gebruiksrechtovereenkomst om de 90 dagen door gebruikers moeten accepteren.

Zie voor meer informatie de [toevoegen van de voorwaarden van het gedeelte van de voorwaarden van de Azure Active Directory van functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use).
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Nieuwe Azure AD Privileged Identity Management (PIM) e-mailberichten voor Azure Active Directory-rollen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Klanten die gebruikmaken van Azure AD Privileged Identity Management (PIM) kunnen nu ontvangen voor een wekelijkse e-mail met de samenvatting, met inbegrip van de volgende informatie voor de afgelopen zeven dagen:

- Overzicht van de bovenste in aanmerking komende en permanente roltoewijzingen

- Aantal gebruikers, rollen activeren

- Aantal gebruikers dat is toegewezen aan rollen in PIM

- Aantal gebruikers dat is toegewezen aan rollen buiten PIM

- Aantal gebruikers "en-klare permanente" in PIM

Zie voor meer informatie over PIM en de beschikbare e-mailmeldingen [e-mailmeldingen in PIM](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications).

---

### <a name="group-based-licensing-is-now-generally-available"></a>Groepslicenties is nu algemeen beschikbaar

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Directory

Groepslicenties is uit de openbare preview en is nu algemeen beschikbaar. Als onderdeel van deze algemene versie is uitgebracht, we deze functie beter schaalbaar hebt gemaakt en de mogelijkheid om te opnieuw verwerken licentieverlening toewijzingen op basis van een groep voor één gebruiker en de mogelijkheid om met Groepslicenties met Office 365 E3/A3-licenties hebt toegevoegd.

Zie voor meer informatie over Groepslicenties [wat is licentieverlening in Azure Active Directory op basis van groep?](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - November 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In November 2018, hebben we deze 26 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[CoreStack](https://cloud.corestack.io/site/login), [HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial), [GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial), [grijs Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial), [eHour](https://getehour.com/try-now), [Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial), [Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial), [DriveDollar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview), [Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial), [oneindige Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial), [Alaya](https://alayagood.com/en/demo/), [ HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial), [Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial), [Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial), [Zenegy voor bedrijven centrale 365](https://accounting.zenegy.com/), [Everbridge lid Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial), [IDEO](https://profile.ideo.com/users/sign_up), [Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial), [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial), [Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial), [Plex Apps: Klassieke Test](https://test.plexonline.com/signon), [Plex Apps – klassiek](https://www.plexonline.com/signon), [Plex Apps - UX Test](https://test.cloud.plex.com/sso), [Plex Apps – UX](https://cloud.plex.com/sso), [Plex Apps – IAM](https://accounts.plex.com/), [KNUTSELEN - kinderopvang Records, aanwezigheid en financiële volgsysteem](https://getcrafts.ca/craftsregistration) 

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

## <a name="october-2018"></a>Oktober 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Logboeken in Azure AD werkt nu met Azure Log Analytics (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

We zijn trots te kunnen aankondigen dat u nu uw Azure AD-logboeken naar Azure Log Analytics doorsturen kunt. Deze functie meest gevraagde helpt u nog beter toegang geven tot analytics voor uw bedrijf, bewerkingen, en beveiliging, evenals een manier om u te helpen bij het beheren van uw infrastructuur. Zie voor meer informatie de [Azure Active Directory activiteitenlogboeken in Azure Log Analytics nu beschikbaar](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) blog.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - oktober 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In oktober 2018, hebben we deze 14 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Mijn punten Award](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), kiesvenster, [ON24 virtuele omgeving](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler drie](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot besturingselement](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services-e-mailmeldingen

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD Domain Services  
**Product-mogelijkheid:** Azure AD Domain Services

Azure AD Domain Services biedt waarschuwingen in Azure portal over onjuiste configuraties of problemen met uw beheerde domein. Deze waarschuwingen bevatten stapsgewijze handleidingen, zodat u de problemen kunt oplossen kunt zonder dat contact opnemen met ondersteuning.

Met ingang van oktober, zal het mogelijk om aan te passen van de meldingsinstellingen voor uw beheerde domein dus wanneer er nieuwe waarschuwingen optreden, een e-mailbericht is verzonden naar een aangewezen groep mensen, hoeft u niet voortdurend controleren van de portal voor updates.

Zie voor meer informatie, [meldingsinstellingen in Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD portal ondersteunt het gebruik van het domein ForceDelete API voor aangepaste domeinen verwijderen 

**Type:** Gewijzigde functie  
**Categorie van de service:** Mapbeheer  
**Product-mogelijkheid:** Directory

We zijn trots aan u kunt nu het domein ForceDelete API gebruiken om te verwijderen van uw aangepaste domeinnamen asynchroon naam van referenties, zoals gebruikers, groepen en apps van uw aangepaste domeinnaam (contoso.com) terug naar de naam van de initiële standaard-domein ( Contoso.onmicrosoft.com).

Deze wijziging kunt u snel uw aangepaste domeinnamen als uw organisatie de naam niet meer gebruikt, of te verwijderen als u nodig hebt voor het gebruik van de domeinnaam met een andere Azure AD.

Zie voor meer informatie, [verwijderen van een aangepaste domeinnaam](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

---

## <a name="september-2018"></a>September 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Bijgewerkte machtigingen voor beheerdersrollen voor dynamische groepen

**Type:** Vast  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

We hebben een probleem is opgelost, zodat de specifieke beheerdersrollen nu kunnen maken en bijwerken van dynamisch-lidmaatschapregels, zonder dat de eigenaar van de groep.

De rollen zijn:

- Globale beheerder

- Intune-beheerder

- Gebruikerbeheerder

Zie voor meer informatie, [een dynamische groep maken en de status controleren](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Configuratie-instellingen voor vereenvoudigde eenmalige aanmelding voor sommige apps van derden

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

We realiseren ons dat instellen van eenmalige aanmelding (SSO) voor Software als een Service (SaaS)-apps kunnen lastig zijn vanwege de unieke aard van de configuratie van apps. We hebben een vereenvoudigde configuratie-ervaring voor het automatisch vullen van de SSO-configuratie-instellingen voor de volgende externe SaaS-apps gemaakt:

- Zendesk

- ArcGis Online

- Jamf Pro

Als u wilt gaan met behulp van deze ervaring met één klik, gaat u naar de **Azure-portal** > **SSO-configuratie** pagina voor de app. Zie voor meer informatie, [SaaS-toepassing-integratie met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Azure van de Active Directory - waar bevindt uw gegevens zich? pagina

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** GoLocal

Selecteer de regio van uw bedrijf uit de **Azure Active Directory - waar bevindt uw gegevens zich** pagina om te bekijken welke Azure-datacenter ook uw Azure AD-gegevens in rust voor alle Azure AD-services nieuwste. U kunt de gegevens filteren op specifieke Azure AD-services voor de regio van uw bedrijf.

Voor toegang tot deze functie en voor meer informatie, Zie [Azure Active Directory - waar bevindt uw gegevens zich](https://aka.ms/AADDataMap).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Nieuw implementatieplan beschikbaar voor het deelvenster Mijn apps-toegang

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** SSO

Bekijk de nieuwe implementatieplan die beschikbaar is voor het toegangsvenster voor mijn Apps (https://aka.ms/deploymentplans).
Het deelvenster Mijn Apps toegang biedt gebruikers met één plek om te zoeken en toegang tot hun apps. De portal bevat ook gebruikers met selfservice-mogelijkheden, zoals aanvragen van toegang tot apps en -groepen of beheren van toegang tot deze resources namens anderen.

Zie voor meer informatie, [wat is de portal mijn Apps?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Nieuw tabblad Probleemoplossing en ondersteuning op de pagina met logboeken van aanmeldingen in Azure Portal

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

De nieuwe **probleemoplossing en ondersteuning** tabblad op de **aanmeldingen** pagina van de Azure portal, is bedoeld voor beheerders en ondersteuningstechnici oplossen van problemen met betrekking tot Azure AD-aanmeldingen. Deze nieuwe tabblad bevat de foutcode, foutbericht en herstelaanbevelingen (indien aanwezig) om u te helpen bij het oplossen van het probleem. Als u niet het probleem op te lossen, ook geven we u een nieuwe manier om u te maken van een ondersteuning ticket met de **naar Klembord kopiëren** ondervindt, die vult de **aanvraag-ID** en **datum (UTC)** velden voor het logboekbestand in uw ondersteuningsticket.  

![Meld u in Logboeken van het nieuwe tabblad](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Verbeterde ondersteuning voor aangepaste uitbreidingseigenschappen gebruikt om regels voor dynamisch lidmaatschap te maken

**Type:** Gewijzigde functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking

Met deze update, u kunt nu klikt u op de **aangepaste extensie-eigenschappen ophalen** koppelen vanuit de opbouwfunctie voor dynamische gebruiker groep regel, Voer uw unieke app-ID en de volledige lijst met aangepaste extensie-eigenschappen om te gebruiken bij het maken van een dynamisch te ontvangen het lidmaatschapsregel voor gebruikers. Deze lijst kan ook worden vernieuwd om op te halen van alle nieuwe aangepaste extensie-eigenschappen voor die app.

Zie voor meer informatie over het gebruik van aangepaste extensie-eigenschappen voor de dynamisch-lidmaatschapregels [extensie-eigenschappen en aangepaste extensie-eigenschappen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor Azure AD op Apps gebaseerde voorwaardelijke toegang

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn in de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement):

- Microsoft To-Do

- Microsoft Stream

Zie voor meer informatie:

- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Nieuwe ondersteuning voor selfservice voor wachtwoordherstel op het vergrendelingsscherm van Windows 7/8/8.1

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Na het instellen van deze nieuwe functie, uw gebruikers een koppeling om hun wachtwoord opnieuw in te zien de **vergrendeling** scherm van een apparaat met Windows 7, Windows 8 of Windows 8.1. Door te klikken op de koppeling, wordt de gebruiker geleid door de dezelfde stroom voor wachtwoord opnieuw instellen via de webbrowser.

Zie voor meer informatie, [inschakelen voor wachtwoord opnieuw instellen van Windows 7, 8 en 8.1](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>U ziet dat wijzigen: Autorisatiecodes wordt niet langer beschikbaar voor hergebruik 

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Vanaf 15 November 2018, Azure AD wordt niet meer accepteren van eerder gebruikte verificatie codes voor apps. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor deze en andere wijzigingen met betrekking tot de protocollen, [de volledige lijst met wat is er nieuw voor de verificatie](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie voor Azure AD - september 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In September 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet werving Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [ JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), SSO voor Azure, SurveyMonkey Enlyft [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial), [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Ondersteuning voor aanvullende claimstransformatiemethoden

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

We hebben nieuwe claim transformatie methoden, ToLower() en ToUpper(), die kan worden toegepast op SAML-tokens uit de SAML-gebaseerde geïntroduceerd **configuratie voor eenmalige aanmelding** pagina.

Zie voor meer informatie, [over het aanpassen van uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Bijgewerkte configuratie-UI voor SAML-apps (preview-versie)

**Type:** Gewijzigde functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

Als onderdeel van onze bijgewerkt op basis van SAML appconfiguratie-UI krijgt u het:

- Een bijgewerkte scenario-ervaring voor het configureren van uw apps op basis van SAML.

- Meer zichtbaarheid over wat er ontbreekt of is onjuist in uw configuratie.

- De mogelijkheid om toe te voegen meerdere e-mailadressen voor melding over verlopen certificaat.

- Nieuwe claim transformatie methoden, ToLower() en ToUpper() en meer.

- Een manier voor het uploaden van uw eigen token handtekeningcertificaat voor apps in uw onderneming.

- Een manier om in te stellen de NameID-indeling voor SAML-apps, en een manier om in te stellen de NameID-waarde als uitbreidingen van de Directory.

Als u wilt inschakelen op deze bijgewerkte weergave, klikt u op de **proberen onze nieuwe ervaring voor** koppeling vanaf de bovenkant van de **Single Sign-On** pagina. Zie [Zelfstudie: Configureer SAML gebaseerde eenmalige aanmelding voor een toepassing met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>Augustus 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Wijzigingen in Azure Active Directory-IP-adresbereiken

**Type:** Plan voor wijziging  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Platform

We introduceren grotere IP-adresbereiken naar Azure AD, wat betekent dat als u Azure AD-IP-adresbereiken voor uw firewalls, routers of Netwerkbeveiligingsgroepen hebt geconfigureerd, moet u deze bijwerken. We doorvoeren deze update zodat u uw firewall, router of Network Security groepen IP-bereik configuraties opnieuw wijzigen hoeft wanneer Azure AD nieuwe eindpunten toevoegt. 

Netwerkverkeer wordt verplaatst naar deze nieuwe bereiken in de volgende twee maanden. Als u wilt doorgaan met de service niet wordt onderbroken, moet u deze bijgewerkte waarden toevoegen aan uw IP-adressen voor 10 September 2018:

- 20.190.128.0/18 

- 40.126.0.0/18 

Het is raadzaam de oude IP-adresbereiken niet verwijderen tot al uw netwerkverkeer is verplaatst naar de nieuwe bereiken. Zie voor updates over de overstap en voor meer informatie over wanneer u de oude bereiken kunt verwijderen, [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>U ziet dat wijzigen: Autorisatiecodes wordt niet langer beschikbaar voor hergebruik 

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Vanaf 15 November 2018, Azure AD wordt niet meer accepteren van eerder gebruikte verificatie codes voor apps. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor deze en andere wijzigingen met betrekking tot de protocollen, [de volledige lijst met wat is er nieuw voor de verificatie](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Geconvergeerd beveiligingsinformatiebeheer voor selfservice voor wachtwoordherstel en Multi-Factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie
 
Deze nieuwe functie helpt mensen hun beveiligingsgegevens (zoals, telefoonnummer, mobiele app, enzovoort) beheren voor SSPR en MFA in een enkele locatie en ervaring; in vergelijking met op eerder, waar dit is gedaan in twee verschillende locaties.

Deze ervaring geconvergeerde werkt ook voor mensen met behulp van SSPR of MFA. Bovendien, als uw organisatie niet van MFA of SSPR-registratie afdwingen, kunt mensen nog steeds registreren MFA of SSPR info beveiligingsmethoden toegestaan door uw organisatie van de portal mijn Apps.

Dit is een opt-in voor openbare preview-versie. Beheerders kunnen inschakelen op de nieuwe ervaring (indien gewenst) voor een geselecteerde groep of voor alle gebruikers in een tenant. Zie voor meer informatie over de geconvergeerde ervaring, de [geconvergeerd ervaring blog](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Nieuwe instelling Alleen HTTP-cookies in apps voor de Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer

Er is een nieuwe instelling, **HTTP-Only Cookies** in uw apps met Application Proxy. Deze instelling biedt extra beveiliging door de vlag HTTPOnly opnemen in de HTTP-antwoordheader voor beide Application Proxy toegangs- en sessiebeleid cookies, toegang aan de cookie van een client-side-script stoppen en verder te voorkomen dat bewerkingen zoals kopiëren of het wijzigen van de cookie. Hoewel deze vlag nog niet eerder zijn gebruikt, zijn uw cookies altijd versleuteld en verzonden met behulp van een SSL-verbinding om u te helpen beschermen tegen verkeerde wijzigingen.

Deze instelling is niet compatibel is met apps met behulp van ActiveX-besturingselementen, zoals Extern bureaublad. Als u bent in dit geval is, wordt u aangeraden dat u deze instelling uitschakelen.

Zie voor meer informatie over de instelling van de Cookies HTTP-Only [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Privileged Identity Management (PIM) voor Azure-resources ondersteunt resourcestypen voor beheergroepen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Instellingen voor het activeren en de toewijzing van Just-In-Time kunnen nu worden toegepast op beheergroep-resourcetypen, net zoals u al voor abonnementen, resourcegroepen en Resources (zoals virtuele machines, App Services en meer doet). Bovendien kan iedereen met een rol waarmee de toegang als beheerder voor een beheergroep detecteren en beheren van die resource in PIM.

Zie voor meer informatie over de PIM- en Azure-resources, [detecteren en beheren van Azure-resources met behulp van Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>Toegang tot toepassingen (preview-versie) biedt sneller toegang tot de Azure AD-portal

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Vandaag de dag bij het activeren van een rol met PIM, deze kan meer dan 10 minuten duren voordat de machtigingen voor het van kracht. Als u kiest voor toegang tot toepassingen, dat zich momenteel in openbare preview, beheerders hebben toegang tot de Azure AD-portal zodra de activeringsaanvraag is voltooid.

Toegang tot de toepassing ondersteunt momenteel alleen de Azure AD portal-ervaring en de Azure-resources. Zie voor meer informatie over PIM en de toepassing toegang [wat is Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de app-galerie voor Azure AD - augustus 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In augustus 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline is losgekoppeld](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [saus Labs - mobiele en Web testen](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta netwerken Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [manier waarop wij doen](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (door Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - onkosten rapporten](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [ Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Systeemeigen Tableau-ondersteuning is nu beschikbaar in Azure AD-toepassingsproxy

**Type:** Gewijzigde functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer

Met de update van de OpenID Connect voor het verlenen van OAuth 2.0-Code-protocol voor het protocol van onze vooraf-verificatie hebt u niet langer geen aanvullende instellingen voor het gebruik van Tableau met Application Proxy. Deze wijziging protocol helpt ook bij de toepassingsproxy betere ondersteuning bieden voor moderne apps met behulp van alleen HTTP-omleidingen, die vaak worden ondersteund in JavaScript en HTML-codes.

Zie voor meer informatie over onze systeemeigen ondersteuning voor Tableau [Azure AD Application Proxy nu met systeemeigen ondersteuning voor Tableau](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Nieuwe ondersteuning om Google toe te voegen als een id-provider voor B2B-gastgebruikers in Azure Active Directory (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

Door het instellen van Federatie met Google in uw organisatie, kunt u uitgenodigde Gmail gebruikers zich laten uw gedeelde apps en resources met behulp van hun bestaande Google-account, zonder te hoeven maken van een persoonlijk Microsoft-Account (MSA's) of een Azure AD-account.

Dit is een opt-in voor openbare preview-versie. Zie voor meer informatie over Google federation [Google toevoegen als een id-provider voor B2B-gastgebruikers](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Juli 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Verbeteringen voor e-mailmeldingen van Azure Active Directory

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli
 
Azure Active Directory (Azure AD) e-mailberichten functie nu het ontwerp van een bijgewerkte, evenals wijzigingen in het e-mailadres afzender en de weergavenaam van de afzender, wanneer verzonden vanuit de volgende services:
 
- Azure AD-Toegangsbeoordelingen
- Azure AD Connect Health (Engelstalig) 
- Azure AD-identiteitsbeveiliging 
- Azure AD Privileged Identity Management
- Enterprise-App verloopt certificaat meldingen
- Servicemeldingen voor inrichting van Enterprise-App
 
De e-mailmeldingen worden verzonden van de volgende e-mailadres en de weergavenaam:

- E-mailadres: azure-noreply@microsoft.com
- Weergavenaam: Microsoft Azure
 
Voor een voorbeeld van enkele van de nieuwe e-ontwerpen en meer informatie, Zie [e-mailmeldingen in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD-activiteitenlogboeken zijn nu beschikbaar via Azure Monitor

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

De Azure AD-activiteitenlogboeken zijn nu beschikbaar in openbare preview-versie van de Azure Monitor (van Azure-platform hele monitoring-service). Azure Monitor biedt u met een langetermijnbewaarperiode en naadloze integratie, naast deze verbeteringen:

- Langetermijnretentie van uw logboekbestanden routering naar uw eigen Azure storage-account.

- Naadloze SIEM-integratie, zonder dat u hoeft te schrijven of te onderhouden aangepaste scripts.

- Naadloze integratie met uw eigen aangepaste oplossingen, analysehulpprogramma's of oplossingen van incidentbeheer.

Zie onze blog voor meer informatie over deze nieuwe mogelijkheden, [Azure AD-activiteitenlogboeken in Azure Monitor diagnostics nu in openbare preview is](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) en onze documentatie [activiteitenlogboeken voor Azure Active Directory in Azure Monitor (preview)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Voorwaardelijke toegang hebben tot informatie toegevoegd aan het rapport van Azure AD-aanmeldingen

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging
 
Deze update kunt u zien welke beleidsregels worden geëvalueerd wanneer een gebruiker zich aanmeldt, samen met het resultaat van het beleid. Daarnaast bevat het rapport nu het type van de client-app die wordt gebruikt door de gebruiker, zodat u oudere protocolverkeer kunt identificeren. Rapport vermeldingen kunnen nu ook worden gezocht naar een correlatie-ID, die kan worden gevonden in het foutbericht van de gebruiker gerichte en kan worden gebruikt om te identificeren en oplossen van de overeenkomende aanmeldingsaanvraag.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Verouderde verificaties weergeven via logboeken met aanmeldingsactiviteiten

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage
 
Dankzij de introductie van de **Client-App** veld in de activiteit aanmelden zich aanmeldt, klanten kunnen nu Zie gebruikers die gebruikmaken van verouderde verificaties. Klanten moeten toegang hebben tot deze gegevens met behulp van de aanmeldingen bij MS Graph API of via de aanmelding activiteitenlogboeken in Azure AD-portal waar u kunt de **Client-App** besturingselement te filteren op verouderde verificaties. Bekijk de documentatie voor meer informatie.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Er zijn nieuwe federatieve apps beschikbaar in de galerie met apps van Azure AD - juli 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In juli 2018, hebben we deze 16 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Innovatie Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [bepaalde beheerder SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC fasering, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-automatische](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial) , PowerSchool Unified klas, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar externe ondersteuning](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial) , [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial), [vaardigheden Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Nieuwe SaaS-app-integraties voor het inrichten van gebruikers - juli 2018

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** 3e partij integratie
 
Azure AD kunt u het maken, onderhoud en verwijderen van gebruikers-id's in SaaS-toepassingen, zoals Dropbox, Salesforce, ServiceNow en automatiseren. Voor juli 2018, hebben we ondersteuning voor de volgende toepassingen in de galerie van Azure AD-app inrichten van gebruikers toegevoegd:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Zie voor een lijst van alle toepassingen die ondersteuning bieden voor het inrichten van gebruikers in de Azure AD-galerie, [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health voor synchronisatie - een eenvoudigere manier om synchronisatiefouten met zwevende en dubbele kenmerken te herstellen

**Type:** Nieuwe functie  
**Categorie van de service:** AD Connect  
**Product-mogelijkheid:** Bewaking en rapportage
 
Azure AD Connect Health introduceert herstel van self-service te markeren en synchronisatiefouten oplossen. Deze functie Hiermee lost u dubbel kenmerk synchronisatiefouten en correcties van objecten die zijn zwevende van Azure AD. Deze diagnose biedt de volgende voordelen:

- De taalinstelling van de synchronisatiefouten dubbel kenmerk, bieden specifieke oplossingen

- Van toepassing is een oplossing voor Azure AD-scenario's, het oplossen van fouten in één stap toegewezen

- Er is geen upgrade of de configuratie is in te schakelen en gebruik deze functie vereist

Zie voor meer informatie, [vaststellen en herstellen van de synchronisatiefouten dubbel kenmerk](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Visualupdates voor de aanmeldingservaring van Azure AD en van MSA

**Type:** Gewijzigde functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie

We hebben de gebruikersinterface voor de ervaring van Microsoft online services aanmelden, zoals bijgewerkt voor Office 365 en Azure. Deze wijziging stelt de schermen, minder rommelige en eenvoudiger. Zie voor meer informatie over deze wijziging, de [komende verbeteringen in de Azure Active Directory-aanmeldingservaring](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) blog.

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Nieuwe versie van Azure AD Connect - juli 2018

**Type:** Gewijzigde functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** Beheer van Identiteitslevenscycli

De meest recente versie van Azure AD Connect bevat: 

- Oplossingen voor problemen en ondersteuning-updates 

- Algemene beschikbaarheid van de integratie Ping federeren

- Updates voor de nieuwste SQL 2012-client 

Zie voor meer informatie over deze update [Azure AD Connect: Releasegeschiedenis van versie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>Updates met de voorwaarden van gebruik door eindgebruikers de gebruikersinterface

**Type:** Gewijzigde functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

De acceptatie van de tekenreeks in de gebruikersinterface van de eindgebruiker gebruiksvoorwaarden worden bijgewerkt.

**Huidige tekst.** Voor toegang tot resources [tenantName], moet u de gebruiksvoorwaarden accepteren.<br>**Nieuwe tekst.** Voor toegang tot [tenantName]-resource, moet u de gebruiksvoorwaarden lezen.

**Huidige tekst:** Betekent dat u akkoord met alle van de bovenstaande gebruiksvoorwaarden gaat accepteren te kiezen.<br>**Nieuwe tekst:** Klik op accepteren om te bevestigen dat u hebt gelezen en de gebruiksvoorwaarden begrepen.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>Pass-through-verificatie ondersteunt verouderde protocollen en toepassingen

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Nu Pass through-verificatie biedt ondersteuning voor verouderde protocollen en -apps. De volgende beperkingen zijn nu volledig ondersteund:

- Gebruikersaanmeldingen voor verouderde Office clienttoepassingen, Office 2010 en Office 2013, zonder moderne verificatie.

- Toegang tot de agenda te delen en beschikbaarheidsgegevens in hybride omgevingen voor Office 2010 alleen.

- Gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen zonder moderne verificatie.

- Gebruikersaanmeldingen naar PowerShell versie 1.0.

- Het Apple Device Enrollment Program (DEP) van Apple, met behulp van de iOS-Configuratieassistent. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Geconvergeerd beveiligingsinformatiebeheer voor selfservice voor wachtwoordherstel en Multi-Factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Deze nieuwe functie kan gebruikers hun beveiligingsgegevens (bijvoorbeeld telefoonnummer, e-mailadres, mobiele app, enzovoort) voor selfservice voor wachtwoordherstel (SSPR) en multi-factor Authentication (MFA) in één ervaring beheren. Niet meer hebben gebruikers de dezelfde beveiligingsgegevens registreren voor SSPR en MFA in twee verschillende ervaringen. Deze nieuwe ervaring is ook van toepassing op gebruikers die Self-service voor Wachtwoordherstel of MFA hebben.

Als een organisatie wordt niet van MFA of SSPR-registratie afdwingen, kunnen gebruikers zich registreren hun beveiligingsgegevens via de **mijn Apps** portal. Van daaruit kunnen gebruikers zich registreren ingeschakeld voor MFA of SSPR methoden. 

Dit is een opt-in voor openbare preview-versie. Beheerders kunnen inschakelen op de nieuwe ervaring (indien gewenst) voor een geselecteerde groep van gebruikers of alle gebruikers in een tenant.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>De app Microsoft Authenticator gebruiken om uw identiteit te verifiëren wanneer u uw wachtwoord opnieuw instelt

**Type:** Gewijzigde functie  
**Categorie van de service:** SSPR  
**Product-mogelijkheid:** Gebruikersverificatie

Deze functie kunt niet-beheerders hun identiteit te verifiëren bij het herstellen van een wachtwoord met behulp van een melding of de code van de Microsoft Authenticator (of een andere verificator-app). Nadat beheerders inschakelen dit self-service voor wachtwoord opnieuw instellen van methode, gebruikers die zich hebben geregistreerd een mobiele app via aka.ms/mfasetup of aka.ms/setupsecurityinfo kunnen hun mobiele app gebruiken als een verificatiemethode tijdens hun wachtwoord opnieuw instellen.

Mobiele app-meldingen kan alleen worden ingeschakeld als onderdeel van een beleid dat is vereist twee methoden voor uw wachtwoord opnieuw instellen.

---

## <a name="june-2018"></a>Juni 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>U ziet dat wijzigen: Beveiliging-oplossing voor de autorisatiestroom gedelegeerde voor apps met behulp van Logboeken API van Azure AD-activiteit

**Type:** Plan voor wijziging  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

Vanwege de naleving van onze sterkere beveiliging, moesten we hebben een wijziging aanbrengt in de machtigingen voor apps die gebruikmaken van een autorisatiestroom gedelegeerde voor toegang tot [Azure AD-activiteit logboeken-API's](https://aka.ms/aadreportsapi). Deze wijziging wordt uitgevoerd door **26 juni 2018**.

Als een van uw apps in Azure AD-activiteit Log-API's gebruikt, volg deze stappen om te controleren of dat de app niet wordt beëindigd nadat de wijziging gebeurt.

**Uw app-machtigingen bijwerken**

1. Aanmelden bij Azure portal, selecteer **Azure Active Directory**, en selecteer vervolgens **App-registraties**.
2. Selecteer de app die gebruikmaakt van de activiteit logboeken API van Azure AD, selecteer **instellingen**, selecteer **vereiste machtigingen**, en selecteer vervolgens de **Windows Azure Active Directory** API.
3. In de **overgedragen machtigingen** gebied van de **toegang inschakelen** blade, schakel het selectievakje in naast **lezen directory** gegevens en selecteer vervolgens **opslaan**.
4. Selecteer **machtigingen verlenen**, en selecteer vervolgens **Ja**.
    
    >[!Note]
    >U moet een globale beheerder om machtigingen aan de app te verlenen.

Zie voor meer informatie de [machtigingen verlenen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) gebied van de vereisten voor toegang tot de Azure AD-rapportage-API-artikel.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Verbinding maken met Azure AD-services voor het PCI DSS-compliance TLS-instellingen configureren

**Type:** Nieuwe functie  
**Categorie van de service:** N/A  
**Product-mogelijkheid:** Platform

Transport Layer Security (TLS) is een protocol waarmee privacy en integriteit tussen twee communicerende toepassingen en is het meest geïmplementeerde beveiligingsprotocol tegenwoordig gebruikt.

De [PCI Security Standards Council heeft onlangs](https://www.pcisecuritystandards.org/) heeft vastgesteld dat er vroege versies van TLS en Secure Sockets Layer (SSL) moeten worden uitgeschakeld en vervangen door het inschakelen van nieuwe en beter beveiligde app protocollen, met het starten van naleving op **en met 30 juni 2018**. Deze wijziging betekent dat als u verbinding met Azure AD-services maken en voldoen aan PCI DSS-beleid moet, moet u de TLS 1.0 uitschakelen. Er zijn meerdere versies van TLS beschikbaar, maar TLS 1.2 is de meest recente versie die beschikbaar zijn voor Azure Active Directory-Services. Wij raden verplaatsen rechtstreeks naar TLS 1.2 voor combinaties van zowel de client/server en de browser of de server.

Verouderde browsers ondersteunen mogelijk niet de nieuwere TLS-versies, zoals TLS 1.2. Als u wilt zien welke versies van TLS worden ondersteund door uw browser, Ga naar de [Qualys SSL Labs](https://www.ssllabs.com/) site en op **testen van uw browser**. We raden u upgraden naar de nieuwste versie van uw webbrowser en bij voorkeur inschakelen alleen TLS 1.2.

**Om in te schakelen van TLS 1.2, door de browser**

- **Microsoft Edge en Internet Explorer (beide zijn ingesteld met behulp van Internet Explorer)**

    1. Open Internet Explorer, selecteer **extra** > **Internetopties** > **Geavanceerd**.
    2. In de **Security** gedeelte **gebruik van TLS 1.2**, en selecteer vervolgens **OK**.
    3. Alle browservensters sluiten en opnieuw starten van Internet Explorer. 

- **Google Chrome**

    1. Open Google Chrome, type *chrome://settings/* in de adresbalk en druk op **Enter**.
    2. Vouw de **Geavanceerd** opties, Ga naar de **System** vlak- en selecteer **proxy-instellingen openen**.
    3. In de **Interneteigenschappen** Schakel de **Geavanceerd** tabblad, Ga naar de **Security** gedeelte **gebruik van TLS 1.2**, en selecteer vervolgens  **OK**.
    4. Alle browservensters sluiten en opnieuw starten van Google Chrome.

- **Mozilla Firefox**

    1. Open Firefox, type *over: config* in de adresbalk en druk **Enter**.
    2. Zoek naar de term *TLS*, en selecteer vervolgens de **security.tls.version.max** vermelding.
    3. Stel de waarde voor **3** om af te dwingen de browser wilt gebruiken om versie TLS 1.2 en selecteer vervolgens **OK**.

        >[!NOTE]
        >Firefox versie 60,0 biedt ondersteuning voor TLS 1.3, zodat u kunt ook de waarde security.tls.version.max instellen op **4**.

    4. Sluit alle browservensters en Mozilla Firefox opnieuw.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - juni 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In juni 2018, hebben we deze 15 nieuwe apps met Federatie ondersteuning aan de app-galerie toegevoegd:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [vereffenen muziek](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [SAML 1.1 Token ingeschakeld LOB-App](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [ Back-ups](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh netwerken](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho één](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [ SharePoint on-premises](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [voorzien CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial), [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial). Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD-wachtwoordbeveiliging is beschikbaar in openbare preview

**Type:** Nieuwe functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Gebruikersverificatie

Azure AD-wachtwoord Protection gebruiken om u te helpen voorkomen gemakkelijk geraden wachtwoorden van uw omgeving. Deze wachtwoorden elimineren kunt u het risico van inbreuk op basis van een wachtwoord spray type aanval.

Met name kunt Azure AD-wachtwoord Protection u:

- Beveiligen van uw organisatie-accounts in zowel Azure AD en Windows Server Active Directory (AD). 
- Hiermee stopt u uw gebruikers met een wachtwoord op een lijst van de meest gebruikte wachtwoorden meer dan 500 en meer dan 1 miljoen tekens vervanging varianten van deze wachtwoorden.
- Beheren van Azure AD-wachtwoord beveiliging vanaf één locatie in de Azure AD-portal voor zowel Azure AD en on-premises Windows Server AD.

Zie voor meer informatie over Azure AD-wachtwoord Protection [onjuiste wachtwoorden in uw organisatie te elimineren](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Nieuwe "alle gasten" voorwaardelijke toegang beleidssjabloon gemaakt tijdens de voorwaarden dat gebruik wordt gemaakt

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

Tijdens het maken van de gebruiksrechtovereenkomst, wordt ook een nieuwe beleidssjabloon van voorwaardelijke toegang gemaakt voor 'alle gasten' en 'alle apps'. Deze nieuwe beleidssjabloon is van toepassing de zojuist gemaakte gebruiksvoorwaarden, stroomlijnen het maken en het afdwingen van proces voor gasten.

Zie voor meer informatie, [Azure Active Directory-voorwaarden van de functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>Nieuwe 'aangepaste' voorwaardelijk beleidssjabloon hebt gemaakt tijdens de voorwaarden dat gebruik wordt gemaakt

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Beheer

Tijdens het maken van de gebruiksrechtovereenkomst, wordt ook een nieuwe sjabloon voor 'aangepaste' voorwaardelijk beleid gemaakt. Deze nieuwe beleidssjabloon kunt u de gebruiksvoorwaarden lezen maken en onmiddellijk gaat u naar de blade voor voorwaardelijke toegang beleid voor het maken, zonder handmatig via de portal navigeren.

Zie voor meer informatie, [Azure Active Directory-voorwaarden van de functie gebruiken](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Nieuwe en uitgebreide richtlijnen over het implementeren van Azure multi-factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging
 
We hebben nieuwe Stapsgewijze instructies over het implementeren van Azure multi-factor Authentication (MFA) in uw organisatie uitgebracht.

Als u de MFA-implementatiehandleiding, gaat u naar de [identiteit implementatiehandleidingen](https://aka.ms/DeploymentPlans) op GitHub. Voor feedback over de implementatiehandleidingen, gebruikt u de [implementatie plannen feedbackformulier](https://aka.ms/deploymentplanfeedback). Hebt u vragen hebt over de implementatiehandleidingen, contact met ons op [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD gedelegeerd beheer van apps rollen in openbare preview zijn

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** Toegangsbeheer

Beheerders kunt nu beheertaken delegeren zonder de globale beheerdersrol toewijzen. De nieuwe functies en mogelijkheden zijn:

- **Nieuwe standaard Azure AD-beheerdersrollen:**

    - **Beheerder van de toepassing.** De mogelijkheid voor het beheren van alle aspecten van alle apps, met inbegrip van de registratie, SSO-instellingen, app-toewijzingen en licentieverlening, App proxy-instellingen en toestemming verleent (behalve aan Azure AD-resources).

    - **Beheerder van de cloudtoepassing.** Verleent alle van de mogelijkheden van de beheerder van de toepassing, behalve App proxy omdat het niet mogelijk om toegang tot on-premises.

    - **De ontwikkelaar van de toepassing.** Hebben de mogelijkheid om te maken van app-registraties, zelfs als de **toestaan dat gebruikers om apps te registreren** optie is uitgeschakeld.

- **Eigenaar (per app-registratie en per enterprise-app instellen, vergelijkbaar met het proces voor het eigendom van groep:**
 
    - **De eigenaar van de App-registratie.** Hebben de mogelijkheid voor het beheren van alle aspecten van die eigendom zijn app-registratie, met inbegrip van het app-manifest en meer eigenaren toe te voegen.

    - **Eigenaar van de Enterprise-App.** Hebben de mogelijkheid om u te veel aspecten van eigendom zakelijke apps, inclusief SSO-instellingen, app-toewijzingen en toestemming beheren (met uitzondering van Azure AD-resources).

Zie voor meer informatie over de openbare preview-versie, de [overgedragen Toepassingsbeheer rollen in openbare preview zijn van Azure AD.](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) blog. Zie voor meer informatie over rollen en machtigingen, [beheerdersrollen toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

---

## <a name="may-2018"></a>Mei 2018

### <a name="expressroute-support-changes"></a>Ondersteuning voor ExpressRoute-wijzigingen

**Type:** Plan voor wijziging  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Platform  

Software as a Service biedt, zoals Azure Active Directory (Azure AD) zijn ontworpen voor het beste werkt door te gaan rechtstreeks via het Internet, zonder ExpressRoute of andere persoonlijke VPN-tunnels. Als gevolg hiervan op **vanaf 1 augustus 2018**, we geen ondersteuning meer voor ExpressRoute voor Azure AD-services met behulp van openbare Azure-peering en Azure-community's in Microsoft-peering. Alle services die beïnvloed door deze wijziging mogelijk ziet u Azure AD-verkeer geleidelijk verschuiving van ExpressRoute met Internet.

Terwijl we onze ondersteuning, wijzigt ook weten we er zijn nog steeds situaties waarin u mogelijk wilt gebruiken een toegewezen set met circuits voor uw verificatieverkeer. Als gevolg hiervan blijft Azure AD voor de ondersteuning van per-tenant IP-bereikbeperkingen met behulp van ExpressRoute- en -services al op het Microsoft-peering met de community 'Andere Online Office 365-services'. Als uw services worden beïnvloed, maar u ExpressRoute nodig hebt, moet u het volgende doen:

- **Als u openbare Azure-peering.** Ga naar het Microsoft-peering en zich aanmelden voor de **andere Office 365 Online services (12076:5100)** community. Zie voor meer informatie over het verplaatsen van openbare Azure-peering voor het Microsoft-peering, de [verplaatsen van een openbare peering naar het Microsoft-peering](https://docs.microsoft.com/azure/expressroute/how-to-move-peering) artikel.

- **Als u van Microsoft-peering gebruikmaakt.** Zich aanmelden voor de **andere Office 365 Online service (12076:5100)** community. Zie voor meer informatie over de vereisten voor routering, de [ondersteuning voor BGP-community's sectie](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) van het ExpressRoute-routering vereisten-artikel.

Als u moet echter ook doorgaan met toegewezen circuits, moet u om te communiceren met uw team van Microsoft-Account over het ophalen van autorisatie gebruik van de **andere Office 365 Online service (12076:5100)** community. De MS Office beheerde controleteam controleert of u of u deze circuits moet en zorg ervoor dat u inzicht in de technische gevolgen te houden. Niet-gemachtigde abonnementen bij het maken van routefilters voor Office 365 ontvangt een foutbericht weergegeven. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph-API's voor beheerscenario's voor de gebruiksvoorwaarden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Ervaring voor ontwikkelaars
 
We hebben Microsoft Graph-API's toegevoegd voor de beheerbewerking van Azure AD gebruiksrechtovereenkomst. U zijn kunt maken, bijwerken en verwijderen van de voorwaarden van gebruik object.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Azure AD-multitenant-eindpunt toevoegen als een id-provider in Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C
 
Met behulp van aangepaste beleidsregels, kunt u de algemene Azure AD-eindpunt toevoegen als een id-provider in Azure AD B2C. Hiermee kunt u dat een single point of vermelding voor alle Azure AD-gebruikers die bij uw toepassingen aanmeldt zich. Zie voor meer informatie, [Azure Active Directory B2C: Toestaan dat gebruikers zich aanmelden met een id-provider van het Azure AD met meerdere tenants met behulp van aangepaste beleidsregels](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Interne URL's gebruikt voor toegang tot apps vanaf elke locatie met de extensie mijn Apps, aanmelding en de Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** SSO
 
Gebruikers kunnen nu toegang tot toepassingen via de interne URL's ook buiten uw bedrijfsnetwerk met behulp van de mijn Apps beveiligde aanmelding-extensie voor Azure AD. Dit werkt met elke toepassing die u hebt gepubliceerd met behulp van Azure AD-toepassingsproxy in elke browser waarop ook de Browseruitbreiding van het toegangsvenster geïnstalleerd. De functionaliteit van URL-omleiding is automatisch ingeschakeld wanneer een gebruiker meldt zich aan bij de extensie. De extensie is beschikbaar voor downloaden op [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367), en [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - gegevens in Europa voor Europa-klanten

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** GoLocal

Klanten in Europa vereisen hun gegevens om te blijven in Europa en niet gerepliceerd buiten de Europese datacenters voor vergadering privacy en de Europese wetgeving. Dit [artikel](https://go.microsoft.com/fwlink/?linkid=872328) bevat de specifieke details over welke identiteitsgegevens worden opgeslagen in Europa en bieden ook details van de gegevens die zijn opgeslagen buiten Europese datacenters. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Nieuwe gebruikers inrichten van SaaS-app integraties - mei 2018

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** 3e partij integratie
 
Azure AD kunt u het maken, onderhoud en verwijderen van gebruikers-id's in SaaS-toepassingen, zoals Dropbox, Salesforce, ServiceNow en automatiseren. Voor mei 2018, hebben we ondersteuning voor de volgende toepassingen in de galerie van Azure AD-app inrichten van gebruikers toegevoegd:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Hoeksteen OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Zie voor een lijst van alle toepassingen die ondersteuning bieden voor het inrichten van gebruikers in de Azure AD-galerie, [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Azure AD-toegangsbeoordelingen van groepen en toegang tot de app biedt nu terugkerende beoordelingen

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Beheer
 
Toegangsbeoordeling voor groepen en apps is nu algemeen beschikbaar als onderdeel van Azure AD Premium P2.  Beheerders zich toegangsbeoordelingen van groepslidmaatschappen en -toewijzingen van de toepassing automatisch wordt herhaald met regelmatige intervallen, zoals maandelijkse of driemaandelijkse configureren.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD-activiteitenlogboeken (aanmeldingen en audit) zijn nu beschikbaar zijn via MS Graph

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage
 
Azure AD activiteitenlogboeken, waaronder, aanmeldingen en auditlogboeken, zijn nu beschikbaar zijn via MS Graph. We hebben twee eindpunten via MS Graph voor toegang tot deze logboeken weergegeven. Bekijk onze [documenten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) voor programmatische toegang tot Azure AD Reporting API's aan de slag. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Verbeteringen aan het inwisselen van de B2B-ervaring en een organisatie verlaten

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

**Just-in-time-inschrijving:** Wanneer u een resource delen met een gastgebruiker met B2B-API – u hoeft niet te verzenden van een speciale uitnodiging via e-mail. In de meeste gevallen de gastgebruiker hebben toegang tot de resource en zien wat de ervaring inwisselen just-in-tijd worden uitgevoerd. Er is geen meer impact vanwege gemiste e-mailberichten. Niet meer wordt gevraagd uw gastgebruikers 'Hebt u klikken op de koppeling inwisselen die het systeem naar u verzonden?'. Dit betekent dat zodra SPO maakt gebruik van de manager van de uitnodiging – bewolkt bijlagen de dezelfde standaard-URL voor alle gebruikers, interne en externe – in elke status van inschrijving hebben kunnen.

**De ervaring voor moderne inschrijving:** Niet meer gesplitst scherm inwisselen landingspagina. Gebruikers zien een moderne toestemming geven ervaring met de privacyverklaring van de uitnodigende organisatie, net als voor apps van derden.

**Gastgebruikers kunnen de organisatie verlaten:** Zodra de relatie van een gebruiker met een organisatie uitgevoerd is, kunnen ze zelf dienen de organisatie heeft verlaten. Aanroepen niet meer van de uitnodigende organisatie admin 'verwijderd', niet meer verhogen ondersteuningstickets.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Nieuwe federatieve Apps beschikbaar in de galerie van Azure AD-app - mei 2018

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie
 
In mei 2018, hebben we deze 18 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty regelen, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [landen MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial),[まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [boog publicatie - eenmalige aanmelding ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [koppel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Nieuwe stapsgewijze implementatiehandleidingen voor Azure Active Directory

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Directory
 
Nieuwe, stapsgewijze instructies over het implementeren van Azure Active Directory (Azure AD), met inbegrip van self-service voor wachtwoord opnieuw instellen (SSPR), eenmalige aanmelding (SSO), voorwaardelijke toegang (CA), App-proxy, gebruikers inrichten, Active Directory Federation Services (ADFS) op Pass through-verificatie (PTA) en ADFS voor synchronisatie van wachtwoordhashes (WHS).

Als u de implementatiehandleidingen, gaat u naar de [identiteit implementatiehandleidingen](https://aka.ms/DeploymentPlans) op GitHub. Voor feedback over de implementatiehandleidingen, gebruikt u de [implementatie plannen feedbackformulier](https://aka.ms/deploymentplanfeedback). Hebt u vragen hebt over de implementatiehandleidingen, contact met ons op [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Zakelijke toepassingen zoeken: meer Apps laden

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO
 
Problemen met het vinden van uw toepassingen / service-principals? We hebben de mogelijkheid om te laden meer toepassingen in uw zakelijke toepassingen op de lijst met alle toepassingen toegevoegd. Standaard, laten we zien 20 toepassingen. U kunt nu op klikt, **meer laden** om aanvullende toepassingen weer te geven. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>De versie van AADConnect een openbare preview-versie van de integratie met PingFederate bevat, mei belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe geweldige nieuwe hulpprogramma's voor probleemoplossing. 

**Type:** Gewijzigde functie  
**Categorie van de service:** AD Connect  
**Product-mogelijkheid:** Beheer van Identiteitslevenscycli
 
De versie van AADConnect een openbare preview-versie van de integratie met PingFederate bevat, mei belangrijke beveiligingsupdates, veel oplossingen voor problemen en nieuwe geweldige nieuwe hulpprogramma's voor probleemoplossing. U vindt de opmerkingen bij de release [hier](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD-toegangsbeoordelingen: automatisch toepassen

**Type:** Gewijzigde functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Beheer

Toegangsbeoordelingen voor groepen en apps zijn nu algemeen beschikbaar als onderdeel van Azure AD Premium P2. Een beheerder kan configureren zodat de wijzigingen van de revisor automatisch aan die groep of de app worden toegepast als de toegangsbeoordeling is voltooid. De beheerder kan ook opgeven wat gebeurt er met blijvende toegang van de gebruiker als revisoren niet reageren, toegang verwijderen, toegang houden of systeem aanbevelingen ophalen. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>ID-tokens kunnen niet meer worden geretourneerd met behulp van de query-response_mode voor nieuwe apps. 

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Apps die zijn gemaakt op of na 25 April 2018 niet meer mogelijk om aan te vragen een **id_token** met behulp van de **query** response_mode.  Hiermee wordt Azure AD-inline met de OIDC-specificaties en helpt de kwetsbaarheid van uw apps te beperken.  Apps die zijn gemaakt voor 25 April 2018 worden niet geblokkeerd met behulp van de **query** response_mode met een response_type van **id_token**.  De fout is geretourneerd, bij het aanvragen van een id_token van AAD, is **AADSTS70007: 'query' is geen ondersteunde waarde van 'response_mode' bij het aanvragen van een token**.

De **fragment** en **form_post** response_modes blijven werken - bij het maken van het nieuwe objecten voor toepassingen (bijvoorbeeld voor App-Proxy gebruik), zorg ervoor dat gebruik van een van deze response_modes voordat ze maakt een nieuwe toepassing.  

---
 
## <a name="april-2018"></a>April 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C Access Token zijn algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C 

U hebt nu toegang tot Web-API's beveiligd door Azure AD B2C met-toegangstokens. De functie wordt verplaatst van de openbare preview-versie voor algemene beschikbaarheid. De gebruikersinterface-ervaring voor het configureren van Azure AD B2C-toepassingen en web-API's, is verbeterd en zijn andere kleine verbeteringen aangebracht.
 
Zie voor meer informatie, [Azure AD B2C: Aanvragen van toegangstokens](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Configuratie voor eenmalige aanmelding voor toepassingen op basis van SAML testen

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

Bij het configureren van SAML gebaseerde SSO-toepassingen, u kunt de integratie van op de configuratiepagina testen. Als er een fout opgetreden tijdens het aanmelden, kunt u de fout in de test-ervaring bieden en Azure AD biedt u stappen voor het oplossen van dit specifieke probleem.

Zie voor meer informatie:

- [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
- [Fouten opsporen in SAML gebaseerde eenmalige aanmelding voor toepassingen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD gebruiksrechtovereenkomst nu heeft per gebruiker melden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
Beheerders kunnen nu selecteert u een bepaalde gebruiksvoorwaarden en zien alle gebruikers die hebben ingestemd met de gebruiksvoorwaarden en wat datum/tijd het heeft plaatsgevonden.

Zie voor meer informatie de [functie van Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: Riskant IP-adres voor beveiliging met AD FS extranet accountvergrendeling 

**Type:** Nieuwe functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** Bewaking en rapportage

Connect Health nu ondersteunt de mogelijkheid voor het detecteren van IP adressen die groter zijn dan een drempel voor mislukte U/P aanmeldingen op basis van per uur of dagelijks. De mogelijkheden van deze functie zijn:

- Uitgebreide rapport weergeven met de IP-adres en het aantal mislukte aanmeldingen die zijn gegenereerd op basis van per uur/dag met aanpasbare drempelwaarde.
- Waarschuwingen op basis van e-mailbericht weergegeven wanneer een specifiek IP-adres heeft de drempelwaarde van mislukte U/P aanmeldingen op basis van per uur/dag overschreden.
- Een downloadoptie om een gedetailleerde analyse van de gegevens

Zie voor meer informatie, [rapport riskant IP-adres](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Eenvoudig app-configuratie met de van metagegevensbestand of URL

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO

Op de pagina van de Enterprise-toepassingen kunnen beheerders een SAML-metagegevensbestand als u wilt configureren op basis van SAML-aanmelding voor AAD-galerie en niet toepassing uploaden.

Bovendien kunt u URL voor federatieve metagegevens van Azure AD-toepassing voor het configureren van eenmalige aanmelding met de betreffende toepassing.

Zie voor meer informatie, [configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD gebruiksrechtovereenkomst gebruiken nu algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 

Azure AD gebruiksrechtovereenkomst hebt verplaatst uit de openbare preview-versie naar algemeen beschikbaar.

Zie voor meer informatie de [functie van Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C
 

U kunt nu opgeven welke partnerorganisaties die u wilt delen en samenwerken met in Azure AD B2B-samenwerking. Om dit te doen, kunt u lijst maken met specifieke toestaan of weigeren van domeinen. Wanneer een domein is geblokkeerd met behulp van deze mogelijkheden, kunnen werknemers niet meer uitnodigingen verzenden naar mensen in dat domein.

Zo kunt u voor het beheren van toegang tot uw resources tijdens het inschakelen van een goede ervaring voor goedgekeurde gebruikers.

Deze functie B2B-samenwerking is beschikbaar voor alle klanten van Azure Active Directory en kan worden gebruikt in combinatie met Azure AD Premium-functies, zoals voorwaardelijke toegang en identity protection voor gedetailleerdere controle over wanneer en hoe de externe zakelijke gebruikers zich aanmelden in en toegang te krijgen.

Zie voor meer informatie, [uitnodigingen toestaan of blokkeren voor B2B-gebruikers van bepaalde organisaties](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve apps beschikbaar in Azure AD app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In April 2018, hebben we deze 13 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

Criterium HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [geheim Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [dynamische signaal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [organigram Nu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Prestatiemeter](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), rek, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>GRANT B2B-gebruikers in Azure AD-toegang tot uw on-premises toepassingen (preview-versie)

**Type:** Nieuwe functie  
**Categorie van de service:** B2B  
**Product-mogelijkheid:** B2B/B2C

Als een organisatie die gebruikmaakt van functionaliteit voor samenwerking in Azure Active Directory (Azure AD) B2B uitnodigen van gastgebruikers van partnerorganisaties koppelt aan uw Azure AD, kunt u nu deze B2B-gebruikers toegang bieden tot on-premises toepassingen. Deze on-premises toepassingen kunnen verificatie op basis van SAML of geïntegreerde Windows-verificatie (IWA) gebruiken met Kerberos-beperkte delegatie (KCD).

Zie voor meer informatie, [Grant B2B-gebruikers in Azure AD de toegang tot uw on-premises toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Zelfstudies over integratie van eenmalige aanmelding ophalen uit de Azure-Marketplace

**Type:** Gewijzigde functie  
**Categorie van de service:** Overige  
**Product-mogelijkheid:** 3e partij integratie

Als een toepassing die wordt vermeld in de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) ondersteunt SAML gebaseerde eenmalige aanmelding, te klikken op **nu downloaden** biedt u de zelfstudie voor integratie van die zijn gekoppeld aan deze toepassing. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Snellere prestaties van Azure AD automatisch gebruikers inrichten voor SaaS-toepassingen

**Type:** Gewijzigde functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** 3e partij integratie
 
Voorheen klanten die gebruikmaken van de inrichting van connectors voor SaaS-toepassingen (bijvoorbeeld Salesforce, ServiceNow en vak) Azure Active Directory-gebruiker kunnen traag als hun Azure AD-tenants die meer dan 100.000 gecombineerde gebruikers en groepen, en ze zijn toewijzingen van gebruikers en groepen gebruiken om te bepalen welke gebruikers moeten worden ingericht.

Op 2 April 2018, zijn belangrijke prestatieverbeteringen geïmplementeerd op de Azure AD-inrichtingsservice die aanzienlijk verminderen de hoeveelheid tijd die nodig is om uit te voeren van de initiële synchronisatie tussen Azure Active Directory- en doel SaaS-toepassingen.

Als gevolg hiervan, veel klanten had initiële synchronisaties met apps die het aantal dagen duurde of nooit uitgevoerd, kunt u nu binnen een paar minuten of uren zijn voltooid.

Zie voor meer informatie, [wat er gebeurt tijdens het inrichten?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self-service voor wachtwoord opnieuw instellen van Windows 10-vergrendelingsscherm voor hybride Azure AD verbonden computers

**Type:** Gewijzigde functie  
**Categorie van de service:** Selfservice voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie
 
De functie voor Windows 10 SSPR nu ondersteuning voor machines die zijn toegevoegd aan hybrid Azure AD join is bijgewerkt. Deze functie is beschikbaar in Windows 10 RS4 kunnen gebruikers hun wachtwoord van het vergrendelingsscherm van een Windows 10-computer. Deze functie kunnen gebruikmaken van gebruikers die zijn ingeschakeld en geregistreerd voor selfservice voor wachtwoord opnieuw instellen.

Zie voor meer informatie, [Azure AD-wachtwoord opnieuw instellen vanuit het aanmeldingsscherm](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Maart 2018
 
### <a name="certificate-expire-notification"></a>Certificaat verloopt melding

**Type:** Vast  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO
 
Azure AD verzendt een melding wanneer een certificaat voor een galerie of toepassing buiten de galerie is bijna verlopen. 

Sommige gebruikers is niet ontvangen van meldingen voor bedrijfstoepassingen die zijn geconfigureerd voor SAML gebaseerde eenmalige aanmelding. Dit probleem is opgelost. Azure AD stuurt de melding voor certificaten dat verloopt binnen 7, 30 en 60 dagen. U bent kunnen zien van dit evenement dat in de auditlogboeken. 

Zie voor meer informatie:

- [Certificaten beheren voor federatieve eenmalige aanmelding in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Controleactiviteitenrapporten in de Azure Active Directory-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Twitter- en GitHub-id-providers in Azure AD B2C

**Type:** Nieuwe functie  
**Categorie van de service:** B2C - identiteitsbeheer van consumenten  
**Product-mogelijkheid:** B2B/B2C
 
U kunt nu Twitter of GitHub toevoegen als een id-provider in Azure AD B2C. Twitter wordt verplaatst van de openbare preview-versie voor algemene beschikbaarheid. GitHub is in openbare preview-versie wordt uitgebracht.

Zie voor meer informatie, [wat is Azure AD B2B-samenwerking?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Beperken van toegang via de browser met behulp van Intune Managed Browser met Azure AD voor voorwaardelijke toegang op basis van een toepassing voor iOS en Android

**Type:** Nieuwe functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging
 
**Nu in openbare preview!**

**Intune Managed Browser eenmalige aanmelding:** Uw werknemers kunnen eenmalige aanmelding in systeemeigen clients (zoals Microsoft Outlook) en de Intune Managed Browser gebruiken voor alle Azure AD verbonden apps.

**Intune Managed Browser-ondersteuning voor voorwaardelijke toegang:** U kunt nu vereisen dat werknemers kunnen met behulp van beleid voor voorwaardelijke toegang op basis van een toepassing Intune Managed browser gebruiken.

Meer informatie over hoe dit in onze [blogbericht](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Zie voor meer informatie:

- [Voorwaardelijke toegang op basis van een toepassing instellen](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Beheerde-browserbeleid configureren](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>App-Proxy-Cmdlets in algemene beschikbaarheid van Powershell-Module

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Toegangsbeheer
 
Ondersteuning voor toepassingsproxy-cmdlets is nu in de Powershell-Module voor algemene beschikbaarheid. Dit vereist dat u op de hoogte te blijven van Powershell-modules - als u meer dan een jaar achter, sommige cmdlets worden mogelijk niet meer werken. 

Zie voor meer informatie, [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Office 365 systeemeigen clients worden ondersteund door een naadloze eenmalige aanmelding met een niet-interactieve protocol

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Gebruiker met behulp van Office 365 systeemeigen clients (versie 16.0.8730.xxxx en hoger) ophalen van een op de achtergrond aanmelding met naadloze eenmalige aanmelding. Deze ondersteuning wordt geleverd door de toevoeging een niet-interactieve protocol (WS-Trust) naar Azure AD.

Zie voor meer informatie, [hoe aanmelding op een systeemeigen client met naadloze eenmalige aanmelding werk?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Gebruikers krijgen een op de achtergrond aanmelding, met naadloze eenmalige aanmelding, als een toepassing aanmeldingsaanvragen naar Azure AD-tenant-eindpunten verzendt

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Gebruikers krijgen een op de achtergrond aanmelding, met naadloze eenmalige aanmelding, als een toepassing (bijvoorbeeld `https://contoso.sharepoint.com`) aanmeldingsaanvragen naar eindpunten van Azure AD-tenant - dat wil zeggen, verzendt `https://login.microsoftonline.com/contoso.com/<..>` of `https://login.microsoftonline.com/<tenant_ID>/<..>` : in plaats van algemene Azure AD-eindpunt (`https://login.microsoftonline.com/common/<...>`).

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Moet slechts één Azure AD-URL, in plaats van twee URL's eerder, toevoegen aan gebruikers Intranet zone-instellingen voor de implementatie van naadloze eenmalige aanmelding

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Voor implementatie naadloze eenmalige aanmelding in uw gebruikers, moet u slechts één Azure AD-URL met het Intranet van de gebruikers van de zones met behulp van Groepsbeleid in Active Directory toevoegen: `https://autologon.microsoftazuread-sso.com`. Voorheen moesten klanten om toe te voegen twee URL's.

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in Azure AD app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In maart 2018, hebben we deze 15 nieuwe apps met Federatie ondersteuning aan onze app-galerie toegevoegd:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), -assistent door FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech digitale Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM voor Azure-Resources is algemeen beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Als u van Azure AD Privileged Identity Management voor directory-rollen gebruikmaakt, kunt u nu gebruiken de tijdelijke toegang en de mogelijkheden van de toewijzing van PIM voor Azure-Resource-rollen, zoals abonnementen, resourcegroepen, virtuele Machines en een andere bron die wordt ondersteund door Azure Resource Manager. Meervoudige verificatie afdwingen bij het activeren van rollen Just-In-Time en plannen van activeringen in combinatie met windows goedgekeurde wijzigen. Deze versie wordt bovendien niet beschikbaar tijdens de openbare preview, met inbegrip van een bijgewerkte gebruikersinterface, goedkeuringswerkstromen en de mogelijkheid om verlopen rollen vernieuwen uit te breiden rollen verloopt binnenkort verbeteringen toegevoegd.

Zie voor meer informatie, [PIM voor Azure-resources (Preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Toevoegen van optionele Claims voor uw apps-tokens (openbare preview)

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Uw Azure AD-app kunt nu aanvraag aangepaste of optioneel claims in JWTs of SAML-tokens.  Dit zijn de claims over de gebruiker of de tenant die niet zijn opgenomen in het token wordt vanwege beperkingen grootte of toepasselijkheid standaard.  Dit is momenteel in openbare preview voor Azure AD-apps op de v1.0 en v2.0-eindpunten.  Zie de documentatie voor meer informatie over welke claims kunnen worden toegevoegd en hoe u uw toepassingsmanifest om aan te vragen deze te bewerken.  

Zie voor meer informatie, [optioneel claims in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD biedt ondersteuning voor PKCE voor veiliger OAuth-stromen

**Type:** Nieuwe functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Azure AD-documenten zijn bijgewerkt voor ondersteuning voor PKCE, waardoor meer beveiligde communicatie tijdens de stroom voor het verlenen van OAuth 2.0-autorisatiecode opmerking.  Zowel S256 als tekst zonder opmaak code_challenges worden ondersteund op de v1.0 en v2.0-eindpunten. 

Zie voor meer informatie, [aanvragen van een autorisatiecode](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Ondersteuning voor het inrichten van alle gebruiker kenmerkwaarden beschikbaar in de Workday Get_Workers-API

**Type:** Nieuwe functie  
**Categorie van de service:** App-inrichting  
**Product-mogelijkheid:** 3e partij integratie
 
De openbare preview van inkomende inrichten van Workday naar Active Directory en Azure AD nu ondersteunt de mogelijkheid om op te halen en de inrichting van alle waarden van kenmerken beschikbaar in de Workday Get_Workers-API. Dit biedt ondersteuning voor honderden extra standard wordt toegevoegd en aangepaste kenmerken die verzonden met de eerste versie van de Workday inbound provisioning connector.

Zie voor meer informatie: [De lijst met gebruikerskenmerken Workday aanpassen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Groepslidmaatschap wijzigen van dynamisch naar statisch, en vice versa

**Type:** Nieuwe functie  
**Categorie van de service:** Groepsbeheer  
**Product-mogelijkheid:** Samenwerking
 
Het is mogelijk om te wijzigen hoe lidmaatschap in een groep wordt beheerd. Dit is handig als u behouden van de dezelfde naam en de ID in het systeem wilt, zodat alle bestaande verwijzingen naar de groep nog steeds geldig zijn. het maken van een nieuwe groep zorgde deze verwijzingen worden bijgewerkt.
Het Azure AD-beheercentrum ter ondersteuning van deze functionaliteit is bijgewerkt. Klanten kunnen bestaande groepen nu converteren van een dynamisch lidmaatschap toegewezen lidmaatschap en vice versa. De bestaande PowerShell-cmdlets zijn ook nog steeds beschikbaar.

Zie voor meer informatie, [dynamisch-lidmaatschapregels voor groepen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Verbeterde afmelding gedrag met naadloze eenmalige aanmelding

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie
 
Eerder, zelfs als gebruikers wordt expliciet afgemeld bij een toepassing die wordt beveiligd door Azure AD, ze zou worden automatisch aangemeld terug met behulp van naadloze eenmalige aanmelding als ze toegang probeert te krijgen van een Azure AD-toepassing opnieuw binnen het bedrijfsnetwerk vanaf hun apparaten toegevoegd aan een domein. Met deze wijziging, wordt Meld u af ondersteund.  Hiermee kunnen gebruikers kiezen voor de dezelfde of verschillende Azure AD-account aan te melden in met, in plaats van dat automatisch wordt aangemeld bij het gebruik van naadloze eenmalige aanmelding.

Zie voor meer informatie, [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Application Proxy Connector versie 1.5.402.0 die zijn uitgebracht

**Type:** Gewijzigde functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging
 
De versie van deze connector wordt geleidelijk tot en met November wordt geïmplementeerd. Deze nieuwe versie van de connector bevat de volgende wijzigingen:

- De connector nu stelt domein niveau cookies in plaats daarvan subdomein niveau. Dit zorgt ervoor dat een soepeler SSO-ervaring en voorkomt u redundante verificatie wordt gevraagd.
- Ondersteuning voor gesegmenteerde codering aanvragen
- Verbeterde connector statuscontrole 
- Verschillende oplossingen voor problemen en stabiliteitsverbeteringen

Zie voor meer informatie, [over Azure AD Application Proxy connectors](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Februari 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Verbeterde navigatie voor het beheren van gebruikers en groepen

**Type:** Plan voor wijziging  
**Categorie van de service:** Mapbeheer  
**Product-mogelijkheid:** Directory

De navigatie-ervaring voor het beheren van gebruikers en groepen is gestroomlijnd. U kunt nu rechtstreeks aan de lijst met alle gebruikers, met eenvoudiger toegang tot de lijst met verwijderde gebruikers uit de directory-overzicht navigeren. U kunt ook rechtstreeks aan de lijst met alle groepen, eenvoudiger toegang tot de beheerinstellingen van de groep uit de directory-overzicht navigeren. En ook de overzichtspagina voor de directory, u kunt zoeken naar een gebruiker, groep, bedrijfstoepassing of app-registratie. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Beschikbaarheid van aanmeldingen en audit rapporten in Microsoft Azure uitgevoerd door 21Vianet (Azure China 21Vianet)

**Type:** Nieuwe functie  
**Categorie van de service:** Azure Stack  
**Product-mogelijkheid:** Bewaking en rapportage

Azure AD-activiteit log-rapporten zijn nu beschikbaar in Microsoft Azure, bediend door 21Vianet (Azure China 21Vianet) exemplaren. De volgende logboeken zijn opgenomen:

- **Activiteitenlogboeken aanmeldingen** -omvat alle aanmeldingen logboeken die zijn gekoppeld aan uw tenant.

- **Selfservice voor wachtwoord controleren logboeken** -omvat alle de SSPR-auditlogboeken.

- **Controle van Directory-beheer registreert** -omvat alle logboeken voor directory audit-gerelateerde zoals gebruiker management, App-beheer en anderen.

Met deze logboeken krijgt u inzicht in hoe uw omgeving presteert. Met de gegevens kunt u het volgende doen:

- Bepalen hoe uw apps en services worden gebruikt door uw gebruikers.

- Oplossen van problemen te voorkomen dat uw gebruikers van het ontvangen van hun werk afmaken.

Zie voor meer informatie over het gebruik van deze rapporten [Azure Active Directory-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>'Rapportlezer'-rol (niet-beheerdersrol) gebruiken om Azure AD-activiteitenrapporten weer te geven

**Type:** Nieuwe functie  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Bewaking en rapportage

Als onderdeel van feedback van klanten om in te schakelen van niet-beheerdersrollen toegang hebben tot Azure AD-activiteit zich aanmeldt, is voorzien van de mogelijkheid voor gebruikers die zich in de rol "Lezer rapport" voor toegang tot aanmeldingen en Audit-activiteit in de Azure-portal, evenals met onze Graph API's. 

Voor meer informatie over het gebruik van deze rapporten, [Azure Active Directory-rapportage](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>EmployeeID-claim die beschikbaar zijn als gebruikerskenmerk en gebruikers-id

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** SSO
 
U kunt configureren **EmployeeID** als de gebruikers-id en het gebruikerskenmerk voor lidgebruikers en B2B-gasten in SAML gebaseerde aanmelding toepassingen vanuit de gebruikersinterface van de Enterprise-toepassing.

Zie voor meer informatie, [in het SAML-token voor bedrijfstoepassingen in Azure Active Directory wordt uitgegeven claims aanpassen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Vereenvoudigd Toepassingsbeheer met jokertekens in Azure AD-toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Gebruikersverificatie
 
Implementatie van toepassingen te vereenvoudigen en reduceren uw administratieve overhead, ondersteunen we nu de mogelijkheid voor het publiceren van toepassingen die gebruikmaken van jokertekens. U kunt voor het publiceren van een jokertekentoepassing, volgt u de standard-toepassing publiceren stroom maar een jokerteken gebruiken in de interne en externe URL's.

Zie voor meer informatie, [Wildcard-toepassingen in de Azure Active Directory-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Er zijn nieuwe cmdlets voor de ondersteuning van de configuratie van de toepassingsproxy

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Platform

De nieuwste versie van de Preview van AzureAD PowerShell-module bevat nieuwe cmdlets waarmee klanten het configureren van Application Proxy-toepassingen met behulp van PowerShell.

De nieuwe-cmdlets zijn: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Er zijn nieuwe cmdlets voor de ondersteuning van de configuratie van groepen

**Type:** Nieuwe functie  
**Categorie van de service:** App-Proxy  
**Product-mogelijkheid:** Platform

De nieuwste versie van de AzureAD PowerShell-module bevat cmdlets voor het beheren van groepen in Azure AD. Deze cmdlets waren eerder beschikbaar in de AzureADPreview-module en zijn nu toegevoegd aan de module AzureAD

De groep-cmdlets die nu release voor algemene beschikbaarheid zijn zijn: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Er is een nieuwe versie van Azure AD Connect beschikbaar

**Type:** Nieuwe functie  
**Categorie van de service:** AD synchroniseren  
**Product-mogelijkheid:** Platform
 
Azure AD Connect is de gewenste hulpprogramma voor het synchroniseren van gegevens tussen Azure AD en lokale gegevensbronnen, met inbegrip van Windows Server Active Directory en LDAP.

>[!Important]
>Deze versie introduceert schema en de synchronisatie regel wijzigingen. De Azure AD Connect-synchronisatieservice wordt een volledige importeerbewerking en een volledige synchronisatie stappen geactiveerd na een upgrade. Zie voor meer informatie over het wijzigen van dit gedrag [het uitstellen van volledige synchronisatie na de upgrade](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Deze release bevat de volgende updates en wijzigingen:

**Opgeloste problemen**

- Los tijdvenster op achtergrondtaken voor pagina partities filteren wanneer u overschakelt naar de volgende pagina.

- Een opgelost waardoor toegangsfout tijdens de aangepaste actie ConfigDB.

- Een opgelost om te herstellen van de time-out van de sql-verbinding.

- Een bug opgelost waarbij certificaten met jokertekens SAN controle van vereisten mislukt.

- Een opgelost die ervoor zorgt miiserver.exe crashes tijdens het exporteren van AAD-connector dat.

- Een bug opgelost waar een onjuist wachtwoord poging uitgevoerd veroorzaakt de AAD DC aangemeld verbinding maken met de wizard configuratie te wijzigen

**Nieuwe functies en verbeteringen**
 
- Toepassingstelemetrie - beheerders kunnen overschakelen naar deze klasse van gegevens in-of uitschakelen.

- Statusgegevens van Azure AD - beheerders moeten Ga naar de health-portal voor het beheren van de health-instellingen. Zodra de service-beleid is gewijzigd, wordt de agents lezen en deze toepassen.

- Apparaat terugschrijven Configuratieacties en een voortgangsbalk voor de initialisatie van de pagina toegevoegd.

- Verbeterde algemene diagnostische gegevens met HTML-rapport en de volledige gegevens verzamelen in een ZIP-tekst / HTML-rapport.

- Verbeterde betrouwbaarheid van Automatische upgrade en extra telemetrie om te controleren of de status van de server kan worden bepaald toegevoegd.

- Machtigingen die beschikbaar zijn beperkt tot beschermde accounts in AD Connector-account. Voor nieuwe installaties van de wizard beperkt de machtigingen die accounts met bevoegdheden hebben op de MSOL-account na het maken van de MSOL-account. De wijzigingen die invloed hebben op installaties van snelle en aangepaste installaties met account automatisch maken.

- Het installatieprogramma niet verplicht SA-bevoegdheden op een schone installatie van AADConnect gewijzigd.

- Nieuwe hulpprogramma voor het oplossen van synchronisatieproblemen met voor een specifiek object. Op dit moment controleert het hulpprogramma voor de volgende zaken:

    - UserPrincipalName komt niet overeen tussen gesynchroniseerde gebruikersobject en het gebruikersaccount in Azure AD-Tenant.
  
    - Als het object is uitgesloten voor synchronisatie vanwege domein filteren
  
    - Als het object is uitgesloten voor synchronisatie vanwege een organisatie-eenheid (OE) filteren

- Nieuwe hulpprogramma voor het synchroniseren van de huidige wachtwoord-hash die zijn opgeslagen in de on-premises Active Directory voor een specifiek gebruikersaccount. Het hulpprogramma is niet vereist voor een wachtwoordwijziging. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Toepassingen die ondersteuning van Intune App Protection-beleid toegevoegd voor gebruik met Azure AD-toepassing gebaseerde voorwaardelijke toegang

**Type:** Gewijzigde functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

We hebt meer toepassingen die ondersteuning bieden voor voorwaardelijke toegang op basis van een toepassing toegevoegd. Nu krijgt u toegang tot Office 365 en andere Azure AD verbonden cloudapps met behulp van deze goedgekeurde client-apps.

De volgende toepassingen worden toegevoegd aan het einde van februari:

- Microsoft Power BI

- Microsoft Launcher

- Facturering van Microsoft

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Voorwaarden van de update gebruiken voor de mobiele ervaring 

**Type:** Gewijzigde functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving

Wanneer de gebruiksvoorwaarden worden weergegeven, kunt u nu klikken **hebt u problemen met weergeven? Klik hier**. Op deze koppeling te klikken, opent de gebruiksvoorwaarden zelf op uw apparaat. Ongeacht de tekengrootte in het document of het schermformaat van het apparaat, kunt u inzoomen en het document lezen. 

---
 
## <a name="january-2018"></a>Januari 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nieuwe federatieve Apps beschikbaar in Azure AD app-galerie 

**Type:** Nieuwe functie  
**Categorie van de service:** Zakelijke Apps  
**Product-mogelijkheid:** 3e partij integratie

In januari 2018 zijn de volgende nieuwe apps met ondersteuning voor identiteitsfederatie in de app-galerie toegevoegd:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy beheersoftware](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Directory, Gefedereerd en [kwaliteit NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Meld u aan met extra risico gedetecteerd

**Type:** Nieuwe functie  
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** Identiteitbeveiliging en -beveiliging

Het inzicht u voor een gedetecteerde risicogebeurtenis krijgt is gekoppeld aan uw Azure AD-abonnement. Met de Azure AD Premium P2-editie krijgt u de meest gedetailleerde informatie over alle onderliggende detecties.

Met de Azure AD Premium P1-editie worden detecties die niet bij uw licentie inbegrepen zijn weergegeven als de risicogebeurtenis aanmelden met extra risico gedetecteerd.

Zie [Risicogebeurtenissen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) voor meer informatie.
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Office 365-toepassingen uit de toegangsdeelvensters van de eindgebruiker verbergen

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** SSO

U kunt nu beter beheren hoe Office 365-toepassingen weergegeven op de toegangsdeelvensters van uw gebruikers via een nieuwe gebruikersinstelling. Deze optie is nuttig voor het verminderen van het aantal apps in de toegangsdeelvensters van een gebruiker als u liever alleen Office-apps weergeven in de Office-portal. De instelling bevindt zich in de **gebruikersinstellingen** en is met het label, **gebruikers zien alleen Office 365-apps in de Office 365-beheerportal**.

Zie voor meer informatie, [een toepassing gebruikerservaring in Azure Active Directory verbergen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Naadloze Meld u aan bij de apps die zijn ingeschakeld voor eenmalige aanmelding wachtwoord rechtstreeks vanuit de URL van app 

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** SSO

De Apps in mijn browser-extensie is nu beschikbaar via een handig hulpmiddel waarmee u beschikt over de mijn Apps van eenmalige aanmelding over mogelijkheid als snelkoppeling in uw browser. Na het installeren ziet van de gebruiker een pictogram wafel in hun browser waarmee ze snel toegang tot apps. Gebruikers kunnen nu profiteren van:

- De mogelijkheid rechtstreeks aanmelden bij wachtwoord-eenmalige aanmelding op basis van apps van de aanmeldingspagina van de app
- Een app met behulp van de functie snelle zoekactie starten
- Snelkoppelingen naar onlangs gebruikte apps van de extensie
- De extensie is beschikbaar voor Microsoft Edge, Chrome en Firefox.
 
Zie voor meer informatie, [mijn Apps beveiligde aanmelding extensie](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Beheer van Azure AD-ervaring in de klassieke Azure Portal is buiten gebruik gesteld

**Type:** Afgeschaft   
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Directory

Vanaf 8 januari 2018, het beheer van Azure AD-ervaring in de klassieke Azure portal is buiten gebruik gesteld. Dit heeft plaatsgevonden in combinatie met het buiten gebruik stellen van de klassieke Azure portal zelf. In de toekomst, moet u de [Azure AD-beheercentrum](https://aad.portal.azure.com) voor alle uw portal-gebaseerd beheer van Azure AD.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>De PhoneFactor-web-portal is buiten gebruik gesteld

**Type:** Afgeschaft  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Directory
 
Vanaf 8 januari 2018 de PhoneFactor-web-portal is buiten gebruik gesteld. Deze portal is gebruikt voor het beheer van MFA-server, maar deze functies zijn verplaatst naar de Azure-portal op portal.azure.com. 

De configuratie van MFA bevindt zich in: **Azure Active Directory \> MFA Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Afschaffen van Azure AD-rapporten

**Type:** Afgeschaft  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Beheer van Identiteitslevenscycli  


Met de algemene beschikbaarheid van de nieuwe Azure Active Directory Administration console en de nieuwe API's nu beschikbaar voor zowel de activiteit en de beveiliging van rapporten, het rapport API's onder '/ rapporten'-eindpunt is buiten gebruik gesteld vanaf het einde van 31 December 2017.

**Wat is er beschikbaar?**

Als onderdeel van de overgang naar de nieuwe beheerconsole, 2 nieuwe API's beschikbaar gemaakt voor het ophalen van Azure AD-activiteitenlogboeken. De nieuwe set API's biedt uitgebreidere filteren en sorteren van functionaliteit naast het leveren van uitgebreide controle- en aanmeldingsactiviteiten. De gegevens die eerder beschikbaar tot en met de beveiligingsrapporten kan nu worden geopend via de Identity Protection risicogebeurtenissen API in Microsoft Graph.

Zie voor meer informatie:

- [Aan de slag met de Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>December 2017

### <a name="terms-of-use-in-the-access-panel"></a>Gebruiksvoorwaarden in het toegangsvenster

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
U kunt nu gaat u naar het toegangsvenster en weergeven van de gebruiksvoorwaarden die u eerder hebt geaccepteerd.

Volg deze stappen:

1. Ga naar de [MyApps-portal](https://myapps.microsoft.com), en meld u aan.

2. Selecteer uw naam in de rechterbovenhoek en selecteer vervolgens **profiel** in de lijst. 

3. Op uw **profiel**, selecteer **gebruiksvoorwaarden controleren**. 

4. Nu kunt u de gebruiksvoorwaarden bekijken die u hebt geaccepteerd. 

Zie voor meer informatie de [Azure AD gebruiksrechtovereenkomst functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Nieuwe Azure Active Directory-aanmeldingservaring

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie
 
De Azure AD en Microsoft-account-identiteitssysteem gebruikersinterfaces zijn opnieuw ontworpen zodat ze beschikken over een consistent uiterlijk geven. Bovendien verzamelt de Azure AD-aanmeldingspagina de naam van de gebruiker eerst, gevolgd door de referentie in een tweede scherm.

Zie voor meer informatie, [de nieuwe Azure Active Directory-aanmeldingservaring is nu in openbare preview-versie](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Minder aanmeldingsprompts: Een nieuwe "aangemeld blijven'-ervaring voor aanmelding bij Azure AD

**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** Gebruikersverificatie
 
De **aangemeld blijven** selectievakje op de aanmeldingspagina van Azure AD is vervangen door een nieuwe opdrachtprompt die wordt weergegeven nadat u kunnen verifiëren. 

Als u reageren **Ja** deze prompt, de service biedt u een permanente vernieuwingstoken. Dit gedrag is hetzelfde als wanneer u hebt geselecteerd de **aangemeld blijven** het selectievakje in de oude ervaring. Voor federatieve tenants ziet dit bericht nadat u met de federatieve service verifiëren.

Zie voor meer informatie, [minder prompts voor aanmelden: De nieuwe 'aangemeld blijven'-ervaring voor Azure AD is beschikbaar als preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Configuratie om te vereisen dat de gebruiksvoorwaarden worden uitgebreid alvorens deze te accepteren toevoegen

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving
 
Een optie voor beheerders moeten hun gebruikers om uit te breiden de gebruiksvoorwaarden voorafgaand aan de voorwaarden te accepteren.

Selecteer een **op** of **uit** gebruikers moeten de gebruiksvoorwaarden uitbreiden. De **op** instelling verplicht gebruikers om weer te geven van de gebruiksvoorwaarden alvorens deze te accepteren.

Zie voor meer informatie de [Azure AD gebruiksrechtovereenkomst functie gebruiken (preview)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Scoped activering voor in aanmerking komende roltoewijzingen

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Roltoewijzingen in aanmerking komende Azure-resource met minder autonomie dan de oorspronkelijke toewijzing standaardwaarden activeren kunt u binnen het bereik activering. Een voorbeeld is als u als eigenaar van een abonnement in uw tenant toegewezen bent. Met een bereik-activering, kunt u de rol van eigenaar voor maximaal vijf resources die zich in het abonnement (zoals resourcegroepen en virtuele machines) activeren. Bereik van de activering kan de mogelijkheid van de uitvoering van ongewenste wijzigingen om essentiële Azure-resources te beperken.

Zie voor meer informatie, [wat is Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Nieuwe federatieve apps in de Azure AD-app-galerie

**Type:** Nieuwe functie  
**Categorie van de service:** Enterprise-apps  
**Product-mogelijkheid:** 3e partij integratie

In December 2017, hebben we de ondersteuning van deze nieuwe apps met Federatie naar onze app-galerie toegevoegd:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager [EFI digitale winkel](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [INSTALLATIEKOPIE werkt](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron Azure AD-integratie](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO voor Bamboe resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO voor Bitbucket resolutie GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD-integratie.

Zie voor meer informatie over de apps [SaaS-toepassing-integratie met Azure Active Directory](https://aka.ms/appstutorial).

Zie voor meer informatie over het aanbieden van uw toepassing in de Azure AD-app-galerie [uw toepassing weergeven in de Azure Active Directory-toepassingsgalerie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Goedkeuringswerkstromen voor Azure AD-maprollen

**Type:** Gewijzigde functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Werkstroom voor goedkeuring voor Azure AD-maprollen is algemeen beschikbaar.

Met de werkstroom voor goedkeuring, bevoegde rol beheerders kunnen vereisen dat leden van een in aanmerking komende-rol op aanvraag voor rolactivering voordat ze de bevoorrechte rol kunnen gebruiken. Meerdere gebruikers en groepen kunnen worden gedelegeerd goedkeuring verantwoordelijkheden. Leden van een in aanmerking komende rol ontvangen meldingen wanneer goedkeuring is voltooid en hun rol actief is.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Pass through-verificatie: Skype voor bedrijven-ondersteuning

**Type:** Gewijzigde functie  
**Categorie van de service:** Verificaties (aanmeldingen)  
**Product-mogelijkheid:** Gebruikersverificatie

Nu Pass through-verificatie ondersteunt gebruikersaanmeldingen tot Skype voor bedrijven-clienttoepassingen die ondersteuning bieden voor moderne verificatie, waaronder online en hybride topologieën. 

Zie voor meer informatie, [Skype voor bedrijven-topologieën met moderne verificatie ondersteund](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Updates voor Azure AD Privileged Identity Management voor Azure RBAC (preview)

**Type:** Gewijzigde functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management
 
Met het vernieuwen van de openbare preview van Azure AD Privileged Identity Management (PIM) voor op rollen gebaseerd toegangsbeheer (RBAC) kunt u nu:

* Just Enough Administration gebruiken.
* Goedkeuring van de resource-rollen activeren.
* Plan een toekomstige activering van een rol die moet worden goedgekeurd voor beide Azure AD en Azure RBAC-rollen.
 
Zie voor meer informatie, [Privileged Identity Management voor Azure-resources (preview)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>November 2017
 
### <a name="access-control-service-retirement"></a>Access Control service buiten gebruik stellen

**Type:** Plan voor wijziging  
**Categorie van de service:** Access Control-service  
**Product-mogelijkheid:** Access Control-service 

Azure Active Directory Access Control (ook wel bekend als de Access Control service) wordt in latere 2018 beëindigd. Meer informatie, waaronder gedetailleerde plan en hulp bij de migratie op hoog niveau zijn, beschikbaar in de komende weken. U kunt opmerkingen achterlaten op deze pagina met vragen over de Access Control-service en een teamlid zal deze beantwoorden.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Browsertoegang beperken tot de Intune Managed Browser 

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

U kunt browsertoegang tot Office 365 en andere Azure AD verbonden cloud-apps beperken met behulp van de Intune Managed Browser als een goedgekeurde app. 

U kunt nu de volgende voorwaarde voor voorwaardelijke toegang op basis van een toepassing configureren:

**Client-apps:** Browser

**Wat is het effect van de wijziging?**

Vandaag de dag toegang geblokkeerd wanneer u deze voorwaarde gebruiken. Wanneer de Preview-versie beschikbaar is, wordt alle toegang tot het gebruik van de toepassing van de beheerde browser vereist. 

Zoeken naar deze mogelijkheid en meer informatie in toekomstige blogs en release-opmerkingen. 

Zie voor meer informatie, [voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor Azure AD op Apps gebaseerde voorwaardelijke toegang

**Type:** Plan voor wijziging  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn in de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Gebruiksvoorwaarden van ondersteuning voor meerdere talen

**Type:** Nieuwe functie    
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving

Beheerders kunnen nu nieuwe gebruiksvoorwaarden die meerdere PDF-documenten bevatten maken. U kunt deze PDF-documenten met een bijbehorende taal kunt labelen. Gebruikers worden weergegeven van het PDF-bestand met de overeenkomende taal op basis van hun voorkeuren. Als er geen overeenkomst is, wordt de standaardtaal weergegeven.

---
 
### <a name="real-time-password-writeback-client-status"></a>Status van de client realtime wachtwoord terugschrijven

**Type:** Nieuwe functie  
**Categorie van de service:** Self-service voor wachtwoord opnieuw instellen  
**Product-mogelijkheid:** Gebruikersverificatie

U kunt nu de status van uw on-premises wachtwoord-Write-backclient bekijken. Deze optie is beschikbaar in de **On-premises integratie** sectie van de [wachtwoordherstel](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) pagina. 

Als er problemen met de verbinding met uw on-premises Write-backclient zijn, ziet u een foutmelding krijgen dat beschikt u over:

- Informatie over waarom u geen verbinding met uw on-premises Write-backclient.
- Een koppeling naar de documentatie die u helpt bij het oplossen van het probleem. 

Zie voor meer informatie, [on-premises integratie](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Azure AD op Apps gebaseerde voorwaardelijke toegang 
 
**Type:** Nieuwe functie  
**Categorie van de service:** Azure AD  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

U kunt toegang nu beperken tot Office 365 en andere Azure AD verbonden cloud-apps naar [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) die ondersteuning bieden voor Intune beveiligingsbeleid voor apps met behulp van [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Intune beveiligingsbeleid voor apps worden gebruikt voor het configureren en beveiligen van bedrijfsgegevens op deze clienttoepassingen.

Door te combineren [op basis van app](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) met [apparaatgebaseerde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) beleid voor voorwaardelijke toegang hebt u de flexibiliteit om gegevens voor persoonlijke en bedrijfsapparaten te beveiligen.

De volgende voorwaarden en besturingselementen zijn nu beschikbaar voor gebruik met Apps gebaseerde voorwaardelijke toegang:

**Ondersteund platform voorwaarde**

- iOS
- Android

**Voorwaarde voor client-apps**

- Mobiele apps en bureaubladclients

**Toegangsbeheer**

- Goedgekeurde client-apps vereisen

Zie voor meer informatie, [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Azure AD-apparaten in de Azure-portal beheren

**Type:** Nieuwe functie  
**Categorie van de service:** Device Registration service en beheer  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

Nu kunt u uw apparaten die zijn verbonden met Azure AD en de activiteiten met betrekking tot apparaat op één plek. Er is een nieuwe beheerervaring voor het beheren van uw apparaat-id's en instellingen in de Azure portal. In deze release kunt u het volgende doen:

- Bekijk al uw apparaten die beschikbaar voor voorwaardelijke toegang in Azure AD zijn.
- Eigenschappen, waaronder uw hybride Azure AD gekoppelde apparaten.
- BitLocker-sleutels voor uw Azure AD join-apparaten zoeken, beheren van uw apparaat met Intune en meer.
- Apparaat met betrekking tot Azure AD-instellingen beheren.

Zie voor meer informatie, [apparaten beheren met behulp van de Azure-portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Ondersteuning voor macOS als een platform voor apparaten voor Azure AD voor voorwaardelijke toegang 

**Type:** Nieuwe functie    
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming 

U kunt nu opnemen (of uitsluiten) macOS als een voorwaarde voor het platform van apparaat in het beleid van uw Azure AD voor voorwaardelijke toegang. Met de toevoeging van macOS om de ondersteunde apparaatplatformen, kunt u het volgende doen:

- **Registreren en macOS-apparaten beheren met behulp van Intune.** Net als bij andere platformen, zoals iOS en Android, een bedrijf portal toepassing beschikbaar is voor macOS geïntegreerde inschrijvingen doen. De nieuwe bedrijfsportal-app voor macOS kunt u een apparaat met Intune inschrijven en Registreer het bij Azure AD.
- **Zorg ervoor dat op macOS-apparaten voldoen aan uw organisatie-nalevingsbeleid dat is gedefinieerd in Intune.** In Intune in Azure portal kunt kunt u nu instellen nalevingsbeleid voor macOS-apparaten. 
- **Toegang tot toepassingen in Azure AD om alleen compatibele macOS-apparaten te beperken.** Beleid voor voorwaardelijke toegang maken heeft macOS als een afzonderlijk apparaat platform-optie. Nu kunt u macOS-specifieke voorwaardelijk toegangsbeleid voor de doeltoepassing is ingesteld in Azure maken.

Zie voor meer informatie:

- [Een apparaatnalevingsbeleid maken voor macOS-apparaten in Intune](https://aka.ms/macoscompliancepolicy)
- [Voorwaardelijke toegang in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Network Policy Server-extensie voor Azure multi-factor Authentication 

**Type:** Nieuwe functie    
**Categorie van de service:**  Multi-Factor Authentication  
**Product-mogelijkheid:** Gebruikersverificatie

De Network Policy Server-extensie voor Azure multi-factor Authentication wordt cloud-gebaseerde multi-factor Authentication-mogelijkheden toegevoegd aan uw infrastructuur voor verificatie met behulp van uw bestaande servers. Met de extensie voor Network Policy Server, kunt u telefonische oproepen, SMS-bericht of verificatie via de telefoon-app kunt toevoegen aan uw bestaande verificatiestroom. U hoeft niet te installeren, configureren en onderhouden van nieuwe servers. 

Deze extensie is bedoeld voor organisaties die verbindingen virtual private network beveiligen willen zonder de Azure multi-factor Authentication-Server te implementeren. De Network Policy Server-extensie als een adapter tussen RADIUS- en cloud-gebaseerde Azure multi-factor Authentication fungeert voor een tweede factor-verificatie voor federatieve of gesynchroniseerde gebruikers.

Zie voor meer informatie, [uw bestaande infrastructuur van de Network Policy Server integreren met Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Herstellen of verwijderde gebruikers permanent verwijderen

**Type:** Nieuwe functie    
**Categorie van de service:** Gebruikersbeheer  
**Product-mogelijkheid:** Directory 

In het beheercentrum van Azure AD kunt u nu:

- Een verwijderde gebruiker herstellen. 
- Een gebruiker permanent verwijdert.

**Om het te proberen:**

1. Selecteer in het Azure AD-beheercentrum [alle gebruikers](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) in de **beheren** sectie. 

2. Uit de **weergeven** in de lijst met **onlangs verwijderde gebruikers**. 

3. Selecteer een of meer onlangs verwijderde gebruikers, en vervolgens ofwel herstellen of deze definitief verwijderen.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nieuwe goedgekeurde client-apps voor Azure AD op Apps gebaseerde voorwaardelijke toegang
 
**Type:** Gewijzigde functie  
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

De volgende apps zijn toegevoegd aan de lijst met [goedgekeurde client-apps](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 

Zie voor meer informatie:

- [Vereiste voor goedgekeurde client-app](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD op Apps gebaseerde voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Gebruik 'Of' tussen de besturingselementen in een beleid voor voorwaardelijke toegang 

**Type:** Gewijzigde functie    
**Categorie van de service:** Voorwaardelijke toegang  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming
 
Nu kunt u gebruiken ' of ' (een van de geselecteerde besturingselementen vereisen) voor voor voorwaardelijke toegang besturingselementen. U kunt deze functie gebruiken om beleid te maken met 'of' tussen de besturingselementen voor toegang. Bijvoorbeeld, kunt u deze functie om een beleid waarvoor een gebruiker zich aanmeldt met behulp van multi-factor Authentication 'of' op een compatibel apparaat te maken.

Zie voor meer informatie, [besturingselementen in Azure AD voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Aggregatie van realtime risicogebeurtenissen

**Type:** Gewijzigde functie    
**Categorie van de service:** Identiteitsbeveiliging  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming

In Azure AD Identity Protection worden nu alle realtime risicogebeurtenissen die afkomstig van hetzelfde IP-adres op een bepaalde dag zijn samengevoegd voor elk type risicogebeurtenis. Deze wijziging beperkt het volume van de risicogebeurtenissen die worden weergegeven zonder wijziging in de gebruikersbeveiliging.

De onderliggende realtime detectie werkt telkens wanneer de gebruiker zich aanmeldt. Als u een aanmeldingsrisico beveiligingsbeleid instellen voor multi-factor Authentication of de toegang blokkeert hebt, is het nog steeds geactiveerd tijdens elke riskante aanmelding.
 
---
 
## <a name="october-2017"></a>Oktober 2017

### <a name="deprecate-azure-ad-reports"></a>Afschaffen van Azure AD-rapporten

**Type:** Plan voor wijziging  
**Categorie van de service:** Rapportage  
**Product-mogelijkheid:** Beheer van Identiteitslevenscycli  

De Azure-portal beschikt u over:

- Een nieuwe Azure AD-beheerconsole.
- Nieuwe API's voor rapporten van activiteit en beveiliging.
 
Vanwege deze nieuwe mogelijkheden, het rapport API's onder het eindpunt/Reports zijn buiten gebruik gesteld op 10 December 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detectie van het veld voor automatische aanmelding

**Type:** Vast   
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** Eenmalige aanmelding  

Azure AD biedt ondersteuning voor detectie van het veld voor automatische aanmelding voor toepassingen die een gebruiker HTML-veld naam en het wachtwoord weergeven. Deze stappen worden beschreven in [automatisch vastleggen aanmeldingsvelden voor een toepassing](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). U vindt deze mogelijkheid door toe te voegen een *niet in de galerij* toepassing voor de **bedrijfstoepassingen** pagina in de [Azure-portal](https://aad.portal.azure.com). Bovendien kunt u de **Single Sign-on** modus op deze nieuwe toepassing **wachtwoord gebaseerde Single Sign-on**, Geef een web-URL en sla vervolgens op de pagina.
 
Deze functionaliteit is vanwege een serviceprobleem tijdelijk uitgeschakeld. Het probleem is opgelost en de detectie van het veld voor automatische aanmelding weer beschikbaar is.

---

### <a name="new-multi-factor-authentication-features"></a>Nieuwe functies van multi-factor Authentication

**Type:** Nieuwe functie  
**Categorie van de service:** Multi-Factor Authentication  
**Product-mogelijkheid:** , Identiteitbeveiliging en bescherming  

Multi-factor authentication (MFA) is een essentieel onderdeel van uw organisatie te beschermen. Als u meer geavanceerde referenties en de ervaring biedt een naadloze ervaring, zijn de volgende functies toegevoegd: 

- Resultaten van de basis van meerdere factoren uitdaging zijn rechtstreeks geïntegreerd in het Azure AD aanmelden rapport, waaronder programmatische toegang tot de MFA-resultaten.
- De configuratie van MFA dieper is geïntegreerd in de configuratie van de Azure AD-ervaring in Azure portal.

Met deze preview-versie, MFA-beheer en rapportage een geïntegreerde deel uitmaken van de belangrijkste Azure AD-configuratie-ervaring. U kunt nu de portal beheerfunctionaliteit MFA in de Azure AD-ervaring beheren.

Zie voor meer informatie, [verwijzing voor het melden van MFA in de Azure-portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

**Type:** Nieuwe functie  
**Categorie van de service:** Gebruiksvoorwaarden  
**Product-mogelijkheid:** Naleving  

U kunt Azure AD gebruiksrechtovereenkomst gebruiken om gegevens te presenteren, zoals relevante disclaimers voor juridische vereisten of nalevingsvereisten voor gebruikers.

U kunt Azure AD gebruiksrechtovereenkomst gebruiken in de volgende scenario's:

- Algemene gebruiksrechtovereenkomst voor alle gebruikers in uw organisatie
- Specifieke gebruiksrechtovereenkomst op basis van kenmerken van een gebruiker (bijvoorbeeld artsen versus verpleegkundigen) of binnenlandse versus werknemers, uitgevoerd door dynamische groepen
- Specifieke gebruiksrechtovereenkomst gebruiken voor toegang tot indrukwekkende business-apps, zoals Salesforce

Zie voor meer informatie, [Azure AD gebruiksrechtovereenkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---

### <a name="enhancements-to-privileged-identity-management"></a>Verbeteringen voor Privileged Identity Management

**Type:** Nieuwe functie  
**Categorie van de service:** Privileged Identity Management  
**Product-mogelijkheid:** Privileged Identity Management  

U kunt met Azure AD Privileged Identity Management kunt beheren, controleren en toegang tot Azure-resources (preview) binnen uw organisatie te controleren:

- Abonnementen
- Resourcegroepen
- Virtuele machines 

Alle resources in Azure portal die gebruikmaken van de functionaliteit voor Azure RBAC kunnen profiteren van alle beveiligings- en functionaliteit voor het beheer die Azure AD Privileged Identity Management te bieden heeft.

Zie voor meer informatie, [Privileged Identity Management voor Azure-resources](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Toegangsbeoordelingen

**Type:** Nieuwe functie  
**Categorie van de service:** Toegangsbeoordelingen  
**Product-mogelijkheid:** Naleving  

Organisaties kunnen toegangsbeoordelingen (preview) gebruiken op efficiënte wijze groepslidmaatschappen en toegang tot bedrijfstoepassingen beheren: 

- U kunt toegang voor gastgebruikers opnieuw certificeren door gebruik te maken van toegangsbeoordelingen van hun toegang tot toepassingen en lidmaatschappen van groepen. Revisoren kunnen efficiënt kunnen beslissen of het om toe te staan van gasten toegang op basis van de inzichten geleverd door de toegangsbeoordelingen blijvende.
- Met toegangsbeoordelingen kunt u de toegang van werknemers voor toepassingen en groepslidmaatschappen opnieuw certificeren.

U kunt de controles van toegangsbeoordelingen verzamelen in programma's die relevant zijn voor uw organisatie, om beoordelingen bij te houden voor naleving of risicogevoelige toepassingen.

Zie voor meer informatie, [Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Toepassingen van derden van mijn Apps en het startprogramma voor Office 365 verbergen

**Type:** Nieuwe functie  
**Categorie van de service:** Mijn Apps  
**Product-mogelijkheid:** Eenmalige aanmelding  

Nu kunt u beter apps beheren die worden weergegeven op uw gebruikers portals door middel van een nieuwe **app verbergen** eigenschap. U kunt apps om in gevallen waar de app-tegels voor back-end-services of dubbele tegels en overbodige items gebruikers-app voor ruimtevaartuigen weergegeven te verbergen. De wisselknop is in de **eigenschappen** sectie van de app van derden en heet **zichtbaar voor gebruiker?** U kunt ook een app via een programma via PowerShell verbergen. 

Zie voor meer informatie, [verbergen van een toepassing van derden uit de ervaring van een gebruiker in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**Wat is er beschikbaar?**

 Als onderdeel van de overgang naar de nieuwe beheerconsole, twee nieuwe API's voor het ophalen van Azure AD-activiteit logboeken beschikbaar zijn. De nieuwe set API's biedt uitgebreidere filteren en sorteren van functionaliteit naast het leveren van uitgebreide controle- en aanmeldingsactiviteiten. De gegevens die eerder beschikbaar tot en met de beveiligingsrapporten nu zijn toegankelijk via de API voor Identity Protection risico gebeurtenissen in Microsoft Graph.


## <a name="september-2017"></a>September 2017

### <a name="hotfix-for-identity-manager"></a>Hotfix voor Identity Manager

**Type:** Gewijzigde functie  
**Categorie van de service:** Identity Manager  
**Product-mogelijkheid:** Beheer van identiteitslevenscycli  

Een hotfix totaliseren-pakket (build 4.4.1642.0) is beschikbaar vanaf 25 September 2017 voor Identity Manager 2016 Service Pack 1. Dit pakket totaliseren:

- Lost problemen op en voegt verbeteringen toe.
- Is een cumulatieve update die wordt vervangen door alle Identity Manager 2016 Service Pack 1-updates maximaal build 4.4.1459.0 voor Identity Manager 2016. 
- Vereist dat u beschikt over Identity Manager 2016 4.4.1302.0 bouwen. 

Zie voor meer informatie, [hotfixpakket (build 4.4.1642.0) is beschikbaar voor Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---
