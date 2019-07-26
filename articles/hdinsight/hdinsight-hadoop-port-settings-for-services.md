---
title: Poorten die worden gebruikt door Hadoop-Services op HDInsight-Azure
description: Een lijst met poorten die worden gebruikt door Hadoop-services die worden uitgevoerd op HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/27/2019
ms.author: hrasheed
ms.openlocfilehash: 34ab49378f9237a42bed869a6f6d67249b5238f9
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464695"
---
# <a name="ports-used-by-apache-hadoop-services-on-hdinsight"></a>Poorten die worden gebruikt door Apache Hadoop Services in HDInsight

Dit document bevat een lijst met de poorten die worden gebruikt door Apache Hadoop services die worden uitgevoerd op HDInsight-clusters op basis van Linux. Het bevat ook informatie over poorten die worden gebruikt om via SSH verbinding te maken met het cluster.

## <a name="public-ports-vs-non-public-ports"></a>Open bare poorten versus niet-open bare poorten

HDInsight-clusters op basis van Linux bieden alleen drie poorten openbaar op internet. 22, 23 en 443. Deze poorten worden gebruikt voor een veilige toegang tot het cluster met behulp van SSH en services die worden weer gegeven via het beveiligde HTTPS-protocol.

HDInsight wordt intern geïmplementeerd door verschillende Azure-Virtual Machines (de knoop punten in het cluster) die worden uitgevoerd op een Azure-Virtual Network. Vanuit het virtuele netwerk hebt u toegang tot poorten die niet beschikbaar zijn via internet. Als u bijvoorbeeld verbinding maakt met een van de hoofd knooppunten via SSH, kunt u vanuit het hoofd knooppunt rechtstreeks toegang krijgen tot services die worden uitgevoerd op de cluster knooppunten.

> [!IMPORTANT]  
> Als u geen Azure Virtual Network opgeeft als een configuratie optie voor HDInsight, wordt er automatisch een gemaakt. U kunt echter geen andere computers (zoals andere Azure Virtual Machines of uw client development machine) toevoegen aan dit virtuele netwerk.

Als u extra computers wilt toevoegen aan het virtuele netwerk, moet u eerst het virtuele netwerk maken en dit vervolgens opgeven wanneer u uw HDInsight-cluster maakt. Zie [een virtueel netwerk plannen voor HDInsight](hdinsight-plan-virtual-network-deployment.md)voor meer informatie.

## <a name="public-ports"></a>Open bare poorten

Alle knoop punten in een HDInsight-cluster bevinden zich in een Azure-Virtual Network en zijn niet rechtstreeks toegankelijk vanaf internet. Een open bare gateway biedt Internet toegang tot de volgende poorten, die gemeen schappelijk zijn voor alle typen HDInsight-clusters.

| Service | Port | Protocol | Description |
| --- | --- | --- | --- |
| sshd |22 |SSH |Verbindt clients met sshd op de primaire hoofd knooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |22 |SSH |Verbindt clients met sshd op het Edge-knoop punt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| sshd |23 |SSH |Verbindt clients met sshd op de secundaire hoofd knooppunt. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie. |
| Ambari |443 |HTTPS |Ambari-webgebruikersinterface. Zie [HDInsight beheren met de Web-UI van Apache Ambari](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. Zie [HDInsight beheren met behulp van de Apache Ambari rest API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. Zie [MapReduce gebruiken met krul](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |Maakt verbinding met de Hive met behulp van ODBC. Zie [Excel verbinden met HDInsight met het micro soft ODBC-stuur programma](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |Maakt verbinding met ApacheHive met behulp van JDBC. Zie [verbinding maken met Apache Hive op HDInsight met behulp van het Hive JDBC-stuur programma](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

De volgende opties zijn beschikbaar voor specifieke cluster typen:

| Service | Port | Protocol | Clustertype | Description |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API. Zie [aan de slag met Apache HBase](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API. Zie [Apache Spark-taken extern verzenden met Apache livy](spark/apache-spark-livy-rest-interface.md) |
| Spark Thrift-server |443 |HTTPS |Spark |Spark Thrift-server gebruikt om Hive-query's te verzenden. Zie [Beeline gebruiken met Apache Hive op HDInsight](hadoop/apache-hadoop-use-hive-beeline.md) |
| Storm |443 |HTTPS |Storm |Storm web-gebruikers interface. Zie [Apache Storm-topologieën implementeren en beheren in HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Authentication

Alle services die openbaar beschikbaar zijn op het Internet, moeten worden geverifieerd:

| Port | Referenties |
| --- | --- |
| 22 of 23 |De SSH-gebruikers referenties die zijn opgegeven tijdens het maken van het cluster |
| 443 |De aanmeldings naam (standaard: beheerder) en het wacht woord dat is ingesteld tijdens het maken van het cluster |

## <a name="non-public-ports"></a>Niet-open bare poorten

> [!NOTE]  
> Sommige services zijn alleen beschikbaar voor specifieke cluster typen. HBase is bijvoorbeeld alleen beschikbaar op HBase-cluster typen.

> [!IMPORTANT]  
> Sommige services kunnen slechts op één hoofd knooppunt tegelijk worden uitgevoerd. Als u probeert verbinding te maken met de service op de primaire hoofd knooppunt en er een fout bericht wordt weer gegeven, voert u de secundaire hoofd knooppunt opnieuw uit.

### <a name="ambari"></a>Ambari

| Service | Knooppunten | Port | URL-pad | Protocol | 
| --- | --- | --- | --- | --- |
| Ambari-webgebruikersinterface | Hoofdknooppunten | 8080 | / | HTTP |
| Ambari REST API | Hoofdknooppunten | 8080 | /api/v1 | HTTP |

Voorbeelden:

* Ambari REST API: `curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| NameNode-webgebruikersinterface |Hoofdknooppunten |30070 |HTTPS |Web-UI voor weer gave van status |
| NameNode-meta gegevens service |hoofd knooppunten |8020 |IPC |Meta gegevens van het bestands systeem |
| DataNode |Alle worker-knoop punten |30075 |HTTPS |Webinterface voor het weer geven van de status, logboeken enzovoort. |
| DataNode |Alle worker-knoop punten |30010 |&nbsp; |Gegevensoverdracht |
| DataNode |Alle worker-knoop punten |30020 |IPC |Meta gegevens bewerkingen |
| Secundaire NameNode |Hoofdknooppunten |50090 |HTTP |Controle punt voor NameNode-meta gegevens |

### <a name="yarn-ports"></a>GAREN-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| Webgebruikersinterface van Resource Manager |Hoofdknooppunten |8088 |HTTP |Web-UI voor Resource Manager |
| Webgebruikersinterface van Resource Manager |Hoofdknooppunten |8090 |HTTPS |Web-UI voor Resource Manager |
| Resource Manager-beheer interface |hoofd knooppunten |8141 |IPC |Voor het indienen van toepassingen (Hive, Hive-server, Pig, enz.) |
| Resource Manager-planner |hoofd knooppunten |8030 |HTTP |Beheer interface |
| Resource Manager-toepassings interface |hoofd knooppunten |8050 |HTTP |Adres van de interface van de toepassings Manager |
| NodeManager |Alle worker-knoop punten |30050 |&nbsp; |Het adres van de container beheerder |
| NodeManager-webgebruikersinterface |Alle worker-knoop punten |30060 |HTTP |Resource Manager-interface |
| Tijdlijn adres |Hoofdknooppunten |10200 |RPC |De RPC-service van de tijdlijn service. |
| Tijdlijn Web-UI |Hoofdknooppunten |8188 |HTTP |De Web-UI van de tijdlijn service |

### <a name="hive-ports"></a>Hive-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| HiveServer2 |Hoofdknooppunten |10001 |Thrift |Service voor het maken van een verbinding met hive (Thrift/JDBC) |
| Hive-metastore |Hoofdknooppunten |9083 |Thrift |Service voor verbinding maken met meta gegevens van Hive (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| WebHCat-server |Hoofdknooppunten |30111 |HTTP |Web-API boven op HCatalog en andere Hadoop-Services |

### <a name="mapreduce-ports"></a>MapReduce-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| JobHistory |Hoofdknooppunten |19888 |HTTP |MapReduce JobHistory-Web-UI |
| JobHistory |Hoofdknooppunten |10020 |&nbsp; |MapReduce JobHistory-server |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Overdracht van tussenliggende toewijzings uitvoer om verminderers aan te vragen |

### <a name="oozie"></a>Oozie

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| Oozie-server |Hoofdknooppunten |11000 |HTTP |URL voor Oozie-service |
| Oozie-server |Hoofdknooppunten |11001 |HTTP |Poort voor Oozie-beheerder |

### <a name="ambari-metrics"></a>Metrische gegevens van Ambari

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| Tijd lijn (toepassings geschiedenis) |Hoofdknooppunten |6188 |HTTP |De Web-UI van de tijdlijn service |
| Tijd lijn (toepassings geschiedenis) |Hoofdknooppunten |30200 |RPC |De Web-UI van de tijdlijn service |

### <a name="hbase-ports"></a>HBase-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| HMaster |Hoofdknooppunten |16000 |&nbsp; |&nbsp; |
| HMaster info-webinterface |Hoofdknooppunten |16010 |HTTP |De poort voor de HBase Master Web-UI |
| Regio server |Alle worker-knoop punten |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |De poort die clients gebruiken om verbinding te maken met ZooKeeper |

### <a name="kafka-ports"></a>Kafka-poorten

| Service | Knooppunten | Port | Protocol | Description |
| --- | --- | --- | --- | --- |
| Broker |Worker-knooppunten |9092 |[Kafka Wire Protocol](https://kafka.apache.org/protocol.html) |Gebruikt voor client communicatie |
| &nbsp; |ZooKeeper-knooppunten |2181 |&nbsp; |De poort die clients gebruiken om verbinding te maken met Zookeeper |

### <a name="spark-ports"></a>Spark-poorten

| Service | Knooppunten | Port | Protocol | URL-pad | Description |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift-servers |Hoofdknooppunten |10002 |Thrift | &nbsp; | Service voor het maken van verbinding met Spark SQL (Thrift/JDBC) |
| Livy-server | Hoofdknooppunten | 8998 | HTTP | &nbsp; | Service voor het uitvoeren van instructies, taken en toepassingen |
| Jupyter Notebook | Hoofdknooppunten | 8001 | HTTP | &nbsp; | Website van Jupyter-notebook |

Voorbeelden:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. In dit voor beeld `10.0.0.11` is het IP-adres van de hoofd knooppunt die als host fungeert voor de livy-service.
