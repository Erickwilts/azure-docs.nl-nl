---
title: Meer informatie over het bieden van optionele claims voor uw Azure AD-toepassing | Microsoft Docs
description: Een handleiding voor het toevoegen van aangepaste of extra claims voor de SAML 2.0 en JSON Web Tokens (JWT)-tokens die zijn uitgegeven door Azure Active Directory.
documentationcenter: na
author: rwike77
services: active-directory
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/03/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 98b0ec2e1defc4701bff798b2fa93900ec8a9a64
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595163"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Procedure: Geef optioneel claims voor uw Azure AD-app

Ontwikkelaars van toepassingen kunnen optionele claims in hun Azure AD-apps gebruiken om op te geven welke claims dat ze willen in tokens die aan hun toepassing worden gestuurd. 

U kunt optioneel claims te gebruiken:

- Selecteer aanvullende claims in tokens voor uw toepassing moeten worden opgenomen.
- Het gedrag van bepaalde claims die Azure AD in tokens retourneert wijzigen.
- Toevoegen en toegang tot aangepaste claims voor uw toepassing.

Zie voor de lijsten met standard claims, de [toegangstoken](access-tokens.md) en [id_token](id-tokens.md) claims documentatie. 

Hoewel u optionele claims worden ondersteund in zowel v1.0 en v2.0 indeling tokens, evenals SAML-tokens, beschikt over de meeste van de waarde bij het verplaatsen van v1.0 naar versie 2.0. Een van de doelstellingen van de [v2.0 Microsoft identity-platform-eindpunt](active-directory-appmodel-v2-overview.md) is token kleinere om optimale prestaties door clients. Als gevolg hiervan meerdere claims voorheen opgenomen in de toegang en de ID-tokens zijn niet meer aanwezig zijn in v2.0-tokens en moeten worden gevraagd om specifiek op basis van de per toepassing.

**Tabel 1: Toepasselijkheid**

| Accounttype | V1.0 tokens | V2.0-tokens  |
|--------------|---------------|----------------|
| Persoonlijke Microsoft-account  | N/A  | Ondersteund |
| Azure AD-account      | Ondersteund | Ondersteund |

## <a name="v10-and-v20-optional-claims-set"></a>V1.0 en v2.0 optioneel claims instellen

De set optioneel claims die standaard beschikbaar is voor toepassingen om te gebruiken, worden hieronder vermeld. Zie het toevoegen van aangepaste optionele claims voor uw toepassing [Mapextensies](#configuring-directory-extension-optional-claims)hieronder. Bij het toevoegen van claims voor de **toegangstoken**, dit geldt voor toegangstokens aangevraagd *voor* de toepassing (een web-API), niet de *door* de toepassing. Dit zorgt ervoor dat, ongeacht de toegang tot uw API-client de juiste gegevens aanwezig in het toegangstoken dat ze gebruiken is voor verificatie op basis van uw API.

> [!NOTE]
> De meeste van deze claims kan worden opgenomen in JWTs voor v1.0 en v2.0-tokens, maar geen SAML-tokens, tenzij anders is aangegeven in de kolom Type Token. Consumenten-accounts ondersteunen slechts een subset van deze claims, gemarkeerd in de kolom 'Type gebruiker'.  Veel van de claims die worden vermeld niet van toepassing op gebruikers van de consument (ze dus er is geen tenant hebben `tenant_ctry` heeft geen waarde).  

**Tabel 2: v1.0 en optionele v2.0 claim instellen**

| Name                       |  Description   | Tokentype | Gebruikerstype | Opmerkingen  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Tijd wanneer de gebruiker laatste geverifieerd. Zie de OpenID Connect-specificatie.| JWT        |           |  |
| `tenant_region_scope`      | De regio van de resource-tenant | JWT        |           | |
| `home_oid`                 | Voor gastgebruikers, de object-ID van de gebruiker in de starttenant van de gebruiker.| JWT        |           | |
| `sid`                      | Sessie-ID die wordt gebruikt voor aanmelding door een gebruiker per sessie af. | JWT        |  Persoonlijke en Azure AD-accounts.   |         |
| `platf`                    | Apparaatplatform    | JWT        |           | Beperkt tot beheerde apparaten die u kunnen controleren of apparaattype.|
| `verified_primary_email`   | Afkomstig uit van de gebruiker PrimaryAuthoritativeEmail      | JWT        |           |         |
| `verified_secondary_email` | Afkomstig uit van de gebruiker SecondaryAuthoritativeEmail   | JWT        |           |        |
| `enfpolids`                | Afgedwongen beleid voor id's. Een lijst van het beleid voor id's die zijn geëvalueerd voor de huidige gebruiker. | JWT |  |  |
| `vnet`                     | Informatie over VNET-aanduiding. | JWT        |           |      |
| `fwd`                      | IP-adres.| JWT    |   | Het oorspronkelijke IPv4-adres van de aanvragende client (binnen een VNET) toegevoegd |
| `ctry`                     | Land van gebruiker | JWT |  | Azure AD-retourneert de `ctry` optionele claim als deze aanwezig is en de waarde van de claim een standaard twee letters landnummer, zoals FR, JP en SZ is. |
| `tenant_ctry`              | Land van de tenant van de resource | JWT | | |
| `xms_pdl`          | Gewenste gegevenslocatie   | JWT | | Voor meerdere geografische gebieden tenants is dit de 3-letterige code met de geografische regio waarin die de gebruiker zich bevindt. Zie voor meer informatie de [Azure AD Connect-documentatie over de gewenste gegevenslocatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation).<br/>Bijvoorbeeld: `APC` voor Azië en Stille Oceaan. |
| `xms_pl`                   | Gebruiker gewenste taal  | JWT ||De gebruiker de taal, bij voorkeur als instellen. Afkomstig uit de starttenant, in de Gast-scenario's. LLE CC opgemaakt ("en-us '). |
| `xms_tpl`                  | Tenant van de taal van voorkeur| JWT | | De resource-tenant de taal, bij voorkeur als instellen. Opgemaakte LLE ('en'). |
| `ztdid`                    | Zero-touch-implementatie-ID | JWT | | De apparaat-id die wordt gebruikt voor [Windows AutoPilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) |
| `email`                    | De adresseerbare e-mailadres voor deze gebruiker, als de gebruiker een heeft.  | JWT, SAML | MSA, Azure AD | Deze waarde is standaard opgenomen als de gebruiker een gast in de tenant.  Voor beheerde gebruikers (die in de tenant), moet deze worden gevraagd via deze optionele claim of, op v2.0 alleen met het bereik OpenID.  Voor beheerde gebruikers het e-mailadres moet worden ingesteld in de [Office-beheerportal](https://portal.office.com/adminportal/home#/users).| 
| `groups`| Optionele opmaak voor groepclaims |JWT, SAML| |Gebruikt in combinatie met de instelling GroupMembershipClaims in de [toepassingsmanifest](reference-app-manifest.md), die ook moeten worden ingesteld. Zie voor meer informatie [groep claims](#Configuring-group-optional claims) hieronder. Zie voor meer informatie over groepclaims [groepclaims configureren](../hybrid/how-to-connect-fed-group-claims.md)
| `acct`             | Accountstatus gebruikers in de tenant. | JWT, SAML | | Als de gebruiker een lid van de tenant is, is de waarde `0`. Als ze een gast zijn, is de waarde `1`. |
| `upn`                      | UserPrincipalName claim. | JWT, SAML  |           | Hoewel deze claim automatisch geïnstalleerd wordt, kunt u deze kunt opgeven als een optionele claim extra eigenschappen voor het wijzigen van het gedrag in het geval van de gebruiker Gast koppelen.  |

### <a name="v20-optional-claims"></a>Optionele claims v2.0

Deze claims worden altijd in de Azure AD-tokens v1.0 opgenomen, maar niet opgenomen in v2.0-tokens, tenzij aangevraagd. Deze claims zijn alleen van toepassing voor JWTs (ID-tokens en Tokens voor toegang). 

**Tabel 3: v2.0-alleen optioneel claims**

| JWT Claim     | Name                            | Description                                | Opmerkingen |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP-adres                      | De client heeft aangemeld vanaf het IP-adres.   |       |
| `onprem_sid`  | On-Premises beveiligings-id |                                             |       |
| `pwd_exp`     | Wachtwoordverlooptijd        | De datum en tijd waarop het wachtwoord is verlopen. |       |
| `pwd_url`     | URL van wijzigen wachtwoord             | Een URL die de gebruiker bezoeken kan om hun wachtwoord te wijzigen.   |   |
| `in_corp`     | Inside Corporate Network        | Signalen als de client is aangemeld vanuit het bedrijfsnetwerk bevinden. Als ze niet, niet de claim is opgenomen.   |  Op basis van de [vertrouwde IP-adressen](../authentication/howto-mfa-mfasettings.md#trusted-ips) instellingen in MFA.    |
| `nickname`    | Nickname                        | Een andere naam voor de gebruiker te scheiden van de naam van eerste of laatste. | 
| `family_name` | Achternaam                       | Bevat de laatste naam, de achternaam of familienaam van de gebruiker gedefinieerd in het gebruikersobject. <br>"family_name": "Kleefstra" | Ondersteund in beheerde Serviceaccounts en Azure AD   |
| `given_name`  | Voornaam                      | De eerste biedt of als u ' ' naam van de gebruiker, zoals ingesteld op het gebruikersobject.<br>'given_name': "Frank"                   | Ondersteund in beheerde Serviceaccounts en Azure AD  |
| `upn`         | User Principal Name | Een id voor de gebruiker die kan worden gebruikt met de parameter username_hint.  Geen een gebruiksartikel-id voor de gebruiker en mag niet worden gebruikt om belangrijke gegevens. | Zie [extra eigenschappen](#additional-properties-of-optional-claims) hieronder voor de configuratie van de claim. |

### <a name="additional-properties-of-optional-claims"></a>Aanvullende eigenschappen van optionele claims

Sommige optionele claims kunnen worden geconfigureerd voor het wijzigen van de manier waarop die de claim wordt geretourneerd. De aanvullende eigenschappen worden voornamelijk gebruikt om te helpen bij de migratie van on-premises toepassingen met verschillende verwachtingen (bijvoorbeeld `include_externally_authenticated_upn_without_hash` helpt bij de clients die niet kunnen hekjes verwerken (`#`) in de UPN)

**Tabel 4: Waarden voor het configureren van optionele claims**

| Naam van eigenschap  | Aanvullende eigenschapsnaam | Description |
|----------------|--------------------------|-------------|
| `upn`          |                          | Kan worden gebruikt voor SAML- en JWT antwoorden en v1.0 en v2.0-tokens. |
|                | `include_externally_authenticated_upn`  | Bevat de UPN die is opgeslagen in de resource-tenant van de Gast. Bijvoorbeeld: `foo_hometenant.com#EXT#@resourcetenant.com` |             
|                | `include_externally_authenticated_upn_without_hash` | Hetzelfde als hierboven, behalve dat de hash markeert (`#`) zijn vervangen door een onderstrepingsteken (`_`), bijvoorbeeld `foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Voorbeeld van de aanvullende eigenschappen

```json
 "optionalClaims": 
   {
       "idToken": [ 
             { 
                "name": "upn", 
            "essential": false,
                "additionalProperties": [ "include_externally_authenticated_upn"]  
              }
        ]
}
```

Dit object OptionalClaims zorgt ervoor dat de ID-token dat is geretourneerd naar de client om op te nemen van een andere upn met de extra starttenant en informatie over de resource-tenant. Hiermee wordt alleen gewijzigd de `upn` claim in het token als de gebruiker een gast in de tenant (die gebruikmaakt van een andere id-provider voor verificatie). 

## <a name="configuring-optional-claims"></a>Optionele claims configureren

U kunt optioneel claims voor uw toepassing configureren door het wijzigen van het toepassingsmanifest (Zie onderstaand voorbeeld). Zie voor meer informatie de [inzicht in de Azure AD application manifest artikel](reference-app-manifest.md).

> [!IMPORTANT]
> Toegangstokens zijn **altijd** gegenereerd met behulp van het manifest van de resource, niet door de client.  Dit het geval is in de aanvraag `...scope=https://graph.microsoft.com/user.read...` de resource is Graph.  Het toegangstoken is zo gemaakt met behulp van het manifest van de grafiek, niet het manifest van de client.  Het manifest voor uw toepassing worden nooit wijzigt, tokens voor Graph anders.  Als u wilt valideren dat uw `accessToken` wijzigingen worden van kracht, aanvragen van een token voor uw toepassing, niet een andere app.  

**Voorbeeldschema:**

```json
"optionalClaims":  
   {
      "idToken": [
            {
                  "name": "auth_time", 
                  "essential": false
             }
      ],
      "accessToken": [
             {
                    "name": "ipaddr", 
                    "essential": false
              }
      ],
      "saml2Token": [
              {
                    "name": "upn", 
                    "essential": false
               },
               {
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": false
               }
       ]
   }
```

### <a name="optionalclaims-type"></a>Type OptionalClaims

Verklaart de optionele claims aangevraagd door een toepassing. Een toepassing kunt configureren voor optionele claims moeten worden geretourneerd in elk van de drie typen van tokens (id-token, token, SAML 2 toegangstoken) kan ontvangen van de service voor beveiligingstokens. De toepassing kan een andere set optioneel claims moeten worden geretourneerd in elke tokentype configureren. De eigenschap OptionalClaims van de toepassing-entiteit is een OptionalClaims-object.

**Tabel 5: Eigenschappen van het type OptionalClaims**

| Name        | Type                       | Description                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Verzameling (OptionalClaim) | De optionele claims geretourneerd in de ID van de JWT-token. |
| `accessToken` | Verzameling (OptionalClaim) | De optionele claims geretourneerd in de JWT-toegangstoken. |
| `saml2Token`  | Verzameling (OptionalClaim) | De optionele claims geretourneerd in het SAML-token.   |

### <a name="optionalclaim-type"></a>Type OptionalClaim

Bevat een optionele claim die zijn gekoppeld aan een toepassing of een service-principal. De idToken accessToken en saml2Token eigenschappen van de [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) type is een verzameling van OptionalClaim.
Als dit wordt ondersteund door een specifieke claim, kunt u ook het gedrag van het gebruik van het veld AdditionalProperties OptionalClaim wijzigen.

**Tabel 6: OptionalClaim type-eigenschappen**

| Name                 | Type                    | Description                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | De naam van de optionele claim.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | De bron (directory-object) van de claim. Er zijn vooraf gedefinieerde claims en de gebruiker gedefinieerde claims van extensie-eigenschappen. Als de waarde null is, is de claim een vooraf gedefinieerde optioneel claim. Als waarde voor de gebruiker is, is de waarde in de naameigenschap van de extensie-eigenschap van het gebruikersobject. |
| `essential`            | Edm.Boolean             | Als de waarde true is, is de claim die is opgegeven door de client die nodig zijn om te controleren of een goede autorisatie-ervaring voor de specifieke taak die is aangevraagd door de eindgebruiker. De standaardwaarde is false.                                                                                                             |
| `additionalProperties` | Verzameling (Edm.String) | Aanvullende eigenschappen van de claim. Als een eigenschap in deze verzameling bestaat, wijzigt u over de werking van de optionele claim die in de eigenschap name is opgegeven.                                                                                                                                               |
## <a name="configuring-directory-extension-optional-claims"></a>Directory-extensie optioneel claims configureren

Naast de standaard optioneel claims is ingesteld, kunt u ook tokens om op te nemen van directory-schema-uitbreidingen configureren. Zie voor meer informatie, [Directory-schema-uitbreidingen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions). Deze functie is handig voor het koppelen van aanvullende informatie die uw app, bijvoorbeeld gebruiken kunt, een extra id of een belangrijke configuratie-optie die de gebruiker heeft ingesteld. 

> [!Note]
> - Directory-schema-uitbreidingen zijn een alleen-AD-functie van Azure, dus als uw toepassing manifest van de aanvragen voor een aangepaste extensie- en een MSA-gebruiker meldt zich aan bij uw app, worden deze extensies, niet geretourneerd.
> - Azure AD optioneel claims werken alleen met de Azure AD-extensie en werkt niet met de Microsoft Graph-directory-extensie. Beide API's vereisen de `Directory.ReadWriteAll` machtiging kan alleen door beheerders worden gegeven.

### <a name="directory-extension-formatting"></a>Directory-extensie opmaak

Gebruik de volledige naam van de extensie voor extensiekenmerken (in de indeling: `extension_<appid>_<attributename>`) in het toepassingsmanifest. De `<appid>` moet overeenkomen met de ID van de aanvraag om de claim. 

Binnen de JWT, wordt deze claims worden verzonden met de volgende indeling: `extn.<attributename>`.

In het SAML-tokens, wordt deze claims worden verzonden met de volgende URI-indeling: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="configuring-group-optional-claims"></a>Optionele groepclaims configureren

   > [!NOTE]
   > De mogelijkheid om te verzenden van namen voor gebruikers en groepen die zijn gesynchroniseerd van on-premises is openbare Preview.

In deze sectie bevat informatie over de configuratieopties onder optioneel claims voor het wijzigen van de groepskenmerken gebruikt in de groepclaims van de objectID van de groep standaard op kenmerken vanuit on-premises Windows Active Directory gesynchroniseerd.

> [!IMPORTANT]
> Zie [groepclaims voor toepassingen met Azure AD configureren](../hybrid/how-to-connect-fed-group-claims.md) voor meer informatie, zoals belangrijke aanvullende opmerkingen voor de openbare preview van groepclaims uit on-premises kenmerken.

1. In de portal -> Azure Active Directory -> Application rapporten -> Selecteer Application Manifest ->

2. Groepslidmaatschap claims inschakelen door het veranderen van de groupMembershipClaim

   De geldige waarden zijn:

   - 'Alle'
   - "SecurityGroup"
   - "DistributionList"
   - "DirectoryRole"

   Bijvoorbeeld:

   ```json
   "groupMembershipClaims": "SecurityGroup"
   ```

   Standaard die groep objectid's wordt verzonden in de groep claimwaarde.  Voor het wijzigen van de waarde van de claim bevat op de lokale groepskenmerken of te wijzigen van het claimtype rol, moet u OptionalClaims configuratie als volgt gebruiken:

3. De naam van configuratie optioneel groepclaims instellen.

   Als u groepen in het token bevat de on-premises AD-groepskenmerken in de sectie optionele claims welke tokentype optioneel claim moet worden toegepast opgeven op, de naam van de optionele claim aangevraagd en eventuele aanvullende eigenschappen gewenst.  Meerdere typen tokens kunnen worden weergegeven:

   - idToken voor het token OIDC-ID
   - accessToken voor de OAuth/OIDC-toegangstoken
   - Saml2Token voor SAML-tokens.

   > [!NOTE]
   > Het type Saml2Token is van toepassing op zowel SAML1.1 en SAML2.0 indeling tokens

   Voor elke relevante tokentype, wijzigt u de claim groepen naar de sectie OptionalClaims in het manifest gebruiken. Het schema OptionalClaims is als volgt:

   ```json
   {
   "name": "groups",
   "source": null,
   "essential": false,
   "additionalProperties": []
   }
   ```

   | Optionele claims schema | Value |
   |----------|-------------|
   | **Naam:** | Moet 'groepen' |
   | **Bron:** | Niet gebruikt. Weglaat of geef null-waarde |
   | **essential:** | Niet gebruikt. Weglaat of ONWAAR opgeven |
   | **additionalProperties:** | Lijst met extra eigenschappen.  Valid options are "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”, "emit_as_roles" |

   In additionalProperties slechts één 'sam_account_name","dns_domain_and_sam_account_name", zijn"netbios_domain_and_sam_account_name"vereist.  Als meer dan één aanwezig is, wordt de eerste wordt gebruikt en alle andere genegeerd.

   Sommige toepassingen vereisen groepsinformatie over de gebruiker in de rol-claim.  Het claimtype om toe te voegen uit een groepclaim op een claim rol, "emit_as_roles" om aanvullende eigenschappen te wijzigen.  De waarden van de groep wordt in de claim rol worden verzonden.

   > [!NOTE]
   > Als 'emit_as_roles' wordt gebruikt een toepassingsrollen geconfigureerd dat de gebruiker wordt toegewezen niet wordt weergegeven in de claim rol

**Voorbeelden:** Groepen aan de groepsnamen van de in OAuth-toegangstokens dnsDomainName\sAMAccountName indeling verzenden

```json
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Namen moeten worden geretourneerd in netbiosDomain\sAMAccountName indeling als de rollen in SAML en OIDC-ID-Tokens claim verzenden:

```json
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }

 ```

## <a name="optional-claims-example"></a>Voorbeeld van de optionele claims

In deze sectie kan u helpen bij een scenario om te zien hoe u de functie optioneel claims voor uw toepassing kunt gebruiken.
Er zijn meerdere opties beschikbaar voor het bijwerken van de eigenschappen van de configuratie van de identiteit van een toepassing inschakelen en configureren van optionele claims:
-   U kunt het toepassingsmanifest kunt wijzigen. In het volgende voorbeeld gebruikt deze methode om de configuratie. Lees de [inzicht krijgen in het document van Azure AD-toepassing-manifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest) eerste voor een inleiding tot het manifest.
-   Het is ook mogelijk om te schrijven van een toepassing die gebruikmaakt van de [Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) om bij te werken van uw toepassing. De [entiteit en complex type reference](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) in de Graph API reference guide kan u helpen bij de optionele claims configureren.

**Voorbeeld:** In het onderstaande voorbeeld wijzigt u een toepassingsmanifest om toe te voegen van claims voor toegang, -ID en SAML tokens die zijn bedoeld voor de toepassing.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Nadat u hebt geverifieerd, kiest u uw Azure AD-tenant door deze te selecteren in de rechterbovenhoek van de pagina.
1. Selecteer **App-registraties** vanaf de linkerzijde.
1. De toepassing die u wilt configureren van optionele claims voor in de lijst en klik op het niet vinden.
1. Klik op de toepassingspagina **Manifest** om het manifest inline-editor te openen. 
1. U kunt het manifest met behulp van deze editor rechtstreeks bewerken. Het manifest volgt het schema voor de [Toepassingsentiteit](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest), en het manifest één keer opgeslagen automatische-indelingen. Nieuwe elementen worden toegevoegd aan de `OptionalClaims` eigenschap.

    ```json
      "optionalClaims": 
      {
            "idToken": [ 
                  { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                  }
            ],
            "accessToken": [ 
                  {
                        "name": "auth_time", 
                        "essential": false
                  }
            ],
            "saml2Token": [ 
                  { 
                        "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                        "source": "user", 
                        "essential": true
                  }
            ]
      }

    ```

    In dit geval zijn verschillende optionele claims toegevoegd aan elk type token dat de toepassing kan ontvangen. De ID-tokens bevat nu de UPN voor federatieve gebruikers in het volledige formulier (`<upn>_<homedomain>#EXT#@<resourcedomain>`). De toegangstokens die andere clients voor deze toepassing vragen bevat nu de claim auth_time. De SAML-tokens bevat nu de skypeId directory-schema-uitbreiding (in dit voorbeeld wordt de app-ID voor deze app is ab603c56068041afb2f6832e2a17e237). De SAML-tokens wordt weergegeven de Skype-ID als `extension_skypeId`.

1. Wanneer u klaar bent met het bijwerken van het manifest, klikt u op **opslaan** om op te slaan van het manifest

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de standaard claims geleverd door Azure AD.

- [ID-tokens](id-tokens.md)
- [Toegangstokens](access-tokens.md)
