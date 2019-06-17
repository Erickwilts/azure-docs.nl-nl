---
title: Spark gebruiken om te lezen en schrijven van gegevens in HBase - Azure HDInsight
description: De Spark-HBase-Connector gebruiken om te lezen en schrijven van gegevens van een Spark-cluster naar een HBase-cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/06/2019
ms.openlocfilehash: e747f39ca84bb859b37550efef51e01cffd96876
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056748"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Apache Spark gebruiken om Apache HBase-gegevens te lezen en schrijven

Apache HBase is meestal een query uitgevoerd met de laag niveau API (scans, opgehaald en plaatst) of met een SQL-syntaxis met behulp van Apache Phoenix. Apache biedt ook de Apache Spark HBase-Connector dit een handige is en goed presterende alternatieve om te vragen en gegevens die zijn opgeslagen door HBase te wijzigen.

## <a name="prerequisites"></a>Vereisten

* Twee afzonderlijke HDInsight-clusters die zijn geïmplementeerd in hetzelfde virtuele netwerk. Een HBase- en een Spark-met ten minste Spark 2.1 (HDInsight 3.6) geïnstalleerd. Zie voor meer informatie, [maken Linux gebaseerde clusters in HDInsight met behulp van de Azure-portal](hdinsight-hadoop-create-linux-clusters-portal.md).

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

* De [URI-schema](hdinsight-hadoop-linux-information.md#URI-and-scheme) voor uw clusters primaire opslag. Zou dit wasb: / / voor Azure Blob Storage, abfs: / / voor Azure Data Lake Storage Gen2 of adl: / / voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Blob Storage of Data Lake Storage Gen2, zou de URI wasbs: / / of abfss: / /, respectievelijk Zie ook [veilige overdracht](../storage/common/storage-require-secure-transfer.md).

## <a name="overall-process"></a>Algehele proces

Het proces op hoog niveau voor het inschakelen van uw Spark-cluster query uitvoeren op uw HDInsight-cluster is als volgt:

1. Voorbeeldgegevens in HBase maken.
2. De hbase-site.xml-bestand van de configuratiemap voor HBase-cluster (/ etc/hbase/conf) aan te schaffen.
3. Plaats een kopie van hbase-site.xml in de map van uw Spark-2-configuratie (/ etc/spark2/conf).
4. Voer `spark-shell` verwijst naar de HBase-Connector voor Spark, door de Maven-coördinaten het `packages` optie.
5. Definieer een catalogus die het schema van Spark voor HBase-kaarten.
6. Interactie met de HBase-gegevens met behulp van de RDD of DataFrame APIs.

## <a name="prepare-sample-data-in-apache-hbase"></a>Voorbeeldgegevens in Apache HBase voorbereiden

In deze stap maakt u maken en vullen van een tabel in Apache HBase die u kunt vervolgens een query met behulp van Spark.

1. Gebruik de `ssh` opdracht verbinding maken met uw HBase-cluster. De onderstaande opdracht bewerken door te vervangen `HBASECLUSTER` met de naam van uw HBase-cluster en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. Gebruik de `hbase shell` opdracht om te beginnen de interactieve HBase-shell. Voer de volgende opdracht in uw SSH-verbinding:

    ```bash
    hbase shell
    ```

3. Gebruik de `create` opdracht voor het maken van een HBase-tabel met twee kolomfamilies. Voer de volgende opdracht in:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Gebruik de `put` opdracht voor het invoegen van waarden op een opgegeven kolom in een opgegeven rij in een bepaalde tabel. Voer de volgende opdracht in:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
    put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'
    ```

5. Gebruik de `exit` opdracht om te stoppen van de interactieve HBase-shell. Voer de volgende opdracht in:

    ```hbase
    exit
    ```

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>Hbase-site.xml kopiëren naar Spark-cluster

Kopieer de hbase-site.xml van lokale opslag naar de hoofdmap van de standaardopslag voor uw Spark-cluster.  Bewerk de volgende opdracht in overeenstemming met uw configuratie.  Voer vervolgens de opdracht uit uw open SSH-sessie met de HBase-cluster:

| Van de syntaxiswaarde | Nieuwe waarde|
|---|---|
|[URI-schema](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Wijzigen om uw opslag weer te geven.  De volgende syntaxis is voor blob-opslag met veilige overdracht.|
|`SPARK_STORAGE_CONTAINER`|Vervangen door de containernaam van de standaard opslag gebruikt voor het Spark-cluster.|
|`SPARK_STORAGE_ACCOUNT`|Vervangen door de naam van het standaardopslagaccount gebruikt voor het Spark-cluster.|

```bash
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

Sluit de ssh-verbinding met uw HBase-cluster.

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Hbase-site.xml plaatsen op uw Spark-cluster

1. Verbinding maken met het hoofdknooppunt van uw Spark-cluster via SSH.

2. Voer de volgende opdracht om te kopiëren `hbase-site.xml` uit uw Spark-cluster standaardopslag naar de map Spark 2-configuratie op de lokale opslag van het cluster:

    ```bash
    sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf
    ```

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Uitvoeren van Spark-Shell die verwijst naar de Spark-Connector voor HBase

1. Voer de onderstaande opdracht om te beginnen een spark-shell uit uw open SSH-sessie met de Spark-cluster:

    ```bash
    spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/
    ```  

2. Houd dit exemplaar van de Spark-Shell geopend en Ga door met de volgende stap.

## <a name="define-a-catalog-and-query"></a>Een catalogus en een Query definiëren

In deze stap definieert u een catalogusobject dat het schema van Apache Spark voor Apache HBase-kaarten.  

1. In uw open Spark-Shell, voert u de volgende `import` instructies:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

2. Voer de volgende opdracht voor het definiëren van een catalogus voor de tabel met contacten dat u hebt gemaakt in HBase:

    ```scala
    def catalog = s"""{
        |"table":{"namespace":"default", "name":"Contacts"},
        |"rowkey":"key",
        |"columns":{
        |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
        |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
        |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
        |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
        |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
        |}
    |}""".stripMargin
    ```

    De code voert het volgende:  

     a. Een catalogusschema voor de HBase-tabel met de naam definiëren `Contacts`.  
     b. Identificeren van de rowkey als `key`, en de namen van de kolommen die worden gebruikt in Spark kolomfamilie, kolomnaam en het kolomtype, zoals de term in HBase toewijzen.  
     c. De rowkey heeft ook worden gedefinieerd in detail als een benoemde kolom (`rowkey`), die is voorzien van een specifieke kolom-familie `cf` van `rowkey`.  

3. Voer de volgende opdracht voor het definiëren van een methode waarmee een DataFrame rond uw `Contacts` tabel in HBase:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

4. Maak een instantie van het gegevensframe:

    ```scala
    val df = withCatalog(catalog)
    ```  

5. Query van het gegevensframe:

    ```scala
    df.show()
    ```

6. Hier ziet u twee rijen met gegevens:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Een tijdelijke tabel registreren, zodat u kunt de HBase-tabel met behulp van Spark SQL query:

    ```scala
    df.createTempView("contacts")
    ```

8. Een SQL-query op basis van de `contacts` tabel:

    ```scala
    val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
    query.show()
    ```

9. Hier ziet u de resultaten zoals deze:

    ```output
    +-------------+--------------------+
    | personalName|       officeAddress|
    +-------------+--------------------+
    |    John Dole|1111 San Gabriel Dr.|
    |  Calvin Raji|5415 San Gabriel Dr.|
    +-------------+--------------------+
    ```

## <a name="insert-new-data"></a>Nieuwe gegevens invoegen

1. Voor het invoegen van een nieuwe record van de contactpersoon, definieert een `ContactRecord` klasse:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

2. Maak een instantie van `ContactRecord` en plaatst deze in een matrix:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

3. De matrix van nieuwe gegevens in HBase opslaan:

    ```scala
    sc.parallelize(newData).toDF.write.options(Map(HBaseTableCatalog.tableCatalog -> catalog, HBaseTableCatalog.newTable -> "5")).format("org.apache.spark.sql.execution.datasources.hbase").save()
    ```

4. Bekijk de resultaten:

    ```scala  
    df.show()
    ```

5. De uitvoer ziet er als volgt uit:

    ```output
    +------+--------------------+--------------+------------+--------------+
    |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
    +------+--------------------+--------------+------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
    | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
    |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+------------+--------------+
    ```

6. Sluit de spark-shell met de volgende opdracht:

    ```scala
    :q
    ```

## <a name="next-steps"></a>Volgende stappen

* [Apache Spark HBase Connector](https://github.com/hortonworks-spark/shc)
