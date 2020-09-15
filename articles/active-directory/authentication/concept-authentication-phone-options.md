---
title: Verificatie methoden voor de telefoon-Azure Active Directory
description: Meer informatie over het gebruik van methoden voor telefonische verificatie in Azure Active Directory om aanmeldings gebeurtenissen te verbeteren en te beveiligen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 09093a5dbd2142d3542e86a99e24f228320453d4
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532660"
---
# <a name="authentication-methods-in-azure-active-directory---phone-options"></a>Verificatie methoden in Azure Active Directory-telefoon opties

Voor directe verificatie met een tekst bericht kunt u [gebruikers configureren en inschakelen voor verificatie op basis van SMS (preview-versie)](howto-authentication-sms-signin.md). Aanmelding op basis van SMS is uitstekend voor front-line werk nemers. Met aanmelden op basis van SMS hoeven gebruikers geen gebruikers naam en wacht woord te kennen om toegang te krijgen tot toepassingen en services. De gebruiker voert het geregistreerde mobiele telefoon nummer in, ontvangt een SMS-bericht met een verificatie code en voert die in de aanmeldings interface in.

Gebruikers kunnen zich ook zelf verifiëren via een mobiele telefoon of een zakelijke telefoon als een secundaire vorm van verificatie die wordt gebruikt tijdens Azure Multi-Factor Authentication of selfservice voor wachtwoord herstel (SSPR).

Om goed te kunnen werken, moeten telefoon nummers de notatie *+ CountryCode phonenumber*hebben, bijvoorbeeld *+ 1 4251234567*.

> [!NOTE]
> Er moet een spatie zijn tussen de land-/regiocode en het telefoon nummer.
>
> Het opnieuw instellen van wacht woorden biedt geen ondersteuning voor telefoon uitbreidingen. Zelfs in de indeling van de *+ 1-4251234567X12345* worden uitbrei dingen verwijderd voordat de oproep wordt geplaatst.

## <a name="mobile-phone-verification"></a>Verificatie van mobiele telefoon

Voor Azure Multi-Factor Authentication of SSPR kunnen gebruikers ervoor kiezen om een SMS-bericht te ontvangen met een verificatie code om in te voeren in de aanmeldings interface, of om een telefoon gesprek te ontvangen met een prompt om hun gedefinieerde pincode in te voeren.

Als gebruikers niet willen dat hun mobiele telefoon nummer wordt weer gegeven in de map, maar u dit wilt gebruiken voor het opnieuw instellen van wacht woorden, moeten beheerders het telefoon nummer in de map niet vullen. In plaats daarvan moeten gebruikers hun kenmerk voor de verificatie van de **telefoon** invullen via de registratie van gecombineerde beveiligings gegevens op [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) . Beheerders kunnen deze informatie zien in het profiel van de gebruiker, maar ze worden niet elders gepubliceerd.

![Scherm afbeelding van de Azure Portal waarin de verificatie methoden met een ingevuld telefoon nummer worden weer gegeven](media/concept-authentication-methods/user-authentication-methods.png)

Micro soft biedt geen garantie voor consistente op SMS of spraak gebaseerde Azure Multi-Factor Authentication prompts met hetzelfde nummer. In het belang van onze gebruikers kunnen we op elk gewenst moment korte codes toevoegen of verwijderen, omdat we route aanpassingen aanbrengen om de bezorgings mogelijkheden van SMS te verbeteren. Micro soft biedt geen ondersteuning voor korte codes voor landen/regio's, naast de Verenigde Staten en Canada.

### <a name="text-message-verification"></a>Verificatie van SMS-berichten

Met de verificatie van SMS-berichten tijdens SSPR of Azure Multi-Factor Authentication, wordt er een sm's verzonden naar het mobiele telefoon nummer met een verificatie code. Om het aanmeldings proces te volt ooien, wordt de opgegeven verificatie code ingevoerd in de aanmeldings interface.

### <a name="phone-call-verification"></a>Verificatie via telefoon oproep

Bij een verificatie via telefoon gesprekken tijdens SSPR of Azure Multi-Factor Authentication wordt een automatische telefoon oproep uitgevoerd naar het telefoon nummer dat door de gebruiker is geregistreerd. Om het aanmeldings proces te volt ooien, wordt de gebruiker gevraagd hun pincode in te voeren, gevolgd door # op hun toetsen blok.

## <a name="office-phone-verification"></a>Verificatie op bedrijfs telefoon

Het kenmerk Office Phone wordt beheerd door de Azure AD-beheerder en kan niet worden geregistreerd door een gebruiker zelf.

Bij een verificatie via telefoon gesprekken tijdens SSPR of Azure Multi-Factor Authentication wordt een automatische telefoon oproep uitgevoerd naar het telefoon nummer dat door de gebruiker is geregistreerd. Om het aanmeldings proces te volt ooien, wordt de gebruiker gevraagd hun pincode in te voeren, gevolgd door # op hun toetsen blok.

## <a name="troubleshooting-phone-options"></a>Problemen met telefoonopties oplossen

Als u problemen ondervindt met telefoon verificatie voor Azure AD, raadpleegt u de volgende stappen voor probleem oplossing:

* De geblokkeerde beller-ID op één apparaat.
   * Bekijk alle geblokkeerde nummers die op het apparaat zijn geconfigureerd.
* Onjuist telefoon nummer of onjuiste land-/regionummer of Verwar ring tussen privé telefoon nummer en telefoon nummer van werk.
   * Problemen oplossen met het gebruikers object en de geconfigureerde verificatie methoden. Controleer of de juiste telefoon nummers zijn geregistreerd.
* Verkeerde pincode ingevoerd.
   * Bevestig dat de gebruiker de juiste pincode heeft gebruikt als geregistreerd voor hun account.
* Oproep doorgestuurd naar Voice mail.
   * Zorg ervoor dat de telefoon is ingeschakeld voor de gebruiker en dat de service beschikbaar is in hun gebied of gebruik een alternatieve methode.
* Gebruiker is geblokkeerd
   * Laat een Azure AD-beheerder de gebruiker blok keren in de Azure Portal.
* SMS is niet geabonneerd op het apparaat.
   * Laat de gebruiker methoden wijzigen of SMS activeren op het apparaat.
* Defecte telecommunicatie providers, zoals geen telefoon invoer gedetecteerd, ontbrekende DTMF Toon problemen, een geblokkeerde beller-ID op meerdere apparaten of geblokkeerde SMS-computers op meerdere apparaten.
   * Micro soft gebruikt meerdere telecom providers om telefoon gesprekken en SMS-berichten voor verificatie te routeren. Als u een van de bovenstaande problemen ziet, moet een gebruiker de methode ten minste vijf keer binnen vijf minuten gebruiken en de gegevens van die gebruiker beschikbaar hebben als u contact opneemt met micro soft ondersteuning.

## <a name="next-steps"></a>Volgende stappen

Zie de [zelfstudie voor self-service voor wachtwoordherstel (SSPR)][tutorial-sspr] en [Azure Multi-Factor Authentication][tutorial-azure-mfa] om aan de slag te gaan.

Zie [hoe Azure AD self-service password reset werkt][concept-sspr]voor meer informatie over SSPR-concepten.

Zie [hoe Azure multi-factor Authentication werkt][concept-mfa]voor meer informatie over MFA-concepten.

Meer informatie over het configureren van verificatie methoden met behulp van de [Microsoft Graph rest API bèta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
