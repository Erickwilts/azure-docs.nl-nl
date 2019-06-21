---
title: Geavanceerd gebruik van verificatie en autorisatie - Azure App Service | Microsoft Docs
description: Laat zien hoe u verificatie en autorisatie in App Service aanpassen en gebruikers- en andere tokens ophalen.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: b628f88e3d0d644cf5a9471be1d7a766c2b9575b
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67143931"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Geavanceerd gebruik van verificatie en autorisatie in Azure App Service

Dit artikel leest u over het aanpassen van de ingebouwde [verificatie en autorisatie in App Service](overview-authentication-authorization.md), en voor het beheren van de identiteit van uw toepassing. 

Als u wilt snel aan de slag, ziet u een van de volgende zelfstudies:

* [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
* [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service voor Linux](containers/tutorial-auth-aad.md)
* [Uw app configureren voor aanmelding met Azure Active Directory](configure-authentication-provider-aad.md)
* [Uw app configureren voor aanmelding met Facebook](configure-authentication-provider-facebook.md)
* [Uw app configureren voor aanmelding met Google](configure-authentication-provider-google.md)
* [Uw app configureren voor aanmelding met een Microsoft Account](configure-authentication-provider-microsoft.md)
* [Uw app configureren voor aanmelding met Twitter](configure-authentication-provider-twitter.md)

## <a name="use-multiple-sign-in-providers"></a>Aanmelden-providers gebruiken

De configuratie van de portal biedt geen een directe manier om meerdere providers voor aanmelden bij uw gebruikers (zoals Facebook en Twitter) bieden. Het is echter niet moeilijk om de functionaliteit toevoegen aan uw app. De stappen uit worden als volgt beschreven:

Eerste in de **verificatie / autorisatie** pagina in Azure portal, het configureren van elk van de id-provider die u wilt inschakelen.

In **te ondernemen actie wanneer de aanvraag niet is geverifieerd**, selecteer **toestaan anonieme aanvragen (Er is geen actie)** .

In de aanmeldingspagina, of de navigatiebalk, of een andere locatie van uw app, kunt u een koppeling aanmelden toevoegen aan elk van de providers die u hebt ingeschakeld (`/.auth/login/<provider>`). Bijvoorbeeld:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Wanneer de gebruiker op een van de koppelingen klikt, wordt de desbetreffende pagina aanmelden geopend om te melden bij de gebruiker.

Als u wilt omleiden van de gebruiker na-aanmelden bij een aangepaste URL, gebruikt u de `post_login_redirect_url` query-tekenreeksparameter (niet te verwarren met de omleidings-URI in uw id-providerconfiguratie). Bijvoorbeeld, aan de gebruiker naar `/Home/Index` na het aanmelden, gebruik de volgende HTML-code:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Valideren van tokens van providers

In een client omgeleid aanmelden, de toepassing handmatig meldt zich aan de gebruiker met de provider en vervolgens verzendt het verificatietoken naar App Service voor de validatie (Zie [verificatiestroom](overview-authentication-authorization.md#authentication-flow)). Deze validatie zelf verlenen niet daadwerkelijk u toegang tot de gewenste app-resources, maar een succesvolle validatie krijgt u een sessietoken dat u gebruiken kunt voor toegang tot app-resources. 

Voor het valideren van de token van de provider moet App Service-app eerst worden geconfigureerd met de gewenste provider. Tijdens runtime, nadat u het verificatietoken uit uw provider ophalen, plaats het token voor `/.auth/login/<provider>` voor validatie. Bijvoorbeeld: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

De token indeling verschilt enigszins op basis van de provider. Zie de volgende tabel voor meer informatie:

| Waarde van de provider | In de aanvraagtekst is vereist | Opmerkingen |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | De `expires_in` eigenschap is optioneel. <br/>Bij het aanvragen van de token van Live-services, aanvragen altijd de `wl.basic` bereik. |
| `google` | `{"id_token":"<id_token>"}` | De `authorization_code` eigenschap is optioneel. Als u opgeeft, wordt eventueel ook kan worden gecombineerd met de `redirect_uri` eigenschap. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Gebruik een geldige [token voor gebruikerstoegang](https://developers.facebook.com/docs/facebook-login/access-tokens) uit Facebook. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Als het provider-token is gevalideerd, wordt de API retourneert met een `authenticationToken` in de hoofdtekst van het antwoord is uw sessietoken. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Zodra u dit sessietoken hebt, kunt u beveiligde app-resources openen door het toevoegen van de `X-ZUMO-AUTH` header op uw HTTP-aanvragen. Bijvoorbeeld: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Een sessie afmelden

Gebruikers kunnen een afmelding starten door het verzenden van een `GET` aanvraag van de app `/.auth/logout` eindpunt. De `GET` aanvraag doet het volgende:

- Hiermee schakelt u verificatiecookies uit de huidige sessie.
- Hiermee verwijdert u de huidige gebruiker tokens van de tokenopslag.
- Voor Azure Active Directory en Google, voert een server-side afmelden op de id-provider.

Dit is een eenvoudige afmelding koppeling in een webpagina:

```HTML
<a href="/.auth/logout">Sign out</a>
```

Standaard een geslaagde afmelden stuurt de client naar de URL `/.auth/logout/done`. U kunt de post-sign-out omleidingspagina wijzigen door toe te voegen de `post_logout_redirect_uri` queryparameter. Bijvoorbeeld:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Het is raadzaam dat u [coderen](https://wikipedia.org/wiki/Percent-encoding) de waarde van `post_logout_redirect_uri`.

Wanneer u de volledig gekwalificeerde URL's, moet de URL worden gehost in hetzelfde domein of zijn geconfigureerd als een toegestane externe Omleidings-URL voor uw app. In het volgende voorbeeld, om te leiden `https://myexternalurl.com` die niet in hetzelfde domein wordt gehost:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

U moet de volgende opdracht uitvoeren de [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>URL-fragmenten behouden

Nadat gebruikers zich bij uw app aanmelden, die meestal worden omgeleid naar dezelfde sectie van de dezelfde pagina, zoals wilt `/wiki/Main_Page#SectionZ`. Echter, omdat [URL fragmenten](https://wikipedia.org/wiki/Fragment_identifier) (bijvoorbeeld `#SectionZ`) nooit worden verzonden naar de server ze blijven niet behouden standaard nadat de OAuth-aanmelding is voltooid en terug naar de app leidt. Gebruikers krijgen vervolgens een suboptimale ervaring wanneer ze nodig hebben om te navigeren naar het gewenste anker opnieuw. Deze beperking geldt voor alle server-side ' verificatieoplossingen.

In App Service-verificatie, kunt u de URL-fragmenten behouden in de OAuth-aanmelding. Om dit te doen, stelt u een app-instelling met de naam `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` naar `true`. U kunt dit doen de [Azure-portal](https://portal.azure.com), of gewoon de volgende opdracht uit te voeren de [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Claims van de gebruiker toegang

App Service doorgegeven gebruikersclaims aan uw toepassing met behulp van speciale headers. Externe aanvragen zijn niet toegestaan om in te stellen deze headers, zodat ze aanwezig zijn alleen als ingesteld door App Service. Enkele voorbeeld-headers zijn onder andere:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Code die is geschreven in elke taal of framework kunt krijgen van de informatie die nodig is van deze headers. Voor ASP.NET 4.6 apps, de **ClaimsPrincipal** is automatisch ingesteld met de juiste waarden.

Uw toepassing vindt ook meer informatie over de geverifieerde gebruiker door het aanroepen van `/.auth/me`. De Mobile Apps-server SDK's bieden helpermethoden voor gebruik van deze gegevens. Zie voor meer informatie, [over het gebruik van de Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), en [werken met de .NET back-endserver SDK voor Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Tokens ophalen in app-code

De provider-specifieke tokens zijn opgenomen in de aanvraagheader vanuit uw servercode zodat u ze gemakkelijk kunt openen. De volgende tabel bevat de naam van de mogelijke token header:

| Provider | Naam van de header |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook-Token | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft-account | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Verzenden van uw clientcode (zoals een mobiele app of in de browser JavaScript), een HTTP `GET` aanvragen `/.auth/me`. De geretourneerde JSON is de provider-specifieke-tokens.

> [!NOTE]
> Toegangstokens zijn voor toegang tot resources van de provider, zodat ze alleen aanwezig als u uw provider configureren met een clientgeheim in. Zie voor meer informatie over vernieuwingstokens ophalen, vernieuwingstokens toegang.

## <a name="refresh-identity-provider-tokens"></a>Id-provider vernieuwingstokens

Wanneer het toegangstoken van de provider (niet de [sessietoken](#extend-session-token-expiration-grace-period)) is verlopen, moet u de gebruiker verifiëren voordat u dit token opnieuw gebruiken. U kunt verlopen van het token voorkomen door het maken van een `GET` aanroep naar de `/.auth/refresh` eindpunt van uw toepassing. Indien aangeroepen, wordt de toegangstokens te geven in het tokenarchief voor de geverifieerde gebruiker in App Service automatisch vernieuwd. De volgende aanvragen voor tokens door code van de app verkrijgen de vernieuwd tokens. Maar het tokenarchief voor tokenvernieuwing om te werken, moet bevatten [vernieuwingstokens](https://auth0.com/learn/refresh-tokens/) voor uw provider. De manier om vernieuwingstokens door elke provider worden beschreven, maar de volgende lijst bevat een korte samenvatting:

- **Google**: Toevoeg-een `access_type=offline` query-tekenreeksparameter aan uw `/.auth/login/google` API-aanroep. Als u met behulp van de Mobile Apps SDK, kunt u de parameter toevoegen aan een van de `LogicAsync` overloads (Zie [Google vernieuwen Tokens](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: Biedt geen vernieuwingstokens. Lange levensduur hebben tokens verlopen binnen 60 dagen (Zie [Facebook vervaldatum en -extensie van toegangstokens](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: Toegangstokens niet verlopen (Zie [Twitter OAuth Veelgestelde vragen over het](https://developer.twitter.com/en/docs/basics/authentication/FAQ)).
- **Microsoft-Account**: Wanneer [verificatie-instellingen voor Microsoft-Account configureren](configure-authentication-provider-microsoft.md), selecteer de `wl.offline_access` bereik.
- **Azure Active Directory**: In [ https://resources.azure.com ](https://resources.azure.com), de volgende stappen uit:
    1. Aan de bovenkant van de pagina, selecteer **lezen/schrijven**.
    2. Navigeer in de browser links naar **abonnementen** >  ** _\<abonnement\_naam_**   >  **resourceGroups** >  _ **\<resource\_groep\_naam >** _   >  **providers** > **Microsoft.Web** > **sites** >  _ **\<app \_naam >** _ > **config** > **authsettings**. 
    3. Klik op **Bewerken**.
    4. De volgende eigenschap wijzigen. Vervang  _\<app\_id >_ met Azure Active Directory-toepassings-ID van de service die u wilt openen.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. Klik op **plaatsen**. 

Als uw provider is geconfigureerd, kunt u [het vernieuwingstoken en verlooptijd voor het toegangstoken van de bovenliggende map vinden](#retrieve-tokens-in-app-code) in het tokenarchief. 

Als u wilt uw toegangstoken op elk gewenst moment vernieuwen, roept u `/.auth/refresh` in elke taal. Het volgende fragment wordt jQuery uw toegangstokens van een JavaScript-client vernieuwen.

```JavaScript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Als een gebruiker de machtigingen verleend aan uw app, de aanroep van ingetrokken `/.auth/me` kan mislukken met een `403 Forbidden` antwoord. Voor het opsporen van fouten, Controleer uw toepassingslogboeken voor meer informatie.

## <a name="extend-session-token-expiration-grace-period"></a>Sessie verlopen van het token respijtperiode uitbreiden

De geverifieerde sessie verloopt na 8 uur. Nadat een geverifieerde sessie is verlopen, moet u er een respijtperiode 72-uur is standaard. U mag de sessie met App Service zonder herverifiëren van de gebruiker vernieuwingstoken binnen deze periode. U kunt alleen aanroepen `/.auth/refresh` wanneer uw sessietoken is ongeldig, en hoeft u voor het bijhouden van verlopen van het token. Nadat de respijtperiode 72-uur afgebroken is, moet de gebruiker zich aanmelden weer naar een geldige sessie-token ophalen.

Als 72 uur niet voldoende tijd voor u is, kunt u dit venster verlopen kunt uitbreiden. Uitbreiding van de vervaldatum gedurende een lange periode kan aanzienlijke security gevolgen hebben (zoals wanneer een verificatietoken is vermist of gestolen). U moet dus 72 uur laat de standaardwaarde of de extensie is ingesteld op de kleinste waarde.

Om uit te breiden het venster van de vervaldatum standaard, kunt u de volgende opdracht uitvoeren de [Cloud Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> De respijtperiode is alleen van toepassing op de Appservice geverifieerde sessie, niet de tokens van de id-providers. Er is geen respijtperiode is voor de tokens verlopen provider. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Limiet voor het domein van e-mailaccounts

Zowel Microsoft-Account en Azure Active Directory kunt u zich aanmelden vanaf meerdere domeinen. Microsoft-Account kan bijvoorbeeld _outlook.com_, _live.com_, en _hotmail.com_ accounts. Azure Active Directory kunt een onbeperkt aantal aangepaste domeinen voor de accounts aanmelden. Dit is mogelijk ongewenste voor een interne app, die u niet dat iedereen met wilt een _outlook.com_ account voor toegang tot. Volg deze stappen om te beperken de domeinnaam van het e-mailaccounts.

In [ https://resources.azure.com ](https://resources.azure.com), gaat u naar **abonnementen** >  ** _\< abonnement\_ naam_**   >  **resourceGroups** >  _ **\< resource\_ groep\_ naam >** _   >  **providers** > **Microsoft.Web** > **sites**  >    _ **\< app\_ naam >** _ > **config** > **authsettings**. 

Klik op **bewerken**, de volgende eigenschap wijzigen en klik vervolgens op **plaatsen**. Vervang  _\<domein\_naam >_ met het domein dat u wilt.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end (Windows)](app-service-web-tutorial-auth-aad.md)
> [zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end (Linux)](containers/tutorial-auth-aad.md)
