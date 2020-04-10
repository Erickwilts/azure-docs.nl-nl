---
title: Microsoft-identiteitsplatform Windows UWP snelstart | Azure
description: Ontdek hoe een XAML-toepassing (Universal Windows Platform) een access token kan krijgen en een API kan aanroepen die is beschermd door het eindpunt van microsoft-identiteitsplatform.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/12/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:UWP
ms.openlocfilehash: feb1fa82653a0db7b3041a4f745d0563c220bd31
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991089"
---
# <a name="quickstart-call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Quickstart: De Microsoft Graph-API aanroepen vanuit een UWP-toepassing (Universeel Windows-platform)

Deze quickstart bevat een codevoorbeeld dat laat zien hoe een UWP-toepassing (Universal Windows Platform) gebruikers kan aanmelden met persoonlijke accounts of werk- en schoolaccounts, een toegangstoken kan krijgen en de Microsoft Graph API kan aanroepen. (Zie [hoe het voorbeeld werkt](#how-the-sample-works) voor een illustratie.)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De snelstart-app registreren en downloaden
> [!div renderon="docs" class="sxs-lookup"]
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens de voorbeeldcode](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: de app registreren en automatisch configureren, en vervolgens de voorbeeldcode downloaden
>
> 1. Ga naar de nieuwe [Azure-portal - deelvenster App-registraties.](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/UwpQuickstartPage/sourceType/docs)
> 1. Voer een naam in voor de toepassing en klik op **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app toe te voegen aan de oplossing:
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com) met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Navigeer naar de pagina Microsoft-identiteitsplatform voor ontwikkelaars [App-registraties.](https://aka.ms/MobileAppReg)
> 1. Selecteer **Nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **Naam** een beschrijvende toepassingsnaam in die zichtbaar is voor gebruikers van de app. Bijvoorbeeld: `UWP-App-calling-MsGraph`.
>      - Selecteer in de sectie **Ondersteunde accounttypen** de optie **Accounts in alle organisatiemappen en persoonlijke Microsoft-accounts (bijvoorbeeld Skype, Xbox, Outlook.com**.
>      - Selecteer **Registreren** om de toepassing te maken.
> 1. Selecteer in de lijst met pagina’s voor de app de optie **Verificatie**.
> 1. Schakel in de sectie **URL's** | die worden voorgesteld om te leiden**naar URL's voor openbare clients (mobiel, desktop)** de controle **https://login.microsoftonline.com/common/oauth2/nativeclient**.
> 1. Selecteer **Opslaan**.

> [!div renderon="portal" class="sxs-lookup"]
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor het codevoorbeeld voor deze quickstart moet u een **https://login.microsoftonline.com/common/oauth2/nativeclient**omleidingsURI toevoegen als .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-uwp/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-your-visual-studio-project"></a>Stap 2: Uw Visual Studio-project downloaden

> [!div renderon="docs"]
> [Download het Visual Studio-project](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Voer het project uit met Visual Studio 2019.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Stap 3: Uw app is geconfigureerd en klaar om uit te voeren
> We hebben uw project geconfigureerd met waarden van de eigenschappen van uw app en het is klaar om te worden uitgevoerd. 

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-visual-studio-project"></a>Stap 3: Uw Visual Studio-project configureren
> 
> 1. Pak het zip-bestand uit in een lokale map dicht bij de hoofdmap van de schijf, bijvoorbeeld **C:\Azure-Samples**.
> 1. Open het project in Visual Studio. Mogelijk wordt u gevraagd een UWP SDK te installeren. In dat geval, accepteren.
> 1. Bewerk **MainPage.Xaml.cs** en vervang `ClientId` de waarden van het veld:
>
>    ```csharp
>    private const string ClientId = "Enter_the_Application_Id_here";
>    ```
> Waar:
> - `Enter_the_Application_Id_here`: de toepassings-id voor de toepassing die u hebt geregistreerd.
>
> > [!TIP]
> > Als u de waarde van *toepassings-id wilt*vinden, gaat u naar de sectie **Overzicht** in de portal

#### <a name="step-4-run-your-application"></a>Stap 4: Uw toepassing uitvoeren

Als u de quickstart op uw Windows-machine wilt proberen:

1. Kies op de werkbalk Visual Studio het juiste platform (waarschijnlijk **x64** of **x86**, niet ARM). U zult merken dat het doelapparaat verandert van *apparaat* naar *lokale machine*
1. Foutopsporing selecteren | **Zonder foutopsporing starten**

## <a name="more-information"></a>Meer informatie

Deze sectie biedt meer informatie over de quickstart.

### <a name="how-the-sample-works"></a>Hoe het voorbeeld werkt
![Laat zien hoe de voorbeeld-app die door deze quickstart wordt gegenereerd, werkt](media/quickstart-v2-uwp/uwp-intro.svg)

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) is de bibliotheek die wordt gebruikt om gebruikers aan te melden en beveiligingstokens aan te vragen. De beveiligingstokens worden gebruikt om toegang te krijgen tot een API die is beschermd door het Microsoft Identity-platform voor ontwikkelaars. U kunt MSAL installeren door de volgende opdracht uit te voeren in *Package Manager Console* van Visual Studio:

```powershell
Install-Package Microsoft.Identity.Client
```

### <a name="msal-initialization"></a>MSAL initialiseren

U kunt de verwijzing voor MSAL toevoegen door de volgende code toe te voegen:

```csharp
using Microsoft.Identity.Client;
```

Vervolgens wordt MSAL geïnitialiseerd met behulp van de volgende code:

```csharp
public static IPublicClientApplication PublicClientApp;
PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                                                .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
                                                    .Build();
```

> |Waar: ||
> |---------|---------|
> | `ClientId` | Is de **Toepassings-id (client-id)** voor de toepassing die is geregistreerd in de Azure-portal. U vindt deze waarde op de pagina **Overzicht** in de Azure-portal. |

### <a name="requesting-tokens"></a>Tokens aanvragen

MSAL heeft twee methoden voor het verkrijgen van `AcquireTokenInteractive` `AcquireTokenSilent`tokens in een UWP-app: en .

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

In sommige situaties moeten gebruikers worden gedwongen om via een pop-upvenster te communiceren met het eindpunt van het Microsoft-identiteitsplatform om hun referenties te valideren of toestemming te geven. Voorbeelden zijn:

- De eerste keer dat gebruikers zich aanmelden bij de toepassing
- Wanneer gebruikers mogelijk hun referenties opnieuw moeten opgeven omdat het wachtwoord is verlopen
- Wanneer uw toepassing toegang vraagt tot een bron, moet de gebruiker toestemming geven
- Wanneer tweeledige verificatie is vereist

```csharp
authResult = await App.PublicClientApp.AcquireTokenInteractive(scopes)
                      .ExecuteAsync();
```

> |Waar:||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd, bijvoorbeeld `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's. |

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Gebruik `AcquireTokenSilent` de methode om tokens te verkrijgen `AcquireTokenInteractive` om toegang te krijgen tot beveiligde bronnen na de eerste methode. U wilt niet dat de gebruiker zijn referenties elke keer dat hij of zij toegang nodig heeft tot een bron, valideert. Meestal wilt u token-acquisities en -verlenging zonder enige interactie van de gebruiker

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
var firstAccount = accounts.FirstOrDefault();
authResult = await App.PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                      .ExecuteAsync();
```

> |Waar: ||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd, bijvoorbeeld `{ "user.read" }` voor Microsoft Graph of `{ "api://<Application ID>/access_as_user" }` voor aangepaste web-API's |
> | `firstAccount` | Hiermee geeft u het eerste gebruikersaccount in de cache op (MSAL ondersteunt meerdere gebruikers in één app) |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Volgende stappen

Volg de zelfstudie voor Windows-bureaublad voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen en nieuwe functies, waaronder een volledige uitleg van deze quickstart.

> [!div class="nextstepaction"]
> [UWP - Zelfstudie voor het aanroepen van Graph API](tutorial-v2-windows-uwp.md)
