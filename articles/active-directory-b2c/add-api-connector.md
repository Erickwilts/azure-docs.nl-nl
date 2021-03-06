---
title: API-connectors toevoegen aan gebruikers stromen (preview-versie)
description: Een API-connector configureren voor gebruik in een gebruikers stroom.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 9117474c3cbf5087a5b63512fcc17c4771bf7aa6
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2020
ms.locfileid: "96343872"
---
# <a name="add-an-api-connector-to-a-sign-up-user-flow-preview"></a>Een API-connector toevoegen aan een registratie gebruikers stroom (preview-versie)

> [!IMPORTANT]
> API-connectors voor aanmelden is een open bare preview-functie van Azure AD B2C. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Als u een [API-connector](api-connectors-overview.md)wilt gebruiken, maakt u eerst de API-connector en schakelt u deze in in een gebruikers stroom.

## <a name="create-an-api-connector"></a>Een API-connector maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Onder **Azure-Services** selecteert u **Azure AD B2C**.
4. Selecteer **API-connectors (preview)** en selecteer vervolgens **nieuwe API-connector**.

   ![Een nieuwe API-connector toevoegen](./media/add-api-connector/api-connector-new.png)

5. Geef een weergave naam op voor de aanroep. U kunt bijvoorbeeld **gebruikers gegevens valideren**.
6. Geef de **eind punt-URL** voor de API-aanroep op.
7. Geef de verificatie gegevens op voor de API.

   - Momenteel wordt alleen basis verificatie ondersteund. Als u een API zonder basis verificatie voor ontwikkelings doeleinden wilt gebruiken, voert u gewoon een ' dummy ' **gebruikers naam** en **wacht woord** in die door uw API kunnen worden genegeerd. Voor gebruik met een Azure-functie met een API-sleutel kunt u de code als een query parameter in de **eind punt-URL** toevoegen (bijvoorbeeld https []() ://contoso.azurewebsites.net/API/endpoint <b>? code = 0123456789</b>).

   ![Een nieuwe API-connector configureren](./media/add-api-connector/api-connector-config.png)
8. Selecteer **Opslaan**.

## <a name="the-request-sent-to-your-api"></a>De aanvraag die naar uw API wordt verzonden
Een API-connector resultatenset als een **http post-** aanvraag, waarbij gebruikers kenmerken (' claims ') worden verzonden als sleutel-waardeparen in een JSON-hoofd tekst. Kenmerken worden op dezelfde manier geserialiseerd als [Microsoft Graph](/graph/api/resources/user#properties) gebruikers eigenschappen. 

**Voorbeeldaanvraag**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

Alleen gebruikers eigenschappen en aangepaste kenmerken die in de **Azure AD B2C**  >  **gebruikers kenmerken** zijn opgenomen, kunnen in de aanvraag worden verzonden.

Aangepaste kenmerken bestaan in de indeling **extension_ \<extensions-app-id> _CustomAttribute**  in de map. Uw API moet verwachten dat er claims in dezelfde geserialiseerde indeling worden ontvangen. Zie [aangepaste kenmerken definiëren in azure Active Directory B2C](user-flow-custom-attributes.md)voor meer informatie over aangepaste kenmerken.

Daarnaast wordt de **gebruikers interface land instellingen (' ui_locales ')** standaard in alle aanvragen verzonden. Het biedt de land instellingen van een gebruiker, zoals geconfigureerd op hun apparaat, dat door de API kan worden gebruikt om internationale reacties te retour neren.

> [!IMPORTANT]
> Als een claim geen waarde heeft op het moment dat het API-eind punt wordt aangeroepen, wordt de claim niet verzonden naar de API. Uw API moet zo worden ontworpen dat er expliciet wordt gecontroleerd of de aanvraag wordt verwerkt.

> [!TIP] 
> [**identiteiten (' Identities ')**](/graph/api/resources/objectidentity) en het **e-mail adres (' e-mail ')** kunnen worden gebruikt door uw API om een gebruiker te identificeren voordat ze een account in uw Tenant hebben. 

## <a name="enable-the-api-connector-in-a-user-flow"></a>De API-connector inschakelen in een gebruikers stroom

Volg deze stappen om een API-connector toe te voegen aan een registratie gebruikers stroom.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Onder **Azure-Services** selecteert u **Azure AD B2C**.
4. Selecteer **gebruikers stromen** en selecteer vervolgens de gebruikers stroom waaraan u de API-connector wilt toevoegen.
5. Selecteer **API-connectors** en selecteer vervolgens de API-eind punten die u wilt aanroepen met de volgende stappen in de gebruikers stroom:

   - **Nadat u zich hebt aangemeld met een id-provider**
   - **Voordat u de gebruiker maakt**

   ![Api's toevoegen aan de gebruikers stroom](./media/add-api-connector/api-connectors-user-flow-select.png)

6. Selecteer **Opslaan**.

## <a name="after-signing-in-with-an-identity-provider"></a>Nadat u zich hebt aangemeld met een id-provider

Een API-connector in deze stap van het registratie proces wordt onmiddellijk geactiveerd nadat de gebruiker is geverifieerd met een id-provider (zoals Google, Facebook, & Azure AD). Deze stap gaat vooraf aan de *-*_kenmerk verzamelings pagina_* _. Dit is het formulier dat wordt weer gegeven aan de gebruiker om gebruikers kenmerken te verzamelen. Deze stap wordt niet aangeroepen als een gebruiker wordt geregistreerd met een lokaal account.

### <a name="example-request-sent-to-the-api-at-this-step"></a>Voorbeeld aanvraag die wordt verzonden naar de API in deze stap
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ 
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

De exacte claims die worden verzonden naar de API, zijn afhankelijk van welke informatie wordt verstrekt door de ID-provider. e-mail bericht wordt altijd verzonden.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Verwachte antwoord typen van de Web-API bij deze stap

Wanneer de Web-API tijdens een gebruikers stroom een HTTP-aanvraag van Azure AD ontvangt, kan deze de volgende antwoorden retour neren:

- Vervolg reactie
- Antwoord blok keren

#### <a name="continuation-response"></a>Vervolg reactie

Een vervolg antwoord geeft aan dat de gebruikers stroom moet door gaan naar de volgende stap: de pagina kenmerk verzameling.

In een vervolg reactie kan de API claims retour neren. Als een claim wordt geretourneerd door de API, doet de claim het volgende:

- Hiermee wordt het invoer veld vooraf ingevuld op de pagina kenmerk verzameling.

Bekijk een voor beeld van een [vervolg reactie](#example-of-a-continuation-response).

#### <a name="blocking-response"></a>Antwoord blok keren

Een blokkerende reactie sluit de gebruikers stroom af. Het kan worden uitgegeven door de API om de voortzetting van de gebruikers stroom te stoppen door een blok pagina voor de gebruiker weer te geven. Op de pagina blok keren wordt de weer gegeven `userMessage` van de API.

Bekijk een voor beeld van een [blokkerend antwoord](#example-of-a-blocking-response).

## <a name="before-creating-the-user"></a>Voordat u de gebruiker maakt

Een API-connector in deze stap van het registratie proces wordt aangeroepen na de pagina kenmerk verzameling, als er een is opgenomen. Deze stap wordt altijd aangeroepen voordat een gebruikers account wordt gemaakt.

<!-- The following are examples of scenarios you might enable at this point during sign-up: -->
<!-- 
- Validate user input data and ask a user to resubmit data.
- Block a user sign-up based on data entered by the user.
- Perform identity verification.
- Query external systems for existing data about the user and overwrite the user-provided value. -->

### <a name="example-request-sent-to-the-api-at-this-step"></a>Voorbeeld aanvraag die wordt verzonden naar de API in deze stap

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```
De exacte claims die worden verzonden naar de API, zijn afhankelijk van welke gegevens van de gebruiker worden verzameld of door de ID-provider worden verstrekt.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Verwachte antwoord typen van de Web-API bij deze stap

Wanneer de Web-API tijdens een gebruikers stroom een HTTP-aanvraag van Azure AD ontvangt, kan deze de volgende antwoorden retour neren:

- Vervolg reactie
- Antwoord blok keren
- Validatie antwoord

#### <a name="continuation-response"></a>Vervolg reactie

Een vervolg antwoord geeft aan dat de gebruikers stroom moet door gaan naar de volgende stap: Maak de gebruiker in de Directory.

In een vervolg reactie kan de API claims retour neren. Als een claim wordt geretourneerd door de API, doet de claim het volgende:

- Overschrijft elke waarde die al is toegewezen aan de claim op de pagina kenmerk verzameling.

Bekijk een voor beeld van een [vervolg reactie](#example-of-a-continuation-response).

#### <a name="blocking-response"></a>Antwoord blok keren
Een blokkerende reactie sluit de gebruikers stroom af. Het kan worden uitgegeven door de API om de voortzetting van de gebruikers stroom te stoppen door een blok pagina voor de gebruiker weer te geven. Op de pagina blok keren wordt de weer gegeven `userMessage` van de API.

Bekijk een voor beeld van een [blokkerend antwoord](#example-of-a-blocking-response).

### <a name="validation-error-response"></a>Validatie-fout bericht
 Wanneer de API reageert met een antwoord op validatie-fout, blijft de gebruikers stroom op de pagina kenmerk verzameling en `userMessage` wordt er een weer gegeven voor de gebruiker. De gebruiker kan vervolgens het formulier bewerken en opnieuw verzenden. Dit type antwoord kan worden gebruikt voor invoer validatie.

Bekijk een voor beeld van een [validatie fout](#example-of-a-validation-error-response).

## <a name="example-responses"></a>Voorbeeld reacties

### <a name="example-of-a-continuation-response"></a>Voor beeld van een vervolg reactie

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| Parameter                                          | Type              | Vereist | Beschrijving                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| versie                                            | Tekenreeks            | Ja      | De versie van de API.                                                                                                                                                                                                                                                                |
| actie                                             | Tekenreeks            | Ja      | Waarde moet zijn `Continue` .                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Nee       | Geretourneerde waarden kunnen waarden overschrijven die zijn verzameld van een gebruiker. Ze kunnen ook worden geretourneerd in het token als het is geselecteerd als een _ *-toepassings claim * *.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Nee       | De claim hoeft niet te bevatten `_<extensions-app-id>_` . Geretourneerde waarden kunnen waarden overschrijven die zijn verzameld van een gebruiker. Ze kunnen ook worden geretourneerd in het token als deze als een **toepassings claim** wordt geselecteerd.  |

### <a name="example-of-a-blocking-response"></a>Voor beeld van een blokkerend antwoord

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
}

```

| Parameter   | Type   | Vereist | Beschrijving                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| versie     | Tekenreeks | Ja      | De versie van de API.                                                    |
| actie      | Tekenreeks | Ja      | Waarde moet `ShowBlockPage`                                              |
| userMessage | Tekenreeks | Ja      | Bericht dat wordt weergegeven aan de gebruiker.                                            |

**Eind gebruikers ervaring met een blokkerend antwoord**

![Voor beeld blok pagina](./media/add-api-connector/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Voor beeld van een validatie-fout bericht



```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code."
}
```

| Parameter   | Type    | Vereist | Beschrijving                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| versie     | Tekenreeks  | Ja      | De versie van de API.                                                    |
| actie      | Tekenreeks  | Ja      | Waarde moet zijn `ValidationError` .                                           |
| status      | Geheel getal | Ja      | Dit moet een waarde zijn `400` voor een ValidationError-antwoord.                        |
| userMessage | Tekenreeks  | Ja      | Bericht dat wordt weergegeven aan de gebruiker.                                            |

> [!NOTE]
> De HTTP-status code moet naast de status waarde in de hoofd tekst van het antwoord ' 400 ' zijn.

**Eind gebruikers ervaring met validatie-fout bericht**

![Voorbeeld validatie pagina](./media/add-api-connector/validation-error-postal-code.png)


## <a name="best-practices-and-how-to-troubleshoot"></a>Aanbevolen procedures en problemen oplossen

### <a name="using-serverless-cloud-functions"></a>Cloud functies zonder server gebruiken
Serverloze functies, zoals HTTP-triggers in Azure Functions, bieden een eenvoudige manier om API-eind punten te maken voor gebruik met de API-connector. U kunt de Cloud functie zonder server gebruiken om [bijvoorbeeld](code-samples.md#api-connectors)validatie logica uit te voeren en aanmeldings pogingen te beperken tot specifieke e-mail domeinen. De Cloud functie zonder server kan ook andere web-Api's, gebruikers archieven en andere Cloud Services aanroepen en aanroepen voor complexere scenario's.

### <a name="best-practices"></a>Aanbevolen procedures
Zorg ervoor dat:
* Uw API volgt de API-aanvraag-en respons contracten zoals hierboven wordt beschreven. 
* De **eind punt-URL** van de API-connector verwijst naar het juiste API-eind punt.
* Uw API controleert expliciet op null-waarden van ontvangen claims.
* Uw API reageert zo snel mogelijk om een onervaren gebruikers ervaring te garanderen.
    * Als u een serverloze functie of schaal bare webservice gebruikt, gebruikt u een hosting abonnement waarmee de API ' actief ' of ' warme ' wordt bewaard. in productie. Voor Azure Functions is de aanbevolen het Premium- [abonnement](../azure-functions/functions-scale.md) te gebruiken


### <a name="use-logging"></a>Logboek registratie gebruiken
Over het algemeen is het handig om de hulpprogram ma's voor logboek registratie te gebruiken die zijn ingeschakeld door uw web API-service, zoals [Application Insights](../azure-functions/functions-monitoring.md), om uw API te controleren op onverwachte fout codes, uitzonde ringen en slechte prestaties.
* Monitor voor HTTP-status codes die niet HTTP 200 of 400 zijn.
* Een 401-of 403 HTTP-status code geeft meestal aan dat er een probleem is met uw verificatie. Controleer de Authentication Layer van de API en de bijbehorende configuratie in de API-connector.
* Gebruik zo nodig meer agressieve niveaus van logboek registratie (bijvoorbeeld ' tracering ' of ' fout opsporing ') in ontwikkeling.
* Controleer uw API op lange reactie tijden.

## <a name="next-steps"></a>Volgende stappen
<!-- - Learn how to [add a custom approval workflow to sign-up](add-approvals.md) -->
- Ga aan de slag met onze voor [beelden](code-samples.md#api-connectors).
