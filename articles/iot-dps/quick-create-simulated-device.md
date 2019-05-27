---
title: Een gesimuleerd TPM-apparaat inrichten voor Azure IoT Hub met behulp van C | Microsoft Docs
description: In deze snelstart wordt gebruikgemaakt van afzonderlijke inschrijvingen. In deze snelstart maakt u een gesimuleerd TPM-apparaat met de SDK voor C voor Azure IoT Hub Device Provisioning Service en richt u het in.
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: ca6914967d855123c70bf746a9d68d2e045e76d9
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65908673"
---
# <a name="quickstart-provision-a-simulated-tpm-device-using-the-azure-iot-c-sdk"></a>Snelstartgids: Een gesimuleerd TPM-apparaat inrichten met de Azure IoT C SDK

[!INCLUDE [iot-dps-selector-quick-create-simulated-device-tpm](../../includes/iot-dps-selector-quick-create-simulated-device-tpm.md)]

In deze snelstart leert u hoe u een TPM-apparaatsimulator (Trusted Platform Module) op een Windows-ontwikkelcomputer kunt maken en uitvoeren. U verbindt dit gesimuleerde apparaat met een IoT-hub met behulp van een Device Provisioning Service-exemplaar. Er wordt voorbeeldcode van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) gebruikt om u te helpen bij het registreren van het apparaat bij een Device Provisioning Service-exemplaar en een opstartvolgorde voor het apparaat te simuleren.

Raadpleeg [Concepten voor automatische inrichting](concepts-auto-provisioning.md) als u niet bekend bent met het proces van automatisch inrichten. Controleer ook of u de stappen in [IoT Hub Device Provisioning Service instellen met Azure Portal](./quick-setup-auto-provision.md) hebt voltooid voordat u verdergaat met deze snelstart. 

Azure IoT Device Provisioning Service ondersteunt twee typen inschrijvingen:
- [Inschrijvingsgroepen](concepts-service.md#enrollment-group): Wordt gebruikt om meerdere gerelateerde apparaten in te schrijven.
- [Afzonderlijke inschrijvingen](concepts-service.md#individual-enrollment): Wordt gebruikt om één apparaat in te schrijven.

In dit artikel worden afzonderlijke registraties gedemonstreerd.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 of hoger met de [' bureaubladontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) werkbelasting ingeschakeld.
* Meest recente versie van [Git](https://git-scm.com/download/) geïnstalleerd.


<a id="setupdevbox"></a>

## <a name="prepare-a-development-environment-for-the-azure-iot-c-sdk"></a>Een ontwikkelomgeving voorbereiden voor de Azure IoT C SDK

In deze sectie bereidt u een ontwikkelomgeving voor die wordt gebruikt om de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) en het voorbeeld van de [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)-apparaatsimulator te bouwen.

1. Download de [CMake-bouwsysteem](https://cmake.org/download/).

    Het is belangrijk dat de vereisten voor Visual Studio met (Visual Studio en de workload Desktopontwikkeling met C++) op uw computer zijn geïnstalleerd **voordat** de `CMake`-installatie wordt gestart. Zodra aan de vereisten is voldaan en de download is geverifieerd, installeert u het CMake-bouwsysteem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Deze bewerking kan enkele minuten in beslag nemen.


3. Maak de submap `cmake` in de hoofdmap van de Git-opslagplaats en navigeer naar die map. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

## <a name="build-the-sdk-and-run-the-tpm-device-simulator"></a>De SDK bouwen en de TPM-apparaatsimulator uitvoeren

In deze sectie maakt u de Azure IoT C SDK, die de voorbeeldcode van de TPM-apparaatsimulator bevat. Dit voorbeeld biedt een TPM [attestation-mechanisme](concepts-security.md#attestation-mechanism) via SAS-tokenverificatie (Shared Access Signature).

1. Voer vanuit de submap `cmake` die u hebt gemaakt in de azure-iot-sdk-c git-opslagplaats de volgende opdracht uit om het voorbeeld te maken. Er wordt door deze buildopdracht ook een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd.

    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    ```

    Als `cmake` uw C++-compiler niet kan vinden, kunnen er fouten in de build optreden tijdens het uitvoeren van de bovenstaande opdracht. Als dit gebeurt, voert u deze opdracht uit bij de [Visual Studio-opdrachtprompt](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```

2. Navigeer naar de hoofdmap van de git-opslagplaats die u hebt gekloond en voer de [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)-simulator uit via het pad dat hieronder wordt weergegeven. Deze simulator luistert via een socket op poorten 2321 en 2322. Sluit dit opdrachtvenster niet; de simulator moet actief blijven tot u deze snelstart hebt voltooid. 

   Als u de map *cmake* hebt geopend, voert u van daaruit de volgende opdrachten uit:

    ```cmd/sh
    cd ..
    .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
    ```

    U ziet niet alle uitvoer van de simulator. Laat deze verder gaan met het simuleren van een TPM-apparaat.

<a id="simulatetpm"></a>

## <a name="read-cryptographic-keys-from-the-tpm-device"></a>Cryptografische sleutels van het TPM-apparaat lezen

In deze sectie bouwt u een voorbeeld dat de goedkeuringssleutel en registratie-ID leest van de TPM-simulator die u actief hebt en luistert op de poorten 2321 en 2322 en voert u dit voorbeeld uit. Deze waarden worden gebruikt voor de inschrijving van apparaten met uw Device Provisioning Service-exemplaar.

1. Open Visual Studio en open het nieuwe oplossingbestand met de naam `azure_iot_sdks.sln`. Dit oplossingsbestand bevindt zich in de map `cmake` die u eerder hebt gemaakt in de hoofdmap van de azure-iot-sdk-c git-opslagplaats.

2. Selecteer in het menu van Visual Studio, **Build** > **Build Solution** om alle projecten in de oplossing te bouwen.

3. Navigeer in het deelvenster *Solution Explorer* van Visual Studio naar de map **Provision\_Tools**. Klik met de rechtermuisknop op het **tpm_device_provision**-project en selecteer **Set as Startup Project**. 

4. Selecteer in het menu van Visual Studio de optie **Debug** > **Start without debugging** om de oplossing uit te voeren. De app leest een **_registratie-ID_** en een **_goedkeuringssleutel_** en geeft deze weer. Kopieer deze waarden. Deze worden gebruikt in de volgende sectie voor apparaatinschrijving. 


<a id="portalenrollment"></a>

## <a name="create-a-device-enrollment-entry-in-the-portal"></a>Een vermelding voor apparaatregistratie maken in de portal

1. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **Alle bronnen** en open Device Provisioning Service.

2. Selecteer het tabblad **Registraties beheren** en klik vervolgens op de knop **Afzonderlijke inschrijvingen toevoegen** bovenaan. 

3. Voer bij **Registratie toevoegen** de volgende gegevens in en klik op de knop **Opslaan**.

    - **Mechanisme:** Selecteer **TPM** als *mechanisme* voor identiteitscontrole.
    - **Goedkeuringssleutel:** Voer de *goedkeuringssleutel* in die u hebt gegenereerd voor uw TPM-apparaat door het project *tpm_device_provision* uit te voeren.
    - **Registratie-id:** Voer de *registratie-id* in die u hebt gegenereerd voor uw TPM-apparaat door het project *tpm_device_provision* uit te voeren.
    - **IoT Edge-apparaat:** Selecteer **uitschakelen**.
    - **Apparaat-id voor IoT Hub:** Voer **test-docs-device** in als id voor het apparaat.

      ![Gegevens van apparaatinschrijving invoeren in de portal](./media/quick-create-simulated-device/enter-device-enrollment.png)  

      Als het apparaat is ingeschreven, wordt de *Registration ID* ervan weergegeven in de lijst onder het tabblad *Individual Enrollments*. 


<a id="firstbootsequence"></a>

## <a name="simulate-first-boot-sequence-for-the-device"></a>Eerste opstartvolgorde voor het apparaat simuleren

In deze sectie configureert u voorbeeldcode voor het gebruik van het [Advanced Message Queuing Protocol (AMQP)](https://wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) om de opstartvolgorde van het apparaat naar uw Device Provisioning Service-exemplaar te verzenden. Deze opstartvolgorde zorgt ervoor dat het apparaat kan worden herkend en toegewezen aan een IoT-hub die is gekoppeld aan het Device Provisioning Service-exemplaar.

1. Selecteer in Azure Portal het tabblad **Overzicht** voor uw Device Provisioning Service en kopieer de waarde bij **_Id-bereik_**.

    ![Device Provisioning Service-eindpuntgegevens uit de portal extraheren](./media/quick-create-simulated-device/extract-dps-endpoints.png) 

2. Navigeer in het deelvenster *Solution Explorer* van Visual Studio naar de map **Provision\_Samples**. Vouw het voorbeeldproject met de naam **prov\_dev\_client\_sample** uit. Vouw **Source Files** uit en open **prov\_dev\_client\_sample.c**.

3. Zoek boven aan het bestand de instructies `#define` op voor elk apparaatprotocol, zoals hieronder wordt weergegeven. Zorg ervoor dat alleen `SAMPLE_AMQP` geen commentaar bevat.

    Op dit moment wordt het [MQTT-protocol niet ondersteund voor een afzonderlijke TPM-inschrijving](https://github.com/Azure/azure-iot-sdk-c#provisioning-client-sdk).

    ```c
    //
    // The protocol you wish to use should be uncommented
    //
    //#define SAMPLE_MQTT
    //#define SAMPLE_MQTT_OVER_WEBSOCKETS
    #define SAMPLE_AMQP
    //#define SAMPLE_AMQP_OVER_WEBSOCKETS
    //#define SAMPLE_HTTP
    ```

4. Zoek de constante `id_scope` op en vervang de waarde door uw **Id-bereik**-waarde die u eerder hebt gekopieerd. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Zoek de definitie voor de functie `main()` op in hetzelfde bestand. Zorg ervoor dat de variabele `hsm_type` is ingesteld op `SECURE_DEVICE_TYPE_TPM` in plaats van `SECURE_DEVICE_TYPE_X509`, zoals hieronder wordt weergegeven.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

6. Klik met de rechtermuisknop op het **prov\_dev\_client\_sample**-project en selecteer **Set as Startup Project**. 

7. Selecteer in het menu van Visual Studio de optie **Debug** > **Start without debugging** om de oplossing uit te voeren. Klik in de prompt om het project opnieuw te bouwen op **Yes** om het project opnieuw te bouwen voordat het wordt uitgevoerd.

    De volgende uitvoer is een voorbeeld van de voorbeeldcode van de Provisioning Device-client die met succes opstart en verbinding maakt met het Device Provisioning Service-exemplaar om IoT hub-informatie op te halen en zich te registreren:

    ```cmd
    Provisioning API Version: 1.2.7
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED

    Registering... Press enter key to interrupt.

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service:
    test-docs-hub.azure-devices.net, deviceId: test-docs-device
    ```

8. Als het gesimuleerde apparaat is ingericht met de IoT-hub door de Provisioning-service, wordt de apparaat-id weergegeven met de **IoT-apparaten** van de hub. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/quick-create-simulated-device/hub-registration.png) 


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt blijven doorwerken met het voorbeeld van de apparaatclient en deze beter wilt leren kennen, wis de resources die in deze Snelstartgids zijn gemaakt dan niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze Snelstartgids zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het voorbeeld van de apparaatclient op de computer.
2. Sluit het TPM-simulatorvenster op de computer.
3. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer uw Device Provisioning Service. Open het tabblad **Inschrijvingen beheren** voor uw service en klik vervolgens op het tabblad **Afzonderlijke inschrijvingen**. Selecteer de *registratie-id* van het apparaat dat u hebt ingeschreven met behulp van deze quickstart. Klik vervolgens bovenaan op de knop **Verwijderen**. 
4. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Open **IoT-apparaten** voor uw hub, selecteer de *apparaat-id* van het apparaat dat u hebt geregistreerd in deze snelstart en klik vervolgens bovenaan op de knop **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een gesimuleerd TPM-apparaat op de computer gemaakt en dit ingericht voor uw IoT-hub met IoT Hub Device Provisioning Service. Als u wilt weten hoe u uw TPM-apparaat programmatisch kunt registreren, gaat u verder met de quickstart voor programmatische registratie van een TPM-apparaat. 

> [!div class="nextstepaction"]
> [Azure-quickstart: TPM-apparaat registreren bij Azure IoT Hub Device Provisioning Service](quick-enroll-device-tpm-java.md)

