---
title: Achtergrond installatie van Azure Backup Server v2
description: Gebruik een Power shell-script om Azure Backup Server v2 op de achtergrond te installeren. Dit type installatie wordt ook wel een installatie zonder toezicht genoemd.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: raynew
ms.openlocfilehash: add1f4057b5b52310f53553dcd23e3357fb1ee29
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465014"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Een installatie zonder toezicht van Azure Backup Server uitvoeren

Meer informatie over het uitvoeren van een installatie zonder toezicht van Azure Backup Server.

Deze stappen zijn niet van toepassing als u Azure Backup Server v1 installeert.

## <a name="install-backup-server"></a>Back-upserver installeren

1. Maak een tekst bestand op de server die als host fungeert voor Azure Backup Server v2 of hoger. (U kunt het bestand in Klad blok of in een andere tekst editor maken.) Sla het bestand op als MABSSetup. ini.

2. Plak de volgende code in het bestand MABSSetup. ini. Vervang de tekst tussen de haakjes (\< \>) door waarden uit uw omgeving. De volgende tekst is een voor beeld:

   ```
   [OPTIONS]
   UserName=administrator
   CompanyName=<Microsoft Corporation>
   SQLMachineName=localhost
   SQLInstanceName=<SQL instance name>
   SQLMachineUserName=administrator
   SQLMachinePassword=<admin password>
   SQLMachineDomainName=<machine domain>
   ReportingMachineName=localhost
   ReportingInstanceName=<reporting instance name>
   SqlAccountPassword=<admin password>
   ReportingMachineUserName=<username>
   ReportingMachinePassword=<reporting admin password>
   ReportingMachineDomainName=<domain>
   VaultCredentialFilePath=<vault credential full path and complete name>
   SecurityPassphrase=<passphrase>
   PassphraseSaveLocation=<passphrase save location>
   UseExistingSQL=<1/0 use or do not use existing SQL>
   ```

3. Sla het bestand op. Voer vervolgens de volgende opdracht uit vanaf een opdracht prompt met verhoogde bevoegdheid op de installatie server:

   ```
   start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
   ```

U kunt deze vlaggen gebruiken voor de installatie:</br>
**/f**:. ini-bestandspad</br>
**/l**: Logboekpad</br>
**/i**: Installatiepad</br>
**/x**: Uninstall-pad</br>

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-upserver installeert, leert u hoe u uw server voorbereidt of een werk belasting gaat beveiligen.

- [Werk belastingen voor de back-upserver voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Back-upserver gebruiken om een back-up te maken van een VMware-Server](backup-azure-backup-server-vmware.md)
- [Back-upserver gebruiken om back-ups te maken van SQL Server](backup-azure-sql-mabs.md)
- [Modern Backup Storage toevoegen aan de back-upserver](backup-mabs-add-storage.md)
