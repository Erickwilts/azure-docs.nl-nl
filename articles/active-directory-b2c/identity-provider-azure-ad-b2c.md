---
title: Registratie instellen en aanmelden met een Azure AD B2C account van een andere Azure AD B2C Tenant
titleSuffix: Azure AD B2C
description: Bied u de mogelijkheid om zich aan te melden bij klanten met Azure AD B2C accounts van een andere Tenant in uw toepassingen met behulp van Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/15/2021
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit, project-no-code
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 8a0d69ea57eb5b8b2a074c37d4798a99c576ce95
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2021
ms.locfileid: "98538174"
---
# <a name="set-up-sign-up-and-sign-in-with-an-azure-ad-b2c-account-from-another-azure-ad-b2c-tenant"></a>Registratie instellen en aanmelden met een Azure AD B2C account van een andere Azure AD B2C Tenant

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u een Federatie met een andere Azure AD B2C Tenant instelt. Wanneer uw toepassingen met uw Azure AD B2C worden beveiligd, kunnen gebruikers van andere Azure AD-B2C's zich aanmelden met hun bestaande accounts. In het volgende diagram kunnen gebruikers zich aanmelden bij een toepassing die wordt beveiligd door de Azure AD B2C van *Contoso*, met een account dat wordt beheerd door de Azure AD B2C Tenant van *fabrikam* 

![Federatie Azure AD B2C met een andere Azure AD B2C Tenant](./media/identity-provider-azure-ad-b2c/azure-ad-b2c-federation.png)


## <a name="prerequisites"></a>Vereisten

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Als u aanmelden wilt inschakelen voor gebruikers met een account van een andere Azure AD B2C Tenant (bijvoorbeeld fabrikam), in uw Azure AD B2C (bijvoorbeeld Contoso):

1. Maak een [gebruikers stroom](tutorial-create-user-flows.md)of een [aangepast beleid](custom-policy-get-started.md).
1. Maak vervolgens een toepassing in de Azure AD B2C, zoals beschreven in deze sectie. 

Om een toepassing te maken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Zorg ervoor dat u de map gebruikt die uw andere Azure AD B2C Tenant bevat (bijvoorbeeld Fabrikam.com).
1. Zoek en selecteer **Azure AD B2C** in de Azure-portal.
1. Selecteer **App-registraties** en selecteer vervolgens **Nieuwe registratie**.
1. Voer een **naam** in voor de toepassing. Bijvoorbeeld *ContosoApp*.
1. Selecteer onder **Ondersteunde accounttypen** **Accounts in een identiteitsprovider of organisatieadreslijst (voor het verifiëren van gebruikers met gebruikersstromen)** .
1. Onder **omleidings-URI** selecteert u **Web** en voert u de volgende URL in in kleine letters, waar `your-B2C-tenant-name` wordt vervangen door de naam van uw Azure AD B2C Tenant (bijvoorbeeld Contoso).

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Bijvoorbeeld `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

1. Schakel onder Machtigingen het selectie vakje **verlenen beheerder toestemming geven aan OpenID Connect-en offline_access machtigingen** in.
1. Selecteer **Registreren**.
1. Selecteer op de pagina **Azure AD B2C-app-registraties** de toepassing die u hebt gemaakt, bijvoorbeeld *ContosoApp*.
1. Noteer de **id van de toepassing (client)** die wordt weer gegeven op de overzichts pagina van de toepassing. U hebt deze nodig wanneer u de ID-provider in de volgende sectie configureert.
1. Selecteer in het linkermenu onder **Beheren** de optie **Certificaten en geheimen**.
1. Selecteer **Nieuw clientgeheim**.
1. Voer een beschrijving voor het clientgeheim in het vak **Beschrijving** in. Bijvoorbeeld *clientsecret1*.
1. Selecteer onder **Verloopt** een duur waarvoor het geheim geldig is en selecteer vervolgens **Toevoegen**.
1. Noteer de **Waarde** van het geheim. U hebt deze nodig wanneer u de ID-provider in de volgende sectie configureert.


::: zone pivot="b2c-user-flow"

## <a name="configure-azure-ad-b2c-as-an-identity-provider"></a>Azure AD B2C configureren als een id-provider

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Zorg ervoor dat u de Directory gebruikt die de Azure AD B2C Tenant bevat waarvan u de Federatie wilt configureren (bijvoorbeeld Contoso). Selecteer het filter **Directory + abonnement** in het bovenste menu en kies de map die uw Azure AD B2C Tenant bevat (bijvoorbeeld Contoso).
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **Id-providers** en selecteer vervolgens **Nieuwe OpenID Connect-provider**.
1. Voer een **naam** in. Voer bijvoorbeeld *fabrikam* in.
1. Voor **meta gegevens-URL** voert u de volgende URL `{tenant}` in die wordt vervangen door de domein naam van uw Azure AD B2C Tenant (bijvoorbeeld fabrikam). Vervang de `{policy}` door de naam van het beleid dat u in de andere Tenant configureert:

    ```
    https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/v2.0/.well-known/openid-configuration
    ```

    Bijvoorbeeld `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/B2C_1_susi/v2.0/.well-known/openid-configuration`.

1. Voor **Client-id** voert u de toepassings-id in die u eerder hebt genoteerd.
1. Voor **Clientgeheim** voert u het clientgeheim in dat u eerder hebt genoteerd.
1. Voer de `openid` in voor **Bereik**.
1. Behoud de standaardwaarden voor **Antwoordtype** en **Antwoordmodus**.
1. Beschrijving Voer voor de **domein Hint** de domein naam in die u wilt gebruiken voor de [directe aanmelding](direct-signin.md#redirect-sign-in-to-a-social-provider). Bijvoorbeeld *fabrikam.com*.
1. Selecteer onder **Claimstoewijzing voor id-provider** de volgende claims:

    - **Gebruikers-id**: *Sub*
    - **Weergavenaam**: *name*
    - **Voornaam**: *given_name*
    - **Achternaam**: *family_name*
    - **E-mail**: *e-mail*

1. Selecteer **Opslaan**.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Een beleids sleutel maken

U moet de toepassings sleutel opslaan die u eerder hebt gemaakt in uw Azure AD B2C-Tenant.

1. Zorg ervoor dat u de Directory gebruikt die de Azure AD B2C Tenant bevat waarvan u de Federatie wilt configureren (bijvoorbeeld Contoso). Selecteer het filter **Directory + abonnement** in het bovenste menu en kies de map die uw Azure AD B2C Tenant bevat (bijvoorbeeld Contoso).
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Onder **beleids regels** selecteert u **identiteits ervaring-Framework**.
1. Selecteer **beleids sleutels** en selecteer vervolgens **toevoegen**.
1. Kies voor **Opties** `Manual` .
1. Voer een **naam** in voor de beleids sleutel. Bijvoorbeeld `FabrikamAppSecret`.  Het voor voegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van de sleutel wanneer deze wordt gemaakt, zodat de verwijzing in de XML in de volgende sectie wordt *B2C_1A_FabrikamAppSecret*.
1. Voer in het **geheim** uw client geheim in dat u eerder hebt vastgelegd.
1. Selecteer voor **sleutel gebruik** `Signature` .
1. Selecteer **Maken**.

## <a name="add-a-claims-provider"></a>Een claim provider toevoegen

Als u wilt dat gebruikers zich aanmelden met behulp van de andere Azure AD B2C (fabrikam), moet u de andere Azure AD B2C definiëren als een claim provider waarmee Azure AD B2C met behulp van een eind punt kunt communiceren. Het eind punt biedt een set claims die wordt gebruikt door Azure AD B2C om te controleren of een specifieke gebruiker is geverifieerd.

U kunt Azure AD B2C als een claim provider definiëren door Azure AD B2C toe te voegen aan het **ClaimsProvider** -element in het extensie bestand van uw beleid.

1. Open het *TrustFrameworkExtensions.xml* -bestand.
1. Zoek het element **ClaimsProviders** . Als deze niet bestaat, voegt u deze toe onder het hoofd element.
1. Voeg als volgt een nieuwe **ClaimsProvider** toe:
    ```xml
    <ClaimsProvider>
      <Domain>fabrikam.com</Domain>
      <DisplayName>Federation with Fabrikam tenant</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Fabrikam-OpenIdConnect">
        <DisplayName>Fabrikam</DisplayName>
        <Protocol Name="OpenIdConnect"/>
        <Metadata>
          <!-- Update the Client ID below to the Application ID -->
          <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
          <!-- Update the metadata URL with the other Azure AD B2C tenant name and policy name -->
          <Item Key="METADATA">https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/v2.0/.well-known/openid-configuration</Item>
          <Item Key="UsePolicyInRedirectUri">false</Item>
          <Item Key="response_types">code</Item>
          <Item Key="scope">openid</Item>
          <Item Key="response_mode">form_post</Item>
          <Item Key="HttpBinding">POST</Item>
        </Metadata>
        <CryptographicKeys>
          <Key Id="client_secret" StorageReferenceId="B2C_1A_FabrikamAppSecret"/>
        </CryptographicKeys>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
          <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
          <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
          <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
          <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss"  />
          <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          <OutputClaim ClaimTypeReferenceId="otherMails" PartnerClaimType="emails"/>    
        </OutputClaims>
        <OutputClaimsTransformations>
          <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
          <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
          <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
      </TechnicalProfile>
     </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Werk de volgende XML-elementen bij met de relevante waarde:

    |XML-element  |Waarde  |
    |---------|---------|
    |ClaimsProvider\Domain  | De domein naam die wordt gebruikt voor [direct aanmelden](direct-signin.md?pivots=b2c-custom-policy#redirect-sign-in-to-a-social-provider). Voer de domein naam in die u wilt gebruiken voor de directe aanmelding. Bijvoorbeeld *fabrikam.com*. |
    |TechnicalProfile\DisplayName|Deze waarde wordt weer gegeven op de knop aanmelden op het aanmeldings scherm. Bijvoorbeeld *fabrikam*. |
    |Meta gegevens \ client_id|De toepassings-id van de ID-provider. Werk de client-ID bij met de toepassings-ID die u eerder hebt gemaakt in de andere Azure AD B2C Tenant.|
    |Metadata\METADATA|Een URL die verwijst naar een configuratie document van een OpenID Connect Connect-ID-provider, dat ook wel bekend staat als OpenID Connect goed bekend configuratie-eind punt. Voer de volgende URL `{tenant}` in die wordt vervangen door de domein naam van de andere Azure AD B2C Tenant (fabrikam). Vervang de `{tenant}` door de naam van het beleid dat u configureert in de andere Tenant en `{policy]` met de naam van het beleid: `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/v2.0/.well-known/openid-configuration` . Bijvoorbeeld `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/B2C_1_susi/v2.0/.well-known/openid-configuration`.|
    |CryptographicKeys| Werk de waarde van **StorageReferenceId** bij naar de naam van de beleids sleutel die u eerder hebt gemaakt. Bijvoorbeeld `B2C_1A_FabrikamAppSecret`.| 
    

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensie bestand voor verificatie

Nu hebt u uw beleid zodanig geconfigureerd dat Azure AD B2C weet hoe u kunt communiceren met de andere Azure AD B2C Tenant. Upload het extensie bestand van uw beleid alleen om te bevestigen dat er tot nu toe geen problemen zijn.

1. Selecteer op de pagina **aangepaste beleids regels** in uw Azure AD B2C-Tenant de optie **beleid uploaden**.
1. Schakel **het beleid overschrijven als dit bestaat** in en selecteer vervolgens het *TrustFrameworkExtensions.xml* bestand.
1. Klik op **Uploaden**.

## <a name="register-the-claims-provider"></a>De claim provider registreren

De ID-provider is op dit moment ingesteld, maar is nog niet beschikbaar op de aanmeldings pagina's van de registratie/aanmelding. Om het beschikbaar te maken, maakt u een kopie van een bestaande sjabloon gebruiker en wijzigt u deze zodat deze ook de Azure AD-ID-provider heeft:

1. Open het *TrustFrameworkBase.xml* -bestand in het Starter Pack.
1. Zoek en kopieer de volledige inhoud van het **UserJourney** -element dat bevat `Id="SignUpOrSignIn"` .
1. Open de *TrustFrameworkExtensions.xml* en zoek het element **UserJourneys** . Als het element niet bestaat, voegt u er een toe.
1. Plak de volledige inhoud van het **UserJourney** -element dat u hebt gekopieerd als onderliggend element van het onderdeel **UserJourneys** .
1. Wijzig de naam van de gebruikers traject. Bijvoorbeeld `SignUpSignInFabrikam`.

### <a name="display-the-button"></a>De knop weer geven

Het element **ClaimsProviderSelection** is vergelijkbaar met een id-provider knop op een registratie-en aanmeldings pagina. Als u een **ClaimsProviderSelection** -element toevoegt voor Azure AD B2C, wordt er een nieuwe knop weer gegeven wanneer een gebruiker op de pagina terechtkomt.

1. Zoek het **OrchestrationStep** -element dat is opgenomen `Order="1"` in de gebruikers traject die u hebt gemaakt in *TrustFrameworkExtensions.xml*.
1. Voeg onder **ClaimsProviderSelections** het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** in op een geschikte waarde, bijvoorbeeld `FabrikamExchange` :

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="FabrikamExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop aan een actie koppelen

Nu er een knop aanwezig is, moet u deze koppelen aan een actie. De actie in dit geval is voor Azure AD B2C om te communiceren met de andere Azure AD B2C om een token te ontvangen. Koppel de knop aan een actie door het technische profiel voor de Azure AD B2C claim provider te koppelen:

1. Zoek de **OrchestrationStep** die `Order="2"` in de gebruikers reis zijn opgenomen.
1. Voeg het volgende **ClaimsExchange** -element toe om ervoor te zorgen dat u dezelfde waarde gebruikt voor de **id** die u hebt gebruikt voor **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="FabrikamExchange" TechnicalProfileReferenceId="Fabrikam-OpenIdConnect" />
    ```

    Werk de waarde van **TechnicalProfileReferenceId** bij naar de **id** van het technische profiel dat u eerder hebt gemaakt. Bijvoorbeeld `Fabrikam-OpenIdConnect`.

1. Sla het *TrustFrameworkExtensions.xml* bestand op en upload het opnieuw voor verificatie.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-azure-ad-b2c-identity-provider-to-a-user-flow"></a>Azure AD B2C ID-provider toevoegen aan een gebruikers stroom 

1. Selecteer in uw Azure AD B2C-Tenant **gebruikers stromen**.
1. Klik op de gebruikers stroom die u wilt toevoegen aan de Azure AD B2C-ID-provider.
1. Selecteer **fabrikam** onder de **id-providers voor sociale netwerken**.
1. Selecteer **Opslaan**.
1. Als u het beleid wilt testen, selecteert u **gebruikers stroom uitvoeren**.
1. Selecteer voor **toepassing** de webtoepassing met de naam *testapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet `https://jwt.ms` weergeven.
1. Klik op **gebruikers stroom uitvoeren**
1. Selecteer *fabrikam* om u aan te melden met de andere Azure AD B2C Tenant van de registratie-of aanmeldings pagina.

::: zone-end

::: zone pivot="b2c-custom-policy"


## <a name="update-and-test-the-relying-party-file"></a>Het Relying Party bestand bijwerken en testen

Werk het Relying Party (RP)-bestand bij waarmee de door u gemaakte gebruikers traject wordt gestart.

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam ervan. Wijzig de naam bijvoorbeeld in *SignUpSignInFabrikam.xml*.
1. Open het nieuwe bestand en werk de waarde van het kenmerk **PolicyId** voor **TrustFrameworkPolicy** met een unieke waarde bij. Bijvoorbeeld `SignUpSignInFabrikam`.
1. Werk de waarde van **PublicPolicyUri** bij met de URI voor het beleid. Bijvoorbeeld `http://contoso.com/B2C_1A_signup_signin_fabrikam`.
1. Werk de waarde van het kenmerk **ReferenceId** in **DefaultUserJourney** bij zodat dit overeenkomt met de id van de gebruikers traject die u eerder hebt gemaakt. Bijvoorbeeld *SignUpSignInFabrikam*.
1. Sla de wijzigingen op en upload het bestand.
1. Selecteer onder **aangepast beleid** het nieuwe beleid in de lijst.
1. Selecteer in de vervolg keuzelijst **toepassing selecteren** de Azure AD B2C toepassing die u eerder hebt gemaakt. Bijvoorbeeld *testapp1*.
1. Selecteer **nu uitvoeren** 
1. Selecteer *fabrikam* om u aan te melden met de andere Azure AD B2C Tenant van de registratie-of aanmeldings pagina.

::: zone-end

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [het door geven van het andere Azure AD B2C-token aan uw toepassing](idp-pass-through-user-flow.md).
