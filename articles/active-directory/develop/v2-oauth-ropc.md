---
title: Gebruik Microsoft identity-platform aan te melden bij de gebruikers met behulp van resource-eigenaar wachtwoord (ROPC)-referentietoekenning | Azure
description: Ondersteuning voor browser zonder verificatie van stromen met behulp van de resource-eigenaar wachtwoord-referentietoekenning.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: da111311de7b873be6453862ffcbd56fe546ea7f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482386"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-resource-owner-password-credential"></a>Microsoft identity-platform en de wachtwoordreferenties van OAuth 2.0-resource-eigenaar

Microsoft identity-platform ondersteunt de [wachtwoordreferenties van resource-eigenaar (ROPC) verlenen](https://tools.ietf.org/html/rfc6749#section-4.3), waarmee een toepassing aan te melden bij de gebruikers hun wachtwoord direct kan verwerken. De stroom ROPC een hoge mate van blootstelling van vertrouwen en de gebruiker is vereist en moet u deze stroom alleen gebruiken wanneer andere, beter te beveiligen, stromen kunnen niet worden gebruikt.

> [!IMPORTANT]
>
> * Het eindpunt van de Microsoft identity-platform ondersteunt alleen ROPC voor Azure AD-tenants, geen persoonlijke accounts. Dit betekent dat u een tenant-specifieke eindpunt moet gebruiken (`https://login.microsoftonline.com/{TenantId_or_Name}`) of de `organizations` eindpunt.
> * Persoonlijke accounts die worden uitgenodigd voor een Azure AD-tenant niet ROPC gebruiken.
> * Accounts waarvoor geen wachtwoorden kunnen niet aanmelden via ROPC. Voor dit scenario raden wij aan dat u een andere stroom voor uw app in plaats daarvan.
> * Als gebruikers multi-factor authentication (MFA) gebruiken moeten voor aanmelding bij de toepassing, wordt deze in plaats daarvan geblokkeerd.

## <a name="protocol-diagram"></a>Protocol-diagram

Het volgende diagram toont de ROPC-stroom.

![Diagram van de resource-eigenaar wachtwoord referentie-gegevensstroom](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>Autorisatie-aanvraag

De stroom ROPC is één aanvraag&mdash;deze stuurt de client-id en de referenties van gebruiker aan de id-provider en ontvangt u vervolgens de tokens in. De client moet het e-mailadres (UPN) van de gebruiker en het wachtwoord voordat u aanvragen. De client moet direct na de aanvraag is gelukt, veilig vrijgeven van de referenties van de gebruiker uit het geheugen. Het moet nooit opslaan.

> [!TIP]
> Probeer deze aanvraag wordt uitgevoerd in Postman.
> [![Probeer uit te voeren van deze aanvraag in Postman](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


```
// Line breaks and spaces are for legibility only.

POST {tenant}/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parameter | Voorwaarde | Description |
| --- | --- | --- |
| `tenant` | Vereist | De directory-tenant die u wilt vastleggen van de gebruiker in. Dit kan zijn in de beschrijvende naamindeling of GUID. Deze parameter kan niet worden ingesteld op `common` of `consumers`, maar kan worden ingesteld op `organizations`. |
| `grant_type` | Vereist | Moet worden ingesteld op `password`. |
| `username` | Vereist | E-mailadres van de gebruiker. |
| `password` | Vereist | Wachtwoord van de gebruiker. |
| `scope` | Aanbevolen | Een door spaties gescheiden lijst van [scopes](v2-permissions-and-consent.md), of de machtigingen die de app nodig heeft. In een interactieve stroom, de beheerder of de gebruiker moet toestemming geven tot deze bereiken tevoren. |

### <a name="successful-authentication-response"></a>Verificatie is geslaagd antwoord

Het volgende voorbeeld ziet u een geslaagde respons token:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parameter | Indeling | Description |
| --------- | ------ | ----------- |
| `token_type` | String | Altijd ingesteld op `Bearer`. |
| `scope` | Tekenreeksen gescheiden door spaties | Als een toegangstoken is geretourneerd, zijn deze parameter de scopes die het toegangstoken is ongeldig voor. |
| `expires_in`| int | Het aantal seconden dat de opgenomen toegangstoken is ongeldig voor. |
| `access_token`| Ondoorzichtige tekenreeks | Uitgegeven voor de [scopes](v2-permissions-and-consent.md) die zijn aangevraagd. |
| `id_token` | JWT | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen de `openid` bereik. |
| `refresh_token` | Ondoorzichtige tekenreeks | Uitgegeven als de oorspronkelijke `scope` parameter opgenomen `offline_access`. |

U kunt het vernieuwingstoken dat nieuwe toegangstokens verkrijgen en vernieuwen met behulp van dezelfde stroom wordt beschreven in de [OAuth Code flow-documentatie](v2-oauth2-auth-code-flow.md#refresh-the-access-token).

### <a name="error-response"></a>Foutbericht

Als de gebruiker de juiste gebruikersnaam of wachtwoord is niet opgegeven of de client nog niet de aangevraagde toestemming verkregen, mislukt de verificatie.

| Fout | Description | Clientactie |
|------ | ----------- | -------------|
| `invalid_grant` | De verificatie is mislukt | De referenties zijn onjuist of de client geen toestemming voor de aangevraagde bereiken. Als de scopes niet zijn verleend, een `consent_required` fout wordt geretourneerd. Als dit het geval is, moet de client de gebruiker doorsturen naar een interactieve prompt met behulp van een webweergave of in de browser. |
| `invalid_request` | De aanvraag is niet goed samengesteld. | Het machtigingstype wordt niet ondersteund op de `/common` of `/consumers` verificatie contexten.  Gebruik `/organizations` in plaats daarvan. |
| `invalid_client` | De app is niet goed ingesteld | Dit kan gebeuren als de `allowPublicClient` eigenschap niet is ingesteld op ' True ' in de [toepassingsmanifest](reference-app-manifest.md). De `allowPublicClient` eigenschap is nodig omdat de toekenning ROPC beschikt niet over een omleidings-URI. Azure AD kan niet vaststellen of de app een openbare client-toepassing of een vertrouwelijke client-toepassing, tenzij de eigenschap is ingesteld. ROPC wordt alleen ondersteund voor openbare client-apps. |

## <a name="learn-more"></a>Meer informatie

* ROPC uitproberen voor zelf met behulp van de [console voorbeeldtoepassing](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2).
* Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [beperkingen van het Microsoft identity platform](active-directory-v2-limitations.md).
