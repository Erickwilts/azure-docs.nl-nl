---
title: Apache Ambari-weergaven gebruiken om te werken met Hive in HDInsight (Apache Hadoop) - Azure
description: Leer hoe u de Hive-weergave in uw webbrowser gebruiken voor het indienen van Hive-query's. Het Hive-weergave maakt deel uit van de Ambari-Webgebruikersinterface voorzien van uw Linux gebaseerde HDInsight-cluster.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: hrasheed
ms.openlocfilehash: 55f8f453faf35d52c5c292e6b309194443980466
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64719579"
---
# <a name="use-apache-ambari-hive-view-with-apache-hadoop-in-hdinsight"></a>Apache Ambari Hive-weergave gebruiken met Apache Hadoop in HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Leer hoe u Hive-query's uitvoert met behulp van Apache Ambari Hive-weergave. De weergave Hive kunt u ontwerpen, optimaliseren en Hive-query's uitvoeren vanuit de webbrowser.

## <a name="prerequisites"></a>Vereisten

* Een Hadoop-cluster op HDInsight. Zie [aan de slag met HDInsight op Linux](./apache-hadoop-linux-tutorial-get-started.md).
* Een webbrowser

## <a name="run-a-hive-query"></a>Een Hive-query uitvoeren

1. Uit de [Azure-portal](https://portal.azure.com/), selecteert u uw cluster.  Zie [clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#showClusters) voor instructies. Het cluster wordt geopend in een nieuwe portalblade.

2. Van **Clusterdashboards**, selecteer **Ambari-weergaven**. Wanneer u hierom wordt gevraagd om te verifiëren, de aanmelding bij cluster gebruiken (standaard `admin`) account naam en het wachtwoord die u hebt opgegeven tijdens het maken van het cluster.

3. Selecteer in de lijst met weergaven __Hive-weergave__.

    ![De Hive-weergave geselecteerd](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

    De pagina van de Hive-weergave is vergelijkbaar met de volgende afbeelding:

    ![Afbeelding van het werkblad query voor de Hive-weergave](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

4. Uit de __Query__ tabblad, plak de volgende HiveQL-instructies in het werkblad:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' 
        GROUP BY t4;
    ```

    Deze instructies uit uitvoeren de volgende acties:

   * `DROP TABLE`: Hiermee verwijdert u de tabel en het gegevensbestand als de tabel al bestaat.

   * `CREATE EXTERNAL TABLE`: Maakt een nieuwe 'extern' tabel in Hive.
     Externe tabellen worden alleen de definitie van de tabel in Hive opslaan. De gegevens is achtergebleven in de oorspronkelijke locatie.

   * `ROW FORMAT`: Laat zien hoe de gegevens zijn opgemaakt. In dit geval worden de velden in elk logboek gescheiden door een spatie.

   * `STORED AS TEXTFILE LOCATION`: U ziet waar de gegevens worden opgeslagen en dat deze opgeslagen als tekst.

   * `SELECT`: Selecteert een telling van alle rijen waar kolom t4 de waarde [fout bevat].

   > [!IMPORTANT]  
   > Laat de __Database__ selectie __standaard__. De voorbeelden in dit document gebruikt de standaarddatabase die is opgenomen in HDInsight.

5. Selecteer eerst de query **Execute** onder het werkblad. Hiermee schakelt u de knop oranje en verandert de tekst in **stoppen**.

6. Nadat de query is voltooid, de **resultaten** tabblad geeft de resultaten van de bewerking. De volgende tekst is het resultaat van de query:

        loglevel       count
        [ERROR]        3

    U kunt de **LOG** tabblad om de logboekgegevens die de taak hebt gemaakt.

   > [!TIP]  
   > Downloaden of opslaan van resultaten van de **acties** vervolgkeuzelijst dialoogvenster onder de **resultaten** tabblad.

### <a name="visual-explain"></a>Visueel element wordt uitgelegd

Als u een visualisatie van het queryplan weergeven, selecteert u de **Visual uitgelegd** tabblad onder het werkblad.

De **Visual uitgelegd** weergave van de query kan nuttig zijn bij het begrijpen van de stroom van complexe query's zijn.

### <a name="tez-ui"></a>Tez-gebruikersinterface

Als de Tez UI voor de query weergeven, selecteert u de **Tez UI** tabblad onder het werkblad.

> [!IMPORTANT]  
> Tez wordt niet gebruikt voor het oplossen van alle query's. U kunt veel query's oplossen zonder gebruik van Tez. 

## <a name="view-job-history"></a>Taakgeschiedenis weergeven

De __taken__ tabblad geeft een geschiedenis van Hive-query's.

![Afbeelding van de taakgeschiedenis](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Database-tabellen

U kunt de __tabellen__ tabblad om te werken met tabellen in een Hive-database.

![Afbeelding van het tabblad tabellen](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Opgeslagen query 's

Uit de **Query** tabblad kunt u eventueel query's opslaan. Nadat u een query hebt opgeslagen, kunt u het hergebruik van de __opgeslagen query's__ tabblad.

![Afbeelding van tabblad opgeslagen query 's](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

> [!TIP]  
> Opgeslagen query's worden opgeslagen in standaardopslag voor het cluster. U vindt de opgeslagen query's onder het pad `/user/<username>/hive/scripts`. Deze worden opgeslagen als tekst zonder opmaak `.hql` bestanden.
>
> Als u het cluster verwijdert, maar de opslag blijven, kunt u een hulpprogramma zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) of Data Lake Storage Explorer (van de [Azure Portal](https://portal.azure.com)) om op te halen van de query's.

## <a name="user-defined-functions"></a>De gebruiker gedefinieerde functies

U kunt component uitbreiden door gebruiker gedefinieerde functies (UDF's). Gebruik een UDF-functionaliteit of logica die is niet eenvoudig gemodelleerd in HiveQL implementeren.

Declareer en een set van UDF's opslaan met behulp van de **UDF** tabblad aan de bovenkant van de Hive-weergave. Deze UDF's kunnen worden gebruikt met de **Query-Editor**.

![Afbeelding van tabblad UDF](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Nadat u hebt een UDF toegevoegd aan de weergave Hive, een **UDF's invoegen** knop wordt weergegeven aan de onderkant van de **Query-Editor**. Dit item selecteren, wordt een vervolgkeuzelijst van het UDF's gedefinieerd in de weergave Hive weergegeven. Selecteren van een UDF, voegt HiveQL-instructies toe aan uw query waarmee de UDF.

Bijvoorbeeld, als u hebt een UDF met de volgende eigenschappen gedefinieerd:

* Resourcenaam: myudfs

* Resourcepad: /myudfs.jar

* De naam van de UDF: myawesomeudf

* De naam van de UDF-klasse: com.myudfs.Awesome

Met behulp van de **UDF's invoegen** knop wordt een vermelding die met de naam **myudfs**, met een andere vervolgkeuzelijst voor elke UDF gedefinieerd voor die bron. In dit geval is het **myawesomeudf**. Dit item selecteren, worden de volgende toegevoegd aan het begin van de query:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

U kunt vervolgens de UDF gebruiken in uw query. Bijvoorbeeld `SELECT myawesomeudf(name) FROM people;`.

Zie de volgende artikelen voor meer informatie over het gebruik van UDF's met Hive in HDInsight:

* [Python gebruiken met Apache Hive en Apache Pig in HDInsight](python-udf-hdinsight.md)
* [Een aangepaste Apache Hive-UDF toevoegen aan HDInsight](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive-instellingen

U kunt verschillende Hive-instellingen, zoals het wijzigen van de engine voor het uitvoeren voor Hive van Tez (de standaardinstelling) naar MapReduce wijzigen.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)
