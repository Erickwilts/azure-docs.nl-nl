---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: d4e8d99cd7c67136f359772664eb017c6207e6e4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165535"
---
### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Een TCP-eindpunt maken voor de virtuele machine
Voor toegang tot SQL Server via internet, moet de virtuele machine een eindpunt hebben om te luisteren naar binnenkomende TCP-communicatie. Deze configuratiestap van Azure zorgt ervoor dat binnenkomend TCP-poortverkeer naar een TCP-poort wordt geleid die toegankelijk is voor de virtuele machine.

> [!NOTE]
> Als u verbinding binnen de dezelfde cloudservice of een virtueel netwerk maakt, hoeft u niet om een openbaar toegankelijk eindpunt te maken. In dat geval kunt u doorgaan met de volgende stap. Zie [Verbindingsscenario's](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios) voor meer informatie.
> 
> 

1. Selecteer **Virtuele machines (klassiek)** in Azure Portal.
2. Selecteer vervolgens uw virtuele machine voor SQL Server.
3. Selecteer **Eindpunten** en klik bovenaan de blade Eindpunten op de knop **Toevoegen**.
   
    ![Stappen in Azure Portal voor het maken van een eindpunt](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. Op de blade **Eindpunt toevoegen** geeft u een **naam** op, zoals SQLEndpoint.
5. Selecteer **TCP** bij **Protocol**.
6. Voor **Openbare poort** geeft u een poortnummer op, zoals **57500**.
7. Voor **Particuliere poort** geeft u de luisterende poorten van de SQL Server op. De standaardwaarde is **1433**.
8. Klik op **OK** om het eindpunt te maken.

