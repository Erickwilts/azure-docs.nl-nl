---
title: Een web-API aanroepen vanuit een web-app-micro soft Identity platform | Azure
description: Meer informatie over het bouwen van een web-app die web-Api's aanroept (waarmee een beveiligde web-API wordt aangeroepen)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1e448f52f4e8c24dd8552cae873edac841e57fc6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87058443"
---
# <a name="a-web-app-that-calls-web-apis-call-a-web-api"></a>Een web-app die web-Api's aanroept: een web-API aanroepen

Nu u een token hebt, kunt u een beveiligde web-API aanroepen.

## <a name="call-a-protected-web-api"></a>Een beveiligde web-API aanroepen

Het aanroepen van een beveiligde web-API is afhankelijk van uw taal en het gewenste Framework:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Hier volgt een vereenvoudigde code voor de actie van `HomeController` . Met deze code wordt een token opgehaald om Microsoft Graph aan te roepen. Code is toegevoegd om te laten zien hoe Microsoft Graph moet worden aangeroepen als een REST API. De URL voor de Microsoft Graph-API wordt opgegeven in het appsettings.jsbestand en wordt gelezen in een variabele met de naam `webOptions` :

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    ...
  },
  ...
  "GraphApiUrl": "https://graph.microsoft.com"
}
```

```csharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token.
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenForUserAsync(scopes);

 // Use the access token to call a protected web API.
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

  var response = await httpClient.GetAsync($"{webOptions.GraphApiUrl}/beta/me");

  if (response.StatusCode == HttpStatusCode.OK)
  {
   var content = await response.Content.ReadAsStringAsync();

   dynamic me = JsonConvert.DeserializeObject(content);
   ViewData["Me"] = me;
  }

  return View();
}
```

> [!NOTE]
> U kunt dezelfde methode gebruiken om een web-API aan te roepen.
>
> De meeste Azure-Web-Api's bieden een SDK die het aanroepen van de API vereenvoudigt. Dit geldt ook voor Microsoft Graph. In het volgende artikel leert u hoe u een zelf studie kunt vinden die API-gebruik illustreert.

# <a name="java"></a>[Java](#tab/java)

```Java
private String getUserInfoFromGraph(String accessToken) throws Exception {
    // Microsoft Graph user endpoint
    URL url = new URL("https://graph.microsoft.com/v1.0/me");

    HttpURLConnection conn = (HttpURLConnection) url.openConnection();

    // Set the appropriate header fields in the request header.
    conn.setRequestProperty("Authorization", "Bearer " + accessToken);
    conn.setRequestProperty("Accept", "application/json");

    String response = HttpClientHelper.getResponseStringFromConn(conn);

    int responseCode = conn.getResponseCode();
    if(responseCode != HttpURLConnection.HTTP_OK) {
        throw new IOException(response);
    }

    JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
    return responseObject.toString();
}

```

# <a name="python"></a>[Python](#tab/python)

```Python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service.
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

---

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verplaatsen naar productie](scenario-web-app-call-api-production.md)
