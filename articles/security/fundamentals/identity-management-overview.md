---
title: Azure-beveiligings functies die u helpen bij het identiteits beheer | Microsoft Docs
description: " Dit artikel bevat een overzicht van de belangrijkste Azure-beveiligings functies die u helpen bij het identiteits beheer. Met de oplossingen voor identiteits-en toegangs beheer van micro soft kunt u de toegang tot toepassingen en resources in het bedrijfs centrum en in de Cloud beveiligen, waardoor er extra validatie niveaus, zoals multi-factor Authentication en voorwaardelijke toegang, mogelijk zijn. restrictie. "
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: terrylan
Customer intent: As an IT Pro or decision maker I am trying to learn about identity management capabilities in Azure
ms.openlocfilehash: ee9f771483b0478ec4b7d1bf92095d9dd07f45ad
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727243"
---
# <a name="azure-identity-management-security-overview"></a>Overzicht van Azure Identity Management-beveiliging

 Identiteits beheer is het proces van het verifiëren en autoriseren van [beveiligings](/windows/security/identity-protection/access-control/security-principals)-principals. Het omvat ook het beheren van informatie over deze principals (identiteiten). Beveiligings-principals (identiteiten) kunnen services, toepassingen, gebruikers, groepen, enzovoort bevatten. Met de oplossingen voor identiteits-en toegangs beheer van micro soft kunt u de toegang tot toepassingen en resources in het bedrijfs centrum en in de Cloud beveiligen. Dergelijke bescherming maakt extra validatie niveaus mogelijk, zoals multi-factor Authentication en beleid voor voorwaardelijke toegang. Het bewaken van verdachte activiteiten via geavanceerde beveiligings rapportage, controle en waarschuwingen helpt mogelijke beveiligings problemen te verhelpen. [Azure Active Directory Premium](/azure/active-directory/active-directory-editions) biedt eenmalige aanmelding (SSO) voor duizenden cloud software as a Service (SaaS)-apps en toegang tot web-apps die u on-premises uitvoert.
 
Door gebruik te maken van de beveiligings voordelen van Azure Active Directory (Azure AD) kunt u het volgende doen:

* Maak en beheer één identiteit voor elke gebruiker in uw hybride onderneming, zodat gebruikers, groepen en apparaten synchroon blijven. 
* Bied SSO-toegang tot uw toepassingen, waaronder duizenden vooraf geïntegreerde SaaS-apps.
* Beveiliging van de toegang tot de toepassing inschakelen door op regels gebaseerde multi-factor Authentication af te dwingen voor zowel on-premises als Cloud toepassingen.
* U kunt veilige externe toegang tot on-premises webtoepassingen inrichten via Azure AD-toepassingsproxy.

Het doel van dit artikel is een overzicht te geven van de belangrijkste Azure-beveiligings functies die u helpen bij het identiteits beheer. We bieden ook koppelingen naar artikelen met meer informatie over elke functie, zodat u meer informatie kunt krijgen.  

Het artikel is gericht op de volgende kern mogelijkheden van Azure Identity Management:

* Eenmalige aanmelding
* Omgekeerde proxy
* Multi-Factor Authentication
* Op rollen gebaseerd toegangs beheer (RBAC)
* Beveiligings bewaking, waarschuwingen en rapporten op basis van machine learning
* Identiteits- en toegangsbeheer van consumenten
* Apparaatregistratie
* Privileged identity management
* Identiteitsbeveiliging
* Hybride identiteits beheer/Azure AD Connect
* Azure AD-toegangsbeoordelingen

## <a name="single-sign-on"></a>Eenmalige aanmelding

SSO houdt in dat u toegang hebt tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, door zich slechts één keer aan te melden met één gebruikers account. Wanneer u bent aangemeld, hebt u toegang tot alle toepassingen die u nodig hebt zonder dat u een tweede keer hoeft te verifiëren (Typ bijvoorbeeld een wacht woord).

Veel organisaties zijn afhankelijk van SaaS-toepassingen, zoals Office 365, box en Sales Force voor gebruikers productiviteit. In het verleden moesten IT-mede werkers in elke SaaS-toepassing afzonderlijke gebruikers accounts maken en bijwerken, en moeten gebruikers een wacht woord onthouden voor elke SaaS-toepassing.

Azure AD breidt on-premises Active Directory omgevingen uit in de Cloud, zodat gebruikers hun primaire organisatie-account kunnen gebruiken om zich niet alleen aan te melden bij apparaten en bedrijfs bronnen in het domein, maar ook voor alle web-en SaaS-toepassingen die ze nodig hebben. voor hun taken.

Gebruikers hoeven niet alleen meerdere sets met gebruikers namen en wacht woorden te beheren, maar u kunt de toegang tot toepassingen automatisch inrichten of ongedaan maken op basis van hun organisatie groepen en hun werknemers status. Azure AD introduceert beveiligings-en toegangs beheer Programma's waarmee u de toegang van gebruikers tot SaaS-toepassingen centraal kunt beheren.

Meer informatie:

* [Overzicht van eenmalige aanmelding](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [What is application access and single sign-on with Azure Active Directory?](../../active-directory/manage-apps/what-is-single-sign-on.md) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?)
* [Azure Active Directory eenmalige aanmelding integreren met SaaS-apps](../../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Omgekeerde proxy

Met Azure AD-toepassingsproxy kunt u on-premises toepassingen, zoals [share point](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) -sites, [Outlook Web app](https://technet.microsoft.com/library/jj657718.aspx)en [IIS](https://www.iis.net/)-apps in uw particuliere netwerk publiceren en beveiligde toegang bieden tot gebruikers buiten uw netwerk. Toepassings proxy biedt externe toegang en eenmalige aanmelding voor veel typen on-premises webtoepassingen met duizenden SaaS-toepassingen die door Azure AD worden ondersteund. Werk nemers kunnen zich vanaf hun eigen apparaten aanmelden bij uw apps en zich verifiëren via deze proxy op basis van de Cloud.

Meer informatie:

* [Azure AD-toepassingsproxy inschakelen](/azure/active-directory/manage-apps/application-proxy-enable)
* [Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](/azure/active-directory/active-directory-application-proxy-publish)
* [Eenmalige aanmelding met toepassings proxy](../../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Werken met voorwaardelijke toegang](../../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure multi-factor Authentication is een verificatie methode waarvoor het gebruik van meer dan één verificatie methode is vereist en waarmee een kritieke tweede beveiligingslaag wordt toegevoegd aan gebruikers aanmeldingen en trans acties. Multi-factor Authentication helpt de toegang tot gegevens en toepassingen te beschermen terwijl de vraag naar gebruikers wordt gevergaderd voor een eenvoudig aanmeldings proces. Het biedt sterke verificatie via verschillende verificatie opties: telefoon gesprekken, tekst berichten, meldingen over mobiele apps of verificatie codes en OAuth-tokens van derden.

Meer informatie:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](/azure/active-directory/authentication/multi-factor-authentication)
* [Hoe Azure multi-factor Authentication werkt](../../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="rbac"></a>RBAC

RBAC is een autorisatie systeem dat is gebaseerd op Azure Resource Manager die een nauw keurig toegangs beheer van resources in Azure biedt. Met RBAC kunt u het toegangs niveau voor gebruikers nauw keurig beheren. U kunt bijvoorbeeld een gebruiker beperken tot het beheer van virtuele netwerken en een andere gebruiker voor het beheren van alle resources in een resource groep. Azure bevat verschillende ingebouwde rollen die u kunt gebruiken. Hier volgen vier fundamentele ingebouwde rollen. De eerste drie zijn op alle resourcetypen van toepassing.

Meer informatie:

* [Wat is toegangsbeheer op basis van rollen (RBAC)?](/azure/role-based-access-control/overview)
* [Ingebouwde rollen voor Azure-resources](/azure/role-based-access-control/built-in-roles)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligings bewaking, waarschuwingen en rapporten op basis van machine learning

Beveiligings bewaking, waarschuwingen en op machine learning gebaseerde rapporten waarmee inconsistente toegangs patronen worden geïdentificeerd, kunnen u helpen bij het beveiligen van uw bedrijf. U kunt Azure AD-toegangs-en gebruiks rapporten gebruiken om inzicht te krijgen in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kan een Directory-beheerder beter bepalen waar mogelijke beveiligings Risico's zich kunnen voordoen, zodat ze adequaat kunnen plannen om deze Risico's te beperken.

In het Azure Portal vallen rapporten in de volgende categorieën:

* **Afwijkende rapporten**: Bevatten aanmeldings gebeurtenissen die afwijkend zijn aangetroffen. Ons doel is om u op de hoogte te stellen van deze activiteit en u te laten bepalen of een gebeurtenis verdacht is.
* **Geïntegreerde toepassings rapporten**: Geef inzicht in de manier waarop Cloud toepassingen in uw organisatie worden gebruikt. Azure AD biedt integratie met duizenden Cloud toepassingen.
* **Fout rapporten**: Geef fouten op die kunnen optreden wanneer u accounts inricht op externe toepassingen.
* **Gebruikersspecifieke rapporten**: Gegevens van de aanmeldings activiteit voor een apparaat weer geven voor een specifieke gebruiker.
* **Activiteiten logboeken**: Een record bevatten van alle gecontroleerde gebeurtenissen in de afgelopen 24 uur, de afgelopen 7 dagen of de afgelopen 30 dagen, en de groeps activiteiten wijzigen en het opnieuw instellen van het wacht woord en de registratie activiteit.

Meer informatie:

* [Uw toegangs- en gebruiksrapporten weergeven](/azure/active-directory/active-directory-view-access-usage-reports)
* [Aan de slag met Azure Active Directory rapportage](/azure/active-directory/active-directory-reporting-getting-started)
* [Azure Active Directory-rapportage gids](/azure/active-directory/active-directory-reporting-guide)

## <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten

Azure AD B2C is een Maxi maal beschik bare, wereld wijde service voor identiteits beheer voor consumenten toepassingen die worden geschaald naar honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw consumenten kunnen zich bij al uw toepassingen aanmelden met behulp van hun bestaande sociale accounts of door nieuwe referenties te maken.

In het verleden hebben ontwikkel aars van toepassingen die klanten wilden registreren en deze aanmelden bij hun toepassingen, hun eigen code geschreven. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure AD B2C biedt uw organisatie een betere manier om identiteits beheer van consumenten te integreren in toepassingen met behulp van een beveiligd, op standaarden gebaseerd platform en een groot aantal flexibele beleids regels.

Wanneer u Azure AD B2C gebruikt, kunnen uw consumenten zich registreren voor uw toepassingen met hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door nieuwe referenties te maken (e-mail adres en wacht woord of gebruikers naam en wacht woord).

Meer informatie:

* [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Azure Active Directory B2C Preview: Gebruikers registreren en aanmelden bij uw toepassingen](../../active-directory-b2c/active-directory-b2c-overview.md)
* [Azure Active Directory B2C Preview: Typen toepassingen](../../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Apparaatregistratie

Registratie van Azure AD-apparaten is de basis voor op apparaten gebaseerde scenario's voor [voorwaardelijke toegang](/azure/active-directory/active-directory-conditional-access-device-registration-overview) . Wanneer een apparaat is geregistreerd, geeft Azure AD-apparaatregistratie het apparaat een identiteit die wordt gebruikt om het apparaat te verifiëren wanneer een gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt voor het afdwingen van beleid voor voorwaardelijke toegang voor toepassingen die worden gehost in de Cloud en on-premises.

In combi natie met een Mobile Device Management oplossing, zoals intune, worden de kenmerken van het apparaat in azure AD bijgewerkt met aanvullende informatie over het apparaat. U kunt vervolgens regels voor voorwaardelijke toegang maken waarmee de toegang wordt afgedwongen van apparaten om te voldoen aan uw normen voor beveiliging en naleving.

Meer informatie:

* [Aan de slag met Azure AD-apparaatregistratie](/azure/active-directory/active-directory-conditional-access-device-registration-overview)
* [Automatische apparaatregistratie met Azure AD voor Windows-apparaten die lid zijn van een domein](/azure/active-directory/active-directory-conditional-access-automatic-device-registration)
* [Automatische registratie van Windows-apparaten die lid zijn van een domein instellen met Azure AD](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)

## <a name="privileged-identity-management"></a>Privileged identity management

Met Azure AD Privileged Identity Management kunt u uw bevoorrechte identiteiten en toegang tot resources in azure AD en andere micro soft-onlineservices, zoals Office 365 en Microsoft Intune, beheren, controleren en bewaken.

Gebruikers moeten soms privileged-bewerkingen uitvoeren in azure-of Office 365-resources of in andere SaaS-apps. Dit betekent vaak dat organisaties gebruikers permanente toegang moeten geven in azure AD. Dergelijke toegang is een groeiende beveiligings risico voor in de Cloud gehoste resources, omdat organisaties niet voldoende kunnen controleren wat de gebruikers doen met hun beheerders bevoegdheden. Als een gebruikers account met bevoorrechte toegang is aangetast, kan dit ook van invloed zijn op de algehele Cloud beveiliging van de organisatie. Azure AD Privileged Identity Management helpt dit risico te verhelpen.

Met Azure AD Privileged Identity Management kunt u het volgende doen:

* Zie welke gebruikers Azure AD-beheerders zijn.
* Schakel just-in-time-beheer toegang op aanvraag in voor micro soft-services zoals Office 365 en intune.
* Rapporten over de geschiedenis van beheerders toegang en wijzigingen in beheerders toewijzingen ophalen.
* Ontvang waarschuwingen over toegang tot een bevoorrechte rol.

Meer informatie:

* [Wat is Azure AD Privileged Identity Management?](../../active-directory/privileged-identity-management/pim-configure.md)
* [Azure AD-directory-rollen in PIM toewijzen](../../active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging

Azure AD Identity Protection is een beveiligings service die een geconsolideerde weer gave biedt voor risico gebeurtenissen en mogelijke beveiligings problemen die van invloed zijn op de identiteiten van uw organisatie. Identiteits bescherming maakt gebruik van de bestaande Azure AD-detectie mogelijkheden voor afwijkingen, die beschikbaar zijn via rapporten van afwijkende activiteiten van Azure AD. Identiteits beveiliging introduceert ook nieuwe risico gebeurtenis typen waarmee afwijkingen in realtime kunnen worden gedetecteerd.

Meer informatie:

* [Azure AD Identity Protection](/azure/active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en identiteits weergave: Preview van identiteits beveiliging](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-managementazure-ad-connect"></a>Hybride identiteits beheer/Azure AD Connect

De identiteitsoplossingen van Microsoft omvatten mogelijkheden voor zowel on-premises als in de cloud. Er wordt één gebruikersidentiteit gemaakt voor verificatie en autorisatie bij alle resources, ongeacht hun locatie. We noemen dit hybride identiteit. Azure AD Connect is het programma van Microsoft dat is ontworpen om te voldoen aan uw doelstellingen voor een hybride identiteit. Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD. Deze biedt de volgende functies:

* Synchronisatie
* Integratie van AD FS en Federatie
* Pass Through-verificatie
* Statuscontrole

Meer informatie:

* [Technisch document hybride identiteit](https://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blog van het team van Azure AD](https://blogs.technet.microsoft.com/ad/)

## <a name="azure-ad-access-reviews"></a>Azure AD-toegangsbeoordelingen

Met Azure AD-toegangsbeoordelingen (Azure Active Directory) kunnen organisaties op efficiënte wijze groepslidmaatschappen, de toegang tot bedrijfstoepassingen en de toewijzing van bevoorrechte rollen beheren.

Meer informatie:

* [Azure AD-toegangs beoordelingen](../../active-directory/governance/access-reviews-overview.md)
* [Gebruikerstoegang beheren met Azure AD-toegangsbeoordelingen](../../active-directory/governance/access-reviews-overview.md)
