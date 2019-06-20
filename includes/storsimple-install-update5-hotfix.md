---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 19d2dedc2ccf7015696504a94f5ef7c43a90d3be
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176232"
---
#### <a name="to-download-hotfixes"></a>Hotfixes downloaden

Voer de volgende stappen uit om de software-update te downloaden uit de Microsoft Update-catalogus.

1. Start Internet Explorer en navigeer naar [ http://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).
2. Als dit de eerste keer is dat u de Microsoft Update-catalogus op deze computer gebruikt, klikt u op **Installeren** wanneer u wordt gevraagd of u de invoegtoepassing voor de Microsoft Update-catalogus wilt installeren.

    ![Catalogus installeren](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Voer in het zoekvak van de Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u downloaden wilt, bijvoorbeeld **4037264**, en klik vervolgens op **zoeken**.
   
    De lijst met hotfixes wordt weergegeven, bijvoorbeeld **cumulatieve Software bundel Update 5.0 voor StorSimple 8000-serie**.
   
    ![Catalogus doorzoeken](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. Klik op **Downloaden**. Typ of **blader naar** een lokale locatie waar u de downloads wilt weergeven. Klik op de bestanden te downloaden naar de opgegeven locatie en de map. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.
5. Zoeken naar aanvullende hotfixes die worden vermeld in de bovenstaande tabel (**4037266**), en de bijbehorende bestanden downloaden naar de specifieke mappen, zoals vermeld in de voorgaande tabel.

> [!NOTE]
> De hotfixes moeten toegankelijk zijn via beide controllers voor het detecteren van mogelijke foutmeldingen vanuit de peer-controller.
>
> De hotfixes moeten naar 3 afzonderlijke mappen worden gekopieerd. Bijvoorbeeld, het bijwerken van het configuratie-items/software/MDS-agent kan worden gekopieerd _FirstOrderUpdate_ map, alle andere niet-storende updates kunnen worden gekopieerd de _SecondOrderUpdate_ map, en updates voor de onderhoudsmodus gekopieerd _ThirdOrderUpdate_ map.

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Hotfixes in de normale modus installeren en controleren

Voer de volgende stappen uit om hotfixes in de normale modus te installeren en te controleren. Als u al hebt geïnstalleerd met behulp van de Azure-portal, gaat u verder met [installeren en controleren van hotfixes in de onderhoudsmodus](#to-install-and-verify-maintenance-mode-hotfixes).

1. U installeert de hotfixes door de Windows PowerShell-interface op de seriële console van het StorSimple-apparaat te openen. Volg de gedetailleerde instructies in [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). Druk op de opdrachtregel op **Enter**.
2. Selecteer optie 1, **Meld u aan met volledige toegang tot de**. U wordt geadviseerd om de hotfix eerst op de passieve controller te installeren.
3. Installeer de hotfix door achter de opdrachtprompt het volgende te typen:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Gebruik in bovenstaande opdracht IP in plaats van DNS in het Pad naar share. De referentieparameter wordt alleen gebruikt voor toegang tot een geverifieerde share.
   
    U wordt geadviseerd om de referentieparameter te gebruiken voor toegang tot shares. Zelfs shares die voor 'iedereen' zijn geopend, zijn gewoonlijk niet geopend voor niet-geverifieerde gebruikers.
   
4. Geef het wachtwoord op wanneer dit wordt gevraagd. Hieronder ziet u een voorbeeld van de uitvoer voor de installatie van belangrijkste updates. Voor de eerste bijwerken moet u verwijzen naar het specifieke bestand.

    >[!NOTE] 
    > U moet installeren de _HcsSoftwareUpdate.exe_ eerste. Nadat deze installatie is voltooid, klikt u vervolgens installeren _CisMdsAgentUpdate.exe_.
   
        ```
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ```
5. Typ **J** wanneer u wordt gevraagd de hotfixinstallatie te bevestigen.
6. Bewaak de update met behulp van de cmdlet `Get-HcsUpdateStatus`. De update wordt eerst voltooid op de passieve controller. Zodra de passieve controller is bijgewerkt, wordt er een failover uitgevoerd en wordt de update toegepast op de andere controller. De update is voltooid wanneer beide controllers zijn bijgewerkt.
   
    Hieronder ziet u een voorbeeld van uitvoer terwijl de update nog bezig is. De `RunInprogress` is `True` wanneer de update wordt uitgevoerd.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     In de volgende voorbeelduitvoer wordt aangegeven dat de update is voltooid. De `RunInProgress` is `False` wanneer de update is voltooid.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Af en toe meldt de cmdlet `False` wanneer de update nog wordt uitgevoerd. Als u er zeker van wilt zijn dat de hotfix is voltooid, wacht u enkele minuten en voert u deze opdracht opnieuw uit. Controleer of de `RunInProgress` `False` is. Als dat het geval is, is de hotfix voltooid.

7. Wanneer de software-update is voltooid, controleert u de versies van de systeemsoftware. Type:
   
    `Get-HcsSystem`
   
    De volgende versies moeten worden weergegeven:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   * `HcsSoftwareVersion: 6.3.9600.17845`
   
     Als het versienummer niet is gewijzigd nadat de update is toegepast, kon de hotfix blijkbaar niet worden toegepast. Neem in dat geval contact op met [Microsoft Support](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) voor verdere hulp.
     
     > [!IMPORTANT]
     > U moet opnieuw opstarten van de actieve controller via de `Restart-HcsController` cmdlet voordat u de volgende update is toegepast.
     
8. Herhaal stap 3-6 voor het installeren van de _CisMDSAgentupdate.exe_ agent gedownload naar uw _FirstOrderUpdate_ map.
8. Herhaal stap 3-6 voor het installeren van de tweede belangrijkste updates. 

    > [!NOTE] 
    > Voor de tweede belangrijkste updates, meerdere updates kunnen worden geïnstalleerd door gewoon de `Start-HcsHotfix cmdlet` en die verwijst naar de map waar de tweede belangrijkste updates zich bevinden. De cmdlet voert alle updates in de map uit. Als een update al is geïnstalleerd, wordt dit door de updatelogica gedetecteerd en wordt die update niet toegepast.

    Nadat alle hotfixes zijn geïnstalleerd, gebruikt u de cmdlet `Get-HcsSystem`. De versies moeten zijn:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Hotfixes in de onderhoudsmodus installeren en controleren

Gebruik KB4037263 schijf firmware-updates te installeren. Dit zijn updates waarvoor de computer opnieuw moet worden opgestart. De uitvoering ervan neemt ongeveer 30 minuten in beslag. U kunt ervoor kiezen om deze te installeren in een gepland onderhoudsvenster door verbinding te maken met de console van het seriële apparaat.

> [!NOTE] 
> Als de schijffirmware al bijgewerkt is, hoeft u deze updates te installeren. Voer de cmdlet `Get-HcsUpdateAvailability` uit vanaf de console van het seriële apparaat om te controleren of er updates beschikbaar zijn en of de computer voor de updates opnieuw moet worden opgestart (onderhoudsmodus) of niet (normale modus).

Volg onderstaande instructies om de updates van de schijffirmware te installeren.

1. Plaats het apparaat in de onderhoudsmodus. 

    > [!NOTE] 
    > Gebruik Windows PowerShell voor externe toegang niet bij het verbinden met een apparaat in de onderhoudsmodus bevindt. In plaats daarvan moet u deze cmdlet uitvoeren op de apparaatcontroller wanneer verbonden zijn via de seriële console van het apparaat.

    Als u wilt de controller in de onderhoudsmodus plaatst, typt u:
   
    `Enter-HcsMaintenanceMode`
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Beide controllers starten vervolgens opnieuw op in de onderhoudsmodus.
2. Typ het volgende om de update van de schijffirmware te installeren:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Bewaak de installatievoortgang met de opdracht `Get-HcsUpdateStatus`. De update is voltooid als de `RunInProgress` verandert in `False`.
4. Wanneer de installatie is voltooid, wordt de controller waarop de hotfix van de onderhoudsmodus is geïnstalleerd, opnieuw opstart. Meld u aan in optie 1, **Meld u aan met volledige toegang tot de**, en controleer of de versie van de schijffirmware. Type:
   
   `Get-HcsFirmwareVersion`
   
   De verwachte versies van de schijffirmware zijn:
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Hieronder ziet u een voorbeeld van de uitvoer.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    Voer op de tweede controller de opdracht `Get-HcsFirmwareVersion` uit om te controleren of de versie van de software is bijgewerkt. Daarna kunt u de onderhoudsmodus afsluiten. Daartoe typt u de volgende opdracht voor elke apparaatcontroller:
   
   `Exit-HcsMaintenanceMode`

5. De controllers starten opnieuw op wanneer u de onderhoudsmodus afsluit. Na de schijffirmware updates met succes zijn toegepast en de onderhoudsmodus, Ga terug naar de Azure-portal is afgesloten. Het is mogelijk dat in de portal gedurende 24 uur niet wordt weergegeven dat u de updates van de onderhoudsmodus hebt geïnstalleerd.

