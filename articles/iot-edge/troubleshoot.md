---
title: Problemen oplossen - Azure IoT Edge | Microsoft Docs
description: Gebruik dit artikel voor meer informatie over standard diagnostische vaardigheden voor Azure IoT Edge, net als bij het ophalen van Onderdeelstatus en logboeken en oplossen van veelvoorkomende problemen
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 00147002317f15345f01c88e81973837d16e6669
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65797609"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Veelvoorkomende problemen en oplossingen voor Azure IoT Edge

Als u problemen hebt met het uitvoeren van Azure IoT Edge in uw omgeving, kunt u dit artikel als richtlijn gebruiken voor het oplossen van problemen.

## <a name="run-the-iotedge-check-command"></a>Voer de opdracht ' controleren' iotedge

Uw eerste stap bij het oplossen van IoT Edge moet gebruiken de `check` opdracht, waarbij een verzameling van configuratie en connectiviteit tests voor veelvoorkomende problemen uitvoert. De `check` opdracht is beschikbaar in [release 1.0.7](https://github.com/Azure/azure-iotedge/releases/tag/1.0.7) en hoger.

U kunt uitvoeren de `check` opdracht als volgt en bevatten de `--help` vlag voor een volledige lijst met opties:

* Op Linux:

  ```bash
  sudo iotedge check
  ```

* In Windows:

  ```powershell
  iotedge check
  ```

De typen controles uitvoeren door het hulpprogramma kunnen worden geclassificeerd als:

* Configuratie controleert: Details die u voorkomen Edge-apparaten verbinden met de cloud dat kunnen, met inbegrip van problemen met onderzoekt *config.yaml* en de container-engine.
* Controles van de verbinding: Controleert of de IoT Edge-runtime toegang hebben tot poorten op de hostapparaat en alle onderdelen van de IoT Edge kunnen verbinding maken met de IoT-Hub.
* Productie-gereedheidscontroles: Aanbevolen procedures voor aanbevolen productie, zoals de status van apparaatcertificaten van certificeringsinstanties (CA) en configuratie van het logboekbestand module zoekt.

Zie voor een volledige lijst van diagnostische controles, [ingebouwde functionaliteit voor probleemoplossing](https://github.com/Azure/iotedge/blob/master/doc/troubleshoot-checks.md).

## <a name="standard-diagnostic-steps"></a>Standaard diagnostische stappen

Als u een probleem ondervindt, kunt u meer informatie over de status van uw IoT Edge-apparaat aan de hand van de containerlogboeken van de en de berichten die doorgeven naar en van het apparaat. Gebruik de opdrachten en hulpprogramma's in deze sectie om informatie te verzamelen.

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Controleer de status van de IoT Edge Security Manager en de logboeken

Op Linux:
- De status van de IoT Edge Security Manager weergeven:

   ```bash
   sudo systemctl status iotedge
   ```

- De logboeken van de IoT Edge Security Manager weergeven:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Gedetailleerde logboeken van de IoT Edge Security Manager om informatie weer te geven:

   - De instellingen van de daemon iotedge bewerken:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Werk de volgende regels:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - De IoT Edge Security-Daemon opnieuw:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

In Windows:
- De status van de IoT Edge Security Manager weergeven:

   ```powershell
   Get-Service iotedge
   ```

- De logboeken van de IoT Edge Security Manager weergeven:

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Als de IoT Edge Security Manager wordt niet uitgevoerd, controleert u of uw yaml-configuratiebestand

> [!WARNING]
> YAML-bestanden mag geen tabbladen als inspringing bevatten. Gebruik in plaats daarvan 2 spaties.

Op Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

In Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Raadpleeg de containerlogboeken voor problemen

Zodra de IoT Edge Security-Daemon wordt uitgevoerd, bekijkt u de logboeken van de containers om problemen te detecteren. Beginnen met uw geïmplementeerde containers en kijk vervolgens naar de containers die gezamenlijk de IoT Edge-runtime: edgeAgent en edgeHub. De IoT Edge agent-logboeken bieden doorgaans informatie over de levenscyclus van elke container. De IoT Edge hub-logboeken bieden informatie over berichten en routering. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-iot-edge-hub"></a>Bekijk de berichten die via de IoT Edge hub

U kunt de berichten die via de IoT Edge hub bekijken, en inzichten te verwerven uit uitgebreide Logboeken uit de runtimecontainers. Als u wilt inschakelen op de uitgebreide logboeken op deze containers, instellen `RuntimeLogLevel` in uw configuratiebestand yaml. Open het bestand:

Op Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

In Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Standaard de `agent` element ziet er als volgt:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Vervang `env: {}` met:

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

   > [!WARNING]
   > YAML-bestanden mag geen tabbladen als inspringing bevatten. Gebruik in plaats daarvan 2 spaties.

Sla het bestand op en start de IoT Edge security manager opnieuw.

U kunt ook de berichten controleren die worden verzonden tussen IoT Hub en de IoT Edge-apparaten. Deze berichten weergeven met behulp van de [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit-extensie)-extensie voor Visual Studio Code. Zie voor meer informatie, [handig hulpmiddel bij het ontwikkelen met Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Opnieuw opstarten van containers
Na onderzoek van de logboeken en berichten voor meer informatie, kunt u proberen opnieuw te starten containers:

```
iotedge restart <container name>
```

Start opnieuw op de IoT Edge-runtime-containers:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Opnieuw opstarten van de IoT Edge-beveiligingsbeheer

Als het probleem nog steeds in de persistent maken, kunt u proberen de IoT Edge security manager opnieuw te starten.

Op Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

In Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="iot-edge-agent-stops-after-about-a-minute"></a>IoT Edge agent stopt na ongeveer een minuut

De module edgeAgent wordt gestart en is voor ongeveer een minuut uitgevoerd en vervolgens gestopt. De logboeken geven aan dat de IoT Edge-agent probeert te verbinden met IoT Hub via AMQP en vervolgens verbinding probeert te maken met behulp van AMQP via WebSocket. Als dat mislukt, wordt de IoT Edge agent afgesloten. 

Voorbeeld edgeAgent Logboeken:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Hoofdoorzaak
Een netwerkconfiguratie op het hostnetwerk verhindert dat de IoT Edge agent het netwerk wordt bereikt. De agent probeert eerst verbinding maken via AMQP (poort 5671). Als de verbinding is mislukt, probeert deze WebSockets (poort 443).

De IoT Edge-runtime stelt een netwerk in voor elk van de modules waarmee moet worden gecommuniceerd. In Linux is dit netwerk een brugnetwerk. In Windows wordt NAT gebruikt. Dit probleem komt vaker voor op Windows-apparaten die gebruikmaken van Windows-containers die het NAT-netwerk gebruiken. 

### <a name="resolution"></a>Oplossing
Zorg ervoor dat er een route naar internet is voor de IP-adressen die aan deze brug/dit NAT-netwerk zijn toegewezen. Soms heeft een VPN-configuratie op de host voorrang op het IoT Edge-netwerk. 

## <a name="iot-edge-hub-fails-to-start"></a>IoT Edge hub niet kan worden gestart

De module edgeHub is mislukt voor de begin- en drukt het volgende bericht naar de logboeken: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Hoofdoorzaak
Poort 443 is bezig met een ander proces op de hostmachine. De IoT Edge hub toegewezen poorten 5671 en 443 voor gebruik in gatewayscenario's. Deze poorttoewijzing mislukt als er al een ander proces bezig is op deze poort. 

### <a name="resolution"></a>Oplossing
Zoek en stop het proces dat poort 443 gebruikt. Dit proces is meestal een webserver.

## <a name="iot-edge-agent-cant-access-a-modules-image-403"></a>IoT Edge-agent geen toegang tot de installatiekopie van een module (403)
Een container kan niet worden uitgevoerd en de logboeken edgeAgent fout 403 weergegeven. 

### <a name="root-cause"></a>Hoofdoorzaak
De Iot Edge agent beschikt niet over machtigingen voor toegang tot de installatiekopie van een module. 

### <a name="resolution"></a>Oplossing
Zorg ervoor dat de registerreferenties van uw juist zijn opgegeven in uw manifest implementatie

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>IoT Edge security-daemon is mislukt met een ongeldige hostnaam

De opdracht `sudo journalctl -u iotedge` mislukt dat en wordt het volgende bericht: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Hoofdoorzaak
IoT Edge-runtime biedt alleen ondersteuning voor hostnamen die korter zijn dan 64 tekens lang zijn. Fysieke machines gewoonlijk lang hostnamen niet hebben, maar het probleem komt vaker voor op een virtuele machine. De automatisch gegenereerde hostnamen voor Windows virtuele machines die worden gehost in Azure, meestal in het bijzonder te lang. 

### <a name="resolution"></a>Oplossing
Wanneer u deze fout ziet, kunt u deze kunt oplossen door de DNS-naam van uw virtuele machine configureren en vervolgens de DNS-naam als de hostnaam in de setup-opdracht in te stellen.

1. Navigeer naar de overzichtspagina van de virtuele machine in Azure portal. 
2. Selecteer **configureren** onder DNS-naam. Als uw virtuele machine is al een DNS-naam die is geconfigureerd, hoeft u niet te configureren van een nieuwe. 

   ![DNS-naam van de virtuele machine configureren](./media/troubleshoot/configure-dns.png)

3. Geef een waarde voor **DNS-naamlabel** en selecteer **opslaan**.
4. Kopiëren van de nieuwe DNS-naam, de indeling moet  **\<DNSnamelabel\>.\< VM-locatie\>. cloudapp.azure.com**.
5. Gebruik de volgende opdracht voor het instellen van de IoT Edge-runtime met uw DNS-naam in de virtuele machine:

   - Op Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - In Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Problemen met de stabiliteit van bron beperkte apparaten 
U kunt de stabiliteitsproblemen op beperkte apparaten, zoals de Raspberry Pi, ondervinden met name wanneer die wordt gebruikt als een gateway. Symptomen zijn buiten het geheugen uitzonderingen in de edge hub-module, downstream apparaten kunnen geen verbinding maken of het apparaat reageert verzenden van berichten over telemetrie na een paar uur.

### <a name="root-cause"></a>Hoofdoorzaak
De IoT Edge hub, die deel van de IoT Edge-runtime uitmaakt, standaard is geoptimaliseerd voor prestaties en grote hoeveelheden geheugen wordt toegewezen. Deze optimalisatie is niet ideaal voor beperkte edge-apparaten en stabiliteitsproblemen kan veroorzaken.

### <a name="resolution"></a>Oplossing
Voor de IoT Edge hub, stelt u een omgevingsvariabele **OptimizeForPerformance** naar **false**. Er zijn twee manieren om dit te doen:

In de gebruikersinterface: 

Navigeer in de portal naar **Apparaatdetails** > **Modules instellen** > **geavanceerde instellingen voor Edge-Runtime configureren**. Maken van een omgevingsvariabele voor de Edge Hub-module met de naam *OptimizeForPerformance* die is ingesteld op *false*.

![OptimizeForPerformance ingesteld op false](./media/troubleshoot/optimizeforperformance-false.png)

**OR**

In het manifest van de implementatie:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Kan niet op Windows op de IoT Edge daemon-logboeken ophalen
Als u een EventLogException ontvangt wanneer u `Get-WinEvent` op Windows, controleert u de Register-invoer.

### <a name="root-cause"></a>Hoofdoorzaak
De `Get-WinEvent` PowerShell-opdracht is afhankelijk van een registervermelding moet aanwezig zijn om te zoeken van Logboeken op een specifieke `ProviderName`.

### <a name="resolution"></a>Oplossing
Een registervermelding voor de IoT Edge-daemon instellen. Maak een **iotedge.reg** bestand met de volgende inhoud en importeren naar het Windows-register door erop te dubbelklikken of met behulp van de `reg import iotedge.reg` opdracht:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>IoT Edge-module is mislukt voor het verzenden van een bericht naar de edgeHub met 404-fout

Een aangepaste IoT Edge-module is mislukt voor het verzenden van een bericht naar de edgeHub met een 404-fout `Module not found` fout. De IoT Edge-daemon afdrukken van het volgende bericht naar de logboeken: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Hoofdoorzaak
De IoT Edge-daemon wordt afgedwongen proces-id in voor alle modules die verbinding maken met de edgeHub uit veiligheidsoverwegingen. Hiermee wordt gecontroleerd dat alle berichten worden verzonden door een module afkomstig van de belangrijkste proces-ID van de module zijn. Als een bericht wordt verzonden door een module van de ID van een ander proces dan in eerste instantie tot stand gebracht, wordt deze het bericht met een 404-fout-bericht negeren.

### <a name="resolution"></a>Oplossing
Zorg ervoor dat de hetzelfde proces-ID altijd door de aangepaste IoT Edge-module gebruikt wordt om berichten te verzenden naar de edgeHub. Bijvoorbeeld, zorg ervoor dat u `ENTRYPOINT` in plaats van `CMD` opdracht in uw Docker-bestand, aangezien `CMD` zal leiden tot een proces-ID voor de module en een ander proces-ID voor de bash-opdracht uitvoeren van de belangrijkste programma dat `ENTRYPOINT` zal leiden tot een een enkel proces-ID.


## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>Firewall- en poortinstellingen configuratieregels voor IoT Edge-implementatie
Azure IoT Edge kunt u communicatie tussen een on-premises server en Azure-cloud met ondersteunde protocollen van IoT Hub, Zie [een communicatieprotocol kiezen](../iot-hub/iot-hub-devguide-protocols.md). Voor een betere beveiliging zijn altijd communicatiekanalen tussen Azure IoT Edge en Azure IoT-Hub geconfigureerd om uitgaand. Deze configuratie is gebaseerd op de [Services communicatiepatroon](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), die minimaliseert de kwetsbaarheid voor aanvallen voor een schadelijke entiteit om te verkennen. Binnenkomende communicatie is alleen vereist voor specifieke scenario's waar Azure IoT Hub moeten push-berichten naar de Azure IoT Edge-apparaat. Cloud-naar-apparaat-berichten worden beschermd met behulp van beveiligde TLS-kanalen en verder kunnen beveiligd met X.509-certificaten en -modules voor TPM-apparaat. De Azure IoT Edge Security Manager bepaalt hoe deze communicatie kan worden tot stand gebracht, Zie [IoT Edge Security Manager](../iot-edge/iot-edge-security-manager.md).

Hoewel IoT Edge verbeterde configuratie biedt voor het beveiligen van Azure IoT Edge-runtime en modules geïmplementeerd, is het nog steeds afhankelijk van de onderliggende machine en het netwerk. Daarom kan het is belangrijk om ervoor te zorgen goede netwerk en firewall-regels zijn ingesteld voor beveiligde edge naar de cloud communicatie. De volgende tabel kan worden gebruikt als richtlijn wanneer configuratie firewallregels voor de onderliggende servers waar Azure IoT Edge-runtime wordt gehost:

|Protocol|Poort|binnenkomende|Uitgaand|Richtlijnen|
|--|--|--|--|--|
|MQTT|8883|GEBLOKKEERDE (standaard)|GEBLOKKEERDE (standaard)|<ul> <li>Uitgaand (uitgaand) te openen bij gebruik van MQTT als het communicatieprotocol configureren.<li>. 1883 voor MQTT wordt niet ondersteund door de IoT Edge. <li>Binnenkomende verbindingen voor (inkomend) moeten worden geblokkeerd.</ul>|
|AMQP|5671|GEBLOKKEERDE (standaard)|OPEN (standaard)|<ul> <li>Standaard communicatieprotocol voor IoT Edge. <li> Moet worden geconfigureerd om te worden geopend als Azure IoT Edge is niet geconfigureerd voor andere ondersteunde protocollen of AMQP het gewenste communicatieprotocol is.<li>5672 voor AMQP wordt niet ondersteund door de IoT Edge.<li>Deze poort blokkeren wanneer Azure IoT Edge maakt gebruik van een andere IoT-Hub ondersteund protocol.<li>Binnenkomende verbindingen voor (inkomend) moeten worden geblokkeerd.</ul></ul>|
|HTTPS|443|GEBLOKKEERDE (standaard)|OPEN (standaard)|<ul> <li>Configureer uitgaande (uitgaand) te openen op 443 voor IoT Edge wordt ingericht. Deze configuratie is vereist bij het gebruik van handmatige scripts of Azure IoT Device Provisioning Service (DPS). <li>Inkomende (inkomend) verbinding moet Open zijn alleen voor specifieke scenario's: <ul> <li>  Als u een transparante gateway met leaf-apparaten die methodeaanvragen kunnen worden verzonden hebt. Poort 443 hoeft in dit geval niet te worden geopend voor externe netwerken verbinding maken met IoTHub of IoTHub-services leveren via Azure IoT Edge. De binnenkomende regel kan zo worden beperkt tot alleen binnenkomende (binnenkomend) openen van het interne netwerk. <li> Voor de Client naar apparaat (C2D)-scenario's.</ul><li>80 voor HTTP wordt niet ondersteund door de IoT Edge.<li>Als niet-HTTP-protocollen (bijvoorbeeld AMQP of MQTT) kunnen niet worden geconfigureerd in de onderneming; de berichten kunnen worden verzonden via WebSockets. In dat geval wordt poort 443 gebruikt voor communicatie van de WebSocket.</ul>|

## <a name="edge-agent-module-continually-reports-empty-config-file-and-no-modules-start-on-the-device"></a>Edge Agent-module voortdurend rapporten leeg config 'file' en er zijn geen modules worden gestart op het apparaat

Het apparaat heeft problemen bij het starten van modules die zijn gedefinieerd in de implementatie. Alleen de edgeAgent is uitgevoerd maar voortdurend reporting '... leeg configuratiebestand'.

### <a name="potential-root-cause"></a>Mogelijke hoofdoorzaak te achterhalen
IoT Edge wordt standaard gestart van modules in hun eigen geïsoleerde container-netwerk. Het apparaat heeft mogelijk problemen met DNS-naamomzetting binnen dit particuliere netwerk.

### <a name="resolution"></a>Oplossing

**Optie 1: Instellingen voor het DNS-server in de container engine**

Geef de DNS-server voor uw omgeving in de instellingen van de container-engine die van toepassing op alle container modules aan de slag door de engine zijn. Maak een bestand met de naam `daemon.json` op te geven van de DNS-server te gebruiken. Bijvoorbeeld:

```
{
    "dns": ["1.1.1.1"]
}
```

Het bovenstaande voorbeeld Hiermee stelt u de DNS-server met een openbaar toegankelijke DNS-service. Als het edge-apparaat geen toegang deze IP van de omgeving tot, vervangen door het adres van de DNS-server die toegankelijk is.

Plaats `daemon.json` in de juiste locatie voor uw platform: 

| Platform | Locatie |
| --------- | -------- |
| Linux | `/etc/docker` |
| Windows-host met Windows-containers | `C:\ProgramData\iotedge-moby\config` |

Als de locatie al bevat `daemon.json` bestand, voeg de **dns** sleutel toe en sla het bestand.

*De container-engine voor de updates van kracht opnieuw opstarten*

| Platform | Opdracht |
| --------- | -------- |
| Linux | `sudo systemctl restart docker` |
| Windows (Admin Powershell) | `Restart-Service iotedge-moby -Force` |

**Optie 2: Stel de DNS-server in IoT Edge-implementatie per module**

U kunt DNS-server voor van elke module instellen *createOptions* in de IoT Edge-implementatie. Bijvoorbeeld:

```
"createOptions": {
  "HostConfig": {
    "Dns": [
      "x.x.x.x"
    ]
  }
}
```

Zorg ervoor dat u dit voor de *edgeAgent* en *edgeHub* ook modules. 

## <a name="next-steps"></a>Volgende stappen
Denkt u dat u een fout op het IoT Edge-platform hebt gevonden? [Verzend een probleem](https://github.com/Azure/iotedge/issues) zodat we verder kunnen blijven verbeteren. 

Als u meer vragen hebt, maakt u een [ondersteuningsaanvraag](https://portal.azure.com/#create/Microsoft.Support) voor hulp. 

