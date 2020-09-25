---
title: Aanmelding met Microsoft toevoegen aan ASP.NET Core-web-apps | Azure
titleSuffix: Microsoft identity platform
description: Informatie over het implementeren van Microsoft-aanmelding in een ASP.NET Core-web-app met behulp van OpenID Connect
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/11/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: 1d31fc70aaf8449ed8bdafe4e290113e20865906
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902356"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Quickstart: aanmelding met Microsoft toevoegen aan een ASP.NET Core-web-app

In deze quickstart gebruikt u een codevoorbeeld om te leren hoe een ASP.NET Core-web-app persoonlijke accounts (hotmail.com, outlook.com, enz.) en werk- en schoolaccounts kan aanmelden vanuit een willekeurig exemplaar van Azure Active Directory (Azure AD). (Zie [Hoe het voorbeeld werkt](#how-the-sample-works) voor een illustratie.)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Ga naar de [Azure-portal - App-registraties](https://aka.ms/aspnetcore2-1-aad-quickstart-v2).
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
> 1. Als u toegang hebt tot meerdere tenants, gebruikt u het filter **Directory + abonnement** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: in het bovenste menu om de tenant te selecteren waarin u een toepassing wilt registreren.
> 1. Zoek en selecteer de optie **Azure Active Directory**.
> 1. Selecteer onder **Beheren** de optie **App-registraties** en vervolgens **Nieuwe registratie**.
> 1. Voer een **Naam** in voor de toepassing, bijvoorbeeld `AspNetCore-Quickstart`. Gebruikers van uw app kunnen de naam zien. U kunt deze later wijzigen.
> 1. Voer een **Omleidings-URI** van `https://localhost:44321/` in
> 1. Selecteer **Registreren**.
> 1. Selecteer **Verificatie** onder **Beheren**.
> 1. Selecteer onder **Omleidings-URI's** **Toevoegen URI** en voer vervolgens `https://localhost:44321/signin-oidc` in
> 1. Voer een **Logout-URL** van `https://localhost:44321/signout-oidc` in
> 1. Selecteer **id-tokens** onder **Impliciete toekenning**.
> 1. Selecteer **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
> Als u het codevoorbeeld uit deze snelstart wilt gebruiken, moet u antwoord-URL's toevoegen als `https://localhost:44321/` en `https://localhost:44321/signin-oidc`, voegt u de afmeldings-URL toe als `https://localhost:44321/signout-oidc` en vraagt u de id-tokens aan die moeten worden uitgegeven door het autorisatie-eindpunt.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-webapp/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-your-aspnet-core-project"></a>Stap 2: uw ASP.NET Core-project downloaden

> [!div renderon="docs"]
> [De ASP.NET Core-oplossing downloaden](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div renderon="portal" class="sxs-lookup"]
> Voer het project uit.

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Stap 3: Uw app is geconfigureerd en klaar om te worden uitgevoerd
> Uw project is geconfigureerd met waarden van de eigenschappen van uw app en is klaar om te worden uitgevoerd.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-aspnet-core-project"></a>Stap 3: uw ASP.NET Core-project configureren
> 1. Pak het zip-archief uit in een lokale map in de buurt van de hoofdmap van uw station. Bijvoorbeeld in *C:\Azure-Samples*.
> 1. Open de oplossing in Visual Studio.
> 1. Open het bestand *appSettings.json* en wijzig het volgende:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "common",
>    ```
>
>    - Vervang `Enter_the_Application_Id_here` met de **Toepassings(client)-ID** voor de toepassing die is geregistreerd in de Azure-portal. U vindt de **toepassings-id (client-id)** op de pagina **Overzicht** van de app.
>    - Vervang `common` door een van de volgende opties:
>       - Als uw toepassing **Accounts alleen in deze organisatiemap** ondersteunt, vervangt u deze waarde door de **Map (tenant)-ID**  (een GUID) of **tenantnaam** (bijvoorbeeld, `contoso.onmicrosoft.com`). U vindt de **Directory (Tenant)-ID** op de pagina **Overzicht**van de apps.
>       - Als uw toepassing **Accounts in elke organisatiemap** ondersteunt, vervang deze waarde dan door `organizations`
>       - Als uw toepassing **Alle Microsoft-accountgebruikers** ondersteunt, vervang deze waarde dan door `common`
>
> Wijzig voor deze Snelstart geen andere waarden in het bestand  *appSettings.json*.
>
> #### <a name="step-4-build-and-run-the-application"></a>Stap 4: de toepassing bouwen en uitvoeren
>
> Bouw en voer de app in Visual Studio uit door het menu **Foutopsporing** te selecteren > **Foutopsporing starten** of door op de `F5`-toets te drukken.
>
> U wordt gevraagd om uw referenties op te geven, vervolgens wordt u gevraagd om toestemming te geven voor de machtigingen die uw app nodig heeft. Selecteer **Accepteren** op de toestemmingprompt.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-01-consent.png" alt-text="Dialoogvenster Toestemming met de machtigingen die de app aanvraagt bij de > gebruiker":::
>
> Nadat u de gewenste machtigingen hebt opgegeven, wordt in de app weer gegeven dat u bent aangemeld met uw referenties voor Azure Active Directory.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-02-signed-in.png" alt-text="Webbrowser met de actieve web-app en de gebruiker die is aangemeld":::

## <a name="more-information"></a>Meer informatie

Deze sectie bevat een overzicht van de code die vereist is voor het aanmelden van gebruikers. Dit overzicht kan handig zijn om te begrijpen hoe de code werkt, wat de belangrijkste argumenten zijn en ook als u aanmelding wilt toevoegen aan een bestaande ASP.NET Core-toepassing.

### <a name="how-the-sample-works"></a>Hoe het voorbeeld werkt
![Toont hoe de voorbeeld-app werkt die is gegenereerd door deze quickstart](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Opstartklasse

De *Microsoft.AspNetCore.Authentication*-middleware maakt gebruik van een `Startup` klasse die wordt uitgevoerd wanneer in het hostingproces het volgende wordt geïnitialiseerd:

```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"));

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

Met de methode `AddAuthentication()` wordt de service geconfigureerd voor het toevoegen van verificatie op basis van cookies. Deze verificatie wordt gebruikt in browserscenario's en om vragen te sturen naar OpenID Connect.

Met de regel die `.AddMicrosoftIdentityWebApp` bevat, wordt het Microsoft-identiteitsplatform toegevoegd aan uw toepassing. Deze wordt vervolgens geconfigureerd om u aan te melden met het Microsoft Identity platform-eindpunt op basis van de informatie in de sectie `AzureAD` van het bestand *appSettings.json* :

| Sleutel *appSettings.json* | Description                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | **Toepassings(client)-ID** van de toepassing die is geregistreerd in de Azure-portal.                                                                                       |
| `Instance`             | STS-eindpunt (Security Token Service) voor de gebruiker te verifiëren. Deze waarde is doorgaans `https://login.microsoftonline.com/`, wat de openbare Azure-cloud aangeeft. |
| `TenantId`             | De naam van uw tenant of de tenant-ID (een GUID) of *algemene* om gebruikers met werk- of schoolaccounts of persoonlijke Microsoft-accounts aan te melden.                             |

De methode `Configure()` bevat twee belangrijke methoden, `app.UseCookiePolicy()` en `app.UseAuthentication()`, waarmee de benoemde functionaliteit kan worden ingeschakeld.

```csharp
// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // more code
    app.UseAuthentication();
    app.UseAuthorization();
    // more code
}
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Een controller of de methode van een controller beveiligen

U kunt een controller of controllermethoden beveiligen met behulp van het kenmerk `[Authorize]`. Met dit kenmerk wordt toegang tot de controller of methoden beperkt door alleen geverifieerde gebruikers toe te staan. Dit betekent dat de verificatiecontrole kan worden gestart om de controller te benaderen als de gebruiker niet is geverifieerd.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

De GitHub-opslagplaats die deze ASP.NET Core-handleiding bevat, bevatten instructies en meer codevoorbeelden die laten zien hoe u het volgende kunt doen:

- Verificatie toevoegen aan een nieuwe ASP.NET Core Web-toepassing
- Microsoft Graph, andere Microsoft-API's of uw eigen web-API's aanroepen
- Autorisatie toevoegen
- Gebruikers aanmelden bij nationale clouds of met sociale identiteiten

> [!div class="nextstepaction"]
> [ASP.NET Core Web-app-handleidingen op GitHub](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
