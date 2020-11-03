---
title: 'Quickstart: Verificatie toevoegen aan een knooppunt-web-app met MSAL-knooppunt | Azure'
titleSuffix: Microsoft identity platform
description: In deze quickstart leert u hoe u verificatie kunt implementeren met een Node.js-web-app en de Microsoft Authentication Library (MSAL) voor Node.js.
services: active-directory
author: amikuma
manager: saeeda
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2020
ms.author: amikuma
ms.custom: aaddev, scenarios:getting-started, languages:js, devx-track-js
ms.openlocfilehash: cfe1b66662d73c18934ef9ecc884adee79493eb6
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92640973"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-node-web-app-using-the-auth-code-flow"></a>Quickstart: Gebruikers aanmelden en een toegangstoken verkrijgen in een Node.js-web-app met behulp van de verificatiecodestroom

In deze quickstart voert u een codevoorbeeld uit waarin wordt getoond hoe een Node.js-web-app gebruikers van persoonlijke accounts, werkaccounts en schoolaccounts kan aanmelden met behulp van de verificatiecodestroom. In het codevoorbeeld ziet u ook hoe u een toegangstoken krijgt om een web-API aan te roepen, in dit geval de Microsoft Graph API. Zie [Hoe het voorbeeld werkt](#how-the-sample-works) voor een illustratie.

In deze quickstart wordt de Microsoft Authentication Library voor Node.js (MSAL Node) gebruikt met de verificatiecodestroom.

> [!IMPORTANT]
> MSAL Node [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [Gratis een Azure-abonnement maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) of een andere code-editor

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>De quickstart-toepassing registreren en downloaden
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
> 1. Als u via uw account toegang hebt tot meerdere tenants, selecteert u uw account rechts bovenin en stelt u de portalsessie in op de Azure Active Directory-tenant die u wilt gebruiken.
> 1. Selecteer [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908).
> 1. Selecteer **Nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
> 1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
> 1. Stel de waarde van **Omleidings-URI** in op `http://localhost:3000/redirect`.
> 1. Selecteer **Registreren**. 
> 1. Noteer de waarde **Toepassings-id (client)** op de app-pagina **Overzicht** voor later gebruik.
> 1. Onder **Certificaten & geheimen** selecteert u **Nieuw clientgeheim**.  Laat de beschrijving leeg en de standaard vervaldatum staan en klik vervolgens op **Toevoegen**.
> 1. Noteer de **Waarde** van het **Clientgeheim** voor later gebruik.

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

> [!div renderon="docs"]
> Als u het project wilt uitvoeren met een webserver met behulp van Node.js, [downloadt u de kernprojectbestanden](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Het project uitvoeren met een webserver met behulp van Node.js

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-node-app"></a>Stap 3: De Node-app configureren
>
> Pak het project uit en open de map *ms-identity-node-main* en open vervolgens het *index.js* -bestand.
> Stel de `clientID` in met de **Toepassings-id (client)** .
> Stel de `clientSecret` in met de **Waarde** van het **Clientgeheim**.
>
>```javascript
>const config = {
>    auth: {
>        clientId: "Enter_the_Application_Id_Here",
>        authority: "https://login.microsoftonline.com/common",
>        clientSecret: "Enter_the_Client_Secret_Here"
>    },
>    system: {
>        loggerOptions: {
>            loggerCallback(loglevel, message, containsPii) {
>                console.log(message);
>            },
>            piiLoggingEnabled: false,
>            logLevel: msal.LogLevel.Verbose,
>        }
>    }
>};
> ```

> [!div renderon="docs"]
>
> Wijzig de waarden in de sectie `config`, zoals hier wordt beschreven:
>
> - `Enter_the_Application_Id_Here` is de **Toepassings-id (client-id)** voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Client_Secret_Here` is de **Waarde** van het **Clientgeheim** voor de toepassing die u hebt geregistreerd.
>
> De standaard `authority`-waarde vertegenwoordigt de belangrijkste (globale) Azure-cloud:
>
> ```javascript
> authority: "https://login.microsoftonline.com/common",
> ```
>
> > [!TIP]
> > Als u de waarde van **Toepassings-id (client)** wilt vinden, gaat u naar de pagina **Overzicht** van de app-registratie in de Azure Portal. Ga naar **Certificaten & geheimen** om een nieuw **Clientgeheim** te verkrijgen of te genereren.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Stap 3: Uw app is geconfigureerd en klaar om te worden uitgevoerd
>
> [!div renderon="docs"]
>
> #### <a name="step-4-run-the-project"></a>Stap 4: Het project uitvoeren

Voer het project uit met behulp van Node.js:

1. Voer de volgende opdracht uit vanuit de projectmap om de server te starten:
    ```console
    npm install
    npm start
    ```
1. Blader naar `http://localhost:3000/`.

1. Selecteer **Aanmelden** om het aanmeldingsproces te starten.

    De eerste keer dat u zich aanmeldt, wordt u gevraagd om toestemming te geven zodat de toepassing uw profiel kan openen en u kan aanmelden. Nadat u bent aangemeld, wordt een logboekbericht weergegeven in de opdrachtregel.

## <a name="more-information"></a>Meer informatie

### <a name="how-the-sample-works"></a>Hoe het voorbeeld werkt

Het voorbeeld, wanneer dit wordt uitgevoerd, fungeert als host voor een webserver op localhost, poort 3000. Wanneer een webbrowser toegang heeft tot deze site, wordt de gebruiker door het voorbeeld onmiddellijk omgeleid naar een Microsoft-verificatiepagina. Daarom bevat het voorbeeld geen *html* of weergave-elementen. Verificatie geslaagd geeft het bericht ‘OK’ weer.

### <a name="msal-node"></a>MSAL Node

De MSAL Node-bibliotheek meldt gebruikers aan en verzoekt dat tokens worden gebruikt voor toegang tot een API die wordt beveiligd door het Microsoft Identity-platform. U kunt de nieuwste versie downloaden met behulp van Node.js Package Manager (npm):

```console
npm install @azure/msal-node
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verificatie toevoegen aan een bestaande web-app - GitHub-codevoorbeeld >](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/msal-node-samples/standalone-samples/auth-code/readme.md)
