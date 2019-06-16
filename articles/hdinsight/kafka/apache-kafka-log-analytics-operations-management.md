---
title: Azure Monitor-logboeken voor Apache Kafka - Azure HDInsight
description: Informatie over het gebruik van Azure Monitor-logboeken voor het analyseren van Logboeken van Apache Kafka-cluster in Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2019
ms.openlocfilehash: 1e141aea3b22bfdcb981513f03e595b6c2f15466
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147972"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Logboeken voor Apache Kafka in HDInsight analyseren

Informatie over het gebruik van Azure Monitor-logboeken voor het analyseren van logboeken die worden gegenereerd door Apache Kafka in HDInsight.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Azure Monitor-logboeken voor Apache Kafka inschakelen

De stappen voor het inschakelen van Azure Monitor-logboeken voor HDInsight zijn hetzelfde voor alle HDInsight-clusters. Gebruik de volgende koppelingen voor meer informatie over het maken en configureren van de vereiste services:

1. Een Log Analytics-werkruimte maken. Zie voor meer informatie de [registreert in Azure Monitor](../../azure-monitor/platform/data-platform-logs.md) document.

2. Maken van een Kafka op HDInsight-cluster. Zie voor meer informatie de [beginnen met Apache Kafka in HDInsight](apache-kafka-get-started.md) document.

3. Configureer de Kafka-cluster voor het gebruik van Azure Monitor-Logboeken. Zie voor meer informatie de [gebruikt Azure Monitor-logboeken voor het controleren van HDInsight](../hdinsight-hadoop-oms-log-analytics-tutorial.md) document.

> [!IMPORTANT]  
> Het kan ongeveer 20 minuten duren voordat gegevens beschikbaar voor Azure Monitor-logboeken zijn.

## <a name="query-logs"></a>Logboeken voor query 's

1. Uit de [Azure-portal](https://portal.azure.com), selecteer uw Log Analytics-werkruimte.

2. In het menu links onder **algemene**, selecteer **logboeken**. Hier vindt u de gegevens die worden verzameld van Kafka. Voer een query in het query-venster en selecteer vervolgens **uitvoeren**. Hier volgen enkele voorbeelden van zoekopdrachten:

* Gebruik van de schijf:

    ```kusto
    Perf 
    | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* CPU-gebruik:

    ```kusto
    Perf 
    | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* Binnenkomende berichten per seconde:

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)
    ```

* Binnenkomende bytes per seconde:

    ```kusto
    metrics_kafka_CL 
    | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

* Uitgaande bytes per seconde:

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

    > [!IMPORTANT]  
    > Vervang de querywaarden met de specifieke gegevens van uw cluster. Bijvoorbeeld, `ClusterName_s` moet worden ingesteld op de naam van uw cluster. `HostName_s` moet worden ingesteld op de domeinnaam van een worker-knooppunt in het cluster.
    
    U kunt ook opgeven `*` om te zoeken naar alle typen in het logboek geregistreerd. De volgende logboeken zijn momenteel beschikbaar voor query's:
    
    | Logboektype | Description |
    | ---- | ---- |
    | log\_kafkaserver\_CL | Kafka-broker server.log |
    | log\_kafkacontroller\_CL | Kafka-broker controller.log |
    | metrics\_kafka\_CL | Kafka JMX-meetwaarden |
    
    ![Afbeelding van het CPU-gebruik zoeken](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Monitor [overzicht van Azure Monitor](../../log-analytics/log-analytics-get-started.md), en [Query Azure Monitor-logboeken voor het controleren van HDInsight-clusters](../hdinsight-hadoop-oms-log-analytics-use-queries.md).

Zie de volgende documenten voor meer informatie over het werken met Apache Kafka:

* [Mirror Apache Kafka tussen clusters met HDInsight](apache-kafka-mirroring.md)
* [Verhoog de schaalbaarheid van Apache Kafka in HDInsight](apache-kafka-scalability.md)
* [Apache Spark streaming (DStreams) met Apache Kafka gebruiken](../hdinsight-apache-spark-with-kafka.md)
* [Apache Spark structured streaming met Apache Kafka gebruiken](../hdinsight-apache-kafka-spark-structured-streaming.md)
