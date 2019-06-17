---
title: Over het integreren met Azure Active Directory | Microsoft Docs
description: Informatie over de voordelen van uw toepassing integreren met Azure Active Directory, en profiteer van resources functies zoals eenvoudige aanmelding, identiteitsbeheer, meervoudige verificatie, en toegangsbeheer.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/08/2019
ms.author: ryanwi
ms.reviewer: bryanla
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4af8cdc1072678de7e34458fc4b54d52aae9eff1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111599"
---
# <a name="integrating-with-azure-active-directory"></a>Integreren met Azure Active Directory

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

In dit artikel meer informatie over de voordelen van uw toepassing integreren met Azure Active Directory (Azure AD) en profiteer van integratie van resources. Azure AD biedt organisaties met zakelijke Identiteitsbeheer voor cloudtoepassingen. Integratie van Azure AD biedt uw gebruikers een gestroomlijnde ervaring aanmelden en uw toepassing in overeenstemming zijn met de IT-beleid.

## <a name="how-to-integrate"></a>Het integreren van

Er zijn verschillende manieren voor uw toepassing om te integreren met Azure AD. Maak gebruik van zo veel of weinig van deze scenario's zoals geschikt is voor uw toepassing.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Ondersteuning voor Azure AD als een manier om aan te melden bij uw toepassing

**Meld problemen verminderen en ondersteuning verlagen.** Uw gebruikers geen met behulp van Azure AD om aan te melden bij uw toepassing, een meer naam en wachtwoord te onthouden. Als ontwikkelaar hebt u een minder wachtwoord opslaan en beschermen. Omdat u niet hoeft voor het afhandelen van vergeten wachtwoord opnieuw instellen van wachtwoorden, kan een aanzienlijke besparingen die alleen zijn. Azure AD wordt gebruikt door aanmelden voor een aantal van's werelds meest populaire cloudtoepassingen, waaronder Office 365 en Microsoft Azure. Honderden miljoenen gebruikers van miljoenen organisaties, waarschijnlijk uw gebruiker is al aangemeld bij Azure AD. Meer informatie over [ondersteuning toegevoegd voor aanmelding bij Azure AD](authentication-scenarios.md).

**Aanmelding van vereenvoudigen voor uw toepassing.**  Tijdens de registratie voor uw toepassing, kan Azure AD essentiële informatie over een gebruiker verzenden zodat u kunt vooraf uw aanmelding formulier vullen of volledig elimineren. Gebruikers kunnen zich aanmelden voor uw toepassing met behulp van hun Azure AD-account via een vertrouwde toestemming ervaring die vergelijkbaar is met deze gevonden in sociale media- en mobiele toepassingen. Elke gebruiker kan zich registreren en aanmelden bij een toepassing die is geïntegreerd met Azure AD zonder tussenkomst van de IT. Meer informatie over [melden voor uw toepassing voor Azure AD-Account aanmelding](../../app-service/configure-authentication-provider-aad.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Zoeken naar gebruikers, inrichten van gebruikers beheren en toegang tot uw toepassing beheren

**Zoeken naar gebruikers in de directory.**  De Graph API gebruiken om u te helpen gebruikers zoeken naar en zoeken naar andere personen in hun organisatie wanneer anderen uitnodigen of verlenen van toegang, in plaats van dat ze naar het type e-adressen. Gebruikers kunnen bladeren met behulp van een bekend adres book stijlinterface, inclusief het weergeven van de details van de organisatie-hiërarchie. Meer informatie over de [Graph API](active-directory-graph-api.md).

**Active Directory-groepen en distributielijsten die al wordt beheerd door de klant opnieuw gebruiken.**  Azure AD bevat de groepen die de klant al wordt gebruikt voor e-maildistributie en toegang beheren. Met de Graph API, deze groepen in plaats van dat van uw klant maken en beheren van een afzonderlijke set met groepen in uw toepassing opnieuw gebruiken. Groepsgegevens kunnen ook worden verzonden aan uw toepassing in de aanmelding van tokens. Meer informatie over de [Graph API](active-directory-graph-api.md).

**Azure AD om te bepalen wie toegang tot uw toepassing heeft gebruiken.**  Beheerders en toepassingseigenaren in Azure AD kunnen de toegang tot toepassingen op specifieke gebruikers en groepen toewijzen. Met de Graph API, kunt u lezen van deze lijst en het beheren van de inrichting en ongedaan maken inrichting van resources en toegang in uw toepassing gebruiken.

**Gebruik Azure AD voor rollen gebaseerd toegangsbeheer.**  Beheerders en toepassingseigenaren van kunnen gebruikers en groepen toewijzen aan rollen die u definieert wanneer u uw toepassing in Azure AD registreren. Rol informatie wordt verzonden naar uw toepassing in tokens aanmelden en kan ook worden gelezen met de Graph API. Meer informatie over [met behulp van Azure AD voor autorisatie](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles/).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Toegang krijgen tot van gebruiker-profiel, agenda, e-mail, contactpersonen, bestanden en meer

**Azure AD is de autorisatie-server voor Office 365 en andere zakelijke services van Microsoft.**  Als u Azure AD voor aanmelden bij uw toepassing of de accounts van uw huidige gebruikers koppelen aan Azure AD-gebruikersaccounts met behulp van OAuth 2.0-ondersteuning ondersteunt, kunt u lees- en schrijftoegang tot het profiel van een gebruiker, agenda, e-mail, contactpersonen, bestanden en andere informatie aanvragen. U kunt naadloos gebeurtenissen schrijven naar de agenda van de gebruiker, en lezen of schrijven van bestanden naar hun OneDrive. Meer informatie over [toegang tot de Office 365 API's](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Promoot uw toepassing in Azure en Office 365-marktplaatsen

**Promoot uw toepassing op miljoenen organisaties die al van Azure AD gebruikmaken.**  Gebruikers die zoeken en deze marktplaatsen al gebruikmaakt van een of meer cloudservices, waardoor ze gekwalificeerde cloud service-klanten. Meer informatie over het promoveren van uw toepassing in [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Wanneer gebruikers zich aanmelden voor uw toepassing, wordt deze weergegeven in hun Azure AD-toegangspaneel en startprogramma voor Office 365-app.**  Gebruikers zich snel en eenvoudig terug naar uw toepassing later betrokkenheid van gebruikers verbeteren. Meer informatie over de [Azure AD-toegangspaneel](../user-help/active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Beveiligde communicatie van apparaat-naar-service en service-naar-service

**Met behulp van Azure AD voor identiteitsbeheer van services en apparaten de code hoeft vermindert te schrijven en kunnen IT-om toegang te beheren.**  Services en apparaten kunnen tokens verkrijgen van Azure AD met behulp van OAuth en gebruiken van deze tokens voor toegang tot web-API's. Met behulp van Azure AD kunt u voorkomen schrijven van complexe verificatiecode op te geven. Omdat de identiteit van de services en apparaten worden opgeslagen in Azure AD, IT-sleutels en intrekken van documenten op één locatie in plaats van dat hiervoor apart in uw toepassing kunt beheren.

## <a name="benefits-of-integration"></a>Voordelen van-integratie

Integratie met Azure AD wordt geleverd met voordelen die niet nodig hebt u extra code te schrijven.

### <a name="integration-with-enterprise-identity-management"></a>Integratie met enterprise-identiteitsbeheer

**Help uw toepassing te voldoen aan de IT-beleid.**  Organisaties hun onderneming identity management-systemen integreren met Azure AD, zodat wanneer een persoon die een organisatie verlaat, ze automatisch toegang tot uw toepassing zonder verliezen zullen IT hoeven extra stappen uitvoeren. IT kan beheren wie kan toegang tot uw toepassing en te bepalen welk toegangsbeleid zijn vereist - voor voorbeeld van de multi-factor authentication - verminderen de noodzaak code om te voldoen aan complexe bedrijfsbeleid te schrijven. Azure AD biedt beheerders met een gedetailleerd logboek van die aangemeld bij uw toepassing zodat IT kunt bijhouden.

**Azure AD uitbreiding van Active Directory naar de cloud zodat uw toepassing kan worden geïntegreerd met AD.**  Veel organisaties over de hele wereld Active Directory gebruiken als hun principal aanmelden en het beheersysteem voor identiteit en hun toepassingen werken met AD vereist. Uw app integreren met Azure AD is geïntegreerd met Active Directory.

### <a name="advanced-security-features"></a>Geavanceerde beveiligingsfuncties

**Meervoudige verificatie.**  Azure AD biedt systeemeigen meervoudige verificatie. IT-beheerders kunnen multi-factor authentication toegang tot uw toepassing, vereisen, zodat u geen code voor deze ondersteuning zelf. Meer informatie over [multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Afwijkende aanmelden detectie.**  Azure AD verwerkt meer dan een miljard aanmeldingen per dag, tijdens het gebruik van machine learning-algoritmen voor detecteren van verdachte activiteiten en de kennisgeving van IT-beheerders van mogelijke problemen. Door de ondersteuning van Azure AD-aanmelding, krijgt uw toepassing het voordeel van deze beveiligingsgroep. Meer informatie over [weergeven van Azure Active Directory-rapport](../active-directory-view-access-usage-reports.md).

**Voorwaardelijke toegang.**  Naast verificatie met meerdere factoren, beheerders kunnen vereisen dat aan bepaalde voorwaarden worden voldaan voordat gebruikers kunnen aanmelden bij uw toepassing. Voorwaarden die kunnen worden ingesteld omvatten het IP-adresbereik van clientapparaten, lidmaatschap van de opgegeven groepen en de status van het apparaat wordt gebruikt om toegang te krijgen. Meer informatie over [Azure Active Directory voor voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

### <a name="easy-development"></a>Eenvoudige ontwikkeling

**Protocollen volgens de industrienorm.**  Microsoft is gericht op de ondersteuning van industrienormen. De OAuth 2.0 en OpenID Connect 1.0 standaardprotocollen biedt ondersteuning voor het Microsoft identity-platform. Meer informatie over [Microsoft identity-platform-verificatieprotocollen](active-directory-v2-protocols.md).

**Open source-bibliotheken.**  Microsoft biedt volledig ondersteunde open source-bibliotheken voor populaire talen en platforms Versnel de ontwikkeling. De broncode is in licentie gegeven onder Apache 2.0 en u bent vrij te vertakken en bijdragen terug naar de projecten. Meer informatie over [Microsoft Authentication Library (MSAL)](reference-v2-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Wereldwijde aanwezigheid en hoge beschikbaarheid

**Azure AD wordt wordt geïmplementeerd in datacenters over de hele wereld en beheerd en bewaakt nacht.**  Azure AD het beheersysteem voor identiteit voor Microsoft Azure en Office 365 en wordt geïmplementeerd in 28 datacenters over de hele wereld. Directory-gegevens wordt worden gerepliceerd naar ten minste drie datacenters gegarandeerd. Globale load balancers Zorg ervoor dat gebruikers toegang tot de dichtstbijzijnde kopie van de Azure AD met hun gegevens en aanvragen voor andere datacenters automatisch opnieuw verzenden als er een probleem is gedetecteerd.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag schrijven van code](v2-overview.md#getting-started).

[Gebruikers aanmelden met behulp van Microsoft identity-platform](authentication-scenarios.md)

