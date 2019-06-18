---
title: Verbinding maken met en beheren van Microsoft Azure Data Box Edge-apparaat via de Windows PowerShell-interface | Microsoft Docs
description: Beschrijft hoe u verbinding maken met en gegevens in Edge vervolgens beheren via de Windows PowerShell-interface.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/25/2019
ms.author: alkohli
ms.openlocfilehash: 8cd89b21e80662ec50746e0c7721a5544cfbce30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717498"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell"></a>Een Azure Data Box-Edge-apparaat via Windows PowerShell beheren

Azure Data Box-Edge-oplossing kunt u gegevens verwerken en te verzenden via het netwerk naar Azure. Dit artikel worden enkele van de configuratie en beheer taken voor uw gegevens in het Edge-apparaat. U kunt de Azure portal, de lokale web-UI of de Windows PowerShell-interface gebruiken om uw apparaat te beheren.

In dit artikel richt zich op de taken die u met behulp van de PowerShell-interface.

In dit artikel bevat de volgende procedures:

- Verbinding maken met de PowerShell-interface
- Een ondersteuningspakket maken
- Certificaat uploaden
- Het apparaat opnieuw instellen
- Gegevens van een apparaat weergeven
- Compute-logboeken ophalen
- Bewaken en problemen oplossen compute-modules

## <a name="connect-to-the-powershell-interface"></a>Verbinding maken met de PowerShell-interface

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Een ondersteuningspakket maken

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Certificaat uploaden

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

U kunt ook IoT Edge-certificaten om in te schakelen van een beveiligde verbinding tussen uw IoT Edge-apparaat en de downstream-apparaten die verbinding met het maken kunnen uploaden. Er zijn drie certificaten voor IoT Edge ( *.pem* indeling) die u nodig hebt om te installeren:

- Basis-CA-certificaat of de eigenaar van de CA
- Device CA-certificaat
- Apparaat-sleutelcertificaat

Het volgende voorbeeld ziet u het gebruik van deze cmdlet om IoT Edge-certificaten te installeren:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username/password"
```

Voor meer informatie over certificaten, gaat u naar [Azure IoT Edge certificaten](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) of [-certificaten installeren op een gateway](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway#install-certificates-on-the-gateway).

## <a name="view-device-information"></a>Gegevens van een apparaat weergeven
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Uw apparaat opnieuw instelt

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Compute-logboeken ophalen

Als de compute-rol is geconfigureerd op uw apparaat, kunt u ook de compute-logboeken ophalen via de PowerShell-interface.

1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Get-AzureDataBoxEdgeComputeRoleLogs` om op te halen van de compute-logboeken voor uw apparaat.

    Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```

    Hier volgt een beschrijving van de parameters voor de cmdlet:
    - `Path`: Geef een netwerkpad naar de bestandsshare waar u de compute-log-pakket maken.
    - `Credential`: Geef de gebruikersnaam en wachtwoord voor de netwerkshare.
    - `RoleInstanceName`: Geef deze tekenreeks `IotRole` voor deze parameter.
    - `FullLogCollection`: Deze parameter zorgt ervoor dat het logboek-pakket alle compute-Logboeken bevat. Het logboek-pakket bevat standaard alleen een subset van Logboeken.

## <a name="monitor-and-troubleshoot-compute-modules"></a>Bewaken en problemen oplossen compute-modules

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/data-box-edge-monitor-troubleshoot-compute.md)]

## <a name="exit-the-remote-session"></a>De externe sessie afsluiten

Sluit het PowerShell-venster om af te sluiten van de externe PowerShell-sessie.

## <a name="next-steps"></a>Volgende stappen

- [Azure Data Box Edge](data-box-edge-deploy-prep.md) in de Azure-portal implementeren.
