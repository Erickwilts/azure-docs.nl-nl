---
title: Micro soft Identity platform java script Quick Start-Azure
description: Meer informatie over hoe Java script-toepassingen een API kunnen aanroepen die toegangs tokens vereist met behulp van micro soft Identity platform.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ca7e7f282d63160cd1f729fba00f6d7d7704270
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334130"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application"></a>Quickstart: Gebruikers aanmelden en een toegangs token verkrijgen van een Java script-toepassing met één pagina

In deze Quick Start leert u hoe u een code voorbeeld gebruikt dat laat zien hoe een Java script-toepassing met één pagina (SPA) kan aanmelden bij gebruikers van persoonlijke accounts, werk accounts en school accounts. Een Java script-SPA kan ook een toegangs token verkrijgen om de Microsoft Graph-API of een web-API aan te roepen.

![Hoe de voor beeld-app in deze Snelstartgids werkt](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Vereisten

Voor deze Quick start is de volgende configuratie vereist:
* Als u het project wilt uitvoeren met een node. js-server, downloadt en installeert u [node. js](https://nodejs.org/en/download/).
* Als u de project bestanden wilt bewerken, downloadt en installeert u [Visual Studio code](https://code.visualstudio.com/download).
* Down load en Installeer [Visual studio 2019](https://visualstudio.microsoft.com/downloads/)om het project uit te voeren als een Visual Studio-oplossing.

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>De quickstart-toepassing registreren en downloaden
> Gebruik een van de volgende opties om uw Quick Start-toepassing te starten.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1 (Express): registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
> 1. Als uw account u toegang geeft tot meer dan één Tenant, selecteert u het account in de rechter bovenhoek en stelt u vervolgens uw portal-sessie in op de Azure Active Directory (Azure AD)-Tenant die u wilt gebruiken.
> 1. Ga naar het deel venster nieuwe [Azure Portal-app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) .
> 1. Voer een naam in voor uw toepassing en selecteer **registreren**.
> 1. Volg de instructies om uw nieuwe toepassing te downloaden en automatisch te configureren.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2 (hand matig): registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> 1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
>
> 1. Als uw account u toegang geeft tot meer dan één Tenant, selecteert u uw account in de rechter bovenhoek en stelt u vervolgens uw portal-sessie in op de Azure AD-Tenant die u wilt gebruiken.
> 1. Ga naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) .
> 1. Selecteer **nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
> 1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
> 1. Selecteer onder de sectie omleidings- **URI** in de vervolg keuzelijst het **webplatform en** stel vervolgens de waarde in op `http://localhost:30662/`.
> 1. Selecteer **Registreren**. Noteer de waarde van de **toepassing (client)** op de pagina app- **overzicht** voor later gebruik.
> 1. Voor deze quickstart moet de [Impliciete toekenningsstroom](v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het linkerdeel venster van de geregistreerde toepassing **verificatie**.
> 1. Selecteer in de sectie **Geavanceerde instellingen** onder **impliciete toekenning**de selectie vakjes **id-tokens** en **toegangs tokens** . ID-tokens en toegangs tokens zijn vereist omdat deze app gebruikers moet aanmelden en een API kan aanroepen.
> 1. Selecteer boven in het deel venster **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
> Het code voorbeeld voor deze Quick Start werkt alleen als `http://localhost:30662/` u een omleidings-URI toevoegt en impliciete **toekenning**inschakelt.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-javascript/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

Selecteer de optie die geschikt is voor uw ontwikkel omgeving:

* Als u het project wilt uitvoeren met een webserver met behulp van node. js, [downloadt u de kern project bestanden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip). Als u de bestanden wilt openen, gebruikt u een editor zoals [Visual Studio code](https://code.visualstudio.com/).

* Beschrijving Als u het project wilt uitvoeren met de IIS-server, [downloadt u het Visual Studio-project](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip). Pak het zip-bestand uit naar een lokale map (bijvoorbeeld *C:\Azure-samples*).



#### <a name="step-3-configure-your-javascript-app"></a>Stap 3: Configureer de JavaScript-app

> [!div renderon="docs"]
> Bewerk in de map *JavaScriptSPA* *index. html*en stel de `clientID` waarden en `authority` in op `msalConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> Bewerk in de map *JavaScriptSPA* *index. html*en vervang `msalConfig` deze door de volgende code:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_info_here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```
> [!div renderon="portal"]
> > [!NOTE]
> > Deze Quick Start biedt ondersteuning voor Enter_the_Supported_Account_Info_Here.


> [!div renderon="docs"]
>
> Waar:
> - Enter_the_Application_Id_here > is de **client-id** van de toepassing die u hebt geregistreerd.  *\<*
> - Enter_the_Tenant_info_here > is ingesteld op een van de volgende opties:  *\<*
>    - Als uw toepassing *accounts in deze organisatie Directory*ondersteunt, vervangt u deze waarde door de **Tenant-id** of **Tenant naam** (bijvoorbeeld *contoso.Microsoft.com*).
>    - Als uw toepassing *accounts in een organisatorische Directory*ondersteunt, vervangt u deze waarde door **organisaties**.
>    - Als uw toepassing *accounts in een organisatorische map en persoonlijke micro soft-accounts*ondersteunt, vervangt u deze waarde door **common**. Als u de ondersteuning voor *persoonlijke micro soft-accounts*wilt beperken, moet u deze waarde vervangen door **consumenten**.
>
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)** , **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.
>

#### <a name="step-4-run-the-project"></a>Stap 4: Het project uitvoeren

* Als u [node. js](https://nodejs.org/en/download/)gebruikt:

    1. Als u de server wilt starten, voert u de volgende opdracht uit vanuit de projectmap:

        ```batch
        npm install
        node server.js
        ```

    1. Open een webbrowser en ga `http://localhost:30662/`naar.
    1. Selecteer **Aanmelden** om de aanmelding te starten en roep vervolgens de Microsoft Graph-API aan.


* Als u [Visual Studio](https://visualstudio.microsoft.com/downloads/)gebruikt, selecteert u de project-oplossing en selecteert u vervolgens F5 om het project uit te voeren.

Nadat de toepassing door de browser is geladen, selecteert u **Aanmelden**. De eerste keer dat u zich aanmeldt, wordt u gevraagd uw toestemming te geven om uw profiel te openen en u aan te melden. Nadat u bent aangemeld, moeten de gegevens van uw gebruikers profiel worden weer gegeven op de pagina.

## <a name="more-information"></a>Meer informatie

### <a name="msaljs"></a>msal.js

De MSAL-bibliotheek ondertekent gebruikers en vraagt de tokens aan die worden gebruikt voor toegang tot een API die wordt beveiligd door micro soft Identity platform. Het bestand Quick Start *index. html* bevat een verwijzing naar de bibliotheek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> U kunt de voor gaande versie vervangen door de nieuwste release versie onder [MSAL. js-releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


Als node. js is geïnstalleerd, kunt u ook de nieuwste versie downloaden via node. js Package Manager (NPM):

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL initialiseren

In de Quick Start-code ziet u ook hoe u de MSAL-bibliotheek initialiseert:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Waar  |  |
> |---------|---------|
> |`ClientId`     | De toepassings-ID van de toepassing die is geregistreerd in de Azure Portal.|
> |`authority`    | Beschrijving De CA-URL die ondersteuning biedt voor account typen, zoals eerder is beschreven in de sectie configuratie. De standaard instantie is `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | Beschrijving Hiermee stelt u de opslag voor de browser in voor de verificatie status. De standaard waarde is sessionStorage.   |
> |`storeAuthStateInCookie`  | Beschrijving De bibliotheek waarin de status van de verificatie aanvraag wordt opgeslagen die vereist is voor de validatie van de verificatie stromen in de browser cookies. Deze cookie is ingesteld voor IE-en Edge-browsers om bepaalde [bekende problemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues)te verhelpen. |

Zie [client toepassingen initialiseren](msal-js-initializing-client-applications.md)voor meer informatie over beschik bare opties die kunnen worden geconfigureerd.

### <a name="sign-in-users"></a>Gebruikers aanmelden

In het volgende codefragment wordt weergegeven hoe u gebruikers kunt aanmelden:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Beschrijving Bevat bereiken die worden aangevraagd voor toestemming van de gebruiker bij het aanmelden. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`). |

> [!TIP]
> U kunt ook de `loginRedirect` methode gebruiken om de huidige pagina om te leiden naar de aanmeldings pagina in plaats van een pop-upvenster.

### <a name="request-tokens"></a>Tokens aanvragen

MSAL maakt gebruik van drie methoden om tokens `acquireTokenRedirect`te `acquireTokenPopup`verkrijgen:, en`acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Met de methode `acquireTokenSilent` wordt het verkrijgen en vernieuwen van tokens verwerkt zonder tussenkomst van de gebruiker. Nadat de methode `loginRedirect` of `loginPopup` de eerste keer is uitgevoerd, wordt voor volgende aanroepen meestal de methode `acquireTokenSilent` gebruikt om tokens te verkrijgen die worden gebruikt voor toegang tot beveiligde resources. Aanroepen voor het aanvragen of vernieuwen van tokens worden op de achtergrond uitgevoerd.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Bevat bereiken die worden aangevraagd om te worden geretourneerd in het toegangstoken voor de API. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Er zijn situaties waarin u gebruikers moet dwingen te communiceren met het micro soft Identity platform-eind punt. Bijvoorbeeld:
* Gebruikers moeten mogelijk hun referenties opnieuw invoeren, omdat het wacht woord is verlopen.
* Uw toepassing vraagt toegang tot extra resource scopes aan die de gebruiker nodig heeft om toestemming te geven.
* Verificatie met twee factoren is vereist.

Het gebruikelijke aanbevolen patroon voor de meeste toepassingen is om `acquireTokenSilent` eerst aan te roepen, vervolgens de uitzonde ring `acquireTokenPopup` op te `acquireTokenRedirect`vangen en vervolgens (of) te bellen om een interactieve aanvraag te starten.

Het oproepen `acquireTokenPopup` van de resultaten in een pop-upvenster voor het aanmelden. (Of `acquireTokenRedirect` resulteert in het omleiden van gebruikers naar het micro soft Identity platform-eind punt.) In dat venster moeten gebruikers communiceren door hun referenties te bevestigen, toestemming te geven voor de vereiste resource of de twee ledige verificatie uit te voeren.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> [!NOTE]
> Deze Snelstartgids maakt gebruik `loginRedirect` van `acquireTokenRedirect` de methoden en van micro soft Internet Explorer, vanwege een [bekend probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) met betrekking tot het verwerken van pop-upvensters door Internet Explorer.

## <a name="next-steps"></a>Volgende stappen

Zie voor een gedetailleerde stapsgewijze hand leiding voor het bouwen van de toepassing voor deze Quick Start:

> [!div class="nextstepaction"]
> [Zelf studie om u aan te melden en MS Graph aan te roepen](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Als u wilt zoeken in de MSAL-opslag plaats op documentatie, veelgestelde vragen, problemen en meer, raadpleegt u:

> [!div class="nextstepaction"]
> [MSAL.js GitHub repo](https://github.com/AzureAD/microsoft-authentication-library-for-js)

Help ons het micro soft Identity-platform te verbeteren. Vertel ons wat u denkt door een korte enquête met twee vragen te volt ooien.

> [!div class="nextstepaction"]
> [Micro soft Identity platform-enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)