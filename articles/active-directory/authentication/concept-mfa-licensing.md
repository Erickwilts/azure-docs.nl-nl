---
title: Azure MFA-versies en verbruiksabonnementen - Azure Active Directory
description: Informatie over de multi-factor Authentication-client en de verschillende methoden en versies die beschikbaar zijn.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a1ee55dd3aebca869da47bbc994f546aa4fe528
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496773"
---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Over het verkrijgen van Azure multi-factor Authentication

Als het gaat om het beveiligen van uw accounts, moet verificatie in twee stappen standaard in uw organisatie. Deze functie is vooral belangrijk voor accounts met toegang tot bronnen uitgebreide. Microsoft biedt eenvoudige verificatie in twee stappen verificatie functies aan Office 365 om die reden en beheerders voor Azure Active Directory (Azure AD) zonder extra kosten. Als u wilt upgraden van de functies voor uw beheerders of uitbreiden van verificatie in twee stappen voor de rest van uw gebruikers, kunt u de Azure multi-factor Authentication op verschillende manieren kunt kopen.

> [!IMPORTANT]
> In dit artikel is bedoeld als richtlijn om te begrijpen van de verschillende manieren voor het kopen van Azure multi-factor Authentication. Voor specifieke informatie over prijzen en facturering, u moet altijd verwijzen naar de [multi-factor Authentication-pagina met prijzen](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).
>

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Beschikbare versies van Azure multi-factor Authentication

De volgende tabel beschrijft de verschillen tussen de drie versies van multi-factor authentication:

| Versie | Description |
| --- | --- |
| Multi-factor Authentication voor Office 365 <br> Microsoft 365 Business | Deze versie wordt beheerd via de Office 365 of Microsoft 365-portal. Beheerders kunnen [beveiligen van Office 365-resources met verificatie in twee stappen](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Deze versie is onderdeel van een abonnement op Office 365 of Microsoft 365 Business. |
| Multi-factor Authentication voor Azure AD-beheerders | Gebruikers die zijn toegewezen de rol globale beheerder van Azure AD in Azure AD-tenants kunt verificatie in twee stappen zonder extra kosten. |
| Azure Multi-Factor Authentication | Azure multi-factor Authentication is vaak aangeduid als de 'volledige' versie, biedt de uitgebreidste set mogelijkheden. Het biedt aanvullende configuratie-opties via de [Azure-portal](https://portal.azure.com), geavanceerde rapportage en ondersteuning voor een bereik van on-premises en cloudtoepassingen. Azure multi-factor Authentication is een functie van [Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features). |

> [!NOTE]
> Nieuwe klanten kunnen niet meer Azure multi-factor Authentication aanschaffen als zelfstandige aanbieding met 1 September 2018. Meervoudige verificatie blijft beschikbaar als een functie in Azure AD Premium-licenties.

## <a name="feature-comparison-of-versions"></a>Vergelijking van versies

De volgende tabel geeft een lijst van de functies die beschikbaar in de verschillende versies van Azure multi-factor Authentication zijn.

> [!NOTE]
> Deze van de vergelijkingstabel worden de functies die deel van elke versie van multi-factor Authentication uitmaken besproken. Als u de volledige Azure multi-factor Authentication-service hebt, sommige functies mogelijk niet beschikbaar, afhankelijk van of u gebruiken [MFA in de cloud of MFA on-premises](concept-mfa-whichversion.md).
>

| Functie | Multi-factor Authentication voor Office 365 | Multi-factor Authentication voor Azure AD-beheerders | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Beveiligen van Azure AD-beheerdersaccounts met MFA |● |● (alleen in een globale beheerder van Azure AD-accounts) |● |
| Mobiele app als een tweede factor |● |● |● |
| Telefoonoproep als tweede factor |● |● |● |
| SMS als een tweede factor |● |● |● |
| App-wachtwoorden voor clients die geen ondersteuning voor MFA bieden |● |● |● |
| Beheerdercontrole over verificatiemethoden |● |● |● |
| Beveiligen van niet-beheerdersaccounts met MFA |● | |● |
| Pincodemodus | | |● |
| Fraudewaarschuwing | | |● |
| MFA-rapporten | | |● |
| Eenmalige toegang | | |● |
| Aangepaste begroeting voor telefoongesprekken | | |● |
| Aangepaste Nummerweergave voor telefoongesprekken | | |● |
| Goedgekeurde IP-adressen | | |● |
| MFA herinneren voor vertrouwde apparaten |● |● |● |
| MFA voor on-premises toepassingen | | |● |

> [!IMPORTANT]
> Vanaf maart van 2019 de opties telefoongesprek zich niet beschikbaar voor gebruikers voor MFA en Self-service voor Wachtwoordherstel in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefonische oproep blijft beschikbaar voor gebruikers in betaalde Azure AD-tenants. Deze wijziging geldt alleen voor gratis/proefversie van Azure AD-tenants.

## <a name="how-to-turn-on-azure-multi-factor-authentication-for-azure-ad-administrators"></a>Het inschakelen van Azure multi-factor Authentication voor beheerders van Azure AD

Gebruikers die zijn toegewezen de rol globale beheerder in Azure AD-tenants kunt verificatie in twee stappen voor de globale beheerder van Azure AD-accounts zonder extra kosten. Als u van een Microsoft-Account gebruikmaakt, kunt u registreren voor meervoudige verificatie met behulp van de richtlijnen in het ondersteuningsartikel voor Microsoft-account gevonden [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification). Als u een Microsoft-Account niet gebruikt, schakelt u multi-factor authentication voor globale beheerders volgens de richtlijnen die zijn gevonden in het artikel [hoe u verificatie in twee stappen vereist voor een gebruiker of groep](howto-mfa-userstates.md).

## <a name="how-to-purchase-azure-multi-factor-authentication"></a>Over het aanschaffen van Azure multi-factor Authentication

Licenties die Azure multi-factor Authentication, zoals Azure Active Directory Premium of een licentie-bundel met Azure AD Premium of voorwaardelijke toegang en deze toewijzen aan uw gebruikers in Azure Active Directory.

### <a name="consumption-based-licensing"></a>Licentieverlening op basis van gebruik

Licenties op basis van verbruik van is niet meer beschikbaar voor nieuwe klanten vanaf 1 September 2018.

Vanaf 1 September 2018 nieuwe auth-providers kunnen niet meer worden gemaakt. Bestaande auth-providers kunnen nog steeds worden gebruikt en bijgewerkt. Meervoudige verificatie wordt nog steeds een beschikbare functie in Azure AD Premium-licenties.

Wanneer u een Azure multi-factor Authentication-Provider gebruikt, zijn er twee gebruiksmodellen beschikbaar die worden in rekening gebracht via uw Azure-abonnement:

1. **Per ingeschakelde gebruiker** - voor ondernemingen die verificatie in twee stappen voor een vast aantal werknemers die regelmatig verificatie moeten inschakelen. Facturering per gebruiker is gebaseerd op het aantal gebruikers dat is ingeschakeld voor MFA in uw Azure AD-tenant en uw Azure MFA-Server. Als gebruikers zijn ingeschakeld voor MFA in zowel Azure AD en Azure MFA-Server en domein synchroniseren (Azure AD Connect) is ingeschakeld, en we tellen het grotere set van gebruikers. Als synchronisatie van domein is niet ingeschakeld, wordt dat we de som van alle gebruikers die zijn ingeschakeld voor MFA in Azure AD tellen en Azure MFA-Server. Facturering is Pro rata berekend en dagelijks gerapporteerd aan de Commerce-systeem.

   > [!NOTE]
   > Facturering-voorbeeld 1: U hebt 5000 gebruikers dat is ingeschakeld voor MFA vandaag nog. Het systeem MFA deelt dit getal door 31 en 161.29 gebruikers van rapporten voor die dag. Morgen dat u 15 meer gebruikers, zodat het systeem MFA 161.77 gebruikers voor die dag rapporten. Aan het einde van de factureringscyclus, wordt het totale aantal gebruikers in rekening gebracht op basis van uw Azure-abonnement maximaal toegevoegd rond 5000.
   >
   > Facturering-voorbeeld 2: U hebt een combinatie van gebruikers met licenties en gebruikers zonder, zodat u beschikt over een per gebruiker Azure MFA-Provider om het verschil. Er zijn 4500 Enterprise Mobility + Security-licenties in uw tenant, maar 5000 gebruikers dat is ingeschakeld voor MFA. Uw Azure-abonnement wordt gefactureerd voor 500 gebruikers, pro rata berekend en dagelijks gerapporteerd als 16.13 gebruikers.
   >

1. **Per verificatie** - voor ondernemingen die verificatie in twee stappen voor een grote groep met gebruikers die niet vaak nodig verificatie inschakelen. Facturering is gebaseerd op het aantal aanvragen verificatie in twee stappen, ongeacht of deze verificaties slagen of wordt geweigerd. Deze factureringsmethode wordt weergegeven op uw gebruik van Azure-instructie in de packs van 10 verificaties en elke dag wordt gerapporteerd.

   > [!NOTE]
   > Facturering voorbeeld 3: De Azure MFA-service ontvangen vandaag de dag 3,105 verificatie in twee stappen verwerken. Uw Azure-abonnement, wordt in rekening gebracht voor 310.5 packs voor verificatie.
   >

Het is belangrijk te weten dat u kunt licenties hebt, maar nog steeds wordt gefactureerd voor de configuratie op basis van verbruik. Als u een per authenticatie Azure MFA-Provider hebt ingesteld, wordt u gefactureerd voor elke aanvraag van de verificatie in twee stappen ook die door gebruikers die licenties hebben uitgevoerd. Als u een per gebruiker Azure MFA-Provider hebt ingesteld op een domein dat niet is gekoppeld aan uw Azure AD-tenant, wordt u gefactureerd per ingeschakelde gebruiker, zelfs als uw gebruikers licenties in Azure AD hebben.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer prijsinformatie [prijzen voor Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Kies of u wilt implementeren, Azure MFA [in de cloud of on-premises](concept-mfa-whichversion.md)
