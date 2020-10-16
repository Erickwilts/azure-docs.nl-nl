---
title: Sessie gedrag configureren met aangepaste beleids regels-Azure Active Directory B2C | Microsoft Docs
description: Configureer het gedrag van de sessie met aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 31257d795dbd06da65e3d07e18a16d9bdf7e782a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91961099"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>Sessie gedrag configureren met aangepaste beleids regels in Azure Active Directory B2C

Met [eenmalige aanmelding (SSO)-sessie](session-overview.md) beheer in Azure Active Directory B2C (Azure AD B2C) kan een beheerder de interactie met een gebruiker beheren nadat de gebruiker al is geverifieerd. De beheerder kan bijvoorbeeld bepalen of de selectie van id-providers wordt weer gegeven of dat de account gegevens opnieuw moeten worden ingevoerd. In dit artikel wordt beschreven hoe u de SSO-instellingen voor Azure AD B2C kunt configureren.

## <a name="session-behavior-properties"></a>Eigenschappen van sessie gedrag

U kunt de volgende eigenschappen gebruiken voor het beheren van sessies van webtoepassingen:

- **Levens duur van de web-app (minuten)** : de levens duur van de Azure AD B2C's-sessie cookie die is opgeslagen in de browser van de gebruiker wanneer de verificatie is geslaagd.
  - Standaard instelling = 86400 seconden (1440 minuten).
  - Mini maal (inclusief) = 900 seconden (15 minuten).
  - Maximum (inclusief) = 86400 seconden (1440 minuten).
- **Sessietime-out van web-app** -het [type sessie verloop](session-overview.md#session-expiry-type), *Rolling*of *absoluut*. 
- **Configuratie van eenmalige aanmelding** : het [sessie bereik](session-overview.md#session-scope) van het gedrag van eenmalige aanmelding (SSO) over meerdere apps en gebruikers stromen in uw Azure AD B2C-Tenant. 

## <a name="configure-the-properties"></a>De eigenschappen configureren

Als u uw sessie-en SSO-configuraties wilt wijzigen, voegt u een **UserJourneyBehaviors** -element toe in het element [RelyingParty](relyingparty.md) .  Het **UserJourneyBehaviors** -element moet direct volgen op de **DefaultUserJourney**. Het **UserJourneyBehavors** -element moet er als volgt uitzien:

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="configure-sign-out-behavior"></a>Afmeldings gedrag configureren

### <a name="secure-your-logout-redirect"></a>Uw afmeldings omleiding beveiligen

Na afmelden wordt de gebruiker omgeleid naar de URI die is opgegeven in de `post_logout_redirect_uri` para meter, ongeacht de antwoord-url's die zijn opgegeven voor de toepassing. Als er echter een geldig `id_token_hint` wordt door gegeven en het token voor het **vereisen van een id in afmeldings aanvragen** is ingeschakeld, wordt door Azure AD B2C gecontroleerd of de waarde `post_logout_redirect_uri` overeenkomt met een van de geconfigureerde omleidings-uri's van de toepassing voordat de omleiding wordt uitgevoerd. Als er geen overeenkomende antwoord-URL voor de toepassing is geconfigureerd, wordt een fout bericht weer gegeven en wordt de gebruiker niet omgeleid. 

Als u een ID-token in afmeldings aanvragen wilt vereisen, voegt u een **UserJourneyBehaviors** -element toe in het element [RelyingParty](relyingparty.md) . Stel vervolgens de **EnforceIdTokenHintOnLogout** van het **SingleSignOn** -element in op `true` . Het **UserJourneyBehaviors** -element moet er als volgt uitzien:

```xml
<UserJourneyBehaviors>
  <SingleSignOn Scope="Tenant" EnforceIdTokenHintOnLogout="true"/>
</UserJourneyBehaviors>
```

De afmeldings-URL van de toepassing configureren:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Zorg ervoor dat u de map met uw Azure AD B2C-Tenant gebruikt door het filter **Directory + abonnement** te selecteren in het bovenste menu en de map te kiezen die uw Azure AD B2C Tenant bevat.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **app-registraties**en selecteer vervolgens uw toepassing.
1. Selecteer **Verificatie**.
1. Typ in het tekstvak **Afmeldings-URL** de omleidings-URI voor post afmelden en selecteer vervolgens **Opslaan**.

### <a name="single-sign-out"></a>Eenmalige afmelding

#### <a name="configure-the-applications"></a>De toepassingen configureren

Wanneer u de gebruiker omleidt naar het eind punt van de Azure AD B2C-afmelding (voor zowel OAuth2-als SAML-protocollen), Azure AD B2C de gebruikers sessie wissen uit de browser.  Als u [eenmalige afmelding](session-overview.md#single-sign-out)wilt toestaan, stelt u de `LogoutUrl` toepassing in op de Azure portal:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Kies uw Azure AD B2C Directory door te klikken op uw account in de rechter bovenhoek van de pagina.
1. Kies in het menu links de optie **Azure AD B2C**, selecteer **app-registraties**en selecteer vervolgens uw toepassing.
1. Selecteer **Verificatie**.
1. Typ in het tekstvak **Afmeldings-URL** de omleidings-URI voor post afmelden en selecteer vervolgens **Opslaan**.

#### <a name="configure-the-token-issuer"></a>De token Uitgever configureren 

Voor de ondersteuning van eenmalige afmelding moeten voor de token Uitgever technische profielen voor zowel JWT als SAML worden opgegeven:

- De protocol naam, zoals `<Protocol Name="OpenIdConnect" />`
- De verwijzing naar het profiel voor technische sessie, zoals `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />` .

In het volgende voor beeld ziet u de JWT-en SAML-token verleners met eenmalige afmelding:

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure AD B2C sessie](session-overview.md).
