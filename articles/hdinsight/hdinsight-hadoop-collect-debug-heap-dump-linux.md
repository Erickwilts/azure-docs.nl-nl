---
title: Heapdumps voor Apache Hadoop-services op HDInsight - Azure inschakelen
description: Heapdumps voor Apache Hadoop-services van HDInsight op basis van Linux-clusters voor foutopsporing en analyse inschakelen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 62c9dcc039c68b0b6c8b8bf29ed9f13f88936723
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059505"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Heapdumps voor Apache Hadoop-services op Linux gebaseerde HDInsight inschakelen

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps bevatten een momentopname van het geheugen van de toepassing, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Ze zijn daarom nuttig voor het oplossen van problemen die tijdens de uitvoering optreden.

## <a name="whichServices"></a>Services

Heapdumps voor de volgende services, kunt u inschakelen:

* **Apache hcatalog** -tempelton
* **Apache hive** -hiveserver2, metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **Apache yarn** -resourcemanager, nodemanager, timelineserver
* **Apache hdfs** -datanode, secondarynamenode, namenode

U kunt ook heapdumps voor de kaart inschakelen en verminder processen die worden uitgevoerd door HDInsight.

## <a name="configuration"></a>Understanding heap dump configuratie

Heapdumps zijn ingeschakeld door opties (soms ook wel bekend als kan worden gebruikt, of parameters) naar de JVM wanneer een service wordt gestart. Voor de meeste [Apache Hadoop](https://hadoop.apache.org/) services, kunt u de shell-script gebruikt voor het starten van de service voor het doorgeven van deze opties wijzigen.

In elk script, er is een export voor  **\* \_OPTS**, die de opties die zijn doorgegeven aan de JVM bevat. Bijvoorbeeld, in de **hadoop-env.sh** script, de regel die begint met `export HADOOP_NAMENODE_OPTS=` bevat de opties voor de service NameNode.

Worden toegewezen en verminder processen zijn iets anders, omdat deze bewerkingen een onderliggend proces van de MapReduce-service zijn. Elk toewijzen of verminder het proces wordt uitgevoerd in een container onderliggend en er zijn twee vermeldingen die de JVM-opties bevatten. Beide die deel uitmaken van **mapred site.xml**:

* **mapreduce.admin.map.child.java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> Wordt u aangeraden [Apache Ambari](https://ambari.apache.org/) voor het wijzigen van de scripts en de instellingen voor mapred-site.xml, als de Ambari verwerken repliceren van wijzigingen op knooppunten in het cluster. Zie de [met behulp van Apache Ambari](#using-apache-ambari) sectie voor specifieke stappen.

### <a name="enable-heap-dumps"></a>Heapdumps inschakelen

De volgende optie kunt heapdumps wanneer er een OutOfMemoryError optreedt:

    -XX:+HeapDumpOnOutOfMemoryError

De **+** geeft aan dat deze optie is ingeschakeld. Uitgeschakeld is de standaardinstelling.

> [!WARNING]  
> Heapdumps zijn niet ingeschakeld voor Hadoop-services op HDInsight standaard, zoals de dumpbestanden kunnen erg groot zijn. Als u deze voor het oplossen van inschakelt, moet u deze uitschakelen nadat u hebt gereproduceerd van het probleem en de bestanden die worden verzameld.

### <a name="dump-location"></a>Locatie van de dump

De standaardlocatie voor het dumpbestand wordt de huidige werkmap. U kunt bepalen waar het bestand is opgeslagen met behulp van de volgende optie:

    -XX:HeapDumpPath=/path

Bijvoorbeeld, met behulp van `-XX:HeapDumpPath=/tmp` zorgt ervoor dat de dumpbestanden voor foutopsporing worden opgeslagen in de map/TMP-map.

### <a name="scripts"></a>Scripts

U kunt ook een script activeren wanneer een **OutOfMemoryError** optreedt. Bijvoorbeeld, een melding wordt geactiveerd, zodat u weet dat de fout is opgetreden. Gebruik de volgende optie voor het activeren van een script op een __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]  
> Aangezien Apache Hadoop een gedistribueerd systeem is, moet een script dat wordt gebruikt op alle knooppunten in het cluster dat de service wordt uitgevoerd op worden geplaatst.
> 
> Het script moet ook worden op een locatie die toegankelijk is via het account dat de service wordt uitgevoerd als en moet machtigingen voor uitvoeren. Bijvoorbeeld, u kunt desgewenst voor het opslaan van scripts in `/usr/local/bin` en gebruik `chmod go+rx /usr/local/bin/filename.sh` verleent u lees-en machtigingen voor uitvoeren.

## <a name="using-apache-ambari"></a>Met behulp van Apache Ambari

Als u wilt wijzigen van de configuratie van een service, gebruikt u de volgende stappen uit:

1. Open de Ambari-Webinterface voor uw cluster. De URL is https://YOURCLUSTERNAME.azurehdinsight.net.

    Wanneer u hierom wordt gevraagd, verifiëren bij de site met de naam van het HTTP-account (standaard: beheerder) en het wachtwoord voor uw cluster.

   > [!NOTE]  
   > U mogelijk gevraagd een tweede maal door Ambari voor de gebruikersnaam en wachtwoord. Als dit het geval is, voert u de dezelfde naam en het wachtwoord.

2. Met behulp van de lijst aan de linkerkant, selecteer het gebied van de service dat u wilt wijzigen. Bijvoorbeeld, **HDFS**. Selecteer in het gebied center, de **Peeringconfiguraties** tabblad.

    ![Afbeelding van Ambari web met configuraties voor HDFS-tabblad geselecteerd](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Met behulp van de **filteren...**  item, voer **kan worden gebruikt om**. Alleen items geretourneerd die deze tekst worden weergegeven.

    ![Gefilterde lijst](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Zoek de  **\* \_OPTS** vermelding voor de service die u wilt heapdumps voor inschakelen en toevoegen van de opties die u wilt inschakelen. In de volgende afbeelding, ik heb toegevoegd `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` naar de **HADOOP\_NAMENODE\_OPTS** post:

    ![HADOOP_NAMENODE_OPTS met - XX: + HeapDumpOnOutOfMemoryError - XX: HeapDumpPath = / tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]  
   > Wanneer heap inschakelen voor de kaart geheugendumps of verminder onderliggend proces, zoekt u naar voor de velden met de naam **mapreduce.admin.map.child.java.opts** en **mapreduce.admin.reduce.child.java.opts**.

    Gebruik de **opslaan** knop de wijzigingen op te slaan. U kunt een korte opmerking met een beschrijving van de wijzigingen kunt invoeren.

5. Nadat de wijzigingen zijn toegepast, de **opnieuw opstarten is vereist** pictogram verschijnt naast een of meer services.

    ![vereist het pictogram Start opnieuw op en start opnieuw op de knop](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Selecteer elke service die opnieuw worden opgestart moet en gebruik de **serviceacties** knop **onderhoudsmodus inschakelen**. In de onderhoudsmodus wordt voorkomen dat waarschuwingen worden gegenereerd op basis van de service wanneer u deze opnieuw starten.

    ![Menu voor onderhoud-modus inschakelen](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. Zodra u de onderhoudsmodus hebt ingeschakeld, gebruikt u de **opnieuw** knop om de service te **alle heeft opnieuw opstarten**

    ![Start opnieuw op alle betrokken item](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]  
   > de vermeldingen voor de **opnieuw** knop kan mogelijk verschillen voor andere services.

8. Zodra de services opnieuw zijn opgestart, gebruikt u de **serviceacties** knop **inschakelen uit de onderhoudsmodus**. Deze Ambari bewaking voor waarschuwingen voor de service wordt hervat.

