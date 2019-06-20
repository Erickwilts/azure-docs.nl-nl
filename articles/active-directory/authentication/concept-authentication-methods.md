---
title: Verificatiemethoden - Azure Active Directory
description: Verificatiemethoden die beschikbaar zijn in Azure AD zijn voor MFA en Self-service voor Wachtwoordherstel
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1322c919906dc2d0dd23de538fa2c1992fbe5da0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164831"
---
# <a name="what-are-authentication-methods"></a>Wat zijn verificatiemethoden?

Als beheerder, kiest verificatiemethoden voor meervoudige verificatie en selfservice voor wachtwoordherstel (SSPR) het wordt aanbevolen dat u vereisen dat gebruikers meerdere verificatiemethoden registreren. Wanneer een verificatiemethode niet beschikbaar voor een gebruiker is, kunnen ze kiezen voor verificatie met een andere methode.

Beheerders kunnen definiëren in welke verificatiemethoden beschikbaar voor gebruikers van SSPR en MFA zijn-beleid. Sommige verificatiemethoden zijn mogelijk niet beschikbaar voor alle functies. Uw beleid Zie voor meer informatie over het configureren van de artikelen [implementeren van selfservice voor wachtwoordherstel](howto-sspr-deployment.md) en [plannen van een cloud-gebaseerde Azure multi-factor Authentication](howto-mfa-getstarted.md)

Microsoft raadt beheerders kunnen gebruikers meer dan de minimaal vereiste aantal van de ondersteunde verificatiemethoden selecteren in het geval ze geen toegang tot één.

|Verificatiemethode|Gebruik|
| --- | --- |
| Wachtwoord | MFA en Self-service voor Wachtwoordherstel |
| Beveiligingsvragen | Alleen SSPR |
| E-mailadres | Alleen SSPR |
| Microsoft Authenticator-app | MFA en preview-versie voor SSPR |
| OATH-token van Hardware | Openbare preview-versie van MFA en Self-service voor Wachtwoordherstel |
| Sms | MFA en Self-service voor Wachtwoordherstel |
| Spraakoproep | MFA en Self-service voor Wachtwoordherstel |
| App-wachtwoorden | MFA alleen in bepaalde gevallen |

![Verificatiemethoden in gebruik is op het aanmeldingsscherm](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| OATH-Hardwaretokens voor melding van MFA en de SSPR en de mobiele app of mobiele app-code als methoden voor het Azure AD Self-service voor wachtwoord opnieuw instellen van de openbare preview-functies van Azure Active Directory zijn. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="password"></a>Wachtwoord

Uw Azure AD-wachtwoord wordt beschouwd als een verificatiemethode. Dit is het een methode die **kan niet worden uitgeschakeld**.

## <a name="security-questions"></a>Beveiligingsvragen

Beveiligingsvragen zijn beschikbaar **alleen in Azure AD Self-service voor wachtwoord opnieuw instellen** naar niet-beheerdersaccounts.

Als u vragen over de beveiliging gebruikt, raden wij deze gebruikt in combinatie met een andere methode. Vragen over de beveiliging is minder veilig dan andere methoden, omdat sommige mensen u de antwoorden op vragen van een andere gebruiker kent mogelijk.

> [!NOTE]
> Beveiligingsvragen zijn privé en veilig opgeslagen op een gebruikersobject in de map en kunnen alleen door gebruikers worden beantwoord tijdens de registratie. Er is geen manier voor een beheerder om te lezen of wijzigen van de gebruiker vragen of antwoorden.
>

### <a name="predefined-questions"></a>Vooraf gedefinieerde vragen

* In welke stad u voldoet aan uw eerste partner ontmoet?
* In welke plaats hebben uw ouders?
* In welke plaats uw dichtstbijzijnde op hetzelfde niveau live?
* In welke plaats is uw vader geboren?
* In welke plaats is uw eerste taak?
* In welke plaats is uw moeder geboren?
* Welke plaats bevond u zich op Nieuwjaarsavond 2000?
* Wat is de achternaam van uw favoriete leraar op de middelbare school?
* Wat is de naam van een college u toegepast op, maar niet hebben bijgewoond?
* Wat is de naam van de plaats waar u uw trouwreceptie gehouden?
* Wat is de tweede voornaam van uw vader?
* Wat is uw lievelingseten?
* Wat is uw moeder grootmoeder eerst en achternaam van?
* Wat is de tweede voornaam van uw moeder?
* Wat is uw oudste Broer of zus geboren maand en jaar? (bijvoorbeeld November 1985.)
* Wat is er van uw oudste Broer of zus?
* Wat is uw uw grootvader eerst en achternaam van?
* Wat is er van uw jongste Broer of zus?
* Op welke school u Brugklas?
* Wat is de eerste en laatste naam van uw beste jeugdvriend?
* Wat is de eerste en laatste naam van uw eerste andere belangrijke?
* Wat is de achternaam van uw favoriete geavanceerde school leraar?
* Wat is het merk en model van uw eerste auto of motor?
* Wat is de naam van de eerste school?
* Wat is de naam van het ziekenhuis waarin u geboren?
* Wat is de naam van de straat van uw eerste home jeugd?
* Wat is de naam van uw jeugdidool?
* Wat is de naam van uw favoriete knuffeldier?
* Wat is de naam van uw eerste huisdier?
* Wat was uw bijnaam als kind?
* Wat was uw favoriete sport op de middelbare school?
* Wat was uw eerste baan?
* Wat zijn de laatste vier cijfers van het telefoonnummer van uw Ouderlijk huis?
* Als u jonge zijn, wat u wilt worden toen u klein?
* Wie is de grootste beroemdheid die u ooit hebt ontmoet?

Alle van de vooraf gedefinieerde beveiligingsvragen zijn vertaald en gelokaliseerd in de volledige set met Office 365-talen op basis van landinstellingen van de browser van de gebruiker.

### <a name="custom-security-questions"></a>Aangepaste beveiligingsvragen

Aangepaste beveiligingsvragen zijn niet gelokaliseerd. Alle aangepaste vragen worden weergegeven in dezelfde taal als ze zijn ingevoerd in de interface van de gebruiker met beheerdersrechten, zelfs als de landinstellingen van de browser van de gebruiker is anders. Als u gelokaliseerde vragen, moet u de vooraf gedefinieerde vragen.

De maximale lengte van een aangepaste beveiligingsvraag is 200 tekens.

### <a name="security-question-requirements"></a>Vraag beveiligingsvereisten

* Het maximum aantal tekens minimumlengte voor antwoord is drie tekens.
* De maximale beantwoorden tekenlimiet is 40 tekens.
* De dezelfde vraag meer dan één keer kunnen niet worden beantwoord door gebruikers.
* Gebruikers kunnen niet hetzelfde antwoord op meer dan één vraag opgeven.
* Een tekenset kan worden gebruikt om de vragen en antwoorden, met inbegrip van Unicode-tekens te definiëren.
* Het aantal vragen dat is gedefinieerd moet groter zijn dan of gelijk zijn aan het aantal vragen die zijn vereist om u te registreren.

## <a name="email-address"></a>E-mailadres

E-mailadres is beschikbaar **alleen in Azure AD Self-service voor wachtwoord opnieuw instellen**.

Microsoft raadt het gebruik van een e-mailaccount dat geen van de gebruiker Azure AD-wachtwoord voor toegang tot nodig.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator-app

De Microsoft Authenticator-app biedt een extra beveiligingsniveau voor uw Azure AD-werk of school-account of uw Microsoft-account.

De Microsoft Authenticator-app is beschikbaar voor [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) en [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071).

> [!NOTE]
> Gebruikers hebben niet de optie voor het registreren van hun mobiele app tijdens het registreren van self-service voor wachtwoord opnieuw instellen. In plaats daarvan gebruikers hun mobiele app kunnen registreren [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) of in de security info registratie preview [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).
>

### <a name="notification-through-mobile-app"></a>Melding via mobiele app

De Microsoft Authenticator-app kunt u voorkomt ongeoorloofde toegang tot accounts en frauduleuze transacties stoppen door het pushen van een melding naar uw smartphone of tablet. Gebruikers weergeven van de melding en wanneer deze geldig is, is Selecteer verifiëren. Anders kunnen ze weigeren selecteren.

> [!WARNING]
> Voor de self-service voor wachtwoord opnieuw ingesteld wanneer er slechts één methode vereist voor het opnieuw instellen is, de verificatiecode is de enige optie beschikbaar voor gebruikers **om te controleren of het hoogste niveau van beveiliging**.
>
> Wanneer twee methoden vereist zijn in te stellen met behulp van gebruikers is mogelijk **uitvoeren** melding **of** verificatiecode naast eventuele andere methoden ingeschakeld.
>

Als u het gebruik van inschakelt zijn beide melding via mobiele app en verificatiecode via mobiele app, gebruikers die de Microsoft Authenticator-app met behulp van een melding te registreren kunnen zowel melding en code gebruiken om hun identiteit te verifiëren.

> [!NOTE]
> Als uw organisatie personeel heeft in werkt of onderweg naar China, de **melding via mobiele app** methode voor **Android-apparaten** werkt niet in dat land. Alternatieve methoden moeten beschikbaar zijn voor gebruikers worden gemaakt.

### <a name="verification-code-from-mobile-app"></a>Verificatiecode via mobiele app

De Microsoft Authenticator-app of andere apps van derden kunnen worden gebruikt als een Softwaretoken voor het genereren van een OATH-code voor verificatie. Na het invoeren van uw gebruikersnaam en wachtwoord, voert u de code die is geleverd door de app in het aanmeldingsscherm. De verificatiecode zorgt voor een tweede vorm van verificatie.

> [!WARNING]
> Self-service voor wachtwoord opnieuw ingesteld wanneer alleen één methode vereist is voor het opnieuw instellen van de verificatiecode is de enige optie beschikbaar voor gebruikers **om te controleren of het hoogste niveau van beveiliging**.
>

Gebruikers hebben mogelijk een combinatie van maximaal vijf OATH-tokens voor hardware- of verificator-toepassingen, zoals de Microsoft Authenticator-app is geconfigureerd voor gebruik op elk gewenst moment.

## <a name="oath-hardware-tokens-public-preview"></a>OATH-hardware-tokens (openbare preview)

OATH is een open standaard die aangeeft hoe eenmalig wachtwoord (OTP)-codes worden gegenereerd. Azure AD ondersteunt het gebruik van de mobiele TOTP-SHA-1-OATH-tokens van de verschillende 30 seconden of 60 seconden. Klanten kunnen deze tokens verkrijgen van de leverancier van hun keuze. Geheime sleutels zijn beperkt tot 128 tekens lang, wat mogelijk niet compatibel met alle tokens.

![OATH-tokens geüpload naar de blade MFA Server OATH-tokens](media/concept-authentication-methods/oath-tokens-azure-ad.png)

Hardware-OATH-tokens worden ondersteund als onderdeel van een openbare preview. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

Zodra de tokens worden verkregen moet het worden geüpload in een indeling met door komma's gescheiden waarden (CSV) met inbegrip van de UPN, serienummer, geheime sleutel, tijdsinterval, de fabrikant en model als in het voorbeeld hieronder ziet.

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567890abcdef1234567890abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Zorg ervoor dat u de rij met koppen opnemen in uw CSV-bestand, zoals hierboven.

Eenmaal juist opgemaakt als een CSV-bestand, een beheerder kan vervolgens aanmelden bij de Azure-portal en gaat u naar **Azure Active Directory**, **MFA-Server**, **OATH-tokens**, en Upload het resulterende CSV-bestand.

Afhankelijk van de grootte van het CSV-bestand duurt het enkele minuten om te verwerken. Klik op de **vernieuwen** knop om op te halen van de huidige status. Als er fouten in het bestand zijn, hebt u de optie voor het downloaden van een CSV-bestand met alle fouten voor u om op te lossen.

Wanneer er fouten zijn opgelost, de beheerder vervolgens kan activeren elke sleutel door te klikken op **activeren** voor het token wordt geactiveerd en de invoeren door de OTP op het token wordt weergegeven.

Gebruikers hebben mogelijk een combinatie van maximaal vijf OATH-tokens voor hardware- of verificator-toepassingen, zoals de Microsoft Authenticator-app is geconfigureerd voor gebruik op elk gewenst moment.

## <a name="phone-options"></a>Telefoonopties

### <a name="mobile-phone"></a>Mobiele telefoon

Er zijn twee opties beschikbaar voor gebruikers met mobiele telefoons.

Als gebruikers niet dat het mobiele telefoonnummer wilt zichtbaar is in de map, maar ze nog steeds wilt gebruiken voor wachtwoord opnieuw instellen, moeten beheerders niet vullen het in de map. Gebruikers moeten invullen hun **telefoon voor authenticatie** kenmerk de [-registratieportal voor wachtwoordherstel](https://aka.ms/ssprsetup). Beheerders kunnen deze informatie in het profiel van de gebruiker zien, maar deze niet elders wordt gepubliceerd.

Voor een goede werking telefoonnummers moet zich in de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

> [!NOTE]
> Er moet een spatie tussen de landcode en het telefoonnummer.
>
> Wachtwoord opnieuw instellen biedt geen ondersteuning voor extensies van de telefoon. Zelfs in de indeling van de 4255551234 + 1 X-12345, worden de extensies worden verwijderd voordat de oproep wordt gedaan.

#### <a name="text-message"></a>Sms-bericht

Een SMS-bericht is verzonden naar het mobiele nummer met een verificatiecode. Voer de verificatiecode die is opgegeven in de interface aanmelden om door te gaan.

#### <a name="phone-call"></a>Telefonische oproep

Een geautomatiseerd telefoongesprek is naar het telefoonnummer dat u opgeeft gemaakt. Beantwoord de oproep en druk op # in het toetsenblok telefoon verifiëren

> [!IMPORTANT]
> Vanaf maart van 2019 de opties telefoongesprek zich niet beschikbaar voor gebruikers voor MFA en Self-service voor Wachtwoordherstel in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefonische oproep blijft beschikbaar voor gebruikers in betaalde Azure AD-tenants. Deze wijziging geldt alleen voor gratis/proefversie van Azure AD-tenants.

### <a name="office-phone"></a>Telefoon (werk)

Een geautomatiseerd telefoongesprek is naar het telefoonnummer dat u opgeeft gemaakt. Beantwoord het gesprek en drukt # in het toetsenblok van de telefoon om te verifiëren.

Voor een goede werking telefoonnummers moet zich in de indeling *+ CountryCode PhoneNumber*, bijvoorbeeld: + 1 4255551234.

Het kenmerk van de telefoon office wordt beheerd door uw beheerder.

> [!IMPORTANT]
> Vanaf maart van 2019 de opties telefoongesprek zich niet beschikbaar voor gebruikers voor MFA en Self-service voor Wachtwoordherstel in vrij/proefversie van Azure AD-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefonische oproep blijft beschikbaar voor gebruikers in betaalde Azure AD-tenants. Deze wijziging geldt alleen voor gratis/proefversie van Azure AD-tenants.

> [!NOTE]
> Er moet een spatie tussen de landcode en het telefoonnummer.
>
> Wachtwoord opnieuw instellen biedt geen ondersteuning voor extensies van de telefoon. Zelfs in de indeling van de 4255551234 + 1 X-12345, worden de extensies worden verwijderd voordat de oproep wordt gedaan.

### <a name="troubleshooting-phone-options"></a>Telefoonopties voor probleemoplossing

Veelvoorkomende problemen met betrekking tot verificatiemethoden met behulp van een telefoonnummer:

* Geblokkeerde beller-ID op één apparaat
   * Apparaat oplossen
* Onjuist telefoonnummer, onjuist landnummer, een telefoonnummer thuis ten opzichte van zakelijke telefoonnummer
   * Problemen oplossen gebruikersobject en verificatiemethoden geconfigureerd. Zorg ervoor dat de juiste telefoonnummers worden geregistreerd.
* Onjuiste PINCODE ingevoerd
   * Controleer of de gebruiker heeft de juiste PINCODE geregistreerd in Azure MFA-Server gebruikt.
* Aanroep doorgestuurd naar voicemail
   * Zorg ervoor dat de gebruiker heeft de telefoon is ingeschakeld en dat de service beschikbaar in het gebied is of alternatieve methode gebruiken.
* Gebruiker is geblokkeerd
   * Blokkering van de gebruiker in Azure portal beheerder hebben.
* SMS is niet geabonneerd op het apparaat
   * Laat de gebruiker methoden wijzigen of SMS op het apparaat te activeren.
* Beschadigde telecom-providers (geen telefooninvoer ontbreken, problemen in de DTMF-tonen, geblokkeerde beller-ID op meerdere apparaten, of SMS geblokkeerd op meerdere apparaten)
   * Microsoft maakt gebruik van meerdere telecom-providers voor het routeren van telefoongesprekken en SMS-berichten voor verificatie. Als er een van de bovenstaande problemen hebt een gebruiker poging tot het gebruik van de methode ten minste 5 keer binnen 5 minuten en beschikken over van de gebruiker informatie tijdens de verbinding met Microsoft ondersteuning.

## <a name="app-passwords"></a>App-wachtwoorden

Bepaalde niet-browsertoepassingen geen ondersteuning voor meervoudige verificatie, als een gebruiker is ingeschakeld voor multi-factor authentication en proberen te gebruiken van niet-browsertoepassingen, kan niet worden geverifieerd zijn. Een app-wachtwoord kan gebruikers om door te gaan om te verifiëren

Als u multi-factor Authentication via beleid voor voorwaardelijke toegang en niet via MFA per gebruiker afdwingen, kunt u geen app-wachtwoorden maken. Toepassingen die gebruikmaken van beleid voor voorwaardelijke toegang om toegang te beheren, geen app-wachtwoorden nodig.

Als uw organisatie is gefedereerd voor eenmalige aanmelding met Azure AD en u wilt gebruikmaken van Azure MFA, klikt u rekening houden met de volgende gegevens:

* Het app-wachtwoord wordt gecontroleerd door Azure AD en daarom omzeilt federation. Federatie wordt alleen gebruikt bij het instellen van app-wachtwoorden. Voor federatieve gebruikers (SSO), worden wachtwoorden opgeslagen in de organisatie-ID. Als de gebruiker het bedrijf verlaat, heeft dat informatie naar de organisatie-ID met behulp van DirSync stromen. Account uitschakelen/verwijderen kan tot drie uur duren om te synchroniseren die uitschakelen/verwijderen van app-wachtwoorden in Azure AD uitstelt.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Geen on-premises verificatie logboekregistratie/controle mogelijkheid is beschikbaar voor app-wachtwoorden.
* Bepaalde geavanceerde ontwerpen van de architectuur mogelijk met behulp van een combinatie van organisatie-gebruikersnaam en wachtwoorden en app-wachtwoorden als u de verificatie in twee stappen voor clients, afhankelijk van waar ze worden geverifieerd. Voor clients die verificatie op basis van een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.
* Gebruikers kunnen geen app-wachtwoorden maken standaard. Als u toestaan dat gebruikers wilt kunnen maken van app-wachtwoorden, selecteer de **gebruikers toestaan te maken van app-wachtwoorden aan te melden bij niet-browsertoepassingen optie** onder service-instellingen.

## <a name="next-steps"></a>Volgende stappen

[Selfservice voor wachtwoord opnieuw instellen voor uw organisatie inschakelen](quickstart-sspr.md)

[Azure multi-factor Authentication inschakelen voor uw organisatie](howto-mfa-getstarted.md)

[De gecombineerde registratie in uw tenant inschakelen](howto-registration-mfa-sspr-combined.md)

[Documentatie voor eindgebruikers verificatie methode configuration](https://aka.ms/securityinfoguide)
