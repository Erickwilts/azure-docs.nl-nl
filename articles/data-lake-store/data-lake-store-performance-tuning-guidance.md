---
title: Azure Data Lake Storage Gen1 prestaties richtlijnen over het afstemmen | Microsoft Docs
description: Azure Data Lake Storage Gen1 prestaties richtlijnen over het afstemmen
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: a8a50db5ece242bc00a28e66e21c863388950d6f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61437603"
---
# <a name="tuning-azure-data-lake-storage-gen1-for-performance"></a>Azure Data Lake Storage Gen1 voor prestaties afstemmen

Azure Data Lake Storage Gen1 ondersteunt hoge doorvoer voor i/o intensieve analyse- en -verplaatsing.  In Data Lake Storage Gen1 is het belangrijk om de beste prestaties met behulp van alle beschikbare doorvoer – de hoeveelheid gegevens die kunnen worden gelezen of geschreven per seconde –.  Dit wordt bereikt door het uitvoeren van zoveel leest en schrijft parallel mogelijk.

![Data Lake Storage Gen1 prestaties](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Data Lake Storage Gen1 kunt schalen om de doorvoer die nodig zijn voor alle analytics-scenario. Een Data Lake Storage Gen1-account biedt standaard automatisch voldoende doorvoer om te voldoen aan de behoeften van een uitgebreide categorie van de use-cases. Voor de gevallen waar klanten in de standaardlimiet uitvoeren, kan het Gen1 van Data Lake Storage-account worden geconfigureerd om een meer doorvoer door contact op met Microsoft ondersteuning.

## <a name="data-ingestion"></a>Gegevensopname

Bij het ophalen van gegevens uit een bronsysteem naar Data Lake Storage Gen1, is het belangrijk te bedenken dat de hardware van de bron-, bron netwerkhardware- en de netwerkverbinding met Data Lake Storage Gen1 het knelpunt kunnen worden.  

![Data Lake Storage Gen1 prestaties](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Het is belangrijk om ervoor te zorgen dat de verplaatsing van gegevens wordt niet beïnvloed door deze factoren.

### <a name="source-hardware"></a>Source Hardware

Of u on-premises computers of virtuele machines in Azure gebruikt, moet u zorgvuldig de juiste hardware. Voor bron schijfhardware, SSD's liever HDD's en schijfhardware met snellere aandrijfassen kiezen. Voor bron netwerkhardware, gebruikt u de snelste NIC's mogelijk.  Op Azure, raden we aan Azure D14-VM's waarvoor de op de juiste wijze krachtige schijf- en netwerkhardware.

### <a name="network-connectivity-to-data-lake-storage-gen1"></a>De netwerkverbinding met Data Lake Storage Gen1

De netwerkverbinding tussen de brongegevens en Data Lake Storage Gen1 kan soms het knelpunt zijn. Wanneer de brongegevens bevinden zich On-Premises, kunt u overwegen een speciale koppeling met [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) . Als de brongegevens bevinden zich in Azure, de prestaties worden aanbevolen wanneer de gegevens zich in dezelfde Azure-regio als het Gen1 van Data Lake Storage-account.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Hulpprogramma's voor opname van gegevens voor maximale parallellisering configureren

Nadat u de bron-hardware hebt opgelost en network connectivity knelpunten hierboven, bent u klaar om te configureren van uw opname-hulpprogramma's. De volgende tabel geeft een overzicht van de belangrijkste instellingen voor verschillende populaire gegevensopnamehulpprogramma's en artikelen voor hen afstemmen van de uitgebreide prestaties biedt.  Voor meer informatie over welk hulpprogramma moet worden gebruikt voor uw scenario, gaat u naar dit [artikel](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios).

| Hulpprogramma               | Instellingen     | Meer informatie                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, ConcurrentFileCount |  [Link](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-powershell) |
| AdlCopy    | Azure Data Lake Analytics units  |   [Link](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (mapper)   | [Link](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Link](../data-factory/copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.Block.Size, -m (toewijzen)    |   [Link](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Structuur van uw gegevensset

Wanneer gegevens worden opgeslagen in Data Lake Storage Gen1, de grootte van het hebben aantal bestanden en de structuur van een invloed op prestaties.  De volgende sectie wordt beschreven aanbevolen procedures in deze gebieden.  

### <a name="file-size"></a>Bestandsgrootte

Analytics-engines, zoals HDInsight en Azure Data Lake Analytics hebben meestal een overhead per bestand.  Als u uw gegevens zo veel kleine bestanden opslaat, kan dit de prestaties negatief beïnvloeden.  

Organiseer uw gegevens in het algemeen in grotere grootte bestanden voor betere prestaties.  Indelen als vuistregel, data-sets in bestanden van 256MB of hoger. In sommige gevallen, zoals afbeeldingen en binaire gegevens is het niet mogelijk deze parallel kan verwerken.  In dergelijke gevallen is het raadzaam te houden van afzonderlijke bestanden minder dan 2GB.

Gegevens-pipelines hebt soms beperkte controle over de onbewerkte gegevens die voorzien van tal van kleine bestanden is.  Het wordt aanbevolen dat een 'kookinspiratie' proces dat wordt gegenereerd grotere bestanden moet worden gebruikt voor downstream-toepassingen.

### <a name="organizing-time-series-data-in-folders"></a>Ordenen van Time Series-gegevens in mappen

Voor workloads met Hive en ADLA kunt verwijderen van de partitie van time series-gegevens enkele query's uit het lezen van slechts een subset van de gegevens die zorgt voor betere prestaties.    

Deze pijplijnen die time series-gegevens opnemen, vaak hun bestanden met een zeer gestructureerd naamgeving voor bestanden en mappen te plaatsen. Hieronder volgt een voorbeeld van een heel normaal die we zien voor gegevens die is geordend op datum:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

U ziet dat de datum/tijd-informatie wordt weergegeven als de mappen die zowel in de bestandsnaam.

Voor datum en tijd is het volgende een algemeen patroon

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Nogmaals, de keuze die u maakt met de map en Bestandsorganisatie moet optimaliseren voor de bestandsgrootte en een redelijk aantal bestanden in elke map.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>I/o-intensieve taken op Hadoop en Spark-workloads op HDInsight te optimaliseren

Taken kunnen worden onderverdeeld in een van de volgende drie categorieën:

* **CPU-intensief.**  Deze taken zijn lang berekening tijdstippen met minimale i/o-tijden.  Voorbeelden zijn onder meer het machine learning en taken voor de verwerking van natuurlijke taal.  
* **Geheugenintensieve.**  Deze taken veel geheugen gebruiken.  Voorbeelden zijn PageRank en realtime analytics-taken.  
* **I/o-intensieve.**  Deze taken het merendeel van de tijd i/o doen.  Een veelvoorkomend voorbeeld is een taak voor het kopiëren die alleen lezen en schrijven.  Andere voorbeelden zijn onder meer gegevensvoorbereidingstaken die grote hoeveelheden gegevens lezen, voert enkele gegevenstransformatie en schrijft de gegevens terug naar de store.  

De volgende richtlijnen zijn alleen van toepassing op i/o-intensieve taken.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Algemene overwegingen voor een HDInsight-cluster

* **HDInsight-versies.** Gebruik de nieuwste versie van HDInsight voor de beste prestaties.
* **De regio's.** Plaats de Gen1 van Data Lake Storage-account in dezelfde regio bevinden als het HDInsight-cluster.  

Een HDInsight-cluster bestaat uit twee hoofdknooppunten en het aantal worker-knooppunten. Elk werkrolknooppunt biedt een bepaald aantal kernen en geheugen, wat wordt bepaald door de VM-type.  Wanneer een taak wordt uitgevoerd, is YARN de resource negotiator dat toewijst van het beschikbare geheugen en kernen om containers te maken.  Elke container wordt uitgevoerd de taken die nodig zijn om de taak te voltooien.  Containers voor het snel verwerken van de taken parallel worden uitgevoerd. Daarom worden de prestaties zijn verbeterd door het uitvoeren van zo veel parallelle containers mogelijk.

Er zijn drie lagen in een HDInsight-cluster dat het aantal containers verhogen en het gebruik van alle beschikbare doorvoer kunnen worden afgestemd.  

* **Fysieke laag**
* **YARN-laag**
* **Workload-laag**

### <a name="physical-layer"></a>Fysieke laag

**Cluster met meer knooppunten en/of groter formaat virtuele machines worden uitgevoerd.**  Een groter cluster kunt u meer YARN-containers zoals wordt weergegeven in de volgende afbeelding worden uitgevoerd.

![Data Lake Storage Gen1 prestaties](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Virtuele machines met meer netwerkbandbreedte gebruiken.**  De hoeveelheid netwerkbandbreedte kan een knelpunt zijn als er minder netwerkbandbreedte dan Data Lake Storage Gen1 doorvoer.  Verschillende virtuele machines hebben verschillende grootten voor netwerk-bandbreedte.  Kies een VM-type met de grootste mogelijke netwerkbandbreedte.

### <a name="yarn-layer"></a>YARN-laag

**Kleinere YARN-containers gebruiken.**  Reduceer de grootte van elke container YARN meer containers met dezelfde hoeveelheid resources maken.

![Data Lake Storage Gen1 prestaties](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

Afhankelijk van uw workload wordt altijd er een minimale YARN containergrootte die nodig is. Als u een container te klein kiest, worden de taken worden uitgevoerd in out-van-geheugenproblemen. YARN-containers moeten normaal gesproken niet kleiner zijn dan 1GB. Het is gebruikelijk om 3GB YARN-containers te zien. Voor sommige werkbelastingen moet u mogelijk groter YARN-containers.  

**Kernen per container YARN verhogen.**  Verhoog het aantal kernen dat is toegewezen aan elke container om het aantal parallelle taken die worden uitgevoerd in elke container te vergroten.  Dit werkt voor toepassingen, zoals Spark die meerdere taken per container worden uitgevoerd.  Voor toepassingen, zoals Hive die een enkele thread in elke container wordt uitgevoerd, is het beter om meer containers in plaats van meer kerngeheugens per container hebt.

### <a name="workload-layer"></a>Workload-laag

**Alle beschikbare containers gebruiken.**  Stel het aantal taken moet gelijk zijn aan of groter zijn dan het aantal beschikbare containers, zodat alle resources worden gebruikt.

![Data Lake Storage Gen1 prestaties](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Mislukte taken zijn kostbaar.** Als elke taak een grote hoeveelheid gegevens heeft verwerken, klikt u vervolgens resulteert mislukken van een taak in een dure opnieuw proberen.  Daarom is het beter om meer taken, die elk een kleine hoeveelheid gegevens verwerkt te maken.

Naast de algemene richtlijnen die hierboven heeft elke toepassing andere parameters die beschikbaar zijn om af te stemmen voor de specifieke toepassing. De volgende tabel bevat enkele van de parameters en de koppelingen aan de slag met prestaties afstemmen voor elke toepassing.

| Workload               | Parameter om taken te stellen                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [Spark in HDInsight](data-lake-store-performance-tuning-spark.md)       | <ul><li>NUM Executor</li><li>Executor-geheugen</li><li>Executor-cores</li></ul> |
| [Hive in HDInsight](data-lake-store-performance-tuning-hive.md)    | <ul><li>hive.tez.container.size</li></ul>         |
| [MapReduce in HDInsight](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.map.Memory</li><li>Mapreduce.job.maps</li><li>Mapreduce.reduce.memory</li><li>Mapreduce.job.reduces</li></ul> |
| [Storm op HDInsight](data-lake-store-performance-tuning-storm.md)| <ul><li>Aantal werkprocessen</li><li>Aantal spout executor-exemplaren</li><li>Aantal bolt executor-exemplaren </li><li>Aantal spout-taken</li><li>Bolt-taken</li></ul>|

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
