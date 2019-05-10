---
title: Een Recovery Services-kluis in Azure verwijderen
description: Beschrijft hoe u een Recovery Services-kluis verwijderen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: raynew
ms.openlocfilehash: e7cea725a25d48ac9f1ffad6acc434e21145890e
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473247"
---
# <a name="delete-a-recovery-services-vault"></a>Een Recovery Services-kluis verwijderen

Dit artikel wordt beschreven hoe u verwijdert een [Azure Backup](backup-overview.md) Recovery Services-kluis. Deze bevat instructies voor afhankelijkheden verwijderen en vervolgens een kluis verwijderen en verwijderen van een kluis geforceerd beëindigd.


## <a name="before-you-start"></a>Voordat u begint

Voordat u begint, is het belangrijk om te begrijpen dat u een Recovery Services-kluis met de servers niet verwijderen die is geregistreerd in het of die back-upgegevens bevat.

- Als u wilt verwijderen zonder problemen een kluis, unregister-servers in het, gegevens van de kluis verwijderen en verwijder vervolgens de kluis.
- Als u probeert te verwijderen van een kluis die nog steeds afhankelijkheden heeft, wordt er een foutbericht weergegeven. en moet u handmatig verwijderen van de kluis afhankelijkheden, met inbegrip van:
    - Back-ups van items
    - Beveiligde servers
    - Back-up van beheerservers (Azure Backup Server, DPM) ![selecteert u de kluis om het dashboard te openen](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Als u niet wilt behouden van gegevens in de Recovery Services-kluis en de kluis wilt verwijderen, kunt u de kluis geforceerd beëindigd verwijderen.
- Als u probeert te verwijderen van een kluis, maar niet, is nog steeds de kluis geconfigureerd voor het ontvangen van back-upgegevens.


## <a name="delete-a-vault-from-the-azure-portal"></a>Een kluis verwijderen uit de Azure portal

1. Open het dashboard van de kluis.  
2. Klik in het dashboard op **verwijderen**. Controleer of dat u wilt verwijderen.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Als u een foutbericht krijgt, verwijdert u [back-up items](#remove-backup-items), [infrastructuurservers](#remove-backup-infrastructure-servers), en [herstelpunten](#remove-azure-backup-agent-recovery-points), en verwijder vervolgens de kluis.

![Fout bij de kluis verwijderen](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>De Recovery Services-kluis met behulp van Azure Resource Manager-client verwijderen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installeer chocolatey van [hier](https://chocolatey.org/) en ARMClient uitvoeren te installeren de onderstaande opdracht:

   ` choco install armclient --source=https://chocolatey.org/api/v2/ `
2. Meld u aan bij Azure-account met de onderstaande opdracht

    ` ARMClient.exe login [environment name] `

3. Verzamel de abonnement-ID en resource groepsnaam voor de kluis die u wilt verwijderen in de Azure-portal.

Raadpleeg voor meer informatie over ARMClient opdracht dit [document](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Azure Resource Manager-client gebruiken om te verwijderen van de Recovery Services-kluis

1. Voer de volgende opdracht uit met behulp van uw abonnements-ID, de Resourcegroepnaam en de naam van de kluis. W\hen dat u de opdracht dat de kluis wordt verwijderd als u geen eventuele afhankelijkheden uitvoeren.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Als de kluis niet leeg zijn, ontvangt u de fout 'Kluis kan niet worden verwijderd omdat er bestaande resources binnen deze kluis'. Een beveiligde items verwijderen / container in een kluis, het volgende doen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Controleer of de kluis is verwijderd in de Azure-portal.


## <a name="remove-vault-items-and-delete-the-vault"></a>Items van de kluis verwijderen en de kluis verwijderen

Deze procedure vindt u enkele voorbeelden voor het verwijderen van back-upgegevens en infrastructuurservers. Nadat u alles uit een kluis verwijderd, kunt u deze kunt verwijderen.

### <a name="remove-backup-items"></a>Back-up items verwijderen

Hier vindt u een voorbeeld waarin wordt uitgelegd hoe u back-upgegevens verwijderen uit Azure Files.

1. Klik op **back-up Items** > **Azure Storage (Azure-bestanden)**

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Met de rechtermuisknop op elk item Azure Files verwijderen en klik op **back-up stoppen**.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/stop-backup-item.png)


3. In **back-up stoppen** > **Kies een optie**, selecteer **back-upgegevens verwijderen**.
4. Typ de naam van het item en klikt u op **back-up stoppen**.
   - Hiermee wordt gecontroleerd dat u wilt verwijderen van het item.
   - De **back-up stoppen** knop wordt geactiveerd nadat u hebt gecontroleerd.
   - Als u behouden en de gegevens niet verwijderen, kunt u zich niet aan de kluis verwijderen.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.
6. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
7. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
8. Na het verwijderen van een item in de lijst op de **back-Upitems** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.

      ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>Een back-upinfrastructuur servers verwijderen

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om servers weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Met de rechtermuisknop op het item > **verwijderen**.
4. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
5. Nadat de taak is voltooid, de service verzendt een bericht: **het back-upproces is gestopt en de back-upgegevens is verwijderd**.
6. Na het verwijderen van een item in de lijst op de **back-upinfrastructuur** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.


### <a name="remove-azure-backup-agent-recovery-points"></a>Herstelpunten voor Azure backup-agent verwijderen

1. Klik in het menu kluis dashboard **back-upinfrastructuur**.
2. Klik op **back-up-beheerservers** om de infrastructuurservers weer te geven.

    ![Selecteer uw kluis om het dashboard te openen](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. In de **beschermde Servers** lijst, klikt u op Azure Backup-Agent.

    ![Selecteer de back-uptype](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Klik op de server in de lijst met servers die worden beveiligd met behulp van Azure backup-agent.

    ![Selecteer de beveiligde server](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Klik op het dashboard van de geselecteerde server, **verwijderen**.

    ![de geselecteerde server verwijderen](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. Op de **verwijderen** menu, typ de naam van het item en klikt u op **verwijderen**.

     ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. (Optioneel) Geef een reden op waarom u de gegevens wilt verwijderen, en voeg opmerkingen toe.
8. Om te controleren dat de taak verwijderen is voltooid, controleert u de Azure-berichten ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/messages.png).
9. Na het verwijderen van een item in de lijst op de **back-upinfrastructuur** menu, klikt u op **vernieuwen** om te zien van de items in de kluis.

### <a name="delete-the-vault-after-removing-dependencies"></a>De kluis verwijderen na het verwijderen van afhankelijkheden

1. Wanneer alle afhankelijkheden zijn verwijderd, schuif naar de **Essentials** deelvenster in het kluismenu.
2. Controleren of dat er een **back-up items**, **back-up van beheerservers**, of **gerepliceerde items** vermeld. Als items is nog steeds worden weergegeven in de kluis, verwijdert u deze.

3. Wanneer er zijn geen items meer in de kluis, op het kluisdashboard klikt u op **verwijderen**.

    ![back-upgegevens verwijderen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Om te controleren dat u wilt de kluis verwijderen, klikt u op **Ja**. De kluis wordt verwijderd en u gaat terug naar de **nieuw** Servicemenu.

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>Wat gebeurt er als ik het back-upproces te stoppen, maar blijven de gegevens bewaard?

Als u het back-upproces te stoppen, maar blijven de gegevens per ongeluk bewaard, moet u de back-upgegevens verwijderen, zoals beschreven in de vorige secties.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services-kluizen.
