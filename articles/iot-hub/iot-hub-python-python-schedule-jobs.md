---
title: Taken plannen met Azure IoT Hub (python) | Microsoft Docs
description: Een Azure IoT Hub-taak plannen voor het aanroepen van een directe methode op meerdere apparaten. U gebruikt de Azure IoT Sdk's voor python voor het implementeren van de gesimuleerde apparaat-apps en een service-app om de taak uit te voeren.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2019
ms.author: kgremban
ms.openlocfilehash: f4a7cbb5c4f8f4a019cbf5d63a6f2ffe8092546e
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405890"
---
# <a name="schedule-and-broadcast-jobs-python"></a>Taken plannen en uitzenden (python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee een back-end-app taken kan maken en bijhouden waarmee miljoenen apparaten kunnen worden gepland en bijgewerkt.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Tags bijwerken
* Directe methoden aanroepen

Met een taak wordt een van deze acties gewikkeld en wordt de voortgang van de uitvoering van een set apparaten getraceerd, die wordt gedefinieerd door een dubbele query voor een apparaat.  Een back-end-app kan bijvoorbeeld een taak gebruiken om een methode voor opnieuw opstarten op te roepen op 10.000-apparaten, opgegeven door een dubbele query van een apparaat en in een later tijdstip te worden gepland.  Deze toepassing kan vervolgens de voortgang volgen wanneer deze apparaten de methode voor opnieuw opstarten ontvangen en uitvoeren.

Meer informatie over elk van deze mogelijkheden vindt u in de volgende artikelen:

* Dubbele en eigenschappen van apparaat: [Aan de slag met apparaatdubbels](iot-hub-python-twin-getstarted.md) en [zelf studie: De dubbele eigenschappen van een apparaat gebruiken](tutorial-device-twins.md)

* Directe methoden: [IOT hub ontwikkelaars handleiding-directe methoden](iot-hub-devguide-direct-methods.md) en [zelf studie: directe methoden](quickstart-control-device-python.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maak een met python gesimuleerde apparaat-app die een directe methode heeft, waardoor **lockDoor**kan worden aangeroepen door de back-end van de oplossing.

* Maak een python-console-app die de directe methode **lockDoor** aanroept in de gesimuleerde apparaat-app met behulp van een taak en de gewenste eigenschappen bijwerkt met behulp van een apparaat taak.

Aan het einde van deze zelf studie hebt u twee python-apps:

**simDevice.py**, die verbinding maakt met uw IOT-hub met de apparaat-id en een **lockDoor** directe methode ontvangt.

**scheduleJobService.py**, waarmee een directe methode wordt aangeroepen in de gesimuleerde apparaat-app en de gewenste eigenschappen van het apparaat worden bijgewerkt met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2. x of 3. x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk [pip *installeren of upgraden*, het Python-pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/).

* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) om het gebruik van systeemeigen DLL's van Python mogelijk te maken.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

> [!NOTE]
> De **Azure IOT SDK voor python** biedt geen rechtstreekse ondersteuning voor de functionaliteit van **taken** . In plaats daarvan biedt deze zelf studie een alternatieve oplossing die gebruikmaakt van asynchrone threads en timers. Zie de **Service client SDK** Feature List op de pagina [Azure IOT SDK voor python](https://github.com/Azure/azure-iot-sdk-python) voor verdere updates. 
>

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een python-console-app die reageert op een directe methode die wordt aangeroepen door de Cloud, waardoor een gesimuleerde **lockDoor** -methode wordt geactiveerd.

1. Voer bij de opdracht prompt de volgende opdracht uit om het **Azure-IOT-Device-client-** pakket te installeren:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Maak een nieuw **simDevice.py** -bestand in uw werkmap met behulp van een tekst editor.

3. Voeg de volgende `import` instructies en variabelen toe aan het begin van het **simDevice.py** -bestand. Vervang `deviceConnectionString` door de Connection String van het apparaat dat u hierboven hebt gemaakt:

    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. Voeg de volgende functie-call back toe om de methode **lockDoor** te verwerken:

    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

5. Een andere functie-call back toevoegen voor het verwerken van apparaatdubbels-updates:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

6. Voeg de volgende code toe om de handler voor de methode **lockDoor** te registreren. Neem ook de `main` routine op:

    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

            while True:
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
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

7. Sla het **simDevice.py** -bestand op en sluit het.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productie code moet u beleid voor opnieuw proberen implementeren (zoals een exponentiële uitstel), zoals wordt voorgesteld in het artikel, [tijdelijke fout afhandeling](/azure/architecture/best-practices/transient-faults).
>

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Taken plannen voor het aanroepen van een directe methode en het bijwerken van de eigenschappen van een apparaat met dubbele gegevens

In deze sectie maakt u een python-console-app die een externe **lockDoor** op een apparaat initieert met behulp van een directe methode en de eigenschappen van het apparaat twee bijwerkt.

1. Voer bij de opdracht prompt de volgende opdracht uit om het **Azure-IOT-service-client-** pakket te installeren:

    ```cmd/sh
    pip install azure-iothub-service-client
    ```

2. Maak een nieuw **scheduleJobService.py** -bestand in uw werkmap met behulp van een tekst editor.

3. Voeg de volgende `import` instructies en variabelen toe aan het begin van het **scheduleJobService.py** -bestand:

    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

4. Voeg de volgende functie toe die wordt gebruikt voor het zoeken naar apparaten:

    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)

        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

5. Voeg de volgende methoden toe om de taken uit te voeren die de directe methode en het dubbele apparaat aanroepen:

    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )

    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)

            print ( "" )
            print ( "Device twin updated." )
    ```

6. Voeg de volgende code toe om de taken te plannen en de taak status bij te werken. Neem ook de `main` routine op:

    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()

            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )

            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()

            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )

            while True:
                print ( "" )

                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )

                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )

                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

7. Sla het **scheduleJobService.py** -bestand op en sluit het.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Voer de volgende opdracht uit vanaf de opdracht prompt in de werkmap om te beginnen met Luis teren naar de methode voor het opnieuw opstarten van direct:

    ```cmd/sh
    python simDevice.py
    ```

2. Voer bij een andere opdracht prompt in uw werkmap de volgende opdracht uit om de taken te activeren om de deur te vergren delen en het dubbele te updaten:
  
    ```cmd/sh
    python scheduleJobService.py
    ```

3. U ziet de reacties van het apparaat op de direct-methode en de update van de apparaatdubbels voor apparaten in de-console.

    ![IoT Hub taak voorbeeld 1--uitvoer van apparaat](./media/iot-hub-python-python-schedule-jobs/sample1-deviceoutput.png)

    ![Voor beeld van IoT Hub taak 2--uitvoer van apparaat](./media/iot-hub-python-python-schedule-jobs/sample2-deviceoutput.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een taak gebruikt voor het plannen van een directe methode op een apparaat en het bijwerken van de eigenschappen van het apparaat.

Zie [een firmware-update uitvoeren](tutorial-firmware-update.md)als u wilt door gaan met IOT hub en patronen voor Apparaatbeheer, zoals extern via de Air firmware-update.