---
title: Een uitbreidbare processerver instellen tijdens het herstel na noodgevallen van virtuele VMware-machines en fysieke servers met Azure Site Recovery | Microsoft-Docs
description: In dit artikel wordt beschreven hoe u uitbreidbare processerver instellen tijdens het herstel na noodgevallen van virtuele VMware-machines en fysieke servers.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/23/2019
ms.author: ramamill
ms.openlocfilehash: 1b6084b4e93f3dc17f633f1b8496f9c26e7f576f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64925478"
---
# <a name="scale-with-additional-process-servers"></a>Schaal aanpassen met extra processervers

Wanneer u repliceert virtuele VMware-machines of fysieke servers naar Azure met [siteherstel](site-recovery-overview.md), een processerver is geïnstalleerd op de configuratie van server-machine en wordt gebruikt voor de coördinatie van gegevensoverdracht tussen de Site Recovery en uw on-premises infrastructuur. U kunt aanvullende zelfstandige processervers toevoegen om te vergroten capaciteit en scale-out-implementatie van uw replicatie. In dit artikel wordt beschreven hoe u een uitbreidbare processerver instellen.

## <a name="before-you-start"></a>Voordat u begint

### <a name="capacity-planning"></a>Capaciteitsplanning

Zorg ervoor dat u hebt uitgevoerd [capaciteitsplanning](site-recovery-plan-capacity-vmware.md) voor VMware-replicatie. Dit helpt u om te bepalen hoe en wanneer u aanvullende processenservers te implementeren.

Van 9.24 versie richtlijnen toegevoegd tijdens de selectie van de processerver voor nieuwe replicaties. Processerver gemarkeerd in orde, waarschuwing en kritiek op basis van bepaalde criteria. Voor informatie over de verschillende scenario's die kunnen van invloed zijn op de status van de processerver, raadpleegt de [verwerken van server-waarschuwingen](vmware-physical-azure-monitor-process-server.md#process-server-alerts).

> [!NOTE]
> Gebruik van een gekloonde onderdeel van de processerver wordt niet ondersteund. Volg de stappen in dit artikel voor elke PS scale-out.

### <a name="sizing-requirements"></a>Vereisten voor volumegrootte 

Controleer of de vereisten voor volumegrootte samengevat in de tabel. In het algemeen als hebt tot de schaal van uw implementatie naar meer dan 200 bronmachines, of u een totaal dagelijks verloop snelheid van meer dan 2 TB hebt, moet u extra processervers om het verkeersvolume te verwerken.

| **Aanvullende processerver** | **Cachegrootte van de schijf** | **Veranderingssnelheid van gegevens** | **Beveiligde machines** |
| --- | --- | --- | --- |
|4 vcpu's (2-sockets * 2 kernen \@ 2,5 GHz), 8 GB geheugen |300 GB |250 GB of minder |85 of minder machines repliceren. |
|8 vcpu's (2-sockets * 4 kernen \@ 2,5 GHz), 12 GB geheugen |600 GB |250 GB tot 1 TB |85-150-machines repliceren. |
|12 vcpu's (2-sockets * 6 kernen \@ 2,5 GHz) 24 GB geheugen |1 TB |1 TB tot 2 TB |Repliceren tussen 150 225 machines. |

Waarbij elke beveiligde bron-VM is geconfigureerd met 3 schijven van 100 GB elk.

### <a name="prerequisites"></a>Vereisten

De vereisten voor de aanvullende processerver worden samengevat in de volgende tabel.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="download-installation-file"></a>Installatiebestand downloaden

Download het installatiebestand voor de processerver als volgt:

1. Meld u aan bij Azure portal en blader naar de Recovery Services-kluis.
2. Open **infrastructuur voor Site Recovery** > **VMWare en fysieke Machines** > **configuratieservers** (onder voor VMware en fysieke Machines).
3. Selecteer de configuratieserver Inzoomen op gegevens van de server. Klik vervolgens op **+ processerver**.
4. In **toevoegen processerver** >  **kiezen waar u uw processerver implementeren**, selecteer **implementeren een uitbreidbare processerver on-premises**.

   ![Pagina-Servers toevoegen](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Klik op **downloaden van de Microsoft Azure Site Recovery van geïntegreerde Setup**. Hiermee downloadt u de nieuwste versie van het bestand voor installatie.

   > [!WARNING]
   > Versie-installatie van de processerver moet hetzelfde zijn als of ouder is dan, de versie van de configuratie die u hebt uitgevoerd. Een eenvoudige manier om te controleren of de compatibiliteit van de versie is het gebruik van de dezelfde installatieprogramma, die u hebt onlangs gebruikt om te installeren of bijwerken van uw configuratieserver.

## <a name="install-from-the-ui"></a>Installeren vanuit de gebruikersinterface

Installeer als volgt. Na het instellen van de server, migreert u bronmachines om deze te gebruiken.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Installeren vanaf de opdrachtregel

Installeer de volgende opdracht uit:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Waar opdrachtregelparameters zijn als volgt:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Bijvoorbeeld:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Maak een bestand van proxy-instellingen

Als u nodig hebt voor het instellen van een proxy, wordt de parameter ProxySettingsFilePath een bestand gebruikt als invoer. U kunt het bestand als volgt maken en geven deze als invoerparameter ProxySettingsFilePath.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Serverinstellingen beheren verwerken](vmware-azure-manage-process-server.md)
