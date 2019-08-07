---
title: Aan de slag met Azure IoT Hub Device Management (python) | Microsoft Docs
description: IoT Hub Apparaatbeheer gebruiken om het opnieuw opstarten van een extern apparaat te initiëren. U gebruikt de Azure IoT SDK voor python voor het implementeren van een gesimuleerde apparaat-app die een directe methode en een service-app bevat die de directe methode aanroept.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: 52651ca592c4da9883768cd87e090985e17be47b
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780925"
---
# <a name="get-started-with-device-management-python"></a>Aan de slag met Apparaatbeheer (python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* Gebruik de Azure Portal voor het maken van een IoT Hub en het maken van een apparaat-id in uw IoT-hub.

* Maak een gesimuleerde apparaat-app die een directe methode bevat waarmee dat apparaat opnieuw wordt opgestart. Directe methoden worden vanuit de Cloud aangeroepen.

* Maak een python-console-app die de directe methode voor opnieuw opstarten aanroept in de gesimuleerde apparaat-app via uw IoT-hub.

Aan het einde van deze zelf studie hebt u twee python-console-apps:

* **dmpatterns_getstarted_device. py**, dat verbinding maakt met uw IOT-hub met de apparaat-id die u eerder hebt gemaakt, ontvangt een directe methode voor opnieuw opstarten, simuleert fysieke opnieuw opstarten en rapporteert de tijd voor de laatste keer opnieuw opstarten.

* **dmpatterns_getstarted_service. py**, waarmee een directe methode wordt aangeroepen in de gesimuleerde apparaat-app, het antwoord wordt weer gegeven en de bijgewerkte gerapporteerde eigenschappen worden weer gegeven.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

Hier volgen de installatie-instructies voor de vereisten.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie gaat u het volgende doen:

* Een python-console-app maken die reageert op een directe methode die wordt aangeroepen door de Cloud

* Opnieuw opstarten van een apparaat simuleren

* Gebruik de gerapporteerde eigenschappen om Device-dubbele query's in te scha kelen om apparaten te identificeren en wanneer deze voor het laatst opnieuw zijn opgestart

1. Maak een **dmpatterns_getstarted_device. py** -bestand met behulp van een tekst editor.

2. Voeg de volgende `import` -instructies toe aan het begin van het bestand **dmpatterns_getstarted_device. py** .

    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

3. Voeg variabelen toe met inbegrip van een **CONNECTION_STRING** -variabele en de initialisatie van de client.  Vervang de connection string door de connection string van uw apparaat.  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

4. Voeg de volgende functie-call backs toe om de directe methode op het apparaat te implementeren.

    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS

        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS

        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
            time.sleep(20)

            print ( "Device rebooted." )

            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            print ( "Updating device twins: rebootTime" )

        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200

        return device_method_return_value
    ```

5. Start de listener voor directe methoden en wacht.

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

6. Sla het bestand **dmpatterns_getstarted_device. py** op en sluit het af.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productie code moet u beleid voor opnieuw proberen implementeren (zoals een exponentiële uitstel), zoals wordt voorgesteld in het artikel, [tijdelijke fout afhandeling](/azure/architecture/best-practices/transient-faults).

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een externe keer opnieuw opstarten op het apparaat activeren met behulp van een directe methode

In deze sectie maakt u een python-console-app die extern opnieuw opstarten op een apparaat initieert via een directe methode. De app maakt gebruik van Device-dubbele query's om de tijd van de laatste keer opnieuw opstarten voor dat apparaat te detecteren.

1. Maak een **dmpatterns_getstarted_service. py** -bestand met behulp van een tekst editor.

2. Voeg de volgende `import` -instructies toe aan het begin van het bestand **dmpatterns_getstarted_service. py** .

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

3. Voeg de volgende variabelen declaraties toe. Vervang alleen tijdelijke aanduidingen voor _IoTHubConnectionString_ en _deviceId_.

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

4. Voeg de volgende functie toe om de methode van het apparaat aan te roepen om het doel apparaat opnieuw op te starten en zoek vervolgens naar het apparaat apparaatdubbels en ontvang de laatste keer opnieuw opstarten.

    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )

            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

5. Sla het bestand **dmpatterns_getstarted_service. py** op en sluit het af.

## <a name="run-the-apps"></a>De apps uitvoeren

U kunt nu de apps uitvoeren.

1. Voer bij de opdracht prompt de volgende opdracht uit om te beginnen met Luis teren naar de methode voor direct opnieuw opstarten.

    ```
    python dmpatterns_getstarted_device.py
    ```

2. Voer bij een andere opdracht prompt de volgende opdracht uit om het apparaat op afstand opnieuw op te starten en de query uit te voeren op de tijd van de laatste keer opnieuw opstarten.

    ```
    python dmpatterns_getstarted_service.py
    ```

3. U ziet de reactie van het apparaat op de directe methode in de-console.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]