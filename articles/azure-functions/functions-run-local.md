---
title: Werken met Azure Functions Core Tools | Microsoft Docs
description: Leer hoe u Azure functions kunt coderen en testen vanaf de opdracht prompt of Terminal op uw lokale computer voordat u deze op Azure Functions uitvoert.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: glenga
ms.custom: 80e4ff38-5174-43
ms.openlocfilehash: cee91de9e0a5d75be258705f636248a6a6213664
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444053"
---
# <a name="work-with-azure-functions-core-tools"></a>Werken met Azure Functions Core Tools

Met Azure Functions Core Tools kunt u uw functies op de lokale computer ontwikkelen en testen vanaf de opdracht prompt of Terminal. Uw lokale functies kunnen verbinding maken met Live Azure-Services, en u kunt fouten opsporen in uw functies op uw lokale computer met behulp van de volledige functions-runtime. U kunt zelfs een functie-app implementeren in uw Azure-abonnement.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Het ontwikkelen van functies op uw lokale computer en het publiceren van deze naar Azure met behulp van kern Hulpprogramma's volgt deze basis stappen:

> [!div class="checklist"]
> * [Installeer de kern Hulpprogramma's en-afhankelijkheden.](#v2)
> * [Een functie-app-project maken op basis van een taalspecifieke sjabloon.](#create-a-local-functions-project)
> * [Registreer trigger-en binding-extensies.](#register-extensions)
> * [Geef opslag en andere verbindingen op.](#local-settings-file)
> * [Een functie maken op basis van een trigger en een taalspecifieke sjabloon.](#create-func)
> * [De functie lokaal uitvoeren](#start)
> * [Het project publiceren naar Azure](#publish)

## <a name="core-tools-versions"></a>Kern Hulpprogramma's versies

Er zijn twee versies van Azure Functions Core Tools. Welke versie u gebruikt, is afhankelijk van uw lokale ontwikkel omgeving, de [keuze van de taal](supported-languages.md)en het vereiste ondersteunings niveau:

+ Versie 1. x: ondersteunt versie 1. x van de runtime. Deze versie van de hulpprogram ma's wordt alleen ondersteund op Windows-computers en wordt geïnstalleerd vanuit een [NPM-pakket](https://docs.npmjs.com/getting-started/what-is-npm). Met deze versie kunt u functies maken in experimentele talen die niet officieel worden ondersteund. Zie [ondersteunde talen in azure functions](supported-languages.md) voor meer informatie.

+ [Versie 2. x](#v2): ondersteunt [versie 2. x van de runtime](functions-versions.md). Deze versie biedt ondersteuning voor [Windows](#windows-npm), [macOS](#brew)en [Linux](#linux). Maakt gebruik van platformspecifieke pakket beheerders of NPM voor installatie.

Tenzij anders vermeld, zijn de voor beelden in dit artikel van versie 2. x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure functions core tools] bevat een versie van dezelfde runtime die voorziet in azure functions runtime die u kunt uitvoeren op uw lokale ontwikkel computer. Het bevat ook opdrachten voor het maken van functies, verbinding maken met Azure en functie projecten implementeren.

### <a name="v2"></a>Versie 2. x

Versie 2. x van de hulpprogram ma's maakt gebruik van de Azure Functions runtime 2. x die is gebouwd op .NET core. Deze versie wordt ondersteund op alle platformen .NET Core 2. x ondersteunt, waaronder [Windows](#windows-npm), [macOS](#brew)en [Linux](#linux). 

> [!IMPORTANT]
> U kunt de vereiste voor het installeren van de .NET Core 2. x SDK overs Laan door gebruik te maken van [uitbreidings bundels].

#### <a name="windows-npm"></a>Windows

In de volgende stappen wordt NPM gebruikt om de belangrijkste Hulpprogram Ma's in Windows te installeren. U kunt ook [Choco lade](https://chocolatey.org/)gebruiken. Zie het Leesmij-bestand met [belangrijkste Hulpprogram ma's](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows)voor meer informatie.

1. Installeer [Node.js], dat NPM omvat. Voor versie 2. x van de hulpprogram ma's worden alleen node. js 8,5 en latere versies ondersteund.

1. Installeer het pakket met kern Hulpprogramma's:

    ```bash
    npm install -g azure-functions-core-tools
    ```

   Het kan enkele minuten duren voordat NPM het pakket met kern Hulpprogramma's downloadt en installeert.

1. Als u geen [uitbreidings bundels]wilt gebruiken, installeert u de [.net Core 2. x SDK voor Windows](https://www.microsoft.com/net/download/windows).

#### <a name="brew"></a>MacOS met Homebrew

In de volgende stappen wordt homebrew gebruikt om de belangrijkste Hulpprogram Ma's voor macOS te installeren.

1. Installeer [homebrew](https://brew.sh/)als dit nog niet is gebeurd.

1. Installeer het pakket met kern Hulpprogramma's:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools
    ```

1. Als u geen [uitbreidings bundels]wilt gebruiken, installeert u [.net Core 2. x SDK voor macOS](https://www.microsoft.com/net/download/macos).


#### <a name="linux"></a>Linux (Ubuntu/Debian) met APT

De volgende stappen gebruiken [apt](https://wiki.debian.org/Apt) om kern hulpprogramma's te installeren op uw Ubuntu/Debian Linux-distributie. Raadpleeg het Leesmij-bestand voor de [belangrijkste Hulpprogram ma's](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux)voor andere Linux-distributies.

1. Registreer de product code van micro soft als vertrouwd:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

1. Controleer of op uw Ubuntu-server een van de juiste versies uit de onderstaande tabel wordt uitgevoerd. Voer de volgende handelingen uit om de apt-bron toe te voegen:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux-distributie | Version |
    | --------------- | ----------- |
    | Ubuntu 18,10    | `cosmic`    |
    | Ubuntu 18.04    | `bionic`    |
    | Ubuntu 17,04    | `zesty`     |
    | Ubuntu 16.04/Linux Mint 18    | `xenial`  |

1. Installeer het pakket met kern Hulpprogramma's:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

1. Als u niet van plan bent om [uitbreidings bundels]te gebruiken, installeert u [.net Core 2. x SDK voor Linux](https://www.microsoft.com/net/download/linux).

## <a name="create-a-local-functions-project"></a>Een lokaal Functions-project maken

Een project directory functions bevat de bestanden [host. json](functions-host-json.md) en [Local. settings. json](#local-settings-file), samen met submappen die de code voor afzonderlijke functies bevatten. Deze map is het equivalent van een functie-app in Azure. Zie de [hand leiding voor de Azure functions-ontwikkel aars](functions-reference.md#folder-structure)voor meer informatie over de mapstructuur van de functies.

Versie 2. x vereist dat u een standaard taal voor het project selecteert wanneer deze wordt geïnitialiseerd en alle functies worden toegevoegd standaard taal sjablonen gebruiken. In versie 1. x geeft u de taal op telkens wanneer u een functie maakt.

Voer in het Terminal venster of vanaf een opdracht prompt de volgende opdracht uit om het project en de lokale Git-opslag plaats te maken:

```bash
func init MyFunctionProj
```

Wanneer u een project naam opgeeft, wordt een nieuwe map met die naam gemaakt en geïnitialiseerd. Anders wordt de huidige map geïnitialiseerd.  
In versie 2. x, wanneer u de opdracht uitvoert, moet u een runtime voor uw project kiezen. Als u van plan bent java script-functies te ontwikkelen, kiest u **knoop punt**:

```output
Select a worker runtime:
dotnet
node
```

Gebruik de pijl omhoog/omlaag om een taal te kiezen en druk vervolgens op ENTER. De uitvoer ziet eruit als in het volgende voor beeld voor een Java script-project:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

`func init`biedt ondersteuning voor de volgende opties: versie 2. x-only, tenzij anders vermeld.

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--csx`** | Initialiseert een C# script-project (. CSX). U moet in `--csx` volgende opdrachten opgeven. |
| **`--docker`** | Maak een Dockerfile voor een container met behulp van een basis installatie kopie op basis `--worker-runtime`van de gekozen. Gebruik deze optie wanneer u van plan bent om te publiceren naar een aangepaste Linux-container. |
| **`--force`** | Initialiseer het project zelfs wanneer er bestaande bestanden in het project aanwezig zijn. Deze instelling overschrijft bestaande bestanden met dezelfde naam. Andere bestanden in de projectmap worden niet beïnvloed. |
| **`--no-source-control -n`** | Hiermee wordt voor komen dat een Git-opslag plaats standaard wordt gemaakt in versie 1. x. In versie 2. x wordt de Git-opslag plaats standaard niet gemaakt. |
| **`--source-control`** | Hiermee wordt bepaald of een Git-opslag plaats wordt gemaakt. Een opslag plaats wordt standaard niet gemaakt. Wanneer `true`wordt een opslag plaats gemaakt. |
| **`--worker-runtime`** | Hiermee stelt u de taal runtime voor het project in. Ondersteunde waarden zijn `dotnet`, `node` (Java script) `java`, en `python`. Wanneer deze niet is ingesteld, wordt u gevraagd uw runtime te kiezen tijdens de initialisatie. |

> [!IMPORTANT]
> Versie 2. x van de kern hulpprogramma's maakt standaard functie-app-projecten voor .NET runtime als [ C# class-projecten](functions-dotnet-class-library.md) (. csproj). Deze C# projecten, die kunnen worden gebruikt met Visual Studio of Visual Studio code, worden gecompileerd tijdens het testen en bij het publiceren naar Azure. Als u in plaats daarvan dezelfde C# script bestanden (. CSX) wilt maken en gebruiken die zijn gemaakt in versie 1. x en in de portal, moet u de `--csx` para meter toevoegen wanneer u functies maakt en implementeert.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Deze instellingen worden standaard niet automatisch gemigreerd wanneer het project wordt gepubliceerd naar Azure. Gebruik de `--publish-local-settings` switch [Wanneer u publiceert](#publish) om ervoor te zorgen dat deze instellingen worden toegevoegd aan de functie-app in Azure. Houd er rekening mee dat waarden in **Connections Tring** nooit worden gepubliceerd.

De waarden van de functie-app-instellingen kunnen ook in uw code worden gelezen als omgevings variabelen. Zie de sectie omgevings variabelen van deze taalspecifieke naslag onderwerpen voor meer informatie:

* [C#vooraf gecompileerde](functions-dotnet-class-library.md#environment-variables)
* [C# script (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

Als er geen geldige opslag Connection String is ingesteld [`AzureWebJobsStorage`] voor en de emulator niet wordt gebruikt, wordt het volgende fout bericht weer gegeven:

> Ontbrekende waarde voor AzureWebJobsStorage in Local. settings. json. Dit is vereist voor alle triggers behalve HTTP. U kunt func Azure functionapp fetch-app-Settings \<functionAppName\>uitvoeren of een Connection String in Local. settings. json opgeven.

### <a name="get-your-storage-connection-strings"></a>Uw opslag verbindings reeksen ophalen

Zelfs wanneer u de opslag emulator gebruikt voor ontwikkeling, wilt u wellicht testen met een echte opslag verbinding. Ervan uitgaande dat u al [een opslag account hebt gemaakt](../storage/common/storage-create-storage-account.md), kunt u op een van de volgende manieren een geldig opslag Connection String krijgen:

+ Van de [Azure-portal]. Navigeer naar uw opslag account, selecteer **toegangs sleutels** in **instellingen**en kopieer een van de **verbindings reeks** waarden.

  ![connection string kopiëren van Azure Portal](./media/functions-run-local/copy-storage-connection-portal.png)

+ Gebruik [Azure Storage Explorer](https://storageexplorer.com/) om verbinding te maken met uw Azure-account. Vouw in de **Explorer**uw abonnement uit, selecteer uw opslag account en kopieer de primaire of secundaire Connection String.

  ![connection string kopiëren van Storage Explorer](./media/functions-run-local/storage-explorer.png)

+ Gebruik de basis Hulpprogramma's om de connection string te downloaden van Azure met een van de volgende opdrachten:

  + Alle instellingen van een bestaande functie-app downloaden:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
  + De verbindings reeks ophalen voor een specifiek opslag account:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```

    Wanneer u nog niet bent aangemeld bij Azure, wordt u gevraagd dit te doen.

## <a name="create-func"></a>Een functie maken

Voer de volgende opdracht uit om een functie te maken:

```bash
func new
```

In versie 2. x, wanneer u uitvoert `func new` , wordt u gevraagd een sjabloon in de standaard taal van uw functie-app te kiezen. vervolgens wordt u gevraagd om een naam voor de functie te kiezen. In versie 1. x wordt u ook gevraagd om de taal te kiezen.

```output
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
```

Functie code wordt gegenereerd in een submap met de naam van de geleverde functie, zoals u kunt zien in de volgende uitvoer van de wachtrij trigger:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

U kunt deze opties ook opgeven in de opdracht met behulp van de volgende argumenten:

| Argument     | Description                            |
| ------------------------------------------ | -------------------------------------- |
| **`--csx`** | (Versie 2. x) Hiermee worden dezelfde C# script-sjablonen (. CSX) gegenereerd die worden gebruikt in versie 1. x en in de portal. |
| **`--language -l`**| De programmeer taal van de sjabloon, C#zoals F#, of Java script. Deze optie is vereist in versie 1. x. In versie 2. x gebruikt u deze optie niet of kiest u een taal die overeenkomt met de runtime van de werk nemer. |
| **`--name -n`** | De functie naam. |
| **`--template -t`** | Gebruik de `func templates list` opdracht om de volledige lijst met beschik bare sjablonen voor elke ondersteunde taal weer te geven.   |

Als u bijvoorbeeld een Java script-trigger in één opdracht wilt maken, voert u het volgende uit:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Als u een in een wachtrij geactiveerde functie wilt maken in één opdracht, voert u het volgende uit:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Functies lokaal uitvoeren

Als u een functions-project wilt uitvoeren, voert u de host functions uit. De host schakelt triggers in voor alle functies in het project:

```bash
func host start
```

De `host` opdracht is alleen vereist in versie 1. x.

`func host start`biedt ondersteuning voor de volgende opties:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--no-build`** | Pas het huidige project niet samen om uit te voeren. Alleen voor dotnet-projecten. De standaard waarde is ingesteld op ONWAAR. Alleen versie 2. x. |
| **`--cert`** | Het pad naar een pfx-bestand dat een persoonlijke sleutel bevat. Alleen gebruikt met `--useHttps`. Alleen versie 2. x. |
| **`--cors-credentials`** | Alleen-lezen geverifieerde aanvragen (cookies en de verificatie header) versie 2. x toestaan. |
| **`--cors`** | Een door komma's gescheiden lijst met CORS-oorsprong, zonder spaties. |
| **`--language-worker`** | Argumenten voor het configureren van de taal medewerker. Alleen versie 2. x. |
| **`--nodeDebugPort -n`** | De poort voor het fout opsporingsprogramma van het knoop punt dat moet worden gebruikt. Standaard: Een waarde van Launch. json of 5858. Alleen versie 1. x. |
| **`--password`** | Ofwel het wacht woord of een bestand dat het wacht woord voor een pfx-bestand bevat. Alleen gebruikt met `--cert`. Alleen versie 2. x. |
| **`--port -p`** | De lokale poort waarop moet worden geluisterd. Standaard waarde: 7071. |
| **`--pause-on-error`** | Wacht op extra invoer voordat het proces wordt afgesloten. Wordt alleen gebruikt bij het starten van kern Hulpprogramma's van een Integrated Development Environment (IDE).|
| **`--script-root --prefix`** | Hiermee geeft u het pad op naar de hoofdmap van de functie-app die moet worden uitgevoerd of geïmplementeerd. Dit wordt gebruikt voor gecompileerde projecten die Project bestanden in een submap genereren. Wanneer u bijvoorbeeld een C# klassen bibliotheek project bouwt, worden de bestanden host. json, local. settings. json en function. json gegenereerd in een *hoofdmap* met een pad zoals. `MyProject/bin/Debug/netstandard2.0` In dit geval stelt u het voor voegsel `--script-root MyProject/bin/Debug/netstandard2.0`in als. Dit is de basis van de functie-app wanneer deze wordt uitgevoerd in Azure. |
| **`--timeout -t`** | De time-out voor het starten van de functions-host, in seconden. Standaard: 20 seconden.|
| **`--useHttps`** | Maak een `https://localhost:{port}` binding aan in `http://localhost:{port}`plaats van aan. Met deze optie wordt standaard een vertrouwd certificaat op uw computer gemaakt.|

Voor een C# Class Library-project (. csproj) moet u de `--build` optie voor het genereren van de bibliotheek. dll toevoegen.

Wanneer de functie host wordt gestart, wordt de URL van de met HTTP geactiveerde functies uitgevoerd:

```output
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

>[!IMPORTANT]
>Bij lokaal uitvoeren wordt verificatie niet afgedwongen voor HTTP-eind punten. Dit betekent dat alle lokale HTTP-aanvragen worden verwerkt `authLevel = "anonymous"`als. Zie het artikel over de [http-binding](functions-bindings-http-webhook.md#authorization-keys)voor meer informatie.

### <a name="passing-test-data-to-a-function"></a>Test gegevens door geven aan een functie

Als u uw functies lokaal wilt testen, [start u de functies host](#start) en roept u eind punten aan op de lokale server met behulp van HTTP-aanvragen. Het eind punt dat u aanroept, is afhankelijk van het type functie.

>[!NOTE]
> Voor beelden in dit onderwerp gebruiken het krul hulp programma voor het verzenden van HTTP-aanvragen van de terminal of een opdracht prompt. U kunt een hulp programma van uw keuze gebruiken om HTTP-aanvragen naar de lokale server te verzenden. Het krul hulp programma is standaard beschikbaar op Linux-systemen. In Windows moet u eerst het [krul hulp programma](https://curl.haxx.se/)downloaden en installeren.

Zie [strategieën voor het testen van uw code in azure functions](functions-test-a-function.md)voor meer algemene informatie over het testen van functies.

#### <a name="http-and-webhook-triggered-functions"></a>Geactiveerde HTTP-en webhook-functies

U roept het volgende eind punt aan om HTTP en door de webhook geactiveerde functies lokaal uit te voeren:

    http://localhost:{port}/api/{function_name}

Zorg ervoor dat u dezelfde server naam en poort gebruikt als waarop de host van de functies luistert. U ziet dit in de uitvoer die wordt gegenereerd bij het starten van de host van de functie. U kunt deze URL aanroepen met de HTTP-methode die wordt ondersteund door de trigger.

Met de volgende krul opdracht wordt `MyHttpTrigger` de Quick Start-functie geactiveerd vanuit een GET-aanvraag met de para meter _name_ door gegeven in de query teken reeks.

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```

Het volgende voor beeld is dezelfde functie die wordt aangeroepen vanuit een POST-aanvraag voor het door geven van een _naam_ in de hoofd tekst van de aanvraag:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

U kunt GET-aanvragen indienen vanuit een browser waarmee gegevens worden door gegeven in de query teken reeks. Voor alle andere HTTP-methoden moet u krul, Fiddler, Postman of een soortgelijk hulp programma voor HTTP-tests gebruiken.

#### <a name="non-http-triggered-functions"></a>Niet-HTTP-geactiveerde functies

Voor alle soorten functies behalve HTTP-triggers en webhooks kunt u uw functies lokaal testen door een beheer eindpunt aan te roepen. Als dit eind punt wordt aangeroepen met een HTTP POST-aanvraag op de lokale server, wordt de functie geactiveerd. U kunt eventueel test gegevens door geven aan de uitvoering in de hoofd tekst van de POST-aanvraag. Deze functionaliteit is vergelijkbaar met het tabblad **testen** in de Azure Portal.

U roept het volgende Administrator-eind punt aan om niet-HTTP-functies te activeren:

    http://localhost:{port}/admin/functions/{function_name}

Als u test gegevens wilt door geven aan het eind punt van de beheerder van een functie, moet u de gegevens opgeven in de hoofd tekst van een bericht van een POST-aanvraag. De hoofd tekst van het bericht moet de volgende JSON-indeling hebben:

```JSON
{
    "input": "<trigger_input>"
}
```

De `<trigger_input>` waarde bevat gegevens in een indeling die door de functie wordt verwacht. Het volgende krul-voor beeld is een post `QueueTriggerJS` naar een functie. In dit geval is de invoer een teken reeks die overeenkomt met het bericht dat naar verwachting in de wachtrij wordt gevonden.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Gebruik de `func run` opdracht in versie 1. x

>[!IMPORTANT]
> De `func run` opdracht wordt niet ondersteund in versie 2. x van de hulpprogram ma's. Zie het onderwerp [How to Azure functions runtime-versies](set-runtime-version.md)voor meer informatie.

U kunt ook rechtstreeks een functie aanroepen met `func run <FunctionName>` behulp van en invoer gegevens opgeven voor de functie. Deze opdracht is vergelijkbaar met het uitvoeren van een functie met behulp van het tabblad **testen** in de Azure Portal.

`func run`biedt ondersteuning voor de volgende opties:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inline-inhoud. |
| **`--debug -d`** | Koppel een fout opsporingsprogramma aan het hostproces voordat u de functie uitvoert.|
| **`--timeout -t`** | De tijd (in seconden) die moet worden gewacht totdat de lokale functions-host gereed is.|
| **`--file -f`** | De bestands naam die moet worden gebruikt als inhoud.|
| **`--no-interactive`** | Er wordt niet gevraagd om invoer. Handig voor automatiserings scenario's.|

Als u bijvoorbeeld een door HTTP geactiveerde functie wilt aanroepen en de hoofd tekst van de inhoud wilt door geven, voert u de volgende opdracht uit:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Publiceren naar Azure

De Azure Functions Core Tools ondersteunt twee typen implementaties: functie project bestanden rechtstreeks implementeren in uw functie-app via [zip-implementatie](functions-deployment-technologies.md#zip-deploy) en [een aangepaste docker-container implementeren](functions-deployment-technologies.md#docker-container). U moet al [een functie-app hebben gemaakt in uw Azure-abonnement](functions-cli-samples.md#create), waar u uw code gaat implementeren. Projecten waarvoor compilatie vereist is, moeten worden gebouwd zodat de binaire bestanden kunnen worden geïmplementeerd.

### <a name="project-file-deployment"></a>Implementatie (project bestanden)

Als u uw lokale code naar een functie-app in azure wilt publiceren `publish` , gebruikt u de opdracht:

```bash
func azure functionapp publish <FunctionAppName>
```

Met deze opdracht wordt gepubliceerd naar een bestaande functie-app in Azure. U krijgt een fout melding als u probeert te publiceren naar een `<FunctionAppName>` die niet voor komt in uw abonnement. Zie [een functie-app maken voor serverloze uitvoering voor](./scripts/functions-cli-create-serverless.md)meer informatie over het maken van een functie-app vanuit de opdracht prompt of het Terminal venster met behulp van de Azure cli. Met deze opdracht wordt de app standaard geïmplementeerd voor [uitvoering vanuit het implementatie pakket](run-functions-from-deployment-package.md). Als u deze aanbevolen implementatie modus wilt uitschakelen, `--nozip` gebruikt u de optie.

>[!IMPORTANT]
> Wanneer u een functie-app maakt in de Azure Portal, gebruikt deze standaard versie 2. x van de functie-runtime. Volg de instructies in [uitvoeren op versie 1. x](functions-versions.md#creating-1x-apps)om de functie-app versie 1. x van de runtime te laten gebruiken.
> U kunt de runtime versie niet wijzigen voor een functie-app met bestaande functies.

De volgende publicatie opties zijn van toepassing voor beide versies, 1. x en 2. x:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publiceer instellingen in Local. settings. json naar Azure en vraag om te worden overschreven als de instelling al bestaat. Als u de opslag emulator gebruikt, wijzigt u eerst de app-instelling in een [echte opslag verbinding](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | De prompt voor het overschrijven van app- `--publish-local-settings -i` instellingen onderdrukken wanneer deze wordt gebruikt.|

De volgende publicatie opties worden alleen ondersteund in versie 2. x:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--publish-settings-only -o`** |  Publiceer instellingen alleen en sla de inhoud over. De standaard instelling is prompt. |
|**`--list-ignored-files`** | Geeft een lijst weer van bestanden die worden genegeerd tijdens het publiceren, dat is gebaseerd op het. funcignore-bestand. |
| **`--list-included-files`** | Geeft een lijst weer van bestanden die zijn gepubliceerd, die zijn gebaseerd op het. funcignore-bestand. |
| **`--nozip`** | Hiermee schakelt u `Run-From-Package` de standaard modus uit. |
| **`--build-native-deps`** | Slaat de map voor het genereren van wielen op tijdens het publiceren van python-functie-apps. |
| **`--additional-packages`** | Lijst met pakketten die moeten worden geïnstalleerd bij het bouwen van systeem eigen afhankelijkheden. Bijvoorbeeld: `python3-dev libevent-dev`. |
| **`--force`** | Verificatie vooraf publiceren in bepaalde scenario's negeren. |
| **`--csx`** | Publiceer een C# script-project (. CSX). |
| **`--no-build`** | Sla het bouwen van DOTNET-functies over. |
| **`--dotnet-cli-params`** | Bij het publiceren C# van gecompileerde (. csproj)-functies roept de kern hulpprogramma's ' DotNet build--output bin/Publish ' aan. Alle para meters die aan dit worden door gegeven, worden toegevoegd aan de opdracht regel. |

### <a name="deployment-custom-container"></a>Implementatie (aangepaste container)

Met Azure Functions kunt u uw functie project implementeren in een [aangepaste docker-container](functions-deployment-technologies.md#docker-container). Zie [een functie maken in Linux met behulp van een aangepaste installatie kopie](functions-create-function-linux-custom-image.md)voor meer informatie. Aangepaste containers moeten een Dockerfile hebben. Als u een app met een Dockerfile wilt maken, gebruikt u de optie- `func init`Dockerfile op.

```bash
func deploy
```

De volgende opties voor de implementatie van de aangepaste container zijn beschikbaar:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--registry`** | De naam van een docker-REGI ster waarmee de huidige gebruiker zich heeft aangemeld. |
| **`--platform`** | Hosting platform voor de functie-app. Geldige opties zijn`kubernetes` |
| **`--name`** | Naam van de functie-app. |
| **`--max`**  | Hiermee stelt u eventueel het maximum aantal functie-app-exemplaren in dat moet worden geïmplementeerd. |
| **`--min`**  | Hiermee stelt u optioneel het minimum aantal functie-app-exemplaren in dat moet worden geïmplementeerd. |
| **`--config`** | Hiermee stelt u een optioneel implementatie configuratie bestand in. |

## <a name="monitoring-functions"></a>Bewakings functies

De aanbevolen manier om de uitvoering van uw functies te controleren is door te integreren met Azure-toepassing Insights. Wanneer u een functie-app maakt in de Azure Portal, wordt deze integratie standaard voor u uitgevoerd. Wanneer u echter uw functie-app maakt met behulp van Azure CLI, wordt de integratie in uw functie-app in azure niet uitgevoerd.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Zie [Azure functions bewaken](functions-monitoring.md)voor meer informatie.
## <a name="next-steps"></a>Volgende stappen

Azure Functions Core Tools is [open source en wordt gehost op github](https://github.com/azure/azure-functions-cli).  
[Open een github-probleem](https://github.com/azure/azure-functions-cli/issues)als u een fout of een functie aanvraag wilt indienen.

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
[`FUNCTIONS_WORKER_RUNTIME`]: functions-app-settings.md#functions_worker_runtime
[`AzureWebJobsStorage`]: functions-app-settings.md#azurewebjobsstorage
[uitbreidings bundels]: functions-bindings-register.md#extension-bundles
