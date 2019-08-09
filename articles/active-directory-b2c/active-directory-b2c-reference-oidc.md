---
title: Webaanmelding met OpenID Connect Connect-Azure Active Directory B2C | Microsoft Docs
description: Bouw webtoepassingen met behulp van het OpenID Connect Connect-verificatie protocol in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 6b5157a71ce15d4dfd199b6826be22235e61df97
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848532"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Webaanmelding met OpenID Connect Connect in Azure Active Directory B2C

OpenID Connect Connect is een verificatie protocol dat is gebouwd op OAuth 2,0 en dat kan worden gebruikt om gebruikers veilig te ondertekenen bij webtoepassingen. Door gebruik te maken van de implementatie van de Azure Active Directory B2C (Azure AD B2C) van OpenID Connect Connect, kunt u zich aanmelden, aanmelden en andere ervaring voor identiteits beheer in uw webtoepassingen tot Azure Active Directory (Azure AD). In deze hand leiding wordt uitgelegd hoe u dit kunt doen op een taal onafhankelijke manier. Hierin wordt beschreven hoe u HTTP-berichten verzendt en ontvangt zonder gebruik te maken van de open source-bibliotheken.

[OpenID Connect Connect](https://openid.net/specs/openid-connect-core-1_0.html) breidt het OAuth 2,0- *autorisatie* protocol uit voor gebruik als *verificatie* protocol. Met dit verificatie protocol kunt u eenmalige aanmelding uitvoeren. Hierin wordt het concept van een *id-token*geïntroduceerd, waarmee de client de identiteit van de gebruiker kan verifiëren en basis profiel informatie over de gebruiker kan verkrijgen.

Omdat het OAuth 2,0 uitbreidt, kunnen toepassingen ook veilig *toegangs tokens*verkrijgen. U kunt toegangs tokens gebruiken om toegang te krijgen tot bronnen die worden beveiligd door een [autorisatie server](active-directory-b2c-reference-protocols.md). OpenID Connect Connect wordt aanbevolen als u een webtoepassing bouwt die wordt gehost op een server en toegankelijk is via een browser. Als u identiteits beheer wilt toevoegen aan uw mobiele of desktop toepassingen met behulp van Azure AD B2C, moet u [OAuth 2,0](active-directory-b2c-reference-oauth-code.md) gebruiken in plaats van OpenID Connect Connect. Zie het [overzicht van tokens in azure Active Directory B2C](active-directory-b2c-reference-tokens.md) voor meer informatie over tokens.

Azure AD B2C breidt het Standard OpenID Connect Connect-protocol uit voor meer dan eenvoudige verificatie en autorisatie. Hiermee wordt de [para meter User flow](active-directory-b2c-reference-policies.md)geïntroduceerd, waarmee u OpenID Connect Connect kunt gebruiken om gebruikers ervaringen toe te voegen aan uw toepassing, zoals aanmelden, aanmelden en Profiel beheer.

## <a name="send-authentication-requests"></a>Verificatie aanvragen verzenden

Wanneer uw webtoepassing de gebruiker moet verifiëren en een gebruikers stroom kan uitvoeren, kan deze de gebruiker naar het `/authorize` eind punt sturen. De gebruiker actie onderneemt afhankelijk van de gebruikers stroom.

In deze aanvraag geeft de client de machtigingen aan die moeten worden opgehaald van de gebruiker in de `scope` para meter en de gebruikers stroom om te worden uitgevoerd in de `p` para meter. In de volgende secties vindt u drie voor beelden (met regel einden voor de Lees baarheid), elk met een andere gebruikers stroom. Als u wilt weten hoe elke aanvraag werkt, kunt u de aanvraag in een browser plakken en deze uitvoeren. U kunt vervangen `fabrikamb2c` door de naam van uw Tenant als u er een hebt en een gebruikers stroom hebt gemaakt. U moet ook vervangen `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6`. Vervang deze client-ID door de App-ID van de registratie van de toepassing die u hebt gemaakt. Wijzig ook de naam `b2c_1_sign_in` van het beleid in de naam van het beleid dat u in uw Tenant hebt. 

#### <a name="use-a-sign-in-user-flow"></a>Een aanmeldings gebruikers stroom gebruiken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>Een gebruikers stroom voor registratie gebruiken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>Een gebruikers stroom voor het bewerken van profielen gebruiken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-ID die de [Azure Portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| response_type | Ja | Moet een ID-token voor OpenID Connect Connect bevatten. Als uw webtoepassing ook tokens nodig heeft voor het aanroepen van een web- `code+id_token`API, kunt u gebruiken. |
| redirect_uri | Nee | De `redirect_uri` para meter van uw toepassing, waar verificatie reacties kunnen worden verzonden en ontvangen door uw toepassing. De waarde moet exact overeenkomen met `redirect_uri` een van de para meters die u hebt geregistreerd in de Azure Portal, behalve dat deze URL moet worden gecodeerd. |
| bereik | Ja | Een lijst met door spaties gescheiden bereiken. Het `openid` bereik geeft een machtiging aan voor het aanmelden van de gebruiker en het ophalen van gegevens over de gebruiker in de vorm van id-tokens. Het `offline_access` bereik is optioneel voor webtoepassingen. Dit geeft aan dat uw toepassing een vernieuwings *token* nodig heeft voor uitgebreide toegang tot resources. |
| response_mode | Nee | De methode die wordt gebruikt om de resulterende autorisatie code terug te sturen naar uw toepassing. Dit kan ofwel `query`, `form_post`, of `fragment`.  De `form_post` antwoord modus wordt aanbevolen voor de beste beveiliging. |
| toestand | Nee | Een waarde die is opgenomen in de aanvraag die ook wordt geretourneerd in de token reactie. Dit kan een teken reeks zijn van elke gewenste inhoud. Een wille keurig gegenereerde unieke waarde wordt doorgaans gebruikt om vervalsing van aanvragen op meerdere sites te voor komen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de toepassing voordat de verificatie aanvraag heeft plaatsgevonden, zoals de pagina waarop deze zich bevonden. |
| nonce | Ja | Een waarde die is opgenomen in de aanvraag (gegenereerd door de toepassing) die is opgenomen in de resulterende ID-token als claim. De toepassing kan vervolgens deze waarde verifiëren om token replay-aanvallen te verhelpen. De waarde is doorgaans een wille keurige unieke teken reeks die kan worden gebruikt om de oorsprong van de aanvraag te identificeren. |
| p | Ja | De gebruikers stroom die wordt uitgevoerd. Dit is de naam van een gebruikers stroom die is gemaakt in uw Azure AD B2C-Tenant. De naam van de gebruikers stroom moet beginnen met `b2c\_1\_`. |
| verschijnt | Nee | Het type gebruikers interactie dat is vereist. De enige geldige waarde is `login`op dit moment, waardoor de gebruiker de referenties voor die aanvraag invoert. |

Op dit moment wordt de gebruiker gevraagd om de werk stroom te volt ooien. De gebruiker moet mogelijk hun gebruikers naam en wacht woord invoeren, zich aanmelden met een sociale identiteit of zich aanmelden voor de Directory. Er kunnen verschillende stappen worden uitgevoerd, afhankelijk van de manier waarop de gebruikers stroom is gedefinieerd.

Nadat de gebruiker de gebruikers stroom heeft voltooid, wordt er met de aangegeven `redirect_uri` para meter een reactie naar uw toepassing geretourneerd met behulp van de methode die is opgegeven in de `response_mode` para meter. Het antwoord is hetzelfde voor elk van de voor gaande gevallen, onafhankelijk van de gebruikers stroom.

Er is een succes `response_mode=fragment` volle reactie met het volgende:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --------- | ----------- |
| id_token | Het ID-token dat de toepassing heeft aangevraagd. U kunt het ID-token gebruiken om de identiteit van de gebruiker te verifiëren en een sessie met de gebruiker te starten. |
| code | De autorisatie code die de toepassing heeft aangevraagd als u deze `response_type=code+id_token`hebt gebruikt. De toepassing kan de autorisatie code gebruiken om een toegangs token voor een doel bron aan te vragen. Autorisatie codes verlopen doorgaans na ongeveer 10 minuten. |
| toestand | Als een `state` para meter in de aanvraag is opgenomen, moet dezelfde waarde in het antwoord worden weer gegeven. De toepassing moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn. |

Fout reacties kunnen ook worden verzonden naar de `redirect_uri` para meter, zodat de toepassing deze op de juiste wijze kan afhandelen:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --------- | ----------- |
| fout | Een code die kan worden gebruikt voor het classificeren van de typen fouten die optreden. |
| error_description | Een specifiek fout bericht dat kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |
| toestand | Als een `state` para meter in de aanvraag is opgenomen, moet dezelfde waarde in het antwoord worden weer gegeven. De toepassing moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn. |

## <a name="validate-the-id-token"></a>Het ID-token valideren

Alleen het ontvangen van een ID-token is voldoende om de gebruiker te verifiëren. Valideer de hand tekening van het ID-token en controleer de claims in het token volgens de vereisten van uw toepassing. Azure AD B2C maakt gebruik van [JSON Web tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en open bare-sleutel cryptografie om tokens te ondertekenen en te controleren of ze geldig zijn. Er zijn veel open-source bibliotheken beschikbaar voor het valideren van JWTs, afhankelijk van de taal van uw voor keur. We raden u aan deze opties te verkennen in plaats van uw eigen validatie logica te implementeren. 

Azure AD B2C heeft een OpenID Connect voor het verbinden van meta gegevens, waarmee een toepassing informatie over Azure AD B2C tijdens runtime kan ophalen. Deze informatie omvat eind punten, token inhoud en sleutels voor token-ondertekening. Er is een JSON-meta gegevens document voor elke gebruikers stroom in uw B2C-Tenant. Het meta gegevens document voor de `b2c_1_sign_in` gebruikers stroom in bevindt zich bijvoorbeeld in: `fabrikamb2c.onmicrosoft.com`

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit configuratie document is `jwks_uri`, waarvan de waarde voor dezelfde gebruikers stroom zou zijn:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Als u wilt bepalen welke gebruikers stroom is gebruikt voor het ondertekenen van een ID-token (en van waaruit de meta gegevens worden opgehaald), hebt u twee opties. Eerst wordt de naam van de gebruikers stroom opgenomen in `acr` de claim in het id-token. De andere optie is het coderen van de gebruikers stroom in de waarde van `state` de para meter wanneer u de aanvraag afgeeft en deze vervolgens decodeert om te bepalen welke gebruikers stroom is gebruikt. Beide methoden zijn geldig.

Nadat u het meta gegevens document van het OpenID Connect Connect-meta gegevens-eind punt hebt opgehaald, kunt u de open bare sleutels van RSA 256 gebruiken om de hand tekening van het ID-token te valideren. Er kunnen meerdere sleutels worden vermeld op dit eind punt, elk geïdentificeerd door `kid` een claim. De header van het id-token bevat ook `kid` een claim, waarmee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van het id-token.

Als u de tokens van Azure AD B2C wilt verifiëren, moet u de open bare sleutel genereren met de exponent (e) en modulus (n). U moet bepalen hoe u dit in uw respectieve programmeer taal kunt doen. De officiële documentatie over het genereren van open bare sleutels met het RSA-protocol vindt u hier: https://tools.ietf.org/html/rfc3447#section-3.1

Nadat u de hand tekening van het ID-token hebt gevalideerd, zijn er verschillende claims die u moet verifiëren. Bijvoorbeeld:

- Valideer `nonce` de claim om token replay-aanvallen te voor komen. De waarde moet zijn wat u hebt opgegeven in het aanmeldings verzoek.
- Valideer `aud` de claim om ervoor te zorgen dat het id-token is uitgegeven voor uw toepassing. De waarde moet de toepassings-ID van uw toepassing zijn.
- Valideer `iat` de `exp` en claims om ervoor te zorgen dat het id-token niet is verlopen.

Er zijn ook meerdere validaties die u moet uitvoeren. De validaties worden gedetailleerd beschreven in de [OpenID Connect Connect core-specificatie](https://openid.net/specs/openid-connect-core-1_0.html). Het is ook mogelijk dat u aanvullende claims wilt valideren, afhankelijk van uw scenario. Enkele veelvoorkomende validaties zijn:

- Controleren of de gebruiker/organisatie zich heeft geregistreerd voor de toepassing.
- Controleren of de gebruiker de juiste autorisatie/bevoegdheden heeft.
- Ervoor zorgen dat er een zekere mate van verificatie is opgetreden, zoals Azure multi-factor Authentication.

Nadat u het ID-token hebt gevalideerd, kunt u met de gebruiker beginnen met een sessie. U kunt de claims in het ID-token gebruiken om informatie over de gebruiker in uw toepassing op te halen. Gebruik voor deze informatie is onder andere weer gave, records en autorisatie.

## <a name="get-a-token"></a>Een Token ophalen

Als uw webtoepassing alleen gebruikers stromen moet uitvoeren, kunt u de volgende gedeelten overs Laan. Deze secties zijn alleen van toepassing op webtoepassingen die geverifieerde aanroepen naar een web-API moeten maken en ook worden beveiligd door Azure AD B2C.

U kunt de autorisatie code die u hebt verkregen (met behulp `response_type=code+id_token`van) voor een token voor de gewenste resource inwisselen door een `/token` `POST` aanvraag naar het eind punt te verzenden. In Azure AD B2C kunt u [toegangs tokens aanvragen voor andere api's](active-directory-b2c-access-tokens.md#request-a-token) zoals gebruikelijk door hun bereik (en) op te geven in de aanvraag.

U kunt ook een toegangs token aanvragen voor de eigen back-end web-API van uw app door middel van het gebruik van de client-ID van de app als het aangevraagde bereik (wat resulteert in een toegangs token met die client-ID als ' doel groep '):

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikers stroom die is gebruikt om de autorisatie code op te halen. U kunt in deze aanvraag niet een andere gebruikers stroom gebruiken. Voeg deze para meter toe aan de query reeks, niet op de hoofd tekst van het bericht. |
| client_id | Ja | De toepassings-ID die de [Azure Portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| grant_type | Ja | Het type toekenning, dat voor de autorisatie `authorization_code` code stroom moet gelden. |
| bereik | Nee | Een lijst met door spaties gescheiden bereiken. Het `openid` bereik geeft een machtiging aan voor het aanmelden van de gebruiker en het ophalen van gegevens over de gebruiker in de vorm van id_token-para meters. Het kan worden gebruikt om tokens te verkrijgen voor de eigen back-end web-API van uw toepassing, die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. Het `offline_access` bereik geeft aan dat uw toepassing een vernieuwings token nodig heeft voor uitgebreide toegang tot resources. |
| code | Ja | De autorisatie code die u aan het begin van de gebruikers stroom hebt verkregen. |
| redirect_uri | Ja | De `redirect_uri` para meter van de toepassing waarin u de autorisatie code hebt ontvangen. |
| client_secret | Ja | Het toepassings geheim dat is gegenereerd in de [Azure Portal](https://portal.azure.com/). Dit toepassings geheim is een belang rijk beveiligings artefact. U moet deze veilig opslaan op uw server. Wijzig dit client geheim op periodieke basis. |

Een geslaagd token antwoord ziet er als volgt uit:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Description |
| --------- | ----------- |
| not_before | Het tijdstip waarop het token als geldig wordt beschouwd in de epoche-tijd. |
| token_type | De waarde van het token type. `Bearer`is het enige type dat wordt ondersteund. |
| access_token | De ondertekende JWT-token die u hebt aangevraagd. |
| bereik | De bereiken waarvoor het token geldig is. |
| expires_in | De tijds duur dat het toegangs token geldig is (in seconden). |
| refresh_token | Een OAuth 2,0-vernieuwings token. De toepassing kan dit token gebruiken om aanvullende tokens te verkrijgen nadat het huidige token verloopt. Vernieuwings tokens kunnen worden gebruikt om de toegang tot bronnen gedurende lange tijd te behouden. Het bereik `offline_access` moet zijn gebruikt in de autorisatie-en Token aanvragen om een vernieuwings token te kunnen ontvangen. |

Fout berichten zien er als volgt uit:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --------- | ----------- |
| fout | Een code die kan worden gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een bericht dat kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |

## <a name="use-the-token"></a>Het token gebruiken

Nu u een toegangs token hebt aangeschaft, kunt u het token gebruiken in aanvragen voor uw back-end-web-api's door dit op te nemen `Authorization` in de header:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Het token vernieuwen

ID-tokens verlopen in korte tijd. Vernieuw de tokens nadat deze verlopen om toegang te kunnen krijgen tot bronnen. U kunt een token vernieuwen door een andere `POST` aanvraag naar het `/token` eind punt te verzenden. Geef deze keer de `refresh_token` para meter op in plaats van de `code` para meter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikers stroom die is gebruikt om het oorspronkelijke vernieuwings token te verkrijgen. U kunt in deze aanvraag niet een andere gebruikers stroom gebruiken. Voeg deze para meter toe aan de query reeks, niet op de hoofd tekst van het bericht. |
| client_id | Ja | De toepassings-ID die de [Azure Portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| grant_type | Ja | Het type toekenning, dat een vernieuwings token moet zijn voor dit deel van de autorisatie code stroom. |
| bereik | Nee | Een lijst met door spaties gescheiden bereiken. Het `openid` bereik geeft een machtiging aan voor het aanmelden van de gebruiker en het ophalen van gegevens over de gebruiker in de vorm van id-tokens. Het kan worden gebruikt voor het verzenden van tokens naar de eigen back-end web-API van uw toepassing, die wordt vertegenwoordigd door dezelfde toepassings-ID als de client. Het `offline_access` bereik geeft aan dat uw toepassing een vernieuwings token nodig heeft voor uitgebreide toegang tot resources. |
| redirect_uri | Nee | De `redirect_uri` para meter van de toepassing waarin u de autorisatie code hebt ontvangen. |
| refresh_token | Ja | Het oorspronkelijke vernieuwings token dat is verkregen in het tweede deel van de stroom. Het `offline_access` bereik moet worden gebruikt in de autorisatie-en Token aanvragen om een vernieuwings token te kunnen ontvangen. |
| client_secret | Ja | Het toepassings geheim dat is gegenereerd in de [Azure Portal](https://portal.azure.com/). Dit toepassings geheim is een belang rijk beveiligings artefact. U moet deze veilig opslaan op uw server. Wijzig dit client geheim op periodieke basis. |

Een geslaagd token antwoord ziet er als volgt uit:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Description |
| --------- | ----------- |
| not_before | Het tijdstip waarop het token als geldig wordt beschouwd in de epoche-tijd. |
| token_type | De waarde van het token type. `Bearer`is het enige type dat wordt ondersteund. |
| access_token | De aangevraagde ondertekende JWT-token. |
| bereik | Het bereik waarvoor het token geldig is. |
| expires_in | De tijds duur dat het toegangs token geldig is (in seconden). |
| refresh_token | Een OAuth 2,0-vernieuwings token. De toepassing kan dit token gebruiken om aanvullende tokens te verkrijgen nadat het huidige token verloopt. Vernieuwings tokens kunnen worden gebruikt om de toegang tot bronnen gedurende lange tijd te behouden. |

Fout berichten zien er als volgt uit:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Description |
| --------- | ----------- |
| fout | Een code die kan worden gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een bericht dat kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |

## <a name="send-a-sign-out-request"></a>Een afmeldings aanvraag verzenden

Wanneer u de gebruiker van de toepassing wilt ondertekenen, is het niet voldoende om de cookies van de toepassing te wissen of de sessie te beëindigen met de gebruiker. De gebruiker omleiden naar Azure AD B2C om u af te melden. Als u dit niet doet, kan de gebruiker mogelijk opnieuw worden geverifieerd bij uw toepassing zonder dat u de referenties opnieuw hoeft in te voeren.

U kunt de gebruiker gewoon omleiden naar `end_session` het eind punt dat wordt weer gegeven in het OpenID Connect-meta gegevens document dat eerder is beschreven:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| p | Ja | De gebruikers stroom die u wilt gebruiken voor het ondertekenen van de gebruiker uit uw toepassing. |
| post_logout_redirect_uri | Nee | De URL waarnaar de gebruiker wordt omgeleid na een geslaagde afmelding. Als deze niet is opgenomen, wordt in Azure AD B2C de gebruiker een Gene riek bericht weer gegeven. |

Door de gebruiker naar het `end_session` eind punt te leiden, wordt een deel van de status voor eenmalige aanmelding van de gebruiker met Azure AD B2C gewist, maar de gebruiker wordt niet van de IDP-sessie (Social ID provider) ondertekend. Als de gebruiker tijdens een volgende aanmelding dezelfde IDP selecteert, worden deze opnieuw geverifieerd zonder hun referenties in te voeren. Als een gebruiker zich wil afmelden bij de toepassing, betekent dit niet noodzakelijkerwijs dat ze zich willen afmelden bij hun Facebook-account. Als lokale accounts echter worden gebruikt, wordt de sessie van de gebruiker op de juiste wijze beëindigd.

