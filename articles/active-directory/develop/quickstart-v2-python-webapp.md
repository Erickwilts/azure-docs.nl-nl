---
title: Aanmelden met Microsoft toevoegen aan een Python-webapp voor Microsoft-identiteitsplatform | Azure
description: Informatie over het implementeren van Microsoft-aanmelding in een Python-webapp met behulp van OAuth2
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/25/2019
ms.author: abpati
ms.custom: aaddev, devx-track-python, scenarios:getting-started, languages:Python
ms.openlocfilehash: 6b58e927952b2a51289c3017455cc7d66545fe86
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88120317"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Quickstart: Aanmelden met Microsoft toevoegen aan een Python-webapp

In deze quickstart leert u hoe u een Python-webtoepassing integreert met het Microsoft-identiteitsplatform. Via de app wordt een gebruiker aangemeld, een toegangstoken opgehaald om de Microsoft Graph API aan te roepen, en een aanvraag ingediend bij de Microsoft Graph API.

Wanneer u de handleiding hebt voltooid, accepteert uw toepassing aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en overige accounts), en werk- of schoolaccounts van elk bedrijf of elke organisatie waar Azure Active Directory wordt gebruikt. (Zie [Hoe het voorbeeld werkt](#how-the-sample-works) voor een illustratie.)

## <a name="prerequisites"></a>Vereisten

Als u dit voorbeeld wilt uitvoeren, hebt u het volgende nodig:

- [Python 2.7+](https://www.python.org/downloads/release/python-2713) of [Python 3+](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-Session](https://pypi.org/project/Flask-Session/), [aanvragen](https://requests.kennethreitz.org/en/master/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
>
> U hebt twee opties voor het starten van de quickstart-toepassing: express (Optie 1) en handmatig (Optie 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Ga naar de [Azure-portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/PythonQuickstartPage/sourceType/docs).
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
> 1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Ga naar de pagina [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) in het Microsoft-identiteitsplatform voor ontwikkelaars.
> 1. Selecteer **Nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `python-webapp`.
>      - Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
>      - Selecteer **Registreren**.
>      - Noteer de waarde **Toepassings-id (client)** op de app-pagina **Overzicht** voor later gebruik.
> 1. Selecteer **Verificatie** in het menu en voeg dan de volgende gegevens toe:
>    - Voeg de platformconfiguratie **Web** toe. Voeg `http://localhost:5000/getAToken` toe als **Omleidings-URI's**.
>    - Selecteer **Opslaan**.
> 1. Kies in het menu aan de linkerkant de optie **Certificaten en geheimen** en klik op **Nieuw clientgeheim** in het gedeelte **Clientgeheimen**:
>
>      - Typ een beschrijving voor de sleutel (van het app-geheim van de instantie).
>      - Selecteer een sleutelduur van **Over 1 jaar**.
>      - Wanneer u op **Toevoegen** klikt, wordt de sleutelwaarde weergegeven.
>      - Kopieer de waarde van de sleutel. U hebt dit later nodig.
> 1. Selecteer het gedeelte **API-machtigingen**
>
>      - Klik op de knop **Een machtiging toevoegen** en vervolgens
>      - Zorg ervoor dat het tabblad **Microsoft-API's** is geselecteerd
>      - Klik in het gedeelte *Veelgebruikte Microsoft-API's* op **Microsoft Graph**
>      - Zorg ervoor dat in het gedeelte **Gedelegeerde toestemmingen** de juiste machtigingen zijn aangevinkt: **User.ReadBasic.All**. Gebruik het zoekvak indien nodig.
>      - Selecteer de knop **Toestemmingen toevoegen**
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
>
> Voor een juiste werking van het codevoorbeeld uit deze quickstart, moet u:
>
> 1. Een antwoord-URL toevoegen als `http://localhost:5000/getAToken`.
> 1. Een clientgeheim maken.
> 1. De gedelegeerde toestemming User.ReadBasic.All van Microsoft Graph API toevoegen.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-webapp/green-check.png) Uw toepassing is al geconfigureerd met dit kenmerk

#### <a name="step-2-download-your-project"></a>Stap 2: uw project downloaden
> [!div renderon="docs"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Download het project en pak het zip-bestand uit in een lokale map dichter bij de hoofdmap (bijvoorbeeld **C:\Azure-Samples**)
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Het codevoorbeeld downloaden](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-application"></a>Stap 3: De toepassing configureren
>
> 1. Pak het zip-bestand uit in een lokale map dichter bij de hoofdmap (bijvoorbeeld **C:\Azure-Samples**)
> 1. Als u een Integrated Development Environment gebruikt, opent u het voorbeeld in uw favoriete IDE (optioneel).
> 1. Open het bestand **app_config.py**, dat u kunt vinden in de hoofdmap en vervang dit door het volgende codefragment:
>
> ```python
> CLIENT_ID = "Enter_the_Application_Id_here"
> CLIENT_SECRET = "Enter_the_Client_Secret_Here"
> AUTHORITY = "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here"
> ```
> Waar:
>
> - `Enter_the_Application_Id_here`: de toepassings-id voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Client_Secret_Here`: het **clientgeheim** dat u in **Certificaten en geheimen** hebt gemaakt voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Tenant_Name_Here`: de waarde voor **Map-id (tenant)** van de toepassing die u hebt geregistreerd.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>Stap 3: Het codevoorbeeld uitvoeren

> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>Stap 4: Het codevoorbeeld uitvoeren

1. U moet de MSAL Python-bibliotheek, Flask-framework, Flask-Sessions voor sessiebeheer aan de serverzijde en aanvragen installeren met behulp van PIP als volgt:

    ```Shell
    pip install -r requirements.txt
    ```

2. Voer app.py uit vanuit de shell of de opdrachtregel:

    ```Shell
    python app.py
    ```
   > [!IMPORTANT]
   > Deze quickstarttoepassing gebruikt een clientgeheim om zichzelf te identificeren als vertrouwelijke client. Omdat het clientgeheim als platte tekst aan uw projectbestanden wordt toegevoegd, wordt u om veiligheidsredenen aangeraden een certificaat te gebruiken in plaats van een clientgeheim voordat u de toepassing als productietoepassing beschouwt. Zie voor meer informatie over het gebruik van een certificaat [deze instructies](./active-directory-certificate-credentials.md).

## <a name="more-information"></a>Meer informatie

### <a name="how-the-sample-works"></a>Hoe het voorbeeld werkt
![Toont hoe de voorbeeld-app werkt die is gegenereerd door deze quickstart](media/quickstart-v2-python-webapp/python-quickstart.svg)

### <a name="getting-msal"></a>MSAL ophalen
MSAL is de bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en de aanvraagtokens die worden gebruikt voor toegang tot een API die is beveiligd via het Microsoft-identiteitsplatform.
U kunt MSAL Python met behulp van PIP toevoegen aan uw toepassing.

```Shell
pip install msal
```

### <a name="msal-initialization"></a>MSAL initialiseren
U kunt de verwijzing toevoegen aan MSAL Python door de volgende code toe te voegen aan de bovenkant van het bestand waarin u MSAL gaat gebruiken:

```Python
import msal
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over web-apps waarmee gebruikers zich kunnen aanmelden en die web-API's aanroepen:

> [!div class="nextstepaction"]
> [Scenario: Web-apps waarmee gebruikers zich aanmelden](scenario-web-app-sign-user-overview.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]