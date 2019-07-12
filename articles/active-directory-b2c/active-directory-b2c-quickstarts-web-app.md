---
title: 'Snelstart: aanmelden voor een ASP.NET-toepassing met behulp van Azure Active Directory B2C instellen | Microsoft Docs'
description: Voer een voorbeeld-ASP.NET-web-app uit die gebruikmaakt van Azure Active Directory B2C voor aanmelden bij een account.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 647ea3bdeb914b97fe131d32078ddb610d4d163e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835483"
---
# <a name="quickstart-set-up-sign-in-for-an-aspnet-application-using-azure-active-directory-b2c"></a>Quickstart: Aanmelden voor een ASP.NET-toepassing met behulp van Azure Active Directory B2C instellen

Azure Active Directory (Azure AD) B2C bevat functionaliteit voor identiteitsbeheer in de cloud ter bescherming van uw toepassing, bedrijf en klanten. Met Azure AD B2C kunnen uw toepassingen zich met behulp van open-standaardprotocollen verifiëren bij sociale en enterpriseaccounts. In deze snelstart gebruikt u een ASP.NET-toepassing. Deze toepassing wordt gebruikt voor het aanmelden via een id-provider voor sociale netwerken en voor het aanroepen van een met Azure AD B2C beveiligde web-API.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) met de **ASP.NET en webontwikkeling** werkbelasting.
- Een sociaalnetwerkaccount van Facebook, Google, Microsoft of Twitter.
- [Download een ZIP-bestand ](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) of kloon de voorbeeld-web-app vanuit GitHub.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
    ```

    Deze twee projecten bevinden zich in de voorbeeldoplossing:

    - **TaskWebApp**: een webtoepassing die een takenlijst maakt en bewerkt. De webtoepassing maakt gebruik van de **registreren of aanmelden** gebruikersstroom registreren of aanmelden van gebruikers.
    - **TaskService**: een web-API die ondersteuning biedt voor functionaliteit voor het maken, lezen, bijwerken en verwijderen van takenlijsten. De web-API wordt beveiligd door Azure AD B2C en wordt aangeroepen door de web-app.

## <a name="run-the-application-in-visual-studio"></a>De toepassing uitvoeren in Visual Studio

1. In de projectmap van de voorbeeldtoepassing opent u de oplossing **B2C-WebAPI-DotNet.sln** in Visual Studio.
2. Voor deze snelstart voert u de projecten **TaskWebApp** en **TaskService** tegelijk uit. Klik in Solution Explorer met de rechtermuisknop op de oplossing **B2C-WebAPI-DotNet** en selecteer vervolgens **Opstartprojecten instellen**.
3. Selecteer **Meerdere opstartprojecten** en wijzig de **Actie** voor beide projecten in **Start**.
4. Klik op **OK**.
5. Druk op **F5** om fouten in beide toepassingen op te sporen. Elke toepassing wordt geopend op een eigen browsertabblad:

    - `https://localhost:44316/`: de ASP.NET-webtoepassing. U communiceert rechtstreeks met deze toepassing in de snelstart.
    - `https://localhost:44332/`: de web-API die wordt aangeroepen door de ASP.NET-webtoepassing.

## <a name="sign-in-using-your-account"></a>Aanmelden met uw account

1. Klik in de ASP.NET-webtoepassing op **Registreren/Aanmelden** om de werkstroom te starten.

    ![Voorbeeld-ASP.NET WebApp in de browser met het teken omhoog/aanmelding koppeling gemarkeerd](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

    In de voorbeeldtoepassing worden verschillende registratiemogelijkheden ondersteund. U kunt bijvoorbeeld een id-provider voor sociale netwerken gebruiken of een lokaal account maken met behulp van een e-mailadres. Voor deze snelstart gebruikt u een account van een id-provider voor sociale netwerken (Facebook, Google, Microsoft of Twitter).

2. Azure AD B2C geeft een aangepaste aanmeldingspagina voor het fictieve merk Wingtip Toys weergegeven voor de voorbeeld-web-App. Klik op de knop van de id-provider voor sociale netwerken die u wilt gebruiken om u aan te melden via een id-provider voor sociale netwerken.

    ![Aanmelden of registreren pagina identity provider knoppen worden weergegeven](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

    U verifiëren (aanmelden) met behulp van uw sociaalnetwerkaccount referenties en de toepassing te lezen van gegevens uit uw sociaalnetwerkaccount. Door toegang te verlenen, kan de toepassing profielgegevens van het sociaalnetwerkaccount ophalen, zoals uw naam en plaats.

3. Voltooi het aanmeldingsproces voor de id-provider.

## <a name="edit-your-profile"></a>Het profiel bewerken

Azure Active Directory B2C biedt functionaliteit waarmee gebruikers hun profielen kunnen bijwerken. In de voorbeeldweb-app wordt een Azure AD B2C-gebruikersstroom voor profielbewerking gebruikt voor de werkstroom.

1. Klik in de menubalk van de toepassing op uw profielnaam en selecteer **Profiel bewerken** om het profiel te bewerken dat u hebt gemaakt.

    ![Voorbeeld-web-app in browser met de koppeling profiel bewerken gemarkeerd](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

2. Wijzig uw **weergavenaam** of **plaats** en klik op **Doorgaan** om uw profiel bij te werken.

    De gewijzigde weergavenaam wordt in de rechterbovenhoek van de startpagina van de webtoepassing weergegeven.

## <a name="access-a-protected-api-resource"></a>Toegang tot een beveiligde API-resource

1. Klik op **Takenlijst** om uw takenlijstitems in te voeren en te wijzigen.

2. Voer tekst in het tekstvak **Nieuw item** in. Klik op **Toevoegen** om de met Azure AD B2C beveiligde web-API aan te roepen, waarmee een takenlijstitem wordt toegevoegd.

    ![Voorbeeld-web-app in browser met een takenlijstitem toevoegen](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

    De ASP.NET-webtoepassing bevat een Azure AD-toegangstoken in de aanvraag voor de beveiligde web-API-resource, waarmee bewerkingen op de takenlijstitems van de gebruiker kunnen worden uitgevoerd.

U hebt uw Azure AD B2C-gebruikersaccount gebruikt voor het geautoriseerd aanroepen van een web-API die met Azure AD B2C is beveiligd.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt uw Azure AD B2C-tenant gebruiken voor andere snelstarts of zelfstudies voor Azure AD B2C. Als u deze niet meer nodig hebt, kunt u [uw Azure AD B2C-tenant verwijderen](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u een voorbeeld-ASP.NET-toepassing op:

* Meld u aan met een aangepaste aanmeldingspagina
* Meld u aan met een sociale id-provider
* Een Azure AD B2C-account maken
* Een web-API die wordt beveiligd door Azure AD B2C aanroepen

Aan de slag met het maken van uw eigen Azure AD B2C-tenant.

> [!div class="nextstepaction"]
> [Een Azure Active Directory B2C-tenant maken in Azure Portal](tutorial-create-tenant.md)
