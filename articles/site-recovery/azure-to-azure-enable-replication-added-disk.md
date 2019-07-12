---
title: Replicatie inschakelen voor een schijf die is toegevoegd aan een Azure-VM gerepliceerd door Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u replicatie inschakelen voor een schijf die is toegevoegd aan een Azure-VM die ingeschakeld voor herstel na noodgeval met Azure Site Recovery
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: asgang
ms.openlocfilehash: 4a262a3a0c32516988890a6afc6eef34d8655c89
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671879"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Replicatie inschakelen voor een schijf die is toegevoegd aan een Azure-VM


In dit artikel wordt beschreven hoe u replicatie voor gegevensschijven die zijn toegevoegd aan een Azure-VM al ingeschakeld voor herstel na noodgevallen naar een andere Azure-regio in te schakelen met behulp van [Azure Site Recovery](site-recovery-overview.md).

Inschakelen van replicatie voor een schijf die u aan een virtuele machine toevoegt wordt ondersteund voor virtuele Azure-machines met beheerde schijven.

Wanneer u een nieuwe schijf aan een Azure-VM die wordt gerepliceerd naar een andere Azure-regio toevoegt, gebeurt het volgende:

-   Een waarschuwing weergegeven, ziet u de replicatiestatus voor de virtuele machine en een opmerking in de portal wordt geïnformeerd dat een of meer schijven zijn beschikbaar voor beveiliging.
-   Als u beveiliging voor de toegevoegde schijven inschakelt, wordt de waarschuwing verdwijnt na de initiële replicatie van de schijf.
-   Als u ervoor kiest geen replicatie in te schakelen voor de schijf, kunt u de waarschuwing negeren.

![Nieuwe schijf toegevoegd](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u al hebt ingesteld u herstel na noodgevallen voor de virtuele machine waarop u de schijf wilt toevoegen. Als u nog niet hebt, volgt u de [zelfstudie voor herstel na noodgevallen van Azure naar Azure](azure-to-azure-tutorial-enable-replication.md). 

## <a name="enable-replication-for-an-added-disk"></a>Replicatie inschakelen voor een toegevoegde schijf 

U schakelt replicatie voor een extra schijf, het volgende doen:

1. In de kluis > **gerepliceerde Items**, klikt u op de virtuele machine waaraan u de schijf toegevoegd.
2. Klik op **schijven**, en selecteer vervolgens de gegevensschijf waarvan u wilt replicatie in te schakelen (deze schijven hebben een **niet beveiligd** status).
3.  In **Schijfdetails**, klikt u op **inschakelen replicatie**.

    ![Replicatie inschakelen voor toegevoegde schijf](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Nadat de taak inschakelen van replicatie is uitgevoerd en de initiële replicatie is voltooid, wordt de replicatie: waarschuwing voor het probleem schijf verwijderd.



# <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
