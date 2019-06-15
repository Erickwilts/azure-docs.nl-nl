---
title: Azure Resource Manager-scripts gebruiken voor het beheren van StorSimple-apparaten | Microsoft Docs
description: Informatie over het gebruik van Azure Resource Manager-scripts voor het automatiseren van taken voor StorSimple
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/03/2017
ms.author: alkohli
ms.openlocfilehash: 646b862733e8727c9c8729f1ac038fa88cfa0580
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60726414"
---
# <a name="use-azure-resource-manager-sdk-based-scripts-to-manage-storsimple-devices"></a>Op basis van een Azure Resource Manager SDK-scripts gebruiken voor het beheren van StorSimple-apparaten

Dit artikel wordt beschreven hoe op SDK van Azure Resource Manager gebaseerde scripts kunnen worden gebruikt voor het beheren van uw StorSimple 8000-apparaat. Een voorbeeld van een script is ook opgenomen om u te helpen u bij de stappen voor het configureren van uw omgeving voor het uitvoeren van deze scripts.

In dit artikel is van toepassing op StorSimple 8000-serie-apparaten die worden uitgevoerd in Azure portal bevat.

## <a name="sample-scripts"></a>Voorbeeldscripts

De volgende voorbeelden van scripts zijn beschikbaar voor het automatiseren van verschillende taken voor StorSimple.

#### <a name="table-of-azure-resource-manager-sdk-based-sample-scripts"></a>Tabel met de SDK van Azure Resource Manager gebaseerde voorbeeldscripts

| Azure Resource Manager-Script                    | Description                                                                                                                                                                                                       |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Authorize-ServiceEncryptionRollover.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Authorize-ServiceEncryptionRollover.ps1)          | Met dit script kunt u uw StorSimple-apparaat te wijzigen van de versleutelingssleutel voor servicegegevens autoriseren.                                                                                                           |
| [Create-StorSimpleCloudAppliance.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Create-StorSimpleCloudAppliance.ps1)              | Dit script maakt een 8010 of 8020 StorSimple Cloud Appliance. Het cloudapparaat kan vervolgens worden geconfigureerd en geregistreerd bij uw StorSimple Data Manager-service.                                                       |
| [CreateOrUpdate-Volume.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/CreateOrUpdate-Volume.ps1)                        | Met dit script maakt of wijzigt u StorSimple-volumes.                                                                                                                                                             |
| [Get-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackup.ps1)                             | Met dit script geeft een lijst van alle de back-ups voor een apparaat is geregistreerd bij uw StorSimple Device Manager-service.                                                                                                          |
| [Get-DeviceBackupPolicy.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackupPolicy.ps1)                       | Dit script alle back-upbeleid voor uw StorSimple-apparaat.                                                                                                                                                 |
| [Get-DeviceJobs.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1)                               | Met dit script worden alle van de StorSimple-taken die worden uitgevoerd op uw StorSimple Device Manager-service opgehaald.                                                                                                                     |
| [Get-DeviceUpdateAvailability.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceUpdateAvailability.ps1)                 | Met dit script scant de updateserver en laat u weten dat als er updates zijn beschikbaar voor installatie op uw StorSimple-apparaat.                                                                                          |
| [Install-DeviceUpdate.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Install-DeviceUpdate.ps1)                         | Met dit script worden de beschikbare updates geïnstalleerd op uw StorSimple-apparaat.                                                                                                                                           |
| [Manage-CloudSnapshots.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Manage-CloudSnapshots.ps1)                        | Met dit script wordt een handmatige cloud-momentopname gestart en cloudmomentopnamen die ouder zijn dan de opgegeven bewaartermijn dagen wordt verwijderd.                                                                                                   |
| [Monitor-Backups.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1)                              | Deze Azure Automation-Runbook PowerShell-script rapporteert de status van alle back-uptaken.                                                                                                              |
| [Remove-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Remove-DeviceBackup.ps1)                          | Met dit script wordt één back-object verwijderd.                                                                                                                                                           |
| [Start-DeviceBackupJob.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Start-DeviceBackupJob.ps1)                        | Dit script wordt een handmatige back-up van uw StorSimple-apparaat.                                                                                                                                       |
| [Update-CloudApplianceServiceEncryptionKey.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Update-CloudApplianceServiceEncryptionKey.ps1)    | Met dit script werkt de versleutelingssleutel voor servicegegevens voor alle 8010/8020 StorSimple-Cloudapparaten geregistreerd bij uw StorSimple Device Manager-service.                                     |
| [Verify-BackupScheduleAndBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Verify-BackupScheduleAndBackup.ps1)               | Met dit script ziet u de ontbrekende back-ups na het analyseren van alle schema's die zijn gekoppeld aan back-upbeleid. Hij controleert ook of de back-catalogus met de lijst van beschikbare back-ups.             |




## <a name="configure-and-run-a-sample-script"></a>Configureren en een voorbeeld van een script uitvoeren

Deze sectie wordt een voorbeeld-script en details van de verschillende stappen die nodig zijn om uit te voeren van het script.

### <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u hebt:

*   Azure PowerShell is geïnstalleerd. Azure PowerShell-modules installeren:
    * In een Windows-omgeving, volg de stappen in [installeren en configureren van Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). U kunt Azure PowerShell installeren op uw Windows Server-host voor uw StorSimple als met behulp van een.
    * In een omgeving met Linux of MacOS, volg de stappen in [installeren en configureren van Azure PowerShell in MacOS of Linux](https://docs.microsoft.com/powershell/azure/azurerm/install-azurermps-maclinux).

Voor meer informatie over het gebruik van Azure PowerShell, gaat u naar [aan de slag met behulp van Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

### <a name="run-azure-powershell-script"></a>Azure PowerShell-script uitvoeren

Het script dat wordt gebruikt in dit voorbeeld geeft een lijst van alle taken op een StorSimple-apparaat. Dit omvat de taken die is geslaagd, mislukt of worden uitgevoerd. De volgende stappen uitvoeren om te downloaden en uitvoeren van het script.

1. Start Azure PowerShell. Maak een nieuwe map en wijzig de map naar de nieuwe map.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet-CLI downloaden](https://www.nuget.org/downloads) onder de map in de vorige stap hebt gemaakt. Er zijn verschillende versies van _nuget.exe_. Kies de versie die overeenkomt met de SDK. Elke downloadkoppeling verwijst rechtstreeks naar een _.exe_ bestand. Zorg ervoor dat u met de rechtermuisknop op en sla het bestand op uw computer in plaats van deze uitvoert vanuit de browser.

    U kunt ook de volgende opdracht uit om te downloaden en opslaan van het script in dezelfde map die u eerder hebt gemaakt, uitvoeren.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Download de SDK afhankelijk is.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Het script downloaden uit het voorbeeld van GitHub-project.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1

    ```

5. Voer het script uit. Wanneer u hierom wordt gevraagd om te verifiëren, Geef uw Azure-referenties. Met dit script moet uitvoer van een gefilterde lijst van alle taken die op uw StorSimple-apparaat.
           
    ```           
        .\Get-StorSimpleJob.ps1 -SubscriptionId [subid] -TenantId [tenant id] -DeviceName [name of device] -ResourceGroupName [name of resource group] -ManagerName[name of device manager] -FilterByStatus [Filter for job status] -FilterByJobType [Filter for job type] -FilterByStartTime [Filter for start date time] -FilterByEndTime [Filter for end date time]

    ```

### <a name="sample-output"></a>Voorbeelduitvoer

De volgende uitvoer wordt weergegeven wanneer het voorbeeld van een script wordt uitgevoerd. De uitvoer bevat alle taken die zijn uitgevoerd op een ingeschreven apparaat dat aan de slag op 25 September 2017 en uitgevoerd door 2 oktober 2017.

```
-----------------------------------------
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

PS C:\Scripts\StorSimpleSDKTools> wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1
PS C:\Scripts\StorSimpleSDKTools> .\Get-DeviceJobs.ps1 -SubscriptionId 1234ab5c-678d-910e-9fc4-0accc9c0166e -TenantId 12a345bc-67d8-91ef-01ab-2c7cd123ef45 -DeviceName 8600-ABC1234567D89EF -ResourceGroupName Contoso -ManagerName ContosoDeviceMgr -FilterByStartTime "09/25/2017 08:10:02" -FilterByEndTime "10/02/2017 08:10:02"


Status            : Succeeded
StartTime         : 10/2/2017 10:30:02 PM
EndTime           : 10/2/2017 10:31:36 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : ss-asr-policy1
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC/jobs/75905c48-b153-4af1-8b21-4b9a2ff9
                    825b
Name              : 75905c48-b153-4af1-8b21-4b9a2ff9825b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000
-------------------------------------------
CUT              CUT  
-------------------------------------------
Status            : Succeeded
StartTime         : 9/26/2017 5:00:02 PM
EndTime           : 9/26/2017 5:01:20 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : 8010 policy
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF/jobs/3cfd8108-db60-4e9a-a8da-6d8fe457
                    8d2b
Name              : 3cfd8108-db60-4e9a-a8da-6d8fe4578d2b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000



PS C:\Scripts\StorSimpleSDKTools>
--------------------------------------------

```


## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).