---
title: Modern Backup Storage gebruiken met Azure Backup Server
description: Meer informatie over de nieuwe functies in Azure Backup Server. In dit artikel wordt beschreven hoe u de installatie van de back-upserver bijwerkt.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: adigan
ms.openlocfilehash: 1a265579593c8eb39f0df4037f8ec39c788ddbce
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465126"
---
# <a name="add-storage-to-azure-backup-server"></a>Opslag toevoegen aan Azure Backup Server

Azure Backup Server v2 en hoger ondersteunt Modern Backup Storage die opslag besparingen van 50 procent bieden, back-ups die drie keer sneller zijn, en efficiëntere opslag. Het biedt ook werk belasting bewuste opslag.

> [!NOTE]
> Als u Modern Backup Storage wilt gebruiken, moet u back-upserver v2 of v3 uitvoeren op Windows Server 2016 of v3 op Windows Server 2019.
> Als u back-upserver v2 uitvoert op een eerdere versie van Windows Server, kan Azure Backup Server niet profiteren van Modern Backup Storage. In plaats daarvan beveiligt de werk belasting zoals bij back-upserver v1. Zie voor meer informatie de matrix van de back-upserver versie [beveiliging](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server"></a>Volumes in back-upserver

Back-upserver v2 of hoger accepteert opslag volumes. Wanneer u een volume toevoegt, formatteert de back-upserver het volume naar een ReFS-bestands systeem dat Modern Backup Storage vereist. Om een volume toe te voegen en zo nodig uit te breiden, raden we u aan deze werk stroom te gebruiken:

1.  Stel de back-upserver in op een virtuele machine.
2.  Maak een volume op een virtuele schijf in een opslag groep:
    1.  Voeg een schijf toe aan een opslag groep en maak een virtuele schijf met een eenvoudige indeling.
    2.  Voeg extra schijven toe en breid de virtuele schijf uit.
    3.  Maak volumes op de virtuele schijf.
3.  Voeg de volumes toe aan de back-upserver.
4.  Configureer werk belasting bewuste opslag.

## <a name="create-a-volume-for-modern-backup-storage"></a>Een volume maken voor Modern Backup Storage

Als u back-upserver v2 of nieuwer gebruikt met volumes als schijf opslag, kunt u de controle over de opslag behouden. Een volume kan één schijf zijn. Als u de opslag in de toekomst echter wilt uitbreiden, maakt u een volume uit een schijf die is gemaakt met behulp van opslag ruimten. Dit kan handig zijn als u het volume wilt uitbreiden voor back-upopslag. In deze sectie vindt u aanbevolen procedures voor het maken van een volume met deze installatie.

1. Selecteer in Serverbeheer **Bestands-en opslag Services** > **volumes** > **opslag groepen**. Onder **fysieke schijven**selecteert u **nieuwe opslag groep**.

    ![Een nieuwe opslag groep maken](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. Selecteer in de vervolg keuzelijst **taken** de optie **nieuwe virtuele schijf**.

    ![Een virtuele schijf toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Selecteer de opslag groep en selecteer vervolgens **fysieke schijf toevoegen**.

    ![Een fysieke schijf toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Selecteer de fysieke schijf en selecteer vervolgens **virtuele schijf uitbreiden**.

    ![De virtuele schijf uitbreiden](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Selecteer de virtuele schijf en selecteer vervolgens **Nieuw volume**.

    ![Een nieuw volume maken](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. Selecteer de server en de nieuwe schijf in het dialoog venster **Server en schijf selecteren** . Selecteer vervolgens **Volgende**.

    ![De server en schijf selecteren](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a>Volumes toevoegen aan de back-upserver schijf opslag

Als u een volume wilt toevoegen aan de back-upserver, scant u in het deel venster **beheer** de opslag opnieuw en selecteert u vervolgens **toevoegen**. Er wordt een lijst weer gegeven met alle volumes die kunnen worden toegevoegd voor de opslag van back-upservers. Nadat de beschik bare volumes zijn toegevoegd aan de lijst met geselecteerde volumes, kunt u ze een beschrijvende naam geven om ze te beheren. Als u deze volumes wilt Format teren naar ReFS zodat de back-upserver de voor delen van Modern Backup Storage kan gebruiken, selecteert u **OK**.

![Beschik bare volumes toevoegen](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Werkbelasting bewuste opslag instellen

Met werkbelasting bewuste opslag kunt u de volumes selecteren waarmee bepaalde soorten workloads worden opgeslagen. U kunt bijvoorbeeld dure volumes instellen die een groot aantal i/o-bewerkingen per seconde (IOPS) ondersteunen om alleen de workloads op te slaan waarvoor veelvuldige back-ups van hoge volumes zijn vereist. Een voor beeld is SQL Server met transactie Logboeken. Voor andere werk belastingen waarvan een back-up minder vaak wordt uitgevoerd, zoals Vm's, kan een back-up worden gemaakt naar goedkope volumes.

### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

U kunt werkbelasting bewuste opslag instellen met behulp van de Power shell-cmdlet Update-DPMDiskStorage, waarmee de eigenschappen van een volume in de opslag groep op een Azure Backup Server worden bijgewerkt. 

Syntaxis

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
De volgende scherm afbeelding toont de cmdlet Update-DPMDiskStorage in het Power shell-venster.

![De opdracht update-DPMDiskStorage in het Power shell-venster](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

De wijzigingen die u aanbrengt met Power shell, worden weer gegeven in de Administrator-console van de back-upserver.

![Schijven en volumes in de Administrator-console](./media/backup-mabs-add-storage/mabs-add-storage-9.png)


## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Verouderde opslag migreren naar Modern Backup Storage
Nadat u een upgrade hebt uitgevoerd naar of de back-upserver v2 hebt geïnstalleerd en het besturings systeem hebt bijgewerkt naar Windows Server 2016, moet u de beveiligings groepen bijwerken om Modern Backup Storage te gebruiken. Beveiligings groepen worden standaard niet gewijzigd. Ze blijven functioneren zoals ze voor het eerst zijn ingesteld.

Het bijwerken van beveiligings groepen voor het gebruik van Modern Backup Storage is optioneel. Als u de beveiligings groep wilt bijwerken, stopt u de beveiliging van alle gegevens bronnen met behulp van de optie gegevens behouden. Voeg vervolgens de gegevens bronnen toe aan een nieuwe beveiligings groep.

1. Selecteer in de Administrator-console de functie **beveiliging** . Klik in de lijst **lid van beveiligings groep** met de rechter muisknop op het lid en selecteer vervolgens **beveiliging van lid stoppen**.

   ![Beveiliging van lid stoppen](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. Bekijk in het dialoog venster **verwijderen uit groep** de gebruikte schijf ruimte en de beschik bare vrije ruimte voor de opslag groep. De standaard instelling is om de herstel punten op de schijf te verlaten en te laten verlopen volgens het bijbehorende Bewaar beleid. Klik op **OK**.

   Als u de gebruikte schijf ruimte onmiddellijk wilt retour neren naar de groep met beschik bare opslag, schakelt u het selectie vakje **replica op schijf verwijderen** in om de back-upgegevens (en herstel punten) te verwijderen die aan dat lid zijn gekoppeld.

   ![Verwijderen uit het dialoog venster groep](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Maak een beveiligings groep die gebruikmaakt van Modern Backup Storage. Neem de niet-beveiligde gegevens bronnen op.

## <a name="add-disks-to-increase-legacy-storage"></a>Schijven toevoegen om verouderde opslag te verg Roten

Als u verouderde opslag met een back-upserver wilt gebruiken, moet u mogelijk schijven toevoegen om verouderde opslag te verg Roten.

Schijf opslag toevoegen:

1. Selecteer **beheer** > Disk Storagetoevoegen > in de Administrator-console.

    ![Dialoog venster Disk Storage toevoegen](https://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. Selecteer in het dialoog venster **Disk Storage toevoegen** de optie **schijven toevoegen**.

5. Selecteer de schijven die u wilt toevoegen in de lijst met beschik bare schijven, selecteer **toevoegen**en selecteer vervolgens **OK**.

## <a name="next-steps"></a>Volgende stappen
Nadat u de back-upserver installeert, leert u hoe u uw server voorbereidt of een werk belasting gaat beveiligen.

- [Werk belastingen voor de back-upserver voorbereiden](backup-azure-microsoft-azure-backup.md)
- [Back-upserver gebruiken om een back-up te maken van een VMware-Server](backup-azure-backup-server-vmware.md)
- [Back-upserver gebruiken om back-ups te maken van SQL Server](backup-azure-sql-mabs.md)
