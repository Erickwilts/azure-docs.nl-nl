---
title: Versie van de update IoT Edge op apparaten - Azure IoT Edge | Microsoft Docs
description: Het bijwerken van IoT Edge-apparaten voor het uitvoeren van de meest recente versies van de security-daemon en de IoT Edge-runtime
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c461da44d3d9075d66a68fe8994a4e970288fca
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543750"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>De IoT Edge security-daemon en runtime bijwerken

Zodra de IoT Edge-service worden nieuwe versies uitgebracht, moet u uw IoT Edge-apparaten voor de nieuwste functies en verbeteringen in de beveiliging bij te werken. In dit artikel bevat informatie over het bijwerken van uw IoT Edge-apparaten wanneer een nieuwe versie beschikbaar is. 

Twee onderdelen van een IoT Edge-apparaat moeten worden bijgewerkt als u wilt verplaatsen naar een nieuwere versie. De eerste is de daemon voor beveiliging, die wordt uitgevoerd op het apparaat en de runtimemodules wordt gestart wanneer het apparaat wordt gestart. De daemon voor beveiliging op dit moment kan alleen van het apparaat zelf worden bijgewerkt. Het tweede onderdeel is de runtime, die bestaan uit de IoT Edge hub en IoT Edge agent-modules. Afhankelijk van hoe u uw implementatie structureren, kan de runtime van het apparaat of extern worden bijgewerkt. 

De nieuwste versie van Azure IoT Edge, Zie [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

>[!IMPORTANT]
>Als u Azure IoT Edge op een Windows-apparaat uitvoert, niet bijwerken naar versie 1.0.5 als een van de volgende van toepassing op uw apparaat is: 
>* U hebt uw apparaat niet bijgewerkt naar Windows build 17763. IoT Edge-versie 1.0.5 biedt geen ondersteuning voor Windows bouwt ouder zijn dan 17763.
>* U uitvoeren Java- of Node.js-modules op uw Windows-apparaat. Versie 1.0.5 overslaan, zelfs als u uw Windows-apparaat hebt bijgewerkt naar de nieuwste build. 
>
>Zie voor meer informatie over IoT Edge versie 1.0.5 [1.0.5 opmerkingen bij de release](https://github.com/Azure/azure-iotedge/releases/tag/1.0.5). Zie voor meer informatie over hoe om te voorkomen dat uw ontwikkelhulpmiddelen bijwerken naar de nieuwste versie, [de IoT-ontwikkelaarsblog](https://devblogs.microsoft.com/iotdev/).


## <a name="update-the-security-daemon"></a>Update de daemon voor beveiliging

De daemon van de beveiliging IoT Edge is een systeemeigen onderdeel dat moet worden bijgewerkt met behulp van de package manager op het IoT Edge-apparaat. 

Controleer de versie van de security-daemon op uw apparaat met behulp van de opdracht `iotedge version`. 

### <a name="linux-devices"></a>Linux-apparaten

Op Linux x64 apparaten, moet u apt-get of uw juiste Pakketbeheer gebruiken om bij te werken van de security-daemon. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

Op apparaten met Linux ARM32, gebruikt u de stappen in [installeren Azure IoT Edge-runtime op Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) voor het installeren van de meest recente versie van de daemon voor beveiliging. 

### <a name="windows-devices"></a>Windows-apparaten

Gebruik het PowerShell-script om bij te werken van de security-daemon op Windows-apparaten. De nieuwste versie van de daemon beveiliging automatisch door het script worden opgehaald. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Met de opdracht Update IoTEdge Hiermee verwijdert u de daemon van de beveiliging van uw apparaat, samen met de twee runtime-containerinstallatiekopieën. Het bestand config.yaml wordt opgeslagen op het apparaat, evenals de gegevens van de container Moby engine (als u Windows-containers). De configuratie-informatie betekent waarvoor u geen voor de verbindingsreeks of Device Provisioning Service-informatie voor uw apparaat opnieuw tijdens het updateproces houden. 

Als u wilt voor het installeren van een specifieke versie van de daemon voor beveiliging, download u het juiste bestand voor Microsoft-Azure-IoTEdge.cab van [IoT Edge-versies](https://github.com/Azure/azure-iotedge/releases). Vervolgens gebruikt u de `-OfflineInstallationPath` parameter om te verwijzen naar de bestandslocatie. Zie voor meer informatie, [Offline-installatie](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Bijwerken van de runtimecontainers

De manier waarop de IoT Edge-agent en de IoT Edge hub containers te werken, is afhankelijk van of u rolling tags (zoals 1.0) of specifieke tags (zoals 1.0.7) in uw implementatie gebruiken. 

Controleer de versie van de IoT Edge-agent en de IoT Edge hub-modules die zich momenteel op uw apparaat met de opdrachten `iotedge logs edgeAgent` of `iotedge logs edgeHub`. 

  ![Containerversie niet vinden in Logboeken](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Informatie over IoT Edge-tags

De IoT Edge-agent en de IoT Edge hub afbeeldingen worden gemarkeerd met de IoT Edge-versie die ze zijn gekoppeld. Er zijn twee verschillende manieren om labels te gebruiken met de runtime-installatiekopieën: 

* **Tags rolling** -alleen de eerste twee waarden van het versienummer gebruiken om op te halen van de meest recente installatiekopie die overeenkomt met de cijfers. Bijvoorbeeld wordt 1.0 bijgewerkt wanneer er sprake is van een nieuwe versie om te verwijzen naar de nieuwste versie van de 1.0.x. Als de container-runtime op uw IoT Edge-apparaat opnieuw ophaalt van de installatiekopie, worden de runtimemodules bijgewerkt naar de nieuwste versie. Deze methode wordt aangeraden voor ontwikkelingsdoeleinden. Implementaties van de Azure portal standaardwaarde voor rolling tags. 
* **Specifieke labels** -gebruik van alle drie de waarden van het versienummer expliciet in te stellen de versie van de installatiekopie. Bijvoorbeeld, 1.0.7 worden niet gewijzigd na de eerste release. Wanneer u bent klaar om te werken, kunt u een nieuwe versienummer in het manifest implementatie declareren. Deze methode wordt aangeraden voor productiedoeleinden.

### <a name="update-a-rolling-tag-image"></a>Een rolling tag installatiekopie bijwerken

Als u rolling tags in uw implementatie gebruiken (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0**) moet u de container-runtime afdwingen op uw apparaat voor het ophalen van de meest recente versie van de installatiekopie. 

De lokale versie van de installatiekopie van het verwijderen van uw IoT Edge-apparaat. Op Windows-machines, verwijderen van de security-daemon verwijdert u ook de runtime-installatiekopieën, dus u hoeft voor deze stap opnieuw uit. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

U moet mogelijk gebruik van de force `-f` vlag te verwijderen van de installatiekopieën. 

De IoT Edge-service ophalen van de meest recente versies van de runtime-afbeeldingen en automatisch start deze op uw apparaat opnieuw. 

### <a name="update-a-specific-tag-image"></a>De installatiekopie van een specifieke tag bijwerken

Als u specifieke tags in uw implementatie gebruiken (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0.7**) en u hoeft alleen de code in uw manifest implementatie bijwerken en de wijzigingen toepassen op uw apparaat is. 

In de Azure-portal, de runtime-implementatie-installatiekopieën zijn gedefinieerd in de **geavanceerde instellingen voor Edge-Runtime configureren** sectie. 

![Instellingen voor geavanceerde edge-runtime configureren](./media/how-to-update-iot-edge/configure-runtime.png)

In een manifest van de implementatie JSON bijwerken van de module-afbeeldingen in de **systemModules** sectie. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.7",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.7",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="update-to-a-release-candidate-version"></a>Bijwerken naar een release candidate-versie

Azure IoT Edge regelmatig nieuwe versies van de IoT Edge-service. Voordat elke stabiele versie is er een of meer release candidate (RC)-versies. RC-versies bevatten alle geplande functies voor de release, maar worden nog steeds doorlopen de testen en valideren processen die nodig zijn voor een stabiele versie. Als u een nieuwe functie vroeg testen wilt, kunt u de RC-versie installeren en feedback geven via GitHub. 

Release candidate-versies volgen dezelfde conventie nummering van versies, maar hebben **-rc** plus een volgnummer toegevoegd aan het einde. U ziet de release-kandidaten in dezelfde lijst met [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) als de stabiele versie. Bijvoorbeeld zoeken naar **1.0.7-rc1** en **1.0.7-rc2**, de twee kandidaten die afkomstig vóór zijn de release **1.0.7**. U kunt ook zien dat de RC-versies zijn gemarkeerd met **voorlopige** labels. 

Als voorbeelden, release candidate-versies worden niet opgenomen als de meest recente versie die het doel van de reguliere installatieprogramma's. In plaats daarvan moet u handmatig de activa voor de RC-versie die u wilt testen als doel. Gebruik de volgende secties voor het bijwerken van IoT Edge naar een specifieke versie afhankelijk van uw besturingssysteem voor IoT Edge-apparaat:

* [Linux X64](how-to-install-iot-edge-linux.md#install-a-specific-version)
* [Linux ARM32](how-to-install-iot-edge-linux-arm.md#install-a-specific-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Volgende stappen

Bekijk de meest recente [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Blijf op de hoogte met recente updates en de aankondiging in de [Internet of Things-blog](https://azure.microsoft.com/blog/topics/internet-of-things/) 