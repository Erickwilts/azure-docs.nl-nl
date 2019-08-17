---
title: Python-ontwikkelaars referentie voor Azure Functions
description: Meer informatie over het ontwikkelen van functies met python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure functions, functies, gebeurtenis verwerking, dynamische compute, serverloze architectuur, python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 0cdd7f291b43f442b8471a19f515e4a2d12b4e74
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69562872"
---
# <a name="azure-functions-python-developer-guide"></a>Azure Functions python-ontwikkelaars handleiding

Dit artikel is een inleiding tot het ontwikkelen van Azure Functions met behulp van python. De onderstaande inhoud veronderstelt dat u de [hand leiding voor de Azure functions-ontwikkel aars](functions-reference.md)al hebt gelezen.

## <a name="programming-model"></a>Programmeermodel

Een Azure-functie moet een staatloze methode in uw python-script zijn dat invoer verwerkt en uitvoer produceert. Standaard verwacht de runtime dat de methode wordt geïmplementeerd als globale methode met de naam `main()` in het `__init__.py` bestand.

U kunt de standaard configuratie wijzigen door de `scriptFile` eigenschappen en `entryPoint` in het bestand *Function. json* op te geven. Met de _functie. json_ hieronder wordt bijvoorbeeld aangegeven dat de runtime de `customentry()` methode in het _Main.py_ -bestand moet gebruiken als het invoer punt voor uw Azure-functie.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Gegevens van triggers en bindingen zijn gekoppeld aan de functie via methoden Attributes met `name` behulp van de eigenschap die in het bestand *Function. json* is gedefinieerd. Met de _functie. json_ hieronder wordt bijvoorbeeld een eenvoudige functie beschreven die wordt geactiveerd door een http- `req`aanvraag met de naam:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

Het `__init__.py` bestand bevat de volgende functie code:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Als u de functies van de IntelliSense en automatisch aanvullen van de code-editor wilt benutten, kunt u ook de kenmerk typen en het retour type in de functie met behulp van python-type aantekeningen declareren. 

```python
import azure.functions


def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```

Gebruik de python-aantekeningen in het pakket [Azure. functions. *](/python/api/azure-functions/azure.functions?view=azure-python) om invoer en uitvoer aan uw methoden te koppelen.

## <a name="folder-structure"></a>Mapstructuur

De mapstructuur voor een python functions-project ziet er als volgt uit:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
```

Er is een gedeeld [host. json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen code bestand en een bindings configuratie bestand (function. json). 

Gedeelde code moet in een afzonderlijke map worden bewaard. Als u wilt verwijzen naar modules in de map SharedCode, kunt u de volgende syntaxis gebruiken:

```
from __app__.SharedCode import myFirstHelperFunction
```

Wanneer u een functie project implementeert in uw functie-app in azure, moet de volledige inhoud van de map *FunctionApp* worden opgenomen in het pakket, maar niet in de map zelf.

## <a name="triggers-and-inputs"></a>Triggers en invoer

Invoer wordt onderverdeeld in twee categorieën in Azure Functions: invoer van trigger en aanvullende invoer. Hoewel ze verschillen in het bestand `function.json` , is het gebruik identiek in de python-code.  Verbindings reeksen of geheimen voor trigger-en invoer bronnen worden toegewezen aan waarden `local.settings.json` in het bestand tijdens het uitvoeren van lokaal en de toepassings instellingen wanneer ze worden uitgevoerd in Azure. 

De volgende code toont bijvoorbeeld het verschil tussen de twee:

```json
// function.json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Wanneer de functie wordt aangeroepen, wordt de HTTP-aanvraag door gegeven aan de functie `req`als. Er wordt een item opgehaald uit de Azure-Blob Storage op basis van de id in de route _-_ URL en beschikbaar `obj` gemaakt als in de hoofd tekst van de functie.  Hier is het opgegeven opslag account het Connection String gevonden `AzureWebJobsStorage` dat hetzelfde opslag account is dat wordt gebruikt door de functie-app.


## <a name="outputs"></a>outputs

Output kan worden uitgedrukt in retour waarde en uitvoer parameters. Als er slechts één uitvoer is, raden we u aan de retour waarde te gebruiken. Voor meerdere uitvoer moet u uitvoer parameters gebruiken.

Als u de retour waarde van een functie als de waarde van een uitvoer binding wilt gebruiken `name` , moet u de eigenschap van de binding `$return` instellen `function.json`op in.

Als u meerdere uitvoer wilt maken, gebruikt `set()` u de methode van [`azure.functions.Out`](/python/api/azure-functions/azure.functions.out?view=azure-python) de interface om een waarde toe te wijzen aan de binding. Met de volgende functie kan bijvoorbeeld een bericht naar een wachtrij worden gepusht en wordt ook een HTTP-antwoord geretourneerd.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func


def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Logboekregistratie

Toegang tot de runtime logger van Azure functions is beschikbaar via een [`logging`](https://docs.python.org/3/library/logging.html#module-logging) basis-handler in uw functie-app. Deze logboek registratie is gekoppeld aan Application Insights en stelt u in staat om waarschuwingen en fouten te markeren die tijdens de uitvoering van de functie zijn aangetroffen.

In het volgende voor beeld wordt een info bericht geregistreerd wanneer de functie wordt aangeroepen via een HTTP-trigger.

```python
import logging


def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Er zijn aanvullende logboek registratie methoden beschikbaar waarmee u naar de-console kunt schrijven op verschillende tracerings niveaus:

| Methode                 | Description                                |
| ---------------------- | ------------------------------------------ |
| Userenv. **kritiek (_bericht_)**   | Schrijft een bericht met niveau kritiek op de hoofd logboek registratie.  |
| Userenv. **fout (_bericht_)**   | Schrijft een bericht met een niveau fout in de hoofd logboek registratie.    |
| Userenv. **waarschuwing (_bericht_)**    | Schrijft een bericht met niveau waarschuwing in de hoofd logboek registratie.  |
| Userenv. **info (_bericht_)**    | Schrijft een bericht met niveau-informatie in de hoofd logboek registratie.  |
| Userenv. **debug (_bericht_)** | Schrijft een bericht met niveau DEBUG op de hoofd logboek registratie.  |

## <a name="async"></a>Asynchroon

We raden u aan om uw Azure-functie te schrijven als asynchrone coroutine `async def` met behulp van de-instructie.

```python
# Will be run with asyncio directly


async def main():
    await some_nonblocking_socket_io_op()
```

Als de Main ()-functie synchroon is ( `async` geen kwalificatie), wordt de functie automatisch uitgevoerd `asyncio` in een thread groep.

```python
# Would be run in an asyncio thread-pool


def main():
    some_blocking_socket_io()
```

## <a name="context"></a>Context

Als u de aanroep context van een functie tijdens de uitvoering wilt ophalen, [`context`](/python/api/azure-functions/azure.functions.context?view=azure-python) neemt u het argument op in de hand tekening. 

Bijvoorbeeld:

```python
import azure.functions


def main(req: azure.functions.HttpRequest,
         context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

De [**context**](/python/api/azure-functions/azure.functions.context?view=azure-python) klasse heeft de volgende methoden:

`function_directory`  
De map waarin de functie wordt uitgevoerd.

`function_name`  
De naam van de functie.

`invocation_id`  
ID van de huidige functie aanroep.

## <a name="global-variables"></a>Globale variabelen

Het is niet zeker dat de status van uw app wordt bewaard voor toekomstige uitvoeringen. De Azure Functions runtime gebruikt echter vaak hetzelfde proces voor meerdere uitvoeringen van dezelfde app. Als u de resultaten van een dure berekening in de cache wilt opslaan, declareert u deze als een globale variabele. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="python-version-and-package-management"></a>Python-versie en pakket beheer

Momenteel ondersteunt Azure Functions alleen python 3.6. x (officiële CPython-distributie).

Wanneer u lokaal ontwikkelt met behulp van de Azure functions core tools of Visual Studio code, voegt u de namen en versies van `requirements.txt` de vereiste pakketten toe aan `pip`het bestand en installeert u deze met behulp van.

De volgende vereisten bestand en PIP-opdracht kunnen bijvoorbeeld worden gebruikt om het `requests` pakket te installeren vanuit PyPI.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Publiceren naar Azure

Wanneer u klaar bent om te publiceren, moet u ervoor zorgen dat al uw afhankelijkheden worden weer gegeven in het bestand *Requirements. txt* , dat zich in de hoofdmap van de projectmap bevindt. Als u een pakket gebruikt dat een compiler vereist en de installatie van manylinux-compatibele wielen van PyPI niet ondersteunt, mislukt het publiceren naar Azure met de volgende fout: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Als u automatisch de vereiste binaire bestanden wilt bouwen en configureren, [installeert](https://docs.docker.com/install/) u docker op uw lokale computer en voert u de volgende opdracht uit om te publiceren met behulp van de [Azure functions core tools](functions-run-local.md#v2) (func). Vervang `<app name>` door de naam van uw functie-app in Azure. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Onder de kaften gebruiken basis Hulpprogramma's docker om de [MCR.Microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) -installatie kopie uit te voeren als een container op uw lokale machine. Als u deze omgeving gebruikt, bouwt en installeert u de vereiste modules van de bron distributie, voordat u ze oppakt voor de uiteindelijke implementatie naar Azure.

Als u uw afhankelijkheden wilt maken en publiceren met behulp van een systeem voor continue levering (CD), [gebruikt u Azure DevOps-pijp lijnen](functions-how-to-azure-devops.md). 

## <a name="unit-testing"></a>Eenheids tests

Functies die zijn geschreven in python kunnen worden getest als andere python-code met behulp van standaard test raamwerken. Voor de meeste bindingen is het mogelijk om een invoer object voor een model te maken door een instantie van een geschikte klasse `azure.functions` te maken vanuit het pakket. Omdat het [`azure.functions`](https://pypi.org/project/azure-functions/) pakket niet onmiddellijk beschikbaar is, moet u het installeren via uw bestand `requirements.txt` , zoals beschreven in de sectie [python-versie en pakket beheer](#python-version-and-package-management) hierboven.

Het volgende is bijvoorbeeld een model test van een door HTTP geactiveerde functie:

```json
{
  "scriptFile": "httpfunc.py",
  "entryPoint": "my_function",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

```python
# myapp/httpfunc.py
import azure.functions as func
import logging

def my_function(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

```python
# myapp/test_httpfunc.py
import unittest

import azure.functions as func
from httpfunc import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/HttpTrigger',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'Hello Test',
        )
```

Hier volgt nog een voor beeld van een door de wachtrij geactiveerde functie:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
```

## <a name="known-issues-and-faq"></a>Bekende problemen en veelgestelde vragen

Alle bekende problemen en functie aanvragen worden bijgehouden met de lijst met [github-problemen](https://github.com/Azure/azure-functions-python-worker/issues) . Als u een probleem ondervindt en u het probleem niet kunt vinden in GitHub, opent u een nieuw probleem en voegt u een gedetailleerde beschrijving van het probleem toe.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Documentatie over de API voor Azure Functions-pakketten](/python/api/azure-functions/azure.functions?view=azure-python)
* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Azure Functions triggers en bindingen](functions-triggers-bindings.md)
* [Blob Storage-bindingen](functions-bindings-storage-blob.md)
* [HTTP-en webhook-bindingen](functions-bindings-http-webhook.md)
* [Wachtrij opslag bindingen](functions-bindings-storage-queue.md)
* [Timertrigger](functions-bindings-timer.md)
