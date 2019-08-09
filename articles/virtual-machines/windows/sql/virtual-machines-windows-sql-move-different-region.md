---
title: SQL Server VM verplaatsen naar een andere regio binnen Azure met Azure Site Recovery Services | Microsoft Docs
description: Meer informatie over hoe u uw SQL Server virtuele machine vanuit de ene regio naar de andere kunt migreren in Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/30/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 846b4d95c7eb32af7c1c0ff42c2526612075a033
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882413"
---
# <a name="move-sql-server-vm-to-another-region-within-azure-with-azure-site-recovery-services"></a>SQL Server VM verplaatsen naar een andere regio binnen Azure met Azure Site Recovery Services

In dit artikel leert u hoe u Azure Site Recovery kunt gebruiken om uw SQL Server virtuele machine (VM) te migreren van de ene regio naar een andere in Azure. 

Als u een SQL Server VM naar een andere regio wilt verplaatsen, moet u het volgende doen:
1. [**Voorbereiden**](#prepare-to-move): Controleer of de bron SQL Server virtuele machine en de doel regio adequaat zijn voor bereid voor de verplaatsing. 
1. [**Configureren**](#configure-azure-site-recovery-vault): Als u uw SQL Server-VM wilt verplaatsen, moet dit een gerepliceerd object binnen de Azure Site Recovery kluis zijn. U moet uw SQL Server-VM toevoegen aan de Azure Site Recovery kluis. 
1. [**Testen**](#test-move-process): Voor het migreren van de SQL Server-VM moet het uitvoeren van een failover van de bron regio naar de gerepliceerde doel regio worden uitgevoerd. Om ervoor te zorgen dat het proces wordt verplaatst, moet u eerst testen of uw SQL Server virtuele machine een failover naar de doel regio kan uitvoeren. Op deze manier kunt u alle problemen weer geven en voor komen wanneer u de daad werkelijke verplaatsing uitvoert. 
1. [**Verplaatsen**](#move-the-sql-server-vm): Zodra de testfailover is voltooid en u weet dat u veilig bent om uw SQL Server virtuele machine te migreren, kunt u de virtuele machine verplaatsen naar de doel regio. 
1. [](#clean-up-source-resources)Opschonen: Als u facturerings kosten wilt voor komen, verwijdert u de SQL Server virtuele machine uit de kluis en overbodige resources die resteren in de resource groep. 

## <a name="verify-prerequisites"></a>Vereisten verifiëren 

- Controleer of het verplaatsen van de bron regio naar de doel regio [wordt ondersteund](../../../site-recovery/azure-to-azure-support-matrix.md#region-support).  
- Bekijk de [scenario architectuur en-onderdelen](../../../site-recovery/azure-to-azure-architecture.md) , evenals de [beperkingen en vereisten voor ondersteuning](../../../site-recovery/azure-to-azure-support-matrix.md). 
- Controleer de account machtigingen. Als u uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnements beheerder bent, kunt u met de beheerder samen werken om de benodigde machtigingen toe te wijzen. Als u replicatie wilt inschakelen voor een VM en gegevens wilt kopiëren met behulp van Azure Site Recovery, hebt u het volgende nodig: 
    - Machtigingen voor het maken van een virtuele machine. De ingebouwde rol Inzender voor de *virtuele machine* heeft deze machtigingen, waaronder: 
        - Machtigingen voor het maken van een virtuele machine in de geselecteerde resource groep. 
        - Machtigingen voor het maken van een virtuele machine in het geselecteerde virtuele netwerk. 
        - Machtigingen voor het schrijven naar het geselecteerde opslag account. 
      - Machtigingen voor het beheren van Azure Site Recovery bewerkingen. De rol *site Recovery Inzender* heeft alle machtigingen die nodig zijn voor het beheren van site Recovery bewerkingen in een Recovery Services kluis.  

## <a name="prepare-to-move"></a>Voorbereiden om te verplaatsen
Bereid de bron SQL Server-VM en de doel regio voor de verplaatsing voor. 

### <a name="prepare-the-source-sql-server-vm"></a>De bron SQL Server VM voorbereiden

- Zorg ervoor dat alle meest recente basis certificaten zich op de SQL Server VM bevinden die u wilt verplaatsen. Als de meest recente basis certificaten niet aanwezig zijn, voor komt u dat de gegevens naar de doel regio worden gekopieerd. 
- Voor virtuele Windows-machines installeert u alle meest recente Windows-updates op de VM, zodat alle vertrouwde basis certificaten zich op de computer bevinden. In een niet-verbonden omgeving voert u het standaard Windows UPdate-en certificaat update proces voor uw organisatie uit. 
- Voor Linux-Vm's volgt u de richt lijnen van uw Linux-distributeur om de meest recente vertrouwde basis certificaten en certificaatintrekkingslijsten te verkrijgen op de virtuele machine. 
- Zorg ervoor dat u geen verificatie proxy gebruikt voor het beheren van de netwerk verbinding voor de virtuele machines die u wilt verplaatsen. 
- Als de virtuele machine die u wilt verplaatsen geen toegang tot internet heeft of een firewall proxy gebruikt om uitgaande toegang te beheren, controleert u de vereisten. 
- Identificeer de bron netwerk indeling en alle resources die u momenteel gebruikt. Dit omvat, maar is niet beperkt tot load balancers, netwerk beveiligings groepen (Nsg's) en open bare Ip's. 

### <a name="prepare-the-target-region"></a>De doelregio voorbereiden

- Controleer of u met uw Azure-abonnement virtuele machines kunt maken in de doel regio die wordt gebruikt voor herstel na nood gevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum. 
- Zorg ervoor dat uw abonnement voldoende bronnen heeft voor de ondersteuning van Vm's met een grootte die overeenkomt met uw bron-Vm's. Als u Site Recovery gebruikt voor het kopiëren van gegevens naar het doel, Site Recovery kiest voor dezelfde grootte of de dichtstbijzijnde grootte voor de doel-VM. 
- Zorg ervoor dat u een doel bron maakt voor elk onderdeel dat in de bron netwerk indeling wordt geïdentificeerd. Deze stap is belang rijk om ervoor te zorgen dat uw virtuele machines beschikken over alle functionaliteit en functies in de doel regio die u in de bron regio had. 
    - Azure Site Recovery detecteert en maakt automatisch een virtueel netwerk wanneer u replicatie inschakelt voor de bron-VM. U kunt ook een netwerk vooraf maken en dit toewijzen aan de virtuele machine in de gebruikers stroom voor het inschakelen van replicatie. U moet hand matig andere resources in de doel regio maken.
- Als u de meest gebruikte netwerk bronnen wilt maken die relevant zijn voor u op basis van de configuratie van de bron-VM, raadpleegt u de volgende documentatie: 
    - [Netwerkbeveiligingsgroepen](../../../virtual-network/tutorial-filter-network-traffic.md) 
    - [Load balancer](../../../load-balancer/tutorial-load-balancer-basic-internal-portal.md)
    - [Openbaar IP-adres](../../../virtual-network/virtual-network-public-ip-address.md)
    - Raadpleeg de [documentatie bij netwerken](../../../virtual-network/virtual-networks-overview.md)voor aanvullende netwerk onderdelen.
- Maak hand matig een niet-productie netwerk in de doel regio als u de configuratie wilt testen voordat u de laatste overstap naar de doel regio uitvoert. We raden u aan deze stap uit te voeren omdat deze minimale interferentie van het productie netwerk garandeert. 

## <a name="configure-azure-site-recovery-vault"></a>Azure Site Recovery kluis configureren

De volgende stappen laten zien hoe u Azure Site Recovery kunt gebruiken om gegevens te kopiëren naar de doel regio. Maak de Recovery Services kluis in een andere regio dan de bron regio. 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com). 
1. Kies ervoor om **een resource te maken** in de linkerbovenhoek van het navigatie deel venster. 
1. Selecteer **deze &-beheer hulpprogramma's** en selecteer vervolgens **back-up en site Recovery**. 
1. Op het tabblad **basis beginselen** onder **Project Details**maakt u een nieuwe resource groep in de doel regio of selecteert u een bestaande resource groep in de doel regio. 
1. Geef onder **Details van exemplaar**een naam op voor uw kluis en selecteer vervolgens uw doel **regio** in de vervolg keuzelijst. 
1. Selecteer **controleren + maken** om uw Recovery Services kluis te maken. 
1. Selecteer **alle services** in de linkerbovenhoek van het navigatie deel venster en typ `recovery services`in het zoekvak. 
1. Eventueel Selecteer de ster naast **Recovery Services kluizen** om deze toe te voegen aan uw snelle navigatie balk. 
1. Selecteer **Recovery Services-kluizen** en selecteer vervolgens de Recovery Services kluis die u hebt gemaakt. 
1. Selecteer in het deel venster **overzicht** de optie **repliceren**. 

   ![Replicatie configureren](media/virtual-machines-windows-sql-move-to-new-region/configure-replication.png)

1. Selecteer **bron** en selecteer vervolgens **Azure** als bron. Selecteer de juiste waarden voor de vervolg keuzelijst andere velden, zoals de locatie voor de bron-Vm's. Alleen resource groepen die zich in de **bron locatie** regio bevinden, worden weer gegeven in het veld **bron resource groep** . 
1. Selecteer **virtuele machines** en kies vervolgens de virtuele machines die u wilt migreren. Selecteer **OK** om uw VM-selectie op te slaan. 
1. Selecteer **instellingen**en kies vervolgens uw **doel locatie** in de vervolg keuzelijst. Dit moet de resource groep zijn die u eerder hebt voor bereid. 
1. Wanneer u de replicatie hebt aangepast, selecteert u **doel resources maken** om de resources op de nieuwe locatie te maken. 
1. Nadat het maken van de resource is voltooid, selecteert u **replicatie inschakelen** om de replicatie van uw SQL Server-VM te starten vanuit de bron naar de doel regio.
1. U kunt de status van de replicatie controleren door te navigeren naar uw Recovery-kluis, **gerepliceerde items** te selecteren en de **status** van uw SQL Server virtuele machine weer te geven. De status **Protected** geeft aan dat de replicatie is voltooid. 

   ![Replicatie status controleren](media/virtual-machines-windows-sql-move-to-new-region/check-replication-status.png)

## <a name="test-move-process"></a>Verplaats proces testen
De volgende stappen laten zien hoe u Azure Site Recovery kunt gebruiken om het verplaatsings proces te testen. 

1. Navigeer naar uw **Recovery Services kluis** in de [Azure Portal](https://portal.azure.com) en selecteer **gerepliceerde items**. 
1. Selecteer de SQL Server VM die u wilt verplaatsen, Controleer of de **replicatie status** **in orde** wordt weer gegeven en selecteer vervolgens **failover testen**. 

   ![Testfailover voor uw VM](media/virtual-machines-windows-sql-move-to-new-region/test-failover-of-replicated-vm.png)

1. Selecteer op de pagina **failover testen** het **meest recente toepassings consistente** herstel punt dat voor de failover moet worden gebruikt, zoals dit het enige type moment opname is dat de consistentie van SQL Server gegevens kan garanderen. 
1. Selecteer het virtuele netwerk onder **Azure Virtual Network** en selecteer **OK** om de failover te testen. 
   
   >[!IMPORTANT]
   > U wordt aangeraden een afzonderlijk Azure VM-netwerk te gebruiken voor de failover-test. Gebruik niet het productie netwerk dat is ingesteld toen u replicatie inschakelde en dat u de Vm's uiteindelijk wilt verplaatsen. 

1. Als u de voortgang wilt bewaken, gaat u naar uw kluis, selecteert u **site Recovery taken** onder **bewaking**en selecteert u vervolgens de taak **testfailover** die wordt uitgevoerd.

   ![Voortgang van testen van testfailover controleren](media/virtual-machines-windows-sql-move-to-new-region/monitor-failover-test-job.png)

1. Wanneer de test is voltooid, gaat u naar **virtuele machines** in de portal en bekijkt u de nieuwe virtuele machine. Zorg ervoor dat de SQL Server virtuele machine wordt uitgevoerd, de juiste grootte heeft en is verbonden met het juiste netwerk. 
1. Verwijder de virtuele machine die is gemaakt als onderdeel van de test, omdat de **failover** -optie grijs wordt weer gegeven tot de failover-test resources zijn opgeschoond. Ga terug naar de kluis, selecteer **gerepliceerde items**, selecteer de SQL Server VM en selecteer vervolgens **testfailover**opschonen. Leg alle waarnemingen vast die zijn gekoppeld aan de test in de sectie **notities** en schakel het selectie **vakje in naast testen is voltooid. Virtuele machines**voor testfailover verwijderen. Selecteer **OK** om de resources na de test op te schonen. 

   ![items opschonen na failover-test](media/virtual-machines-windows-sql-move-to-new-region/cleanup-test-items.png)

## <a name="move-the-sql-server-vm"></a>De SQL Server VM verplaatsen 
De volgende stappen laten zien hoe u de SQL Server VM van de bron regio kunt verplaatsen naar de doel regio. 

1. Ga naar de **Recovery Services** kluis, selecteer **gerepliceerde items**, selecteer de virtuele machine en selecteer vervolgens **failover**. 

   ![Failover starten](media/virtual-machines-windows-sql-move-to-new-region/initiate-failover.png)

1. Selecteer het **meest recente toepassings consistente** herstel punt onder **herstel punt**. 
1. Schakel het selectie vakje in naast **de computer afsluiten voordat de failover wordt gestart**. Site Recovery probeert de bron-VM af te sluiten voordat de failover wordt geactiveerd. Failover wordt voortgezet, zelfs als afsluiten mislukt. 
1. Selecteer **OK** om de failover te starten.
1. U kunt het failoverproces bewaken op dezelfde **site Recovery** -pagina die u hebt bekeken bij het controleren van de failover-test in de vorige sectie. 
1. Nadat de taak is voltooid, controleert u of de SQL Server virtuele machine wordt weer gegeven in de doel regio zoals verwacht. 
1. Ga terug naar de kluis, selecteer **gerepliceerde items**, selecteer de SQL Server virtuele machine en selecteer **door voeren** om het verplaatsings proces te volt ooien naar de doel regio. Wacht tot de doorvoer taak is voltooid. 
1. Registreer uw SQL Server-VM met de resource provider van de SQL-VM om de beheer baarheid van **virtuele SQL-machines** in te scha kelen in de Azure Portal en functies die zijn gekoppeld aan de resource provider. Zie [SQL Server VM registreren bij SQL VM resource provider](virtual-machines-windows-sql-register-with-rp.md)voor meer informatie. 

  > [!WARNING]
  > SQL Server consistentie van gegevens wordt alleen gegarandeerd met app-consistente moment opnamen. De **laatste** verwerkte moment opname kan niet worden gebruikt voor SQL Server failover, omdat een moment opname van een crash herstel niet kan garanderen SQL Server consistentie van de gegevens. 

## <a name="clean-up-source-resources"></a>Bron resources opschonen
Als u facturerings kosten wilt voor komen, verwijdert u de SQL Server virtuele machine uit de kluis en verwijdert u overbodige gekoppelde resources. 

1. Ga terug naar de **site Recovery** kluis, selecteer **gerepliceerde items**en selecteer de SQL Server VM. 
1. Selecteer **replicatie uitschakelen**. Selecteer een reden voor het uitschakelen van de beveiliging en selecteer **OK** om de replicatie uit te scha kelen. 

   >[!IMPORTANT]
   > Het is belang rijk om deze stap uit te voeren om te voor komen dat Azure Site Recovery-replicatie in rekening wordt gebracht. 

1. Als u geen plannen hebt om een van de resources in de bron regio opnieuw te gebruiken, verwijdert u alle relevante netwerk resources en de bijbehorende opslag accounts. 


## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen: 

* [Overzicht van SQL Server op een Windows-VM](virtual-machines-windows-sql-server-iaas-overview.md)
* [Veelgestelde vragen over Windows-VM'S SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server over de prijs informatie voor Windows-VM'S](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Opmerkingen bij de release van een Windows-VM SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)


