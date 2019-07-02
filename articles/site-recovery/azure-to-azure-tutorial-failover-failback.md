---
title: Failover uitvoeren en opnieuw beveiligen van virtuele Azure-machines gerepliceerd naar een secundaire Azure-regio voor herstel na noodgeval met de Azure Site Recovery-service.
description: Leer hoe u een failover en opnieuw beveiligen van virtuele Azure-machines gerepliceerd naar een secundaire Azure-regio voor herstel na noodgevallen, met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 836b6bb76ff608fed0f34e40fe450d593fec540a
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514154"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Failover en Azure VM's tussen regio's opnieuw beveiligen

Deze zelfstudie wordt beschreven hoe u een failover uitgevoerd voor een Azure virtuele machine (VM) naar een secundaire Azure-regio met de [Azure Site Recovery](site-recovery-overview.md) service. Nadat u een failover hebt, kunt u de virtuele machine beveiligen. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een failover uitvoeren van de virtuele Azure-machine
> * De secundaire Azure-virtuele machine, opnieuw beveiligen zodat deze wordt gerepliceerd naar de primaire regio.

> [!NOTE]
> In deze zelfstudie bevat de meest eenvoudige pad met standaardinstellingen en minimale aanpassing. Voor complexere scenario's, gebruikt u de artikelen in 'How To' voor virtuele Azure-machines.


## <a name="prerequisites"></a>Vereisten

- Voordat u begint, controleert u [Veelgestelde vragen over](site-recovery-faq.md#failover) over failover.
- Zorg dat u een [herstelanalyse](azure-to-azure-tutorial-dr-drill.md) hebt uitgevoerd om te controleren of alles werkt zoals verwacht.
- Controleer de eigenschappen van de virtuele machine voordat u de testfailover uitvoert. De virtuele machine moet voldoen aan [Azure-vereisten](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).

## <a name="run-a-failover-to-the-secondary-region"></a>Een failover uitvoeren naar de secundaire regio

1. Selecteer in **Gerepliceerde items** de virtuele machine waarnaar u een failover wilt uitvoeren en selecteer **Failover**

   ![Failover](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:

   * **Meest recent** (standaardwaarde): Verwerkt de gegevens in de Site Recovery-service en biedt de laagste Recovery Point Objective (RPO).
   * **Laatst verwerkt**: Hiermee herstelt u de virtuele machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery-service.
   * **Aangepast**: Failover-schakeling naar een bepaald herstelpunt. Deze optie is handig voor het uitvoeren van een testfailover.

3. Selecteer **sluit de computer voordat u begint met failover** als u wilt dat Site Recovery probeert een afsluiten van de bron-VM's voordat de failover wordt geactiveerd. Afsluiten helpt om te controleren of er geen gegevens verloren gaan. De failover wordt voortgezet zelfs als het afsluiten is mislukt. Site Recovery heeft, verwijdert u niet de bron na een failover.

4. Volg de voortgang van de failover op de pagina **Taken**.

5. Valideer de virtuele machine na de failover door u aan te melden bij de virtuele machine. Als u naar een ander herstelpunt voor de virtuele machine wilt gaan, kunt u de optie **Herstelpunt wijzigen** gebruiken.

6. Wanneer u tevreden bent met de virtuele machine waarvoor u de failover hebt uitgevoerd, kunt u de failover **Doorvoeren**.
   Als u de failover doorvoert, worden alle herstelpunten die beschikbaar zijn voor de service verwijderd. Niet nu mogelijk om te wijzigen van het herstelpunt.

> [!NOTE]
> Wanneer u een failover uitvoert voor een virtuele machine waaraan u een schijf toevoegen wanneer u replicatie voor de virtuele machine inschakelt, wordt de schijven die beschikbaar voor herstel zijn door replicatie punten worden weergegeven. Bijvoorbeeld, als een virtuele machine één schijf heeft en u een nieuwe toevoegen, wordt replicatie-punten die zijn gemaakt voordat u de schijf toegevoegd weergegeven dat de replicatie-punt uit '1 van 2 schijven bestaat'.

![Failover met een toegevoegde schijf](./media/azure-to-azure-tutorial-failover-failback/failover-added.png)

## <a name="reprotect-the-secondary-vm"></a>De secundaire virtuele machine opnieuw beveiligen

Na een failover van de virtuele machine moet u deze opnieuw beveiligen zodat deze terug naar de primaire regio wordt gerepliceerd.

1. Zorg ervoor dat de virtuele machine de status **Failover is doorgevoerd** heeft en controleer of de primaire regio beschikbaar is, en of u er nieuwe resources op kunt maken en deze kunt openen.
2. Klik in **Kluis** > **Gerepliceerde items** met de rechtermuisknop op de virtuele machine waarvoor de failover is uitgevoerd is en selecteer vervolgens **Opnieuw beveiligen**.

   ![Klik met de rechtermuisknop om opnieuw te beveiligen](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Controleer of de richting van beveiliging, secundaire naar primaire regio, al is geselecteerd.
3. Controleer de informatie over **resourcegroep, netwerk, opslag en beschikbaarheidssets**. Gemarkeerd als nieuwe resources worden gemaakt als onderdeel van de bewerking opnieuw beveiligen.
4. Klik op **OK** om het opnieuw beveiligen te activeren. Met deze taak wordt de doelsite van de meest recente gegevens voorzien. Vervolgens worden de verschillen naar de primaire regio gerepliceerd. De virtuele machine heeft nu een beveiligde status.

## <a name="next-steps"></a>Volgende stappen
- Na het opnieuw beveiligen, [informatie over hoe u](azure-to-azure-tutorial-failback.md) terug naar de primaire regio mislukken wanneer deze beschikbaar is.
- [Meer informatie](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) over de stroom opnieuw beveiligen.
