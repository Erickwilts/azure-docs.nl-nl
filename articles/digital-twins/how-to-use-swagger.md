---
title: Over het gebruik van Azure digitale dubbele verwijzing Swagger | Microsoft Docs
description: Informatie over het gebruik van Azure digitale dubbele Swagger-referentiedocumentatie.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 842c35a1575c7bcf6f547fb04d5680178b3bee78
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730352"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Digitale dubbele Swagger referentiedocumentatie voor Azure

Elke ingerichte Azure digitale Twins-instantie bevat een eigen automatisch gegenereerde Swagger-referentiedocumentatie.

[Swagger](https://swagger.io/), of [OpenAPI](https://www.openapis.org/), koppelt zijn complexe API-informatie in een resource voor de interactieve en taal-neutraal verwijzing netwerk. Swagger bevat essentiële referentiemateriaal over de JSON-nettoladingen, HTTP-methoden, en specifieke te gebruiken voor het uitvoeren van bewerkingen op een API-eindpunten.

## <a name="swagger-summary"></a>Samenvatting van swagger

Swagger biedt een interactieve samenvatting van uw API, waaronder:

* API- en modelgegevens.
* REST API-eindpunten die de vereiste aanvraag nettoladingen, headers, parameters, context paden en HTTP-methoden opgeeft.
* Testen van de API-functies.
* Voorbeeld van de informatie die wordt gebruikt om te valideren en controleer of HTTP-antwoorden.
* Informatie over foutcodes.

Swagger is een handig hulpmiddel om u te helpen bij het ontwikkelen en testen oproepen naar de Azure digitale dubbels Management API's.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Naslagmateriaal

Het automatisch gegenereerde Swagger-referentiemateriaal levert een kort overzicht van belangrijke concepten, beschikbaar Management-API-eindpunten en een beschrijving van elk objectmodel om te helpen ontwikkelen en testen.

Een beknopt overzicht beschrijft de API.

[![Swagger-bovenaan](media/how-to-use-swagger/swagger_management_top.PNG)](media/how-to-use-swagger/swagger_management_top.PNG#lightbox)

Beheer API objectmodellen worden ook weergegeven.

[![Swagger-modellen](media/how-to-use-swagger/swagger_management_models.PNG)](media/how-to-use-swagger/swagger_management_models.PNG#lightbox)

U kunt elke vermelde objectmodel voor een meer gedetailleerd overzicht van belangrijke kenmerken selecteren.

[![Swagger-model](media/how-to-use-swagger/swagger_management_model.PNG)](media/how-to-use-swagger/swagger_management_model.PNG#lightbox)

De gegenereerde Swagger-objectmodellen zijn handig om te zien van alle beschikbare Azure digitale dubbels [objecten en API's](./concepts-objectmodel-spatialgraph.md). Ontwikkelaars kunnen deze resource gebruiken als ze oplossingen op Azure digitale dubbels bouwen.

## <a name="endpoint-summary"></a>Samenvatting van eindpunt

Swagger bevat ook een uitgebreide overzicht van alle eindpunten waaruit de Management API's.

Elke vermelde eindpunt omvat ook de informatie vereist aanvraag, zoals de:

* Vereiste parameters.
* Vereiste parametergegevenstypen.
* HTTP-methode voor toegang tot de resource.

[![Swagger-eindpunten](media/how-to-use-swagger/swagger_management_endpoints.PNG)](media/how-to-use-swagger/swagger_management_endpoints.PNG#lightbox)

Selecteer elke resource een gedetailleerder overzicht.

## <a name="use-swagger-to-test-endpoints"></a>Gebruik Swagger om te testen eindpunten

Een van de krachtige functies van die swagger biedt is de mogelijkheid voor het testen van een API-eindpunt rechtstreeks via de UI-documentatie.

Nadat u een bepaald eindpunt selecteert, ziet u **Try it out in**.

[![Probeer swagger](media/how-to-use-swagger/swagger_management_try.PNG)](media/how-to-use-swagger/swagger_management_try.PNG#lightbox)

Vouw deze sectie om de invoervelden voor elke vereiste en optionele parameter. Voer de juiste waarden in en selecteer **Execute**.

[![Er is geprobeerd swagger](media/how-to-use-swagger/swagger_management_tried.PNG)](media/how-to-use-swagger/swagger_management_tried.PNG#lightbox)

Nadat u de test uitvoert, kunt u de antwoordgegevens valideren.

## <a name="swagger-response-data"></a>Antwoordgegevens voor swagger

Elke vermelde eindpunt bevat ook de hoofdtekst van de antwoordgegevens voor het valideren van de ontwikkeling en tests. Deze voorbeelden zijn onder meer de statuscodes en JSON die u wilt zien voor geslaagde HTTP-aanvragen.

[![Swagger-antwoord](media/how-to-use-swagger/swagger_management_response.PNG)](media/how-to-use-swagger/swagger_management_response.PNG#lightbox)

De voorbeelden zijn ook foutcodes zodat fouten opspoort of mislukt tests verbeteren.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0-autorisatie

Zie voor meer informatie over het testen van interactief aanvragen die zijn beveiligd met OAuth 2.0, de [officiële documentatie](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> De principal gebruiker die de digitale dubbels Azure-resource gemaakt heeft een roltoewijzing ruimte beheerder en is mogelijk te maken van aanvullende roltoewijzingen voor andere gebruikers.

1. Volg de stappen in [in deze snelstartgids](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) te maken van een Azure AD-toepassing van het type ***Web-app / API***. Of u een bestaande app-registratie opnieuw kunt gebruiken.

2. De volgende antwoord-url toevoegen aan de app-registratie:

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | Name  | Vervangen door | Voorbeeld |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | De URL van uw Management REST API-documentatie te vinden in de portal  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

3. Verleent machtigingen voor uw app voor toegang tot digitale dubbels van Azure. Onder **vereiste machtigingen**, voer `Azure Digital Twins` en selecteer **gedelegeerde machtigingen**. Selecteer vervolgens **machtigingen verlenen**.

    ![Azure AD-app-registraties toevoegen api](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

4. Configureer het toepassingsmanifest voor OAuth 2.0-impliciete stroom toestaan. Klik op **Manifest** manifest voor de toepassing voor uw app te openen. Stel *oauth2AllowImplicitFlow* naar `true`.

    ![Azure AD-impliciete stroom](../../includes/media/digital-twins-permissions/aad-app-allow-implicit-flow.png)

5. Kopieer de ID van uw Azure AD-app.

Na het voltooien van de registratie van de Azure Active Directory:

6. Klik op de **autoriseren** knop op de pagina van uw swagger.

    [![Klik op de Swagger knop autoriseren](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

7. Plak de toepassings-ID in de **client_id** veld.

    [![Swagger client_id field](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

8. U wordt vervolgens omgeleid naar het volgende modale succes.

    [![Swagger-omleiding modale](media/how-to-use-swagger/swagger_auth_redirect.PNG)](media/how-to-use-swagger/swagger_auth_redirect.PNG#lightbox)

## <a name="next-steps"></a>Volgende stappen

- Lees meer informatie over Azure digitale dubbels objectmodellen en de grafiek ruimtelijke intelligence lezen [digitale dubbels Azure begrijpen object modellen](./concepts-objectmodel-spatialgraph.md).

- Lees voor meer informatie over verificatie met uw API Management, [verifiëren met API's](./security-authenticating-apis.md).