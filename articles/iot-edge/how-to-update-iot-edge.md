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
ms.openlocfilehash: 27f0ebab1fd87eb1870c5a8be21c4f80be4132f1
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698591"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>De IoT Edge security-daemon en runtime bijwerken

Wanneer de IoT Edge-service nieuwe versies uitbrengt, moet u uw IoT Edge-apparaten bijwerken voor de nieuwste functies en beveiligings verbeteringen. In dit artikel bevat informatie over het bijwerken van uw IoT Edge-apparaten wanneer een nieuwe versie beschikbaar is. 

Twee onderdelen van een IoT Edge-apparaat moeten worden bijgewerkt als u wilt verplaatsen naar een nieuwere versie. De eerste is de beveiligings-daemon die op het apparaat wordt uitgevoerd en de runtime modules start wanneer het apparaat wordt gestart. De daemon voor beveiliging op dit moment kan alleen van het apparaat zelf worden bijgewerkt. Het tweede onderdeel is de runtime, bestaande uit de modules IoT Edge hub en IoT Edge agent. Afhankelijk van hoe u uw implementatie structureren, kan de runtime van het apparaat of extern worden bijgewerkt. 

De nieuwste versie van Azure IoT Edge, Zie [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

## <a name="update-the-security-daemon"></a>Update de daemon voor beveiliging

De daemon van de beveiliging IoT Edge is een systeemeigen onderdeel dat moet worden bijgewerkt met behulp van de package manager op het IoT Edge-apparaat. 

Controleer de versie van de security-daemon op uw apparaat met behulp van de opdracht `iotedge version`. 

### <a name="linux-devices"></a>Linux-apparaten

Op Linux x64-apparaten gebruikt u apt-get of uw geschikte pakket beheerder om de beveiligings-daemon bij te werken. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

Op Linux ARM32-apparaten gebruikt u de stappen in [Install Azure IOT Edge runtime op Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md) om de nieuwste versie van de beveiligings-daemon te installeren. 

### <a name="windows-devices"></a>Windows-apparaten

Gebruik het Power shell-script voor het bijwerken van de beveiligings-daemon op Windows-apparaten. Het script haalt automatisch de nieuwste versie van de beveiligings-daemon op. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Als u de opdracht update-IoTEdge uitvoert, wordt de beveiligings-daemon van uw apparaat, samen met de twee runtime container installatie kopieën, verwijderd. Het bestand config. yaml wordt op het apparaat bewaard, evenals gegevens van de Moby-container Engine (als u Windows-containers gebruikt). Het houden van de configuratie gegevens betekent dat u de gegevens van de connection string of de Device Provisioning Service voor uw apparaat niet opnieuw hoeft op te geven tijdens het update proces. 

Als u een specifieke versie van de beveiligings-daemon wilt installeren, moet u het juiste Microsoft-Azure-IoTEdge. cab-bestand downloaden uit [IOT Edge releases](https://github.com/Azure/azure-iotedge/releases). Vervolgens gebruikt u de `-OfflineInstallationPath` para meter om naar de bestands locatie te verwijzen. Zie [offline-installatie](how-to-install-iot-edge-windows.md#offline-installation)voor meer informatie.

## <a name="update-the-runtime-containers"></a>Bijwerken van de runtimecontainers

De manier waarop u de IoT Edge agent-en IoT Edge hub-containers bijwerkt, is afhankelijk van of u rollende Tags (zoals 1,0) of specifieke tags (zoals 1.0.7) gebruikt in uw implementatie. 

Controleer de versie van de IOT Edge agent en IOT Edge hub-modules op uw apparaat met behulp `iotedge logs edgeAgent` van `iotedge logs edgeHub`de opdrachten of. 

  ![Containerversie niet vinden in Logboeken](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Informatie over IoT Edge-tags

De installatie kopieën van IoT Edge agent en IoT Edge hub worden gelabeld met de IoT Edge-versie waaraan ze zijn gekoppeld. Er zijn twee verschillende manieren om labels te gebruiken met de runtime-installatiekopieën: 

* **Tags rolling** -alleen de eerste twee waarden van het versienummer gebruiken om op te halen van de meest recente installatiekopie die overeenkomt met de cijfers. Bijvoorbeeld wordt 1.0 bijgewerkt wanneer er sprake is van een nieuwe versie om te verwijzen naar de nieuwste versie van de 1.0.x. Als de container-runtime op uw IoT Edge-apparaat opnieuw ophaalt van de installatiekopie, worden de runtimemodules bijgewerkt naar de nieuwste versie. Deze methode wordt aangeraden voor ontwikkelingsdoeleinden. Implementaties van de Azure portal standaardwaarde voor rolling tags. 
* **Specifieke labels** -gebruik van alle drie de waarden van het versienummer expliciet in te stellen de versie van de installatiekopie. 1\.0.7 wordt bijvoorbeeld niet gewijzigd na de eerste release. Wanneer u bent klaar om te werken, kunt u een nieuwe versienummer in het manifest implementatie declareren. Deze methode wordt aangeraden voor productiedoeleinden.

### <a name="update-a-rolling-tag-image"></a>Een rolling tag installatiekopie bijwerken

Als u rolling tags in uw implementatie gebruiken (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0**) moet u de container-runtime afdwingen op uw apparaat voor het ophalen van de meest recente versie van de installatiekopie. 

De lokale versie van de installatiekopie van het verwijderen van uw IoT Edge-apparaat. Op Windows-computers verwijdert het verwijderen van de Security daemon ook de runtime-installatie kopieën. u hoeft deze stap dus niet opnieuw uit te voeren. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

U moet mogelijk gebruik van de force `-f` vlag te verwijderen van de installatiekopieën. 

De IoT Edge-service ophalen van de meest recente versies van de runtime-afbeeldingen en automatisch start deze op uw apparaat opnieuw. 

### <a name="update-a-specific-tag-image"></a>De installatiekopie van een specifieke tag bijwerken

Als u specifieke tags in uw implementatie gebruikt (bijvoorbeeld mcr.microsoft.com/azureiotedge-hub:**1.0.7**), hoeft u alleen de tag in het implementatie manifest bij te werken en de wijzigingen toe te passen op uw apparaat. 

In de Azure-portal, de runtime-implementatie-installatiekopieën zijn gedefinieerd in de **geavanceerde instellingen voor Edge-Runtime configureren** sectie. 

![Geavanceerde runtime-instellingen voor Edge configureren](./media/how-to-update-iot-edge/configure-runtime.png)

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

## <a name="update-to-a-release-candidate-version"></a>Bijwerken naar een release Candi date-versie

Azure IoT Edge brengt regel matig nieuwe versies van de IoT Edge service uit. Voor elke stabiele versie zijn er een of meer release Candi date (RC)-versies. RC-versies bevatten alle geplande functies voor de release, maar zijn nog steeds bezig met het testen en valideren van validatie processen die vereist zijn voor een stabiele release. Als u een nieuwe functie vroegtijdig wilt testen, kunt u de RC-versie installeren en feedback geven via GitHub. 

Release Candi date-versies volgen dezelfde Nummerings Conventie voor releases, maar hebben wel **-RC** plus een incrementeel nummer toegevoegd aan het einde. U kunt de release kandidaten in dezelfde lijst met [Azure IOT Edge releases](https://github.com/Azure/azure-iotedge/releases) zien als de stabiele versies. Zoek bijvoorbeeld naar **1.0.7-RC1** en **1.0.7-RC2**, de twee release kandidaten die zijn ontvangen vóór **1.0.7**. U kunt ook zien dat RC-versies zijn gemarkeerd met **voorlopige** labels. 

Als previews worden versies van release Candi date niet opgenomen als de meest recente versie die de reguliere installatie Programma's doel. In plaats daarvan moet u hand matig de assets richten voor de RC-versie die u wilt testen. Afhankelijk van uw IoT Edge besturings systeem van het apparaat, gebruikt u de volgende secties om IoT Edge bij te werken naar een specifieke versie:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Volgende stappen

Bekijk de meest recente [releases van Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Blijf up-to-date met recente updates en aankondigingen in het [Internet of Things blog](https://azure.microsoft.com/blog/topics/internet-of-things/) 