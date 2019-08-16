---
title: Azure AD B2C (micro soft Authentication Library voor .NET) | Azure
description: Meer informatie over specifieke overwegingen bij het gebruik van Azure AD B2C met de micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7444ecfd7a59224d0f08390385c508e4ecc40ddd
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532709"
---
# <a name="use-msalnet-to-sign-in-users-with-social-identities"></a>MSAL.NET gebruiken om gebruikers aan te melden met sociale identiteiten

U kunt MSAL.NET gebruiken om gebruikers met sociale identiteiten aan te melden met behulp van [Azure Active Directory B2C (Azure AD B2C)](https://aka.ms/aadb2c). Azure AD B2C is gebaseerd op het principe van beleid. In MSAL.NET, waarmee een beleid wordt gezet om een instantie te leveren.

- Wanneer u een instantie van de open bare client toepassing maakt, moet u het beleid in de instantie opgeven.
- Wanneer u een beleid wilt Toep assen, moet u een onderdrukking `AcquireTokenInteractive` met een `authority` para meter aanroepen.

Deze pagina is voor MSAL 3. x. Als u geïnteresseerd bent in MSAL 2. x, raadpleegt u [Azure AD B2C specifics in MSAL 2. x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-Specifics-MSAL-2.x).

## <a name="authority-for-a-azure-ad-b2c-tenant-and-policy"></a>Instantie voor een Azure AD B2C Tenant en beleid

De te gebruiken instantie is `https://login.microsoftonline.com/tfp/{tenant}/{policyName}` :

- `tenant`is de naam van de Azure AD B2C Tenant, 
- `policyName`de naam van het beleid dat moet worden toegepast (bijvoorbeeld ' b2c_1_susi ' voor aanmelden/aanmelden).

De huidige richt lijnen van Azure AD B2C moeten worden `b2clogin.com` gebruikt als autoriteit. Bijvoorbeeld `$"https://{your-tenant-name}.b2clogin.com/tfp/{your-tenant-ID}/{policyname}"`. Raadpleeg deze [documentatie](/azure/active-directory-b2c/b2clogin)voor meer informatie.

## <a name="instantiating-the-application"></a>De toepassing instantiëren

Wanneer u de toepassing bouwt, moet u de instantie opgeven.

```csharp
// Azure AD B2C Coordinates
public static string Tenant = "fabrikamb2c.onmicrosoft.com";
public static string ClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
public static string PolicySignUpSignIn = "b2c_1_susi";
public static string PolicyEditProfile = "b2c_1_edit_profile";
public static string PolicyResetPassword = "b2c_1_reset";

public static string AuthorityBase = $"https://fabrikamb2c.b2clogin.com/tfp/{Tenant}/";
public static string Authority = $"{AuthorityBase}{PolicySignUpSignIn}";
public static string AuthorityEditProfile = $"{AuthorityBase}{PolicyEditProfile}";
public static string AuthorityPasswordReset = $"{AuthorityBase}{PolicyResetPassword}";

application = PublicClientApplicationBuilder.Create(ClientID)
               .WithB2CAuthority(Authority)
               .Build();
```

## <a name="acquire-a-token-to-apply-a-policy"></a>Een token verkrijgen om een beleid toe te passen

Voor het verkrijgen van een token voor een Azure AD B2C beveiligde API in een open bare client toepassing moet u de onderdrukkingen gebruiken met een instantie:

```csharp
IEnumerable<IAccount> accounts = await application.GetAccountsAsync();
AuthenticationResult ar = await application .AcquireToken(scopes, parentWindow)
                                            .WithAccount(GetAccountByPolicy(accounts, policy))
                                            .ExecuteAsync();
```

door:

- `policy`een van de vorige teken reeksen (bijvoorbeeld `PolicySignUpSignIn`).
- `GetAccountByPolicy(IEnumerable<IAccount>, string)`is een methode die een account voor een bepaald beleid zoekt. Bijvoorbeeld:

  ```csharp
  private IAccount GetAccountByPolicy(IEnumerable<IAccount> accounts, string policy)
  {
   foreach (var account in accounts)
   {
    string userIdentifier = account.HomeAccountId.ObjectId.Split('.')[0];
    if (userIdentifier.EndsWith(policy.ToLower()))
     return account;
   }
   return null;
  }
  ```

Het Toep assen van een beleid (bijvoorbeeld zodat de eind gebruiker het profiel kan bewerken of het wacht woord opnieuw moet worden `AcquireTokenInteractive`ingesteld) wordt momenteel uitgevoerd door aan te roepen. In het geval van deze twee beleids regels gebruikt u niet het geretourneerde token/verificatie resultaat.

## <a name="special-case-of-editprofile-and-resetpassword-policies"></a>Speciaal geval van EditProfile-en ResetPassword-beleid

Als u een ervaring wilt bieden waarbij uw eind gebruikers zich aanmelden met een sociale identiteit en vervolgens het profiel bewerken waarop u het Azure AD B2C EditProfile-beleid wilt Toep assen. De manier om dit te doen, is door `AcquireTokenInteractive` de specifieke instantie voor dat beleid aan `Prompt.NoPrompt` te roepen en een prompt in te stellen om te voor komen dat het dialoog venster voor het selecteren van accounts wordt weer gegeven (omdat de gebruiker al is aangemeld)

```csharp
private async void EditProfileButton_Click(object sender, RoutedEventArgs e)
{
 IEnumerable<IAccount> accounts = await app.GetAccountsAsync();
 try
 {
  var authResult = await app.AcquireToken(scopes:App.ApiScopes)
                               .WithAccount(GetUserByPolicy(accounts, App.PolicyEditProfile)),
                               .WithPrompt(Prompt.NoPrompt),
                               .WithB2CAuthority(App.AuthorityEditProfile)
                               .ExecuteAsync();
  DisplayBasicTokenInfo(authResult);
 }
 catch
 {
  . . .
 }
}
```
## <a name="resource-owner-password-credentials-ropc-with-azure-ad-b2c"></a>Wachtwoord referenties van de resource-eigenaar (ROPC) met Azure AD B2C
Raadpleeg deze [documentatie](v2-oauth-ropc.md)voor meer informatie over de ROPC-stroom.

Deze stroom wordt **niet aanbevolen** omdat uw toepassing die een gebruiker vraagt om zijn of haar wacht woord niet veilig is. Zie [dit artikel](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/)voor meer informatie over dit probleem. 

Door gebruik te maken van gebruikers naam/wacht woord, geeft u een aantal dingen:
- kern tenants van moderne identiteit: wacht woord wordt gevist, opnieuw afgespeeld. Omdat we dit concept hebben van een share geheim dat kan worden onderschept. Dit is niet compatibel met een wacht woord.
- Gebruikers die MFA nodig hebben, kunnen zich niet aanmelden (omdat er geen interactie is).
- Gebruikers kunnen eenmalige aanmelding niet uitvoeren.

### <a name="configure-the-ropc-flow-in-azure-ad-b2c"></a>De ROPC-stroom configureren in Azure AD B2C
Maak in uw Azure AD B2C-Tenant een nieuwe gebruikers stroom en selecteer **Aanmelden met behulp van ROPC**. Hiermee wordt het ROPC-beleid voor uw Tenant ingeschakeld. Zie [de gegevens stroom voor het wacht woord voor de resource-eigenaar configureren](/azure/active-directory-b2c/configure-ropc) voor meer informatie.

`IPublicClientApplication`bevat een methode:
```csharp
AcquireTokenByUsernamePassword(
            IEnumerable<string> scopes,
            string username,
            SecureString password)
```

Deze methode neemt de volgende para meters:
- De *bereiken* voor het aanvragen van een toegangs token voor.
- Een *gebruikers naam*.
- Een SecureString- *wacht woord* voor de gebruiker.

Vergeet niet om de instantie te gebruiken die het ROPC-beleid bevat.

### <a name="limitations-of-the-ropc-flow"></a>Beperkingen van de ROPC-stroom
 - De stroom ROPC **werkt alleen voor lokale accounts** (waarbij u zich registreert bij Azure AD B2C met een e-mail adres of gebruikers naam). Deze stroom werkt niet als federeren naar een van de id-providers die worden ondersteund door Azure AD B2C (Facebook, Google, enzovoort).
 - Er is momenteel **geen id_token geretourneerd door Azure AD B2C** bij het implementeren van de ROPC-stroom van MSAL. Dit betekent dat er geen account object kan worden gemaakt, dus in de cache is er geen account en geen gebruiker. De AcquireTokenSilent-stroom werkt niet in dit scenario. ROPC geeft echter geen gebruikers interface weer, waardoor er geen gevolgen zijn voor de gebruikers ervaring.

## <a name="google-auth-and-embedded-webview"></a>Google auth en embedded webweergave

Als u een Azure AD B2C-ontwikkelaar bent die Google als id-provider gebruikt, wordt de opdracht van de systeem browser gebruikt, aangezien Google geen [authenticatie van Inge sloten webweergave](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html)toestaat. `login.microsoftonline.com` Is momenteel een vertrouwde instantie met Google. Het gebruik van deze instantie werkt met Inge sloten webweergave. Het gebruik `b2clogin.com` van is echter geen vertrouwde instantie met Google, zodat gebruikers niet kunnen worden geauthenticeerd.

We bieden een update voor de wiki en dit [probleem](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/688) als er iets wordt gewijzigd.

## <a name="caching-with-azure-ad-b2c-in-msalnet"></a>Caching met Azure AD B2C in MSAL.Net 

### <a name="known-issue-with-azure-ad-b2c"></a>Bekend probleem met Azure AD B2C

MSAL.Net ondersteunt een [token cache](/dotnet/api/microsoft.identity.client.tokencache?view=azure-dotnet). De cache sleutel voor tokens is gebaseerd op de claims die worden geretourneerd door de ID-provider. Momenteel heeft MSAL.Net twee claims nodig om een token cache sleutel te bouwen:  
- `tid`Dit is de Azure AD-Tenant-ID en 
- `preferred_username` 

Beide claims ontbreken in veel van de Azure AD B2C scenario's. 

Het effect van de klant is dat wanneer u het gebruikers naam veld probeert weer te geven, u de waarde ' ontbreekt in de token reactie ' krijgt Als dit het geval is, is dit omdat Azure AD B2C geen waarde retourneert in de IdToken voor de preferred_username wegens beperkingen met betrekking tot sociale accounts en externe ID-providers (id). Azure AD retourneert een waarde voor preferred_username omdat u weet wie de gebruiker is, maar voor Azure AD B2C, omdat de gebruiker zich kan aanmelden met een lokaal account, Facebook, Google, GitHub, enzovoort. er is geen consistente waarde voor Azure AD B2C voor preferred_username. Voor het deblokkeren van de MSAL van de compatibiliteit met ADAL, hebben we besloten om ' ontbreekt te gebruiken in het token antwoord ' aan het einde van het verwerken van de Azure AD B2C-accounts wanneer de IdToken Nothing retourneert voor preferred_username. MSAL moet een waarde Retour neren voor preferred_username voor het onderhouden van de cache compatibiliteit tussen bibliotheken.

### <a name="workarounds"></a>Tijdelijke oplossingen

#### <a name="mitigation-for-the-missing-tenant-id"></a>Beperking voor de ontbrekende Tenant-ID

De voorgestelde tijdelijke oplossing is het gebruik [van de cache op basis van beleid](#acquire-a-token-to-apply-a-policy)

U kunt de `tid` claim ook gebruiken als u het [aangepaste B2C-beleid](https://aka.ms/ief)gebruikt, omdat het de mogelijkheid biedt om aanvullende claims naar de toepassing te retour neren. Meer informatie over [claim transformatie](/azure/active-directory-b2c/claims-transformation-technical-profile)

#### <a name="mitigation-for-missing-from-the-token-response"></a>Risico beperking voor ' ontbreekt in het token antwoord '
U kunt ook de claim ' naam ' gebruiken als voorkeurs gebruikersnaam. Het proces wordt vermeld in deze [B2C doc](../../active-directory-b2c/active-directory-b2c-reference-policies.md) -> ' Kies in de kolom retour claim de claims die u wilt retour neren in de autorisatie tokens die worden teruggestuurd naar uw toepassing nadat de bewerking is geslaagd voor het bewerken van een profiel. Selecteer bijvoorbeeld weergave naam, post code.

## <a name="next-steps"></a>Volgende stappen 

Meer informatie over het interactief verkrijgen van tokens met MSAL.NET voor Azure AD B2C-toepassingen vindt u in het volgende voor beeld.

| Voorbeeld | Platform | Description|
|------ | -------- | -----------|
|[active-directory-b2c-xamarin-native](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) | Xamarin iOS, Xamarin Android, UWP | Een eenvoudige Xamarin Forms-app die laat zien hoe u MSAL.NET kunt gebruiken om gebruikers te verifiëren via Azure AD B2C en toegang te krijgen tot een web-API met de resulterende tokens.|
