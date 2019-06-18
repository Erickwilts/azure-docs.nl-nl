---
title: Wat is Apache Hive en HiveQL - Azure HDInsight
description: Apache Hive is een datawarehouse-systeem voor Apache Hadoop. U kunt gegevens die zijn opgeslagen in Hive HiveQL, met een query die vergelijkbaar met de Transact-SQL. In dit document leert u hoe u Hive en HiveQL gebruiken met Azure HDInsight.
keywords: hiveql, wat hive, hadoop hiveql, is het gebruik van hive, hive, wat is er hive meer
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/06/2019
ms.openlocfilehash: ea8f14a7013a937ddd77baf0f50b8dca09cabad6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076315"
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Wat is Apache Hive en HiveQL op Azure HDInsight?

[Apache Hive](https://hive.apache.org/) is van een datawarehouse-systeem voor Apache Hadoop. Hive kunt gegevenssamenvatting, query's en analyse van gegevens. Hive-query's worden geschreven in HiveQL, dit een querytaal die vergelijkbaar is met SQL is.

Hive kunt u de projectstructuur van het op grotendeels niet-gestructureerde gegevens. Nadat u de structuur te definiëren, kunt u HiveQL gebruiken om op te vragen van de gegevens zonder kennis van Java- of MapReduce.

HDInsight biedt verschillende clustertypen die zijn afgesteld voor specifieke werkbelastingen. De volgende clustertypen worden meestal gebruikt voor Hive-query's:

* __Interactive Query__: Een Hadoop-cluster waarmee [met lage latentie analytische verwerking (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionaliteit voor de reactietijd voor interactieve query's verbeteren. Zie voor meer informatie de [beginnen met Interactive Query in HDInsight](../interactive-query/apache-interactive-query-get-started.md) document.

* __Hadoop__: Een Hadoop-cluster die is afgestemd op werkbelastingen voor batchverwerking. Zie voor meer informatie de [beginnen met Apache Hadoop in HDInsight](../hadoop/apache-hadoop-linux-tutorial-get-started.md) document.

* __Spark__: Apache Spark heeft ingebouwde functionaliteit voor het werken met Hive. Zie voor meer informatie de [beginnen met Apache Spark in HDInsight](../spark/apache-spark-jupyter-spark-sql.md) document.

* __HBase__: HiveQL kan worden gebruikt om gegevens te doorzoeken die zijn opgeslagen in Apache HBase. Zie voor meer informatie de [beginnen met Apache HBase op HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md) document.

## <a name="how-to-use-hive"></a>Het gebruik van Hive

Gebruik de volgende tabel voor het detecteren van de verschillende manieren voor het gebruik van Hive met HDInsight:

| **Gebruik deze methode** als u wilt... | ... **interactieve** query's | ...**batch** verwerken | ...from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [HDInsight tools voor Visual Studio Code](../hdinsight-for-vscode.md) |✔ |✔ | Linux, Unix, Mac OS X of Windows |
| [HDInsight tools voor Visual Studio](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Windows |
| [Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Elke (op basis van browser) |
| [Beeline client](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux, Unix, Mac OS X of Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux, Unix, Mac OS X of Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Windows |


## <a name="hiveql-language-reference"></a>Naslaggids voor HiveQL

Naslaggids voor HiveQL is beschikbaar in de [taal handmatig](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Structuur van hive en gegevens

Hive begrijpt hoe u werkt met gestructureerd en semi-gestructureerde gegevens. Bijvoorbeeld, tekstbestanden waarin de velden worden gescheiden door een specifieke tekens. De volgende HiveQL-instructie maakt een tabel van gegevens door spaties gescheiden:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive biedt ook ondersteuning voor aangepaste **serializer/deserializers (serde-schrijfbewerking)** voor complexe of onregelmatig gestructureerde gegevens. Zie voor meer informatie de [over het gebruik van een aangepaste JSON serde-schrijfbewerking met HDInsight](https://web.archive.org/web/20190217104719/ https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/) document.

Zie voor meer informatie over ondersteunde bestandsindelingen in Hive, de [handmatige (taal https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

### <a name="hive-internal-tables-vs-external-tables"></a>Hive-interne tabellen vs externe tabellen

Er zijn twee soorten tabellen die u met Hive maken kunt:

* __Interne__: Gegevens worden opgeslagen in de Hive-datawarehouse. Het datawarehouse bevindt zich in `/hive/warehouse/` op de standaardopslag voor het cluster.

    Interne tabellen gebruiken als een van de volgende voorwaarden van toepassing:

    * Gegevens is tijdelijk.
    * Wilt u Hive voor het beheren van de levenscyclus van de tabel en de gegevens.

* __Externe__: Gegevens worden opgeslagen buiten het datawarehouse. De gegevens kunnen worden opgeslagen op opslag die toegankelijk is door het cluster.

    Externe tabellen gebruiken als een van de volgende voorwaarden van toepassing:

    * De gegevens wordt ook buiten Hive gebruikt. Bijvoorbeeld, worden de bestanden bijgewerkt door een ander proces (die wordt niet vergrendeld de bestanden.)
    * Gegevens moeten blijven in de onderliggende locatie, zelfs na het verwijderen van de tabel.
    * U moet een aangepaste locatie, zoals een niet-standaard-storage-account.
    * Een ander programma dan hive beheert de gegevensindeling, locatie, enzovoort.

Zie voor meer informatie de [Hive interne en externe tabellen Intro](https://blogs.msdn.microsoft.com/cindygross/2013/02/05/hdinsight-hive-internal-and-external-tables-intro/) blogbericht.

## <a name="user-defined-functions-udf"></a>Gebruiker gedefinieerde functies (UDF's)

Hive kan verder worden uitgebreid via **gebruikersgedefinieerde functies (UDF's)** . Een UDF kunt u functionaliteit of logica die is niet eenvoudig gemodelleerd in HiveQL implementeren. Zie de volgende documenten voor een voorbeeld van het gebruik van UDF's met Hive:

* [Een Java-gebruiker gedefinieerde functie gebruiken met Apache Hive](../hadoop/apache-hadoop-hive-java-udf.md)

* [Een gebruiker gedefinieerde functie voor Python gebruiken met Apache Hive](../hadoop/python-udf-hdinsight.md)

* [Gebruik een C# door de gebruiker gedefinieerde functie met Apache Hive](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Een aangepaste Apache Hive-de gebruiker gedefinieerde functie toevoegen aan HDInsight](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Een voorbeeld van de gebruiker gedefinieerde functie Apache Hive datum/tijd-indeling converteren naar Hive timestamp](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Voorbeeldgegevens

Hive in HDInsight wordt vooraf geladen geleverd met een interne tabel met de naam `hivesampletable`. HDInsight biedt ook een voorbeeld van de gegevenssets die kunnen worden gebruikt met Hive. Deze gegevenssets worden opgeslagen in de `/example/data` en `/HdiSamples` mappen. Deze mappen bestaan in de standaardopslag voor uw cluster.

## <a id="job"></a>Voorbeeld van de Hive-query

De volgende HiveQL-instructies project kolommen naar de `/example/data/sample.log` bestand:

```hiveql
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
    WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
    GROUP BY t4;
```

In het vorige voorbeeld uitvoeren de HiveQL-instructies de volgende acties:


* `DROP TABLE`: Als de tabel al bestaat, verwijderen.

* `CREATE EXTERNAL TABLE`: Maakt u een nieuwe **externe** tabel in Hive. Externe tabellen worden alleen de definitie van de tabel opslaan in Hive. De gegevens blijft in de oorspronkelijke locatie en in de oorspronkelijke indeling.

* `ROW FORMAT`: Geeft aan hoe de gegevens zijn opgemaakt Hive. In dit geval worden de velden in elk logboek gescheiden door een spatie.

* `STORED AS TEXTFILE LOCATION`: Geeft aan waar de gegevens worden opgeslagen Hive (de `example/data` directory) en dat deze wordt opgeslagen als tekst. De gegevens kunnen worden in één bestand of verdeeld over meerdere bestanden in de map.

* `SELECT`: Selecteert een telling van alle rijen waarin de kolom **t4** bevat de waarde **[fout]** . Deze instructie retourneert een waarde van **3** omdat er drie rijen die deze waarde bevatten.

* `INPUT__FILE__NAME LIKE '%.log'` -Hive probeert het schema toepassen op alle bestanden in de map. In dit geval bevat de map bestanden die niet overeenkomen met het schema. Om te voorkomen dat garbagecollection-gegevens in de resultaten, deze instructie vertelt Hive dat we alleen gegevens uit bestanden hebben die eindigt op moet retourneren. log.

> [!NOTE]  
> Externe tabellen moeten worden gebruikt wanneer u verwacht dat de onderliggende gegevens worden bijgewerkt door een externe bron. Bijvoorbeeld, een geautomatiseerd proces of MapReduce-bewerking uploaden.
>
> Verwijderen van een externe tabel heeft **niet** de gegevens verwijderen, verwijdert alleen de tabeldefinitie.

Maakt een **interne** in plaats van de externe tabel, gebruikt u de volgende HiveQL:

```hiveql
CREATE TABLE IF NOT EXISTS errorLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
STORED AS ORC;
INSERT OVERWRITE TABLE errorLogs
SELECT t1, t2, t3, t4, t5, t6, t7 
    FROM log4jLogs WHERE t4 = '[ERROR]';
```

Deze instructies uit uitvoeren de volgende acties:

* `CREATE TABLE IF NOT EXISTS`: Als de tabel niet bestaat, maakt u dit. Omdat de **externe** trefwoord wordt niet gebruikt, wordt deze instructie maakt u een interne tabel. De tabel wordt opgeslagen in het datawarehouse Hive en volledig wordt beheerd door Hive.

* `STORED AS ORC`: De gegevens worden opgeslagen in geoptimaliseerd rij kolommen (ORC)-indeling. ORC is een uiterst geoptimaliseerde en efficiënte indeling voor het opslaan van gegevens met Hive.

* `INSERT OVERWRITE ... SELECT`: Selecteert de rijen van de **log4jLogs** tabel met **[fout]** , en voegt u de gegevens in de **foutenlogboeken** tabel.

> [!NOTE]  
> In tegenstelling tot externe tabellen verwijdert het verwijderen van een interne tabel u ook de onderliggende gegevens.

## <a name="improve-hive-query-performance"></a>Hive-queryprestaties verbeteren

### <a id="usetez"></a>Apache Tez

[Apache Tez](https://tez.apache.org) is een structuur waarmee gegevensintensieve toepassingen, zoals Hive, veel efficiënter op schaal worden uitgevoerd. Tez is standaard ingeschakeld.  De [Apache Hive op Tez-Ontwerpdocumenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) bevat details over de implementatie-opties en afstemmen configuraties.

### <a name="low-latency-analytical-processing-llap"></a>Low Latency Analytical Processing (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (ook bekend als Live Long and Process) is een nieuwe functie in Hive 2.0 waarmee de caching van query's in het geheugen. LLAP maakt Hive-query's sneller en maximaal [26 x sneller dan Hive 1.x in sommige gevallen](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight biedt LLAP in het clustertype van de interactieve query's. Zie voor meer informatie de [beginnen met Interactive Query](../interactive-query/apache-interactive-query-get-started.md) document.

## <a name="scheduling-hive-queries"></a>Hive-query's plannen

Er zijn diverse services die kunnen worden gebruikt om Hive-query's uitvoeren als onderdeel van een geplande of on-demand-werkstroom.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory kunt u HDInsight gebruiken als onderdeel van een Data Factory-pijplijn. Zie voor meer informatie over het gebruik van Hive in een pijplijn het [gegevens transformeren met behulp van Hive-activiteit in Azure Data Factory](../../data-factory/transform-data-using-hadoop-hive.md) document.

### <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-taken en SQL Server Integration Services

SQL Server Integration Services (SSIS) kunt u een Hive-taak uitvoeren. Het Azure Feature Pack voor SSIS biedt de volgende onderdelen die met Hive-taken in HDInsight werken.

* [Azure HDInsight Hive-taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-hdinsight-hive-task)

* [Azure Subscription Connection Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-subscription-connection-manager)

Zie voor meer informatie de [Azure Feature Pack](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis) documentatie.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie is een werkstroom en coördinatie systeem waarmee Hadoop-taken worden beheerd. Zie voor meer informatie over het gebruik Oozie met Hive de [Apache Oozie gebruiken om te definiëren en het uitvoeren van een werkstroom](../hdinsight-use-oozie-linux-mac.md) document.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd wat Hive is en hoe u deze wilt gebruiken met Hadoop in HDInsight, gebruikt u de volgende koppelingen op andere manieren om te werken met Azure HDInsight.

* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md)
* [Gebruik Python-gebruiker gedefinieerde functies (UDF's) met Apache Hive en Apache Pig in HDInsight](./python-udf-hdinsight.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
