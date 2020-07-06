---
title: Gebruikers aanmelden in Angular-apps met één pagina - Azure
titleSuffix: Microsoft identity platform
description: Meer informatie over hoe u met een Angular-app een API kunt aanroepen waarvoor toegangstokens zijn vereist, met behulp van het Microsoft-identiteitsplatform.
services: active-directory
author: jasonnutter
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ms.topic: quickstart
ms.workload: identity
ms.date: 03/18/2020
ms.author: janutter
ms.openlocfilehash: f457e876cb9484fce29cba35c7570572b2771aed
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860049"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-an-angular-single-page-application"></a>Quickstart: Gebruikers aanmelden en een toegangstoken verkrijgen in een Angular-toepassing met één pagina

In deze quickstart gebruikt u een codevoorbeeld om te leren hoe een Angular-toepassing met één pagina (SPA) gebruikers kan aanmelden met persoonlijke Microsoft-accounts, werkaccounts of schoolaccounts. Een Angular-SPA-verificatie kan ook een toegangstoken krijgen om de Microsoft Graph API of een web-API aan te roepen.

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement. [Maak er gratis een](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Node.js](https://nodejs.org/en/download/).
* [Visual Studio Code](https://code.visualstudio.com/download) voor het bewerken van projectbestanden of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) om het project uit te voeren.

> [!div renderon="docs"]
> ## <a name="register-and-download-the-quickstart-app"></a>De quickstart-app registreren en downloaden
> Gebruik een van de volgende opties om de quickstart-app te starten.
>
> ### <a name="option-1-express-register-and-automatically-configure-the-app-and-then-download-the-code-sample"></a>Optie 1 (express): Registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
> 1. Als uw account toegang heeft tot meer dan één tenant, selecteert u het account in de rechterbovenhoek en stelt u de portalsessie in op de Azure Active Directory-tenant die u wilt gebruiken.
> 1. Open het nieuwe deelvenster [App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) in de Azure-portal.
> 1. Voer een naam in voor de toepassing en selecteer dan **Registreren**.
> 1. Ga naar het deelvenster quickstart en bekijk de Angular-quickstart. Volg de instructies om de nieuwe toepassing te downloaden en automatisch te configureren.
>
> ### <a name="option-2-manual-register-and-manually-configure-the-application-and-code-sample"></a>Optie 2 (handmatig): Registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-the-application"></a>Stap 1: Registreer de toepassing
>
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
> 1. Als uw account toegang heeft tot meer dan één tenant, selecteert u uw account rechts bovenin en stelt u de portalsessie in op de Azure Active Directory-tenant die u wilt gebruiken.
> 1. Volg de [instructies om een toepassing met één pagina te registreren](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-app-registration) in de Azure-portal.
> 1. Voeg een nieuw platform toe aan het deelvenster **Verificatie** van de registratie van uw app en registreer de omleidings-URI: `http://localhost:4200/`.
> 1. In deze quickstart wordt gebruikgemaakt van de [impliciete toewijzingsstroom](v2-oauth2-implicit-grant-flow.md). Selecteer de instellingen **impliciete toekenning** voor **Id-tokens** en **Toegangstokens**. Id-tokens en toegangstokens zijn vereist, omdat via de app gebruikers moeten worden aangemeld en een API moet worden aangeroepen.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-the-azure-portal"></a>Stap 1: De toepassing configureren in de Azure-portal
> Voor een juiste werking van het codevoorbeeld uit deze quickstart moet u een omleidings-URI toevoegen zoals **http://localhost:4200/** en **Impliciete toekenning** inschakelen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-javascript/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-code-sample"></a>Stap 2: Het codevoorbeeld downloaden
>[!div renderon="docs"]
>Als u het project wilt uitvoeren met een webserver met behulp van Node.js, [kloont u de voorbeeldopslagplaats](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) of [downloadt u de kernprojectbestanden](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip). Open de bestanden met behulp van een editor zoals Visual Studio Code.

> [!div renderon="portal" id="autoupdate" class="sxs-lookup nextstepaction"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular/archive/master.zip)

> [!div renderon="docs"]
>#### <a name="step-3-configure-the-javascript-app"></a>Stap 3: De JavaScript-app configureren
>
> Bewerk in de map *src/app* *app.module.ts* en stel de waarden `clientId` en `authority` in onder `MsalModule.forRoot`.
>
>```javascript
>MsalModule.forRoot({
>    auth: {
>        clientId: 'Enter_the_Application_Id_here', // This is your client ID
>        authority: 'https://login.microsoftonline.com/Enter_the_Tenant_Info_Here', // This is your tenant info
>        redirectUri: 'Enter_the_Redirect_Uri_Here' // This is your redirect URI
>    },
>    cache: {
>        cacheLocation: 'localStorage',
>        storeAuthStateInCookie: isIE, // set to true for IE 11
>    },
>},
> //... )
>```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Enter_the_Supported_Account_Info_Here


> [!div renderon="docs"]
>
> Vervang deze waarden:
>
>|Waardenaam|Beschrijving|
>|---------|---------|
>|Voer_hier_de_toepassings-id_in|Op de pagina **Overzicht** van de registratie van uw toepassing, is dit uw waarde voor **Toepassings-id (client)** . |
>|Voer_hier_het_cloud-exemplaar_in|Dit is het exemplaar van de Azure-cloud. Voer **https://login.microsoftonline.com** in voor de primaire of algemene Azure-cloud. Raadpleeg [Nationale clouds](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud) voor nationale clouds (bijvoorbeeld China).|
>|Voer_hier_tenantgegevens_in| Stel een van de volgende opties in: Als uw toepassing *accounts in deze organisatiemap ondersteunt*, vervangt u deze waarde door de map-id (tenant) of tenantnaam (bijvoorbeeld **contoso.microsoft.com**). Als uw toepassing *accounts in elke organisatiemap* ondersteunt, vervang deze waarde dan door **organisaties**. Als uw toepassing *accounts in elke organisatiemap en persoonlijke Microsoft-accounts* ondersteunt, vervang deze waarde dan door **algemeen**. Als u de ondersteuning wilt beperken tot *alleen persoonlijke Microsoft-accounts*, vervang deze waarde dan door **consumenten**. |
>|Voer_hier_de_omleidings-URI_in|Vervang door **http://localhost:4200** .|
>|cacheLocation  | (Optioneel) Stel de browseropslag in voor de verificatiestatus. De standaardwaarde is **sessionStorage**.   |
>|storeAuthStateInCookie  | (Optioneel) De bibliotheek identificeren waarin de status van de verificatie-aanvraag wordt opgeslagen. Deze status is vereist voor het valideren van de verificatiestromen in de browsercookies. Deze cookie is ingesteld voor Internet Explorer en Microsoft Edge om deze twee browsers te kunnen ondersteunen. Zie de pagina met [bekende problemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues->on-IE-and-Edge-Browser#issues). |
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)** , **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.

Raadpleeg [Clienttoepassingen initialiseren](msal-js-initializing-client-applications.md) voor meer informatie over beschikbare opties die u kunt configureren. 

U vindt de broncode voor de MSAL.js-bibliotheek in de opslagplaats [AzureAD/microsoft-authentication-library-for-js](https://github.com/AzureAD/microsoft-authentication-library-for-js) op GitHub.

>[!div class="sxs-lookup" renderon="portal"]
>#### <a name="step-3-run-the-project"></a>Stap 3: Het project uitvoeren

>[!div renderon="docs"]
>#### <a name="step-4-run-the-project"></a>Stap 4: Het project uitvoeren

Als u Node.js gebruikt:

1. Start de server door de volgende opdrachten uit te voeren vanaf de projectmap:

   ```console
   npm install
   npm start
   ```

1. Ga naar **http://localhost:4200/** .
1. Selecteer **Aanmelden**.
1. Selecteer **Profiel** om Microsoft Graph aan te roepen.

Nadat de toepassing door de browser is geladen, selecteert u **Aanmelden**. De eerste keer dat u zich begint aan te melden, wordt u gevraagd om toestemming te geven zodat de toepassing uw profiel kan openen en u kan aanmelden. Nadat u bent aangemeld, selecteert u **Profiel** en worden de gegevens van uw gebruikersprofiel weergegeven op de pagina.

## <a name="how-the-sample-works"></a>Hoe het voorbeeld werkt

![Diagram dat laat zien hoe de voorbeeld-app in deze quickstart werkt](media/quickstart-v2-javascript/javascriptspa-intro.svg)


## <a name="next-steps"></a>Volgende stappen

Lees vervolgens hoe een gebruiker zich aanmeldt en tokens aanschaft in Angular-zelfstudie:

> [!div class="nextstepaction"]
> [Zelfstudies voor Angular](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-angular)

