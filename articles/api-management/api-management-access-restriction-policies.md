---
title: Beleid voor Azure API Management toegangsbeperking | Microsoft Docs
description: Meer informatie over het beleid voor toegangsbeperking beschikbaar voor gebruik in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: apimpm
ms.openlocfilehash: b8c564ef2de22555930f998ccd9918b252d35f17
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65541698"
---
# <a name="api-management-access-restriction-policies"></a>Beleid voor toegangsbeperking API Management

Dit onderwerp bevat een verwijzing voor de volgende API Management-beleid. Zie voor meer informatie over het toevoegen en configureren van beleid [beleidsregels in API Management](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="AccessRestrictionPolicies"></a> Beleid voor toegangsbeperking

-   [Selectievakje HTTP-header](api-management-access-restriction-policies.md#CheckHTTPHeader) -afdwingt bestaan en/of de waarde van een HTTP-Header.
-   [Aanroepfrequentie per abonnement beperken](api-management-access-restriction-policies.md#LimitCallRate) -gebruik van de API wordt voorkomen dat de pieken door aanroepfrequentie per abonnement beperken.
-   [Aanroepfrequentie beperken door sleutel](#LimitCallRateByKey) -gebruik van de API wordt voorkomen dat de pieken door aanroepfrequentie, op basis van per sleutel beperken.
-   [Aanroeper IP-adressen beperken](api-management-access-restriction-policies.md#RestrictCallerIPs) -Filters (kunt/geweigerd) aanroepen van specifieke IP-adressen en/of -adresbereiken.
-   [Gebruiksquotum per abonnement instellen](api-management-access-restriction-policies.md#SetUsageQuota) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van abonnement afdwingen.
-   [Gebruiksquotum instellen op sleutel](#SetUsageQuotaByKey) -Hiermee kunt u een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per sleutel afdwingen.
-   [Valideren van JWT](api-management-access-restriction-policies.md#ValidateJWT) -afdwingt bestaan en de geldigheid van een JWT geëxtraheerd uit een opgegeven HTTP-Header of een opgegeven query-parameter.

## <a name="CheckHTTPHeader"></a> HTTP-header controleren

Gebruik de `check-header` beleid af te dwingen dat een aanvraag voor een opgegeven HTTP-header heeft. U kunt eventueel controleren om te zien of de koptekst met een specifieke waarde of het selectievakje voor een bereik van toegestane waarden. Als de controle mislukt, wordt het beleid wordt beëindigd verwerking van aanvragen en retourneert het HTTP-code en de fout statusbericht opgegeven door het beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="true">
    <value>Value1</value>
    <value>Value2</value>
</check-header>
```

### <a name="example"></a>Voorbeeld

```xml
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>
</check-header>
```

### <a name="elements"></a>Elementen

| Name         | Description                                                                                                                                   | Vereist |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Check-header | Root-element.                                                                                                                                 | Ja      |
| value        | Toegestane HTTP-header-waarde. Als meerdere elementen van de waarde worden opgegeven, de controle wordt beschouwd als geslaagd als een van de waarden overeenkomen. | Nee       |

### <a name="attributes"></a>Kenmerken

| Name                       | Description                                                                                                                                                            | Vereist | Standaard |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| kan de niet-controle-foutbericht | Foutbericht op dat in de hoofdtekst van het HTTP-antwoord retourneren als de header bestaat niet of een ongeldige waarde heeft. Dit bericht moet hebben geen speciale tekens escape. | Ja      | N/A     |
| failed-check-httpcode      | HTTP-statuscode: om terug te keren als de header bestaat niet of een ongeldige waarde heeft.                                                                                        | Ja      | N/A     |
| header-naam                | De naam van de HTTP-Header om te controleren.                                                                                                                                  | Ja      | N/A     |
| ignore-case                | Kan worden ingesteld op True of False. Als is ingesteld op True aanvraag wordt genegeerd als de headerwaarde wordt vergeleken met de set van acceptabele waarden.                                    | Ja      | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend, uitgaand

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="LimitCallRate"></a> Aanroepfrequentie per abonnement beperken

De `rate-limit` gebruikspieken API op basis van een abonnement wordt verhinderd door het beperken van het aantal aanroepen naar een opgegeven getal gedurende een opgegeven periode. Wanneer dit beleid wordt geactiveerd de oproepende functie ontvangt een `429 Too Many Requests` antwoordstatuscode.

> [!IMPORTANT]
> Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.
>
> [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.

> [!CAUTION]
> Door de gedistribueerde aard van de architectuur van beperking, gelden enkele beperkingen nooit volledig juist is. Het verschil tussen geconfigureerd en het werkelijke aantal toegestane aanvragen variëren op basis van het aanvraagvolume en snelheid, back-end-latentie en andere factoren.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<rate-limit calls="number" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
    </api>
</rate-limit>
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <base />
        <rate-limit calls="20" renewal-period="90" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Name      | Description                                                                                                                                                                                                                                                                                              | Vereist |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| set-limiet | Root-element.                                                                                                                                                                                                                                                                                            | Ja      |
| api       | Voeg een of meer van deze elementen in te stellen een limiet van het gesprek op API's binnen het product. Product- en API aanroepfrequentielimiet-limieten onafhankelijk van elkaar worden toegepast. API kan worden verwezen via de `name` of `id`. Als beide kenmerken zijn opgegeven, `id` wordt gebruikt en `name` worden genegeerd.                    | Nee       |
| bewerking | Een of meer van deze elementen te leggen van een aanroep van limiet voor bewerkingen binnen een API toevoegen. Product, API en bewerking aanroepfrequentielimiet-limieten onafhankelijk van elkaar worden toegepast. Bewerking kan worden verwezen via de `name` of `id`. Als beide kenmerken zijn opgegeven, `id` wordt gebruikt en `name` worden genegeerd. | Nee       |

### <a name="attributes"></a>Kenmerken

| Name           | Description                                                                                           | Vereist | Standaard |
| -------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| naam           | De naam van de API voor de frequentielimiet wordt toegepast.                                                | Ja      | N/A     |
| oproepen          | Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`. | Ja      | N/A     |
| vernieuwingsperiode | De tijd in seconden waarna het quotum wordt opnieuw ingesteld.                                              | Ja      | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend

-   **Beleid bereiken:** product

## <a name="LimitCallRateByKey"></a> Aanroepfrequentie beperken met sleutel

> [!IMPORTANT]
> Deze functie is niet beschikbaar in de **verbruik** laag van API Management.

De `rate-limit-by-key` beleid voorkomt gebruikspieken API op basis van per sleutel door het beperken van het aantal aanroepen naar een opgegeven getal gedurende een opgegeven periode. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door met behulp van een beleidsexpressie voor een. Optionele incrementele voorwaarde kan worden toegevoegd om op te geven welke aanvragen naar de limiet moeten worden geteld. Wanneer dit beleid wordt geactiveerd de oproepende functie ontvangt een `429 Too Many Requests` antwoordstatuscode.

Zie voor meer informatie en voorbeelden van dit beleid [geavanceerde aanvraagbeperking met Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).

> [!CAUTION]
> Door de gedistribueerde aard van de architectuur van beperking, gelden enkele beperkingen nooit volledig juist is. Het verschil tussen geconfigureerd en het werkelijke aantal toegestane aanvragen variëren op basis van het aanvraagvolume en snelheid, back-end-latentie en andere factoren.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<rate-limit-by-key calls="number"
                   renewal-period="seconds"
                   increment-condition="condition"
                   counter-key="key value" />

```

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt is de frequentielimiet opgegeven met het IP-adres van oproepende functie.

```xml
<policies>
    <inbound>
        <base />
        <rate-limit-by-key  calls="10"
              renewal-period="60"
              increment-condition="@(context.Response.StatusCode == 200)"
              counter-key="@(context.Request.IpAddress)"/>
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Name      | Description   | Vereist |
| --------- | ------------- | -------- |
| set-limiet | Root-element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Name                | Description                                                                                           | Vereist | Standaard |
| ------------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| oproepen               | Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`. | Ja      | N/A     |
| tegenpartij sleutel         | De sleutel te gebruiken voor het beleid voor frequentielimiet.                                                             | Ja      | N/A     |
| Increment-voorwaarde | De Boole-expressie op te geven als de aanvraag moet worden geteld naar het quotum (`true`).        | Nee       | N/A     |
| vernieuwingsperiode      | De tijd in seconden waarna het quotum wordt opnieuw ingesteld.                                              | Ja      | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend

-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="RestrictCallerIPs"></a> Aanroeper IP-adressen beperken

De `ip-filter` beleid filters (kunt/geweigerd) aanroepen van specifieke IP-adressen en/of -adresbereiken.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address" />
</ip-filter>
```

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld kan het beleid alleen aanvragen die afkomstig zijn van de één IP-adres of het bereik van IP-adressen die zijn opgegeven

```xml
<ip-filter action="allow">
    <address>13.66.201.169</address>
    <address-range from="13.66.140.128" to="13.66.140.143" />
</ip-filter>
```

### <a name="elements"></a>Elementen

| Name                                      | Description                                         | Vereist                                                       |
| ----------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------- |
| IP-filter                                 | Root-element.                                       | Ja                                                            |
| address                                   | Hiermee geeft u één IP-adres waarop u wilt filteren.   | Ten minste één `address` of `address-range` element is vereist. |
| address-range from="address" to="address" | Hiermee geeft u een bereik van IP-adres waarop u wilt filteren. | Ten minste één `address` of `address-range` element is vereist. |

### <a name="attributes"></a>Kenmerken

| Name                                      | Description                                                                                 | Vereist                                           | Standaard |
| ----------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------- | ------- |
| address-range from="address" to="address" | Een bereik van IP-adressen wilt toestaan of weigeren van toegang voor.                                        | Vereist wanneer de `address-range` element wordt gebruikt. | N/A     |
| IP-adresfilter actie = "toestaan &#124; verbieden"    | Geeft aan of aanroepen moeten worden toegestaan of niet voor de opgegeven IP-adressen en -bereiken. | Ja                                                | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend
-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="SetUsageQuota"></a> Gebruiksquotum per abonnement instellen

De `quota` beleid zorgt ervoor dat een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van een abonnement.

> [!IMPORTANT]
> Dit beleid kan slechts één keer per beleidsdocument worden gebruikt.
>
> [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
    </api>
</quota>
```

### <a name="example"></a>Voorbeeld

```xml
<policies>
    <inbound>
        <base />
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Name      | Description                                                                                                                                                                                                                                                                                  | Vereist |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| quota     | Root-element.                                                                                                                                                                                                                                                                                | Ja      |
| api       | Voeg een of meer van deze elementen te leggen van de aanroep van quota op API's binnen het product. Product- en API-aanroep quota worden afzonderlijk toegepast. API kan worden verwezen via de `name` of `id`. Als beide kenmerken zijn opgegeven, `id` wordt gebruikt en `name` worden genegeerd.                    | Nee       |
| bewerking | Een of meer van deze elementen te leggen van de aanroep van quota op bewerkingen binnen een API toevoegen. Product, API en bewerking aanroep quota's worden afzonderlijk toegepast. Bewerking kan worden verwezen via de `name` of `id`. Als beide kenmerken zijn opgegeven, `id` wordt gebruikt en `name` worden genegeerd. | Nee       |

### <a name="attributes"></a>Kenmerken

| Name           | Description                                                                                               | Vereist                                                         | Standaard |
| -------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| naam           | De naam van de API of bewerking waarvoor het quotum van toepassing.                                             | Ja                                                              | N/A     |
| bandbreedte      | Het maximum aantal kilobytes is toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`. | Een van beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven. | N/A     |
| oproepen          | Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`.     | Een van beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven. | N/A     |
| vernieuwingsperiode | De tijd in seconden waarna het quotum wordt opnieuw ingesteld.                                                  | Ja                                                              | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend
-   **Beleid bereiken:** product

## <a name="SetUsageQuotaByKey"></a> Gebruiksquotum instellen op sleutel

> [!IMPORTANT]
> Deze functie is niet beschikbaar in de **verbruik** laag van API Management.

De `quota-by-key` beleid zorgt ervoor dat een vernieuwd of levensduur aanroep volume en/of bandbreedte quotum, op basis van per sleutel. De sleutel kan een willekeurige tekenreekswaarde en wordt meestal verzorgd door met behulp van een beleidsexpressie voor een. Optionele incrementele voorwaarde kan worden toegevoegd om op te geven welke aanvragen moeten worden geteld naar het quotum. Als er meerdere beleidsregels opgehoogd dezelfde sleutelwaarde, wordt er slechts één keer verhoogd per aanvraag. Als de oproep is bereikt, wordt de oproepende functie ontvangt een `403 Forbidden` antwoordstatuscode.

Zie voor meer informatie en voorbeelden van dit beleid [geavanceerde aanvraagbeperking met Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).

> [Beleidsexpressies](api-management-policy-expressions.md) kan niet worden gebruikt in een van de kenmerken van het beleid voor dit beleid.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<quota-by-key calls="number"
              bandwidth="kilobytes"
              renewal-period="seconds"
              increment-condition="condition"
              counter-key="key value" />

```

### <a name="example"></a>Voorbeeld

In het volgende voorbeeld wordt is het quotum opgegeven met het IP-adres van oproepende functie.

```xml
<policies>
    <inbound>
        <base />
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"
                      counter-key="@(context.Request.IpAddress)" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Name  | Description   | Vereist |
| ----- | ------------- | -------- |
| quota | Root-element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Name                | Description                                                                                               | Vereist                                                         | Standaard |
| ------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| bandbreedte           | Het maximum aantal kilobytes is toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`. | Een van beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven. | N/A     |
| oproepen               | Het maximum aantal aanroepen toegestaan tijdens het tijdsinterval dat is opgegeven in de `renewal-period`.     | Een van beide `calls`, `bandwidth`, of beide moeten samen worden opgegeven. | N/A     |
| tegenpartij sleutel         | De sleutel te gebruiken voor het quotumbeleid.                                                                      | Ja                                                              | N/A     |
| Increment-voorwaarde | De Boole-expressie op te geven als de aanvraag moet worden geteld naar het quotum (`true`)             | Nee                                                               | N/A     |
| vernieuwingsperiode      | De tijd in seconden waarna het quotum wordt opnieuw ingesteld.                                                  | Ja                                                              | N/A     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend
-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="ValidateJWT"></a> JWT valideren

De `validate-jwt` beleid zorgt ervoor dat bestaan, en de geldigheid van een JWT uit een van beide een opgegeven HTTP-Header of een opgegeven query-parameter.

> [!IMPORTANT]
> De `validate-jwt` beleid vereist dat de `exp` geregistreerde claim is opgenomen in de JWT-token, tenzij `require-expiration-time` kenmerk is opgegeven en ingesteld op `false`.
> De `validate-jwt` beleid ondersteunt HS256 en RS256 algoritmes voor ondertekening. Voor HS256 moet inline in het beleid in de met base64 gecodeerde vorm op de sleutel worden opgegeven. Voor RS256 heeft de sleutel voor via een eindpunt van de configuratie van Open-ID.
> De `validate-jwt` beleid ondersteunt tokens die zijn versleuteld met symmetrische sleutels die gebruikmaken van de volgende versleutelingsalgoritmen A128CBC-HS256 A192CBC-HS384 A256CBC HS512.

### <a name="policy-statement"></a>Beleidsverklaring

```xml
<validate-jwt
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"
    failed-validation-httpcode="http status code to return on failure"
    failed-validation-error-message="error message to return on failure"
    token-value="expression returning JWT token as a string"
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"
    clock-skew="allowed clock skew in seconds"
    output-token-variable-name="name of a variable to receive a JWT object representing successfully validated token">
  <issuer-signing-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </issuer-signing-keys>
  <decryption-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </decryption-keys>
  <audiences>
    <audience>audience string</audience>
    <!-- if there are multiple possible audiences, then add additional audience elements -->
  </audiences>
  <issuers>
    <issuer>issuer string</issuer>
    <!-- if there are multiple possible issuers, then add additional issuer elements -->
  </issuers>
  <required-claims>
    <claim name="name of the claim as it appears in the token" match="all|any" separator="separator character in a multi-valued claim">
      <value>claim value as it is expected to appear in the token</value>
      <!-- if there is more than one allowed values, then add additional value elements -->
    </claim>
    <!-- if there are multiple possible allowed values, then add additional value elements -->
  </required-claims>
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />
  <zumo-master-key id="key identifier">key value</zumo-master-key>
</validate-jwt>

```

### <a name="examples"></a>Voorbeelden

#### <a name="simple-token-validation"></a>Eenvoudige token validatie

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key>  <!-- signing key specified as a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>  <!-- audience is set to API Management host name -->
    </audiences>
    <issuers>
        <issuer>http://contoso.com/</issuer>
    </issuers>
</validate-jwt>
```

#### <a name="azure-active-directory-token-validation"></a>Azure Active Directory-tokenverificatie

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>
        <claim name="id" match="all">
            <value>insert claim here</value>
        </claim>
    </required-claims>
</validate-jwt>
```

#### <a name="azure-active-directory-b2c-token-validation"></a>Validatie van Azure Active Directory B2C-tokens

```xml
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>
        <claim name="id" match="all">
            <value>insert claim here</value>
        </claim>
    </required-claims>
</validate-jwt>
```

#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Toegang verlenen aan bewerkingen op basis van claims token

In dit voorbeeld ziet u hoe u de [JWT valideren](api-management-access-restriction-policies.md#ValidateJWT) beleid toegang verlenen aan bewerkingen op basis van claims token waarde.

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer" output-token-variable-name="jwt">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key> <!-- signing key is stored in a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>
    </audiences>
    <issuers>
        <issuer>contoso.com</issuer>
    </issuers>
    <required-claims>
        <claim name="group" match="any">
            <value>finance</value>
            <value>logistics</value>
        </claim>
    </required-claims>
</validate-jwt>
<choose>
    <when condition="@(context.Request.Method == "POST" && !((Jwt)context.Variables["jwt"]).Claims["group"].Contains("finance"))">
        <return-response>
            <set-status code="403" reason="Forbidden" />
        </return-response>
    </when>
</choose>
```

#### <a name="azure-mobile-services-token-validation"></a>Azure Mobile Services-tokenverificatie

```xml
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">
    <issuers>
        <issuer>urn:microsoft:windows-azure:zumo</issuer>
    </issuers>
    <audiences>
        <audience>Facebook</audience>
    </audiences>
    <issuer-signing-keys>
        <zumo-master-key id="0">insert key here</zumo-master-key>
    </issuer-signing-keys>
</validate-jwt>
```

### <a name="elements"></a>Elementen

| Element             | Description                                                                                                                                                                                                                                                                                                                                           | Vereist |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| validate-jwt        | Root-element.                                                                                                                                                                                                                                                                                                                                         | Ja      |
| doelgroepen           | Bevat een lijst met geaccepteerde doelgroep claims die gebruikt op het token worden kunnen. Als meerdere doelgroepwaarden aanwezig zijn, wordt elke waarde wordt geprobeerd totdat de alle zijn uitgeput (in dat geval validatie mislukt) of totdat het lukt. Ten minste één doelgroep moet worden opgegeven.                                                                     | Nee       |
| aanmelden-sleutels van uitgever | Een lijst met Base64 gecodeerde beveiligingssleutels gebruikt om ondertekende tokens te valideren. Als meerdere beveiligingssleutels aanwezig zijn, wordt elke sleutel wordt geprobeerd totdat de alle zijn uitgeput (in dat geval validatie mislukt) of totdat het lukt (dit is handig voor de rollover van token). Belangrijkste elementen hebben een optioneel `id` kenmerk dat wordt gebruikt voor het vergelijken van `kid` claim.               | Nee       |
| ontsleuteling-sleutels     | Een lijst met Base64-gecodeerd sleutels voor het ontsleutelen van de tokens. Als meerdere beveiligingssleutels aanwezig zijn, klikt u vervolgens elke sleutel geprobeerd totdat een van beide alle sleutels zijn uitgeput (in dat geval validatie mislukt) of een sleutel is geslaagd. Belangrijkste elementen hebben een optioneel `id` kenmerk dat wordt gebruikt voor het vergelijken van `kid` claim.                                                 | Nee       |
| uitgevers van certificaten             | Een lijst met toegestane principals die het token is uitgegeven. Als meerdere verlenerwaarden aanwezig zijn, wordt elke waarde wordt geprobeerd totdat de alle zijn uitgeput (in dat geval validatie mislukt) of totdat het lukt.                                                                                                                                         | Nee       |
| openid-config       | Het element dat wordt gebruikt voor het opgeven van een compatibel Open ID configuratie-eindpunt van waaruit ondertekenen van sleutels en uitgever kan worden verkregen.                                                                                                                                                                                                                        | Nee       |
| required-claims     | Bevat een lijst met claims verwacht aanwezig zijn op het token voor het als geldig beschouwd. Wanneer de `match` kenmerk is ingesteld op `all` elke claimwaarde in het beleid moet aanwezig zijn in het token voor de validatie te voltooien. Wanneer de `match` kenmerk is ingesteld op `any` ten minste één claim moet aanwezig zijn in het token voor de validatie te voltooien. | Nee       |
| zumo hoofdsleutel     | Hoofdsleutel voor tokens die zijn uitgegeven door Azure Mobile Services                                                                                                                                                                                                                                                                                                 | Nee       |

### <a name="attributes"></a>Kenmerken

| Name                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                            | Vereist                                                                         | Standaard                                                                           |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| clock-skew                      | Timespan. Gebruiken om op te geven van de verwachte tijd voor de maximale verschil tussen de systeemklok van de uitgever van het token en het exemplaar van API Management.                                                                                                                                                                                                                                                                                                               | Nee                                                                               | 0 seconden                                                                         |
| failed-validation-error-message | Foutbericht op dat in de hoofdtekst van het HTTP-antwoord retourneren als de JWT niet kan gevalideerd worden. Dit bericht moet hebben geen speciale tekens escape.                                                                                                                                                                                                                                                                                                 | Nee                                                                               | Standaard foutbericht weergegeven is afhankelijk van validatieprobleem, bijvoorbeeld 'JWT niet aanwezig is." |
| failed-validation-httpcode      | HTTP-statuscode: om terug te keren als de JWT niet gevalideerd worden.                                                                                                                                                                                                                                                                                                                                                                                         | Nee                                                                               | 401                                                                               |
| header-naam                     | De naam van de HTTP-header die het token.                                                                                                                                                                                                                                                                                                                                                                                                         | Een van de `header-name`, `query-parameter-name` of `token-value` moet worden opgegeven. | N/A                                                                               |
| query-parameter-name            | De naam van de queryparameter houden van het token.                                                                                                                                                                                                                                                                                                                                                                                                     | Een van de `header-name`, `query-parameter-name` of `token-value` moet worden opgegeven. | N/A                                                                               |
| token-waarde                     | Expressie die retourneert een tekenreeks met JWT-token                                                                                                                                                                                                                                                                                                                                                                                                     | Een van de `header-name`, `query-parameter-name` of `token-value` moet worden opgegeven. | N/A                                                                               |
| id                              | De `id` kenmerk in de `key` element kunt u opgeven de tekenreeks die wordt vergeleken met de `kid` claim in het token (indien aanwezig) om de juiste sleutel te gebruiken voor handtekeningvalidatie erachter te komen.                                                                                                                                                                                                                                           | Nee                                                                               | N/A                                                                               |
| overeenkomst                           | De `match` kenmerk in de `claim` element geeft aan of de waarde van elke claim in het beleid aanwezig zijn in het token voor de validatie moet te voltooien. Mogelijke waarden zijn:<br /><br /> - `all` -de waarde van elke claim in het beleid moet aanwezig zijn in het token voor de validatie te voltooien.<br /><br /> - `any` -ten minste één claimwaarde moet aanwezig zijn in het token voor de validatie te voltooien.                                                       | Nee                                                                               | alle                                                                               |
| require-expiration-time         | Booleaanse waarde. Hiermee geeft u op of een expiration-claim is vereist in het token.                                                                                                                                                                                                                                                                                                                                                                               | Nee                                                                               | true                                                                              |
| vereisen dat schema                  | De naam van het token-schema, bijvoorbeeld "Bearer". Wanneer dit kenmerk is ingesteld, kan het beleid zorgt ervoor dat het opgegeven schema is aanwezig in de waarde van de autorisatie-header.                                                                                                                                                                                                                                                                                    | Nee                                                                               | N/A                                                                               |
| vereisen dat-ondertekend-tokens           | Booleaanse waarde. Hiermee geeft u op of een token is vereist om te worden ondertekend.                                                                                                                                                                                                                                                                                                                                                                                           | Nee                                                                               | true                                                                              |
| separator                       | tekenreeks. Hiermee geeft u een scheidingsteken (bijvoorbeeld ",") moet worden gebruikt om een set waarden uit een claim meerdere waarden.                                                                                                                                                                                                                                                                                                                                          | Nee                                                                               | N/A                                                                               |
| url                             | Open ID configuratie-eindpunt-URL van waar de metagegevens van de Open-ID-configuratie kan worden verkregen. Het antwoord moet overeenkomen met de technische specificaties zoals gedefinieerd op de URL:`https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata`. Gebruik de volgende URL voor Azure Active Directory: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` bijvoorbeeld de naam van uw directory-tenant, waarbij `contoso.onmicrosoft.com`. | Ja                                                                              | N/A                                                                               |
uitvoer-token-variabele-naam|tekenreeks. Naam van de contextvariabele die Tokenwaarde als een object van het type ontvangen [ `Jwt` ](api-management-policy-expressions.md) na geslaagde token validatie|Nee|N/A

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in het volgende beleid [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [scopes](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleid secties:** inkomend
-   **Beleid bereiken:** wereldwijd, product, API, bewerking

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

-   [Beleid in API Management](api-management-howto-policies.md)
-   [API's transformeren](transform-api.md)
-   [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
-   [Voorbeelden van beleid](policy-samples.md)
