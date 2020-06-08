---
title: 'Quickstart: Telemetrie naar Azure IoT Hub verzenden met Java'
description: In deze snelstart voert u twee Java-voorbeeldtoepassingen uit om gesimuleerde telemetrie te verzenden naar een IoT-hub en telemetrie van de IoT-hub te lezen voor verwerking in de cloud.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom:
- mvc
- seo-java-august2019
- seo-java-september2019
- mqtt
ms.date: 05/26/2020
ms.openlocfilehash: 225b9407bff4294288bb4834cbb0d919698b78c6
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873173"
---
# <a name="quickstart-send-telemetry-to-an-azure-iot-hub-and-read-it-with-a-java-application"></a>Quickstart: Telemetrie verzenden naar een Azure IoT-hub en deze lezen met een Java-toepassing

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

In deze quickstart verzendt u telemetrie naar Azure IoT Hub en leest u deze met een Java-toepassing. IoT Hub is een Azure-service waarmee grote hoeveelheden telemetrie van uw IoT-apparaten naar de cloud kunt opnemen voor opslag of verwerking. In deze quickstart worden twee vooraf geschreven Java-toepassingen gebruikt, één voor het verzenden van de telemetrie en één voor het lezen van de telemetrie van de hub. Voordat u deze twee toepassingen kunt uitvoeren, moet u een IoT-hub maken en een apparaat registreren bij de hub.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account met een actief abonnement. [Maak er gratis een](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

* Java SE Development Kit 8. In [Java-langetermijnondersteuning voor Azure en Azure Stack](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable) onder **Langetermijnondersteuning** selecteert u **Java 8**.

* [Apache Maven 3](https://maven.apache.org/download.cgi).

* [Een voorbeeld van een Java-project](https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip).

* Poort 8883 is geopend in de firewall. In het apparaatvoorbeeld in deze quickstart wordt het MQTT-protocol gebruikt, dat communiceert via poort 8883. Deze poort is in sommige netwerkomgevingen van bedrijven en onderwijsinstellingen mogelijk geblokkeerd. Zie [Verbinding maken met IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub) voor meer informatie en manieren om dit probleem te omzeilen.

Gebruik de volgende opdracht om de huidige versie van Java op uw ontwikkelcomputer te controleren:

```cmd/sh
java -version
```

Gebruik de volgende opdracht om de huidige versie van Maven op uw ontwikkelcomputer te controleren:

```cmd/sh
mvn --version
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT-extensie toevoegen

Voer de volgende opdracht uit om de Microsoft Azure IoT-extensie voor Azure CLI aan uw Cloud Shell-instantie toe te voegen. Met de IoT-extensie worden IoT Hub-, IoT Edge- en DPS-specifieke (Microsoft System Center Data Protection Manager) opdrachten toegevoegd aan Azure CLI.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Een apparaat registreren

Een apparaat moet zijn geregistreerd bij uw IoT-hub voordat het verbinding kan maken. In deze snelstart gebruikt u Azure Cloud Shell om een gesimuleerd apparaat te registreren.

1. Voer de volgende opdrachten uit in Azure Cloud Shell om de apparaat-id te maken.

   **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

   **MyJavaDevice** : dit is de naam van het apparaat dat u gaat registreren. Het is raadzaam om **MyJavaDevice** te gebruiken zoals wordt weergegeven. Als u een andere naam voor het apparaat kiest, moet u deze naam ook in de rest van dit artikel gebruiken, en moet u de apparaatnaam bijwerken in de voorbeeldtoepassingen voordat u deze uitvoert.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

2. Voer de volgende opdrachten uit in Azure Cloud Shell om de _apparaatverbindingsreeks_ op te halen voor het apparaat dat u zojuist hebt geregistreerd:

    **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyJavaDevice --output table
    ```

    Noteer de apparaatverbindingsreeks. Deze ziet er ongeveer als volgt uit:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyJavaDevice;SharedAccessKey={YourSharedAccessKey}`

    U gebruikt deze waarde verderop in de quickstart.

3. U hebt ook het _Event Hubs-compatibele eindpunt_, het _Event Hubs-compatibele pad_ en de _primaire sleutel service_ van uw IoT-hub nodig om de back-endtoepassing in staat te stellen verbinding te maken met uw IoT-hub en de berichten op te halen. Met de volgende opdrachten worden deze waarden opgehaald voor uw IoT-hub:

     **YourIoTHubName**: vervang deze tijdelijke aanduiding door een door u gekozen naam voor de IoT-hub.

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name service --query primaryKey --hub-name {YourIoTHubName}
    ```

    Noteer deze drie waarden die u later in deze quickstart gaat gebruiken.

## <a name="send-simulated-telemetry"></a>Gesimuleerde telemetrie verzenden

De toepassing voor het gesimuleerde apparaat maakt verbinding met een apparaatspecifiek eindpunt op uw IoT-hub en verstuurt gesimuleerde telemetrie over temperatuur en luchtvochtigheid.

1. Navigeer in een lokaal terminalvenster naar de hoofdmap van het voorbeeldproject in Java. Navigeer vervolgens naar de map **iot-hub\Quickstarts\simulated-device**.

2. Open het bestand **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** in een teksteditor van uw keuze.

    Vervang de waarde van de variabele `connString` door de apparaatverbindingsreeks die u eerder hebt genoteerd. Sla daarna de wijzigingen op in **SimulatedDevice.java**.

3. Voer in het lokale terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de toepassing voor het gesimuleerde apparaat te compileren:

    ```cmd/sh
    mvn clean package
    ```

4. Voer in het lokale terminalvenster de volgende opdrachten uit om de toepassing voor het gesimuleerde apparaat uit te voeren:

    ```cmd/sh
    java -jar target/simulated-device-1.0.0-with-deps.jar
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de toepassing voor het gesimuleerde apparaat telemetriegegevens naar uw IoT-hub verzendt:

    ![Uitvoer van de telemetrie die door het apparaat wordt verzonden naar uw IoT-hub](media/quickstart-send-telemetry-java/simulated-device.png)

## <a name="read-the-telemetry-from-your-hub"></a>De telemetrie van uw hub lezen

De back-endtoepassing maakt verbinding met het eindpunt **Events** aan de servicezijde van uw IoT-hub. De toepassing ontvangt de berichten die van het gesimuleerde apparaat naar de cloud worden verzonden. Een back-endtoepassing van IoT Hub wordt meestal uitgevoerd in de cloud om berichten van apparaat naar cloud te ontvangen en verwerken.

1. Navigeer in een ander lokaal terminalvenster naar de hoofdmap van het voorbeeldproject in Java. Navigeer vervolgens naar de map **iot-hub\Quickstarts\read-d2c-messages**.

2. Open het bestand **src/main/java/com/microsoft/docs/iothub/samples/ReadDeviceToCloudMessages.java** in een teksteditor van uw keuze. Werk de volgende variabelen bij en sla de wijzigingen in het bestand op.

    | Variabele | Waarde |
    | -------- | ----------- |
    | `EVENT_HUBS_COMPATIBLE_ENDPOINT` | Vervang de waarde van de variabele door het met Event Hubs compatibele eindpunt dat u eerder hebt genoteerd. |
    | `EVENT_HUBS_COMPATIBLE_PATH`     | Vervang de waarde van de variabele door het met Event Hubs compatibele pad dat u eerder hebt genoteerd. |
    | `IOT_HUB_SAS_KEY`                | Vervang de waarde van de variabele door de primaire sleutel service die u eerder hebt genoteerd. |

3. Voer in het lokale terminalvenster de volgende opdrachten uit om de vereiste bibliotheken te installeren en de back-endtoepassing te compileren:

    ```cmd/sh
    mvn clean package
    ```

4. Voer in het lokale terminalvenster de volgende opdrachten uit om de back-endtoepassing uit te voeren:

    ```cmd/sh
    java -jar target/read-d2c-messages-1.0.0-with-deps.jar
    ```

    In de volgende schermafbeelding ziet u de uitvoer op het moment dat de back-endtoepassing telemetriegegevens ontvangt die door het gesimuleerde apparaat naar de hub zijn verzonden:

    ![Uitvoer als back-endtoepassing telemetriegegevens ontvangt die worden verzonden naar uw IoT-hub](media/quickstart-send-telemetry-java/read-device-to-cloud.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een IoT-hub geconfigureerd, een apparaat geregistreerd, gesimuleerde telemetrie verzonden naar de hub met behulp van een Java-toepassing en de telemetrie gelezen van de hub met behulp van een eenvoudige back-endtoepassing.

Ga verder met de volgende snelstartgids als u wilt weten hoe u een gesimuleerd apparaat beheert vanuit een back-endtoepassing.

> [!div class="nextstepaction"]
> [Snelstart: Een apparaat beheren dat is verbonden met een IoT-hub](quickstart-control-device-java.md)
