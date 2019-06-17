---
title: De connectiviteit en telemetriegegevens verzamelt apparaat met digitale dubbels Azure | Microsoft Docs
description: Overzicht van hoe u een apparaat voorbereiden met Azure digitale dubbels mee te brengen
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: alinast
ms.openlocfilehash: 35d12d0114f9677905c85a9df94ecd074e5f8f75
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60925866"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Apparaatconnectiviteit en inkomende telemetriegegevens

De telemetriegegevens die zijn verzonden door apparaten en sensoren vormen de ruggengraat van een IoT-oplossing. Hoe u deze verschillende resources geven en deze beheren via de context van een locatie zijn chief opmerkingen kunnen in de ontwikkeling van IoT-Apps. Azure van digitale dubbels vereenvoudigt het proces van het ontwikkelen van IoT-oplossingen door samenvoeging van apparaten en sensoren met een ruimtelijke intelligence-grafiek.

Om te beginnen, maakt u een Azure IoT Hub-resource in de hoofdmap van de ruimtelijke grafiek. De resource van de IoT Hub kunt u alle apparaten onder de hoofdmap ruimte om berichten te verzenden. Nadat de IoT-Hub is gemaakt, kunt u apparaten registreren met sensoren binnen het digitale Twins-exemplaar. De apparaten gegevens kunnen verzenden naar een digitale Twins-service via de [Azure IoT device-SDK](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks).

Zie voor een stapsgewijze handleiding voor het toevoegen van apparaten onboarden, de [zelfstudie om te implementeren en configureren van digitale dubbels](tutorial-facilities-setup.md). In een oogopslag zijn de stappen:

- Implementeren van een digitale Twins-exemplaar van de [Azure-portal](https://portal.azure.com).
- Opslagruimten maken in de grafiek.
- De resource van een IoT-Hub maken en toewijzen aan een spatie in de grafiek.
- Apparaten en sensoren in uw grafiek maken en deze toewijzen aan de spaties in de vorige stappen hebt gemaakt.
- Maak een matcher om te filteren op basis van voorwaarden berichten over telemetrie.
- Maak een [de gebruiker gedefinieerde functie](concepts-user-defined-functions.md), en deze toewijzen aan een spatie in de grafiek voor aangepaste verwerking van berichten over uw telemetrie.
- Een rol waarmee de gebruiker gedefinieerde functie voor toegang tot de grafiekgegevens toewijzen.
- De IoT Hub apparaat-verbindingsreeks ophalen uit de digitale dubbels Management API's.
- Configureer de apparaatverbindingsreeks op het apparaat met de Azure IoT-device-SDK.

In de volgende gedeelten leert u hoe u de IoT Hub-verbindingsreeks ophalen uit de API voor beheer van digitale dubbels. U leert ook hoe u de berichtindeling voor telemetrie van IoT Hub gebruiken om op basis van een sensor telemetrie te verzenden. Digitale dubbels is vereist voor elk onderdeel van de telemetrie die wordt ontvangen om te worden gekoppeld aan een sensor binnen de ruimtelijke grafiek. Deze vereiste zorgt ervoor de gegevens worden verwerkt en gerouteerd binnen het juiste ruimtelijke context.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>De IoT Hub-verbindingsreeks ophalen uit de beheer-API

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Voer een GET-aanroep van de apparaat-API met een `includes=ConnectionString` parameter om op te halen van de verbindingsreeks van de IoT Hub-apparaat. Filteren op het apparaat GUID of de hardware-ID om te bepalen van het opgegeven apparaat.

```plaintext
YOUR_MANAGEMENT_API_URL/devices/YOUR_DEVICE_GUID?includes=ConnectionString
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_DEVICE_GUID* | De apparaat-ID |

```plaintext
YOUR_MANAGEMENT_API_URL/devices?hardwareIds=YOUR_DEVICE_HARDWARE_ID&includes=ConnectionString
```

| Parameterwaarde | Vervangen door |
| --- | --- |
| *YOUR_DEVICE_HARDWARE_ID* | De hardware-ID van het apparaat |

Kopieer in de nettolading van de reactie van het apparaat **connectionString** eigenschap. U dit gebruiken wanneer u de Azure IoT-device-SDK om gegevens te verzenden naar digitale dubbels aanroepen.

## <a name="device-to-cloud-message"></a>Apparaat-naar-cloud bericht

U kunt aanpassen van uw apparaat berichtindeling en -nettolading aanpassen aan de behoeften van uw oplossing. Gebruik een gegevensovereenkomst die kan worden geserialiseerd in een bytematrix of een stroom die wordt ondersteund door de [bericht (byteArray byte []) van Azure IoT Device Client Message klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). Het bericht is een aangepaste binaire indeling van uw keuze, zolang u de opdracht gegevens in een bijbehorende door de gebruiker gedefinieerde functie decoderen. Er is slechts één vereiste voor een apparaat-naar-cloud-bericht. U moet een set eigenschappen om te controleren of dat uw bericht op de juiste wijze wordt doorgestuurd naar de engine voor gebeurtenisverwerking behouden.

### <a name="telemetry-properties"></a>Telemetrie-eigenschappen

 De nettolading van de inhoud van een **bericht** willekeurige gegevens mag maximaal 256 KB groot. Er zijn enkele vereisten verwacht voor eigenschappen van de [ `Message.Properties` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet) type. De tabel bevat de vereiste en optionele eigenschappen die worden ondersteund door het systeem.

| Naam van eigenschap | Value | Vereist | Description |
|---|---|---|---|
| **DigitalTwins-Telemetry** | 1.0 | Ja | Een constante waarde die een bericht op het systeem wordt geïdentificeerd. |
| **DigitalTwins-SensorHardwareId** | `string(72)` | Ja | Een unieke id van de sensor die verzendt de **bericht**. Deze waarde moet overeenkomen met een object **HardwareId** eigenschap voor het systeem te verwerken. Bijvoorbeeld `00FF0643BE88-CO2`. |
| **CreationTimeUtc** | `string` | Nee | Een [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) opgemaakt datumtekenreeks waarmee de steekproeven tijd van de nettolading. Bijvoorbeeld `2018-09-20T07:35:00.8587882-07:00`. |
| **CorrelationId** | `string` | Nee | Een UUID die is gebruikt om te traceringsgebeurtenissen in het systeem. Bijvoorbeeld `cec16751-ab27-405d-8fe6-c68e1412ce1f`.

### <a name="send-your-message-to-digital-twins"></a>Uw bericht verzenden naar digitale dubbels

Gebruik de DeviceClient [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) of [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) aanroep van uw bericht te verzenden naar digitale dubbels.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over Azure digitale dubbels gegevensverwerking en mogelijkheden van de gebruiker gedefinieerde functies, [Azure digitale dubbele gegevens verwerken en de gebruiker gedefinieerde functies](concepts-user-defined-functions.md).
