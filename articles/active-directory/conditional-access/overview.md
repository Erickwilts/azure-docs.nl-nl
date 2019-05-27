---
title: Wat is voorwaardelijke toegang in Azure Active Directory? | Microsoft Docs
description: Leer hoe u met voorwaardelijke toegang in Azure Active Directory geautomatiseerde beslissingen voor toegang kunt implementeren die niet alleen zijn gebaseerd op wie er toegang tot een resource wil hebben, maar ook op de manier waarop een resource wordt geopend.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32ad8c12834ee538e231b38f9098c741fdc17954
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65997253"
---
# <a name="what-is-conditional-access-in-azure-active-directory"></a>Wat is voorwaardelijke toegang in Azure Active Directory?

Beveiliging is een topprioriteit voor organisaties die gebruikmaken van de cloud. Als het om het beheren van uw cloudresources gaat, vormen identiteit en toegang belangrijke aspecten bij het beveiligen van de cloud. In een wereld waar mobiliteit en cloud belangrijke begrippen zijn, kunnen gebruikers overal toegang krijgen tot de resources van uw organisatie met behulp van diverse apparaten en apps. Dus het is niet meer voldoende als u zich alleen richt op wie er toegang tot een resource mogen hebben. Voor de juiste balans tussen beveiliging en productiviteit dient u ook rekening te houden met de manier waarop een resource wordt geopend door middel van een beslissing voor toegangsbeheer. Met voorwaardelijke toegang van Azure Active Directory (Azure AD) kunt u dit vereiste aanpakken. Voorwaardelijke toegang is een functionaliteit van Azure Active Directory. Met voorwaardelijke toegang kunt u geautomatiseerde beslissingen voor toegangsbeheer implementeren voor het openen van uw cloud-apps op basis van voorwaarden.

Beleid voor voorwaardelijke toegang wordt afgedwongen nadat de verificatie van de eerste factor is voltooid. Daarom is voorwaardelijke toegang niet zozeer bedoeld als een eerste verdedigingslinie voor bijvoorbeeld DoS-aanvallen (Denial of Service), maar kan het gebruikmaken van signalen van dergelijke gebeurtenissen (bijvoorbeeld het niveau van aanmeldingsrisico, de locatie van de aanvraag, enzovoort) om wel of geen toegang te verlenen.  

![Besturingselement](./media/overview/81.png)

In dit artikel wordt een conceptueel overzicht gegeven van voorwaardelijke toegang in Azure AD.

## <a name="common-scenarios"></a>Algemene scenario's

In een wereld waar mobiliteit en cloud belangrijke begrippen zijn maakt Azure Active Directory vanaf een willekeurige locatie eenmalige aanmelding mogelijk voor apparaten, apps en services. Als gevolg van een toenemend aantal apparaten (waaronder BYOD), werken buiten de organisatie en SaaS-apps van derden, hebt u te maken met twee tegengestelde doelen:

- Geef gebruikers de mogelijkheid overal en altijd productief te zijn
- Beveilig de zakelijke bezittingen - op elk moment

Door gebruik te maken van voorwaardelijk toegangsbeleid kunt u de juiste besturingselementen voor toegang toepassen onder de juiste voorwaarden. Voorwaardelijke toegang van Azure AD biedt aanvullende beveiliging alleen als het nodig is. In andere gevallen heeft de gebruiker er geen last van.

Hieronder vindt u enkele punten van zorg met betrekking tot toegang waarbij voorwaardelijke toegang u kan helpen:

- **[Aanmeldingsrisico](conditions.md#sign-in-risk)**: Azure AD Identity Protection detecteert aanmeldingsrisico's. Hoe beperkt u de toegang als een gedetecteerd aanmeldingsrisico op ongeautoriseerde toegang wijst? En als u sterker bewijs wilt hebben dat een aanmelding door een legitieme gebruiker is uitgevoerd? En als uw twijfels sterk genoeg zijn om zelfs bepaalde gebruikers de toegang tot een app te ontzeggen?  

- **[Netwerklocatie](location-condition.md)**: Azure AD is overal toegankelijk. Wat te doen als een toegangspoging wordt uitgevoerd vanaf een netwerklocatie die niet onder het beheer staat van uw IT-afdeling? Een gebruikersnaam in combinatie met een wachtwoord kan voldoende zijn als bewijs van identiteit voor toegang tot uw bedrijfsnetwerk. Maar wat te doen als u een sterker bewijs van de identiteit wilt bij toegangspogingen uit andere, onverwachte landen of regio's? En als u zelfs toegangspogingen wilt blokkeren vanaf bepaalde locaties?  

- **[Apparaatbeheer](conditions.md#device-platforms)**: In Azure AD hebben gebruikers toegang tot cloud-apps vanaf een groot aantal apparaten, waaronder mobiele en persoonlijke apparaten. Maar stel dat u eist dat toegangspogingen alleen mogen worden uitgevoerd met apparaten die door de IT-afdeling worden beheerd? En als u zelfs bepaalde typen apparaten de toegang tot cloud-apps in uw omgeving wilt ontzeggen?

- **[Clienttoepassing](conditions.md#client-apps)**: Tegenwoordig is toegang tot talloze cloud-apps mogelijk met verschillende typen apps, zoals web-apps, mobiele apps of bureaublad-apps. Maar als er nu een toegangspoging wordt ondernomen met een type client-app die bekende problemen veroorzaakt? Stel dat u voor bepaalde typen apps een apparaat nodig hebt dat door de IT-afdeling wordt beheerd?

Deze vragen en de antwoorden erop hebben te maken met veelvoorkomende toegangsscenario's met voorwaardelijke toegang van Azure AD.
Voorwaardelijke toegang is een functionaliteit van Azure Active Directory waardoor u toegangsscenario's kunt beheren met een op beleid gerichte aanpak.

> [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

## <a name="conditional-access-policies"></a>Voorwaardelijk toegangsbeleid

Voorwaardelijk toegangsbeleid is de naam voor een toegangsscenario waarbij gebruik wordt gemaakt van het volgende patroon:

![Besturingselement](./media/overview/10.png)

**Ga als volgt te werk** geeft de reactie van het beleid aan. Merk op dat het doel van voorwaardelijk toegangsbeleid is om geen toegang tot een cloud-app te verlenen. In Azure AD is het verlenen van toegang tot cloud-apps onderhevig aan gebruikerstoewijzingen. Met voorwaardelijk toegangsbeleid bepaalt u hoe gemachtigde gebruikers (gebruikers die toegang tot een cloud-app is verleend) cloud-apps onder bepaalde voorwaarden mogen openen. In uw respons dwingt u aanvullende vereisten af, zoals bijvoorbeeld meervoudige verificatie of een beheerd apparaat. Binnen de context van voorwaardelijke toegang van Azure AD worden de door het beleid afgedwongen vereisten besturingselementen voor toegang genoemd. In zijn meest beperkende vorm kan het beleid de toegang blokkeren. Zie [Besturingselementen voor toegang in voorwaardelijke toegang van Azure Active Directory](controls.md) voor meer informatie.

**Als dit gebeurt** definieert de reden voor het activeren van het beleid. De reden wordt gekenmerkt als een groep voorwaarden waaraan is voldaan. In voorwaardelijke toegang van Azure AD spelen de twee toewijzingsvoorwaarden een speciale rol:

- **[Gebruikers](conditions.md#users-and-groups)**: De gebruikers die een toegangspoging uitvoeren (**Wie**).

- **[Cloud-apps](conditions.md#cloud-apps-and-actions)**: De doelen van een toegangspoging (**Wat**).

Deze twee voorwaarden zijn verplicht in een voorwaardelijk toegangsbeleid. Naast de twee verplichte voorwaarden, kunt u ook aanvullende voorwaarden opnemen. Deze beschrijven hoe de toegangspoging wordt uitgevoerd. Bekende voorbeelden zijn het gebruik van mobiele apparaten of locaties buiten uw bedrijfsnetwerk. Zie [Voorwaarden in Azure Active Directory](conditions.md) voor meer informatie.

De combinatie van voorwaarden en uw besturingselementen voor toegang stellen een beleid voor voorwaardelijke toegang voor.

![Beheer](./media/overview/51.png)

Met voorwaardelijke toegang van Azure AD kunt u bepalen hoe gemachtigde gebruikers uw cloud-apps kunnen openen. Het doel van een beleid voor voorwaardelijke toegang is om aanvullende besturingselementen voor toegang af te dwingen bij een toegangspoging tot een cloud-app op basis van de manier waarop de poging wordt uitgevoerd.

Een benadering op basis van beleid voor het beveiligen van toegang tot uw cloud-apps biedt u de mogelijkheid de beleidsvereisten voor uw omgeving te gaan schetsen aan de hand van de structuur die in dit artikel uiteen wordt gezet en zonder dat u zich zorgen hoeft te maken over de technische implementatie.

## <a name="azure-ad-conditional-access-and-federated-authentication"></a>Voorwaardelijke toegang en federatieve verificatie van Azure Active Directory

Beleid voor voorwaardelijke toegang werkt naadloos met [federatieve verificatie](../../security/azure-ad-choose-authn.md#federated-authentication). Deze ondersteuning omvat alle ondersteunde voorwaarden en besturingselementen en inzicht in hoe beleid wordt toegepast op actieve gebruiker-aanmeldingen met behulp van [Azure AD-rapportage](../reports-monitoring/concept-sign-ins.md).

*Federatieve verificatie met Azure AD* betekent dat gebruikersverificatie bij Azure Active Directory wordt verwerkt door een vertrouwde verificatieservice. Een vertrouwde verificatieservice is bijvoorbeeld Active Directory Federation Services (AD FS) of een andere federatieservice. In deze configuratie wordt verificatie van de primaire gebruiker uitgevoerd bij de service en wordt vervolgens Azure Active Directory gebruikt voor aanmelding bij afzonderlijke toepassingen. Voorwaardelijke toegang van Azure Active Directory wordt toegepast voordat toegang wordt verleend tot de toepassing die de gebruiker wil openen. 

Wanneer het geconfigureerde beleid voor voorwaardelijke toegang meervoudige verificatie vereist, gebruikt Azure Active Directory standaard Azure MFA. Als u de federatieve service voor MFA gebruikt, kunt u Azure Active Directory configureren voor omleiding naar de federatieve services wanneer MFA nodig is door `-SupportsMFA` op `$true` in te stellen in [PowerShell](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings). Deze instelling werkt voor federatieve verificatieservices die ondersteuning bieden voor de aanvraag voor de MFA-uitdaging die door Azure Active Directory wordt uitgegeven met behulp van `wauth= http://schemas.microsoft.com/claims/multipleauthn`.

Nadat de gebruiker is aangemeld bij de federatieve verificatieservice, worden andere beleidsvereisten, zoals het nalevingsbeleid voor apparaten of een goedgekeurde toepassing, door Azure Active Directory verwerkt.

## <a name="license-requirements"></a>Licentievereisten

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Uw implementatie van voorwaardelijke toegang in Azure Active Directory plannen](plan-conditional-access.md) voor informatie over het implementeren van voorwaardelijke toegang tot uw omgeving.
