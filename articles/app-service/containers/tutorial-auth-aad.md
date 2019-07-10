---
title: Gebruikers end-to-end verifiëren en autoriseren onder Linux - Azure App Service | Microsoft Docs
description: Leer hoe u verificatie en autorisatie van App Service kunt gebruiken om uw App Service-apps onder Linux te beveiligen, waaronder toegang tot externe API's.
keywords: app service, azure app service, authN, authZ, beveiligen, beveiliging, meerdere lagen, azure active directory, azure ad
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/26/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 2c173da9bfb60f74b90a17f4f3c5ea6f930ca528
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705842"
---
# <a name="tutorial-authenticate-and-authorize-users-end-to-end-in-azure-app-service-on-linux"></a>Zelfstudie: gebruikers end-to-end verifiëren en autoriseren in Azure App Service in Linux

[App Service onder Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie onder het Linux-besturingssysteem. Daarnaast bevat App Service ingebouwde ondersteuning voor [verificatie en autorisatie van gebruikers](../overview-authentication-authorization.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json). In deze zelfstudie leest u hoe u apps kunt beveiligen met verificatie en autorisatie van App Service. Er wordt gebruikgemaakt van een ASP.NET Core-app met een Angular.js-front-end, maar dat is slechts voor het voorbeeld. Verificatie en autorisatie van App Service ondersteunt runtime voor alle talen en u leert hoe u deze kunt toepassen op uw taal van voorkeur door de zelfstudie te volgen.

Er wordt gebruikgemaakt van de voorbeeld-app om te laten zien hoe u een zelfstandige app beveiligt (in [Verificatie en autorisatie inschakelen voor back-end-app](#enable-authentication-and-authorization-for-back-end-app)).

![Eenvoudige verificatie en autorisatie](./media/tutorial-auth-aad/simple-auth.png)

U leest ook hoe u een app met meerdere lagen beveiligt door namens de geverifieerde gebruiker een beveiligde back-end-API te openen, zowel door middel van [servercode](#call-api-securely-from-server-code) als [browsercode](#call-api-securely-from-browser-code).

![Geavanceerde verificatie en autorisatie](./media/tutorial-auth-aad/advanced-auth.png)

Dit zijn slechts enkele van de mogelijke scenario's voor verificatie en autorisatie in App Service. 

Hieronder vindt u een meer uitgebreide lijst met zaken die u in de zelfstudie leert:

> [!div class="checklist"]
> * Ingebouwde verificatie en autorisatie inschakelen
> * Apps beveiligen tegen niet-geverifieerde aanvragen
> * Azure Active Directory gebruiken als id-provider
> * Externe apps openen namens de aangemelde gebruiker
> * Aanroepen tussen services beveiligen met tokenverificatie
> * Toegangstokens gebruiken vanuit servercode
> * Toegangstokens gebruiken vanuit clientcode (browser)

U kunt de stappen in deze zelfstudie volgen voor macOS, Linux en Windows.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* [Installeer Git](https://git-scm.com/).
* [Installeer .NET Core 2.0](https://www.microsoft.com/net/core/).

## <a name="create-local-net-core-app"></a>Lokale .NET Core-app maken

In deze stap stelt u het lokale .NET Core-project in. U gebruikt hetzelfde project voor het implementeren van een back-end-API-app en een front-end-web-app.

### <a name="clone-and-run-the-sample-application"></a>De voorbeeldtoepassing klonen en uitvoeren

Voer de volgende opdrachten uit om de voorbeeldopslagplaats te klonen en voer deze uit.

```bash
git clone https://github.com/Azure-Samples/dotnet-core-api
cd dotnet-core-api
dotnet run
```

Ga naar `http://localhost:5000` en voeg taken toe, bewerk deze en verwijder ze. 

![ASP.NET Core-API lokaal uitvoeren](./media/tutorial-auth-aad/local-run.png)

Druk op `Ctrl+C` in de terminal als u ASP.NET Core wilt stoppen.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-apps-to-azure"></a>Apps in Azure implementeren

In deze stap implementeert u het project in twee App Service-apps. De ene is de front-end-app en de andere is de back-end-app.

### <a name="create-azure-resources"></a>Azure-resources maken

Voer in Azure Cloud Shell de volgende opdrachten uit om twee App Serviceapps te maken. Vervang _&lt;front\_end\_app\_name>_ en _&lt;back\_end\_app\_name>_ door twee globaal unieke app-namen (geldige tekens zijn `a-z`, `0-9` en `-`). Zie [Een .NET Core-app maken in App-Service in Linux](quickstart-dotnetcore.md) voor meer informatie over elke opdracht.

```azurecli-interactive
az group create --name myAuthResourceGroup --location "West Europe"
az appservice plan create --name myAuthAppServicePlan --resource-group myAuthResourceGroup --sku B1 --is-linux
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <front_end_app_name> --runtime "dotnetcore|2.0" --deployment-local-git --query deploymentLocalGitUrl
az webapp create --resource-group myAuthResourceGroup --plan myAuthAppServicePlan --name <back_end_app_name> --runtime "dotnetcore|2.0" --deployment-local-git --query deploymentLocalGitUrl
```

> [!NOTE]
> Sla de URL's van de externe Git-instanties voor de front-en-app en de back-end-app op. Deze worden in de uitvoer van `az webapp create` weergegeven.
>

### <a name="configure-cors"></a>CORS configureren

Deze stap heeft geen betrekking op verificatie en autorisatie. De stap is echter nodig om later [de back-end-API aan te roepen vanuit de code van de front-endbrowser](#call-api-securely-from-browser-code), zodat uw browser de API-aanroepen tussen domeinen vanuit de app Angular.js toestaat. App Service on Linux biedt nu ondersteuning voor CORS-functionaliteit, zoals [bijbehorende equivalent Windows biedt](../app-service-web-tutorial-rest-api.md#add-cors-functionality).

Open het bestand _Startup.cs_ in de lokale opslagplaats. Voeg de volgende coderegel toe aan de methode `ConfigureServices(IServiceCollection services)`:

```csharp
services.AddCors();
```

Voeg aan het begin van de methode `Configure(IApplicationBuilder app)` de volgende coderegel toe (vervang *\<front_end_app_name >* ):

```csharp
app.UseCors(builder =>
    builder.WithOrigins("http://<front_end_app_name>.azurewebsites.net"));
```

Sla uw wijzigingen op. Ga terug naar het _lokale terminalvenster_ en voer de volgende opdrachten uit om de wijzigingen op te slaan in de Git-opslagplaats.

```bash
git add .
git commit -m "add CORS to back end"
```

> [!NOTE]
> U hoeft zich geen zorgen te maken over het delen van deze code tussen de front-end- en back-end-apps. Er is geen sprake van CORS-invloed op de front-endapp.
> 

### <a name="push-to-azure-from-git"></a>Pushen naar Azure vanaf Git

Voer in het lokale terminalvenster de volgende Git-opdrachten uit om de implementatie in de back-end-app uit te voeren. Vervang _&lt;deploymentLocalGitUrl-of-back-end-app>_ door de URL van de externe Git-instantie die u hebt opgeslagen in [Azure-resources maken](#create-azure-resources). Wanneer u door Git Credential Manager om referenties wordt gevraagd, geeft u [de referenties voor implementatie](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) op en niet de referenties die u gebruikt om u aan te melden bij Azure Portal.

```bash
git remote add backend <deploymentLocalGitUrl-of-back-end-app>
git push backend master
```

Voer in het lokale terminalvenster de volgende Git-opdrachten uit om dezelfde code in de front-end-app te implementeren. Vervang _&lt;deploymentLocalGitUrl-of-front-end-app>_ door de URL van de externe Git-instantie die u hebt opgeslagen in [Azure-resources maken](#create-azure-resources).

```bash
git remote add frontend <deploymentLocalGitUrl-of-front-end-app>
git push frontend master
```

### <a name="browse-to-the-azure-apps"></a>Naar de Azure-app bladeren

Ga in een browser naar de volgende URL's om de twee apps in werking te zien.

```
http://<back_end_app_name>.azurewebsites.net
http://<front_end_app_name>.azurewebsites.net
```

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/azure-run.png)

> [!NOTE]
> Als de app opnieuw wordt gestart, hebt u wellicht gezien dat nieuwe gegevens zijn gewist. Dit gedrag is inherent aan het ontwerp omdat de ASP.NET Core-voorbeeld-app gebruikmaakt van een database in het geheugen.
>
>

## <a name="call-back-end-api-from-front-end"></a>Back-end-API aanroepen vanuit front-end

In deze stap legt u voor de servercode van de front-end-app vast dat toegang tot de back-end-API mogelijk is. Later schakelt u geverifieerde toegang in vanaf de front-end tot de back-end.

### <a name="modify-front-end-code"></a>Front-end-code wijzigen

Open _Controllers/TodoController.cs_ in de lokale opslagplaats. Voeg aan het begin van de `TodoController`-klasse de volgende regels toe en vervang _&lt;back\_end\_app\_name>_ door de naam van de back-end-app:

```cs
private static readonly HttpClient _client = new HttpClient();
private static readonly string _remoteUrl = "https://<back_end_app_name>.azurewebsites.net";
```

Zoek de `GetAll()`-methode en vervang de code binnen de accolades door:

```cs
var data = _client.GetStringAsync($"{_remoteUrl}/api/Todo").Result;
return JsonConvert.DeserializeObject<List<TodoItem>>(data);
```

In de eerste regel wordt een `GET /api/Todo`-aanroep naar de back-end-API-app uitgevoerd.

Zoek vervolgens de `GetById(long id)`-methode en vervang de code binnen de accolades door:

```cs
var data = _client.GetStringAsync($"{_remoteUrl}/api/Todo/{id}").Result;
return Content(data, "application/json");
```

In de eerste regel wordt een `GET /api/Todo/{id}`-aanroep naar de back-end-API-app uitgevoerd.

Zoek vervolgens de `Create([FromBody] TodoItem item)`-methode en vervang de code binnen de accolades door:

```cs
var response = _client.PostAsJsonAsync($"{_remoteUrl}/api/Todo", item).Result;
var data = response.Content.ReadAsStringAsync().Result;
return Content(data, "application/json");
```

In de eerste regel wordt een `POST /api/Todo`-aanroep naar de back-end-API-app uitgevoerd.

Zoek vervolgens de `Update(long id, [FromBody] TodoItem item)`-methode en vervang de code binnen de accolades door:

```cs
var res = _client.PutAsJsonAsync($"{_remoteUrl}/api/Todo/{id}", item).Result;
return new NoContentResult();
```

In de eerste regel wordt een `PUT /api/Todo/{id}`-aanroep naar de back-end-API-app uitgevoerd.

Zoek vervolgens de `Delete(long id)`-methode en vervang de code binnen de accolades door:

```cs
var res = _client.DeleteAsync($"{_remoteUrl}/api/Todo/{id}").Result;
return new NoContentResult();
```

In de eerste regel wordt een `DELETE /api/Todo/{id}`-aanroep naar de back-end-API-app uitgevoerd.

Sla alle wijzigingen op. Implementeer via het lokale terminalvenster de wijzigingen aan de front-end-app met de volgende Git-opdrachten:

```bash
git add .
git commit -m "call back-end API"
git push frontend master
```

### <a name="check-your-changes"></a>Wijzigingen controleren

Ga naar `http://<front_end_app_name>.azurewebsites.net` en voeg enkele items toe, bijvoorbeeld `from front end 1` en `from front end 2`.

Ga naar `http://<back_end_app_name>.azurewebsites.net` om de items te zien die aan de front-end-app zijn toegevoegd. Voeg nu ook een paar items toe, zoals `from back end 1` en `from back end 2`, en vernieuw de front-end-app om de doorgevoerde wijzigingen te zien.

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/remote-api-call-run.png)

## <a name="configure-auth"></a>Verificatie configureren

In deze stap schakelt u verificatie en autorisatie voor de twee apps in. U configureert ook de front-end-app om een toegangstoken te genereren dat u kunt gebruiken om geverifieerde aanroepen uit te voeren naar de back-end-app.

U gebruikt Azure Active Directory als id-provider. Zie [Verificatie van Azure Active Directory configureren voor de App Services-toepassing](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) voor meer informatie.

### <a name="enable-authentication-and-authorization-for-back-end-app"></a>Verificatie en autorisatie voor de back-end-app inschakelen

Open in de [Azure-portal](https://portal.azure.com) de beheerpagina voor de back-end-app door in het linkermenu te klikken op: **Resourcegroepen** > **myAuthResourceGroup** >  _\<terug\_end\_app\_naam>_ .

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/portal-navigate-back-end.png)

Klik in het linkermenu van de back-end-app op **Verificatie/autorisatie** en schakel App Service-verificatie in door op **Aan** te klikken.

Selecteer **Te ondernemen actie wanneer de aanvraag niet is geverifieerd** de optie **Aanmelden met Azure Active Directory**.

Klik bij **Verificatieproviders** op **Azure Active Directory** 

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/configure-auth-back-end.png)

Klik op **Express** en accepteer de standaardinstellingen om een nieuwe AD-app te maken. Klik vervolgens op **OK**.

Klik op de pagina **Verificatie/autorisatie** op **Opslaan**. 

Vernieuw de pagina als de melding met het bericht `Successfully saved the Auth Settings for <back_end_app_name> App` wordt weergegeven.

Klik nogmaals op **Azure Active Directory** en klik op **Toepassing beheren**.

Kopieer vanaf de beheerpagina van de AD-toepassing de **toepassings-id** in Kladblok. U hebt deze waarde later nog nodig.

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/get-application-id-back-end.png)

### <a name="enable-authentication-and-authorization-for-front-end-app"></a>Verificatie en autorisatie voor de front-end-app inschakelen

Volg dezelfde stappen voor de front-end-app, maar sla de laatste stap over. U hebt de **toepassings-id** niet voor de front-end-app nodig. Houd de pagina **Azure Active Directory-instellingen** geopend.

Ga, als u wilt, naar `http://<front_end_app_name>.azurewebsites.net`. Hier moet u worden doorgestuurd naar een aanmeldingspagina. Nadat u zich hebt aangemeld, hebt u nog steeds geen toegang tot de gegevens in de back-end-app, omdat u nog drie dingen moet doen:

- De front-end toegang geven tot de back-end
- App Service configureren zodat een bruikbaar token wordt geretourneerd
- De token in de code gebruiken

> [!TIP]
> Als u tegen fouten aanloopt en de instellingen voor verificatie en autorisatie opnieuw configureert, kunnen de tokens in het tokenarchief mogelijk niet met de nieuwe instellingen opnieuw worden gegenereerd. Als u er zeker van wilt zijn dat de tokens opnieuw worden gegenereerd, dient u zich af te melden en vervolgens opnieuw aan te melden. U kunt dit doen door de browser in de privémodus te zetten en de browser vervolgens te sluiten en opnieuw in de privémodus te openen nadat u de instellingen in de apps hebt gewijzigd.

### <a name="grant-front-end-app-access-to-back-end"></a>Front-end-app toegang verlenen tot de back-end-app

Nu u voor beide apps verificatie en autorisatie hebt ingeschakeld, worden beide door een AD-toepassing ondersteund. In deze stap geeft u de front-end-app namens de gebruiker machtigingen voor toegang tot de back-end-app. (Technisch gesproken geeft u de _AD-toepassing_ van de front-end-app namens de gebruiker machtigingen voor toegang tot de _AD-toepassing_ van de back-end-app.)

Op dit punt moet u zich bevinden op de pagina **Azure Active Directory Settings** voor de front-end-app. Als dat niet zo is, gaat u terug naar die pagina. 

Klik op **Machtigingen beheren** > **Toevoegen** > **Select an API**.

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/add-api-access-front-end.png)

Typ op de pagina **Select an API** de naam van de AD-toepassing voor de back-end-app. Dit is dezelfde naam als de standaardnaam van de back-end-app. Selecteer de naam in de lijst en klik op **Selecteren**.

Schakel het selectievakje in naast **Access _&lt;AD\_application\_name>_** . Klik op **Selecteren** > **Gereed**.

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/select-permission-front-end.png)

### <a name="configure-app-service-to-return-a-usable-access-token"></a>App Service configureren zodat een bruikbaar toegangstoken wordt geretourneerd

De front-end-app heeft nu de vereiste machtigingen. In deze stap configureert u App Service-verificatie en -autorisatie zodat u een bruikbaar toegangstoken hebt voor toegang tot de back-end. Voor deze stap hebt u de toepassings-id van de back-end nodig die u hebt gekopieerd vanuit [Verificatie en autorisatie voor de back-end-app inschakelen](#enable-authentication-and-authorization-for-back-end-app).

Meld u aan bij [Azure Resource Explorer](https://resources.azure.com). Klik boven aan de pagina op **Lezen/schrijven** om de Azure-resources te kunnen bewerken.

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/resources-enable-write.png)

Klik in de linkerbrowser op **subscriptions** > ** _&lt;your\_subscription>_**  > **resourceGroups** > **myAuthResourceGroup** > **providers** > **Microsoft.Web** > **sites** >  ** _\<front\_end\_app\_name>_**  > **config** > **authsettings**.

Klik in de weergave **authsettings** op **Bewerken**. Stel `additionalLoginParams` in op de volgende JSON-tekenreeks met behulp van de toepassings-id die u hebt gekopieerd. 

```json
"additionalLoginParams": ["response_type=code id_token","resource=<back_end_application_id>"],
```

![ASP.NET Core-API uitvoeren in Azure App Service](./media/tutorial-auth-aad/additional-login-params-front-end.png)

Sla de instellingen op door op **PUT** te klikken.

Uw apps zijn nu geconfigureerd. De front-end is nu gereed voor toegang tot de back-end met het juiste toegangstoken.

Zie [Toegangstokens vernieuwen](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#refresh-identity-provider-tokens) voor informatie over het configureren hiervan voor andere providers.

## <a name="call-api-securely-from-server-code"></a>API veilig aanroepen vanuit servercode

In deze stap activeert u de eerder gewijzigde servercode, zodat u geverifieerde aanroepen naar de back-end-API kunt uitvoeren.

De front-end-app heeft nu de vereiste machtiging en voegt tevens de toepassings-id van de back-end-app toe aan de aanmeldingsparameters. Op die manier kan een toegangstoken voor verificatie van de back-end-app verkregen worden. App Service stelt dit token aan de servercode beschikbaar door in elke geverifieerde aanvraag een `X-MS-TOKEN-AAD-ACCESS-TOKEN`-header te injecteren (zie [Tokens ophalen in app-code](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#retrieve-tokens-in-app-code)).

> [!NOTE]
> Deze headers worden voor alle ondersteunde talen geïnjecteerd. U kunt ze openen met het standaardpatroon voor elke betreffende taal.

Open opnieuw _Controllers/TodoController.cs_ in de lokale opslagplaats. Voeg onder de `TodoController(TodoContext context)`-constructor de volgende code toe:

```cs
public override void OnActionExecuting(ActionExecutingContext context)
{
    base.OnActionExecuting(context);

    _client.DefaultRequestHeaders.Accept.Clear();
    _client.DefaultRequestHeaders.Authorization =
        new AuthenticationHeaderValue("Bearer", Request.Headers["x-ms-token-aad-access_token"]);
}
```

Met deze code wordt de standaard-HTTP-header `Authorization: Bearer <access_token>` aan alle externe API-aanroepen toegevoegd. In de ASP.NET Core-pijplijn voor de uitvoering van de aanvraag wordt `OnActionExecuting` vlak voor de desbetreffende actiemethode (zoals `GetAll()`) uitgevoerd, zodat nu elke uitgaande API-aanroep over het toegangstoken beschikt.

Sla alle wijzigingen op. Implementeer via het lokale terminalvenster de wijzigingen aan de front-end-app met de volgende Git-opdrachten:

```bash
git add .
git commit -m "add authorization header for server code"
git push frontend master
```

Meld u opnieuw aan bij `http://<front_end_app_name>.azurewebsites.net`. Klik op de pagina over de gebruiksovereenkomst voor gebruikersgegevens op **Accepteren**.

U moet nu net als eerder gegevens uit de back-end-app kunnen maken, lezen, bijwerken en verwijderen. Het enige verschil is dat beide apps nu worden beveiligd door App Service-verificatie en -autorisatie, waaronder de aanroepen tussen services.

Gefeliciteerd! De servercode heeft nu toegang tot de gegevens van de back-end namens de geverifieerde gebruiker.

## <a name="call-api-securely-from-browser-code"></a>API veilig vanuit browsercode aanroepen

In deze stap legt u vast dat de Angular.js-app van de front-end naar de back-end-API verwijst. Op die manier leert u hoe u het toegangstoken kunt ophalen en er API-aanroepen naar de back-end-app mee kunt uitvoeren.

Terwijl de servercode toegang heeft tot de aanvraag-headers, heeft de clientcode toegang tot `GET /.auth/me` om dezelfde toegangstokens te kunnen ophalen (zie [Tokens ophalen in app-code](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#retrieve-tokens-in-app-code)).

> [!TIP]
> In deze sectie worden de standaard-HTTP-methoden gebruikt om de veilige HTTP-aanroepen te demonstreren. U kunt echter [Active Directory Authentication Library (ADAL) voor JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js) gebruiken om het Angular.js-toepassingspatroon te vereenvoudigen.
>

### <a name="point-angularjs-app-to-back-end-api"></a>Angular.js-app naar back-end-API laten verwijzen

Open _wwwroot/index.html_ in de lokale opslagplaats.

Stel in regel 51 de variabele `apiEndpoint` in op de URL van uw back-end-app (`http://<back_end_app_name>.azurewebsites.net`). Vervang _\<back\_end\_app\_name>_ door de naam van uw app in App Service.

Open _wwwroot/app/scripts/todoListSvc.js_ in de lokale opslagplaats en controleer of alle API-aanroepen door `apiEndpoint` vooraf worden gegaan. De Angular.js-app roept nu de back-end-API's aan. 

### <a name="add-access-token-to-api-calls"></a>Toegangstoken toevoegen aan API-aanroepen

Voeg in _wwwroot/app/scripts/todoListSvc.js_, boven de lijst met API-aanroepen (boven de regel `getItems : function(){`), de volgende functie aan de lijst toe:

```javascript
setAuth: function (token) {
    $http.defaults.headers.common['Authorization'] = 'Bearer ' + token;
},
```

Deze functie wordt aangeroepen om de standaard-`Authorization`-header in te stellen met het toegangstoken. De functie wordt in de volgende stap aangeroepen.

Open _wwwroot/app/scripts/app.js_ in de lokale opslagplaats en zoek de volgende code:

```javascript
$routeProvider.when("/Home", {
    controller: "todoListCtrl",
    templateUrl: "/App/Views/TodoList.html",
}).otherwise({ redirectTo: "/Home" });
```

Vervang het hele codeblok door de volgende code:

```javascript
$routeProvider.when("/Home", {
    controller: "todoListCtrl",
    templateUrl: "/App/Views/TodoList.html",
    resolve: {
        token: ['$http', 'todoListSvc', function ($http, todoListSvc) {
            return $http.get('/.auth/me').then(function (response) {
                todoListSvc.setAuth(response.data[0].access_token);
                return response.data[0].access_token;
            });
        }]
    },
}).otherwise({ redirectTo: "/Home" });
```

Door deze wijziging wordt de toewijzing `revolve` toegevoegd, waarmee `/.auth/me` wordt aangeroepen en het toegangstoken ingesteld. Hiermee wordt gegarandeerd dat u over het toegangstoken beschikt voordat de controller `todoListCtrl` wordt geïnstantieerd. Op die manier bevatten alle API-aanroepen van de controller het token.

### <a name="deploy-updates-and-test"></a>Updates implementeren en tests uitvoeren

Sla alle wijzigingen op. Implementeer via het lokale terminalvenster de wijzigingen aan de front-end-app met de volgende Git-opdrachten:

```bash
git add .
git commit -m "add authorization header for Angular"
git push frontend master
```

Ga opnieuw naar `http://<front_end_app_name>.azurewebsites.net`. U moet nu rechtstreeks in de Angular.js-app gegevens in de back-end kunnen maken, lezen, bijwerken en verwijderen.

Gefeliciteerd! De clientcode heeft nu toegang tot de gegevens van de back-end namens de geverifieerde gebruiker.

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een resourcegroep gemaakt. Als u deze resources in de toekomst niet verwacht te gebruiken, moet u de resourcegroep verwijderen met de volgende opdracht in de Cloud-Shell:

```azurecli-interactive
az group delete --name myAuthResourceGroup
```

Het kan een minuut duren voordat deze opdracht is uitgevoerd.

<a name="next"></a>
## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Ingebouwde verificatie en autorisatie inschakelen
> * Apps beveiligen tegen niet-geverifieerde aanvragen
> * Azure Active Directory gebruiken als id-provider
> * Externe apps openen namens de aangemelde gebruiker
> * Aanroepen tussen services beveiligen met tokenverificatie
> * Toegangstokens gebruiken vanuit servercode
> * Toegangstokens gebruiken vanuit clientcode (browser)

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw app kunt toewijzen.

> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure App Service](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
