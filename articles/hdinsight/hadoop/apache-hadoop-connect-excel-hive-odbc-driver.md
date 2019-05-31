---
title: Excel verbinden met Apache Hadoop met het Hive ODBC-stuurprogramma - Azure HDInsight
description: Informatie over het instellen en het Microsoft Hive ODBC-stuurprogramma voor Excel gebruiken om gegevens te doorzoeken in HDInsight-clusters uit Microsoft Excel.
keywords: hadoop excel-, hive-excel-, hive odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: hrasheed
ms.openlocfilehash: fcb9171d2285efab0f65e6ab424908bc42c0ea2f
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391879"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Excel verbinden met Apache Hadoop in Azure HDInsight met het Microsoft Hive ODBC-stuurprogramma

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft Business Intelligence (BI) onderdelen van Microsoft Big Data-oplossing geïntegreerd met Apache Hadoop-clusters die zijn geïmplementeerd in Azure HDInsight. Een voorbeeld van deze integratie is de mogelijkheid Excel verbindt met het Hive-datawarehouse van een Hadoop-cluster in HDInsight met behulp van het stuurprogramma Microsoft Hive Open Database Connectivity (ODBC).

Het is ook mogelijk om de gegevens die zijn gekoppeld aan een HDInsight-cluster en andere gegevensbronnen, met inbegrip van andere (niet-HDInsight) Hadoop-clusters uit Excel met de Microsoft Power Query-invoegtoepassing voor Excel te verbinden. Zie voor meer informatie over het installeren en met behulp van Power Query [Excel verbinding maken met HDInsight met Power Query](../hdinsight-connect-excel-power-query.md).

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel, hebt u de volgende items:

* Een HDInsight Hadoop-cluster. Zie voor het maken van een [aan de slag met Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md).
* Een werkstation met Office 2010 Professional Plus of hoger, of Excel 2010 of hoger.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC-stuurprogramma installeren
Download en installeer de [stuurprogramma Microsoft Hive ODBC](https://go.microsoft.com/fwlink/?LinkID=286698) versie die overeenkomt met de versie van de toepassing waar u het ODBC-stuurprogramma wordt gebruikt.  Voor deze zelfstudie wordt het stuurprogramma voor Office Excel gebruikt.

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC-gegevensbron maken
De volgende stappen laten zien hoe u een Hive ODBC-gegevensbron maken.

1. Vanuit Windows, navigeert u naar Start > Windows-beheerprogramma's > ODBC-gegevensbronnen (32-bit)/(64-bit).  Hiermee opent u de **ODBC-gegevensbronbeheer** venster.

    ![OBDC gegevensbronbeheer](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "configureren met behulp van ODBC-gegevensbronbeheer DSN")

1. Uit de **gebruiker DSN** tabblad **toevoegen** openen de **nieuwe gegevensbron maken** venster.

1. Selecteer **stuurprogramma Microsoft Hive ODBC**, en selecteer vervolgens **voltooien** openen de **Microsoft Hive ODBC-stuurprogramma DSN Setup** venster.

1. Typ of selecteer de volgende waarden:

   | Eigenschap | Description |
   | --- | --- |
   |  Naam van de gegevensbron |Geef uw gegevensbron een naam |
   |  Host (s) |Voer `HDInsightClusterName.azurehdinsight.net` in. Bijvoorbeeld: `myHDICluster.azurehdinsight.net` |
   |  Poort |Gebruik **443**. (Deze poort is gewijzigd van 563 in 443.) |
   |  Database |Gebruik **standaard**. |
   |  Mechanisme |Selecteer **Windows Azure HDInsight Service** |
   |  Gebruikersnaam |HDInsight-cluster HTTP-gebruikersnaam invoeren. De standaardgebruikersnaam **admin**. |
   |  Wachtwoord |Voer gebruikerswachtwoord van HDInsight-cluster. Schakel het selectievakje **wachtwoord opslaan (versleutelde)** .|

1. Optioneel: Selecteer **geavanceerde opties...**  

   | Parameter | Description |
   | --- | --- |
   |  Systeemeigen Query gebruiken |Wanneer deze optie is geselecteerd, probeert het ODBC-stuurprogramma niet TSQL converteren naar HiveQL. U dient deze alleen als u 100% zeker dat u verzendt pure HiveQL-instructies gebruiken. Bij het verbinden met SQL Server of Azure SQL Database, laat u het vakje uitgeschakeld. |
   |  Rijen per blok wordt opgehaald |Tijdens het ophalen van een groot aantal records, kan afstemmen van deze parameter worden vereist om ervoor te zorgen van optimale prestaties. |
   |  Standaard kolomlengte tekenreeks, lengte van de binaire kolom, decimaal kolomschaal |De lengte van het gegevenstype en Precision-systemen kunnen beïnvloeden hoe gegevens worden geretourneerd. Ze ervoor zorgen dat de onjuiste gegevens worden geretourneerd vanwege verlies van precisie-en/of moet worden afgekapt. |

    ![Geavanceerde opties](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN geavanceerde configuratieopties")

1. Selecteer **testen** voor het testen van de gegevensbron. Wanneer de gegevensbron correct is geconfigureerd, ziet u de testresultaten **SUCCES!** .  

1. Selecteer **OK** om de Test-venster te sluiten.  

1. Selecteer **OK** sluiten de **Microsoft Hive ODBC-stuurprogramma DSN Setup** venster.  

1. Selecteer **OK** sluiten de **ODBC-gegevensbronbeheer** venster.  

## <a name="import-data-into-excel-from-hdinsight"></a>Gegevens in Excel importeren vanuit HDInsight

De volgende stappen beschrijven de manier waarop gegevens importeren uit een Hive-tabel in een Excel-werkmap met behulp van de ODBC-gegevensbron die u in de vorige sectie hebt gemaakt.

1. Open een nieuwe of bestaande werkmap in Excel.

2. Uit de **gegevens** tabblad, gaat u naar **gegevens ophalen** > **van andere bronnen** > **uit ODBC** starten de **Uit ODBC** venster.

    ![Wizard Gegevensverbinding Open](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Open wizard Gegevensverbinding")

3. Selecteer de naam van de gegevensbron die u hebt gemaakt in de laatste sectie van de vervolgkeuzelijst en selecteer vervolgens **OK**.

4. Voor het eerste gebruik, een **ODBC-stuurprogramma** dialoogvenster wordt geopend. Selecteer **Windows** in het menu links. Selecteer vervolgens **Connect** openen de **Navigator** venster.

5. Van **Navigator**, gaat u naar **HIVE** > **standaard** > **hivesampletable**, en selecteer vervolgens  **Load**. Het duurt een paar minuten voordat de gegevens naar Excel worden geïmporteerd.

    ![HDInsight Hive ODBC-navigator](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "Open wizard Gegevensverbinding")

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u gegevens ophaalt uit de HDInsight-Service in Excel met het Microsoft Hive ODBC-stuurprogramma. Op deze manier kunt u gegevens ophalen uit de HDInsight-Service in SQL-Database. Het is ook mogelijk om gegevens te uploaden naar een HDInsight-Service. Voor meer informatie zie:

* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met Apache Hadoop met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).