---
title: Microsoft identity-platform en OAuth 2.0 op namens-stroom | Azure
description: In dit artikel wordt beschreven hoe u gebruik van HTTP-berichten voor het implementeren van service-to-service-verificatie met behulp van de OAuth 2.0 op namens-stroom.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce0c1c4dcf7e4ff0c82157af83aa15544cf092e2
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544755"
---
# <a name="microsoft-identity-platform-and-oauth-20-on-behalf-of-flow"></a>Microsoft identity-platform en OAuth 2.0 namens-stroom

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

De OAuth 2.0 namens-stroom (OBO) fungeert de use-case die waar een toepassing een service of web-API aanroept die op zijn beurt moet een andere service/web-API aanroepen. Het idee is dat de gemachtigde gebruiker identiteits- en machtigingen via de aanvraagketen doorgegeven. Voor de middelste laag service voor geverifieerde aanvragen versturen naar de downstream-service, moet voor het beveiligen van een toegangstoken van de Microsoft identity-platform, namens de gebruiker.

> [!NOTE]
>
> - Het eindpunt van de Microsoft identity-platform biedt geen ondersteuning voor alle scenario's en onderdelen. Meer informatie over om te bepalen of moet u het eindpunt van de Microsoft identity-platform, [beperkingen van het Microsoft identity platform](active-directory-v2-limitations.md). Met name worden niet bekend clienttoepassingen ondersteund voor apps met Microsoft-account (MSA) en Azure AD doelgroepen. Een algemeen patroon toestemming voor OBO werkt dus niet voor clients die zich in zowel persoonlijke en werk-of schoolaccount. Zie voor meer informatie over het afhandelen van deze stap van de stroom, [krijgen toestemming voor de middelste laag-toepassing](#gaining-consent-for-the-middle-tier-application).
> - Vanaf mei 2018, bepaalde impliciete stroom afgeleid `id_token` voor OBO stroom kan niet worden gebruikt. Apps van één pagina (kuuroorden) moeten worden verwerkt een **toegang** token gebruikt voor een middelste laag vertrouwelijke client om uit te voeren OBO stromen in plaats daarvan. Zie voor meer informatie over welke clients OBO aanroepen kunnen uitvoeren, [beperkingen](#client-limitations).

## <a name="protocol-diagram"></a>Protocol-diagram

Wordt ervan uitgegaan dat de gebruiker is geverifieerd op een toepassing met behulp van de [OAuth 2.0-autorisatiecode verlenen stroom](v2-oauth2-auth-code-flow.md). Op dit moment de toepassing heeft een toegangstoken *voor API A* (token A) met claims van de gebruiker en toestemming voor toegang tot de middelste laag web-API (A-API). Nu moet API A maken van een geverifieerde aanvraag naar de downstream web-API (API-B).

De volgende stappen de stroom OBO vormen en met behulp van het volgende diagram worden uitgelegd.

![OAuth 2.0 op namens-stroom](./media/v2-oauth2-on-behalf-of-flow/protocols-oauth-on-behalf-of-flow.png)

1. De clienttoepassing een aanvraag doet bij API A met A-token (met een `aud` claim van API-A).
1. API A wordt geverifieerd op het eindpunt van de Microsoft identity platform token-uitgifte en vraagt een token voor toegang tot API B.
1. Het eindpunt van de Microsoft identity platform token-uitgifte van de A-API-referenties met een token valideert en problemen met het toegangstoken voor API-B (token B).
1. Token B is ingesteld in de autorisatie-header van de aanvraag voor API B.
1. Gegevens van de beveiligde resource wordt geretourneerd door API B.

> [!NOTE]
> In dit scenario heeft de middelste laag service geen tussenkomst van de gebruiker om op te halen van de gebruiker toestemming voor toegang tot de downstream-API. De optie voor het verlenen van toegang tot de downstream API daarom is vooraf verschijnt als een deel van de toestemming stap tijdens de verificatie is. Zie voor meer informatie over deze instellen voor uw app, [krijgen toestemming voor de middelste laag-toepassing](#gaining-consent-for-the-middle-tier-application).

## <a name="service-to-service-access-token-request"></a>Service-naar-service toegangstokenaanvraag

Om aan te vragen een toegangstoken, moet u een HTTP POST maken voor de tenant-specifieke Microsoft identity platform token-eindpunt met de volgende parameters.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

Er zijn twee mogelijke situaties, afhankelijk van of de clienttoepassing moet worden beveiligd door een gedeeld geheim of een certificaat kiest.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: Aanvraag voor een toegangstoken met een gedeeld geheim

Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor de service-naar-service toegang tot de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| `grant_type` | Vereist | Het type token aan te vragen. Voor een aanvraag met behulp van een JWT, de waarde moet `urn:ietf:params:oauth:grant-type:jwt-bearer`. |
| `client_id` | Vereist | De toepassing (client)-ID die [Azure portal - App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina is toegewezen aan uw app. |
| `client_secret` | Vereist | Het clientgeheim die u hebt gegenereerd voor uw app in Azure portal - pagina voor App-registraties. |
| `assertion` | Vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| `scope` | Vereist | Een spatie gescheiden lijst met bereiken voor het token aan te vragen. Zie voor meer informatie, [scopes](v2-permissions-and-consent.md). |
| `requested_token_use` | Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom OBO de waarde moet worden ingesteld op `on_behalf_of`. |

#### <a name="example"></a>Voorbeeld

De volgende HTTP-POST vraagt een toegangstoken en een vernieuwingstoken met `user.read` voor het bereik van de https://graph.microsoft.com web-API.

```
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE0OTM5MjA5MTYsIm5iZiI6MTQ5MzkyMDkxNiwiZXhwIjoxNDkzOTI0ODE2LCJhaW8iOiJBU1FBMi84REFBQUFnZm8vNk9CR0NaaFV2NjJ6MFFYSEZKR0VVYUIwRUlIV3NhcGducndMMnVrPSIsIm5hbWUiOiJOYXZ5YSBDYW51bWFsbGEiLCJvaWQiOiJkNWU5NzljNy0zZDJkLTQyYWYtOGYzMC03MjdkZDRjMmQzODMiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuYWNhbnVtYUBtaWNyb3NvZnQuY29tIiwic3ViIjoiZ1Q5a1FMN2hXRUpUUGg1OWJlX1l5dVZNRDFOTEdiREJFWFRhbEQzU3FZYyIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjN5U3F4UHJweUVPd0ZsTWFFMU1PQUEiLCJ2ZXIiOiIyLjAifQ.TPPJSvpNCSCyUeIiKQoLMixN1-M-Y5U0QxtxVkpepjyoWNG0i49YFAJC6ADdCs5nJXr6f-ozIRuaiPzy29yRUOdSz_8KqG42luCyC1c951HyeDgqUJSz91Ku150D9kP5B9-2R-jgCerD_VVuxXUdkuPFEl3VEADC_1qkGBiIg0AyLLbz7DTMp5DvmbC09DhrQQiouHQGFSk2TPmksqHm3-b3RgeNM1rJmpLThis2ZWBEIPx662pjxL6NJDmV08cPVIcGX4KkFo54Z3rfwiYg4YssiUc4w-w3NJUBQhnzfTl4_Mtq2d7cVlul9uDzras091vFy32tWkrpa970UvdVfQ
&scope=https://graph.microsoft.com/user.read+offline_access
&requested_token_use=on_behalf_of
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: Aanvraag voor een toegangstoken met een certificaat

Een service-naar-service toegangstokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| `grant_type` | Vereist | Het type van het token aan te vragen. Voor een aanvraag met behulp van een JWT, de waarde moet `urn:ietf:params:oauth:grant-type:jwt-bearer`. |
| `client_id` | Vereist |  De toepassing (client)-ID die [Azure portal - App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina is toegewezen aan uw app. |
| `client_assertion_type` | Vereist | De waarde moet `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion` | Vereist | Een bewering (een JSON webtoken) die u wilt maken en te ondertekenen met het certificaat dat u geregistreerd als referenties voor uw toepassing. Zie voor meer informatie over het registreren van uw certificaat en de indeling van de verklaring [referenties van het certificaat](active-directory-certificate-credentials.md). |
| `assertion` | Vereist | De waarde van het token wordt gebruikt in de aanvraag. |
| `requested_token_use` | Vereist | Hiermee geeft u op hoe de aanvraag moet worden verwerkt. In de stroom OBO de waarde moet worden ingesteld op `on_behalf_of`. |
| `scope` | Vereist | Een door spaties gescheiden lijst met bereiken voor het token aan te vragen. Zie voor meer informatie, [scopes](v2-permissions-and-consent.md).|

U ziet dat de parameters zijn bijna hetzelfde als het in het geval van de aanvraag van het gedeelde geheim, behalve dat de `client_secret` parameter wordt vervangen door twee parameters: een `client_assertion_type` en `client_assertion`.

#### <a name="example"></a>Voorbeeld

De volgende HTTP-POST vraagt een toegangstoken met `user.read` voor het bereik van de https://graph.microsoft.com web-API met een certificaat.

```
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read+offline_access
```

## <a name="service-to-service-access-token-response"></a>Service aan service access token antwoord

Een geslaagde reactie is een JSON OAuth 2.0-antwoord met de volgende parameters.

| Parameter | Description |
| --- | --- |
| `token_type` | Geeft aan dat de waarde van het token. Het enige type die door Microsoft identity-platform ondersteunt is `Bearer`. Zie voor meer informatie over het bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| `scope` | Het bereik van de toegang is verleend in het token. |
| `expires_in` | De lengte van de tijd in seconden die het toegangstoken ongeldig is. |
| `access_token` | Het aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om te verifiëren bij de ontvangende service. |
| `refresh_token` | Het vernieuwingstoken dat voor de aangevraagde toegangstoken. De aanroepende service kunt u dit token gebruiken om aan te vragen van een andere toegangstoken nadat het huidige toegangstoken is verlopen. Het vernieuwingstoken dat is alleen beschikbaar als de `offline_access` bereik is aangevraagd. |

### <a name="success-response-example"></a>Voorbeeld van de reactie geslaagd

Het volgende voorbeeld toont een geslaagd antwoord op een verzoek om een token voor de https://graph.microsoft.com web-API.

```
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4AnqPnvFqnzS3GiikHr5wBM1bV1YyjH3nUeIhKhqJWGwqJFRqs2sE_rqUfz7__3J92JDpi6gDdCZNNaXgreQsH89kLCVNYZeN6kGuFGZrjwxp1wS2JYc97E_3reXBxkHrA09K5aR-WsSKCEjf6WI23FhZMTLhk_ZKOe_nWvcvLj13FyvSrTMZV2cmzyCZDqEHtPVLJgSoASuQlD2NXrfmtcmgWfc3uJSrWLIDSn4FEmVDA63X6EikNp9cllH3Gp7Vzapjlnws1NQ1_Ff5QrmBHp_LKEIwfzVKnLLrQXN0EzP8f6AX6fdVTaeKzm7iw6nH0vkPRpUeLc3q_aNsPzqcTOnFfgng7t2CXUsMAGH5wclAyFCAwL_Cds7KnyDLL7kzOS5AVZ3Mqk2tsPlqopAiHijZaJumdTILDudwKYCFAMpUeUwEf9JmyFjl2eIWPmlbwU7cHKWNvuRCOYVqbsTTpJthwh4PvsL5ov5CawH_TaV8omG_tV6RkziHG9urk9yp2PH9gl7Cv9ATa3Vt3PJWUS8LszjRIAJmyw_EhgHBfYCvEZ8U9PYarvgqrtweLcnlO7BfnnXYEC18z_u5wemAzNBFUje2ttpGtRmRic4AzZ708tBHva2ePJWGX6pgQbiWF8esOrvWjfrrlfOvEn1h6YiBW291M022undMdXzum6t1Y1huwxHPHjCAA"
}
```

> [!NOTE]
> Het bovenstaande toegangstoken is een token v1.0-indeling. Dit is omdat het token is opgegeven op basis van de bron waartoe toegang wordt verkregen. De Microsoft Graph-aanvragen v1.0 tokens, zodat Microsoft identity-platform v1.0 toegangstokens levert wanneer een client een aanvraag tokens voor Microsoft Graph. Alleen toepassingen te toegangstokens bekijken. Clients nodig controleren ze niet.

### <a name="error-response-example"></a>Foutvoorbeeld antwoord

Reactie op een fout wordt geretourneerd door het tokeneindpunt wanneer u wilt een toegangstoken verkrijgen voor de downstream-API, als de downstream-API een beleid voor voorwaardelijke toegang (zoals meervoudige verificatie heeft) ingesteld. De middelste laag service moet deze fout naar de clienttoepassing surface, zodat de clienttoepassing kan de tussenkomst van de gebruiker om te voldoen aan het beleid voor voorwaardelijke toegang verlenen.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Gebruik het toegangstoken voor toegang tot de beveiligde bron

Nu de middelste laag service het token verkregen gebruiken kunt boven aan de geverifieerde aanvragen versturen naar de downstream web-API, door het instellen van het token in de `Authorization` header.

### <a name="example"></a>Voorbeeld

```
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## <a name="gaining-consent-for-the-middle-tier-application"></a>Krijgen toestemming voor de middelste laag-toepassing

Afhankelijk van de doelgroep voor uw toepassing, kunt u overwegen om verschillende strategieën om ervoor te zorgen dat de stroom OBO voltooid is. In alle gevallen moet is het uiteindelijke doel om ervoor te zorgen goede toestemming krijgt. Hoe dat, maar gebeurt is afhankelijk van welke gebruikers uw toepassing ondersteunt.

### <a name="consent-for-azure-ad-only-applications"></a>Toestemming voor Azure AD alleen-lezen-toepassingen

#### <a name="default-and-combined-consent"></a>/.Default en gecombineerde toestemming

Voor toepassingen waarvoor alleen aanmelden werk of school-accounts, is de traditionele aanpak voor "Clienttoepassingen bekend" voldoende. De middelste laag-toepassing de client toegevoegd aan de lijst met bekende clients toepassingen in het manifest en vervolgens de client een gecombineerde instemmingsstroom zelf en de middelste laag-toepassing kunt activeren. Op het eindpunt van Microsoft identity-platform, dit wordt gedaan met behulp van de [ `/.default` bereik](v2-permissions-and-consent.md#the-default-scope). Bij het activeren van een instemmingsscherm met behulp van bekende clienttoepassingen en `/.default`, het instemmingsscherm worden machtigingen voor zowel de client op de middelste laag API weergeven en ook vragen welke machtigingen zijn vereist voor de middelste laag-API. De gebruiker heeft ingestemd voor beide toepassingen en vervolgens de stroom OBO werkt.

Op dit moment gecombineerde toestemming biedt geen ondersteuning voor het persoonlijke Microsoft-accountsysteem en deze benadering werkt dus niet voor apps die u wilt aanmelden specifiek persoonlijke accounts. Persoonlijke Microsoft-accounts wordt gebruikt als Gast-account in een tenant worden verwerkt met behulp van het Azure AD-systeem en gecombineerde toestemming kunnen doorlopen.

#### <a name="pre-authorized-applications"></a>Vooraf gemachtigde toepassingen

Een functie van de portal van de toepassing is 'vooraf gemachtigde toepassingen'. Op deze manier kan een resource geeft aan dat een bepaalde toepassing altijd gemachtigd is voor het ontvangen van bepaalde bereiken. Dit is vooral handig om verbindingen te maken tussen een front-end-client en een back-end-bron biedt een naadloze ervaring. Een resource kan meerdere vooraf gemachtigde toepassingen declareren - een dergelijke toepassing kan aanvragen deze machtigingen in een OBO flow en ontvangt zonder dat de gebruiker die toestemming verleent.

#### <a name="admin-consent"></a>Toestemming van de beheerder

Een tenantbeheerder kan garanderen dat toepassingen kan worden gemachtigd om aan te roepen hun vereist API's door te geven van toestemming van een beheerder voor de middelste laag-toepassing. U doet dit door de beheerder de middelste laag-toepassing in hun tenant niet vinden, opent u de pagina van de vereiste machtigingen en wilt machtigen voor de app. Zie voor meer informatie over de toestemming van een beheerder, de [machtigingen en toestemming documentatie](v2-permissions-and-consent.md).

### <a name="consent-for-azure-ad--microsoft-account-applications"></a>Toestemming voor op Azure AD en Microsoft-account-toepassingen

Vanwege de beperkingen in het machtigingenmodel voor persoonlijke accounts en het ontbreken van een geldende tenant, moet aan de vereisten toestemming voor persoonlijke accounts zijn iets anders uit Azure AD. Er is geen tenant toestemming voor tenant-brede en is er de mogelijkheid voor gecombineerde toestemming geven. Dus, andere strategieën aanwezig zelf - Houd er rekening mee dat dit voor toepassingen die alleen nodig werkt voor de ondersteuning van Azure AD-accounts.

#### <a name="use-of-a-single-application"></a>Gebruik van één toepassing

In sommige scenario's kan u slechts één koppeling van de middelste laag en front-client hebben. In dit scenario wordt wellicht vindt u het eenvoudiger te kunnen dit één toepassing, zodat de noodzaak van een toepassing voor de middelste laag kan worden overgeslagen. Als u wilt verifiëren tussen de front-end en de web-API, kunt u cookies, een id_token of een toegangstoken aangevraagd voor de toepassing zelf. Vervolgens, aanvragen toestemming van deze één toepassing naar de back-end-bron.

## <a name="client-limitations"></a>Clientbeperkingen

Als een client de impliciete stroom gebruikt om op te halen van een id_token en dat de client ook jokertekens in een antwoord-URL heeft, kan de id_token kan niet worden gebruikt voor een OBO-stroom.  Toegangstokens die zijn verkregen via de impliciete stroom kunnen echter nog steeds worden ingewisseld door een vertrouwelijke client, zelfs als de initiërende client een antwoord-URL van het jokerteken geregistreerd heeft.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het OAuth 2.0-protocol en een andere manier voor het uitvoeren van service-to-service-verificatie met behulp van clientreferenties.

* [Referenties voor OAuth 2.0-client verlenen in Microsoft identity-platform](v2-oauth2-client-creds-grant-flow.md)
* [Stroom voor OAuth 2.0-code in Microsoft identity-platform](v2-oauth2-auth-code-flow.md)
* [Met behulp van de `/.default` bereik](v2-permissions-and-consent.md#the-default-scope)
