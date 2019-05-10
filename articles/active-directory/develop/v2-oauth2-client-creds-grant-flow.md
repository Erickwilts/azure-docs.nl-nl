---
title: Gebruik Microsoft identity-platform voor toegang tot beveiligde resources zonder tussenkomst van de gebruiker | Azure
description: Bouw webtoepassingen met behulp van de Microsoft identity-platform-implementatie van het OAuth 2.0-verificatieprotocol.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: df75d692bc61d155b35f5ce4e2bf08da6e4cbcc3
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507107"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-client-credentials-flow"></a>Microsoft identity-platform en de OAuth 2.0-clientreferentiestroom

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

U kunt de [verlenen van OAuth 2.0-clientreferenties](https://tools.ietf.org/html/rfc6749#section-4.4) opgegeven in RFC 6749, ook wel genoemd *twee-legged OAuth*, toegang krijgen tot web gehoste bronnen met behulp van de identiteit van een toepassing. Dit type verlenen wordt vaak gebruikt voor interactie met server-naar-server die moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. Dergelijke toepassingen worden vaak aangeduid als *daemons* of *serviceaccounts*.

De referenties voor OAuth 2.0-client verlenen flow kunnen inhoud een webservice (vertrouwelijke client) voor het gebruik van zijn eigen referenties, in plaats van een gebruiker imiteren om te verifiëren bij het aanroepen van een andere webservice. In dit scenario wordt de client is doorgaans een middelste laag webservice, een daemon-service of een website. Voor een hoger niveau van zekerheid kan de Microsoft identity-platform ook de aanroepende service een certificaat (in plaats van een gedeelde geheim genoemd) gebruikt als referentie.

> [!NOTE]
> Het eindpunt van de Microsoft identity-platform biedt geen ondersteuning voor alle Azure AD-scenario's en onderdelen. Meer informatie over om te bepalen of moet u het eindpunt van de Microsoft identity-platform, [beperkingen van het Microsoft identity platform](active-directory-v2-limitations.md).

In de typische *3-legged OAuth*, een clienttoepassing is gemachtigd voor toegang tot een resource namens een specifieke gebruiker. De machtiging is overgedragen van de gebruiker voor de toepassing, meestal tijdens de [toestemming geven](v2-permissions-and-consent.md) proces. Echter, in de referenties van de client (*twee-legged OAuth*) stroom, machtigingen worden verleend om rechtstreeks naar de toepassing zelf. Wanneer de app een token aan een resource geeft, de bron wordt afgedwongen dat de app zelf is gemachtigd om uit te voeren van een actie en niet de gebruiker.

## <a name="protocol-diagram"></a>Protocol-diagram

De volledige clientreferentiestroom lijkt op het volgende diagram. We beschrijven elk van de stappen verderop in dit artikel.

![Clientreferenties-stroom](./media/v2-oauth2-client-creds-grant-flow/convergence-scenarios-client-creds.svg)

## <a name="get-direct-authorization"></a>Directe autorisatie ophalen

Een app wordt doorgaans rechtstreekse toestemming voor toegang tot een bron op twee manieren:

* [Via een toegangsbeheerlijst (ACL) op de resource](#access-control-lists)
* [Door toepassing machtiging toewijzen in Azure AD](#application-permissions)

Deze twee methoden worden het meest gebruikt in Azure AD en het is raadzaam deze voor clients en resources die de client uitvoeren clientreferenties-stroom. Een resource kan ook voor kiezen om te autoriseren van de clients op andere manieren. Elke resource-server kan de methode die het meest zinvol voor de toepassing kiezen.

### <a name="access-control-lists"></a>Toegangsbeheerlijsten

Een resourceprovider kan een autorisatie-controle op basis van een lijst van toepassing (client)-id's die deze kent en een specifiek niveau van toegang tot verleent afdwingen. Wanneer de resource een token van het eindpunt van Microsoft identity-platform ontvangt, kan het token-decodering en uitpakken van de client toepassings-ID van de `appid` en `iss` claims. Vervolgens wordt de toepassing meer veiligheid tegen een toegangsbeheerlijst (ACL) die het onderhoudt vergeleken. De granulariteit en methode van de ACL's kunnen aanzienlijk verschillen tussen resources.

Een gebruikelijk is het gebruik van een ACL testen voor een web-App of voor een web-API. De web-API kan alleen een subset van volledige machtigingen verlenen aan een specifieke client. End-to-end-tests uitvoeren op de API, maakt u een testclient die tokens van het eindpunt van de Microsoft identity platform verkrijgt en zendt deze naar de API. De API controleert vervolgens of de ACL van de testclient toepassings-ID voor volledige toegang tot de volledige functionaliteit van de API. Als u dit type ACL gebruikt, moet u niet alleen van de oproepende functie valideren `appid` waarde, maar ook valideren dat de `iss` waarde van het token wordt vertrouwd.

Dit type autorisatie is gebruikelijk voor daemons en service-accounts die toegang moeten krijgen tot gegevens die eigendom zijn van consumenten-gebruikers met persoonlijke Microsoft-accounts. Voor gegevens die eigendom zijn van organisaties, wordt u aangeraden dat u de benodigde machtiging via machtigingen van de toepassing krijgt.

### <a name="application-permissions"></a>Toepassingsmachtigingen

U kunt in plaats van ACL's, API's gebruiken om een set machtigingen van de toepassing zichtbaar te maken. De machtiging voor een toepassing te krijgen tot een toepassing door de beheerder van een organisatie en kan alleen worden gebruikt voor toegang tot gegevens die eigendom zijn van die organisatie en haar medewerkers. Microsoft Graph beschrijft bijvoorbeeld verschillende machtigingen van de toepassing het volgende doen:

* E-mail in alle postvakken lezen
* E-mail in alle postvakken lezen en schrijven
* E-mail met elke willekeurige gebruiker als afzender verzenden
* Adreslijstgegevens lezen

Voor meer informatie over de machtigingen van de toepassing, gaat u naar [Microsoft Graph](https://developer.microsoft.com/graph).

Voor het gebruik van machtigingen van de toepassing in uw app, volgt u de stappen in de volgende secties besproken.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de portal voor app-registratie van aanvragen

1. Registreren en maken van een app door middel van de nieuwe [(Preview) van de App-registraties ervaring](quickstart-register-app.md).
2. Ga naar uw toepassing in de App-registraties (Preview)-ervaring. Navigeer naar de **certificaten en geheimen** sectie en voeg een **nieuwe clientgeheim**, omdat u moet ten minste één clientgeheim om aan te vragen van een token.
3. Zoek de **API-machtigingen** sectie en voeg vervolgens de **Toepassingsmachtigingen** die uw app nodig heeft.
4. **Sla** de app-registratie.

#### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: Meld u aan de gebruiker in uw app

Wanneer u een toepassing die gebruikmaakt van machtigingen voor een toepassing bouwt, moet de app normaal gesproken een pagina of een weergave waarop de beheerder van de app-machtigingen goedkeurt. Deze pagina kan onderdeel van een van de app-aanmeldingsstroom, onderdeel van de app-instellingen, of een specifieke stroom met 'verbinding maken'. In veel gevallen is het handig voor de app weer te geven 'connect' weergave alleen nadat een gebruiker is aangemeld met een werk- of school Microsoft-account.

Als u de gebruiker zich in uw app, kunt u de organisatie waarvan de gebruiker deel uitmaakt op voordat u vraagt de gebruiker om goed te keuren van de machtigingen van de toepassing te identificeren. Hoewel niet strikt noodzakelijk is, kunt u bij het maken van een meer intuïtieve ervaring voor uw gebruikers. Volg voor het tekenen van de gebruiker in onze [Microsoft identity platform protocol zelfstudies](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>De machtigingen aanvragen van een directory-beheerder

Wanneer u klaar om aan te vragen van machtigingen van de organisatie-beheerder bent, kunt u de gebruiker omleiden naar de Microsoft identity-platform *toestemming het beheereindpunt*.

> [!TIP]
> Probeer deze aanvraag wordt uitgevoerd in Postman. (Uw eigen app-ID gebruiken voor de beste resultaten - zelfstudie-de toepassing wordt niet handig machtigingen aanvragen.) [![Uitvoeren in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser.
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Voorwaarde | Description |
| --- | --- | --- |
| `tenant` | Vereist | De directory-tenant die u wilt toestemming van aanvragen. Dit kan zijn in de beschrijvende naamindeling of GUID. Als u niet welk tenant de gebruiker behoort en u dat ze zich aanmelden met een tenant weet wilt, gebruikt u `common`. |
| `client_id` | Vereist | De **(client) toepassings-ID** die de [Azure-portal – App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) ervaring die zijn toegewezen aan uw app. |
| `redirect_uri` | Vereist | De omleidings-URI waar u het antwoord moet worden verzonden voor uw app om af te handelen. Het moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het URL-codering moet en aanvullende padsegmenten kunnen hebben. |
| `state` | Aanbevolen | Een waarde die opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van de inhoud die u wilt dat kan zijn. De status wordt gebruikt om informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |

Azure AD kunt u op dit moment afgedwongen op die alleen een tenantbeheerder zich bij volledige de aanvraag aanmelden kan. De beheerder wordt gevraagd om goed te keuren alle machtigingen direct van de toepassing die u hebt aangevraagd voor uw app in de portal van de registratie van de app.

##### <a name="successful-response"></a>Geslaagde reactie

Als de beheerder de machtigingen voor uw toepassing goedkeurt, de geslaagde respons ziet er uit als volgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Description |
| --- | --- |
| `tenant` | De directory-tenant die uw toepassing, de machtigingen die zij gevraagd, in GUID-indeling. |
| `state` | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van de inhoud die u wilt dat kan zijn. De status wordt gebruikt om informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| `admin_consent` | Ingesteld op **waar**. |

##### <a name="error-response"></a>Foutbericht

Als de beheerder worden niet goedgekeurd voor de machtigingen voor uw toepassing, de mislukte reactie ziet er uit als volgt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Description |
| --- | --- |
| `error` | Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van typen fouten, en die u kunt gebruiken om te reageren op fouten. |
| `error_description` | Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een fout identificeren. |

Nadat u een geslaagd antwoord van het eindpunt van app-inrichting ontvangen hebt, hebben de directe Toepassingsmachtigingen aangevraagde opgedaan met uw app. U kunt nu een token voor de resource die u wilt aanvragen.

## <a name="get-a-token"></a>Een token verkrijgen

Nadat u de benodigde machtiging hebt voor uw toepassing hebt verkregen, kunt u doorgaan met het verkrijgen van toegang tot tokens voor API's. Als u een token met behulp van de client clientreferenties, verzendt u een POST-aanvraag naar de `/token` Microsoft identity-platform-eindpunt:

> [!TIP]
> Probeer deze aanvraag wordt uitgevoerd in Postman. (Uw eigen app-ID gebruiken voor de beste resultaten - zelfstudie-de toepassing wordt niet handig machtigingen aanvragen.) [![Uitvoeren in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: Aanvraag voor een toegangstoken met een gedeeld geheim

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | Voorwaarde | Description |
| --- | --- | --- |
| `tenant` | Vereist | De directory-tenant de toepassing wil werken tegen in GUID of domeinnaam indeling. |
| `client_id` | Vereist | De toepassings-ID die toegewezen aan uw app. U kunt deze informatie vinden in de portal waar u uw app hebt geregistreerd. |
| `scope` | Vereist | De waarde die wordt doorgegeven voor de `scope` parameter in deze aanvraag moet de resource-id (toepassings-ID-URI) van de resource die u wilt, aangebracht met de `.default` achtervoegsel. De waarde voor de Microsoft Graph-bijvoorbeeld heeft `https://graph.microsoft.com/.default`. <br/>Deze waarde geeft het eindpunt van de Microsoft identity-platform dat van alle rechtstreekse Toepassingsmachtigingen die u hebt geconfigureerd voor uw app, het eindpunt moet uitgeven van een token voor de regels die zijn gekoppeld aan de resource die u wilt gebruiken. Voor meer informatie over de `/.default` scope, raadpleegt u de [toestemming geven documentatie](v2-permissions-and-consent.md#the-default-scope). |
| `client_secret` | Vereist | Het clientgeheim die u voor uw app in de portal van de registratie van de app hebt gegenereerd. Het clientgeheim moet URL gecodeerd voordat het wordt verzonden. |
| `grant_type` | Vereist | Moet worden ingesteld op `client_credentials`. |

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: Aanvraag voor een toegangstoken met een certificaat

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

| Parameter | Voorwaarde | Description |
| --- | --- | --- |
| `tenant` | Vereist | De directory-tenant de toepassing wil werken tegen in GUID of domeinnaam indeling. |
| `client_id` | Vereist |De toepassing (client)-ID die toegewezen aan uw app. |
| `scope` | Vereist | De waarde die wordt doorgegeven voor de `scope` parameter in deze aanvraag moet de resource-id (toepassings-ID-URI) van de resource die u wilt, aangebracht met de `.default` achtervoegsel. De waarde voor de Microsoft Graph-bijvoorbeeld heeft `https://graph.microsoft.com/.default`. <br/>Deze waarde informeert het eindpunt van de Microsoft identity-platform dat van alle rechtstreekse Toepassingsmachtigingen die u voor uw app hebt geconfigureerd, er moet worden verleend een token voor de regels die zijn gekoppeld aan de resource die u wilt gebruiken. Voor meer informatie over de `/.default` scope, raadpleegt u de [toestemming geven documentatie](v2-permissions-and-consent.md#the-default-scope). |
| `client_assertion_type` | Vereist | De waarde moet worden ingesteld op `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion` | Vereist | Een bewering (een JSON webtoken) die u wilt maken en te ondertekenen met het certificaat dat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de verklaring.|
| `grant_type` | Vereist | Moet worden ingesteld op `client_credentials`. |

U ziet dat de parameters bijna hetzelfde als in het geval van de aanvraag van het gedeelde geheim zijn, behalve dat de waarde voor client_secret-parameter is vervangen door twee parameters: een client_assertion_type en client_assertion.

### <a name="successful-response"></a>Geslaagde reactie

Een geslaagd antwoord ziet er zo uit:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Description |
| --- | --- |
| `access_token` | Het aangevraagde toegangstoken. De app kunt u dit token gebruiken om te verifiëren met de beveiligde bron, zoals een Web-API. |
| `token_type` | Geeft aan dat de waarde van het token. Het enige type die door Microsoft identity-platform ondersteunt is `bearer`. |
| `expires_in` | De hoeveelheid tijd die een toegangstoken geldig is (in seconden). |

### <a name="error-response"></a>Foutbericht

Reactie op een fout ziet er als volgt:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Description |
| --- | --- |
| `error` | Een tekenreeks voor de foutcode die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| `error_description` | Een bericht specifieke fout die u kan helpen bij de hoofdoorzaak van een verificatiefout identificeren. |
| `error_codes` | Een lijst van de STS-specifieke foutcodes die u met diagnostische gegevens helpen kunnen. |
| `timestamp` | De tijd waarop de fout is opgetreden. |
| `trace_id` | Een unieke id voor de aanvraag om te helpen met diagnostische gegevens. |
| `correlation_id` | Een unieke id voor de aanvraag om te helpen met diagnostische gegevens voor onderdelen. |

> [!NOTE]
> U kunt het manifestbestand van de toepassing in azure portal bijwerken in volgorde voor uw toepassing kunnen zijn voor het ontvangen van de v2-token. U kunt het kenmerk toevoegen `accessTokenAcceptedVersion` en stel de waarde op 2 als `"accessTokenAcceptedVersion": 2`. Controleer of het artikel [toepassingsmanifest](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-reference) voor meer informatie over hetzelfde. Standaard de toepassing op dit moment krijgen een v1-token. Als dit niet is gedefinieerd in het manifest toepassing/Web-API, wordt de waarde voor dit kenmerk in het manifest standaard ingesteld op 1 en kan daarom de toepassing v1-token ontvangt.  


## <a name="use-a-token"></a>Een token gebruiken

Nu dat u hebt een token hebt aangeschaft, kunt u het token gebruiken om aan te vragen naar de resource. Wanneer het token is verlopen, herhaalt u de aanvraag voor de `/token` eindpunt om een nieuwe toegangstoken te verkrijgen.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try the following command! (Replace the token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-samples-and-other-documentation"></a>Codevoorbeelden en andere documentatie

Lees de [clientreferenties overzicht documentatie](https://aka.ms/msal-net-client-credentials) van de Microsoft Authentication Library

| Voorbeeld | Platform |Description |
|--------|----------|------------|
|[active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) | .NET core 2.1-Console | Een eenvoudige .NET Core-toepassing die de gebruikers van een tenant met behulp van de identiteit van de toepassing namens een gebruiker in plaats van Microsoft Graph uitvoeren van query's weergegeven. Het voorbeeld ziet u ook de variatie met behulp van certificaten voor verificatie. |
|[active-directory-dotnet-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)|ASP.NET MVC | Een webtoepassing die worden gesynchroniseerd met gegevens uit met behulp van de identiteit van de toepassing namens een gebruiker in plaats van Microsoft Graph. |
