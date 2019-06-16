---
title: AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Instellen van AD FS 2016 met de SAML-protocol en aangepaste beleidsregels in Azure Active Directory B2C
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 4510074619ff513b7284819d88fdb2532e4ce33a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66510436"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelden voor een gebruikersaccount van de AD FS inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C. U aanmelden inschakelen door toe te voegen een [technische SAML-profiel](saml-technical-profile.md) naar een aangepast beleid.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Zorg ervoor dat u toegang tot een certificaat-pfx-bestand met een persoonlijke sleutel hebt. U kunt uw eigen ondertekend certificaat genereren en uploaden naar Azure AD B2C. Azure AD B2C gebruikt dit certificaat voor het ondertekenen van de SAML-aanvraag verzonden naar de SAML-identiteitsprovider.

## <a name="create-a-policy-key"></a>De beleidssleutel van een maken

U moet uw certificaat wordt opgeslagen in uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin uw tenant.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer op de pagina overzicht **Identity-Ervaringsframework**.
5. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
6. Voor **opties**, kiest u `Upload`.
7. Voer een **naam** voor de beleidssleutel. Bijvoorbeeld `SamlCert`. Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
8. Blader naar en selecteer het PFX-certificaatbestand met de persoonlijke sleutel.
9. Klik op **Create**.

## <a name="add-a-claims-provider"></a>Toevoegen van een claimprovider

Als u wilt dat gebruikers zich aanmelden met een AD FS-account, moet u het account als een claimprovider waarmee Azure AD B2C via een eindpunt communiceren kunnen definiëren. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt een AD FS-account als een claimprovider definiëren door toe te voegen aan de **ClaimsProviders** element in het bestand uitbreiding van uw beleid.

1. Open de *TrustFrameworkExtensions.xml*.
2. Zoek de **ClaimsProviders** element. Als deze niet bestaat, kunt u deze onder het root-element toevoegen.
3. Toevoegen van een nieuwe **ClaimsProvider** als volgt:

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your ADFS account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
            <Item Key="XmlSignatureAlgorithm">Sha256</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Vervang `your-ADFS-domain` met de naam van uw AD FS-domein en vervang de waarde van de **identityProvider** uitvoerclaim met uw DNS (willekeurige waarde die aangeeft van uw domein).
5. Sla het bestand op.

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met AD FS-account. Upload het bestand uitbreiding van uw beleid om te bevestigen dat er geen problemen met tot nu toe.

1. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
2. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
3. Klik op **Uploaden**.

## <a name="register-the-claims-provider"></a>De claimprovider registreren

Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen registreren of aanmelden. Om het beschikbaar maken, kunt u een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat het bevat ook de AD FS-id-provider.

1. Open de *TrustFrameworkBase.xml* -bestand van het starter-pack.
2. Zoeken en kopieer de gehele inhoud van de **UserJourney** element met `Id="SignUpOrSignIn"`.
3. Open de *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
4. Plak de volledige inhoud van de **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
5. Wijzig de naam van de ID van de gebruikersbeleving. Bijvoorbeeld `SignUpSignInADFS`.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelection** element is vergelijkbaar met een id-provider-knop op het scherm registreren of aanmelden. Als u een **ClaimsProviderSelection** -element voor een AD FS-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt.

1. Zoek de **OrchestrationStep** element met `Order="1"` in de gebruikersbeleving die u hebt gemaakt.
2. Onder **ClaimsProviderSelections**, voegt u het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** naar een geschikte waarde, bijvoorbeeld `ContosoExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met een AD FS-account voor het ontvangen van een token.

1. Zoek de **OrchestrationStep** die bevat `Order="2"` in de gebruikersbeleving.
2. Voeg de volgende **ClaimsExchange** element ervoor te zorgen dat u dezelfde waarde voor de ID die u gebruikt voor **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```
    
    Werk de waarde van **TechnicalProfileReferenceId** aan de ID van het technische profiel dat u eerder hebt gemaakt. Bijvoorbeeld `Contoso-SAML2`.

3. Sla de *TrustFrameworkExtensions.xml* -bestand en upload het opnieuw om te verifiëren.


## <a name="configure-an-adfs-relying-party-trust"></a>Een vertrouwensrelatie van relying party ADFS configureren

Voor het gebruik van ADFS als id-provider in Azure AD B2C, moet u een AD FS Relying Party Trust maken met de Azure AD B2C-SAML-metagegevens. Het volgende voorbeeld ziet u een URL-adres in de SAML-metagegevens van het technische profiel van een Azure AD B2C:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Vervang de volgende waarden:

- **uw tenant** met de naam van uw tenant, zoals uw tenant.onmicrosoft.com.
- **uw beleid** met de naam van uw beleid. Bijvoorbeeld: B2C_1A_signup_signin_adfs.
- **uw op technisch profiel** met de naam van uw id-provider technische SAML-profiel. Bijvoorbeeld Contoso-SAML2.
 
Open een browser en navigeer naar de URL. Controleer of u typt u de juiste URL en dat u toegang tot de XML-bestand met metagegevens hebt. Als u wilt toevoegen van een nieuwe relying party trust met behulp van de AD FS-beheermodule en de instellingen handmatig configureren, moet u de volgende procedure uitvoeren op een federatieserver. Lidmaatschap van **beheerders** of gelijkwaardig op de lokale computer is de minimale vereiste om deze procedure te voltooien.

1. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **AD FS Management**.
2. Selecteer **toevoegen vertrouwensrelatie van Relying Party**.
3. Op de **Welkom** pagina, kies **Claims bewuste**, en klik vervolgens op **Start**.
4. Op de **gegevensbron selecteren** pagina, selecteert u **importeren van gegevens over de relying party gepubliceerd online of op een lokaal netwerk**, geef de URL van uw Azure AD B2C-metagegevens en klik vervolgens op **volgende**.
5. Op de **weergavenaam opgeven** pagina een **weergavenaam**onder **opmerkingen bij de**, voer een beschrijving voor deze relying partyvertrouwensrelatie, en klik vervolgens op **volgende**.
6. Op de **kiezen toegangscontrolebeleid** pagina, selecteer een beleid en klik vervolgens op **volgende**.
7. Op de **gereed voor toevoegen vertrouwensrelatie** pagina, Controleer de instellingen en klik vervolgens op **volgende** om op te slaan, uw relying party trust informatie.
8. Op de **voltooien** pagina, klikt u op **sluiten**, deze actie wordt automatisch de **Claimregels bewerken** in het dialoogvenster.
9. Selecteer **regel toevoegen**.  
10. In **claimregelsjabloon**, selecteer **verzenden LDAP-kenmerken als claims**.
11. Geef een **naam van Claimregel**. Voor de **kenmerkopslag**, selecteer **Active Directory selecteren**, voegt u de volgende claims en klik vervolgens op **voltooien** en **OK**.

    | LDAP-kenmerk | Type uitgaande claim |
    | -------------- | ------------------- |
    | User-Principal-Name | userPrincipalName |
    | Achternaam | family_name |
    | Given-Name | given_name |
    | E-Mail-adres | e-mail |
    | Display-Name | naam |
    
12.  Op basis van het certificaattype, wellicht u stelt het HASH-algoritme. Selecteer op de relying party trust (B2C-Demo) eigenschappenvenster, de **Geavanceerd** tabblad en wijzig de **veilige hash-algoritme** naar `SHA-256`, en klikt u op **Ok**.  
13. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **AD FS Management**.
14. Selecteer de relying party trust die u hebt gemaakt, selecteer **Update van Federatiemetagegevens**, en klik vervolgens op **Update**. 

## <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Communicatie met Azure AD B2c vindt plaats via een toepassing die u in uw tenant maakt. Deze sectie vindt u optionele stappen die u uitvoeren kunt voor het maken van een testtoepassing als u dat nog niet hebt gedaan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de adreslijst gebruikt die uw Azure AD B2C-tenant bevat door te klikken op het **filter voor adreslijsten en abonnementen** in het bovenste menu en de adreslijst te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
5. Voer een naam voor de toepassing, bijvoorbeeld *testapp1*.
6. Voor **Web-App / Web-API**, selecteer `Yes`, en voer vervolgens `https://jwt.ms` voor de **antwoord-URL**.
7. Klik op **Create**.

### <a name="update-and-test-the-relying-party-file"></a>Bijwerken en testen van de relying party-bestand

Bijwerken van de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt.

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam. Bijvoorbeeld, wijzig de naam *SignUpSignInADFS.xml*.
2. Open het nieuwe bestand en werk de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** met een unieke waarde. Bijvoorbeeld `SignUpSignInADFS`.
3. Werk de waarde van **PublicPolicyUri** aan de URI voor het beleid. Bijvoorbeeld:`http://contoso.com/B2C_1A_signup_signin_adfs`
4. Werk de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpSignInADFS).
5. Sla uw wijzigingen, upload het bestand en selecteer vervolgens het nieuwe beleid in de lijst.
6. Zorg ervoor dat Azure AD B2C-toepassing die u hebt gemaakt is geselecteerd de **toepassing selecteren** veld en vervolgens testen door te klikken op **nu uitvoeren**.

