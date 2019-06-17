---
title: Azure Active Directory - verificatie met Azure App Service configureren
description: Informatie over het configureren van Azure Active Directory-verificatie voor uw App Services-toepassing.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/20/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: d687e770fae6c32ee351a597e12d1aca6094e5cb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60851372"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>Uw App Service-app voor het gebruik van Azure Active Directory-aanmelding configureren

> [!NOTE]
> Op dit moment, AAD V2 (met inbegrip van MSAL) wordt niet ondersteund voor Azure App Services en Azure Functions. Controleer binnenkort voor updates.
>

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit artikel leest u hoe het configureren van Azure App Services voor het gebruik van Azure Active Directory als verificatieprovider.

## <a name="express"> </a>Met express-instellingen configureren

1. In de [Azure Portal], gaat u naar uw App Service-app. Selecteer in het linkernavigatievenster **verificatie / autorisatie**.
2. Als **verificatie / autorisatie** niet is ingeschakeld, selecteert **op**.
3. Selecteer **Azure Active Directory**, en selecteer vervolgens **Express** onder **beheermodus**.
4. Selecteer **OK** voor het registreren van de App Service-app in Azure Active Directory. Hiermee maakt u een nieuwe app-registratie. Als u een bestaande app-registratie in plaats daarvan kiezen wilt, klikt u op **selecteert u een bestaande app** en zoek vervolgens de naam van een eerder gemaakte app-registratie binnen uw tenant.
   Klik op de app-registratie te selecteren en klik op **OK**. Klik vervolgens op **OK** op de pagina voor Azure Active Directory-instellingen.
   Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code.
5. (Optioneel) Instellen om toegang te beperken naar uw site tot alleen gebruikers die zijn geverifieerd door Azure Active Directory, **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **aanmelden met Azure Active Directory**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
6. Klik op **Opslaan**.

## <a name="advanced"> </a>Met de geavanceerde instellingen configureren

U kunt ook opgeven de configuratie-instellingen handmatig. Dit is de beste oplossing als de Azure Active Directory-tenant die u wilt gebruiken, wijkt af van de tenant waarmee u zich bij Azure aanmeldt. De configuratie te voltooien, moet u eerst een registratie maken in Azure Active Directory en vervolgens moet u enkele van de registratiedetails van de naar App Service.

### <a name="register"> </a>Uw App Service-app registreren bij Azure Active Directory

1. Aanmelden bij de [Azure Portal], en Ga naar de App Service-app. Uw app kopiëren **URL**. U gebruikt deze om te configureren van de registratie van uw Azure Active Directory-app.
2. Navigeer naar **Active Directory**en selecteer vervolgens de **App-registraties**, klikt u vervolgens op **nieuwe toepassing registreren** boven aan het starten van een nieuwe app-registratie. 
3. In de **maken** pagina een **naam** voor uw app-registratie, selecteert u de **Web-App / API** Instellingstype, in de **aanmeldings-URL** vak plakken de de URL van de toepassing (uit stap 1). Klik vervolgens op **maken**.
4. In een paar seconden ziet u de nieuwe app-registratie die u zojuist hebt gemaakt.
5. Zodra de app-registratie is toegevoegd, klikt u op de naam van de app-registratie, klikt u op **instellingen** aan de bovenkant en klik vervolgens op **eigenschappen** 
6. In de **App ID URI** vak, plak de URL van de toepassing (uit stap 1), ook in de **URL van startpagina** plakken in de URL van de toepassing (uit stap 1), en klik vervolgens op **opslaan**
7. Klik nu op de **antwoord-URL's**, bewerk de **antwoord-URL**, plak de URL van de toepassing (uit stap 1) en voegt deze toe aan het einde van de URL */.auth/login/aad/callback* (voor bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`). Klik op **Opslaan**.

   > [!NOTE]
   > U kunt de dezelfde app-registratie voor meerdere domeinen gebruiken door toe te voegen extra **antwoord-URL's**. Zorg ervoor dat als model voor elke App Service-exemplaar met een eigen registratie, zodat er een eigen machtigingen en toestemming. Ook overwegen om afzonderlijke app-registraties voor afzonderlijke sitesleuven. Dit is om te voorkomen dat machtigingen worden gedeeld tussen omgevingen, zodat een bug in nieuwe code die u wilt testen heeft geen invloed op de productie.
    
8. Op dit moment, Kopieer de **toepassings-ID** voor de app. Houd het voor later gebruik. U moet het configureren van uw App Service-app.
9. Sluit de **geregistreerde app** pagina. Op de **App-registraties** pagina, klikt u op de **eindpunten** knop aan de bovenkant en kopieer de **WS-FEDERATION SIGN-ON ENDPOINT** URL, maar Verwijder de `/wsfed` beëindigen via de URL. Het eindresultaat ziet er als `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000`. Naam van het domein kan mogelijk verschillen voor een onafhankelijke Clouds. Dit moet fungeren als de URL-verlener voor later.

### <a name="secrets"> </a>Azure Active Directory-informatie toevoegen aan uw App Service-app

1. Klik in de [Azure Portal], gaat u naar uw App Service-app. Klik op **verificatie/autorisatie**. Als de verificatie/autorisatie-functie niet is ingeschakeld, zet u de switch op **op**. Klik op **Azure Active Directory**, onder Authentication-Providers, uw app configureren.

    (Optioneel) Standaard is App Service-verificatie biedt, maar biedt geautoriseerde toegang tot uw API's en site-inhoud niet beperken. U moet autoriseren van gebruikers in uw app-code. Stel **te ondernemen actie wanneer de aanvraag niet is geverifieerd** naar **aanmelden met Azure Active Directory**. Deze optie vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.
2. Klik in de configuratie van Active Directory-verificatie, op **Geavanceerd** onder **beheermodus**. Plak de toepassings-ID in het Client-ID (uit stap 8) en plak de URL-verlener-waarde in de URL (uit stap 9). Klik vervolgens op **OK**.
3. Klik op de pagina van de configuratie van Active Directory-verificatie **opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw App Service-app.

## <a name="configure-a-native-client-application"></a>Een systeemeigen clienttoepassing configureren
U kunt registreren systeemeigen clients waarmee u meer controle over de machtigingen toewijzen. U hebt deze nodig als u wilt uitvoeren van aanmeldingen met behulp van een clientbibliotheek, zoals de **Active Directory Authentication Library**.

1. Navigeer naar **Azure Active Directory** in de [Azure Portal].
2. Selecteer in het linkernavigatievenster **App-registraties**. Klik op **nieuwe app-registratie** aan de bovenkant.
4. In de **maken** pagina een **naam** voor uw app-registratie. Selecteer **systeemeigen** in **toepassingstype**.
5. In de **omleidings-URI** voert u uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet zijn vergelijkbaar met *https://contoso.azurewebsites.net/.auth/login/done* . Als het maken van een Windows-toepassing, in plaats daarvan gebruikt de [pakket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als de URI.
5. Klik op **Create**.
6. Zodra de app-registratie is toegevoegd, selecteert u deze om dit te openen. Zoek de **toepassings-ID** en noteer deze waarde.
7. Klik op **alle instellingen** > **vereiste machtigingen** > **toevoegen** > **Select an API**.
8. Typ de naam van de App Service-app die u eerder hebt geregistreerd, om te zoeken en vervolgens selecteert u deze en klikt u op **Selecteer**.
9. Selecteer **toegang \<app_name >** . Klik vervolgens op **Selecteren**. Klik vervolgens op **Gereed**.

U hebt nu een systeemeigen clienttoepassing die krijgen uw App Service-app tot toegang geconfigureerd.

## <a name="related-content"> </a>Gerelateerde inhoud

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[alternative method]:#advanced
