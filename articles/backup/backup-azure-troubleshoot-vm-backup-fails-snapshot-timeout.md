---
title: 'Azure Backup fout oplossen: De status van de gast agent is niet beschikbaar'
description: Symptomen, oorzaken en oplossingen voor Azure Backup fouten met betrekking tot agent, uitbrei ding en schijven.
ms.reviewer: saurse
author: dcurwin
manager: carmonm
keywords: Azure backup; VM-agent; Netwerk verbinding;
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: dacurwin
ms.openlocfilehash: f47afd450350226aa944287e756b73f61b15b32d
ms.sourcegitcommit: acffa72239413c62662febd4e39ebcb6c6c0dd00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952049"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup fout oplossen: Problemen met de agent of uitbrei ding

Dit artikel bevat probleemoplossings stappen die u kunnen helpen bij het oplossen van Azure Backup fouten met betrekking tot communicatie met de VM-agent en-extensie.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]



## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable: de VM-agent kan niet communiceren met Azure Backup

**Fout code**: UserErrorGuestAgentStatusUnavailable <br>
**Fout bericht**: De VM-agent kan niet communiceren met Azure Backup<br>

De Azure VM-agent is mogelijk gestopt, verouderd, in een inconsistente status of niet geïnstalleerd en voor komen dat Azure Backup Service moment opnamen kan activeren.  
    
- Als de VM-agent is gestopt of niet consistent is, start u **de agent opnieuw** en voert u de back-upbewerking opnieuw uit (probeer een ad-hoc back-up). Zie [Windows vm's](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) of [Linux-vm's](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent)voor stappen om de agent opnieuw te starten. 
- Als de VM-agent niet is geïnstalleerd of is verouderd, installeert u de VM-agent en voert u de back-upbewerking opnieuw uit. Zie [Windows vm's](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) of [Linux-vm's](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent)voor stappen om de agent te installeren of bij te werken.  

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError: kan niet communiceren met de VM-agent voor de status van de moment opname

**Fout code**: GuestAgentSnapshotTaskStatusError<br>
**Fout bericht**: Kan niet communiceren met de VM-agent voor de status van de momentopname <br>

Nadat u een virtuele machine voor de Azure Backup-service hebt geregistreerd en gepland, wordt met de back-up de taak gestart door te communiceren met de back-upextensie van de VM om een tijdgebonden moment opname te maken. Een van de volgende situaties kan verhinderen dat de moment opname wordt geactiveerd. Als de moment opname niet wordt geactiveerd, kan er een back-upfout optreden. Voer de volgende stappen voor probleem oplossing in de aangegeven volg orde uit en voer de bewerking opnieuw uit:  
**Oorzaak 1: [De agent is geïnstalleerd op de VM, maar reageert niet (voor Windows-Vm's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Oorzaak 2: [De agent die is geïnstalleerd in de virtuele machine is verouderd (voor Linux-Vm's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Oorzaak 3: [De status van de moment opname kan niet worden opgehaald, of een moment opname kan niet worden gemaakt](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**     
**Oorzaak 4: [De back-upextensie kan niet worden bijgewerkt of geladen](#the-backup-extension-fails-to-update-or-load)** 

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached-de maximale limiet voor de verzameling met herstel punten is bereikt

**Fout code**: UserErrorRpCollectionLimitReached <br>
**Fout bericht**: De maximale limiet voor de verzameling met herstel punten is bereikt. <br>
* Dit probleem kan zich voordoen als er een vergren deling op de resource groep voor herstel punten optreedt waardoor het automatisch opruimen van het herstel punt wordt voor komen.
* Dit probleem kan ook optreden als meerdere back-ups per dag worden geactiveerd. Momenteel wordt slechts één back-up per dag aanbevolen, omdat de onmiddellijke herstel punten gedurende 1-5 dagen worden bewaard volgens de geconfigureerde moment opname en slechts 18 direct RPs kunnen worden gekoppeld aan een VM op een bepaald moment. <br>

Aanbevolen actie:<br>
U kunt dit probleem oplossen door de vergren deling van de resource groep van de virtuele machine te verwijderen en de bewerking opnieuw uit te voeren om het opschonen te activeren.
> [!NOTE]
> Backup-service maakt een afzonderlijke resource groep dan de resource groep van de virtuele machine om de herstel punt verzameling op te slaan. Klanten wordt aangeraden de resource groep die is gemaakt voor gebruik door de back-upservice niet te vergren delen. De naamgevings indeling van de resource groep die is gemaakt door de back-upservice is: AzureBackupRG_`<Geo>`_`<number>` bijvoorbeeld: AzureBackupRG_northeurope_1

**Stap 1: [Vergren deling van de resource groep voor het herstel punt verwijderen](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Stap 2: [Herstel punt verzameling opruimen](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured-backup heeft niet voldoende machtigingen voor de sleutel kluis voor het maken van een back-up van versleutelde Vm's

**Fout code**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Fout bericht**: Backup heeft niet voldoende machtigingen voor de sleutel kluis voor het maken van een back-up van versleutelde Vm's. <br>

Als u een back-upbewerking op versleutelde Vm's wilt uitvoeren, moet deze zijn gemachtigd voor toegang tot de sleutel kluis. U kunt dit doen met behulp van de [Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) of via [Power shell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork-momentopname bewerking is mislukt omdat er geen netwerk verbinding is op de virtuele machine

**Fout code**: ExtensionSnapshotFailedNoNetwork<br>
**Fout bericht**: Momentopname mislukt omdat er geen netwerkverbinding beschikbaar is op de virtuele machine<br>

Nadat u een virtuele machine voor de Azure Backup-service hebt geregistreerd en gepland, wordt met de back-up de taak gestart door te communiceren met de back-upextensie van de VM om een tijdgebonden moment opname te maken. Een van de volgende situaties kan verhinderen dat de moment opname wordt geactiveerd. Als de moment opname niet wordt geactiveerd, kan er een back-upfout optreden. Voer de volgende stappen voor probleem oplossing in de aangegeven volg orde uit en voer de bewerking opnieuw uit:    
**Oorzaak 1: [De status van de moment opname kan niet worden opgehaald, of een moment opname kan niet worden gemaakt](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Oorzaak 2: [De back-upextensie kan niet worden bijgewerkt of geladen](#the-backup-extension-fails-to-update-or-load)**  
**Oorzaak 3: [De virtuele machine heeft geen Internet toegang](#the-vm-has-no-internet-access)**

## <a name="ExtensionOperationFailed-vmsnapshot-extension-operation-failed"></a>Bewerking voor ExtensionOperationFailedForManagedDisks-VMSnapshot-extensie is mislukt

**Fout code**: ExtensionOperationFailedForManagedDisks <br>
**Fout bericht**: Bewerking van VMSnapshot-extensie mislukt<br>

Nadat u een virtuele machine voor de Azure Backup-service hebt geregistreerd en gepland, wordt met de back-up de taak gestart door te communiceren met de back-upextensie van de VM om een tijdgebonden moment opname te maken. Een van de volgende situaties kan verhinderen dat de moment opname wordt geactiveerd. Als de moment opname niet wordt geactiveerd, kan er een back-upfout optreden. Voer de volgende stappen voor probleem oplossing in de aangegeven volg orde uit en voer de bewerking opnieuw uit:  
**Oorzaak 1: [De status van de moment opname kan niet worden opgehaald, of een moment opname kan niet worden gemaakt](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Oorzaak 2: [De back-upextensie kan niet worden bijgewerkt of geladen](#the-backup-extension-fails-to-update-or-load)**  
**Oorzaak 3: [De agent is geïnstalleerd op de VM, maar reageert niet (voor Windows-Vm's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Oorzaak 4: [De agent die is geïnstalleerd in de virtuele machine is verouderd (voor Linux-Vm's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed/BackUpOperationFailedV2-back-up mislukt, met een interne fout

**Fout code**: BackUpOperationFailed / BackUpOperationFailedV2 <br>
**Fout bericht**: De back-up is mislukt met een interne fout. Voer de bewerking over een paar minuten opnieuw uit <br>

Nadat u een virtuele machine voor de Azure Backup-service hebt geregistreerd en gepland, wordt met de back-up de taak gestart door te communiceren met de back-upextensie van de VM om een tijdgebonden moment opname te maken. Een van de volgende situaties kan verhinderen dat de moment opname wordt geactiveerd. Als de moment opname niet wordt geactiveerd, kan er een back-upfout optreden. Voer de volgende stappen voor probleem oplossing in de aangegeven volg orde uit en voer de bewerking opnieuw uit:  
**Oorzaak 1: [De agent die is geïnstalleerd op de VM, maar niet reageert (voor Windows-Vm's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Oorzaak 2: [De agent die is geïnstalleerd in de virtuele machine is verouderd (voor Linux-Vm's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Oorzaak 3: [De status van de moment opname kan niet worden opgehaald, of een moment opname kan niet worden gemaakt](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Oorzaak 4: [De back-upextensie kan niet worden bijgewerkt of geladen](#the-backup-extension-fails-to-update-or-load)**  
**Oorzaak 5: De back-upservice is niet gemachtigd om de oude herstel punten te verwijderen vanwege een vergren deling van een resource groep** <br>
**Oorzaak 6: [De virtuele machine heeft geen Internet toegang](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-4095gb"></a>UserErrorUnsupportedDiskSize-momenteel Azure Backup biedt geen ondersteuning voor schijf grootten die groter zijn dan 4095 GB

**Fout code**: UserErrorUnsupportedDiskSize <br>
**Fout bericht**: Momenteel Azure Backup biedt geen ondersteuning voor schijf grootten die groter zijn dan 4095 GB <br>

De back-upbewerking kan mislukken bij het maken van een back-up van een virtuele machine met een schijf grootte groter dan 4.095 GB. Als u zich wilt aanmelden voor een beperkte open bare preview van Azure Backup ondersteuning voor grote schijven voor schijven die groter zijn dan 4 TB en tot Maxi maal 30 TB groot zijn, raadpleegt u [een overzicht van Azure VM-back-up](backup-azure-vms-introduction.md#limited-public-preview-backup-of-vm-with-disk-sizes-up-to-30tb).

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>UserErrorBackupOperationInProgress: kan geen back-up initiëren omdat er momenteel een andere back-upbewerking wordt uitgevoerd

**Fout code**: UserErrorBackupOperationInProgress <br>
**Fout bericht**: De back-up kan niet worden gestart omdat er momenteel een andere back-upbewerking wordt uitgevoerd<br>

De recente back-uptaak is mislukt, omdat er een bestaande back-uptaak wordt uitgevoerd. U kunt pas een nieuwe back-uptaak starten als de huidige taak is voltooid. Controleer of de back-upbewerking die momenteel wordt uitgevoerd, is voltooid voordat u een andere back-up maakt of plant. Voer de onderstaande stappen uit om de status van de back-uptaken te controleren:

1. Meld u aan bij de Azure-portal en klik op **Alle services**. Typ Recovery Services en klik op **Recovery Services-kluizen**. De lijst met Recovery Services-kluizen wordt weergegeven.
2. Selecteer in de lijst met Recovery Services-kluizen een kluis waarin de back-up is geconfigureerd.
3. Klik in het menu van het kluis dashboard op **back-uptaken** . alle back-uptaken worden weer gegeven.

    * Als er een back-uptaak wordt uitgevoerd, wacht u totdat deze klaar is of annuleert u de back-uptaak.
        * Als u de back-uptaak wilt annuleren, klikt u met de rechter muisknop op de back-uptaak en klikt u op **Annuleren** of [Power shell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0).
    * Als u de back-up opnieuw hebt geconfigureerd in een andere kluis, controleert u of er geen back-uptaken worden uitgevoerd in de oude kluis. Als deze bestaat, annuleert u de back-uptaak.
        * Klik met de rechter muisknop op de back-uptaak en klik op **Annuleren** of [Power shell](https://docs.microsoft.com/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0) gebruiken om de back-uptaak te annuleren.
4. Voer de back-upbewerking opnieuw uit.

Als de geplande back-upbewerking meer tijd in beslag neemt met de volgende back-upconfiguratie, raadpleegt u de [Aanbevolen procedures](backup-azure-vms-introduction.md#best-practices), [back-upprestaties](backup-azure-vms-introduction.md#backup-performance) en Restore- [overwegingen](backup-azure-vms-introduction.md#backup-and-restore-considerations).


## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-vm-has-no-internet-access"></a>De virtuele machine heeft geen Internet toegang
De virtuele machine heeft geen toegang tot internet volgens de implementatie vereiste. Het kan ook zijn dat er beperkingen gelden waardoor toegang tot de Azure-infra structuur wordt voor komen.

Voor een juiste werking moet de back-upextensie verbinding hebben met open bare IP-adressen van Azure. De uitbrei ding verzendt opdrachten naar een Azure Storage-eind punt (HTTPs-URL) voor het beheren van de moment opnamen van de virtuele machine. Als de extensie geen toegang heeft tot het open bare Internet, mislukt het maken van de back-up uiteindelijk.

####  <a name="solution"></a>Oplossing
Zie [netwerk verbinding maken](backup-azure-arm-vms-prepare.md#establish-network-connectivity)om het netwerk probleem op te lossen.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent is geïnstalleerd op de VM, maar reageert niet (voor Windows-Vm's)

#### <a name="solution"></a>Oplossing
De VM-agent is mogelijk beschadigd of de service is gestopt. Het opnieuw installeren van de VM-agent helpt de meest recente versie op te halen. Het helpt ook om communicatie met de service opnieuw te starten.

1. Bepaal of de Windows Azure Guest Agent-service wordt uitgevoerd in de VM-Services (Services. msc). Probeer de service Windows Azure Guest agent opnieuw te starten en de back-up te initiëren.    
2. Als de Windows Azure Guest Agent-service niet wordt weer gegeven in Services, gaat u in het configuratie scherm naar **Program ma's en onderdelen** om te bepalen of de service Windows Azure Guest agent is geïnstalleerd.
4. Als de Windows Azure Guest-agent wordt weer gegeven in **Program ma's en onderdelen**, verwijdert u de Windows Azure-gast agent.
5. Down load en installeer de [nieuwste versie van de agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet over beheerders rechten beschikken om de installatie te volt ooien.
6. Controleer of de services van de Windows Azure Guest agent in Services worden weer gegeven.
7. Een back-up op aanvraag uitvoeren:
    * Selecteer in de portal **Nu back-up maken**.

Controleer ook of [Microsoft .NET 4,5 is geïnstalleerd](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) in de VM. .NET 4,5 is vereist voor de VM-agent om te communiceren met de service.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent die is geïnstalleerd in de virtuele machine is verouderd (voor Linux-Vm's)

#### <a name="solution"></a>Oplossing
De meeste fout-en extensie-gerelateerde storingen voor Linux-Vm's worden veroorzaakt door problemen die van invloed zijn op een verouderde VM-agent. Volg deze algemene richt lijnen om dit probleem op te lossen:

1. Volg de instructies voor [het bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md).

   > [!NOTE]
   > We *raden u ten zeerste* aan de agent alleen bij te werken via een distributie opslagplaats. Het is niet raadzaam de agent code rechtstreeks vanuit GitHub te downloaden en bij te werken. Als de meest recente agent voor uw distributie niet beschikbaar is, neemt u contact op met de distributie ondersteuning voor instructies over het installeren ervan. Als u de meest recente agent wilt controleren, gaat u naar de pagina van de [Windows Azure Linux-agent](https://github.com/Azure/WALinuxAgent/releases) in de GitHub-opslag plaats.

2. Zorg ervoor dat de Azure-agent op de virtuele machine wordt uitgevoerd door de volgende opdracht uit te voeren:`ps -e`

   Als het proces niet wordt uitgevoerd, start u het opnieuw met behulp van de volgende opdrachten:

   * Voor Ubuntu:`service walinuxagent start`
   * Voor andere distributies:`service waagent start`

3. [Configureer de agent voor automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Voer een nieuwe test back-up uit. Als de fout zich blijft voordoen, verzamelt u de volgende logboeken van de VM:

   * /var/lib/waagent/*. XML
   * /var/log/waagent.log
   * /var/log/azure/*

Als u uitgebreide logboek registratie voor waagent nodig hebt, voert u de volgende stappen uit:

1. Zoek de volgende regel in het bestand/etc/waagent.conf: **Uitgebreide logboek registratie inschakelen (y | n)**
2. Wijzig de **Logboeken. uitgebreide** waarde van *n* tot en met *y*.
3. Sla de wijziging op en start waagent opnieuw door de stappen uit te voeren die eerder in deze sectie zijn beschreven.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>De status van de moment opname kan niet worden opgehaald, of een moment opname kan niet worden gemaakt
De back-up van de virtuele machine is afhankelijk van het uitgeven van een momentopname opdracht aan het onderliggende opslag account. Het maken van een back-up kan mislukken omdat deze geen toegang heeft tot het opslag account, of omdat de uitvoering van de momentopname taak is vertraagd.

#### <a name="solution"></a>Oplossing
De volgende voor waarden kunnen ertoe leiden dat de momentopname taak mislukt:

| Oorzaak | Oplossing |
| --- | --- |
| De status van de virtuele machine wordt onjuist gerapporteerd, omdat de VM wordt afgesloten in Remote Desktop Protocol (RDP). | Als u de virtuele machine in RDP afsluit, controleert u de portal om te bepalen of de status van de virtuele machine juist is. Als dat niet het geval is, sluit u de virtuele machine in de portal af met de optie **Afsluiten** in het VM-dash board. |
| De VM kan de host of het infrastructuur adres niet ophalen van DHCP. | DHCP moet zijn ingeschakeld in de gast voor het werken met de IaaS VM-back-up. Als de VM de host of het infrastructuur adres niet kan ophalen van DHCP-antwoord 245, kan er geen uitbrei dingen worden gedownload of uitgevoerd. Als u een statisch privé IP-adres nodig hebt, moet u dit configureren via de **Azure Portal** of **Power shell** en ervoor zorgen dat de DHCP-optie in de virtuele machine is ingeschakeld. Meer [informatie](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) over het instellen van een statisch IP-adres met Power shell.

### <a name="the-backup-extension-fails-to-update-or-load"></a>De back-upextensie kan niet worden bijgewerkt of geladen
Als uitbrei dingen niet kunnen worden geladen, mislukt de back-up omdat er geen moment opname kan worden gemaakt.

#### <a name="solution"></a>Oplossing

Verwijder de extensie om ervoor te zorgen dat de VMSnapshot-extensie opnieuw wordt geladen. Bij de volgende back-uppoging wordt de uitbrei ding opnieuw geladen.

De uitbrei ding verwijderen:

1. Ga in het [Azure Portal](https://portal.azure.com/)naar de virtuele machine waarop een back-up is mislukt.
2. Selecteer **instellingen**.
3. Selecteer **Extensies**.
4. Selecteer **Vmsnapshot-extensie**.
5. Selecteer **verwijderen**.

Als de VMSnapshot-extensie niet wordt weer gegeven in de Azure Portal, werkt u voor de Linux-VM naar de [Azure Linux-agent](../virtual-machines/linux/update-agent.md)en voert u de back-up uit.

Bij het uitvoeren van deze stappen wordt de extensie opnieuw geïnstalleerd tijdens de volgende back-up.

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Vergren deling van de resource groep herstel punt verwijderen
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar de **optie alle resources**en selecteer de resource groep voor de verzameling van herstel punten in`<Geo>`de`<number>`volgende indeling AzureBackupRG_ _.
3. Selecteer in de sectie **instellingen** de optie vergren delen om de vergren delingen weer te geven.
4. Als u de vergren deling wilt verwijderen, selecteert u het weglatings teken en klikt u op **verwijderen**.

    ![Vergren deling verwijderen](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a>Herstel punt verzameling opruimen
Na het verwijderen van de vergren deling, moeten de herstel punten worden opgeruimd. Volg een van de volgende methoden om de herstel punten op te schonen:<br>
* [Herstel punt verzameling opschonen door ad hoc-back-up uit te voeren](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Herstel punt verzameling opschonen van Azure Portal](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Herstel punt verzameling opschonen door ad hoc-back-up uit te voeren
Nadat u de vergren deling hebt verwijderd, activeert u een ad hoc/hand matige back-up Dit zorgt ervoor dat de herstel punten automatisch worden opgeruimd. U verwacht dat deze ad hoc/hand matige bewerking de eerste keer mislukt. het zorgt er echter voor dat automatisch opschonen in plaats van het hand matig verwijderen van herstel punten. Na het opschonen van de volgende geplande back-up is geslaagd.

> [!NOTE]
> Automatische opschoning gebeurt na enkele uren na het activeren van de ad-hoc/hand matige back-up. Als uw geplande back-up nog steeds mislukt, probeert u de verzameling met herstel punten hand matig te verwijderen met behulp van de stappen die [hier](#clean-up-restore-point-collection-from-azure-portal)worden beschreven.

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Herstel punt verzameling opschonen van Azure Portal <br>

Voer de volgende stappen uit om de verzameling herstel punten hand matig te wissen die niet is gewist vanwege de vergren deling van de resource groep:
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu **hub** op **alle resources**, selecteer de resource groep met de volgende indeling AzureBackupRG_`<Geo>`_`<number>` waar de virtuele machine zich bevindt.

    ![Vergren deling verwijderen](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Klik op resource groep, de Blade **overzicht** wordt weer gegeven.
4. Selecteer de optie **verborgen typen weer geven** om alle verborgen resources weer te geven. Selecteer de herstel punt verzamelingen met de volgende indeling AzureBackupRG_`<VMName>`_`<number>`.

    ![Vergren deling verwijderen](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Klik op **verwijderen**om de verzameling met herstel punten op te schonen.
6. Voer de back-upbewerking opnieuw uit.

> [!NOTE]
 >Als de resource (RP-verzameling) een groot aantal herstel punten heeft, kan het verwijderen van de portal op dezelfde time-out worden uitgevoerd. Dit is een bekend CRP-probleem, waarbij alle herstel punten niet worden verwijderd binnen de vastgestelde tijd en er een time-out optreedt voor de bewerking. de verwijderings bewerking slaagt echter normaal gesp roken na 2 of 3 nieuwe pogingen.
