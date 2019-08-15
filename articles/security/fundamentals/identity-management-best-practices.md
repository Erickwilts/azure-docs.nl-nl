---
title: Aanbevolen procedures voor Azure Identity & Access-beveiliging | Microsoft Docs
description: Dit artikel bevat een aantal aanbevolen procedures voor het beheer van identiteiten en toegangs beheer met ingebouwde Azure-functies.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2019
ms.author: barclayn
ms.openlocfilehash: 371c3b9fde17bba33fb6f2526be68fe89aec6b01
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934692"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Aanbevolen procedures voor de beveiliging van Azure Identity Management en Access Control
In dit artikel bespreken we een verzameling van de aanbevolen procedures voor het beheren van Azure-identiteits beheer en-toegangs beheer. Deze aanbevolen procedures zijn afgeleid van onze ervaring met [Azure AD](../../active-directory/fundamentals/active-directory-whatis.md) en de ervaringen van klanten, zoals uzelf.

Voor elke best practice wordt uitgelegd:

* Wat de best practice is
* Waarom u die best practice wilt inschakelen
* Wat kan het gevolg zijn als u de best practice niet inschakelt
* Mogelijke alternatieven voor de best practice
* Informatie over het inschakelen van de best practice

Dit artikel over best practices voor het beheren van Azure-identiteits beheer en-toegang is gebaseerd op een advies-en Azure-platform mogelijkheden en-functie sets, zoals deze bestaan op het moment dat dit artikel werd geschreven. Meningen en technologieën veranderen in de loop van de tijd en dit artikel wordt regel matig bijgewerkt om deze wijzigingen weer te geven.

De aanbevolen procedures voor het beheren van Azure Identity Management en Access Control die in dit artikel worden beschreven, zijn:

* Identiteit behandelen als de primaire beveiligings verbinding
* Identiteits beheer centraliseren
* Verbonden tenants beheren
* Eenmalige aanmelding inschakelen
* Voorwaardelijke toegang inschakelen
* Wachtwoordbeheer inschakelen
* Multi-factor-verificatie afdwingen voor gebruikers
* Op rollen gebaseerd toegangsbeheer gebruiken
* Minder bloot stelling van accounts met bevoegdheden
* Controle locaties waar resources zich bevinden
* Azure AD gebruiken voor opslag verificatie

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Identiteit behandelen als de primaire beveiligings verbinding

Het is veel zo dat identiteit de primaire verbinding is voor beveiliging. Dit is een verschuiving van de traditionele focus op netwerk beveiliging. Netwerk verbindingen blijven meer poreuze op elkaar en de verdediging van de verbinding kan niet zo effectief zijn als voorheen vóór de explosie van [BYOD](https://aka.ms/byodcg) -apparaten en Cloud toepassingen.

[Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) is de Azure-oplossing voor identiteits-en toegangs beheer. Azure AD is een multi tenant-, Cloud-en identiteits beheer service van micro soft. Hierin worden de belangrijkste adreslijst Services, het beheer van toepassings toegang en identiteits beveiliging gecombineerd tot één oplossing.

In de volgende secties worden aanbevolen procedures beschreven voor identiteits-en toegangs beveiliging met behulp van Azure AD.

## <a name="centralize-identity-management"></a>Identiteits beheer centraliseren

In een scenario met [hybride identiteit](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?) raden we u aan uw on-premises en Cloud directory's te integreren. Met integratie kan uw IT-team accounts beheren vanaf één locatie, ongeacht waar een account wordt gemaakt. Integratie helpt uw gebruikers ook productiever te zijn door een algemene identiteit op te geven voor toegang tot zowel Cloud-als on-premises resources.

**Aanbevolen procedure**: Stel één Azure AD-exemplaar in. Consistentie en één gezaghebbende bron leiden tot meer duidelijkheid en verminderen de beveiligings Risico's van menselijke fouten en configuratie complexiteit.
**Details**: Wijs één Azure AD-Directory aan als de gezaghebbende bron voor zakelijke en organisatie accounts.

**Aanbevolen procedure**: Integreer uw on-premises directory's met Azure AD.  
**Details**: Gebruik [Azure AD Connect](/azure/active-directory/connect/active-directory-aadconnect) om uw on-premises adres lijst te synchroniseren met uw Cloud Directory.

> [!Note]
> Er zijn [factoren die van invloed zijn op de prestaties van Azure AD Connect](../../active-directory/hybrid/plan-connect-performance-factors.md). Zorg ervoor dat Azure AD Connect voldoende capaciteit heeft om te voor komen dat systemen de veiligheid en productiviteit belemmeren. Grote of complexe organisaties (organisaties die meer dan 100.000 objecten inrichten) moeten de [aanbevelingen](../../active-directory/hybrid/whatis-hybrid-identity.md) volgen om hun Azure AD Connect-implementatie te optimaliseren.

**Aanbevolen procedure**: Synchroniseer geen accounts met Azure AD met hoge bevoegdheden in uw bestaande Active Directory-exemplaar.
**Details**: Wijzig de standaard [Azure AD Connect configuratie](../../active-directory/hybrid/how-to-connect-sync-configure-filtering.md) die deze accounts uitfiltert niet. Deze configuratie vermindert het risico dat aanvallers draait van Cloud naar on-premises activa (waardoor een belang rijk incident kan worden gemaakt).

**Aanbevolen procedure**: Wachtwoord hash synchronisatie inschakelen.  
**Details**: Synchronisatie van wacht woord-hashes is een functie die wordt gebruikt voor het synchroniseren van wacht woord-hashes van gebruikers van een on-premises Active Directory-exemplaar naar een Azure AD-exemplaar in de Cloud. Deze synchronisatie helpt te beschermen tegen gelekte referenties die worden herhaald bij eerdere aanvallen.

Zelfs als u besluit Federatie met Active Directory Federation Services (AD FS) of andere id-providers te gebruiken, kunt u optioneel de wachtwoord hash-synchronisatie instellen als back-up voor het geval uw on-premises servers mislukken of tijdelijk niet beschikbaar zijn. Deze synchronisatie stelt gebruikers in staat om zich bij de service aan te melden met hetzelfde wacht woord dat ze gebruiken om zich aan te melden bij hun on-premises Active Directory-exemplaar. Daarnaast kan identiteits beveiliging de aangetaste referenties detecteren door gesynchroniseerde wacht woord-hashes te vergelijken met wacht woorden waarvan bekend is dat ze worden aangetast, als een gebruiker hetzelfde e-mail adres en wacht woord heeft gebruikt op andere services die niet zijn verbonden met Azure AD.

Zie [wachtwoord hash synchronisatie implementeren met Azure AD Connect Sync](/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)voor meer informatie.

**Aanbevolen procedure**: Gebruik Azure AD voor verificatie om nieuwe toepassingen te ontwikkelen.
**Details**: Gebruik de juiste mogelijkheden om verificatie te ondersteunen:

  - Azure AD voor mede werkers
  - [Azure AD B2B](../../active-directory/b2b/index.yml) voor gast gebruikers en externe partners
  - [Azure AD B2C](../../active-directory-b2c/index.yml) om te bepalen hoe klanten zich registreren, aanmelden en hun profielen beheren wanneer ze uw toepassingen gebruiken

Organisaties die hun on-premises identiteit niet integreren met hun Cloud identiteit, kunnen meer overhead hebben bij het beheer van accounts. Deze overhead verhoogt de kans op fouten en schending van de beveiliging.

> [!Note]
> U moet kiezen in welke mappen essentiële accounts zich bevinden en of het beheer werkstation dat wordt gebruikt, wordt beheerd door nieuwe Cloud Services of bestaande processen. Het gebruik van bestaande beheer-en identiteits Provisioning-processen kan een aantal Risico's verminderen, maar kan ook het risico vormen dat een aanvaller een on-premises account in gevaar brengt en draait naar de Cloud. U kunt een andere strategie voor verschillende rollen gebruiken (bijvoorbeeld IT-beheerders en beheerders van bedrijfs eenheden). U hebt twee opties. De eerste optie is het maken van Azure AD-accounts die niet zijn gesynchroniseerd met uw on-premises Active Directory-exemplaar. Voeg uw beheer werkstation toe aan Azure AD, dat u kunt beheren en gebruiken met behulp van Microsoft Intune. Tweede optie is het gebruik van bestaande beheerders accounts door te synchroniseren met uw on-premises Active Directory-exemplaar. Gebruik bestaande werk stations in uw Active Directory domein voor beheer en beveiliging.

## <a name="manage-connected-tenants"></a>Verbonden tenants beheren
Uw beveiligings organisatie heeft inzicht nodig om Risico's te beoordelen en te bepalen of het beleid van uw organisatie en eventuele wettelijke vereisten wordt gevolgd. Zorg ervoor dat uw beveiligings organisatie inzicht heeft in alle abonnementen die zijn verbonden met uw productie omgeving en netwerk (via [Azure ExpressRoute](../../expressroute/expressroute-introduction.md) of [site-naar-site-VPN](../../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)). Een [globale beheerder/bedrijfs beheerder](../../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) in azure AD kan hun toegang tot de rol [beheerder van gebruikers toegang](../../role-based-access-control/built-in-roles.md#user-access-administrator) verhogen en alle abonnementen en beheerde groepen zien die zijn verbonden met uw omgeving.

Zie [toegang verhogen voor het beheer van alle Azure-abonnementen en-beheer groepen](../../role-based-access-control/elevate-access-global-admin.md) om ervoor te zorgen dat u en uw beveiligings groep alle abonnementen of beheer groepen kunnen weer geven die zijn verbonden met uw omgeving. U moet deze verhoogde toegang verwijderen nadat u de Risico's hebt beoordeeld.

## <a name="enable-single-sign-on"></a>Eenmalige aanmelding inschakelen

In de Cloud-eerste wereld kunt u eenmalige aanmelding (SSO) inschakelen voor apparaten, apps en services vanaf elke locatie zodat uw gebruikers overal en altijd productief kunnen zijn. Wanneer u meerdere identiteits oplossingen hebt om te beheren, wordt dit een probleem met het beheer, maar ook voor gebruikers die meerdere wacht woorden moeten onthouden.

Door dezelfde identiteits oplossing voor al uw apps en resources te gebruiken, kunt u SSO bezorgen. En uw gebruikers kunnen dezelfde set referenties gebruiken om zich aan te melden en toegang te krijgen tot de resources die ze nodig hebben, of de resources zich lokaal of in de cloud bevinden.

**Aanbevolen procedure**: Eenmalige aanmelding inschakelen.  
**Details**: Azure AD [breidt on-premises Active Directory](/azure/active-directory/connect/active-directory-aadconnect) naar de Cloud. Gebruikers kunnen hun primaire werk-of school account gebruiken voor hun aan het domein gekoppelde apparaten, bedrijfs bronnen en alle web-en SaaS-toepassingen die ze nodig hebben om hun taken uit te voeren. Gebruikers hoeven niet meerdere sets met gebruikers namen en wacht woorden te onthouden en hun toepassings toegang kan automatisch worden ingericht (of ongedaan gemaakt) op basis van de lidmaatschappen van hun organisatie groep en hun status als werk nemer. En u kunt deze toegang beheren voor galerie-apps of voor uw eigen on-premises apps die u hebt ontwikkeld en gepubliceerd via de [Azure AD-toepassingsproxy](/azure/active-directory/active-directory-application-proxy-get-started).

Gebruik SSO om gebruikers toegang te geven tot hun [SaaS-toepassingen](/azure/active-directory/active-directory-appssoaccess-whatis) op basis van hun werk-of school account in azure AD. Dit is niet alleen van toepassing op micro soft SaaS-apps, maar ook voor andere apps, zoals [Google Apps](/azure/active-directory/active-directory-saas-google-apps-tutorial) en [Sales Force](/azure/active-directory/active-directory-saas-salesforce-tutorial). U kunt uw toepassing configureren voor het gebruik van Azure AD als [ID-provider op basis van SAML](/azure/active-directory/fundamentals-identity) . Als beveiligings beheer verleent Azure AD geen token waarmee gebruikers zich kunnen aanmelden bij de toepassing, tenzij ze toegang hebben gekregen via Azure AD. U kunt rechtstreeks toegang verlenen, of via een groep waarvan gebruikers lid zijn.

Organisaties die geen gemeen schappelijke identiteit voor het opzetten van SSO voor hun gebruikers en toepassingen maken, zijn meer beschikbaar in scenario's waarin gebruikers meerdere wacht woorden hebben. Deze scenario's verg Roten de kans dat gebruikers wacht woorden opnieuw gebruiken of zwakke wacht woorden gebruiken.

## <a name="turn-on-conditional-access"></a>Voorwaardelijke toegang inschakelen

Gebruikers hebben vanaf elke locatie toegang tot de resources van uw organisatie door gebruik te maken van verschillende apparaten en apps. Als IT-beheerder wilt u er zeker van zijn dat deze apparaten voldoen aan uw standaarden voor beveiliging en naleving. U hoeft alleen maar te focussen op wie toegang heeft tot een resource die niet meer voldoende is.

Als u de beveiliging en productiviteit wilt afstemmen, moet u nadenken over de manier waarop een resource wordt geopend voordat u een beslissing kunt nemen over toegangs beheer. Met voorwaardelijke toegang van Azure AD kunt u deze vereiste aanpakken. Met voorwaardelijke toegang kunt u automatische beslissingen voor toegangs beheer maken op basis van de voor waarden voor toegang tot uw Cloud-apps.

**Aanbevolen procedure**: Beheer en beheer de toegang tot bedrijfs resources.  
**Details**: [Voorwaardelijke toegang tot](/azure/active-directory/active-directory-conditional-access-azure-portal) Azure AD configureren op basis van een groep, locatie en toepassings gevoeligheid voor SaaS-apps en Azure AD-verbonden apps.

**Aanbevolen procedure**: Verouderde verificatie protocollen blok keren.
**Details**: Aanvallers maken misbruik van zwakke plekken in oudere protocollen elke dag, met name voor aanvallen met een wacht woord. Configureer voorwaardelijke toegang om verouderde protocollen te blok keren. Zie de video [Azure AD: Doe dit](https://www.youtube.com/watch?v=wGk0J4z90GI) voor meer informatie.

## <a name="enable-password-management"></a>Wachtwoordbeheer inschakelen

Als u meerdere tenants hebt of gebruikers in staat wilt stellen [hun eigen wacht woord opnieuw](../../active-directory/user-help/active-directory-passwords-update-your-own-password.md)in te stellen, is het belang rijk dat u het juiste beveiligings beleid gebruikt om misbruik te voor komen.

**Aanbevolen procedure**: Stel selfservice voor wachtwoord herstel (SSPR) in voor uw gebruikers.  
**Details**: Gebruik de Azure AD [self-service voor het opnieuw instellen van wacht woorden](/azure/active-directory-b2c/active-directory-b2c-reference-sspr) .

**Aanbevolen procedure**: Bewaak hoe of als SSPR werkelijk wordt gebruikt.  
**Details**: Bewaak de gebruikers die worden geregistreerd met behulp van het Azure AD- [rapport registratie activiteit voor het opnieuw instellen van het wacht woord](/azure/active-directory/active-directory-passwords-get-insights). De rapportage functie die door Azure AD wordt geboden, helpt u bij het beantwoorden van vragen met behulp van vooraf ontwikkelde rapporten. Als u een juiste licentie hebt, kunt u ook aangepaste query's maken.

**Aanbevolen procedure**: Breid het wachtwoord beleid voor de Cloud uit naar uw on-premises infra structuur.
**Details**: Verbeter het wachtwoord beleid in uw organisatie door dezelfde controles uit te voeren voor on-premises wachtwoord wijzigingen wanneer u wijzigingen aanbrengt in de Cloud op basis van wacht woorden. Installeer [Azure AD-wachtwoord beveiliging](/azure/active-directory/authentication/concept-password-ban-bad) voor Windows Server Active Directory-agents on-premises om geblokkeerde wachtwoord lijsten uit te breiden naar uw bestaande infra structuur. Gebruikers en beheerders die on-premises wacht woorden wijzigen, instellen of opnieuw instellen, moeten voldoen aan hetzelfde wachtwoord beleid als alleen-Cloud gebruikers.

## <a name="enforce-multi-factor-verification-for-users"></a>Multi-factor-verificatie afdwingen voor gebruikers

U kunt het beste verificatie in twee stappen vereisen voor al uw gebruikers. Dit omvat beheerders en andere gebruikers in uw organisatie die aanzienlijke gevolgen kunnen hebben als hun account is aangetast (bijvoorbeeld financiële officieren).

Er zijn meerdere opties voor het vereisen van verificatie in twee stappen. De beste optie voor u is afhankelijk van uw doelen, de Azure AD-editie die u uitvoert en uw licentie programma. Zie [verificatie in twee stappen vereisen voor een gebruiker](/azure/active-directory/authentication/howto-mfa-userstates) om de beste optie voor u te bepalen. Raadpleeg de pagina met prijzen voor [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) en [Azure multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) voor meer informatie over licenties en prijzen.

Hieronder vindt u opties en voor delen voor het inschakelen van verificatie in twee stappen:

**Optie 1**: [Schakel multi-factor Authentication in door de gebruikers status te wijzigen](../../active-directory/authentication/howto-mfa-userstates.md).   
**Voor deel**: Dit is de traditionele methode voor het vereisen van verificatie in twee stappen. Het werkt met [Azure multi-factor Authentication in de Cloud en Azure multi-factor Authentication-Server](/azure/active-directory/authentication/concept-mfa-whichversion). Wanneer u deze methode gebruikt, moeten gebruikers elke keer dat ze zich aanmelden een verificatie in twee stappen uitvoeren en het beleid voor voorwaardelijke toegang negeren.

Als u wilt weten waar multi-factor Authentication moet worden ingeschakeld, raadpleegt u [welke versie van Azure MFA het meest geschikt is voor mijn organisatie?](/azure/active-directory/authentication/concept-mfa-whichversion).

**Optie 2**: [Schakel multi-factor Authentication in met het beleid voor voorwaardelijke toegang](/azure/active-directory/authentication/howto-mfa-getstarted).
**Voor deel**: Met deze optie kunt u een verificatie met twee stappen onder specifieke voor waarden vragen door gebruik te maken van [voorwaardelijke toegang](/azure/active-directory/active-directory-conditional-access-azure-portal). Specifieke voor waarden kunnen gebruikers zich aanmelden vanaf verschillende locaties, niet-vertrouwde apparaten of toepassingen die u als riskant beschouwt. Als u specifieke voor waarden definieert waarvoor u verificatie in twee stappen nodig hebt, kunt u voor uw gebruikers geen constante vragen stellen, wat een onaangename gebruikers ervaring kan zijn.

Dit is de meest flexibele manier om verificatie in twee stappen voor uw gebruikers in te scha kelen. Het inschakelen van een beleid voor voorwaardelijke toegang werkt alleen voor Azure multi-factor Authentication in de Cloud en is een Premium-functie van Azure AD. Meer informatie over deze methode vindt u in [Azure multi-factor Authentication implementeren op basis](/azure/active-directory/authentication/howto-mfa-getstarted)van de Cloud.

**Optie 3**: Schakel multi-factor Authentication in met beleids regels voor voorwaardelijke toegang door de gebruikers-en aanmeldings Risico's van [Azure AD Identity Protection](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa)te evalueren.   
**Voor deel**: Met deze optie kunt u het volgende doen:

- Detecteer mogelijke beveiligings problemen die van invloed zijn op de identiteiten van uw organisatie.
- Automatische antwoorden configureren op gedetecteerde verdachte acties die zijn gerelateerd aan de identiteiten van uw organisatie.
- Onderzoek verdachte incidenten en onderneem passende maat regelen om deze op te lossen.

Deze methode maakt gebruik van de Azure AD Identity Protection risico evaluatie om te bepalen of verificatie in twee stappen is vereist op basis van gebruikers-en aanmeldings Risico's voor alle Cloud toepassingen. Voor deze methode is Azure Active Directory P2-licentie vereist. Meer informatie over deze methode vindt u in [Azure Active Directory Identity Protection](/azure/active-directory/identity-protection/overview).

> [!Note]
> Optie 1, waarbij multi-factor Authentication wordt ingeschakeld door de gebruikers status te wijzigen, overschrijft het beleid voor voorwaardelijke toegang. Omdat opties 2 en 3 gebruikmaken van het beleid voor voorwaardelijke toegang, kunt u de optie 1 niet gebruiken.

Organisaties die geen extra lagen van identiteits beveiliging toevoegen, zoals verificatie in twee stappen, zijn gevoeliger voor aanvallen met referentie diefstal. Een aanval met een referentie diefstal kan leiden tot inbreuk op gegevens.

## <a name="use-role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer gebruiken
Toegangs beheer voor cloud resources is van cruciaal belang voor alle organisaties die gebruikmaken van de Cloud. Met [op rollen gebaseerd toegangs beheer (RBAC)](/azure/role-based-access-control/overview)kunt u beheren wie toegang heeft tot Azure-resources, wat ze kunnen doen met deze resources en op welke gebieden ze toegang hebben.

Het aanwijzen van groepen of afzonderlijke rollen die verantwoordelijk zijn voor specifieke functies in azure, helpt Verwar ring te voor komen die kan leiden tot mensen-en automatiserings fouten die beveiligings Risico's veroorzaken. Het beperken van de toegang op basis van de beveiligings principes [](https://en.wikipedia.org/wiki/Principle_of_least_privilege) van de [nood zaak om te kennen](https://en.wikipedia.org/wiki/Need_to_know) , is van cruciaal belang voor organisaties die beveiligings beleid voor gegevens toegang willen afdwingen.

Uw beveiligings team moet inzicht hebben in uw Azure-resources om Risico's te kunnen beoordelen en oplossen. Als het beveiligings team operationele verantwoordelijkheden heeft, hebben ze extra machtigingen nodig om hun taken uit te voeren.

U kunt [RBAC](/azure/role-based-access-control/overview) gebruiken om machtigingen toe te wijzen aan gebruikers, groepen en toepassingen bij een bepaald bereik. Het bereik van een roltoewijzing kan een abonnement, een resource groep of een enkele resource zijn.

**Aanbevolen procedure**: Verdeel taken binnen uw team en ken alleen de hoeveelheid toegang toe aan gebruikers die ze nodig hebben om hun taken uit te voeren. In plaats van iedereen onbeperkte machtigingen te geven in uw Azure-abonnement of-resources, mag u alleen bepaalde acties in een bepaald bereik toestaan.
**Details**: Gebruik [Ingebouwde RBAC-rollen](/azure/role-based-access-control/built-in-roles) in azure om machtigingen toe te wijzen aan gebruikers.

> [!Note]
> Specifieke machtigingen maken overbodige complexiteit en verwar ring en kunnen worden gecumuleerd in een ' legacy ' configuratie die moeilijk te herstellen is zonder dat u iets hoeft te verbreken. Vermijd resource-specifieke machtigingen. Gebruik in plaats daarvan beheer groepen voor machtigingen voor het hele onderneming en resource groepen voor de bevoegdheden binnen abonnementen. Vermijd gebruikersspecifieke machtigingen. Wijs in plaats daarvan toegang toe aan groepen in azure AD.

**Aanbevolen procedure**: Verleen beveiligings teams met Azure-verantwoordelijkheden toegang om Azure-resources te bekijken zodat ze Risico's kunnen beoordelen en oplossen.
**Details**: Verleen beveiligings teams de rol RBAC- [beveiligings lezer](/azure/role-based-access-control/built-in-roles#security-reader) . U kunt de hoofd beheer groep of de segment beheer groep gebruiken, afhankelijk van het bereik van verantwoordelijkheden:

- **Hoofd beheer groep** voor teams die verantwoordelijk zijn voor alle ondernemings resources
- **Segment beheer groep** voor teams met een beperkt bereik (doorgaans door wettelijke of andere grenzen van de organisatie)

**Aanbevolen procedure**: Verleen de juiste machtigingen aan beveiligings teams die directe operationele verantwoordelijkheden hebben.
**Details**: Controleer de ingebouwde RBAC-rollen voor de juiste roltoewijzing. Als de ingebouwde rollen niet voldoen aan de specifieke behoeften van uw organisatie, kunt u [aangepaste rollen maken voor Azure-resources](/azure/role-based-access-control/custom-roles). Net als bij ingebouwde rollen kunt u aangepaste rollen toewijzen aan gebruikers, groepen en service-principals bij het abonnement, de resource groep en het bereik van de resource.

**Aanbevolen procedures**: Ken Azure Security Center toegang toe aan beveiligings rollen die deze nodig hebben. Met Security Center kunnen beveiligings teams snel Risico's identificeren en oplossen.
**Details**: Voeg beveiligings teams met deze behoeften toe aan de RBAC-beveiligingsrol voor [beveiliging](/azure/role-based-access-control/built-in-roles#security-admin) , zodat ze beveiligings beleid kunnen bekijken, beveiligings status weer geven, beveiligings beleid bewerken, waarschuwingen en aanbevelingen weer geven en waarschuwingen en aanbevelingen negeren. U kunt dit doen met behulp van de hoofd beheer groep of de segment beheer groep, afhankelijk van het bereik van verantwoordelijkheden.

Organisaties die geen gegevens toegangs beheer afdwingen met behulp van mogelijkheden zoals RBAC, kunnen meer bevoegdheden geven dan nodig zijn voor hun gebruikers. Dit kan leiden tot inbreuk op gegevens door gebruikers toe te staan toegang te krijgen tot gegevens typen (bijvoorbeeld een hoge impact op het bedrijf).

## <a name="lower-exposure-of-privileged-accounts"></a>Minder bloot stelling van accounts met bevoegdheden

Het beveiligen van bevoegde toegang is een kritieke eerste stap bij het beveiligen van bedrijfs middelen. Het minimaliseren van het aantal personen die toegang hebben tot beveiligde informatie of resources vermindert de kans dat een kwaadwillende gebruiker toegang krijgt of een geautoriseerde gebruiker per ongeluk een gevoelige bron heeft beïnvloed.

Geprivilegieerde accounts zijn accounts die IT-systemen beheren en beheren. Cyber aanvallers richten zich op deze accounts om toegang te krijgen tot de gegevens en systemen van een organisatie. Voor het beveiligen van bevoegde toegang moet u de accounts en systemen isoleren van het risico dat ze worden blootgesteld aan een kwaadwillende gebruiker.

We raden u aan een plan te ontwikkelen en te volgen om bevoegde toegang te beveiligen tegen Cyber aanvallers. Voor informatie over het maken van een gedetailleerd schema voor het beveiligen van identiteiten en toegang die worden beheerd of gerapporteerd in azure AD, Microsoft Azure, Office 365 en andere Cloud Services, raadpleegt u de [beveiliging van bevoegde toegang voor hybride en Cloud implementaties in azure AD ](/azure/active-directory/users-groups-roles/directory-admin-roles-secure).

Hieronder vindt u een overzicht van de aanbevolen procedures [voor het beveiligen van bevoegde toegang voor hybride en Cloud implementaties in azure AD](/azure/active-directory/users-groups-roles/directory-admin-roles-secure):

**Aanbevolen procedure**: Toegang tot geprivilegieerde accounts beheren, controleren en bewaken.   
**Details**: Schakel [Azure AD privileged Identity Management](/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)in. Nadat u Privileged Identity Management hebt ingeschakeld, ontvangt u een e-mail bericht met meldingen voor de gewijzigde rol van de toegang. Deze meldingen geven een vroegtijdige waarschuwing wanneer extra gebruikers worden toegevoegd aan rollen met zeer privileged in uw Directory.

**Aanbevolen procedure**: Zorg ervoor dat alle essentiële beheerders accounts worden beheerd met Azure AD-accounts.
**Details**: Verwijder alle consumenten accounts uit essentiële beheerders rollen (bijvoorbeeld micro soft-accounts zoals hotmail.com, live.com en outlook.com).

**Aanbevolen procedure**: Zorg ervoor dat alle belang rijke beheerders rollen een apart account hebben voor beheer taken om te voor komen dat phishing en andere aanvallen zich kunnen voordoen.
**Details**: Maak een afzonderlijk beheerders account dat is toegewezen aan de bevoegdheden die nodig zijn om de beheer taken uit te voeren. Blok keer het gebruik van deze beheerders accounts voor dagelijkse productiviteits Programma's, zoals Microsoft Office 365-e-mail of een wille keurige webbrowser.

**Aanbevolen procedure**: Bepaal en Categoriseer accounts die zich in rollen met een hoge bevoegdheden bevinden.   
**Details**: Nadat u Azure AD Privileged Identity Management hebt ingeschakeld, bekijkt u de gebruikers die zich in de globale beheerder, beheerdersrol en andere uiterst privilegede rollen bevinden. Verwijder alle accounts die niet meer nodig zijn voor deze rollen en Categoriseer de overige accounts die aan beheerders rollen zijn toegewezen:

- Individueel toegewezen aan gebruikers met beheerders rechten en kunnen worden gebruikt voor niet-administratieve doel einden (bijvoorbeeld persoonlijk e-mail adres)
- Individueel toegewezen aan gebruikers met beheerders rechten en alleen voor administratieve doel einden worden aangewezen
- Gedeeld door meerdere gebruikers
- Voor scenario's voor nood toegang
- Voor geautomatiseerde scripts
- Voor externe gebruikers

**Aanbevolen procedure**: Implementeer de just-in-time-toegang (JIT) om de belichtings tijd van bevoegdheden verder te verlagen en verbeter uw zicht baarheid in het gebruik van geprivilegieerde accounts.   
**Details**: Met Azure AD Privileged Identity Management kunt u het volgende doen:

- Beperk gebruikers tot het uitvoeren van de JIT-bevoegdheden.
- Wijs rollen toe voor een verkorte duur met vertrouwen dat de bevoegdheden automatisch worden ingetrokken.

**Aanbevolen procedure**: Definieer ten minste twee nood toegangs accounts.   
**Details**: Met accounts voor toegang in een nood geval kunnen organisaties bevoorrechte toegang in een bestaande Azure Active Directory omgeving beperken. Deze accounts zijn zeer privileged en worden niet toegewezen aan specifieke personen. Accounts voor toegang in nood gevallen zijn beperkt tot scenario's waarin normale beheerders accounts niet kunnen worden gebruikt. Organisaties moeten het gebruik van het calamiteiten account beperken tot alleen de benodigde hoeveelheid tijd.

Evalueer de accounts die zijn toegewezen of die in aanmerking komen voor de rol van globale beheerder. Als u geen alleen-Cloud accounts met behulp van het `*.onmicrosoft.com` domein (bedoeld voor nood toegang) ziet, maakt u deze. Zie [Beheer accounts voor nood toegang beheren in azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access)voor meer informatie.

**Aanbevolen procedure**: Een "sche glasproces" in het geval van nood gevallen hebben.
**Details**: Volg de stappen in het [beveiligen van bevoegde toegang voor hybride en Cloud implementaties in azure AD](/azure/active-directory/users-groups-roles/directory-admin-roles-secure).

**Aanbevolen procedure**: Vereisen dat alle essentiële beheerders accounts een wacht woord hebben (voor keur) of multi-factor Authentication vereisen.
**Details**: Gebruik de [Microsoft Authenticator-app](/azure/active-directory/authentication/howto-authentication-phone-sign-in) om u aan te melden bij een Azure ad-account zonder een wacht woord te gebruiken. Net als [Windows hello voor bedrijven](/windows/security/identity-protection/hello-for-business/hello-identity-verification)gebruikt de Microsoft Authenticator verificatie op basis van een sleutel om een gebruikers referentie in te scha kelen die is gekoppeld aan een apparaat en gebruikmaakt van biometrische verificatie of een pincode.

Azure multi-factor Authentication vereisen bij aanmelding voor alle afzonderlijke gebruikers die permanent zijn toegewezen aan een of meer van de Azure AD-beheerders rollen: Globale beheerder, beheerder van geprivilegieerde rol, Exchange Online beheerder en share point online-beheerder. Schakel [multi-factor Authentication in voor uw beheerders accounts](/azure/active-directory/authentication/howto-mfa-userstates) en zorg ervoor dat de gebruikers van het beheerders account zijn geregistreerd.

**Aanbevolen procedure**: Voor kritieke beheerders accounts beschikt u over een beheer werkstation waar productie taken niet zijn toegestaan (bijvoorbeeld bladeren en e-mail). Hiermee worden uw beheerders accounts beschermd tegen aanvals vectoren die Browse en e-mail gebruiken en het risico van een belang rijk incident aanzienlijk verminderen.
**Details**: Gebruik een beheer werkstation. Kies een niveau voor beveiliging van werk station:

- Zeer veilige productiviteits apparaten bieden geavanceerde beveiliging voor surfen en andere productiviteits taken.
- [Privileged Access workstations (paw's)](/windows-server/identity/securing-privileged-access/privileged-access-workstations) bieden een speciaal besturings systeem dat wordt beschermd tegen aanvallen via internet en bedreigings vectoren voor gevoelige taken.

**Aanbevolen procedure**: De inrichting van beheerders accounts ongedaan maken wanneer werk nemers uw organisatie verlaten.
**Details**: Beschikken over een proces dat beheerders accounts uitschakelt of verwijdert wanneer werk nemers uw organisatie verlaten.

**Aanbevolen procedure**: Beheerders accounts regel matig testen met behulp van de huidige aanvals technieken.
**Details**: Gebruik Office 365-aanvals Simulator of een aanbieding van derden om realistische aanvals scenario's uit te voeren in uw organisatie. Dit kan u helpen kwets bare gebruikers te vinden voordat een echte aanval plaatsvindt.

**Aanbevolen procedure**: Onderneem stappen voor het oplossen van de meest gebruikte aanvals technieken.  
**Details**: [Micro soft-accounts in beheerders rollen identificeren die moeten worden overgeschakeld naar werk-of school accounts](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Zorg ervoor dat afzonderlijke gebruikers accounts en e-mail berichten worden doorgestuurd voor globale beheerders accounts](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)  

[Zorg ervoor dat de wacht woorden van de beheerders accounts onlangs zijn gewijzigd](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Wachtwoord hash synchronisatie inschakelen](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#turn-on-password-hash-synchronization)  

[Multi-factor Authentication vereisen voor gebruikers in alle geprivilegieerde rollen en gebruikers die ze kunnen belichten](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Uw Office 365 Secure score verkrijgen (als u Office 365 gebruikt)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#obtain-your-office-365-secure-score-if-using-office-365)  

[Raadpleeg de richt lijnen voor beveiliging en naleving van Office 365 (als u Office 365 gebruikt)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Bewaking van Office 365-activiteiten configureren (als u Office 365 gebruikt)](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#configure-office-365-activity-monitoring-if-using-office-365)  

[Eigen aren van incidenten/nood-antwoord plannen vastleggen](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#establish-incidentemergency-response-plan-owners)  

[On-premises geprivilegieerde beheerders accounts beveiligen](/azure/active-directory/users-groups-roles/directory-admin-roles-secure#turn-on-password-hash-synchronization)

Als u geen beveiligde toegang hebt, is het mogelijk dat u te veel gebruikers hebt in zeer beschermde rollen en kwetsbaarder is voor aanvallen. Kwaadwillende actoren, met inbegrip van Cyber aanvallers, vaak doel beheerders accounts en andere elementen van bevoegde toegang om toegang te krijgen tot gevoelige gegevens en systemen door referentie diefstal te gebruiken.

## <a name="control-locations-where-resources-are-created"></a>Controle locaties waar resources worden gemaakt

Het is belang rijk om Cloud operators in te scha kelen om taken uit te voeren en te voor komen dat ze conventies voor het beheren van de resources van uw organisatie verbreken. Organisaties die de locaties willen bepalen waar resources worden gemaakt, moeten deze locaties op de vaste wijze coderen.

U kunt [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) gebruiken om beveiligings beleid te maken waarvan de definities de acties of resources beschrijven die specifiek worden geweigerd. U wijst deze beleids definities toe aan het gewenste bereik, zoals het abonnement, de resource groep of een afzonderlijke resource.

> [!NOTE]
> Beveiligings beleid is niet hetzelfde als RBAC. Ze gebruiken in feite RBAC om gebruikers te autoriseren om deze resources te maken.
>
>

Organisaties die niet bepalen hoe resources worden gemaakt, zijn gevoeliger voor gebruikers die de service kunnen misbruiken door meer resources te maken dan ze nodig hebben. Het proces voor het maken van resources is een belang rijke stap bij het beveiligen van een scenario met meerdere tenants.

## <a name="actively-monitor-for-suspicious-activities"></a>Actief controleren op verdachte activiteiten

Een actief identiteits bewakings systeem kan snel verdacht gedrag detecteren en een waarschuwing activeren voor verder onderzoek. De volgende tabel bevat twee Azure AD-mogelijkheden die organisaties kunnen helpen hun identiteiten te controleren:

**Aanbevolen procedure**: Een methode voor het identificeren van:

- Er wordt geprobeerd om u aan te melden [zonder te worden](/azure/active-directory/active-directory-reporting-sign-ins-from-unknown-sources)getraceerd.
- [](/azure/active-directory/active-directory-reporting-sign-ins-after-multiple-failures) Beveiligings aanvallen tegen een bepaald account.
- Probeert zich aan te melden vanaf meerdere locaties.
- Aanmeldingen vanaf [geïnfecteerde apparaten](/azure/active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices).
- Verdachte IP-adressen.

**Details**: Gebruik Azure AD Premium [anomalie rapporten](/azure/active-directory/active-directory-view-access-usage-reports). Beschikken over processen en procedures voor IT-beheerders om deze rapporten dagelijks of op aanvraag uit te voeren (meestal in een scenario met een incident respons).

**Aanbevolen procedure**: Beschikken over een actief bewakings systeem dat u op de hoogte brengt van Risico's en het risico niveau (hoog, gemiddeld of laag) kunt aanpassen aan uw bedrijfs vereisten.   
**Details**: Gebruik [Azure AD Identity Protection](/azure/active-directory/active-directory-identityprotection), waarmee de huidige Risico's van een eigen dash board worden gemarkeerd en dagelijkse samenvattings meldingen per e-mail worden verzonden. Als u de identiteiten van uw organisatie wilt helpen beveiligen, kunt u op risico gebaseerd beleid configureren dat automatisch reageert op gedetecteerde problemen wanneer een opgegeven risico niveau wordt bereikt.

Organisaties die hun identiteits systemen niet actief bewaken, lopen een risico dat gebruikers referenties worden aangetast. Zonder kennis dat verdachte activiteiten door deze referenties worden uitgevoerd, kunnen organisaties dit type bedreiging niet oplossen.

## <a name="use-azure-ad-for-storage-authentication"></a>Azure AD gebruiken voor opslag verificatie
[Azure Storage](/azure/storage/common/storage-auth-aad) ondersteunt verificatie en autorisatie met Azure AD voor Blob Storage en Queue Storage. Met Azure AD-verificatie kunt u het toegangs beheer op basis van rollen gebruiken om specifieke machtigingen voor gebruikers, groepen en toepassingen te verlenen aan het bereik van een afzonderlijke BLOB-container of-wachtrij.

We raden u [aan Azure ad te gebruiken voor het verifiëren van de toegang tot opslag](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/).

## <a name="next-step"></a>Volgende stap

Zie [Aanbevolen procedures en patronen voor Azure-beveiliging](best-practices-and-patterns.md) voor meer aanbevolen procedures voor beveiliging bij het ontwerpen, implementeren en beheren van uw cloud oplossingen met behulp van Azure.
