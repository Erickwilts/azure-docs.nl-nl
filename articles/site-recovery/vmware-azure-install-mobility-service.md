---
title: Voorbereiden van de bronmachines voor het installeren van de Mobility-Service via push-installatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure | Microsoft Docs
description: Informatie over het voorbereiden van uw server voor het installeren van de Mobility-agent via push-installatie voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van de Azure Site Recovery-service.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 628be573d03d42ec62a358071074facfe228852d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318166"
---
# <a name="prepare-source-machine-for-push-installation-of-mobility-agent"></a>Voorbereiden van de bron-VM voor push-installatie van de mobility-agent

Wanneer u herstel na noodgevallen voor VMware-VM's en fysieke servers met behulp van instellen [Azure Site Recovery](site-recovery-overview.md), u installeert de [Site Recovery Mobility service](vmware-physical-mobility-service-overview.md) op elke on-premises virtuele VMware-machine en de fysieke server.  De Mobility-service worden vastgelegd schrijven van gegevens op de machine, en ze doorstuurt naar de processerver van Site Recovery.

## <a name="install-on-windows-machine"></a>Installeren op Windows-machine

Op elke Windows-machine die u wilt beveiligen, doet u het volgende:

1. Zorg ervoor dat er een netwerkverbinding tussen de computer en de processerver is. Als u geen afzonderlijke processerver hebt ingesteld, wordt wordt standaard het uitgevoerd op de configuratieserver.
1. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet beheerdersrechten, lokaal of domein hebben. Gebruik dit account alleen voor de push-installatie en agentupdates.
2. Als u niet een domeinaccount, schakelt u toegangsbeheer voor externe gebruikers op de lokale computer als volgt uit:
    - Voeg een nieuwe DWORD onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System registersleutel: **LocalAccountTokenFilterPolicy**. Stel de waarde voor **1**.
    -  Om dit te doen bij een opdrachtprompt, voer de volgende opdracht:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Selecteer in Windows Firewall op de computer die u wilt beveiligen, **toestaan een app of functie via Firewall**. Schakel **bestands- en printerdeling** en **Windows Management Instrumentation (WMI)** . Voor computers die deel uitmaken van een domein, kunt u de firewall-instellingen configureren met behulp van een groepsbeleidsobject (GPO).

   ![Firewallinstellingen](./media/vmware-azure-install-mobility-service/mobility1.png)

4. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool. Om dit te doen, moet u zich aanmelden bij uw configuratieserver.
5. Open **cspsconfigtool.exe**. Het is beschikbaar als snelkoppeling op het bureaublad en in de map %ProgramData%\home\svsystems\bin.
6. Op de **Accounts beheren** tabblad **-Account toevoegen**.
7. Voeg het account toe dat u hebt gemaakt.
8. Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.

## <a name="install-on-linux-machine"></a>Installeren op Linux-machine

Op elke Linux-machine die u wilt beveiligen, het volgende doen:

1. Zorg ervoor dat er een netwerkverbinding tussen de Linux-machine en de processerver is.
2. Maak een account dat op de processerver kan worden gebruikt voor toegang tot de computer. Het account moet een **rootgebruiker** zijn op de Linux-bronserver. Gebruik dit account alleen voor de push-installatie en voor updates.
3. Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
4. Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
5. Zorg ervoor dat SSH (Secure Shell) is ingeschakeld en wordt uitgevoerd op poort 22.
4. SFTP-subsysteem en wachtwoordverificatie verificatie in het bestand sshd_config inschakelen. Om dit te doen, moet u zich aanmelden als **hoofdmap**.
5. In de **/etc/ssh/sshd_config** bestand, zoek de regel die met begint **PasswordAuthentication**.
6. Verwijder opmerkingen bij de regel en wijzig de waarde in **Ja**.
7. Zoek de regel die met begint **subsysteem**, en verwijder opmerkingen bij de regel.

      ![Linux](./media/vmware-azure-install-mobility-service/mobility2.png)

8. Start de service **sshd** opnieuw.
9. Voeg het account toe dat u hebt gemaakt in CSPSConfigtool. Om dit te doen, moet u zich aanmelden bij uw configuratieserver.
10. Open **cspsconfigtool.exe**. Het is beschikbaar als snelkoppeling op het bureaublad en in de map %ProgramData%\home\svsystems\bin.
11. Op de **Accounts beheren** tabblad **-Account toevoegen**.
12. Voeg het account toe dat u hebt gemaakt.
13. Voer de referenties in die u gebruikt wanneer u replicatie voor een computer inschakelt.

## <a name="anti-virus-on-replicated-machines"></a>Antivirusprogramma's op gerepliceerde machines

Als computers die u wilt repliceren actieve antivirusprogramma's software die wordt uitgevoerd, controleert u of u de installatiemap van de Mobility-service uitsluiten van antivirusprogramma's bewerkingen (*C:\ProgramData\ASR\agent*). Dit zorgt ervoor dat de replicatie werkt zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

Nadat de Mobility-Service is geïnstalleerd, in de Azure-portal, selecteert u **+ repliceren** om te beginnen met het beveiligen van deze VM's. Meer informatie over het inschakelen van replicatie voor [VMware VMs(vmware-azure-enable-replication.md) en [fysieke servers](physical-azure-disaster-recovery.md#enable-replication).


