---
title: Implementatiegids Selfservice voor wachtwoordherstel - Azure Active Directory
description: Tips voor een geslaagde implementatie van Azure AD-self-service voor wachtwoordherstel
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c254ef3a71e95b33df2a779c728d47fff3c3759
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65190380"
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Selfservice voor wachtwoordherstel implementeren

De meeste klanten voeren de volgende stappen uit voor een goede implementatie van de functionaliteit van de selfservice voor wachtwoordherstel (SSPR) van Azure Active directory (Azure AD):

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. Voer de implementatie van een prototype van met een kleine subset van uw organisatie.
   * Meer informatie over het pilot vindt u de [zelfstudie: Implementeer complete een prototype van Azure AD Self-service voor wachtwoord opnieuw instellen van](tutorial-sspr-pilot.md).
1. Leer uw helpdesk.
   * Hoe kunnen ze uw gebruikers?
   * Forceert u gebruikers SSPR gebruiken en niet toestaan dat uw helpdesk om gebruikers te helpen?
   * Hebt u opgegeven ze de URL's voor registratie en opnieuw instellen?
      * Registratie:  https://aka.ms/ssprsetup
      * Opnieuw instellen: https://aka.ms/sspr

   > [!WARNING]
   > Gebruik van het selectievakje 'gebruiker moet wachtwoord wijzigen bij volgende aanmelding' in de on-premises Active Directory-beheerprogramma's, zoals Active Directory: gebruikers en Computers of Active Directory Administrative Center wordt niet ondersteund. Bij het wijzigen van een wachtwoord controleren on-premises met deze optie niet. 

1. Leer uw gebruikers.
   * De volgende secties van dit document gaat over de voorbeeld-communicatie, wachtwoord portals, het afdwingen van inschrijving en vullen met gegevens van de verificatie.
   * De Azure Active Directory-productgroep heeft een [stapsgewijs implementatieplan](https://aka.ms/SSPRDeploymentPlan) gemaakt, dat organisaties in combinatie met de documentatie kunnen gebruiken die te vinden is op deze site om een bedrijfsscenario te maken en de implementatie te plannen van selfservice voor wachtwoordherstel.
1. Self-service voor wachtwoord opnieuw instellen voor uw hele organisatie inschakelen.
   * Wanneer u klaar bent, schakelt u wachtwoordherstel in voor alle gebruikers door de schakeloptie **Selfservice voor wachtwoordherstel is ingeschakeld** in te stellen op **Alle**.

## <a name="sample-communication"></a>Voorbeeld-communicatie

Veel klanten vinden een e-mailcampagne, met eenvoudig te gebruiken instructies, de eenvoudigste manier om gebruikers selfservice voor wachtwoordherstel te laten gebruiken. [We hebben gemaakt met eenvoudige e-mailberichten en andere activa die u als sjabloon gebruiken kunt om te helpen bij uw implementatie](https://www.microsoft.com/download/details.aspx?id=56768):

* **Binnenkort**: Een e-mailsjabloon die u in de weken of dagen vóór implementatie gebruiken om gebruikers te informeren dat ze iets moeten gaan doen.
* **Nu beschikbaar**: Een e-mailsjabloon dat u de dag van het programma start station gebruikers zich registreren en hun verificatiegegevens te bevestigen. Als de gebruikers zich nu registreren, kunnen ze selfservice voor wachtwoordherstel gebruiken wanneer het nodig is.
* **Herinnering voor aanmelding**: Een e-mailsjabloon voor een paar dagen tot een paar weken na de implementatie te herinneren dat gebruikers zich registreren en hun verificatiegegevens te bevestigen.
* **SSPR-Posters**: Posters die u kunt aanpassen en weergeven over uw organisatie in de dagen en leidt tot weken en na de implementatie.
* **SSPR naambordjes**: Tabel kaarten die u kunt plaatsen in de ruimte op lunch, vergaderruimten, of op eigen bureau ter bevordering van uw gebruikers kunnen de inschrijving voltooien.
* **SSPR Stickers**: Sticker sjablonen die u kunt aanpassen en afdrukken als u wilt plaatsen, laptops, monitors, toetsenborden of mobiele telefoons om te weten hoe u toegang krijgen tot SSPR.

![Voorbeelden van SSPR e-mailadres kan worden geïmplementeerd voor gebruikers][Email]

## <a name="create-your-own-password-portal"></a>Uw eigen wachtwoordportal maken

Veel klanten kiezen ervoor een webpagina te hosten en een DNS-basisvermelding te maken, zoals https://passwords.contoso.com . Ze vullen deze pagina met koppelingen naar de volgende informatie:

* [Azure AD-portal voor wachtwoordherstel - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD-registratieportal voor wachtwoordherstel https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Azure AD-portal voor wachtwoordwijziging https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Overige organisatiespecifieke informatie

In e-mailberichten of flyers die u verstuurt, kunt u een in huisstijl opgemaakte, gemakkelijk te onthouden URL opnemen die gebruikers kunnen raadplegen wanneer ze de services moeten gebruiken. We hebben een [voorbeeldpagina voor wachtwoordherstel](https://github.com/ajamess/password-reset-page) gemaakt die u kunt gebruiken en aanpassen aan de behoeften van uw organisatie.

## <a name="use-enforced-registration"></a>Gedwongen registratie gebruiken

Als u wilt dat uw gebruikers zich registreren voor wachtwoordherstel, kunt u hen dwingen zich te registreren wanneer ze zich aanmelden via Azure AD. U kunt deze optie inschakelen via het deelvenster **Wachtwoordherstel** van uw directory door de optie **Vereisen dat gebruikers zich bij aanmelding registreren?** te kiezen op het tabblad **Registratie**.

Beheerders kunnen gebruikers dwingen zich opnieuw te registreren na een bepaalde periode. Ze kunnen de optie **Aantal dagen waarna gebruikers wordt gevraagd om de verificatiegegevens opnieuw te bevestigen** instellen op een waarde van 0 tot 730 dagen.

Nadat u deze optie hebt ingeschakeld, zien gebruikers bij aanmelding een bericht dat hen informeert dat de beheerder vereist dat ze hun verificatiegegevens controleren.

## <a name="populate-authentication-data"></a>Verificatiegegevens invullen

U moet rekening houden met [sommige gegevens van de verificatie voor uw gebruikers vooraf invullen](howto-sspr-authenticationdata.md). Op die manier hoeven gebruikers zich niet te registreren voor wachtwoordherstel voordat ze SSPR kunnen gebruiken. Zolang gebruikers de verificatiegegevens hebben gedefinieerd die voldoen aan het beleid voor wachtwoordherstel dat u hebt gedefinieerd, kunnen gebruikers hun wachtwoord opnieuw instellen.

## <a name="disable-self-service-password-reset"></a>Selfservice voor wachtwoordherstel uitschakelen

Als uw organisatie besluit om het uitschakelen van self-service voor wachtwoord opnieuw instellen is een eenvoudig proces. Open uw Azure AD-tenant en ga naar **Wachtwoord opnieuw instellen** > **Eigenschappen** en selecteer vervolgens **Geen** onder **Selfservice voor wachtwoord opnieuw instellen is ingeschakeld**. Gebruikers handhaaft nog steeds hun geregistreerde verificatiemethoden voor toekomstig gebruik.

## <a name="next-steps"></a>Volgende stappen

* [Uw wachtwoord opnieuw instellen of wijzigen](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registreren voor de selfservice voor wachtwoordherstel](../user-help/active-directory-passwords-reset-register.md)
* [Geconvergeerde registratie voor meervoudige verificatie en Azure AD Self-service voor wachtwoord opnieuw instellen inschakelen](concept-registration-mfa-sspr-converged.md)
* [Hebt u een vraag over licenties?](concept-sspr-licensing.md)
* [Welke gegevens worden gebruikt door selfservice voor wachtwoordherstel en welke gegevens moet u voor uw gebruikers invullen?](howto-sspr-authenticationdata.md)
* [Wat zijn de beleidsopties bij selfservice voor wachtwoordherstel?](concept-sspr-policy.md)
* [Wat is Wachtwoord terugschrijven en waarom is dit van belang?](howto-sspr-writeback.md)
* [Hoe maak ik rapporten van activiteit in selfservice voor wachtwoordherstel?](howto-sspr-reporting.md)
* [Wat zijn alle opties in selfservice voor wachtwoordherstel en wat houden ze in?](concept-sspr-howitworks.md)
* [Ik denk dat er iets misgaat. Hoe los ik problemen in selfservice voor wachtwoordherstel op?](active-directory-passwords-troubleshoot.md)
* [Ik heb een vraag die nog niet is beantwoord](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Deze e-mailsjablonen aan de behoeften van uw organisatie aanpassen"
