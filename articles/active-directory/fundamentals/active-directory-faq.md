---
title: Veelgestelde vragen (FAQ) - Azure Active Directory | Microsoft Docs
description: Veelgestelde vragen en antwoorden over Azure en Azure Active Directory, wachtwoordbeheer en toegang tot toepassingen.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c1ee5e849d8004f828a2d92d728ad7925fc05c4
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693956"
---
# <a name="frequently-asked-questions-about-azure-active-directory"></a>Veelgestelde vragen over Azure Active Directory
Azure Active Directory (Azure AD) is een uitgebreide IDaaS-oplossing (Identity as a Service) waarin alle aspecten van identiteit, toegangsbeheer en beveiliging zijn opgenomen.

Zie [Wat is Azure Active Directory?](active-directory-whatis.md) voor meer informatie.


## <a name="access-azure-and-azure-active-directory"></a>Toegang tot Azure en Azure Active Directory
**V: Waarom krijg ik ' geen abonnementen gevonden ' wanneer ik Azure AD wil openen in de Azure Portal?**

**A:** Voor toegang tot de Azure Portal heeft elke gebruiker machtigingen nodig met een Azure-abonnement. Als u een betaald abonnement voor Office 365 of Azure AD hebt, gaat u naar [https://aka.ms/accessAAD](https://aka.ms/accessAAD) voor eenmalige activering in één stap. Anders moet u een gratis [Azure-account](https://azure.microsoft.com/pricing/free-trial/) activeren of een betaald abonnement afsluiten.

Zie voor meer informatie:

* [Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

---
**V: Wat is de relatie tussen Azure AD, Office 365 en Azure?**

**A:** Azure AD biedt u algemene identiteits-en toegangs mogelijkheden voor alle webservices. Als u Office 365, Microsoft Azure, Intune of andere toepassingen gebruikt, gebruikt u Azure AD al bij het inschakelen van aanmeldings- en toegangsbeheer voor deze services.

Alle gebruikers die webservices kunnen gebruiken, worden gedefinieerd als gebruikersaccounts in een of meer exemplaren van Azure AD. U kunt deze accounts instellen voor gratis Azure AD-functies, zoals toegang tot cloudtoepassingen.

Betaalde Azure AD-services zoals Enterprise Mobility + Security vormen een aanvulling op andere webservices, zoals Office 365 en Microsoft Azure, met uitgebreide oplossingen voor beheer en beveiliging die ook geschikt zijn voor grote organisaties.

---

**V:  Wat zijn de verschillen tussen eigenaar en globale beheerder?**

**A:** De persoon die zich aanmeldt voor een Azure-abonnement wordt standaard toegewezen aan de rol van eigenaar voor Azure-resources. Een eigenaar kunt gebruiken voor een Microsoft-account of een werk- of school-account uit de directory waaraan het Azure-abonnement is gekoppeld.  Deze rol is gemachtigd om services in Azure Portal te beheren.

Als anderen zich moeten aanmelden en toegang tot services met behulp van hetzelfde abonnement, kunt u ze toewijzen de juiste [ingebouwde rol](../../role-based-access-control/built-in-roles.md). Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).

Standaard is de persoon die zich aanmeldt voor een Azure-abonnement de rol globale beheerder voor de map toegewezen. De globale beheerder heeft toegang tot alle functies van Azure AD-directory. Azure AD heeft een andere set beheerdersrollen voor het beheren van de directory en identiteitsgerelateerde functies. Deze beheerders hebben toegang tot verschillende functies in Azure portal. De rol van beheerder bepaalt wat ze kunnen doen, zoals het maken of bewerken van gebruikers, beheerdersrollen toewijzen aan anderen, gebruikerswachtwoorden opnieuw instellen, Gebruikerslicenties beheren of domeinen beheren.  Zie voor meer informatie over Azure AD-directorybeheerders en hun rollen, [een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](active-directory-users-assign-role-azure-portal.md) en [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

Daarnaast vormen betaalde Azure AD-services zoals Enterprise Mobility + Security een aanvulling op andere webservices, zoals Office 365 en Microsoft Azure, met uitgebreide oplossingen voor beheer en beveiliging die ook geschikt zijn voor grote organisaties.

---
**V: Is er een rapport waarin wordt weer gegeven wanneer mijn Azure AD-gebruikers licenties verlopen?**

**A:** Nee.  Dit is momenteel niet beschikbaar.

---

## <a name="get-started-with-hybrid-azure-ad"></a>Aan de slag met hybride Azure AD


**V: Hoe kan ik een Tenant laten staan wanneer ik als samen werker is toegevoegd?**

**A:** Wanneer u als samen werker aan de Tenant van een andere organisatie wordt toegevoegd, kunt u de "Tenant schakelaar" in de rechter bovenhoek gebruiken om te scha kelen tussen tenants.  Op dit moment is er geen manier om de organisatie die u heeft uitgenodigd, te verlaten. Microsoft is bezig om deze functie te ontwikkelen.  Tot deze functie beschikbaar is, kunt u de organisatie die u heeft uitgenodigd, vragen om u uit de tenant te verwijderen.

---
**V: Hoe kan ik mijn on-premises Directory verbinden met Azure AD?**

**A:** U kunt uw on-premises Directory verbinden met Azure AD met behulp van Azure AD Connect.

Zie [Integrating your on-premises identities with Azure Active Directory](../hybrid/whatis-hybrid-identity.md) (Uw on-premises identiteiten integreren met Azure Active Directory) voor meer informatie.

---
**V: Hoe kan ik eenmalige aanmelding instellen tussen mijn on-premises Directory en mijn Cloud toepassingen?**

**A:** U hoeft eenmalige aanmelding (SSO) alleen in te stellen tussen uw on-premises Directory en Azure AD. Zo lang u uw cloudtoepassingen opent via Azure AD, wordt er met de service automatisch voor gezorgd dat uw gebruikers juist worden geverifieerd met hun on-premises referenties.

U kunt eenmalige aanmelding eenvoudig on-premises implementeren met federatieoplossingen, zoals Active Directory Federation Services (AD FS), of door wachtwoordhashsynchronisatie te configureren. U kunt beide opties eenvoudig implementeren met de wizard Azure AD Connect-configuratie.

Zie [Integrating your on-premises identities with Azure Active Directory](../hybrid/whatis-hybrid-identity.md) (Uw on-premises identiteiten integreren met Azure Active Directory) voor meer informatie.

---
**V: Biedt Azure AD een self-service portal voor gebruikers in mijn organisatie?**

**A:** Ja, Azure AD biedt u het [Azure AD-toegangs venster](https://myapps.microsoft.com) voor selfservice-en toepassings toegang voor gebruikers. Als u een Office 365-klant bent, kunt u veel van dezelfde mogelijkheden in de [Office 365-Portal](https://portal.office.com)vinden.

Zie [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md) (Inleiding tot het toegangsvenster) voor meer informatie.

---
**V: Helpt Azure AD bij het beheren van mijn on-premises infra structuur?**

**A:** Ja. Azure AD Connect Health is opgenomen in de Azure AD Premium-versie. Azure AD Connect Health helpt u om inzicht te verkrijgen in uw on-premises infrastructuur voor identiteiten en de synchronisatieservices.  

Zie [Monitor your on-premises identity infrastructure and synchronization services in the cloud](../hybrid/whatis-hybrid-identity-health.md) (Uw on-premises infrastructuur voor identiteiten en synchronisatieservices in de cloud controleren) voor meer informatie.  

---
## <a name="password-management"></a>Wachtwoordbeheer
**V: Kan ik het terugschrijven van wacht woorden van Azure AD gebruiken zonder wachtwoord synchronisatie? (Is het in dit scenario mogelijk om Azure AD selfservice voor wachtwoordherstel (SSPR) met terugschrijven van wachtwoorden te gebruiken zonder dat er wachtwoorden in de cloud worden opgeslagen?)**

**A:** U hoeft uw Active Directory wacht woorden niet te synchroniseren met Azure AD om terugschrijven in te scha kelen. In een federatieve omgeving is eenmalige aanmelding van Azure AD (SSO) afhankelijk van de on-premises directory voor het verifiëren van de gebruiker. In dit geval is er geen on-premises wachtwoord vereist dat moet worden bijgehouden in Azure AD.

---
**V: Hoe lang duurt het voordat een wacht woord is teruggeschreven naar Active Directory on-premises?**

**A:** Write-back van wacht woord wordt in realtime uitgevoerd.

Zie voor meer informatie [Getting started with password management](../authentication/quickstart-sspr.md).

---
**V: Kan ik terugschrijven van wacht woord gebruiken met wacht woorden die worden beheerd door een beheerder?**

**A:** Ja, als u wacht woord terugschrijven hebt ingeschakeld, worden de wachtwoord bewerkingen van een beheerder teruggeschreven naar uw on-premises omgeving.  

<a name="for-more-answers-to-password-related-questions-see-password-management-frequently-asked-questionsauthenticationactive-directory-passwords-faqmd"></a>Zie [Password management frequently asked questions](../authentication/active-directory-passwords-faq.md) (Veelgestelde vragen over wachtwoordbeheer) voor meer antwoorden op vragen over wachtwoorden.
---
**V:  Wat kan ik doen als ik mijn bestaande Office 365/Azure AD-wacht woord niet meer weet tijdens het wijzigen van mijn wacht woord?**

**A:** Voor dit type situatie zijn er een aantal opties.  Gebruik selfservice voor wachtwoordherstel (SSPR) als deze optie beschikbaar is.  Of SSPR werkt, is afhankelijk van de configuratie.  Zie voor meer informatie [Hoe werkt de portal voor wachtwoordherstel?](../authentication/howto-sspr-deployment.md).

Office 365-gebruikers kunnen hun beheerder vragen het wachtwoord opnieuw in te stellen via de stappen die worden beschreven in [Gebruikerswachtwoorden opnieuw instellen](https://support.office.com/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).

Beheerders kunnen wachtwoorden van Azure AD-accounts via een van de volgende opties opnieuw instellen:

- [Accounts in Azure Portal opnieuw instellen](active-directory-users-reset-password-azure-portal.md)
- [PowerShell gebruiken](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


---
## <a name="security"></a>Beveiliging
**V: Zijn accounts vergrendeld na een bepaald aantal mislukte pogingen of is er een meer geavanceerde strategie gebruikt?**

We gebruiken een geavanceerdere strategie om account te vergrendelen.  Dit is gebaseerd op het IP-adres van de aanvraag en op de ingevoerde wachtwoorden. De duur van de vergrendeling wordt ook langer op basis van de kans dat het een aanval betreft.  

**V:  Bepaalde (algemene) wacht woorden worden geweigerd met de berichten ' dit wacht woord is veel keer gebruikt ', maar dit is de wacht woorden die worden gebruikt in de huidige Active Directory?**

Dit verwijst naar de wachtwoorden die wereldwijd gemeenschappelijk, zoals varianten van 'Wachtwoord' en '123456' zijn.

**V: Wordt een aanmeldings aanvraag van dubious-bronnen (botnets, Tor-eind punt) geblokkeerd in een B2C-Tenant of heeft dit een basis-of Premium-Tenant?**

We hebben wel een gateway waarmee aanvragen worden gefilterd en die enige bescherming biedt tegen botnets. Deze wordt toegepast op alle B2C-tenants.

## <a name="application-access"></a>Toegang tot toepassingen

**V: Waar vind ik een lijst met toepassingen die vooraf zijn geïntegreerd met Azure AD en hun mogelijkheden?**

**A:** Azure AD heeft meer dan 2.600 vooraf geïntegreerde toepassingen van micro soft, toepassings serviceproviders en partners. Alle vooraf geïntegreerde toepassingen bieden ondersteuning voor eenmalige aanmelding (SSO). Via SSO kunt u uw bedrijfsreferenties gebruiken om toegang te krijgen tot uw apps. Een aantal toepassingen ondersteunt ook geautomatiseerde inrichting en het ongedaan maken van de inrichting.

Zie de [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/) voor een volledige lijst met vooraf geïntegreerde toepassingen.

---
**V: Wat gebeurt er als de toepassing die ik nodig heb niet in de Azure AD Marketplace?**

**A:** Met Azure AD Premium kunt u elke gewenste toepassing toevoegen en configureren. U kunt eenmalige aanmelding en geautomatiseerde inrichting configureren, afhankelijk van de mogelijkheden van uw toepassing en uw voorkeuren.  

Zie voor meer informatie:

* [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](../manage-apps/use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)

---
**V: Hoe melden gebruikers zich aan bij toepassingen met behulp van Azure AD?**

**A:** Azure AD biedt verschillende manieren waarop gebruikers hun toepassingen kunnen weer geven en openen, zoals:

* Het deelvenster Azure AD Access
* Het startprogramma voor toepassingen van Office 365
* Directe aanmelding bij federatieve apps
* Dieptekoppelingen naar federatieve apps, op basis van wachtwoorden, of bestaande apps

Zie voor meer informatie, [eindgebruiker ervaringen voor toepassingen](../manage-apps/end-user-experiences.md).

---
**V: Wat zijn de verschillende manieren waarop Azure AD verificatie en eenmalige aanmelding voor toepassingen mogelijk maakt?**

**A:** Azure AD ondersteunt veel gestandaardiseerde protocollen voor verificatie en autorisatie, zoals SAML 2,0, OpenID Connect Connect, OAuth 2,0 en WS-Federation. Daarnaast ondersteunt Azure AD het gebruik van wachtwoordkluizen en mogelijkheden voor automatische aanmelding voor apps die alleen ondersteuning bieden voor verificatie op basis van formulieren.  

Zie voor meer informatie:

* [Authentication Scenarios for Azure AD](../develop/authentication-scenarios.md) (Verificatiescenario's voor Azure AD)
* [Verificatie- en autorisatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Eenmalige aanmelding voor toepassingen in Azure AD](../manage-apps/what-is-single-sign-on.md)

---
**V: Kan ik toepassingen toevoegen die ik on-premises heb uitgevoerd?**

**A:** Azure AD-toepassingsproxy biedt u eenvoudige en veilige toegang tot on-premises webtoepassingen die u kiest. U kunt deze toepassingen in Azure AD op dezelfde manier openen als uw SaaS-apps (Software as a Service). U hebt geen VPN nodig en u hoeft uw netwerkinfrastructuur niet te wijzigen.  

Zie [How to provide secure remote access to on-premises applications](../manage-apps/application-proxy.md) (Beveiligde externe toegang bieden voor on-premises toepassingen) voor meer informatie.

---
**V: Hoe kan ik multi-factor Authentication vereisen voor gebruikers die toegang hebben tot een bepaalde toepassing?**

**A:** Met voorwaardelijke toegang van Azure AD kunt u voor elke toepassing een uniek toegangs beleid toewijzen. In uw beleid kunt u aangeven dat Multi-Factor Authentication altijd is vereist of alleen wanneer gebruikers niet zijn verbonden met het lokale netwerk.  

Zie [Securing access to Office 365 and other apps connected to Azure Active Directory](../active-directory-conditional-access-azure-portal.md) (De toegang beveiligen tot Office 365 en andere apps die zijn verbonden met Azure Active Directory) voor meer informatie.

---
**V: Wat is geautomatiseerde gebruikers inrichting voor SaaS-apps?**

**A:** Gebruik Azure AD om het maken, onderhouden en verwijderen van gebruikers identiteiten in veel populaire Cloud-apps te automatiseren.

Zie [Automate user provisioning and deprovisioning to SaaS applications with Azure Active Directory](../manage-apps/user-provisioning.md) (Automatisch gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory) voor meer informatie.

---
**V:  Kan ik een veilige LDAP-verbinding instellen met Azure AD?**

**A:**  Nee. Azure AD biedt geen ondersteuning voor het LDAP-protocol (Lightweight Directory Access Protocol) of Secure LDAP rechtstreeks. Het is echter mogelijk om Azure AD Domain Services-instantie (Azure AD DS) in te scha kelen op uw Azure AD-Tenant met correct geconfigureerde netwerk beveiligings groepen via Azure-netwerken om LDAP-connectiviteit te bewerkstelligen. Zie https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap voor meer informatie.
