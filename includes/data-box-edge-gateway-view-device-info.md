---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: d5af557a62f4bd35c242d334c28a38c3d632f7cf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176495"
---
1. [Verbinding maken met de PowerShell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Get-HcsApplianceInfo` om op te halen van de gegevens voor uw apparaat.

    Het volgende voorbeeld ziet u het gebruik van deze cmdlet:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Hier volgt een tabel met een overzicht van enkele van de informatie van belangrijke apparaten:
    
    | Parameter                             | Description                                                                                                                                                  |   |
    |--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    | FriendlyName                   | De beschrijvende naam van het apparaat geconfigureerd via de lokale webgebruikersinterface tijdens de implementatie van het apparaat. De beschrijvende naam die standaard is het serienummer van het apparaat.  |   |
    | Serienummer                   | Het serienummer van het apparaat is een uniek nummer dat is toegewezen in de fabriek.                                                                             |   |
    | Model                          | Het model voor uw gegevens in Microsoft Edge of Data Box-Gateway-apparaat. Het model is voor de Data Box Gateway virtuele en fysieke voor gegevens in Edge.                   |   |
    | FriendlySoftwareVersion        | De beschrijvende tekenreeks die overeenkomt met de versie van de apparaat-software. Voor een systeem waarop preview wordt uitgevoerd, is de beschrijvende softwareversie Data Box Edge 1902. |   |
    | HcsVersion                     | De versie van de HCS software is uitgevoerd op uw apparaat. Bijvoorbeeld, is de versie van de HCS-software overeenkomt met de gegevens in Edge 1902 1.4.771.324.            |   |
    | LocalCapacityInMb              | De totale lokale capaciteit van het apparaat in MB.                                                                                                        |   |
    | IsRegistered                   | Deze waarde geeft aan dat als uw apparaat wordt geactiveerd met de service.                                                                                         |   |


