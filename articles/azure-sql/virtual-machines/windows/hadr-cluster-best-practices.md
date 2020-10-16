---
title: Aanbevolen procedures voor cluster configuratie
description: Meer informatie over de ondersteunde cluster configuraties wanneer u hoge Beschik baarheid en herstel na nood gevallen (HADR) configureert voor SQL Server op Azure Virtual Machines, zoals ondersteunde quorums of routerings opties voor verbindingen.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: e98bfbf58c179fe9df0d99e0522e5747d220ae52
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91317018"
---
# <a name="cluster-configuration-best-practices-sql-server-on-azure-vms"></a>Aanbevolen procedures voor clusterconfiguratie (SQL Server op virtuele machines van Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Een cluster wordt gebruikt voor hoge Beschik baarheid en herstel na nood gevallen (HADR) met SQL Server op Azure Virtual Machines (Vm's). 

In dit artikel vindt u aanbevolen procedures voor cluster configuratie voor [failoverclusterinstanties (failover cluster instances)](failover-cluster-instance-overview.md) en [beschikbaarheids groepen](availability-group-overview.md) wanneer u deze gebruikt met SQL Server op virtuele machines van Azure. 


## <a name="networking"></a>Netwerken

Gebruik één NIC per server (cluster knooppunt) en één subnet. Azure-netwerken hebben fysieke redundantie, waardoor extra Nic's en subnetten onnodig worden op een gast cluster van de virtuele machine van Azure. In het cluster validatie rapport wordt u gewaarschuwd dat de knoop punten alleen bereikbaar zijn op één netwerk. U kunt deze waarschuwing negeren op virtuele Azure-machine gast-failoverclusters.

## <a name="quorum"></a>Quorum

Hoewel een cluster met twee knoop punten werkt zonder [quorum bron](/windows-server/storage/storage-spaces/understand-quorum), zijn klanten strikt verplicht een quorum bron te gebruiken voor productie-ondersteuning. Cluster validatie geeft geen cluster zonder quorum resource door. 

Technisch gesp roken kan een cluster met drie knoop punten een verlies van één knoop punt belopen (tot twee nodes) zonder een quorum resource. Maar nadat het cluster is gedalen op twee knoop punten, is er een risico dat de geclusterde bronnen offline gaan in het geval van een verlies of communicatie fout om te voor komen dat een scenario voor Split wordt gespleten.

Als u een quorum bron configureert, kan het cluster online met slechts één knoop punt online gaan.

De volgende tabel bevat de beschik bare quorum opties in de volg orde die wordt aanbevolen voor gebruik met een virtuele Azure-machine, waarbij de schijfwitness de voorkeurs keuze is: 


||[Schijfwitness](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |[Cloudwitness](/windows-server/failover-clustering/deploy-cloud-witness)  |[Bestandsshare-witness](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |
|---------|---------|---------|---------|
|**Ondersteund besturings systeem**| Alles |Windows Server 2016 +| Alles|




### <a name="disk-witness"></a>Schijfwitness

Een schijfwitness is een kleine geclusterde schijf in het cluster beschik bare opslag groep. Deze schijf is Maxi maal beschikbaar en kan een failover tussen knoop punten uitvoeren. Het bevat een kopie van de cluster database, met een standaard grootte van meestal minder dan 1 GB. De schijfwitness is de voorkeurs quorum optie voor elk cluster dat gebruikmaakt van gedeelde Azure-schijven (of een oplossing voor gedeelde schijven, zoals gedeeld SCSI-, iSCSI-of Fibre Channel-SAN).  Een geclusterd gedeeld volume kan niet worden gebruikt als schijfwitness.

Een gedeelde Azure-schijf configureren als de schijfwitness. 

Zie [een schijfwitness configureren](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)om aan de slag te gaan.


**Ondersteund besturings systeem**: alle   


### <a name="cloud-witness"></a>Cloudwitness

Een cloudwitness is een type quorum-Witness van het failovercluster dat gebruikmaakt van Microsoft Azure om een stem op het cluster quorum te bieden. De standaard grootte is ongeveer 1 MB en bevat alleen de tijds tempel. Een cloudwitness is ideaal voor implementaties in meerdere sites, meerdere zones en meerdere regio's.

Zie [een Cloudwitness configureren](/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp)om aan de slag te gaan.


**Ondersteund besturings systeem**: Windows Server 2016 en hoger   


### <a name="file-share-witness"></a>Bestandsshare-witness

Een bestandssharewitness is een SMB-bestands share die doorgaans is geconfigureerd op een bestands server met Windows Server. Het onderhoudt cluster informatie in een Witness. log-bestand, maar slaat geen kopie van de cluster database op. In azure kunt u een [Azure-bestands share](../../../storage/files/storage-how-to-create-file-share.md) configureren om te gebruiken als de bestandssharewitness of u kunt een bestands share gebruiken op een afzonderlijke virtuele machine.

Als u een Azure-bestands share wilt gebruiken, kunt u deze koppelen aan hetzelfde proces dat wordt gebruikt om [de Premium-bestands share te koppelen](failover-cluster-instance-premium-file-share-manually-configure.md#mount-premium-file-share). 

Zie [Configure a file share Witness](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)om aan de slag te gaan.


**Ondersteund besturings systeem**: Windows Server 2012 en hoger   

## <a name="connectivity"></a>Connectiviteit

In een traditionele on-premises netwerk omgeving lijkt een SQL Server failover-cluster exemplaar één exemplaar van SQL Server uitgevoerd op één computer. Omdat het failover-cluster exemplaar van het knoop punt naar het knoop punt wordt gefailovert, biedt de virtuele netwerk naam (VNN) voor de instantie een uniform verbindings punt en kunnen toepassingen verbinding maken met het SQL Server exemplaar zonder te weten welk knoop punt momenteel actief is. Wanneer een failover optreedt, wordt de naam van het virtuele netwerk geregistreerd bij het nieuwe actieve knoop punt nadat het is gestart. Dit proces is transparant voor de client of toepassing die verbinding maakt met SQL Server, waardoor de uitval tijd van de client of toepassing tijdens een storing wordt geminimaliseerd. 

Gebruik een VNN met Azure Load Balancer of een gedistribueerde netwerk naam (DNN) om verkeer door te sturen naar de VNN van het failovercluster met SQL Server op virtuele machines van Azure. De functie DNN is momenteel alleen beschikbaar voor SQL Server 2019 CU2 en hoger op een virtuele machine met Windows Server 2016 (of hoger). 

De volgende tabel vergelijkt de HADR-verbindings ondersteuning: 

| |**Naam van virtueel netwerk (VNN)**  |**Gedistribueerde netwerknaam (DNN)**  |
|---------|---------|---------|
|**Minimale versie van het besturingssysteem**| Alles | Alles |
|**Minimale SQL Server versie** |Alles |SQL Server 2019 CU2|
|**Ondersteunde HADR-oplossing** | Failover-clusterexemplaar <br/> Beschikbaarheidsgroep | Failover-clusterexemplaar|


### <a name="virtual-network-name-vnn"></a>Naam van virtueel netwerk (VNN)

Omdat het virtuele IP-toegangs punt anders werkt in azure, moet u [Azure Load Balancer](../../../load-balancer/index.yml) zodanig configureren dat verkeer wordt gerouteerd naar het IP-adres van de FCI-knoop punten. In virtuele machines van Azure bevat een load balancer het IP-adres voor de VNN waarop de geclusterde SQL Server resources zijn gebaseerd. De load balancer distribueert inkomende stromen die aan de front-end arriveren en routert dat verkeer vervolgens naar de instanties die zijn gedefinieerd door de back-end-pool. U configureert de verkeers stroom met behulp van regels voor taak verdeling en status controles. Met SQL Server FCI zijn de exemplaren van de back-end-pool de virtuele machines van Azure waarop SQL Server worden uitgevoerd. 

Er is een lichte vertraging bij de failover wanneer u de load balancer gebruikt, omdat de status test altijd om de 10 seconden wordt gecontroleerd. 

Leer hoe u [Azure Load Balancer kunt configureren voor een FCI](hadr-vnn-azure-load-balancer-configure.md)om aan de slag te gaan. 

**Ondersteund besturings systeem**: alle   
**Ondersteunde SQL-versie**: alle   
**Ondersteunde HADR-oplossing**: failover-cluster exemplaar en beschikbaarheids groep   


### <a name="distributed-network-name-dnn"></a>Gedistribueerde netwerknaam (DNN)

De naam van een gedistribueerde netwerk is een nieuwe Azure-functie voor SQL Server 2019 CU2. De DNN biedt een andere manier om clients te SQL Server verbinding maken met het SQL Server failover cluster-exemplaar zonder een load balancer te gebruiken. 

Wanneer er een DNN-resource wordt gemaakt, wordt de DNS-naam met de IP-adressen van alle knoop punten in het cluster gebonden aan het cluster. De SQL-client probeert verbinding te maken met elk IP-adres in deze lijst om het knoop punt te vinden waarop het failovercluster momenteel wordt uitgevoerd. U kunt dit proces versnellen door `MultiSubnetFailover=True` in de Connection String op te geven. Met deze instelling geeft u aan dat de provider alle IP-adressen parallel moet proberen, zodat de client direct verbinding kan maken met de FCI. 

Als dat mogelijk is, wordt een gedistribueerde netwerk naam aanbevolen voor een load balancer omdat: 
- De end-to-end-oplossing is betrouwbaarder omdat u de load balancer resource niet meer hoeft te onderhouden. 
- Als u de load balancer Probe, wordt de duur van de failover geminimaliseerd. 
- De DNN vereenvoudigt het inrichten en beheren van het failovercluster met SQL Server op Azure-Vm's. 

De meeste SQL Server-functies werken op transparante wijze met FCI. In dergelijke gevallen kunt u de bestaande VNN DNS-naam vervangen door de DNS-naam van de DNN of de DNN-waarde instellen met de bestaande VNN DNS-naam. Voor sommige Server onderdelen is echter een netwerk alias vereist waarmee de naam van de VNN wordt toegewezen aan de naam van de DNN. In bepaalde gevallen kan het expliciete gebruik van de DNS-naam van DNN vereist zijn, bijvoorbeeld wanneer u bepaalde Url's in een configuratie aan server zijde definieert. 

Als u aan de slag wilt gaan, leert u hoe u [een DNN-resource kunt configureren voor een FCI](hadr-distributed-network-name-dnn-configure.md). 

**Ondersteund besturings systeem**: Windows Server 2016 en hoger   
**Ondersteunde SQL-versie**: SQL Server 2019 en hoger   
**Ondersteunde HADR-oplossing**: alleen het exemplaar van het failovercluster


## <a name="limitations"></a>Beperkingen

Houd rekening met de volgende beperkingen wanneer u werkt met FCI-of beschikbaarheids groepen en SQL Server op Azure Virtual Machines. 

### <a name="msdtc"></a>MSDTC 

Azure Virtual Machines ondersteunt micro soft Distributed Transaction Coordinator (MSDTC) op Windows Server 2019 met opslag op geclusterde gedeelde volumes (CSV) en [Azure Standard Load Balancer](../../../load-balancer/load-balancer-standard-overview.md) of op SQL Server-vm's die gebruikmaken van gedeelde Azure-schijven. 

In azure Virtual Machines wordt MSDTC niet ondersteund voor Windows Server 2016 of eerder met geclusterde gedeelde volumes, omdat:

- De geclusterde MSDTC-bron kan niet worden geconfigureerd voor het gebruik van gedeelde opslag. Als u in Windows Server 2016 een MSDTC-bron maakt, wordt er geen gedeelde opslag weer gegeven die beschikbaar is voor gebruik, zelfs als de opslag ruimte beschikbaar is. Dit probleem is opgelost in Windows Server 2019.
- Met de basis load balancer worden geen RPC-poorten afgehandeld.


## <a name="next-steps"></a>Volgende stappen

Nadat u de juiste aanbevolen procedures voor uw oplossing hebt vastgesteld, kunt u aan de slag gaan door [uw SQL Server-VM voor te bereiden voor FCI](failover-cluster-instance-prepare-vm.md). U kunt ook uw beschikbaarheids groep maken met behulp van de [Azure cli](availability-group-az-cli-configure.md)-of [Azure Quick](availability-group-quickstart-template-configure.md)start-sjablonen. 

