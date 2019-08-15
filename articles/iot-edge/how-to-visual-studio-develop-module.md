---
title: Modules ontwikkelen en fouten opsporen in Visual Studio-Azure IoT Edge | Microsoft Docs
description: Visual Studio 2019 gebruiken voor het ontwikkelen en opsporen van fouten in modules voor Azure IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 07/22/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7296760a177b949d2f921e11b28105ca6eb67fee
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036280"
---
# <a name="use-visual-studio-2019-to-develop-and-debug-modules-for-azure-iot-edge"></a>Visual Studio 2019 gebruiken voor het ontwikkelen en opsporen van fouten in modules voor Azure IoT Edge

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel laat u zien hoe u Visual Studio 2019 kunt gebruiken als het belangrijkste hulp programma voor het ontwikkelen en opsporen van fouten in modules.

De Azure IoT Edge-Tools voor Visual Studio biedt de volgende voordelen:

- Maken, bewerken, bouwen, uitvoeren en fouten opsporen Azure IoT Edge oplossingen en modules op uw lokale ontwikkel computer.
- Uw oplossing Azure IoT Edge implementeren op Azure IoT Edge-apparaat via Azure IoT Hub.
- Codeer uw Azure IoT-modules in C# C of profiteer van de voor delen van de ontwikkeling van Visual Studio.
- Azure IoT Edge-apparaten en modules met de gebruikersinterface beheren.

Dit artikel laat u zien hoe u met de Azure IoT Edge-Hulpprogram Ma's voor Visual Studio 2019 uw IoT Edge modules kunt ontwikkelen. U leert ook hoe u uw project implementeren in uw Azure IoT Edge-apparaat. Visual Studio 2019 biedt momenteel ondersteuning voor modules die zijn geschreven in C C#en. De ondersteunde architecturen zijn Windows x64 en Linux x64 of ARM32. Zie [ondersteuning voor taal en architectuur](module-development.md#language-and-architecture-support)voor meer informatie over ondersteunde besturings systemen, talen en architecturen.
  
## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows als uw ontwikkelcomputer. Op Windows-computers kunt u Windows-of linux-modules ontwikkelen. Voor het ontwikkelen van Windows-modules gebruikt u een Windows-computer met versie 1809/build 17763 of hoger. Voor het ontwikkelen van linux-modules gebruikt u een Windows-computer die voldoet aan de [vereisten voor docker Desktop](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install). 

Omdat in dit artikel Visual Studio 2019 als belangrijkste ontwikkel programma wordt gebruikt, installeert u Visual Studio. Zorg ervoor dat u de **Azure-ontwikkeling** en de **ontwikkeling C++ van het bureau blad** opneemt met werk belastingen in de installatie van Visual Studio 2019. U kunt [Visual Studio 2019 wijzigen](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2019) om de vereiste werk belastingen toe te voegen.

Nadat uw Visual Studio 2019 gereed is, hebt u ook de volgende hulpprogram ma's en onderdelen nodig:

- Down load en Installeer [Azure IOT Edge-Hulpprogram ma's](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) van Visual Studio Marketplace om een IOT Edge-project te maken in visual studio 2019.

> [!TIP]
> Als u Visual Studio 2017 gebruikt, downloadt en installeert u [Azure IOT Edge-Hulpprogram ma's](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) voor VS 2017 van de Visual Studio Marketplace

- Down load en Installeer [docker Community Edition](https://docs.docker.com/install/) op uw ontwikkel computer om uw module-installatie kopieën te bouwen en uit te voeren. U moet docker CE instellen om te worden uitgevoerd in de Linux-container modus of de Windows-container modus.

- Stel uw lokale ontwikkel omgeving in op fout opsporing, uitvoering en test uw IoT Edge-oplossing door het [Azure IOT EdgeHub dev tool](https://pypi.org/project/iotedgehubdev/)te installeren. Installeer [python (2.7/3.6) en PIP](https://www.python.org/) en installeer het **iotedgehubdev** -pakket door de volgende opdracht uit te voeren in uw Terminal. Zorg ervoor dat uw versie van het Azure IoT EdgeHub Dev hulpprogramma is groter dan 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Kloon de opslag plaats en installeer de Vcpkg-bibliotheek beheerder en installeer vervolgens het **Azure-IOT-SDK-c-pakket** voor Windows.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure container Registry](https://docs.microsoft.com/azure/container-registry/) of [docker hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken.

- Als u uw module op een apparaat wilt testen, hebt u een actieve IoT-hub met ten minste één IoT Edge apparaat nodig. Volg de stappen in de Quick start voor [Linux](quickstart-linux.md) of [Windows](quickstart.md)om uw computer als IOT edge apparaat te gebruiken. Als IoT Edge daemon wordt uitgevoerd op uw ontwikkel computer, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u begint met de ontwikkeling van Visual Studio.

### <a name="check-your-tools-version"></a>Controleer uw versie van de hulpprogramma 's

1. Selecteer in het menu **extra** de optie **extensies en updates**. Vouw **geïnstalleerde >-Hulpprogram ma's** uit en u vindt **Azure IOT Edge tools** en **Cloud Explorer voor Visual Studio**.

1. Houd er rekening mee de geïnstalleerde versie. U kunt deze versie vergelijken met de meest recente versie van Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS2019), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools))

1. Als uw versie ouder is dan beschikbaar is op Visual Studio Marketplace, werkt u uw hulp middelen bij in Visual Studio, zoals wordt weer gegeven in de volgende sectie.

### <a name="update-your-tools"></a>De hulpprogramma's bijwerken

1. Vouw in het dialoog venster **extensies en updates** **updates uit > Visual Studio Marketplace**, selecteer **Azure IOT Edge tools** of **Cloud Explorer voor Visual Studio** en selecteer **Update**.

1. Nadat de hulpprogramma's-update is gedownload, sluit u Visual Studio aan de hulpprogramma's bijwerken met behulp van het installatieprogramma VSIX-trigger.

1. Selecteer in het installatie programma **OK** om te starten en **Wijzig** vervolgens de hulpprogram ma's.

1. Nadat de update is voltooid, selecteert u **sluiten** en Start Visual Studio opnieuw.

### <a name="create-an-azure-iot-edge-project"></a>Een Azure IoT Edge-project maken

De Azure IoT Edge-projectsjabloon in Visual Studio maakt een project dat kan worden geïmplementeerd op Azure IoT Edge-apparaten in Azure IoT Hub. Eerst maakt u een Azure IoT Edge oplossing en vervolgens genereert u de eerste module in die oplossing. Elke IoT Edge-oplossing kan meer dan één module bevatten.

> [!TIP]
> De IoT Edge project structuur die door Visual Studio is gemaakt, is niet hetzelfde als in Visual Studio code.

1. Zoek in het dialoog venster Nieuw project in Visual Studio **Azure IOT Edge** project op en klik op **volgende**. Voer in het venster project configuratie een naam in voor het project en geef de locatie op en selecteer vervolgens **maken**. De standaardnaam van het project is **AzureIoTEdgeApp1**.

   ![Nieuw project maken](./media/how-to-visual-studio-develop-csharp-module/create-new.png)

1. In het venster **IOT Edge toepassing en module toevoegen** selecteert u  **C# module** of **C-module** en geeft u vervolgens uw module naam en module-afbeeldings opslagplaats op. Visual Studio vult de module naam automatisch in met **localhost: 5000/< uw module naam\>** . Vervang deze door uw eigen gegevens. Als u een lokaal docker-REGI ster gebruikt voor het testen, is **localhost** prima. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldings server ziet eruit als   **_\<register\>naam_. azurecr.io**. Vervang alleen het gedeelte **localhost: 5000** van de teken reeks, zodat het uiteindelijke resultaat eruitziet als  **\< *register naam*\>. azurecr.io/ _\<de\>naam van uw module_** . De standaardnaam van de module is **IoTEdgeModule1**

   ![Toepassing en module toevoegen](./media/how-to-visual-studio-develop-csharp-module/add-application-and-module.png)

1. Selecteer **OK** om de Azure IOT EDGE oplossing te maken met een module die gebruikmaakt C# van ofwel of C.

U hebt nu het project **AzureIoTEdgeApp1. Linux. amd64** of het project **AzureIoTEdgeApp1. Windows. amd64** en ook een **IoTEdgeModule1** -project in uw oplossing. Elk **AzureIoTEdgeApp1** -project bevat `deployment.template.json` een bestand waarin de modules worden gedefinieerd die u wilt bouwen en implementeren voor uw IOT EDGE-oplossing. ook worden de routes tussen modules gedefinieerd. De standaard oplossing heeft een **SimulatedTemperatureSensor** -module en een **IoTEdgeModule1** -module. De **SimulatedTemperatureSensor** -module genereert gesimuleerde gegevens in de **IoTEdgeModule1** -module, terwijl de standaard code in de module **IoTEdgeModule1** berichten direct pipet ontvangen op Azure IOT hub.

Het **IoTEdgeModule1** -project is een .net Core 2,1-console toepassing als dit C# een module is. Het bevat vereiste docker-bestanden die u nodig hebt voor uw IoT Edge-apparaat dat wordt uitgevoerd met een Windows-container of een Linux-container. In `module.json` het bestand worden de meta gegevens van een module beschreven. De daad werkelijke module code, waarmee de Azure IOT Device SDK als afhankelijkheid wordt gebruikt, is `Program.cs` te `main.c` vinden in het bestand of.

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard module code die wordt geleverd bij de oplossing bevindt zich op **IoTEdgeModule1** > **Program.cs** (voor C#) of **Main. c** (c). De module en het `deployment.template.json` bestand zijn zo ingesteld dat u de oplossing kunt bouwen, deze naar het container register pusht en deze implementeert op een apparaat om te testen zonder dat u code hoeft te gebruiken. De module is gebouwd om invoer van een bron (in dit geval de **SimulatedTemperatureSensor** -module die gegevens simuleert) te maken en deze naar Azure IOT hub te door sluizen.

Wanneer u klaar bent om de module sjabloon aan te passen met uw eigen code, gebruikt u de [Azure IOT hub sdk's](../iot-hub/iot-hub-devguide-sdks.md) om modules te bouwen die voldoen aan de belangrijkste behoeften van IOT-oplossingen, zoals beveiliging, Apparaatbeheer en betrouw baarheid.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>Iotedgehubdev initialiseren met IoT Edge apparaat connection string

1. Kopieer de connection string van een IoT Edge apparaat vanuit de **primaire verbindings reeks** in de Visual Studio-Cloud Verkenner. Zorg ervoor dat u de connection string van een niet-edge-apparaat niet kopieert, omdat het pictogram van een IoT Edge apparaat verschilt van het pictogram van een niet-edge-apparaat.

   ![Edge-apparaat-verbindingsreeks kopiëren](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. Ga naar **hulpprogram ma's** > **Azure IOT Edge hulpprogram ma's** > **Setup IOT Edge Simulator**, plak de Connection String en klik op **OK**.

   ![Venster tekenreeks Edge-verbinding instellen openen](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. Voer de connection string uit in de eerste stap en selecteer **OK**.

> [!NOTE]
> U hoeft deze stappen slechts eenmaal op uw ontwikkel computer uit te voeren, aangezien de resultaten automatisch worden toegepast op alle volgende Azure IoT Edge oplossingen. Deze procedure kan opnieuw worden gevolgd als u naar een andere connection string moet overschakelen.

## <a name="build-and-debug-single-module"></a>Eén module samen stellen en debuggen

Normaal gesp roken wilt u elke module testen en fouten opsporen voordat u deze uitvoert binnen een volledige oplossing met meerdere modules.

1. Klik met de rechter muisknop op **IoTEdgeModule1** en selecteer **instellen als opstart project** in het context menu.

   ![Opstartproject instellen](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Druk op **F5** of klik op de onderstaande knop om de module uit te voeren. de eerste keer dat&ndash;u dit doet, kan het 10 20 seconden duren voordat u dit doet.

   ![Module uitvoeren](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Als de module is geïnitialiseerd, ziet u een .NET Core-Console-app.

   ![Module uitgevoerd](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. Stel bij het C#ontwikkelen in een onderbrekings punt `PipeMessage()` in voor de functie in **Program.cs**; als u C gebruikt, stelt `InputQueue1Callback()` u een onderbrekings punt in de functie in **Main. C**in. U kunt dit testen door een bericht te verzenden door de volgende opdracht uit te voeren in een **Git-Bash** of **WSL bash** -shell. (U kunt de `curl` opdracht niet uitvoeren vanuit een Power shell of opdracht prompt.)

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Fouten opsporen in één Module](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Het onderbrekingspunt moet worden geactiveerd. U kunt variabelen bekijken in het Visual Studio **lokalen** -venster.

   > [!TIP]
   > U kunt ook [postman](https://www.getpostman.com/) of andere API-hulpprogram ma's gebruiken om berichten te `curl`verzenden in plaats van.

1. Druk op **Ctrl + F5** of klik op de stopknop stop de foutopsporing.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Bouwen en fouten opsporen in IoT Edge-oplossing met meerdere modules

Wanneer u klaar bent met het ontwikkelen van een enkele module, wilt u mogelijk een volledige oplossing met meerdere modules uitvoeren en fouten opsporen.

1. Voeg een tweede module aan de oplossing toe door met de rechter muisknop op **AzureIoTEdgeApp1** te klikken en**nieuwe IOT Edge module** **toevoegen** > te selecteren. De standaard naam van de tweede module is **IoTEdgeModule2** en zal fungeren als een andere pipe-module.

1. Open het bestand `deployment.template.json` en u ziet dat **IoTEdgeModule2** is toegevoegd in de sectie **modules** . Vervang de **routes** sectie met de volgende stappen. Als u de module namen hebt aangepast, moet u deze namen bijwerken zodat deze overeenkomen.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

1. Klik met de rechter muisknop op **AzureIoTEdgeApp1** en selecteer **instellen als opstart project** in het context menu.

1. Maak uw onderbrekings punten en druk vervolgens op **F5** om meerdere modules tegelijk uit te voeren en fouten op te sporen. U ziet nu meerdere .NET Core Console-app-Vensters, waarbij elk venster een andere module vertegenwoordigt.

   ![Fouten opsporen in meerdere Modules](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Druk op **CTRL + F5** of selecteer de knop stoppen om de fout opsporing te stoppen.

## <a name="build-and-push-images"></a>Bouw en installatiekopieën pushen

1. Zorg ervoor dat **AzureIoTEdgeApp1** het start project is. Selecteer **fout opsporing** of **versie** als de configuratie die u wilt bouwen voor uw module installatie kopieën.

    > [!NOTE]
    > Bij het kiezen van **debug**, Visual `Dockerfile.(amd64|windows-amd64).debug` Studio wordt gebruikt voor het bouwen van docker-installatie kopieën. Dit omvat het opdrachtregelprogramma foutopsporingsprogramma van .NET Core VSDBG in uw containerinstallatiekopie tijdens het maken van deze. Voor productie klare IOT Edge modules wordt u aangeraden de **release** configuratie te gebruiken, die zonder VSDBG `Dockerfile.(amd64|windows-amd64)` wordt gebruikt.

1. Als u een persoonlijk REGI ster gebruikt, zoals Azure Container Registry, gebruikt u de volgende docker-opdracht om u aan te melden. Als u lokaal REGI ster gebruikt, kunt u [een lokaal REGI ster uitvoeren](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Als u een persoonlijk REGI ster gebruikt, zoals Azure Container Registry, moet u de aanmeldings gegevens van uw REGI ster toevoegen aan de runtime-instellingen `deployment.template.json`die in het bestand zijn gevonden. Vervang de tijdelijke aanduidingen door de werkelijke ACR-gebruikers naam, het wacht woord en de register naam.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

1. Klik met de rechter muisknop op **AzureIoTEdgeApp1** en selecteer **Build and push Edge Solution** om de docker-installatie kopie voor elke module te bouwen en te pushen.

   ![Bouw en installatiekopieën pushen](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)

## <a name="deploy-the-solution"></a>De oplossing implementeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt ook modules met behulp van de Cloud Explorer voor Visual Studio implementeren. U hebt al een implementatie manifest voor bereid voor uw scenario, `deployment.json` het bestand en alles wat u hoeft te doen, selecteert u een apparaat om de implementatie te ontvangen.

1. Open **Cloud Explorer** door te klikken op **weergave** > **Cloud Explorer**. Zorg ervoor dat u bent aangemeld bij Visual Studio 2019.

1. In **Cloud Explorer**uit, vouw uw abonnement, zoeken naar uw Azure-IoT-Hub en de Azure IoT Edge-apparaat dat u wilt implementeren.

1. Klik met de rechter muisknop op het IoT Edge apparaat om een implementatie voor te maken. u moet het manifest bestand van de implementatie `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`selecteren onder de.

   > [!NOTE]
   > U moet niet selecteren `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

1. Klik op de knop Vernieuwen om de nieuwe modules samen met de **SimulatedTemperatureSensor** -module en **$edgeAgent** en **$edgeHub**weer te geven.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Als u het D2C-bericht voor een specifiek apparaat wilt bewaken, selecteert u het apparaat in de lijst en klikt u vervolgens op **controle van ingebouwd gebeurtenis-eind punt starten** in het **actie** venster.

1. Als u de bewaking van gegevens wilt stoppen, selecteert u het apparaat in de lijst en selecteert u in het **actie** venster het **ingebouwde gebeurtenis eindpunt stoppen met bewaking** .

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van aangepaste modules voor uw IoT Edge-apparaten [en het gebruik van Azure IOT hub sdk's](../iot-hub/iot-hub-devguide-sdks.md).
