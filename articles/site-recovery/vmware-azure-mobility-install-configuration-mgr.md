---
title: Installatie van de Azure Site Recovery Mobility Service voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van System Center Configuration Manager automatiseren | Microsoft Docs
description: Dit artikel helpt u de installatie van de Mobility-Service met System Center Configuration Manager voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Site Recovery automatiseren.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: ramamill
ms.openlocfilehash: 35c317c4b73e9a22e3b0d6192abcfc2a596066b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60598048"
---
# <a name="automate-mobility-service-installation-with-system-center-configuration-manager"></a>Installatie van de Mobility-Service met System Center Configuration Manager automatiseren

De Mobility-service is geïnstalleerd op virtuele VMware-machines en fysieke servers die u repliceren wilt naar Azure met [Azure Site Recovery](site-recovery-overview.md)

Dit artikel bevat een voorbeeld van hoe u System Center Configuration Manager gebruiken kunt om de Azure Site Recovery Mobility Service op een VMware-VM implementeren. Met behulp van een hulpprogramma voor software-implementatie, zoals de Configuration Manager heeft de volgende voordelen:

* Nieuwe installaties en upgrades tijdens het geplande onderhoudsvenster voor software-updates plannen
* Implementatie naar honderden servers gelijktijdig schalen

In dit artikel maakt gebruik van System Center Configuration Manager 2012 R2 ter illustratie van de implementatie-activiteit. Er wordt ervan uitgegaan dat u gebruikmaakt van versie **9.9.4510.1** of hoger van de Mobility-service.

U kunt ook installatie van de Mobility-Service met automatiseren [Azure Automation DSC](vmware-azure-mobility-deploy-automation-dsc.md).

## <a name="prerequisites"></a>Vereisten

1. Een software-implementatie hulpprogramma, zoals Configuration Manager, dat al geïmplementeerd in uw omgeving.
2. Maak twee [apparaatverzamelingen](https://technet.microsoft.com/library/gg682169.aspx), één voor alle **Windows servers**, en een andere voor alle **Linux-servers**, dat u beveiligen wilt met behulp van Site Recovery.
3. Een configuratieserver die al is geregistreerd in de Recovery Services-kluis.
4. Een beveiligd netwerkbestandsshare (SMB-share) die kan worden geopend door de configuration manager-machine.

## <a name="deploy-on-windows-machines"></a>Implementeren op Windows-machines
> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat het IP-adres van de configuratieserver 192.168.3.121 is en of de beveiligde netwerkbestandsshare \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="prepare-for-deployment"></a>Implementatie voorbereiden
1. Maak een map op de netwerkshare en noem het **MobSvcWindows**.
2. Meld u aan bij de configuratieserver en open een beheeropdrachtprompt.
3. Voer de volgende opdrachten om een wachtwoordzin-bestand te genereren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieer de **MobSvc.passphrase** bestand naar de **MobSvcWindows** map op de netwerkshare.
5. Blader naar de opslagplaats van het installatieprogramma op de configuratieserver door het uitvoeren van de volgende opdracht uit:

   `cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

6. Kopieer de **Microsoft ASR\_UA\_*versie*\_Windows\_GA\_*datum*\_Release.exe**  naar de **MobSvcWindows** map op de netwerkshare.
7. Kopieer de volgende code en sla het bestand als **install.bat** in de **MobSvcWindows** map.

   > [!NOTE]
   > Vervang tijdelijke aanduidingen [CSIP] in dit script met de werkelijke waarden van het IP-adres van uw configuratieserver.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```

### <a name="create-a-package"></a>Een pakket maken

1. Aanmelden bij uw Configuration Manager-console.
2. Blader naar **softwarebibliotheek** > **Toepassingsbeheer** > **pakketten**.
3. Met de rechtermuisknop op **pakketten**, en selecteer **pakket maken**.
4. Geef waarden op voor de naam, beschrijving, de fabrikant, taal en versie.
5. Selecteer de **dit pakket bevat bronbestanden** selectievakje.
6. Klik op **Bladeren**, en selecteer de netwerkshare waar het installatieprogramma wordt opgeslagen (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcWindows).

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package.png)

7. Op de **Kies het programmatype dat u wilt maken** weergeeft, schakelt **standaardprogramma**, en klikt u op **volgende**.

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. Op de **informatie opgeven over dit standaardprogramma** pagina, informatie het volgende invoeren en op **volgende**. (U kunnen de standaardwaarden gebruiken voor de andere invoer.)

   | **Parameternaam** | **Waarde** |
   |--|--|
   | Name | Microsoft Azure Mobility-Service (Windows) installeren |
   | Opdrachtregel | Install.bat |
   | Programma kan worden uitgevoerd | Of een gebruiker is aangemeld |

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties.png)

9. Selecteer op de volgende pagina de doel-besturingssystemen. Mobility-Service kan worden geïnstalleerd op Windows Server 2012 R2, Windows Server 2012 en Windows Server 2008 R2.

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-page2.png)

10. Als u wilt de wizard hebt voltooid, klikt u op **volgende** twee keer.


> [!NOTE]
> Het script biedt ondersteuning voor zowel nieuwe installaties van agents van de Mobility-Service en updates voor agents die al zijn geïnstalleerd.

### <a name="deploy-the-package"></a>Het pakket implementeren
1. Met de rechtermuisknop op het pakket in de Configuration Manager-console en selecteer **inhoud distribueren**.
   ![Schermafbeelding van de Configuration Manager-console](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Selecteer de **[distributiepunten](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** u aan bij de pakketten moeten worden gekopieerd.
3. Voltooi de wizard. Het pakket vervolgens wordt gestart die naar de opgegeven distributiepunten worden gerepliceerd.
4. Nadat de pakketdistributie is voltooid, met de rechtermuisknop op het pakket en selecteert u **implementeren**.
   ![Schermafbeelding van de Configuration Manager-console](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Selecteer de Windows Server-apparaatverzameling die u hebt gemaakt in de sectie vereisten als de doelverzameling voor de implementatie.

   ![Schermafbeelding van de wizard Software implementeren](./media/vmware-azure-mobility-install-configuration-mgr/sccm-select-target-collection.png)

6. Op de **opgeven van het doel van inhoud** weergeeft, schakelt de **distributiepunten**.
7. Op de **instellingen opgeven om te bepalen hoe deze software wordt geïmplementeerd** pagina, zorg ervoor dat het doel is **vereist**.

   ![Schermafbeelding van de wizard Software implementeren](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. Op de **Geef het schema voor deze implementatie** pagina, een planning opgeven. Zie voor meer informatie, [planning pakketten](https://technet.microsoft.com/library/gg682178.aspx).
9. Op de **distributiepunten** pagina, configureer de eigenschappen op basis van de behoeften van uw datacenter. Voltooi de wizard.

> [!TIP]
> Plannen om te voorkomen onnodig opnieuw moet opstarten, de installatie van het pakket tijdens uw maandelijkse onderhoudsvenster of het venster voor software-updates.

U kunt de voortgang van de implementatie bewaken met behulp van de Configuration Manager-console. Ga naar **bewaking** > **implementaties** >  *[pakketnaam van uw]* .

  ![Schermafbeelding van Configuration Manager-optie voor het bewaken van implementaties](./media/vmware-azure-mobility-install-configuration-mgr/report.PNG)

## <a name="deploy-on-linux-machines"></a>Implementeren op Linux-machines
> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat het IP-adres van de configuratieserver 192.168.3.121 is en of de beveiligde netwerkbestandsshare \\\ContosoSecureFS\MobilityServiceInstallers.

### <a name="prepare-for-deployment"></a>Implementatie voorbereiden
1. Maak een map op de netwerkshare en geef deze als de naam **MobSvcLinux**.
2. Meld u aan bij de configuratieserver en open een beheeropdrachtprompt.
3. Voer de volgende opdrachten om een wachtwoordzin-bestand te genereren:

    `cd %ProgramData%\ASR\home\svsystems\bin`

    `genpassphrase.exe -v > MobSvc.passphrase`
4. Kopieer de **MobSvc.passphrase** bestand naar de **MobSvcLinux** map op de netwerkshare.
5. Blader naar de opslagplaats van het installatieprogramma op de configuratieserver door het uitvoeren van de opdracht:

   `cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

6. Kopieer de volgende bestanden naar de **MobSvcLinux** map op uw netwerkshare:
   * Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz
   * Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz
   * Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz
   * Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz
   * Microsoft-ASR\_UA\*OL6-64\*release.tar.gz
   * Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz


7. Kopieer de volgende code en sla het bestand als **install_linux.sh** in de **MobSvcLinux** map.
   > [!NOTE]
   > Vervang tijdelijke aanduidingen [CSIP] in dit script met de werkelijke waarden van het IP-adres van uw configuratieserver.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```

### <a name="create-a-package"></a>Een pakket maken

1. Aanmelden bij uw Configuration Manager-console.
2. Blader naar **softwarebibliotheek** > **Toepassingsbeheer** > **pakketten**.
3. Met de rechtermuisknop op **pakketten**, en selecteer **pakket maken**.
4. Geef waarden op voor de naam, beschrijving, de fabrikant, taal en versie.
5. Selecteer de **dit pakket bevat bronbestanden** selectievakje.
6. Klik op **Bladeren**, en selecteer de netwerkshare waar het installatieprogramma wordt opgeslagen (\\\ContosoSecureFS\MobilityServiceInstaller\MobSvcLinux).

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package-linux.png)

7. Op de **Kies het programmatype dat u wilt maken** weergeeft, schakelt **standaardprogramma**, en klikt u op **volgende**.

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. Op de **informatie opgeven over dit standaardprogramma** pagina, informatie het volgende invoeren en op **volgende**. (U kunnen de standaardwaarden gebruiken voor de andere invoer.)

    | **Parameternaam** | **Waarde** |
   |--|--|
   | Name | Mobility-Service voor de Microsoft Azure (Linux) installeren |
   | Opdrachtregel | ./install_linux.sh |
   | Programma kan worden uitgevoerd | Of een gebruiker is aangemeld |

   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-linux.png)

9. Selecteer op de volgende pagina **dit programma kan worden uitgevoerd op elk platform**.
   ![Schermafbeelding van pakket en programma-wizard](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties-page2-linux.png)

10. Als u wilt de wizard hebt voltooid, klikt u op **volgende** twee keer.

> [!NOTE]
> Het script biedt ondersteuning voor zowel nieuwe installaties van agents van de Mobility-Service en updates voor agents die al zijn geïnstalleerd.

### <a name="deploy-the-package"></a>Het pakket implementeren
1. Met de rechtermuisknop op het pakket in de Configuration Manager-console en selecteer **inhoud distribueren**.
   ![Schermafbeelding van de Configuration Manager-console](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Selecteer de **[distributiepunten](https://technet.microsoft.com/library/gg712321.aspx#BKMK_PlanForDistributionPoints)** u aan bij de pakketten moeten worden gekopieerd.
3. Voltooi de wizard. Het pakket vervolgens wordt gestart die naar de opgegeven distributiepunten worden gerepliceerd.
4. Nadat de pakketdistributie is voltooid, met de rechtermuisknop op het pakket en selecteert u **implementeren**.
   ![Schermafbeelding van de Configuration Manager-console](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Selecteer de Linux-Server verzameling apparaten die u hebt gemaakt in de sectie vereisten als de doelverzameling voor de implementatie.

   ![Schermafbeelding van de wizard Software implementeren](./media/vmware-azure-mobility-install-configuration-mgr/sccm-select-target-collection-linux.png)

6. Op de **opgeven van het doel van inhoud** weergeeft, schakelt de **distributiepunten**.
7. Op de **instellingen opgeven om te bepalen hoe deze software wordt geïmplementeerd** pagina, zorg ervoor dat het doel is **vereist**.

   ![Schermafbeelding van de wizard Software implementeren](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. Op de **Geef het schema voor deze implementatie** pagina, een planning opgeven. Zie voor meer informatie, [planning pakketten](https://technet.microsoft.com/library/gg682178.aspx).
9. Op de **distributiepunten** pagina, configureer de eigenschappen op basis van de behoeften van uw datacenter. Voltooi de wizard.

Mobility-Service wordt op de Apparaatverzameling voor Linux-Server geïnstalleerd volgens de planning die u hebt geconfigureerd.


## <a name="uninstall-the-mobility-service"></a>Verwijder de Mobility-service
U kunt Configuration Manager-pakketten voor het verwijderen van de Mobility-Service maken. Het volgende script gebruiken om dit te doen:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Volgende stappen
U bent nu klaar om [beveiliging inschakelen](vmware-azure-enable-replication.md) voor uw virtuele machines.
