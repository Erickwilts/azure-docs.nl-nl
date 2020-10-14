---
title: Modules ontwikkelen voor Azure IoT Edge | Microsoft Docs
description: Ontwikkel aangepaste modules voor Azure IoT Edge die kunnen communiceren met de runtime en IoT Hub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: eae36f6b4baabdcc9831b084602d340a299a7bac
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047621"
---
# <a name="develop-your-own-iot-edge-modules"></a>Uw eigen IoT Edge-modules ontwikkelen

Azure IoT Edge modules kunnen verbinding maken met andere Azure-Services en bijdragen aan uw grotere gegevens pijplijn voor de Cloud. In dit artikel wordt beschreven hoe u modules kunt ontwikkelen om te communiceren met de IoT Edge runtime en IoT Hub, en dus ook de rest van de Azure-Cloud.

## <a name="iot-edge-runtime-environment"></a>Runtime-omgeving IoT Edge

De IoT Edge runtime biedt de infra structuur voor het integreren van de functionaliteit van meerdere IoT Edge modules en voor het implementeren ervan op IoT Edge apparaten. Elk programma kan worden verpakt als een IoT Edge module. Een programma dat in een module wordt uitgevoerd, kan de Azure IoT Device SDK gebruiken om verbinding te maken met de lokale IoT Edge hub om optimaal te profiteren van IoT Edge communicatie-en beheer functionaliteit.

## <a name="using-the-iot-edge-hub"></a>De IoT Edge hub gebruiken

De IoT Edge hub biedt twee belang rijke functies: proxy naar IoT Hub en lokale communicatie.

### <a name="iot-hub-primitives"></a>IoT Hub primitieven

IoT Hub ziet een module-exemplaar vergelijkbaar met een apparaat, in de zin dat:

* het heeft een module die twee ledig is en die is geïsoleerd van het [apparaat](../iot-hub/iot-hub-devguide-device-twins.md) en de andere module apparaatdubbels van dat apparaat.
* Er kunnen [apparaat-naar-Cloud-berichten](../iot-hub/iot-hub-devguide-messaging.md)worden verzonden.
* het kan [directe methoden](../iot-hub/iot-hub-devguide-direct-methods.md) ontvangen die specifiek zijn gericht op de identiteit.

Momenteel kunnen modules geen Cloud-naar-apparaat-berichten ontvangen of de functie voor het uploaden van bestanden gebruiken.

Wanneer u een module schrijft, kunt u de [Azure IOT Device SDK](../iot-hub/iot-hub-devguide-sdks.md) gebruiken om verbinding te maken met de IOT Edge hub en de bovenstaande functionaliteit te gebruiken zoals u zou doen wanneer u IOT hub gebruikt met een apparaat-app. Het enige verschil tussen IoT Edge modules en IoT Device-toepassingen is dat u moet verwijzen naar de module-identiteit in plaats van de apparaat-id.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten

IoT Edge hub biedt declaratieve route ring van berichten tussen modules en tussen modules en IoT Hub om complexe verwerking van apparaat-naar-Cloud-berichten mogelijk te maken. Met declaratieve route ring kunnen modules berichten die worden verzonden door andere modules onderscheppen en verwerken en ze door geven aan complexe pijp lijnen. Zie [modules implementeren en routes instellen in IOT Edge](module-composition.md)voor meer informatie.

Een IoT Edge module, in tegens telling tot een normale IoT Hub apparaat-toepassing, kan apparaat-naar-Cloud-berichten ontvangen die via de lokale IoT Edge hub worden verzonden om ze te verwerken.

IoT Edge hub worden de berichten door gegeven aan uw module op basis van declaratieve routes die in het [implementatie manifest](module-composition.md)worden beschreven. Wanneer u een IoT Edge module ontwikkelt, kunt u deze berichten ontvangen door de bericht afhandeling in te stellen.

IoT Edge voegt het concept van module- *invoer* -en *uitvoer* eindpunten toe om het maken van routes te vereenvoudigen. Een module kan alle apparaat-naar-Cloud-berichten die hiernaar worden doorgestuurd, ontvangen zonder dat er invoer is opgegeven, en kan apparaat-naar-Cloud-berichten verzenden zonder een uitvoer op te geven. Door gebruik te maken van expliciete invoer en uitvoer, worden routerings regels echter eenvoudiger te begrijpen.

Ten slotte worden apparaat-naar-Cloud-berichten die door de Edge hub worden verwerkt, voorzien van de volgende systeem eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| $connectionDeviceId | De apparaat-ID van de client die het bericht heeft verzonden |
| $connectionModuleId | De module-ID van de module die het bericht heeft verzonden |
| $inputName | De invoer die dit bericht heeft ontvangen. Kan leeg zijn. |
| $outputName | De uitvoer die wordt gebruikt om het bericht te verzenden. Kan leeg zijn. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Verbinding maken met IoT Edge hub vanuit een module

Verbinding maken met de lokale IoT Edge hub vanuit een module bestaat uit twee stappen:

1. Maak een ModuleClient-exemplaar in uw toepassing.
2. Zorg ervoor dat uw toepassing het certificaat accepteert dat door de IoT Edge hub op dat apparaat wordt aangeboden.

Maak een ModuleClient-exemplaar om uw module te koppelen aan de IoT Edge hub die op het apparaat wordt uitgevoerd, vergelijkbaar met de manier waarop DeviceClient-instanties IoT-apparaten verbinden met IoT Hub. Voor meer informatie over de ModuleClient-klasse en de communicatie methoden raadpleegt u de API-verwijzing voor uw voorkeurs taal van de SDK: [C#](/dotnet/api/microsoft.azure.devices.client.moduleclient), [C](/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [python](/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient), [Java](/java/api/com.microsoft.azure.sdk.iot.device.moduleclient)of [Node.js](/javascript/api/azure-iot-device/moduleclient).

## <a name="language-and-architecture-support"></a>Ondersteuning voor taal en architectuur

IoT Edge ondersteunt meerdere besturings systemen, architecturen en ontwikkelings talen, zodat u het scenario kunt bouwen dat aan uw behoeften voldoet. Gebruik deze sectie om inzicht te krijgen in uw opties voor het ontwikkelen van aangepaste IoT Edge-modules. U vindt meer informatie over hulp middelen en vereisten voor elke taal in [het voorbereiden van uw ontwikkel-en test omgeving voor IOT Edge](development-environment.md).

### <a name="linux"></a>Linux

IoT Edge ondersteunt de ontwikkeling voor AMD64-en ARM32 Linux-apparaten voor alle talen in de volgende tabel.

| Ontwikkelingstaal | Ontwikkelhulpprogramma’s |
| -------------------- | ----------------- |
| C | Visual Studio Code<br>Visual Studio 2017/2019 |
| C# | Visual Studio Code<br>Visual Studio 2017/2019 |
| Java | Visual Studio Code |
| Node.js | Visual Studio Code |
| Python | Visual Studio Code |

>[!NOTE]
>Ondersteuning voor het ontwikkelen en opsporen van fouten voor ARM64 Linux-apparaten is beschikbaar in de [open bare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Zie [ARM64 IoT Edge-modules ontwikkelen in Visual Studio Code en er fouten in opsporen (preview)](https://devblogs.microsoft.com/iotdev/develop-and-debug-arm64-iot-edge-modules-in-visual-studio-code-preview) voor meer informatie.

### <a name="windows"></a>Windows

IoT Edge ondersteunt de ontwikkeling van AMD64 Windows-apparaten voor alle talen in de volgende tabel.

| Ontwikkelingstaal | Ontwikkelhulpprogramma’s |
| -------------------- | ----------------- |
| C | Visual Studio 2017/2019 |
| C# | Visual Studio code (geen mogelijkheden voor fout opsporing)<br>Visual Studio 2017/2019 |

## <a name="next-steps"></a>Volgende stappen

[Bereid uw ontwikkel-en test omgeving voor op IoT Edge](development-environment.md)

[Visual Studio gebruiken voor het ontwikkelen van C#-modules voor IoT Edge](how-to-visual-studio-develop-module.md)

[Visual Studio code gebruiken voor het ontwikkelen van modules voor IoT Edge](how-to-vs-code-develop-module.md)

[Azure IoT Hub SDK's leren kennen en gebruiken](../iot-hub/iot-hub-devguide-sdks.md)