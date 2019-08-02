---
title: Een door HTTP geactiveerde functie maken in azure
description: Informatie over hoe u met de Azure Functions Core Tools en de Azure CLI uw eerste Python-functie maakt in Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 58f5cfd3718720cafc922bbd7b974a353e0d9d02
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68722783"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Een door HTTP geactiveerde functie maken in azure

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

In dit artikel leest u hoe u opdracht regel Programma's kunt gebruiken om een python-project te maken dat wordt uitgevoerd in Azure Functions. De functie die u maakt, wordt geactiveerd door HTTP-aanvragen. Ten slotte publiceert u uw project om te worden uitgevoerd als een [serverloze functie](functions-scale.md#consumption-plan) in Azure.

Dit artikel is de eerste van twee Quick starts voor Azure Functions. Nadat u dit artikel hebt voltooid, [voegt u een Azure Storage wachtrij-uitvoer binding](functions-add-output-binding-storage-queue-python.md) toe aan uw functie.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u het volgende hebben:

+ Installeer [Python 3.6](https://www.python.org/downloads/).

+ Installeer [Azure functions core tools](./functions-run-local.md#v2) versie 2.6.1071 of een latere versie.

+ Installeer de [Azure cli](/cli/azure/install-azure-cli) versie 2. x of een nieuwere versie.

+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Een virtuele omgeving maken en activeren

Als u python-functies lokaal wilt ontwikkelen en testen, moet u in een python 3,6-omgeving werken. Voer de volgende opdrachten uit om een virtuele omgeving met de naam `.venv` te maken.

### <a name="bash"></a>Bash

```bash
python3.6 -m venv .venv
source .venv/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>Power shell of een Windows-opdracht prompt:

```powershell
py -3.6 -m venv .venv
.venv\scripts\activate
```

De overige opdrachten worden uitgevoerd binnen de virtuele omgeving.

## <a name="create-a-local-functions-project"></a>Een lokaal Functions-project maken

Een functions-project is het equivalent van een functie-app in Azure. Het kan meerdere functies hebben die allemaal dezelfde lokale en hosting configuraties delen.

Voer in de virtuele omgeving de volgende opdracht uit en kies **python** als uw runtime voor de werk nemer.

```console
func init MyFunctionProj
```

Er wordt een map gemaakt met de naam _MyFunctionProj_ , die de volgende drie bestanden bevat:

* `local.settings.json`wordt gebruikt voor het opslaan van app-instellingen en verbindings reeksen wanneer deze lokaal worden uitgevoerd. Dit bestand wordt niet gepubliceerd naar Azure.
* `requirements.txt`bevat de lijst met pakketten die moeten worden geïnstalleerd bij het publiceren naar Azure.
* `host.json`bevat algemene configuratie opties die van invloed zijn op alle functies in een functie-app. Dit bestand wordt gepubliceerd naar Azure.

Ga naar de map New MyFunctionProj:

```console
cd MyFunctionProj
```

Vervolgens werkt u het bestand host. json bij om uitbreidings bundels in te scha kelen.  

## <a name="create-a-function"></a>Een functie maken

Als u een functie wilt toevoegen aan uw project, voert u de volgende opdracht uit:

```console
func new
```

Kies de sjabloon **http-trigger** , `HttpTrigger` Typ als naam voor de functie en druk op ENTER.

Er wordt een submap met de naam _http trigger_ gemaakt die de volgende bestanden bevat:

* **Function. json**: configuratie bestand dat de functie, trigger en andere bindingen definieert. Bekijk dit bestand en controleer of de waarde voor `scriptFile` verwijst naar het bestand met de functie, terwijl de aanroep-en bindingen zijn gedefinieerd in de `bindings` matrix.

  Elke binding vereist een richting, type en een unieke naam. De http-trigger heeft een invoer binding van [`httpTrigger`](functions-bindings-http-webhook.md#trigger) het type en de uitvoer [`http`](functions-bindings-http-webhook.md#output)binding van het type.

* **init.py\_: script bestand dat uw door http geactiveerde functie is.\_ \_ \_** Controleer dit script en controleer of het een standaard waarde `main()`bevat. HTTP-gegevens van de trigger worden door gegeven aan deze functie `req` met behulp van de benoemde bindings parameter. Gedefinieerd in function. json, `req` is een instantie van de [klasse Azure. functions. HttpRequest](/python/api/azure-functions/azure.functions.httprequest). 

    Het retour object, gedefinieerd als `$return` in function. json, is een instantie van de [klasse Azure. functions. HttpResponse](/python/api/azure-functions/azure.functions.httpresponse). Zie [Azure functions HTTP-triggers en-bindingen](functions-bindings-http-webhook.md)voor meer informatie.

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Met de volgende opdracht wordt de functie-app gestart, die lokaal wordt uitgevoerd met dezelfde Azure Functions runtime die zich in azure bevindt.

```bash
func host start
```

Wanneer de host functions wordt gestart, wordt er iets zoals de volgende uitvoer geschreven, die voor de Lees baarheid is afgekapt:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieer de URL van uw `HttpTrigger`-functie uit de uitvoer van de runtime en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de GET-aanvraag die door de lokale functie wordt geretourneerd, weergegeven in de browser:

![Lokaal testen in de browser](./media/functions-create-first-function-python/function-test-local-browser.png)

Nu u de functie lokaal hebt uitgevoerd, kunt u de functie-app en andere vereiste resources maken in Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Een functie-app maken in azure

Een functie-app biedt een omgeving voor het uitvoeren van de functie code. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen.

Voer in de volgende opdracht uit met daarin een unieke functie-appnaam in plaats van de tijdelijke plaatsaanduiding `<APP_NAME>` en gebruik de naam van het opslagaccount in plaats van `<STORAGE_NAME>`. De `<APP_NAME>` is ook het standaard DNS-domein voor de functie-app. Deze naam moet uniek zijn in alle apps in Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

> [!NOTE]
> Azure Functions is het verbruiks abonnement voor Linux momenteel als preview-versie en alleen beschikbaar in de volgende regio's: VS-West, VS-Oost, Europa-west, Azië-oost. Bovendien kunnen Linux-en Windows-apps niet worden gehost in dezelfde resource groep. Als u een bestaande resource groep hebt met `myResourceGroup` de naam met een Windows-functie-app of web-app, moet u een andere resource groep gebruiken.

U bent nu klaar om uw lokale functions-project te publiceren naar de functie-app in Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een python-functies project gemaakt met een door HTTP geactiveerde functie, deze uitvoeren op uw lokale computer en geïmplementeerd in Azure. Breid uw functie nu uit door...

> [!div class="nextstepaction"]
> [Een Azure Storage wachtrij-uitvoer binding toevoegen](functions-add-output-binding-storage-queue-python.md)
