---
title: Problemen met hive oplossen met behulp van Azure HDInsight
description: Krijg antwoorden op veelgestelde vragen over het werken met Apache Hive en Azure HDInsight.
keywords: Azure HDInsight, Hive, veelgestelde vragen, gids voor probleem oplossing, veelgestelde vragen
ms.service: hdinsight
author: dharmeshkakadia
ms.author: dkakadia
ms.topic: conceptual
ms.date: 11/2/2017
ms.openlocfilehash: 91e6803e0a1302a33a3bf176ad84d0b0e0c8c5b6
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875931"
---
# <a name="troubleshoot-apache-hive-by-using-azure-hdinsight"></a>Problemen met Apache Hive oplossen met behulp van Azure HDInsight

Meer informatie over de belangrijkste vragen en hun oplossingen bij het werken met Apache Hive Payloads in Apache Ambari.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hoe kan ik een Hive-metastore exporteren en importeren in een ander cluster?


### <a name="resolution-steps"></a>Oplossingen

1. Verbinding maken met het HDInsight-cluster met behulp van een Secure Shell (SSH)-client. Zie voor meer informatie, [meer lezen](#additional-reading-end).

2. Voer de volgende opdracht uit op het HDInsight-cluster van waaruit u de meta Store wilt exporteren:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

   Met deze opdracht wordt een bestand met de naam allatables. SQL gegenereerd.

3. Kopieer het bestand AllTables. SQL naar het nieuwe HDInsight-cluster en voer de volgende opdracht uit:

   ```apache
   hive -f alltables.sql
   ```

In de code in de oplossings stappen wordt ervan uitgegaan dat de gegevens paden op het nieuwe cluster hetzelfde zijn als de gegevens paden op het oude cluster. Als de gegevens paden verschillen, kunt u het gegenereerde bestand AllTables. SQL hand matig bewerken om eventuele wijzigingen weer te geven.

### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Wilt u Hive-logboeken Hoe kan ik vinden op een cluster?

### <a name="resolution-steps"></a>Oplossingen

1. Maak verbinding met het HDInsight-cluster met behulp van SSH. Zie voor meer informatie, **meer lezen**.

2. Gebruik de volgende opdracht om Hive client-logboeken weer te geven:

   ```apache
   /tmp/<username>/hive.log 
   ```

3. Als u Hive-metastore logboeken wilt weer geven, gebruikt u de volgende opdracht:

   ```apache
   /var/log/hive/hivemetastore.log 
   ```

4. Als u Hiveserver-logboeken wilt weer geven, gebruikt u de volgende opdracht:

   ```apache
   /var/log/hive/hiveserver2.log 
   ```

### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a>Hoe kan ik de Hive-shell starten met specifieke configuraties in een cluster?

### <a name="resolution-steps"></a>Oplossingen

1. Geef een sleutel-waardepaar voor de configuratie op wanneer u de Hive-shell start. Zie voor meer informatie, [meer lezen](#additional-reading-end).

   ```apache
   hive -hiveconf a=b 
   ```

2. Gebruik de volgende opdracht om alle efficiënte configuraties op Hive-shell weer te geven:

   ```apache
   hive> set;
   ```

   Gebruik bijvoorbeeld de volgende opdracht om Hive-shell te starten met logboek registratie voor fout opsporing in te scha kelen op de-console:

   ```apache
   hive -hiveconf hive.root.logger=ALL,console 
   ```

### <a name="additional-reading"></a>Meer lezen

- [Eigenschappen van Hive-configuratie](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hoe kan ik Apache TEZ DAG-gegevens op een cluster kritiek pad analyseren?


### <a name="resolution-steps"></a>Oplossingen
 
1. Als u een Apache TEZ-Directed Acyclic Graph (DAG) wilt analyseren op een cluster-essentiële grafiek, maakt u verbinding met het HDInsight-cluster met behulp van SSH. Zie voor meer informatie, [meer lezen](#additional-reading-end).

2. Voer de volgende opdracht uit op een opdrachtprompt:
   
   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
   ```

3. Als u andere analyse functies die kunnen worden gebruikt voor het analyseren van TEZ DAG, wilt weer geven, gebruikt u de volgende opdracht:

   ```apache
   hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
   ```

   U moet een voorbeeld programma als eerste argument opgeven.

   Geldige programma namen zijn:
    - **ContainerReuseAnalyzer**: Details van de printer container opnieuw gebruiken in een DAG
    - **CriticalPath**: Het kritieke pad van een DAG zoeken
    - **LocalityAnalyzer**: Details van de lokale plaats in een DAG afdrukken
    - **ShuffleTimeAnalyzer**: De details van de wille keurige tijd in een DAG analyseren
    - **SkewAnalyzer**: De scheefheid Details in een DAG analyseren
    - **SlowNodeAnalyzer**: Details van het knoop punt in een DAG afdrukken
    - **SlowTaskIdentifier**: Trage taak Details in een DAG afdrukken
    - **SlowestVertexAnalyzer**: Langzaam hoekpunt Details in een DAG afdrukken
    - **SpillAnalyzer**: Details over overloop afdrukken in een DAG
    - **TaskConcurrencyAnalyzer**: De details van de taak gelijktijdigheid afdrukken in een DAG
    - **VertexLevelCriticalPathAnalyzer**: Het kritieke pad op het hoek punt op het niveau van een DAG zoeken


### <a name="additional-reading"></a>Meer lezen

- [Verbinding maken met een HDInsight-cluster met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>TEZ DAG gegevens van een cluster Hoe kan ik downloaden?


#### <a name="resolution-steps"></a>Oplossingen

Er zijn twee manieren om de TEZ DAG-gegevens te verzamelen:

- Vanaf de opdrachtregel:
 
    Maak verbinding met het HDInsight-cluster met behulp van SSH. Voer bij de opdracht prompt de volgende opdracht uit:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Gebruik de weer gave Ambari TEZ:
   
  1. Ga naar Ambari. 
  2. Ga naar de weer gave TEZ (onder het pictogram tegels in de rechter bovenhoek). 
  3. Selecteer de DAG die u wilt weer geven.
  4. Selecteer **gegevens downloaden**.

### <a name="additional-reading-end"></a>Meer lezen

[Verbinding maken met een HDInsight-cluster met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


### <a name="see-also"></a>Zie ook
[Problemen oplossen met behulp van Azure HDInsight](hdinsight-troubleshoot-guide.md)




