---
title: Uitvoeren van meerdere afhankelijke services met .NET Core en Visual Studio
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s '
ms.openlocfilehash: 487ad5c4f68f2fd965384a33aa9f6c0e8da351a4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800752"
---
# <a name="multi-service-development-with-azure-dev-spaces"></a>Ontwikkeling van meerdere services met Azure Dev Spaces

In deze zelfstudie leert u hoe u toepassingen met meerdere services ontwikkelt met Azure Dev Spaces, samen met enkele aanvullende voordelen van Azure Dev Spaces.

## <a name="call-another-container"></a>Een andere container aanroepen
In deze sectie maakt u een tweede service, `mywebapi`, en laat u deze aanroepen in `webfrontend`. Elke service wordt uitgevoerd in een afzonderlijke container. Vervolgens spoort u fouten op in beide containers.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Voorbeeldcode voor *mywebapi* downloaden
Omwille van de tijd downloaden we voorbeeldcode uit een GitHub-opslagplaats. Ga naar https://github.com/Azure/dev-spaces en selecteer **Clone or Download** om de GitHub-opslagplaats te downloaden. De code voor deze sectie bevindt zich in `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>*mywebapi* uitvoeren
1. Open het `mywebapi`-project in een *afzonderlijk Visual Studio-venster*.
1. Selecteer **Azure Dev Spaces** in de vervolgkeuzelijst met opstartinstellingen zoals u eerder hebt gedaan voor het `webfrontend`-project. In plaats van een nieuw AKS-cluster te maken, kunt u nu de ontwikkelomgeving selecteren die u al hebt gemaakt. Laat ook hier de standaard bij Space ingesteld op `default` en klik op **OK**. In het uitvoervenster merkt u wellicht Visual Studio begint met "opgewarmd' deze nieuwe service in de adresruimte van uw dev om dingen sneller als u foutopsporing start.
1. Druk op F5 en wacht tot de service is gebouwd en geïmplementeerd. Dit proces is voltooid, zodra de Visual Studio-statusbalk oranje wordt
1. Noteer de eindpunt-URL die wordt weergegeven in het deelvenster **Azure Dev Spaces for AKS** in het venster **Uitvoer**. Deze ziet er ongeveer als volgt uit: `http://localhost:<portnumber>`. Het lijkt misschien alsof de container lokaal wordt uitgevoerd, maar dat is niet zo. De container wordt uitgevoerd in de ontwikkelomgeving in Azure.
2. Wanneer `mywebapi` klaar is, opent u de browser naar het localhost-adres en voegt u `/api/values` toe aan het eind van de URL om de standaard-GET API voor de `ValuesController` aan te roepen. 
3. Als alle stappen zijn voltooid, ziet u een reactie van de `mywebapi`-service die er ongeveer als volgt uitziet.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Verzend een aanvraag van *webfrontend* naar *mywebapi*
Nu gaan we code schrijven in `webfrontend` waarmee een aanvraag wordt verzonden naar `mywebapi`. Schakel over naar het Visual Studio-venster met het `webfrontend`-project. *Vervang* de code voor de About-methode in het `HomeController.cs`-bestand door de volgende code:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

In het vorige codevoorbeeld wordt de `azds-route-as`-header van de binnenkomende aanvraag doorgestuurd naar de uitgaande aanvraag. Verderop ziet u hoe deze afgeleide klasse een productievere ontwikkeling in [teamscenario's](team-development-netcore-visualstudio.md) vergemakkelijkt.

### <a name="debug-across-multiple-services"></a>Foutopsporing in meerdere services
1. Op dit punt moet `mywebapi` nog steeds worden uitgevoerd met het bijgevoegde foutopsporingsprogramma. Als dit niet het geval is, drukt u op F5 in het `mywebapi`-project.
1. Stel een onderbrekingspunt in de `Get(int id)`-methode in het `Controllers/ValuesController.cs`-bestand in waarmee `api/values/{id}` GET-aanvragen worden verwerkt.
1. Stel in het `webfrontend`-project waarin u de bovenstaande code hebt geplakt, een onderbrekingspunt in vlak voordat een GET-aanvraag wordt verzonden naar `mywebapi/api/values`.
1. Druk op F5 in het `webfrontend`-project. In Visual Studio wordt opnieuw een browser geopend op de juiste localhost-poort en de web-app wordt weergegeven.
1. Klik boven aan de pagina op de koppeling **About** om het onderbrekingspunt te activeren in het `webfrontend`-project. 
1. Druk F10 om door te gaan. Het onderbrekingspunt in het `mywebapi`-project wordt nu geactiveerd.
1. Druk op F5 om door te gaan. U wordt teruggeleid naar de code in het `webfrontend`-project.
1. Als u nog een keer op F5 drukt, wordt de aanvraag voltooid en gaat u terug naar een pagina in de browser. In de web-app wordt nu op de pagina About een bericht weergegeven dat is samengesteld uit de twee services: "Hello from webfrontend and Hello from mywebapi."

Dat is dus gelukt. U hebt nu een toepassing met meerdere containers waarin elke container afzonderlijk kan worden ontwikkeld en geïmplementeerd.

### <a name="automatic-tracing-for-http-messages"></a>Automatische tracering voor HTTP-berichten
U hebt mogelijk opgemerkt dat, hoewel *webfrontend* geen speciale code bevat om de HTTP-aanroep af te drukken die deze naar *mywebapi* uitvoert, u kunt zien dat HTTP berichten in het uitvoervenster traceert:
```
// The request from your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io --gyk-> webfrontend:
   GET /Home/About HTTP/1.1

// *webfrontend* reaching out to *mywebapi*
webfrontend-668b7ddb9f-n5rhj --pu5-> mywebapi:
   GET /api/values/1 HTTP/1.1

// Response from *mywebapi*
webfrontend-668b7ddb9f-n5rhj <-pu5-- mywebapi:
   HTTP/1.1 200 OK
   Hello from mywebapi

// Response from *webfrontend* to your browser
default.webfrontend.856bb3af715744c6810b.eus.azds.io <-gyk-- webfrontend:
   HTTP/1.1 200 OK
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8" />
       <meta name="viewport" content="width=device-width, initial-sc...<[TRUNCATED]>
```
Dit is een van de 'gratis' voordelen van Dev Spaces-instrumentatie. We voegen onderdelen in die HTTP-aanvragen traceren terwijl deze het systeem doorlopen om het voor u eenvoudiger te maken om tijdens de ontwikkeling aanroepen naar meerdere services te traceren.

### <a name="well-done"></a>Dat is dus gelukt.
U hebt nu een toepassing met meerdere containers waarin elke container afzonderlijk kan worden ontwikkeld en geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over teamontwikkeling in Dev Spaces](team-development-netcore-visualstudio.md)
