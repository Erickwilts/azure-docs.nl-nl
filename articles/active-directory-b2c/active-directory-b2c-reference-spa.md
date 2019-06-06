---
title: Meld u één pagina in met behulp van de impliciete stroom - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het toevoegen van aanmelding met één pagina in het gebruik van de impliciete OAuth 2.0-stroom met Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a66fa70f6f5615257554e98e40e605d6a7e981fe
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508962"
---
# <a name="single-page-sign-in-using-the-oauth-20-implicit-flow-in-azure-active-directory-b2c"></a>Meld u één pagina in met behulp van de impliciete stroom voor OAuth 2.0 in Azure Active Directory B2C

Veel moderne toepassingen hebben een app met één pagina front-end die voornamelijk in JavaScript is geschreven. De app is vaak het geval is, met behulp van een framework zoals AngularJS, Ember.js of Durandal geschreven. Apps van één pagina en andere JavaScript-apps die worden uitgevoerd in een browser voornamelijk hebt enkele aanvullende uitdagingen voor verificatie:

- De beveiligingskenmerken van de van deze apps zijn anders dan traditionele server-gebaseerde webtoepassingen.
- Veel autorisatieservers en id-providers bieden geen ondersteuning voor cross-origin resource sharing (CORS) aanvragen.
- Volledige pagina browser omleidingen weg van de app kunnen worden Invasief tot de gebruikerservaring.

Ter ondersteuning van deze toepassingen, Azure Active Directory B2C (Azure AD B2C) maakt gebruik van de impliciete OAuth 2.0-stroom. De impliciete toekenning van OAuth 2.0 autorisatiestroom wordt beschreven in [sectie 4.2 van de OAuth 2.0-specificatie](https://tools.ietf.org/html/rfc6749). In de impliciete stroom, ontvangt de app tokens rechtstreeks vanuit de Azure Active Directory (Azure AD)-eindpunt, zonder een exchange server-naar-server te autoriseren. Zonder extra paginaomleidingen worden volledig uit in de JavaScript-client, alle verificatielogica en sessieafhandeling uitgevoerd.

Azure AD B2C breidt de standaard OAuth 2.0-impliciete stroom op meer dan een eenvoudige verificatie en autorisatie. Azure AD B2C introduceert de [Beleidsparameter](active-directory-b2c-reference-policies.md). Met de parameter van het beleid, kunt u OAuth 2.0 beleid toevoegen aan uw app, zoals gebruikersregistratie, aanmelding, en de gebruikersstromen management-profiel. In het voorbeeld van de HTTP-aanvragen in dit artikel **fabrikamb2c.onmicrosoft.com** wordt gebruikt als voorbeeld. U kunt vervangen `fabrikamb2c` met de naam van uw tenant als u hebt en een beleid hebt gemaakt.

De impliciete stroom aanmelden uitziet als in de volgende afbeelding. Elke stap wordt verderop in het artikel in detail beschreven.

![OpenID Connect-banen](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Verzenden van aanvragen voor authenticatie

Wanneer uw webtoepassing moet de gebruiker verifiëren en uitvoeren van de gebruikersstroom van een, het kunt instellen dat de gebruiker de `/authorize` eindpunt. De gebruiker maakt een actie afhankelijk van de gebruikersstroom.

In deze aanvraag, de client geeft aan dat de machtigingen die nodig is om te verkrijgen van de gebruiker in de `scope` parameter en de gebruikersstroom moet worden uitgevoerd de `p` parameter. Drie voorbeelden vindt u in de volgende secties (met regeleinden voor de leesbaarheid), elk met behulp van de stroom van een andere gebruiker. Ophalen van een idee van hoe elke aanvraag werkt, probeer de aanvraag in een browser plakken en uitvoeren. U kunt vervangen `fabrikamb2c` met de naam van uw tenant als u hebt en een beleid hebt gemaakt.

### <a name="use-a-sign-in-user-flow"></a>Een gebruikersstroom aanmelden gebruiken

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-user-flow"></a>Gebruik een proefaccount gebruikersstroom
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-user-flow"></a>Gebruik een gebruikersstroom profiel bewerken
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De aanvraag-ID die de [Azure-portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| response_type | Ja | Moet bevatten `id_token` voor aanmelding OpenID Connect. Kan het ook het antwoordtype bevatten `token`. Als u `token`, uw app kunt onmiddellijk een toegangstoken van het eindpunt autoriseren ontvangen zonder dat een tweede aanvraag naar het geautoriseerde eindpunt.  Als u de `token` antwoordtype, het `scope` parameter moet een scope die welke resource aangeeft voor het uitgeven van het token voor bevatten. |
| redirect_uri | Nee | De omleidings-URI van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Het moet een van de omleidings-URI's die u hebt geregistreerd in de portal exact overeenkomen, behalve dat het moet URL gecodeerd. |
| response_mode | Nee | Hiermee geeft u de methode die u wilt gebruiken voor het verzenden van het resulterende token terug naar uw app.  Gebruik voor impliciete stromen `fragment`. |
| scope | Ja | Een door spaties gescheiden lijst met bereiken. Een waarde één scope geeft u aan Azure AD zowel de machtigingen die worden aangevraagd. De `openid` bereik geeft aan dat een machtiging om te melden bij de gebruiker en gegevens over de gebruiker in de vorm van ID-tokens ophalen. De `offline_access` bereik is optioneel voor web-apps. Hiermee wordt aangegeven dat uw app nodig heeft voor een vernieuwingstoken voor lange levensduur hebben toegang tot bronnen. |
| state | Nee | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Kan het zijn een reeks van inhoud die u wilt gebruiken. Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt voor cross-site-aanvraag kunnen worden vervalst aanvallen te voorkomen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina. |
| nonce | Ja | Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die is opgenomen in de resulterende ID-token als een claim. De app kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige, unieke tekenreeks die kan worden gebruikt voor het identificeren van de oorsprong van de aanvraag. |
| p | Ja | Het beleid om uit te voeren. Dit is de naam van een beleid (beleid) dat is gemaakt in uw Azure AD B2C-tenant. De beleidswaarde voor de naam moet beginnen met **b2c\_1\_** . |
| prompt | Nee | Het type van de interactie van de gebruiker die is vereist. Op dit moment de enige geldige waarde is `login`. Deze parameter zorgt ervoor dat de gebruiker zijn referenties invoeren voor deze aanvraag. Eenmalige aanmelding wordt pas van kracht. |

Op dit moment wordt de gebruiker gevraagd om de werkstroom van het beleid te voltooien. De gebruiker mogelijk aanmelden met een sociale identiteit, meld u zich registreren voor de map of een ander getal van de stappen om in te voeren hun gebruikersnaam en wachtwoord. Acties van de gebruiker, is afhankelijk van hoe de gebruikersstroom wordt gedefinieerd.

Nadat de gebruiker is voltooid de gebruikersstroom, Azure AD retourneert een antwoord aan uw app op de waarde die u hebt gebruikt voor `redirect_uri`. Gebruikt de methode die is opgegeven in de `response_mode` parameter. Het antwoord is precies hetzelfde voor elk van de gebruiker actie scenario's, onafhankelijk van de gebruikersstroom die is uitgevoerd.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde respons die gebruikmaakt van `response_mode=fragment` en `response_type=id_token+token` er als volgt met regeleinden voor een betere leesbaarheid:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parameter | Description |
| --------- | ----------- |
| access_token | Het toegangstoken dat de app worden aangevraagd. |
| token_type | De waarde van het type token. Het enige type die ondersteuning biedt voor Azure AD is Bearer. |
| expires_in | De hoeveelheid tijd die het toegangstoken ongeldig (in seconden is). |
| scope | De bereiken die het token geldig is. U kunt ook bereiken die in de cache-tokens gebruiken voor later gebruik. |
| id_token | De ID-token dat de app worden aangevraagd. U kunt de ID-token gebruiken om te controleren of de identiteit van de gebruiker en beginnen met een sessie met de gebruiker. Zie voor meer informatie over ID-tokens en de inhoud ervan, de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). |
| state | Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app deze op de juiste wijze kan verwerken:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Description |
| --------- | ----------- |
| error | Een code die wordt gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| state | Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn.|

## <a name="validate-the-id-token"></a>De ID-token te valideren

Ontvangen van een ID-token is niet voldoende om de gebruiker te verifiëren. De ID-token handtekening valideren en controleer of de claims in het token per vereisten van uw app. Maakt gebruik van Azure AD B2C [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en cryptografie met openbare sleutels voor het ondertekenen van tokens en controleren of ze geldig zijn.

Aantal open source-bibliotheken zijn beschikbaar voor het valideren van JWTs, afhankelijk van de taal die u wilt gebruiken. Houd rekening met verkennen beschikbaar open source-bibliotheken in plaats van uw eigen validatielogica implementeren. U kunt de informatie in dit artikel voor meer informatie over het correct gebruik van deze bibliotheken.

Azure AD B2C is een eindpunt van de metagegevens OpenID Connect. Een app kan het eindpunt gebruiken voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eindpunten, de inhoud van tokens en voor token-ondertekening van sleutels. Er is een document met de JSON-metagegevens voor elke gebruikersstroom in uw Azure AD B2C-tenant. Bijvoorbeeld, bevindt het document met metagegevens voor de gebruikersstroom b2c_1_sign_in in de tenant fabrikamb2c.onmicrosoft.com zich op:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Een van de eigenschappen van dit configuratiedocument is de `jwks_uri`. De waarde voor dezelfde gebruikersstroom zou zijn:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Om te bepalen welke gebruikersstroom is gebruikt voor het ondertekenen van een ID-token (en waar om op te halen de metagegevens van), hebt u twee opties. Eerst, de gebruikersnaam van de stroom is opgenomen in de `acr` claim in `id_token`. Zie voor meer informatie over het parseren van de claims van een ID-token dat de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md). De andere mogelijkheid is het coderen van de gebruikersstroom in de waarde van de `state` parameter wanneer u de aanvraag. Vervolgens worden gedecodeerd de `state` parameter om te bepalen welke gebruikersstroom is gebruikt. Een van beide methoden is geldig.

Nadat het document met metagegevens van het eindpunt van de OpenID Connect metagegevens die u hebt gekocht, kunt u de openbare RSA-256-sleutels (te vinden op dit eindpunt) gebruiken voor het valideren van de handtekening van de ID-token. Er zijn mogelijk meerdere sleutels die worden vermeld op dit eindpunt op een bepaald moment, elk geïdentificeerd door een `kid`. De koptekst van `id_token` bevat ook een `kid` claim. Hiermee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van de ID-token. Voor meer informatie, waaronder informatie over [valideren van tokens](active-directory-b2c-reference-tokens.md), Zie de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Nadat u de handtekening van de ID-token valideert, vereisen meerdere claims verificatie. Bijvoorbeeld:

* Valideer de `nonce` claim om token opnieuw afspelen aanvallen te voorkomen. De waarde is wat u hebt opgegeven in de aanmeldingsaanvraag.
* Valideer de `aud` claim om ervoor te zorgen dat de ID-token dat is uitgegeven voor uw app. De waarde moet de toepassings-ID van uw app.
* Valideer de `iat` en `exp` claims om ervoor te zorgen dat de ID-token niet is verlopen.

Verschillende meer validaties die moeten worden uitgevoerd zijn beschreven in de [OpenID verbinding maken met Core Spec](https://openid.net/specs/openid-connect-core-1_0.html). U kunt ook om aanvullende claims, afhankelijk van uw scenario te valideren. Sommige algemene validaties zijn onder andere:

* Ervoor zorgen dat de gebruiker of de organisatie heeft aangemeld voor de app.
* Ervoor zorgen dat de gebruiker juiste verificatie en bevoegdheden heeft.
* Ervoor zorgen dat een bepaalde sterkte van verificatie heeft plaatsgevonden, zoals zoals met behulp van Azure multi-factor Authentication.

Zie voor meer informatie over de claims in een ID-token, de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md).

Nadat u de ID-token hebt gevalideerd, kunt u een sessie begint met de gebruiker. In uw app, gebruikt u de claims in het ID-token verkrijgen van informatie over de gebruiker. Deze informatie kan worden gebruikt voor het weergeven, records en autorisatie.

## <a name="get-access-tokens"></a>Toegangstokens ophalen
Als het enige wat uw web-apps moet doen gebruikersstromen uitvoeren is, kunt u de volgende gedeelten overslaan. De informatie in de volgende secties is alleen van toepassing op web-apps die u geverifieerde aanroepen naar een web-API wilt maken, en die worden beveiligd door Azure AD B2C.

Nu dat u hebt de gebruiker aangemeld bij uw app met één pagina, krijgt u toegang tot tokens voor het aanroepen van web-API's die zijn beveiligd door Azure AD. Zelfs als u al een token hebt ontvangen met behulp van de `token` antwoordtype, kunt u deze methode gebruiken om te verkrijgen van tokens voor aanvullende bronnen zonder het omleiden van de gebruiker zich opnieuw aanmelden.

In een typische web-app-stroom maakt u een aanvraag voor de `/token` eindpunt. Het eindpunt biedt echter geen CORS-aanvragen, ondersteuning, zodat de AJAX-aanroepen voor het ophalen en vernieuwen van tokens kan niet worden gebruikt. U kunt in plaats daarvan de impliciete stroom in een verborgen HTML-iframe-element gebruiken om op te halen van nieuwe-tokens voor andere web-API's. Hier volgt een voorbeeld, met regeleinden voor een betere leesbaarheid:

```

https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parameter | Vereist? | Description |
| --- | --- | --- |
| client_id |Vereist |De toepassings-ID die is toegewezen aan uw app in de [Azure-portal](https://portal.azure.com). |
| response_type |Vereist |Moet bevatten `id_token` voor aanmelding OpenID Connect.  Het reactietype het ook advies inwinnen `token`. Als u `token` hier uw app kan onmiddellijk ontvangen een toegangstoken van het geautoriseerde eindpunt, zonder dat een tweede aanvraag naar het geautoriseerde eindpunt. Als u de `token` antwoordtype, het `scope` parameter moet een scope die welke resource aangeeft voor het uitgeven van het token voor bevatten. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waarbij verificatiereacties kunnen worden verzonden en ontvangen door uw app. Deze moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het moet URL gecodeerd. |
| scope |Vereist |Een door spaties gescheiden lijst met bereiken.  Voor het ophalen van tokens, zijn alle scopes die u nodig voor de gewenste resource hebt. |
| response_mode |Aanbevolen |Hiermee geeft u de methode die wordt gebruikt voor het verzenden van het resulterende token terug naar uw app.  Kan `query`, `form_post`, of `fragment`. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die in het token antwoord wordt geretourneerd.  Kan het zijn een reeks van inhoud die u wilt gebruiken.  Normaal gesproken wordt een willekeurig gegenereerde unieke waarde gebruikt voor cross-site-aanvraag kunnen worden vervalst aanvallen te voorkomen.  De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatieaanvraag heeft plaatsgevonden. Bijvoorbeeld, de pagina of de weergave is de gebruiker op. |
| nonce |Vereist |Een waarde die is opgenomen in de aanvraag, die worden gegenereerd door de app, die is opgenomen in de resulterende ID-token als een claim.  De app kunt vervolgens controleren of deze waarde token opnieuw afspelen aanvallen te verkleinen. De waarde is doorgaans een willekeurige, unieke tekenreeks die de oorsprong van de aanvraag aanduidt. |
| prompt |Vereist |Als u wilt vernieuwen en tokens in een verborgen iframe ophalen, gebruikt u `prompt=none` om ervoor te zorgen dat het iframe niet op de pagina aanmelden zitten wordt en onmiddellijk retourneert. |
| login_hint |Vereist |Als u wilt vernieuwen en tokens verkrijgen in een verborgen iframe, bevatten de gebruikersnaam van de gebruiker in deze hint onderscheid maken tussen meerdere sessies van de gebruiker kan hebben op een bepaald moment. U kunt de gebruikersnaam van een eerdere aanmelding ophalen met behulp van de `preferred_username` claim. |
| domain_hint |Vereist |Deze waarde kan `consumers` of `organizations` zijn.  Voor het vernieuwen en ophalen van tokens in een verborgen iframe, omvatten de `domain_hint` waarde in de aanvraag.  Pak de `tid` claim van de ID-token van een eerdere aanmelding om te bepalen welke waarde te gebruiken.  Als de `tid` claim waarde `9188040d-6c67-4c5b-b112-36a304b66dad`, gebruikt u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Door in te stellen de `prompt=none` parameter, deze aanvraag ofwel is geslaagd of mislukt onmiddellijk en stuurt naar uw toepassing.  Een geslaagde reactie wordt verzonden naar uw app op de aangegeven omleidings-URI, met behulp van de methode die is opgegeven de `response_mode` parameter.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde respons met behulp van `response_mode=fragment` ziet eruit als in dit voorbeeld:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parameter | Description |
| --- | --- |
| access_token |Het token dat de app worden aangevraagd. |
| token_type |Het type token is altijd Bearer. |
| state |Als een `state` parameter is opgenomen in de aanvraag, dezelfde waarde moet worden weergegeven in het antwoord. De app moet controleren of de `state` waarden in de aanvraag en respons identiek zijn. |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| scope |De bereiken die het toegangstoken is ongeldig voor. |

### <a name="error-response"></a>Foutbericht
Foutberichten kunnen ook worden verzonden naar de omleidings-URI zodat de app deze op de juiste wijze kan verwerken.  Voor `prompt=none`, een verwachte fout ziet eruit zoals in dit voorbeeld:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Description |
| --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden. U kunt ook de tekenreeks gebruiken om te reageren op fouten. |
| error_description |Een bericht specifieke fout die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |

Als u deze fout in de iframe-aanvraag ontvangt, moet de gebruiker interactief aanmelden weer naar een nieuw token opgehaald.

## <a name="refresh-tokens"></a>Vernieuwen van tokens
ID-tokens en toegangstokens verlopen na een korte periode. Uw app moet worden voorbereid voor het periodiek vernieuwen van deze tokens.  Als u wilt beide typen token vernieuwen, moet u dezelfde verborgen iframe-aanvraag die wordt gebruikt in een eerdere voorbeeld uitvoeren met de `prompt=none` parameter voor het beheren van Azure AD-stappen.  Voor het ontvangen van een nieuwe `id_token` waarde, zorg ervoor dat u `response_type=id_token` en `scope=openid`, en een `nonce` parameter.

## <a name="send-a-sign-out-request"></a>Verzenden van een aanvragen voor afmelden
Als u de gebruiker buiten de app aanmelden wilt, moet u de gebruiker omgeleid naar Azure AD zich afmelden. Als u niet de gebruiker wordt omgeleid, is het mogelijk dat ze kunnen andere referenties voor uw app zonder hun referenties opnieuw invoeren omdat ze een geldig aanmeldings-sessie met Azure AD hebben.

U kunt gewoon de gebruiker omleiden de `end_session_endpoint` die worden vermeld in de dezelfde OpenID Connect metadata-document wordt beschreven in [valideren van de ID-token](#validate-the-id-token). Bijvoorbeeld:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Vereist? | Description |
| --- | --- | --- |
| p |Vereist |Het beleid te gebruiken om te ondertekenen van de gebruiker buiten uw toepassing. |
| post_logout_redirect_uri |Aanbevolen |De URL die de gebruiker moet worden omgeleid naar na geslaagde afmelden. Als deze niet opgenomen is, wordt Azure AD B2C een algemeen bericht voor de gebruiker. |

> [!NOTE]
> De gebruiker om te leiden de `end_session_endpoint` enkele van de status van de gebruiker eenmalige aanmelding met Azure AD B2C wordt gewist. Echter niet het de gebruiker buiten de sessie voor sociale id-provider van de gebruiker zich aanmelden. Als de gebruiker selecteert hetzelfde provider identificeren tijdens een volgende aanmelding, de gebruiker is geverifieerd, zonder hun referenties in te voeren. Als een gebruiker wil afmelden bij uw Azure AD B2C-toepassing, komt dit niet noodzakelijkerwijs dat ze willen volledig Meld u af bij hun Facebook-account, bijvoorbeeld. Echter voor lokale accounts wordt de gebruikerssessie beëindigd goed.
> 

