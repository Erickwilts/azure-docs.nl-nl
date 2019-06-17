---
title: Aan de slag met Azure IoT Hub identiteits- en module moduledubbel (Python) | Microsoft Docs
description: Informatie over het module-id maken en bijwerken van de moduledubbel met behulp van IoT SDK's voor Python.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: f887fbd4f82e59c02d6a5b69d0d5b43b426a39bc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61441204"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-python-back-end-and-python-device"></a>Aan de slag met IoT Hub identiteits- en module moduledubbel met behulp van Python-back-end en Python-apparaat

> [!NOTE]
> [Module-id's en moduledubbels](iot-hub-devguide-module-twins.md) zijn vergelijkbaar met Azure IoT Hub-apparaat-id's en apparaatdubbels, maar bieden een hogere granulariteit. Met een Azure IoT Hub-apparaat-id en apparaatdubbel kan een apparaat worden geconfigureerd via de back-endtoepassing en kunt u inzicht krijgen in de toestand van een apparaat. Een module-id en moduledubbel bieden deze mogelijkheden voor afzonderlijke onderdelen van een apparaat. Op apparaten met meerdere onderdelen, zoals apparaten met een besturingssysteem of firmware, kunt u afzonderlijke configuraties en voorwaarden voor elk onderdeel instellen.
>

Aan het einde van deze zelfstudie hebt u twee Python-apps:

* **CreateIdentities**: deze toepassing maakt een apparaat-id, een module-id en de bijbehorende beveiligingssleutel waarmee uw apparaat- en moduleclients verbonden kunnen worden.

* **UpdateModuleTwinReportedProperties**: deze toepassing verzendt bijgewerkte gerapporteerde eigenschappen van de moduledubbel naar uw IoT Hub.

> [!NOTE]
> Zie voor informatie over de Azure IoT SDK's die u gebruiken kunt om beide toepassingen uit te voeren op apparaten en de back-end van uw oplossing te bouwen, [Azure IoT SDK's](iot-hub-devguide-sdks.md).
>

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

* An IoT Hub.

* Installeer de meest recente [Python SDK](https://github.com/Azure/azure-iot-sdk-python).

U hebt nu uw IoT Hub gemaakt en u hebt de hostnaam en de IoT Hub-verbindingsreeks die u nodig hebt voor de rest van deze handleiding.

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Een apparaat-id en de identiteit van een module in IoT-Hub maken

In deze sectie maakt u een Python-app die u een apparaat-id en de identiteit van een module in het identiteitenregister van uw IoT-hub maakt. Een apparaat of module kan alleen verbinding maken met de IoT-hub als het apparaat of de module is vermeld in het identiteitsregister. Zie voor meer informatie de sectie 'id-register van de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide-identity-registry.md). Als u deze consoletoepassing uitvoert, worden er een unieke id en een unieke sleutel gemaakt voor zowel het apparaat als de module. Deze waarden worden door het apparaat en de module gebruikt om zichzelf te identificeren bij het verzenden van apparaat-naar-cloud-berichten naar IoT Hub. De id's zijn hoofdlettergevoelig.

Voeg de volgende code naar uw Python-bestand:

```python
import sys
import iothub_service_client
from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubError

CONNECTION_STRING = "YourConnString"
DEVICE_ID = "myFirstDevice"
MODULE_ID = "myFirstModule"

try:
    # RegistryManager
    iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

    # CreateDevice
    primary_key = ""
    secondary_key = ""
    auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
    new_device = iothub_registry_manager.create_device(DEVICE_ID, primary_key, secondary_key, auth_method)
    print("new_device <" + DEVICE_ID + "> has primary key = " + new_device.primaryKey)

    # CreateModule
    new_module = iothub_registry_manager.create_module(DEVICE_ID, primary_key, secondary_key, MODULE_ID, auth_method)
    print("device/new_module <" + DEVICE_ID + "/" + MODULE_ID + "> has primary key = " + new_module.primaryKey)

except IoTHubError as iothub_error:
    print ( "Unexpected error {0}".format(iothub_error) )
except KeyboardInterrupt:
    print ( "IoTHubRegistryManager sample stopped" )
```

Deze app maakt een apparaat-id met de ID **myFirstDevice** en de identiteit van een module met de ID **myFirstModule** onder apparaat **myFirstDevice**. (Als deze module-id al in het id-register staat, haalt de code gewoon de bestaande modulegegevens op.) De app geeft vervolgens de primaire sleutel voor die identiteit weer. U gebruikt deze sleutel in de gesimuleerde module-app om verbinding te maken met uw IoT-hub.

> [!NOTE]
> In het identiteitsregister van IoT Hub worden alleen apparaat- en module-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. In het identiteitsregister worden apparaat-id's en -sleutels opgeslagen die als beveiligingsreferenties worden gebruikt. In het identiteitsregister wordt ook een vlag ingeschakeld/uitgeschakeld voor elk apparaat opgeslagen die u kunt gebruiken om de toegang tot dat apparaat uit te schakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Er is geen vlag voor ingeschakeld/uitgeschakeld voor module-id's. Zie voor meer informatie, [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide-identity-registry.md).
>

## <a name="update-the-module-twin-using-python-device-sdk"></a>De moduledubbel met de Python SDK bijwerken

In deze sectie maakt u een Python-app op uw gesimuleerde apparaat die updates van de moduledubbel gerapporteerde eigenschappen.

1. **Haal de verbindingsreeks van uw module** --als u zich aanmeldt bij de [Azure-portal](https://portal.azure.com/). Navigeer naar uw IoT Hub en klik op IoT-apparaten. Zoeken naar myFirstDevice, open deze en u ziet myFirstModule is aangemaakt. Kopieer de moduleverbindingsreeks. Deze is vereist voor de volgende stap.

   ![Details van de Azure Portal-module](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

2. **UpdateModuleTwinReportedProperties-app maken**

   Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubDeviceTwin, IoTHubError

    CONNECTION_STRING = "FILL IN CONNECTION STRING"
    DEVICE_ID = "MyFirstDevice"
    MODULE_ID = "MyFirstModule"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"telemetryInterval\":122}}}"

    try:
        iothub_twin = IoTHubDeviceTwin(CONNECTION_STRING)

        twin_info = iothub_twin.get_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Twin before update    :" )
        print ( "{0}".format(twin_info) )

        twin_info_updated = iothub_twin.update_twin(DEVICE_ID, MODULE_ID, UPDATE_JSON)
        print ( "" )
        print ( "Twin after update     :" )
        print ( "{0}".format(twin_info_updated) )

    except IoTHubError as iothub_error:
        print ( "Unexpected error {0}".format(iothub_error) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

In dit codevoorbeeld ziet u hoe u de gerapporteerde eigenschappen van de moduledubbel kunt ophalen en bijwerken met het AMQP-protocol.

## <a name="get-updates-on-the-device-side"></a>Ontvang updates aan de kant van het apparaat

Naast de bovenstaande code kunt u toevoegen onderstaande codeblok om de update dubbele bericht op uw apparaat.

```python
import random
import time
import sys
import iothub_client
from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult

PROTOCOL = IoTHubTransportProvider.AMQP
CONNECTION_STRING = ""

def module_twin_callback(update_state, payload, user_context):
    print ("")
    print ("Twin callback called with:")
    print ("updateStatus: %s" % update_state )
    print ("context: %s" % user_context )
    print ("payload: %s" % payload )

try:
    module_client = IoTHubModuleClient(CONNECTION_STRING, PROTOCOL)
    module_client.set_module_twin_callback(module_twin_callback, 1234)

    print ("Waiting for incoming twin messages.  Hit Control-C to exit.")
    while True:

        time.sleep(1000000)

except IoTHubError as iothub_error:
    print ( "Unexpected error {0}".format(iothub_error) )
except KeyboardInterrupt:
    print ( "module client sample stopped" )
```

## <a name="next-steps"></a>Volgende stappen

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md)

* [Aan de slag met IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)