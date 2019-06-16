---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - best practices voor infrastructuur
description: Informatie over aanbevolen procedures van de infrastructuur voor de migratie on-premises Hadoop-clusters op Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: hrasheed
ms.openlocfilehash: 5bdd5049b7ddeaac4425734aa6f4d633b08cd3b4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057474"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - best practices voor infrastructuur

In dit artikel biedt aanbevelingen voor het beheren van de infrastructuur van Azure HDInsight-clusters. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="plan-for-hdinsight-cluster-capacity"></a>Plan voor de capaciteit van de HDInsight-cluster

De belangrijkste opties om u te maken voor de capaciteitsplanning voor HDInsight-cluster zijn als volgt:

- **Kies de regio** -de Azure-regio bepaalt waar het cluster fysiek is ingericht. Om te beperken de latentie van lees- en schrijfbewerkingen, moet het cluster zich in dezelfde regio als de gegevens.
- **Kies opslaglocatie en grootte** -standaardopslag moet zich in dezelfde regio bevinden als het cluster. Voor een cluster 48-knooppunt, is het aanbevolen dat 4 tot en met 8 storage-accounts. Hoewel er mogelijk al voldoende totale opslag, biedt elk opslagaccount aanvullende netwerken bandbreedte voor de compute-knooppunten. Wanneer er meerdere opslagaccounts, gebruikt u een willekeurige naam voor de storage-account zonder een voorvoegsel. Het doel van een willekeurige naam geven, is de kans op opslag knelpunten (beperking) of gemeenschappelijke-modus fouten verminderen voor alle accounts. Voor betere prestaties, gebruik slechts één container per opslagaccount.
- **Kies de VM-grootte en hetzelfde type (ondersteunt nu de G-serie)** : elk clustertype bevat een set knooppunttypen en elk knooppunttype heeft specifieke opties voor de VM-grootte en hetzelfde type. De VM-grootte en het type wordt bepaald door de CPU-verwerking van kracht, RAM-geheugen en de netwerklatentie. Een gesimuleerde werkbelasting kan worden gebruikt om te bepalen van de optimale VM-grootte en het type voor elk knooppunt.
- **Het aantal worker-knooppunten kiezen** -het oorspronkelijke aantal worker-knooppunten kan worden bepaald met behulp van de gesimuleerde werkbelasting. Het cluster kan later worden geschaald door meer worker-knooppunten om te voldoen aan de piekvraag load toe te voegen. Het cluster kan later worden geschaald wanneer de extra worker-knooppunten niet vereist zijn.

Zie voor meer informatie het artikel [plannen van capaciteit voor HDInsight-clusters](../hdinsight-capacity-planning.md).

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Aanbevolen VM-type gebruiken voor cluster

Zie [standaard configuratie en de virtuele machine knooppuntgrootten voor clusters](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) aanbevolen voor de typen virtuele machines voor elk type van HDInsight-cluster.

## <a name="check-hadoop-components-availability-in-hdinsight"></a>Beschikbaarheid van de Hadoop-onderdelen in HDInsight controleren

Elke versie van HDInsight is een cloud-distributie van een set van onderdelen van Hadoop-ecosysteem. Zie [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) voor meer informatie over alle HDInsight-onderdelen en hun huidige versies.

U kunt ook Apache Ambari-gebruikersinterface of de Ambari REST-API gebruiken om te controleren of de Hadoop-onderdelen en versies in HDInsight.

Toepassingen of onderdelen die beschikbaar waren in clusters on-premises, maar geen deel uitmaken van de HDInsight-clusters kunnen worden toegevoegd op een edge-knooppunt of op een virtuele machine in hetzelfde VNet als de HDInsight-cluster. Een derde partij Hadoop-toepassing die is niet beschikbaar in Azure HDInsight kan worden geïnstalleerd met de optie 'Application' in HDInsight-cluster. Aangepaste Hadoop-toepassingen kunnen worden geïnstalleerd op HDInsight-cluster met behulp van 'scriptacties'. De volgende tabel bevat enkele van de algemene toepassingen en hun HDInsight-integratie-opties:

|**Toepassing**|**Integratie**
|---|---|
|Luchtstroom|IaaS of HDInsight edge-knooppunt
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Geen (alleen HDP)
|Datameer|HDInsight-edge-knooppunt
|Datastax (Cassandra)|IaaS (CosmosDB een alternatief in Azure)
|DataTorrent|IaaS 
|Drill|IaaS 
|Ignite|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|IaaS (CosmosDB een alternatief in Azure)
|NiFi|IaaS 
|Presto|IaaS of HDInsight edge-knooppunt
|Python 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|SAS|IaaS 
|Vertica|IaaS (RESOURCEKLASSE alternatief op Azure)
|Tableau|IaaS 
|Waterlijn|HDInsight-edge-knooppunt
|StreamSets|HDInsight-edge 
|Palantir|IaaS 
|Sailpoint|Iaas 

Zie voor meer informatie het artikel [Apache Hadoop-onderdelen die beschikbaar zijn met verschillende versies van HDInsight](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions)

## <a name="customize-hdinsight-clusters-using-script-actions"></a>HDInsight clusters aanpassen met scriptacties

HDInsight biedt een methode voor configuratie van het cluster met de naam van een **script actie**. Een scriptactie is Bash-script dat wordt uitgevoerd op de knooppunten in een HDInsight-cluster en kan worden gebruikt om extra onderdelen installeren en configuratie-instellingen wijzigen.

Scriptacties moeten worden opgeslagen op een URI die toegankelijk is vanaf het HDInsight-cluster. Ze kunnen worden gebruikt tijdens of na het maken van clusters en kunnen ook worden beperkt tot alleen op bepaalde knooppunttypen worden uitgevoerd.

Het script kan worden opgeslagen of één keer uitgevoerd. De persistente scripts worden gebruikt voor het aanpassen van de nieuwe werkrolknooppunten toegevoegd aan het cluster via het schalen herverdelen. Een persistent script kan ook wijzigingen toepassen op een ander knooppunttype, zoals een hoofdknooppunt, wanneer vergroten / verkleinen optreden.

HDInsight biedt vooraf geschreven scripts voor het installeren van de volgende onderdelen in HDInsight-clusters:

- Een Azure Storage-account toevoegen
- Hue installeren
- Presto installeren
- Solr installeren
- Giraph installeren
- Hive-bibliotheken vooraf laden
- Mono installeren of bijwerken

> [!Note]  
> HDInsight biedt geen rechtstreekse ondersteuning voor aangepaste hadoop-onderdelen of onderdelen worden geïnstalleerd met behulp van scriptacties.

Scriptacties kunnen ook in de Azure Marketplace worden gepubliceerd als een HDInsight-toepassing.

Raadpleeg voor meer informatie de volgende artikelen:

- [Apache Hadoop-toepassingen van derden installeren op HDInsight](../hdinsight-apps-install-applications.md)
- [HDInsight clusters aanpassen met scriptacties](../hdinsight-hadoop-customize-cluster-linux.md)
- [Een HDInsight-toepassing publiceren in de Azure Marketplace](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Aanpassen met Bootstrap HDInsight-configuraties

Wijzigingen in configuraties in de configuratiebestanden, zoals `core-site.xml`, `hive-site.xml` en `oozie-env.xml` kunnen worden gemaakt met Bootstrap. Het volgende script is een voorbeeld met behulp van Powershell [AZ module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) cmdlet [New-AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster):

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzHDInsightClusterConfig '
    | Set—AzHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Zie voor meer informatie het artikel [aanpassen HDInsight-clusters met Bootstrap](../hdinsight-hadoop-customize-cluster-bootstrap.md).  Zie ook [beheren HDInsight-clusters met behulp van de Apache Ambari REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>Toegang client-hulpprogramma's van HDInsight Hadoop cluster edge-knooppunten

Een lege edge-knooppunt is een Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd als op de hoofdknooppunten, maar met geen Hadoop-services die worden uitgevoerd. Het edge-knooppunt kan worden gebruikt voor de volgende doeleinden:

- toegang tot het cluster
- clienttoepassingen testen
- clienttoepassingen die als host fungeert

Edge-knooppunten kunnen worden gemaakt en verwijderd via de Azure-portal en kunnen worden gebruikt tijdens of na het cluster maken. Nadat het edge-knooppunt is gemaakt, kunt u verbinding maken met het edge-knooppunt met behulp van SSH en clienthulpprogramma's voor toegang tot het Hadoop-cluster in HDInsight worden uitgevoerd. Het edge-knooppunt ssh-eindpunt is `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`.


Zie voor meer informatie het artikel [lege edge-knooppunten op Apache Hadoop-clusters in HDInsight gebruiken](../hdinsight-apps-use-edge-node.md).


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Gebruik de functie omhoog en omlaag schalen van clusters

HDInsight biedt flexibiliteit doordat u de optie voor omhoog en omlaag het aantal worker-knooppunten in uw clusters schalen. Deze functie kunt u een cluster verkleinen na uur of in het weekend en vouw dit uit tijdens piek bedrijfsbehoeften te voldoen. Zie voor meer informatie:

* [HDInsight-clusters schalen](../hdinsight-scaling-best-practices.md).
* [Schalen van clusters](../hdinsight-administer-use-portal-linux.md#scale-clusters).

## <a name="use-hdinsight-with-azure-virtual-network"></a>Gebruik HDInsight met Azure Virtual Network

Virtuele netwerken van Azure inschakelen Azure-resources, zoals Azure Virtual Machines, veilig kunt communiceren met elkaar, internet, en on-premises netwerken, door te filteren en routering van netwerkverkeer.

Met behulp van Azure Virtual Network met HDInsight kunt de volgende scenario's:

- Verbinding maken met HDInsight rechtstreeks vanuit een on-premises netwerk.
- Verbinding maken met HDInsight gegevens opslaat in een Azure-netwerk.
- Rechtstreeks toegang hebben tot Hadoop-services die niet openbaar beschikbaar zijn via internet. Bijvoorbeeld, Kafka-API's of de HBase-Java-API.

HDInsight kan ofwel worden toegevoegd aan een nieuwe of bestaande Azure Virtual Network. Als HDInsight wordt toegevoegd aan een bestaand Virtueelnetwerk, de bestaande netwerkbeveiligingsgroepen en de gebruiker gedefinieerde routes moeten worden bijgewerkt voor onbeperkte toegang tot [verschillende IP-adressen](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) in de Azure-Datacenter. Zorg er ook niet op het blokkeren van verkeer naar de [poorten](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ports), die worden gebruikt door HDInsight-services.

> [!Note]  
> HDInsight ondersteunt momenteel geen geforceerde tunneling. Geforceerde tunneling is een subnetinstelling die ervoor zorgt het uitgaande internetverkeer met een apparaat voor controle dat en logboekregistratie. Verwijder geforceerde tunnels zijn voordat het installeren van HDInsight in een subnet of een nieuw subnet maken voor HDInsight. HDInsight ook ondersteunt geen uitgaande netwerkconnectiviteit te beperken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Virtuele-netwerken-overzicht van Azure](../../virtual-network/virtual-networks-overview.md)
- [Azure HDInsight met behulp van een Azure-netwerk uitbreiden](../hdinsight-extend-hadoop-virtual-network.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Veilig verbinding maken met Azure-services met Azure Virtual Network service-eindpunten

HDInsight ondersteunt [virtual network-service-eindpunten](../../virtual-network/virtual-network-service-endpoints-overview.md), waarmee u veilig verbinding maken met Azure Blob Storage, Azure Data Lake Storage Gen2, Cosmos DB en SQL-databases. Als u een Service-eindpunt inschakelt voor Azure HDInsight, wordt verkeer stroomt via een beveiligde route van binnen het Azure-datacentrum. U kunt met deze uitgebreide niveau van beveiliging op de netwerklaag, big data storage-accounts aan de opgegeven virtuele netwerken (VNETs) vergrendelen en HDInsight-clusters nog steeds naadloos toegang tot gegevens en verwerkt die gebruiken.

Raadpleeg voor meer informatie de volgende artikelen:

- [Service-eindpunten voor virtueel netwerk](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [Verbeter de beveiliging van HDInsight met service-eindpunten](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>HDInsight verbinden met de on-premises netwerk

HDInsight kan worden verbonden met de on-premises netwerk met behulp van Azure Virtual Networks en een VPN-gateway. De volgende stappen kunnen worden gebruikt om verbinding te maken:

- Gebruik HDInsight in een Azure Virtual Network die verbonden met de on-premises netwerk is.
- Configureer DNS-naamomzetting tussen het virtuele netwerk en de on-premises netwerk.
- Netwerkbeveiligingsgroepen of de gebruiker gedefinieerde routes (UDR) voor het beheren van netwerkverkeer configureren.

Zie voor meer informatie het artikel [verbinding maken met HDInsight op uw on-premises netwerk](../connect-on-premises-network.md)

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Aanbevolen procedures van de opslag voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-storage.md)
