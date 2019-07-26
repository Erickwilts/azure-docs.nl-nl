---
title: Mobiele app die web-Api's aanroept (code configuratie)-micro soft Identity-platform | Azure
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (de code configuratie van de app)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bdf9210eb88b2057cf861b208f19d3e6f562e9a
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414848"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Mobiele app die web-Api's aanroept-code configuratie

Zodra u de toepassing hebt gemaakt, leert u hoe u de code kunt configureren op basis van de para meters van de toepassing die u tijdens de app-registratie hebt gekregen. Mobiele toepassingen hebben ook enkele complexe specifiek, die te maken hebben met de aanpassing in het Framework voor het bouwen van deze apps

## <a name="msal-libraries-supporting-mobile-apps"></a>MSAL-bibliotheken die mobiele apps ondersteunen

De micro soft-bibliotheken die mobiele apps ondersteunen zijn:

  MSAL-bibliotheek | Description
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Voor het ontwikkelen van draag bare toepassingen. MSAL.NET ondersteunde platforms voor het bouwen van een mobiele toepassing zijn UWP, Xamarin. iOS en Xamarin. Android.
  ![MSAL. iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL. iOS | Systeem eigen iOS-toepassingen ontwikkelen met objectieve C of SWIFT
  ![MSAL. Android](media/sample-v2-code/logo_android.png) <br/> MSAL. Android | Systeem eigen Android-toepassingen ontwikkelen in Java voor Android

## <a name="configuring-the-application"></a>De toepassing configureren

Mobiele toepassingen gebruiken de MSAL `PublicClientApplication` -klasse. Hier kunt u een exemplaar maken:

### <a name="android"></a>Android

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

```swift
// Initialize the app.
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
}
```

### <a name="xamarin-or-uwp"></a>Xamarin of UWP

In de volgende alinea wordt uitgelegd hoe u de code van de toepassing configureert voor Xamarin. iOS-, Xamarin. Android-en UWP-apps. De eerste stap is het instantiëren van de toepassing. Een optionele stap is het configureren van de Broker.

#### <a name="instantiating-the-application"></a>De toepassing instantiëren

In Xamarin, of UWP, is de eenvoudigste manier om de toepassing te instantiëren, waarbij de `ClientId` de GUID van uw toepassing is die is geregistreerd.

```CSharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

Er zijn aanvullende*para meters* voor het instellen van de gebruikers interface, het vervangen van de standaard instantie, het opgeven van een client naam en-versie (voor telemetrie), het opgeven van een omleidings-URI, de http-Factory opgeven die moet worden gebruikt (bijvoorbeeld voor het afhandelen van proxy's, opgeven telemetrie en logboek registratie). Dit is het onderwerp van de volgende alinea's.

##### <a name="specifying-the-parent-uiwindowactivity"></a>De bovenliggende UI/Window/activiteit opgeven

Op Android moet u de bovenliggende activiteit door geven voordat u interactieve verificatie doet. Wanneer u in iOS een Broker gebruikt, moet u de View Controller door geven. Op dezelfde manier kunt u in het bovenliggende venster het beste door geven aan UWP. Dit is mogelijk wanneer u het token aanschaft, maar het is ook mogelijk om een call back op te geven tijdens het maken van de app. een gemachtigde retourneert de UIParent.

```CSharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

In Android raden wij u `CurrentActivityPlugin` [hier](https://github.com/jamesmontemagno/CurrentActivityPlugin)aan.  De code `PublicClientApplication` van de opbouw functie ziet er als volgt uit:

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>Meer para meters voor het bouwen van apps

- Zie de naslag documentatie [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods) voor de lijst met `PublicClientApplicationBuilder`alle wijzigingen die beschikbaar zijn op.
- Voor de beschrijving van alle opties die beschikbaar zijn `PublicClientApplicationOptions` in Zie [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions), in de referentie documentatie

#### <a name="xamarin-ios-specific-considerations"></a>Specifieke overwegingen voor Xamarin iOS

Op Xamarin iOS zijn er verschillende overwegingen die u moet meenemen bij het gebruik van MSAL.NET:

1. [De `OpenUrl` functie vervangen en implementeren in de`AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Sleutel hanger groepen inschakelen](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Het delen van token cache inschakelen](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Sleutel hanger toegang inschakelen](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Details vindt u in [Xamarin IOS-overwegingen](msal-net-xamarin-ios-considerations.md)

#### <a name="other-xamarin-android-specific-considerations"></a>Andere Xamarin Android-specifieke overwegingen

Hier zijn Xamarin Android-specifiek:

- [Controleren of het besturings element terugkeert naar MSAL zodra het interactieve deel van de verificatie stroom is beëindigd](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Het Android-manifest bijwerken](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [De Inge sloten webweergave gebruiken (optioneel)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Problemen oplossen](msal-net-xamarin-android-considerations.md#troubleshooting)

Details vindt u in [Xamarin Android-overwegingen](msal-net-xamarin-android-considerations.md)

Ten slotte zijn er enkele specifieke kenmerken die u moet weten over de browsers op Android. Deze worden beschreven in [Xamarin Android-specifieke overwegingen met MSAL.net](msal-net-system-browser-android-considerations.md)

#### <a name="uwp-specific-considerations"></a>Specifieke aandachtspunten voor UWP

Op UWP kunt u bedrijfs netwerken gebruiken. Zie [universeel Windows-platform-specifieke overwegingen met MSAL.net](msal-net-uwp-considerations.md)voor meer informatie over de UWP-specifiek.

## <a name="configuring-the-application-to-use-the-broker"></a>De toepassing configureren voor het gebruik van de Broker

### <a name="why-use-brokers-on-xamarinios-and-xamarinandroid-applications"></a>Waarom gebruik ik Brokers voor Xamarin. iOS en Xamarin. Android-toepassingen?

Op Android en iOS bieden makelaars de volgende opties:

- Eenmalige aanmelding (SSO). Uw gebruikers hoeven zich niet aan te melden bij elke toepassing.
- Apparaat-id. Hiermee wordt het aan Azure AD-apparaat gekoppelde beleid voor voorwaardelijke toegang ingeschakeld door het certificaat van het apparaat te openen dat is gemaakt op het apparaat toen het was gekoppeld aan de werk plek.
- Verificatie van de toepassings-id. Wanneer een toepassing de Broker aanroept, wordt de omleidings-URL door gegeven en wordt deze door de Broker gecontroleerd.

### <a name="enable-the-brokers-on-xamarin"></a>De makelaars inschakelen op Xamarin

Als u een van deze functies wilt inschakelen, `WithBroker()` gebruikt u de para `PublicClientApplicationBuilder.CreateApplication` meter bij het aanroepen van de-methode. `.WithBroker()`is standaard ingesteld op True. Volg de onderstaande stappen voor [IOS](#brokered-authentication-for-xamarinios).

### <a name="brokered-authentication-for-xamarinios"></a>Brokered authenticatie voor Xamarin. iOS

Volg de onderstaande stappen om uw Xamarin. iOS-app in te scha kelen voor communicatie met de app [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458) .

#### <a name="step-1-enable-broker-support"></a>Stap 1: Ondersteuning voor Broker inschakelen

Broker-`PublicClientApplication` ondersteuning is per basis ingeschakeld. Het is standaard uitgeschakeld. U moet de `WithBroker()` para meter (standaard ingesteld op True) gebruiken bij het maken `PublicClientApplication` van de `PublicClientApplicationBuilder`via.

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Stap 2: AppDelegate bijwerken voor het afhandelen van de call back

Wanneer MSAL.net de Broker aanroept, roept de Broker op zijn beurt terug naar uw toepassing via de `AppDelegate.OpenUrl` -methode. Omdat MSAL wacht op het antwoord van de Broker, moet uw toepassing samen werken om MSAL.NET terug aan te roepen. U kunt dit doen door het `AppDelegate.cs` bestand bij te werken om de onderstaande methode te negeren.

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url,
                             string sourceApplication,
                             NSObject annotation)
{
 if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
 {
  AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
  return true;
 }
 else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
 {
  return false;
 }
 return true;
}
```

Deze methode wordt aangeroepen telkens wanneer de toepassing wordt gestart en wordt gebruikt als een kans om de reactie van de Broker te verwerken en het verificatie proces te volt ooien dat door MSAL.NET wordt gestart.

#### <a name="step-3-set-a-uiviewcontroller"></a>Stap 3: Een UIViewController instellen ()

Met Xamarin iOS hoeft u normaal gesp roken geen object venster in te stellen, maar in dit geval kunt u antwoorden van een Broker verzenden en ontvangen. Stel nog `AppDelegate.cs`steeds een view controller in.

Ga als volgt te werk om het object venster in te stellen:

1) Stel `AppDelegate.cs`in de `App.RootViewController` in op een nieuw `UIViewController()`. Dit zorgt ervoor dat er een `UIViewController` is met de aanroep van de Broker. Als deze niet correct is ingesteld, wordt deze fout mogelijk weer geven:`"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) Gebruik in de AcquireTokenInteractive-aanroep de `.WithParentActivityOrWindow(App.RootViewController)` en geef deze door in de verwijzing naar het object venster dat u gaat gebruiken.

**Bijvoorbeeld:**

In `App.cs`:
```CSharp
   public static object RootViewController { get; set; }
```
In `AppDelegate.cs`:
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
In de aanroep voor het verkrijgen van een token:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

#### <a name="step-4-register-a-url-scheme"></a>Stap 4: Een URL-schema registreren

MSAL.NET maakt gebruik van Url's om de Broker aan te roepen en retour neren het Broker-antwoord terug naar uw app. Als u de retour ronding wilt volt ooien, moet u een URL-schema voor uw `Info.plist` app registreren in het bestand.

`msauth`Voor voegsel `CFBundleURLSchemes` van met. Voeg `CFBundleURLName` vervolgens toe aan het einde.

`$"msauth.(BundleId)"`

**Bijvoorbeeld:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Dit URL-schema wordt onderdeel van de RedirectUri die wordt gebruikt voor het uniek identificeren van uw app bij het ontvangen van het antwoord van de Broker.

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

#### <a name="step-5-lsapplicationqueriesschemes"></a>Stap 5: LSApplicationQueriesSchemes

MSAL wordt `–canOpenURL:` gebruikt om te controleren of de Broker op het apparaat is geïnstalleerd. In iOS 9 zijn de schema's vergrendeld waarvan een toepassing kan zoeken.

**Toevoegen** naar de `LSApplicationQueriesSchemes` sectie van het `Info.plist` bestand. **`msauthv2`**

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Brokered-verificatie voor Xamarin. Android

MSAL.NET biedt nog geen ondersteuning voor Brokers voor Android.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een token verkrijgen](scenario-mobile-acquire-token.md)
