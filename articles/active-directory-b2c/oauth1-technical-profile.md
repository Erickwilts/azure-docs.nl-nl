---
title: Een technisch profiel OAuth1 definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Een technisch profiel OAuth1 definiëren in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 993fc8b2e318b59775f61de391ac75fa765485f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66513122"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technisch profiel OAuth1 definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor de [OAuth 1.0-protocol](https://tools.ietf.org/html/rfc5849) id-provider. Dit artikel beschrijft de details van een technisch profiel voor interactie met een claimprovider die ondersteuning biedt voor dit gestandaardiseerde protocol. OAuth1 technische profiel, kunt u communiceren met een OAuth1 op basis van id-provider, zoals Twitter. Federatie met de id-provider, kunnen gebruikers melden zich aan met hun bestaande sociale of ondernemings-id's.

## <a name="protocol"></a>Protocol

De **naam** kenmerk van de **Protocol** element moet worden ingesteld op `OAuth1`. Bijvoorbeeld, het protocol voor de **Twitter-OAUTH1** technische profiel is `OAuth1`.

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...    
```

## <a name="input-claims"></a>Invoerclaims

De **InputClaims** en **InputClaimsTransformations** elementen zijn leeg of niet aanwezig.

## <a name="output-claims"></a>Gebruikersclaims

De **OutputClaims** element bevat een lijst met claims die wordt geretourneerd door de OAuth1-id-provider. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de id-provider toewijzen. U kunt ook bevatten claims die niet zijn geretourneerd door de id-provider, zolang u de **DefaultValue** kenmerk.

De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.

Het volgende voorbeeld ziet u de claims die wordt geretourneerd door de Twitter-id-provider:

- De **user_id** claim die is toegewezen aan de **issuerUserId** claim.
- De **screen_name** claim die is toegewezen aan de **displayName** claim.
- De **e** claim zonder naam toewijzingen.

Het technische profiel retourneert ook claims die niet zijn geretourneerd door de id-provider: 

- De **identityProvider** claim met de naam van de id-provider.
- De **authenticationSource** claim met een standaardwaarde van `socialIdpAuthentication`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-id van de id-provider. |
| ProviderName | Nee | De naam van de id-provider. |
| request_token_endpoint | Ja | De URL van het eindpunt van de aanvraag-token aan de hand van RFC 5849. |
| authorization_endpoint | Ja | De URL van de autorisatie-eindpunt aan de hand van RFC 5849. |
| access_token_endpoint | Ja | De URL van het token-eindpunt aan de hand van RFC 5849. |
| ClaimsEndpoint | Nee | De URL van het eindpunt van de gebruiker informatie. | 
| ClaimsResponseFormat | Nee | De indeling van de claims-antwoord.|

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| client_secret | Ja | Het clientgeheim van de toepassing van id-provider.   | 

## <a name="redirect-uri"></a>Omleidings-URI

Wanneer u de omleidings-URL van uw id-provider configureren, voert u `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp`. Vervang **tenant** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **policyId** met de id van uw beleid (bijvoorbeeld b2c_1a_policy). De omleidings-URI moet zich in alleen kleine letters. Voeg een Omleidings-URL voor alle beleidsregels die gebruikmaken van aanmelding bij de id-provider. 

Als u de **b2clogin.com** domein in plaats van **login.microsoftonline.com** Zorg ervoor dat u b2clogin.com in plaats van login.microsoftonline.com.

Voorbeelden:

- [Twitter als id-provider OAuth1 toevoegen met behulp van aangepaste beleidsregels](active-directory-b2c-custom-setup-twitter-idp.md)













