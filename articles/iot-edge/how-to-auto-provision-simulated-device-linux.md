---
title: Linux-apparaten automatisch inrichten met DPS-Azure IoT Edge | Microsoft Docs
description: Een gesimuleerd TPM gebruiken op een Linux-VM voor het testen van Azure Device Provisioning Service voor Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/01/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: b48455b6ea9c1cd74e94c10d8f9f938c20512c02
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414571"
---
# <a name="create-and-provision-an-iot-edge-device-with-a-virtual-tpm-on-a-linux-virtual-machine"></a>Een IoT Edge apparaat maken en inrichten met een virtuele TPM op een virtuele Linux-machine

Azure IoT Edge apparaten kunnen automatisch worden ingericht met behulp van de [Device Provisioning Service](../iot-dps/index.yml). Als u niet bekend met het proces van het autoprovisioning bent, raadpleegt u de [autoprovisioning concepten](../iot-dps/concepts-auto-provisioning.md) voordat u doorgaat. 

Dit artikel laat u zien hoe u het autoinrichten op een gesimuleerd IoT Edge apparaat kunt testen met de volgende stappen: 

* Maak een virtuele Linux virtuele machine (VM) in Hyper-V met een gesimuleerde Trusted Platform Module (TPM) voor hardwarebeveiliging.
* Maak een exemplaar van de IoT Hub Device Provisioning Service (DPS).
* Maken van een afzonderlijke inschrijving voor het apparaat
* Installeren van de IoT Edge-runtime en verbind het apparaat met IoT Hub

De stappen in dit artikel zijn bedoeld voor testdoeleinden.

## <a name="prerequisites"></a>Vereisten

* Een Windows-ontwikkelcomputer met [Hyper-V ingeschakeld](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v). In dit artikel maakt gebruik van Windows 10 waarop een Ubuntu-Server-VM wordt uitgevoerd. 
* Een actieve IoT-Hub. 

## <a name="create-a-linux-virtual-machine-with-a-virtual-tpm"></a>Een virtuele Linux-machine maken met een virtuele TPM

In deze sectie maakt u een nieuwe virtuele Linux-machine in Hyper-V. U hebt deze virtuele machine met een gesimuleerde TPM geconfigureerd, zodat u deze kunt gebruiken om te testen hoe automatische inrichting werkt met IoT Edge. 

### <a name="create-a-virtual-switch"></a>Een virtuele switch maken

Een virtuele switch kunt uw virtuele machine verbinding maken met een fysiek netwerk.

1. Open Hyper-V-beheer op de Windows-computer. 

2. In de **acties** in het menu **Virtual Switch Manager**. 

3. Kies een **externe** virtuele switch, en selecteer vervolgens **virtuele Switch maken**. 

4. Geef uw nieuwe virtuele switch een naam, bijvoorbeeld **EdgeSwitch**. Zorg ervoor dat het verbindingstype is ingesteld op **extern netwerk**en selecteer vervolgens **Ok**.

5. Een pop-upvenster waarschuwt u dat de verbinding met het netwerk kan worden onderbroken. Selecteer **Ja** om door te gaan. 

Als u fouten ziet tijdens het maken van de nieuwe virtuele switch, zorg ervoor dat er geen andere switches van de ethernet-adapter gebruikmaakt, en dat er geen andere switches dezelfde naam gebruiken. 

### <a name="create-virtual-machine"></a>Virtuele machine maken

1. Download een schijfimage-bestand wilt gebruiken voor uw virtuele machine en deze lokaal opslaat. Bijvoorbeeld, [Ubuntu server](https://www.ubuntu.com/download/server). 

2. Selecteer in Hyper-V-beheer opnieuw de optie **nieuwe** > **virtuele machine** in het menu **acties** .

3. Voltooi de **Wizard Nieuwe virtuele Machine** met de volgende specifieke configuraties:

   1. **Generatie opgeven**: Selecteer **generatie 2**. Voor virtuele machines van de 2e generatie is geneste virtualisatie ingeschakeld, wat vereist is om IoT Edge uit te voeren op een virtuele machine.
   2. **Netwerken configureren**: Stel de waarde in van **verbinding** met de virtuele switch die u hebt gemaakt in de vorige sectie. 
   3. **Installatie opties**: Selecteer **een besturings systeem installeren vanaf een opstartbaar installatie kopie bestand** en blader naar het schijf kopie bestand dat u lokaal hebt opgeslagen.

4. Selecteer in de wizard **volt ooien** om de virtuele machine te maken.

Het duurt een paar minuten aan de nieuwe virtuele machine maken. 

### <a name="enable-virtual-tpm"></a>Virtuele TPM inschakelen

Zodra de VM is gemaakt, opent u de instellingen om de Virtual trusted platform module (TPM) in te scha kelen waarmee u het apparaat kunt autoinrichten. 

1. Selecteer de virtuele machine en open vervolgens de bijbehorende **instellingen**.

2. Navigeer naar **Security**. 

3. Schakel het selectievakje **beveiligd opstarten inschakelen**.

4. Controleer **inschakelen Trusted Platform Module**. 

5. Klik op **OK**.  

### <a name="start-the-virtual-machine-and-collect-tpm-data"></a>Start de virtuele machine en TPM-gegevens verzamelen

In de virtuele machine, bouwt u een C-SDK-hulpprogramma dat u gebruiken kunt om op te halen van het apparaat **registratie-ID** en **Endorsement Key**. 

1. Start de virtuele machine en maak er verbinding mee.

2. Volg de aanwijzingen in de virtuele machine om het installatie proces te volt ooien en de computer opnieuw op te starten. 

3. Meld u aan bij uw virtuele machine en volg de stappen in [een Linux-ontwikkel omgeving instellen](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) om de Azure IOT Device SDK voor C te installeren en te bouwen. 

   >[!TIP]
   >In het kader van dit artikel kopieert u naar en plakt u de virtuele machine. Dit is niet eenvoudig via de verbindings toepassing Hyper-V-beheer. U kunt een verbinding met de virtuele machine maken via Hyper-V-beheer om het IP-adres `ifconfig`op te halen:. Vervolgens kunt u het IP-adres gebruiken om verbinding te maken via `ssh <username>@<ipaddress>`SSH:.

4. Voer de volgende opdrachten voor het bouwen van een C-SDK-hulpprogramma dat uw device provisioning-gegevens worden opgehaald. 

   ```bash
   cd azure-iot-sdk-c/cmake
   cmake -Duse_prov_client:BOOL=ON ..
   cd provisioning_client/tools/tpm_device_provision
   make
   sudo ./tpm_device_provision
   ```
   >[!TIP]
   >Als u met TPM Simulator wilt testen, moet u een extra para meter `-Duse_tpm_simulator:BOOL=ON` toevoegen om deze in te scha kelen. De volledige opdracht is `cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..`.

5. Kopieer de waarden voor **registratie-ID** en **Endorsement Key**. Deze waarden kunt u een afzonderlijke inschrijving voor uw apparaat in DPS maken. 

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>De IoT Hub Device Provisioning Service instellen

Maak een nieuw exemplaar van de IoT Hub Device Provisioning Service in Azure en een koppeling naar uw IoT hub. U kunt de instructies in [instellen van de IoT Hub-DPS](../iot-dps/quick-setup-auto-provision.md).

Nadat u de Device Provisioning Service die wordt uitgevoerd hebt, kopieert u de waarde van **ID-bereik** van de overzichtspagina. U gebruikt deze waarde bij het configureren van de IoT Edge-runtime. 

## <a name="create-a-dps-enrollment"></a>Maken van een DPS-inschrijving

Ophalen van de Inrichtingsgegevens van uw virtuele machine en maakt u een afzonderlijke inschrijving in Device Provisioning Service. 

Wanneer u een inschrijving in DPS maakt, hebt u de mogelijkheid om te declareren een **oorspronkelijke Apparaatdubbelstatus**. U kunt in de apparaatdubbel labels instellen om apparaten te groeperen door elke meetwaarde die u nodig hebt in uw oplossing, zoals regio, omgeving, locatie, of het apparaat. Deze tags zijn gebruikt voor het maken [automatische implementaties](how-to-deploy-monitor.md). 

1. Ga in het [Azure Portal](https://portal.azure.com)naar uw exemplaar van IOT hub Device Provisioning Service. 

2. Onder **instellingen**, selecteer **registraties beheren**. 

3. Selecteer **afzonderlijke registratie toevoegen** voltooit u de volgende stappen om de registratie te configureren:  

   1. Voor **mechanisme**, selecteer **TPM**. 
   
   2. Geef de **goedkeurings sleutel** en **registratie-id** op die u van de virtuele machine hebt gekopieerd.
   
   3. Selecteer **waar** om te declareren dat deze virtuele machine een IOT edge apparaat is. 
   
   4. Kies de gekoppelde **IoT-Hub** dat u wilt verbinding maken met uw apparaat. U kunt meerdere hubs kiezen en het apparaat wordt toegewezen aan een van deze op basis van het geselecteerde toewijzings beleid. 
   
   5. Als u wilt, moet u een ID opgeven voor uw apparaat. Apparaat-id's kunt u richten op een afzonderlijk apparaat voor de implementatie van beleidsmodule. Als u geen apparaat-ID opgeeft, wordt de registratie-ID gebruikt.
   
   6. Een tagwaarde toevoegen aan de **oorspronkelijke Apparaatdubbelstatus** als u wilt. U kunt tags voor doelgroepen van apparaten gebruiken voor de implementatie van beleidsmodule. Bijvoorbeeld: 

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   7. Selecteer **Opslaan**. 

Nu een inschrijving voor dit apparaat bestaat, kan de IoT Edge runtime automatisch het apparaat inrichten tijdens de installatie. 

## <a name="install-the-iot-edge-runtime"></a>IoT Edge-runtime installeren

De IoT Edge-runtime wordt op alle IoT Edge-apparaten geïmplementeerd. De onderdelen in containers worden uitgevoerd, en kunnen u extra containers implementeren op het apparaat, zodat u kunt de code aan de rand uitvoeren. IoT Edge-runtime installeren op uw virtuele machine. 

Kent uw DPS **ID-bereik** en apparaat **registratie-ID** voordat u begint met het artikel die overeenkomt met uw apparaattype. Als u het voorbeeld van de Ubuntu-server hebt geïnstalleerd, gebruikt u de **x64** instructies. Zorg ervoor dat u de IoT Edge-runtime voor het inrichten van automatische, niet handmatig configureren. 

[Installeer de Azure IoT Edge runtime op Linux](how-to-install-iot-edge-linux.md)

## <a name="give-iot-edge-access-to-the-tpm"></a>IoT Edge toegang geven tot de TPM

In de volgorde voor de IoT Edge-runtime voor het automatisch inrichten van uw apparaat, moet deze toegang hebben tot de TPM. 

U kunt TPM toegang verlenen tot de IoT Edge-runtime door te overschrijven de instellingen voor systemd zodat de **iotedge** service heeft bevoegdheden op hoofdniveau. Als u niet wilt en de service-bevoegdheden te verhogen, kunt u ook de volgende stappen gebruiken handmatig TPM om toegang te bieden. 

1. Het pad naar de TPM-hardware-module vinden op uw apparaat en sla deze op als een lokale variabele. 

   ```bash
   tpm=$(sudo find /sys -name dev -print | fgrep tpm | sed 's/.\{4\}$//')
   ```

2. Maak een nieuwe regel waarmee wordt de IoT Edge runtime toegang tot tpm0. 

   ```bash
   sudo touch /etc/udev/rules.d/tpmaccess.rules
   ```

3. Open het regelbestand. 

   ```bash
   sudo nano /etc/udev/rules.d/tpmaccess.rules
   ```

4. Kopieer de volgende toegang tot gegevens in het regelbestand. 

   ```input 
   # allow iotedge access to tpm0
   KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   ```

5. Opslaan en sluiten van het bestand. 

6. Activeer de udev-systeem om te evalueren van de nieuwe regel. 

   ```bash
   /bin/udevadm trigger $tpm
   ```

7. Controleer of dat de regel is toegepast.

   ```bash
   ls -l /dev/tpm0
   ```

   Geslaagde uitvoer ziet er als volgt uit:

   ```output
   crw-rw---- 1 root iotedge 10, 224 Jul 20 16:27 /dev/tpm0
   ```

   Als u niet ziet dat de juiste machtigingen zijn toegepast, probeer het opnieuw opstarten van uw computer om te vernieuwen udev. 

8. Open de IoT Edge-runtime overschrijft bestand. 

   ```bash
   sudo systemctl edit iotedge.service
   ```

9. Voeg de volgende code voor het maken van een TPM-omgevingsvariabele.

   ```input
   [Service]
   Environment=IOTEDGE_USE_TPM_DEVICE=ON
   ```

10. Opslaan en sluiten van het bestand.

11. Controleren of de onderdrukking geslaagd is.

    ```bash
    sudo systemctl cat iotedge.service
    ```

    Geslaagde uitvoer wordt weergegeven de **iotedge** standaard variabelen voor de service en geeft u vervolgens de omgevingsvariabele op de dat u instelt **override.conf**. 

12. De instellingen voor laden.

    ```bash
    sudo systemctl daemon-reload
    ```

## <a name="restart-the-iot-edge-runtime"></a>Opnieuw opstarten van de IoT Edge-runtime

IoT Edge-runtime opnieuw zodat deze neemt alle configuratiewijzigingen die u hebt aangebracht op het apparaat. 

   ```bash
   sudo systemctl restart iotedge
   ```

Controleer of de IoT Edge-runtime wordt uitgevoerd. 

   ```bash
   sudo systemctl status iotedge
   ```

Als u fouten bij het inrichten ziet, kan het zijn dat de wijzigingen in de configuratie zijn niet doorgevoerd nog. Probeer opnieuw de IoT Edge-daemon opnieuw op te starten. 

   ```bash
   sudo systemctl daemon-reload
   ```
   
Of probeer het opnieuw opstarten van uw virtuele machine om te zien als de wijzigingen van kracht op een nieuwe start. 

## <a name="verify-successful-installation"></a>Controleer of geslaagde installatie

Als de runtime is gestart, kunt u met ingang van uw IoT-Hub en zien dat het nieuwe apparaat automatisch is ingericht. Uw apparaat is nu gereed voor het IoT Edge-modules uitvoeren. 

Controleer de status van de IoT Edge-Daemon.

```cmd/sh
systemctl status iotedge
```

Controleer de daemon-Logboeken.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Lijst met modules.

```cmd/sh
iotedge list
```

U kunt controleren of de afzonderlijke registratie die u hebt gemaakt in Device Provisioning Service is gebruikt. Navigeer naar het Device Provisioning service-exemplaar in het Azure Portal. Open de inschrijvings gegevens voor de afzonderlijke inschrijving die u hebt gemaakt. U ziet dat de status van de registratie is **toegewezen** en dat de apparaat-id wordt vermeld. 

## <a name="next-steps"></a>Volgende stappen

Het inschrijvingsproces Device Provisioning Service kunt u instellen de apparaat-ID en het apparaat dubbele tags op hetzelfde moment als u het nieuwe apparaat inrichten. Deze waarden kunt u afzonderlijke apparaten of groepen van apparaten met behulp van automatische Apparaatbeheer. Meer informatie over het [implementeren en te bewaken IoT Edge-modules op schaal met Azure portal](how-to-deploy-monitor.md) of [met behulp van Azure CLI](how-to-deploy-monitor-cli.md).
