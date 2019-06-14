---
title: Gegevens verkennen in Hive-tabellen met Hive-query's - Team Data Science Process
description: Gebruik de voorbeeld-Hive-scripts die worden gebruikt om gegevens in Hive-tabellen in een HDInsight Hadoop-cluster te verkennen.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c9761ade493641b6445cf151424f9598ab25e384
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303637"
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Gegevens in Hive-tabellen verkennen met Hive-query's

Dit artikel bevat een voorbeeld van Hive-scripts die worden gebruikt om gegevens in Hive-tabellen in een HDInsight Hadoop-cluster te verkennen.

Deze taak is een stap in de [Team Data Science Process](overview.md).

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [maken van een Azure Storage-account](../../storage/common/storage-quickstart-create-account.md)
* Een aangepaste Hadoop-cluster met de HDInsight-service wordt ingericht. Als u instructies nodig hebt, raadpleegt u [aanpassen Azure HDInsight Hadoop-Clusters voor Advanced Analytics](customize-hadoop-cluster.md).
* De gegevens is geüpload naar de Hive-tabellen in Azure HDInsight Hadoop-clusters. Als dit niet het geval is, volgt u de instructies in [maken en gegevens laden met Hive-tabellen](move-hive-tables.md) het uploaden van gegevens met Hive-tabellen eerst.
* Externe toegang tot het cluster ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot de hoofd-knooppunt van Hadoop Cluster](customize-hadoop-cluster.md).
* Als u instructies voor het indienen van Hive-query's nodig hebt, raadpleegt u [hoe u Hive-query's verzenden](move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Voorbeeld van de Hive-query scripts voor gegevens verkennen
1. Het aantal opmerkingen per partitie ophalen  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Het aantal opmerkingen per dag  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. De niveaus in een categorische kolom ophalen  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. De verdeling van de numerieke kolommen ophalen  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Records ophalen uit het koppelen van twee tabellen
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Aanvullende query-scripts voor scenario's reis voor gegevens over taxi 's
Voorbeelden van query's die specifiek zijn voor [NYC Taxi reisgegevens](https://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar [GitHub-opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al hebben gegevensschema opgegeven en kunnen worden verzonden om uit te voeren.

