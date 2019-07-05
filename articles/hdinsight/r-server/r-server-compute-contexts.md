---
title: Opties voor COMPUTE context voor ML-Services op HDInsight - Azure
description: Meer informatie over de verschillende compute-context-opties beschikbaar voor gebruikers met ML-Services op HDInsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: a2c66c5c4f1abe535eb51dba9101757ce6d26157
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444348"
---
# <a name="compute-context-options-for-ml-services-on-hdinsight"></a>Opties voor COMPUTE context voor ML-Services op HDInsight

ML-Services op Azure HDInsight bepaalt hoe aanroepen zijn uitgevoerd door de instelling van de compute-context. In dit artikel bevat een overzicht van de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerd over kernen van het edge-knooppunt of de HDInsight-cluster.

Het edge-knooppunt van een cluster biedt een handige locatie verbinding maken met het cluster en uw R-scripts uit te voeren. Met een edge-knooppunt hebt u de mogelijkheid van het uitvoeren van de geparallelliseerde gedistribueerde functies van RevoScaleR over de kernen van het edge-knooppunt-server. U kunt ook ze uitvoeren op de knooppunten van het cluster met behulp van RevoScaleR Hadoop-Mapreduce of Apache Spark compute-context.

## <a name="ml-services-on-azure-hdinsight"></a>ML-Services op Azure HDInsight
[ML-Services op Azure HDInsight](r-server-overview.md) biedt de nieuwste mogelijkheden voor analyse op basis van R van. Deze kunt gegevens die zijn opgeslagen in een Apache Hadoop HDFS-container in uw [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob-opslag") storage-account, een Data Lake store of het lokale bestandssysteem van Linux. Omdat het ML-Services is gebouwd op open-source R, wordt in de toepassingen op basis van R die u bouwt de 8000 + open source R-pakketten kunnen toepassen. Ze kunnen ook de routines in gebruiken [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), van Microsoft big data analytics-pakket dat is opgenomen in ML-Services.  

## <a name="compute-contexts-for-an-edge-node"></a>COMPUTE-context voor een edge-knooppunt
In het algemeen een R-script dat wordt uitgevoerd in de cluster ML-Services op het edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen zijn de stappen die een RevoScaleR-functie aanroepen. De RevoScaleR-aanroepen uitgevoerd in een compute-omgeving die wordt bepaald door het stelt u in de RevoScaleR compute-context.  Wanneer u uw R-script vanaf een edge-knooppunt uitvoeren, wordt de mogelijke waarden van de compute-context zijn:

- lokale opeenvolgende (*lokale*)
- lokale parallel (*localpar*)
- Mapreduce
- Spark

De *lokale* en *localpar* opties verschillen alleen in het **rxExec** aanroepen worden uitgevoerd. Beide uitvoeren van andere rx-functie aanroepen in een parallelle manier over alle beschikbare kernen, tenzij anders aangegeven door gebruik te maken van de RevoScaleR **numCoresToUse** optie, bijvoorbeeld `rxOptions(numCoresToUse=6)`. Opties voor parallelle uitvoering bieden een optimale prestaties.

De volgende tabel geeft een overzicht van de verschillende opties voor compute-context om in te stellen hoe aanroepen worden uitgevoerd:

| Compute-context  | Over het instellen                      | Context voor uitvoering                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Lokale opeenvolgende | rxSetComputeContext('local')    | Geparallelliseerde uitvoering voor de cores van de edge-knooppunt-server, met uitzondering van rxExec-aanroepen, opeenvolgend worden uitgevoerd |
| Lokale parallel   | rxSetComputeContext('localpar') | Geparallelliseerde kan worden uitgevoerd over de kernen van het edge-knooppunt-server |
| Spark            | RxSpark()                       | Geparallelliseerde gedistribueerde uitvoering via Spark op de knooppunten van het HDI-cluster |
| Mapreduce       | RxHadoopMR()                    | Geparallelliseerde gedistribueerde uitvoeren via Mapreduce op de knooppunten van het HDI-cluster |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Richtlijnen voor het bepalen van een compute-context

Welke van de drie opties waarmee geparallelliseerde worden uitgevoerd die u kiest is afhankelijk van de aard van uw werk analytics, de grootte en de locatie van uw gegevens. Er is geen eenvoudige formule die u welke-context vertelt COMPUTE te gebruiken. Er zijn echter enkele principes die u de juiste keuze of ten minste, kunt u uw keuze verfijnen helpen kunnen voordat u een benchmark uitvoert. Deze principes begeleiden omvatten:

- Het lokale bestandssysteem voor Linux is sneller dan HDFS.
- Herhaalde analyses zijn sneller als de gegevens lokaal en als deze zich binnen XDF.
- Is het beter om te streamen kleine hoeveelheden gegevens uit een tekstgegevensbron. Als de hoeveelheid gegevens groter is, deze converteren naar XDF voor analyse.
- De overhead van het kopiëren of de streaminggegevens op het edge-knooppunt voor de analyse wordt voor zeer grote hoeveelheden gegevens.
- ApacheSpark is sneller dan Mapreduce voor analyse in Hadoop.

Deze beginselen worden gegeven, bieden in de volgende secties sommige algemene vuistregels voor het selecteren van een compute-context.

### <a name="local"></a>Lokaal
* Als de hoeveelheid gegevens te analyseren klein is en geen herhaalde analyse vereist, vervolgens streamen deze rechtstreeks in de analyse van routinematige via *lokale* of *localpar*.
* Als de hoeveelheid gegevens te analyseren is van kleine of middelgrote en herhaalde analyse vereist, klikt u vervolgens kopiëren naar het lokale bestandssysteem, importeert u het naar XDF en analyseer ze via *lokale* of *localpar*.

### <a name="apache-spark"></a>Apache Spark
* Als de hoeveelheid gegevens te analyseren groot is, dan importeren naar een Spark DataFrame met **RxHiveData** of **RxParquetData**, of XDF in HDFS (tenzij opslag een probleem is), en analyseer ze met behulp van de Spark-compute context.

### <a name="apache-hadoop-map-reduce"></a>Apache Hadoop-Mapreduce
* Gebruik de compute-context voor Mapreduce alleen als er een onoverkomelijke probleem met de Spark compute-context omdat deze meestal minder.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Inline-hulp op rxSetComputeContext
Zie voor meer informatie en voorbeelden van RevoScaleR compute-context, de inline in R help bij de methode rxSetComputeContext, bijvoorbeeld:

    > ?rxSetComputeContext

U kunt ook verwijzen naar de [gedistribueerd overzicht van cloudcomputing](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing) in [documentatie voor Machine Learning Server](https://docs.microsoft.com/machine-learning-server/).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd over de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerd over kernen van het edge-knooppunt of de HDInsight-cluster. Zie de volgende onderwerpen voor meer informatie over het gebruik van ML-Services met HDInsight-clusters:

* [Overzicht van ML-Services voor Apache Hadoop](r-server-overview.md)
* [Opties voor Azure Storage voor ML-Services op HDInsight](r-server-storage.md)

