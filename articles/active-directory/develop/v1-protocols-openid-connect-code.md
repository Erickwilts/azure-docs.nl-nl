---
title: Meer informatie over de OpenID Connect Connect-verificatie code stroom in azure AD | Microsoft Docs
description: In dit artikel wordt beschreven hoe u HTTP-berichten gebruikt om toegang te verlenen tot webtoepassingen en Web-Api's in uw Tenant met behulp van Azure Active Directory en OpenID Connect Connect.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a82571260f5da679202e96f5e6f72aa2db6788a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68834686"
---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Toegang tot webtoepassingen toestaan met OpenID Connect Connect en Azure Active Directory

[OpenID Connect Connect](https://openid.net/specs/openid-connect-core-1_0.html) is een eenvoudige identiteits laag die boven op het OAuth 2,0-protocol is gebouwd. OAuth 2,0 definieert mechanismen voor het verkrijgen en gebruiken van [**toegangs tokens**](access-tokens.md) voor toegang tot beveiligde bronnen, maar ze definiëren geen standaard methoden om identiteits gegevens op te geven. OpenID Connect Connect implementeert authenticatie als een uitbrei ding van het OAuth 2,0-autorisatie proces. Het bevat informatie over de eind gebruiker in de vorm van een [`id_token`](id-tokens.md) waarmee de identiteit van de gebruiker wordt gecontroleerd en basis profiel informatie over de gebruiker wordt verstrekt.

OpenID Connect Connect is onze aanbeveling als u een webtoepassing bouwt die wordt gehost op een server en toegankelijk is via een browser.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## <a name="authentication-flow-using-openid-connect"></a>Verificatiestroom waarbij OpenID Connect wordt gebruikt

De meest eenvoudige aanmeldings stroom bevat de volgende stappen: elk daarvan wordt hieronder beschreven.

![OpenID Connect Connect-verificatie stroom](./media/v1-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>OpenID Connect-document met metagegevens

OpenID Connect Connect beschrijft een meta gegevens document met de meeste informatie die nodig is voor een app om het aanmelden uit te voeren. Dit omvat informatie zoals de Url's die moeten worden gebruikt en de locatie van de open bare handtekeningen sleutels van de service. Het OpenID Connect Connect meta data-document vindt u op:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
De meta gegevens zijn een eenvoudig JavaScript Object Notation (JSON)-document. Raadpleeg het volgende fragment voor een voor beeld. De inhoud van het fragment wordt volledig beschreven in de [OpenID Connect Connect-specificatie](https://openid.net). Houd er rekening mee dat het leveren van `common` een Tenant-id in plaats van {Tenant} hierboven resulteert in Tenant-specifieke uri's in het JSON-object dat wordt geretourneerd.

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/{tenant}/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/{tenant}/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt",
        "client_secret_basic"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    "userinfo_endpoint":"https://login.microsoftonline.com/{tenant}/openid/userinfo",
    ...
}
```

Als uw app aangepaste handtekening sleutels heeft als gevolg van het gebruik van de functie voor het [toewijzen van claims](active-directory-claims-mapping.md) , moet `appid` u een query parameter met de app-id toevoegen om `jwks_uri` een verwijzing naar de handtekening sleutel gegevens van uw app te krijgen. Bijvoorbeeld: `https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration?appid=6731de76-14a6-49ae-97bc-6eba6914391e` bevat een `jwks_uri` van `https://login.microsoftonline.com/{tenant}/discovery/keys?appid=6731de76-14a6-49ae-97bc-6eba6914391e`.

## <a name="send-the-sign-in-request"></a>De aanmeldings aanvraag verzenden

Wanneer uw webtoepassing de gebruiker moet verifiëren, moet deze de gebruiker naar het `/authorize` eind punt sturen. Deze aanvraag is vergelijkbaar met de eerste poot van de [OAuth 2,0-autorisatie code stroom](v1-protocols-oauth-code.md), met enkele belang rijke verschillen:

* De aanvraag moet het bereik `openid` in de `scope` para meter bevatten.
* De `response_type` para meter moet `id_token`bevatten.
* De aanvraag moet de `nonce` para meter bevatten.

Een voorbeeld aanvraag zou er als volgt uitzien:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%3a12345
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter |  | Description |
| --- | --- | --- |
| tenant |vereist |De `{tenant}` waarde in het pad van de aanvraag kan worden gebruikt om te bepalen wie zich kan aanmelden bij de toepassing. De toegestane waarden zijn Tenant-id's, bijvoorbeeld `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` `common` of `contoso.onmicrosoft.com` voor Tenant-onafhankelijke tokens |
| client_id |vereist |De toepassings-ID die is toegewezen aan uw app wanneer u deze hebt geregistreerd bij Azure AD. U kunt dit vinden in de Azure Portal. Klik op **Azure Active Directory**, klik op **app**-registraties, kies de toepassing en zoek de toepassings-id op de toepassings pagina. |
| response_type |vereist |Moet zijn `id_token` inbegrepen voor OpenID Connect Connect-aanmelding. Het kan ook andere response_types omvatten, zoals `code` of. `token` |
| bereik | aanbevelingen | De OpenID Connect Connect-specificatie vereist de `openid`Scope, die wordt omgezet in de machtiging ' Meld u aan ' in de gebruikers interface van de toestemming. Deze en andere OIDC-bereiken worden genegeerd op het eind punt van de v 1.0, maar is nog steeds een best practice voor clients die voldoen aan de standaarden. |
| nonce |vereist |Een waarde die is opgenomen in de aanvraag, gegenereerd door de app, die is opgenomen in `id_token` het resultaat als een claim. De app kan vervolgens deze waarde verifiëren om token replay-aanvallen te verhelpen. De waarde is doorgaans een wille keurige, unieke teken reeks of GUID die kan worden gebruikt om de oorsprong van de aanvraag te identificeren. |
| redirect_uri | aanbevelingen |De redirect_uri van uw app, waar verificatie reacties kunnen worden verzonden en ontvangen door uw app. Het moet exact overeenkomen met een van de redirect_uris die u in de portal hebt geregistreerd, behalve het moet een URL-code ring zijn. Als dit niet het geval is, wordt de gebruikers agent teruggestuurd naar een van de omleidings-Uri's die wille keurig zijn geregistreerd voor de app. De maximale lengte is 255 bytes |
| response_mode |optioneel |Hiermee geeft u de methode op die moet worden gebruikt om de resulterende authorization_code terug te sturen naar uw app. Ondersteunde waarden zijn `form_post` voor een *http-formulier post* en `fragment` voor het *URL-fragment*. Voor webtoepassingen raden `response_mode=form_post` we u aan om te zorgen voor de veiligste overdracht van tokens naar uw toepassing. De standaard waarde voor elke stroom, `fragment`met inbegrip van een id_token.|
| toestand |aanbevelingen |Een waarde die is opgenomen in de aanvraag die wordt geretourneerd in de token reactie. Dit kan een teken reeks zijn van alle inhoud die u wilt. Een wille keurig gegenereerde unieke waarde wordt doorgaans gebruikt om [vervalsing van aanvragen op meerdere sites](https://tools.ietf.org/html/rfc6749#section-10.12)te voor komen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatie aanvraag is uitgevoerd, zoals de pagina of weer gave waarin ze zich bevonden. |
| verschijnt |optioneel |Hiermee wordt het type gebruikers interactie aangegeven dat vereist is. Op dit moment zijn de enige geldige waarden aanmelding, none en instemming. `prompt=login`Hiermee wordt de gebruiker gedwongen om hun referenties in te voeren voor die aanvraag, waarbij eenmalige aanmelding wordt genegeerd. `prompt=none`is het tegenovergestelde: Hiermee zorgt u ervoor dat de gebruiker niet in een interactieve prompt wordt weer gegeven. Als de aanvraag niet op de achtergrond kan worden voltooid via eenmalige aanmelding, retourneert het eind punt een fout. `prompt=consent`Hiermee wordt het dialoog venster OAuth-toestemming geactiveerd nadat de gebruiker zich heeft aangemeld, waarin de gebruiker wordt gevraagd om machtigingen te verlenen aan de app. |
| login_hint |optioneel |Kan worden gebruikt om het veld gebruikers naam/e-mail adres vooraf in te vullen op de aanmeldings pagina voor de gebruiker, als u de gebruikers naam van tevoren kent. Vaak gebruiken apps deze para meter tijdens de herauthenticatie, waarbij de gebruikers naam al is geëxtraheerd van een eerdere aanmelding `preferred_username` met de claim. |

Op dit moment wordt de gebruiker gevraagd om hun referenties in te voeren en de verificatie te volt ooien.

### <a name="sample-response"></a>Voorbeeldantwoord

Een voor beeld van een reactie, `redirect_uri` verzonden naar de opgegeven in de aanmeldings aanvraag nadat de gebruiker is geverifieerd, kan er als volgt uitzien:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |De `id_token` aanvraag die de app heeft aangevraagd. U kunt de `id_token` gebruiken om de identiteit van de gebruiker te verifiëren en een sessie met de gebruiker te starten. |
| toestand |Een waarde die in de aanvraag is opgenomen en die ook wordt geretourneerd in de token reactie. Een wille keurig gegenereerde unieke waarde wordt doorgaans gebruikt om [vervalsing van aanvragen op meerdere sites](https://tools.ietf.org/html/rfc6749#section-10.12)te voor komen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatie aanvraag is uitgevoerd, zoals de pagina of weer gave waarin ze zich bevonden. |

### <a name="error-response"></a>Fout bericht

Fout reacties kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan afhandelen:

```
POST / HTTP/1.1
Host: localhost:12345
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| fout |Een teken reeks voor fout codes die kan worden gebruikt voor het classificeren van typen fouten die optreden en kunnen worden gebruikt om te reageren op fouten. |
| error_description |Een specifiek fout bericht dat een ontwikkelaar kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Fout codes voor verificatie eindpunt fouten

In de volgende tabel worden de verschillende fout codes beschreven die kunnen worden geretourneerd `error` in de para meter van het fout bericht.

| Foutcode | Description | Client actie |
| --- | --- | --- |
| invalid_request |Protocol fout, zoals een ontbrekende vereiste para meter. |Corrigeer en verzend de aanvraag opnieuw. Dit is een ontwikkelings fout en wordt doorgaans onderschept tijdens de eerste test. |
| unauthorized_client |De client toepassing mag geen autorisatie code aanvragen. |Dit gebeurt meestal wanneer de client toepassing niet is geregistreerd bij Azure AD of niet is toegevoegd aan de Azure AD-Tenant van de gebruiker. De toepassing kan de gebruiker vragen met instructies voor het installeren van de toepassing en het toevoegen aan Azure AD. |
| access_denied |De resource-eigenaar heeft toestemming geweigerd |De client toepassing kan de gebruiker hiervan op de hoogte stellen dat deze niet kan door gaan tenzij de gebruiker deze heeft toegestuurd. |
| unsupported_response_type |De autorisatie server biedt geen ondersteuning voor het antwoord type in de aanvraag. |Corrigeer en verzend de aanvraag opnieuw. Dit is een ontwikkelings fout en wordt doorgaans onderschept tijdens de eerste test. |
| server_error |Er is een onverwachte fout opgetreden op de server. |Voer de aanvraag opnieuw uit. Deze fouten kunnen worden veroorzaakt door tijdelijke voor waarden. De client toepassing kan bijvoorbeeld verklaren dat het antwoord van de gebruiker is vertraagd vanwege een tijdelijke fout. |
| temporarily_unavailable |De server is tijdelijk niet actief om de aanvraag af te handelen. |Voer de aanvraag opnieuw uit. De client toepassing kan bijvoorbeeld verklaren dat het antwoord van de gebruiker is vertraagd vanwege een tijdelijke voor waarde. |
| invalid_resource |De doel resource is ongeldig omdat deze niet bestaat, Azure AD niet kan worden gevonden of niet juist is geconfigureerd. |Dit geeft aan dat de resource, indien aanwezig, niet is geconfigureerd in de Tenant. De toepassing kan de gebruiker vragen met instructies voor het installeren van de toepassing en het toevoegen aan Azure AD. |

## <a name="validate-the-id_token"></a>De id_token valideren

U ontvangt een `id_token` niet voldoende voor het verifiëren van de gebruiker. u moet de hand tekening valideren en de claims verifiëren `id_token` in de vereisten van uw app. Het Azure AD-eind punt gebruikt JSON-webtokens (JWTs) en open bare-sleutel cryptografie om tokens te ondertekenen en te controleren of ze geldig zijn.

U kunt ervoor kiezen om de `id_token` in-client code te valideren, maar een veelvoorkomende procedure `id_token` is het verzenden van het naar een back-endserver en de validatie daar uit te voeren. 

Het is ook mogelijk dat u aanvullende claims wilt valideren, afhankelijk van uw scenario. Enkele veelvoorkomende validaties zijn:

* Controleren of de gebruiker/organisatie zich heeft geregistreerd voor de app.
* Ervoor zorgen dat de gebruiker over de juiste autorisatie/ `wids` bevoegdheden `roles` beschikt met behulp van de of-claims. 
* Er is een zekere mate van verificatie uitgevoerd, zoals multi-factor Authentication.

Zodra u de `id_token`hebt gevalideerd, kunt u een sessie met de gebruiker starten en de claims in de `id_token` gebruiken om informatie over de gebruiker in uw app te verkrijgen. Deze informatie kan worden gebruikt voor weer gave, records, persoonlijke instellingen, enzovoort. Lees [Aad id_tokens](id-tokens.md)voor `id_tokens` meer informatie over en claims.

## <a name="send-a-sign-out-request"></a>Een afmeldings aanvraag verzenden

Wanneer u de gebruiker uit de app wilt ondertekenen, is het niet voldoende om de cookies van uw app te wissen of de sessie met de gebruiker te beëindigen. U moet de gebruiker ook omleiden naar `end_session_endpoint` de voor afmelden. Als u dit niet doet, kan de gebruiker opnieuw worden geverifieerd bij uw app zonder dat ze hun referenties opnieuw in te voeren, omdat ze een geldige sessie voor eenmalige aanmelding met het Azure AD-eind punt zullen hebben.

U kunt de gebruiker eenvoudigweg omleiden naar `end_session_endpoint` de lijst in het OpenID Connect Connect meta data-document:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter |  | Description |
| --- | --- | --- |
| post_logout_redirect_uri |aanbevelingen |De URL waarnaar de gebruiker wordt omgeleid na een geslaagde afmelding. Als dat niet het geval is, wordt de gebruiker een algemeen bericht weer gegeven. |

## <a name="single-sign-out"></a>Eenmalige afmelding

Wanneer u de gebruiker omleidt naar `end_session_endpoint`de, wordt de sessie van de gebruiker uit de browser gewist door Azure AD. De gebruiker kan echter nog steeds zijn aangemeld bij andere toepassingen die Azure AD gebruiken voor verificatie. Om ervoor te zorgen dat deze toepassingen de gebruiker tegelijk kunnen ondertekenen, verzendt Azure AD een HTTP GET-aanvraag `LogoutUrl` naar de registratie van alle toepassingen waarbij de gebruiker momenteel is aangemeld. Toepassingen moeten reageren op deze aanvraag door een wille keurige sessie te wissen waarmee de `200` gebruiker wordt geïdentificeerd en een antwoord wordt geretourneerd. Als u eenmalige afmeldingen wilt ondersteunen in uw toepassing, moet u deze `LogoutUrl` implementeren in de code van uw toepassing. U kunt de `LogoutUrl` van de Azure Portal instellen:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Kies uw Active Directory door te klikken op uw account in de rechter bovenhoek van de pagina.
3. Kies in het navigatie deel venster links de optie **Azure Active Directory**en kies vervolgens **app-registraties** en selecteer uw toepassing.
4. Klik op **instellingen**en vervolgens op **Eigenschappen** en zoek het tekstvak afmeldings- **URL** . 

## <a name="token-acquisition"></a>Tokens ophalen
Veel web-apps moeten de gebruiker niet alleen kunnen ondertekenen in, maar hebben ook toegang tot een webservice namens die gebruiker met behulp van OAuth. Dit scenario bevat een combi natie van OpenID Connect Connect voor gebruikers verificatie bij `authorization_code` het tegelijkertijd ophalen van een dat `access_tokens` kan worden gebruikt om de [OAuth-autorisatie code stroom](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token)te gebruiken.

## <a name="get-access-tokens"></a>Toegangs tokens ophalen
Als u toegangs tokens wilt verkrijgen, moet u de aanmeldings aanvraag van boven wijzigen:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%3a12345          // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // `form_post' or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F        // The identifier of the protected resource (web API) that your application needs access to
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Door de machtigings bereiken in de aanvraag in te `response_type=code+id_token`stellen en `authorize` te gebruiken, zorgt het eind punt ervoor dat de gebruiker heeft ingestemd `scope` met de machtigingen die zijn aangegeven in de query parameter en moet uw app een autorisatie code retour neren naar Exchange voor een toegangs token.

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagde reactie, verzonden naar `redirect_uri` het `response_mode=form_post`gebruik, ziet er als volgt uit:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Description |
| --- | --- |
| id_token |De `id_token` aanvraag die de app heeft aangevraagd. U kunt de `id_token` gebruiken om de identiteit van de gebruiker te verifiëren en een sessie met de gebruiker te starten. |
| code |De authorization_code die door de app is aangevraagd. De app kan de autorisatie code gebruiken om een toegangs token aan te vragen voor de doel bron. Authorization_codes zijn korte tijd en verlopen normaal gesp roken na ongeveer 10 minuten. |
| toestand |Als een para meter State is opgenomen in de aanvraag, moet dezelfde waarde in het antwoord worden weer gegeven. De app moet controleren of de status waarden in de aanvraag en het antwoord identiek zijn. |

### <a name="error-response"></a>Fout bericht

Fout reacties kunnen ook worden verzonden naar de `redirect_uri` zodat de app deze op de juiste wijze kan afhandelen:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Description |
| --- | --- |
| fout |Een teken reeks voor fout codes die kan worden gebruikt voor het classificeren van typen fouten die optreden en kunnen worden gebruikt om te reageren op fouten. |
| error_description |Een specifiek fout bericht dat een ontwikkelaar kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |

Zie voor een beschrijving van de mogelijke fout codes en de aanbevolen client actie [fout codes voor verificatie eindpunt fouten](#error-codes-for-authorization-endpoint-errors).

Zodra u een autorisatie `code` en een `id_token`hebt ontvangen, kunt u de gebruiker ondertekenen in en [toegangs tokens](access-tokens.md) voor hun naam krijgen. Als u de gebruiker wilt ondertekenen in, moet u `id_token` de precies zoals hierboven beschreven valideren. Als u toegangs tokens wilt ophalen, kunt u de stappen volgen die worden beschreven in de sectie ' de autorisatie code gebruiken om een toegangs token aan te vragen ' van onze documentatie voor de [OAuth-code stroom](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [toegangs tokens](access-tokens.md).
* Meer informatie over de [ `id_token` en claims](id-tokens.md).
