---
title: Wat is Apache Storm - Azure HDInsight
description: Met Apache Storm kunt u gegevensstromen in realtime verwerken. Met Azure HDInsight kunt u eenvoudig Storm-clusters maken in de Azure-cloud. Met Visual Studio kunt u Storm-oplossingen schrijven in C# en deze vervolgens implementeren in uw HDInsight Storm-clusters.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: apache storm use cases,storm cluster,wat is apache storm
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: overview
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: fd377d731b9e916414c7d1a568c7267e73d6bf33
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137238"
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Wat is Apache Storm in Azure HDInsight?

[Apache Storm](https://storm.apache.org/) is een gedistribueerd, fouttolerant en open-source computingsysteem. U kunt Storm gebruiken om te verwerken gegevensstromen in realtime met [Apache Hadoop](https://hadoop.apache.org/). Dankzij de mogelijkheid om gegevens die de eerste keer niet zijn verwerkt, opnieuw te verwerken, bieden Storm-oplossingen u de garantie dat de gegevens worden verwerkt.

## <a name="why-use-apache-storm-on-hdinsight"></a>Waarom Apache Storm op HDInsight gebruiken?

Storm op HDInsight biedt de volgende functies:

* __99% Service Level Agreement (SLA) op Storm uptime__: Zie [SLA-informatie voor HDInsight](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) voor meer informatie.

* Ondersteunt de mogelijkheid om eenvoudig aanpassingen te implementeren door tijdens of na het maken van een Storm-cluster scripts voor dat cluster uit te voeren. Zie [HDInsight-clusters aanpassen met scriptacties](../hdinsight-hadoop-customize-cluster-linux.md) voor meer informatie.

* **Oplossingen maken in meerdere talen**: U kunt Storm-onderdelen schrijven in de taal van uw keuze, bijvoorbeeld in Java, C# of Python.

    * Integreert Visual Studio met HDInsight voor het ontwikkelen, beheren en controleren van C#-topologieën. Zie [C# Storm-topologieën met de hulpprogramma's van HDInsight voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

    * Biedt ondersteuning voor de Trident Java-interface. U kunt Storm-topologieën maken die ondersteuning bieden voor een eenmalige verwerking van berichten, transactionele DataStore-persistentie en een aantal algemene Stream Analytics-bewerkingen.

* **Dynamische schaalbaarheid**: U kunt werkknooppunten toevoegen of verwijderen zonder de actieve Storm-topologieën te beïnvloeden.

    * U moet de lopende topologieën deactiveren en reactiveren om te profiteren van de nieuwe knooppunten die zijn toegevoegd via vergroten/verkleinen.

* **Streaming-pijplijnen met meerdere Azure-services maken**: Storm op HDInsight kan worden geïntegreerd met andere Azure-services zoals Event Hubs, SQL-Database, Azure Storage en Azure Data Lake-opslag.

    Zie voor een Voorbeeldoplossing die kan worden geïntegreerd met Azure-services, [verwerken van gebeurtenissen van Event Hubs met Apache Storm op HDInsight](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

Zie [deze Engelstalige site](https://storm.apache.org/documentation/Powered-By.html) voor een lijst met bedrijven die Apache Storm gebruiken voor hun oplossingen voor realtime analyse.

Als u wilt aan de slag met Storm, Zie [aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md).

## <a name="how-does-apache-storm-work"></a>Hoe werkt de Apache Storm

Storm voert topologieën uit in plaats van de [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) taken die u mogelijk kent. Storm-topologieën bestaan uit meerdere onderdelen die zijn gerangschikt in een Directed Acyclic Graph (DAG). Gegevens stromen tussen de onderdelen in de grafiek. Elk onderdeel verbruikt een of meer gegevensstromen en kan eventueel een of meer stromen genereren. Het volgende diagram toont hoe de gegevens stromen tussen de onderdelen van een eenvoudige topologie voor het tellen van woorden:

![Voorbeeld van hoe onderdelen zijn gerangschikt in een Storm-topologie](./media/apache-storm-overview/example-apache-storm-topology-diagram.png)

* Via Spout-onderdelen worden gegevens overgebracht naar een topologie. Deze onderdelen introduceren een of meer stromen in de topologie.

* Bolt-onderdelen verwerken stromen die afkomstig zijn van spouts of andere bolts. Bolts kunnen eventueel nieuwe stromen in de topologie introduceren. Bolts zijn ook verantwoordelijk voor het wegschrijven van gegevens naar externe services of opslag, zoals HDFS, Kafka of HBase.

## <a name="reliability"></a>Betrouwbaarheid

Apache Storm zorgt ervoor dat elk binnenkomend bericht altijd volledig wordt verwerkt, zelfs wanneer de gegevensanalyse is verspreid over honderden knooppunten.

Het Nimbus-knooppunt biedt een functionaliteit die vergelijkbaar is met de Apache Hadoop JobTracker en wijst taken toe aan andere knooppunten in een cluster via [Apache ZooKeeper](https://zookeeper.apache.org/). Zookeeper-knooppunten geven een cluster coördinatiemogelijkheden en faciliteren de communicatie tussen Nimbus en het Supervisor-proces op de werkknooppunten. Als een verwerkingsknooppunt wordt uitgeschakeld, wordt het Nimbus-knooppunt hiervan op de hoogte gesteld en worden de taak en de bijbehorende gegevens toegewezen aan een ander knooppunt.

Apache Storm-clusters worden standaard geconfigureerd met slechts één Nimbus-knooppunt. Storm in HDInsight ondersteunt twee Nimbus-knooppunten. Als het primaire knooppunt uitvalt, schakelt het Storm-cluster over naar het secundaire knooppunt en wordt het primaire knooppunt hersteld. Het volgende diagram illustreert de taakstroomconfiguratie voor Storm op HDInsight:

![Diagram van nimbus, zookeeper en supervisor](./media/apache-storm-overview/nimbus.png)

## <a name="ease-of-creation"></a>Eenvoudig te maken

U kunt in enkele minuten een nieuw Storm-cluster op HDInsight maken. Zie [Aan de slag met Storm op HDInsight](apache-storm-tutorial-get-started-linux.md) voor meer informatie over het maken van een Storm-cluster.

## <a name="ease-of-use"></a>Gebruiksgemak

* __Secure Shell (SSH)-connectiviteit__: U kunt toegang tot de hoofdknooppunten van het Storm-cluster via Internet met behulp van SSH. U kunt opdrachten rechtstreeks op het cluster uitvoeren met behulp van SSH.

  Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

* __Webconnectiviteit__: Alle HDInsight-clusters bieden de Ambari-Webgebruikersinterface. Met de Ambare-webgebruikersinterface kunt u eenvoudig services op het cluster controleren, configureren en beheren. Storm-clusters bieden ook de Storm-gebruikersinterface. Met behulp van de Storm-gebruikersinterface kunt u actieve Storm-topologieën controleren en beheren vanuit de browser.

  Zie voor meer informatie de [beheren HDInsight met behulp van de Apache Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md) en [bewaken en beheren met behulp van de Apache Storm-gebruikersinterface](apache-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) documenten.

* __Azure PowerShell en Azure klassieke CLI__: PowerShell en klassieke CLI bevatten allebei opdrachtregelprogramma's die u vanuit het clientsysteem gebruiken kunt om te werken met HDInsight en andere Azure-services.

* __Visual Studio-integratie__: Azure Data Lake Tools voor Visual Studio bevatten projectsjablonen voor het maken van C# Storm-topologieën met behulp van het SCP.NET-framework. Data Lake Tools biedt ook hulpprogramma's voor het implementeren, bewaken en beheren van oplossingen met Storm op HDInsight.

  Zie [C# Storm-topologieën met de hulpprogramma's van HDInsight voor Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

## <a name="integration-with-other-azure-services"></a>Integratie met andere Azure-services

* __Azure Data Lake Storage__: Zie voor een voorbeeld van het gebruik van Data Lake-opslag met een Storm-cluster, [Azure Data Lake Storage gebruiken met Apache Storm op HDInsight](apache-storm-write-data-lake-store.md).

* __Eventhubs__: Zie de volgende voorbeelden voor een voorbeeld van het gebruik van Event Hubs met een Storm-cluster:

    * [Process events from Azure Event Hubs met Apache Storm op HDInsight (Java) gebeurtenissen](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/)

    * [Verwerken van gebeurtenissen uit Azure Event Hubs met Apache Storm op HDInsight (C#)](apache-storm-develop-csharp-event-hub-topology.md)

* __SQL-Database__, __Cosmos DB__, __Eventhubs__, en __HBase__: Er zijn sjabloonvoorbeelden opgenomen in de Data Lake Tools voor Visual Studio. Zie voor meer informatie, [ontwikkelen een C# topologie voor Apache Storm op HDInsight](apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="support"></a>Ondersteuning

Storm op HDInsight wordt geleverd met volledige en onafgebroken ondersteuning op ondernemingsniveau. Storm op HDInsight beschikt eveneens over een SLA van 99,9 procent. Dit betekent dat Microsoft garandeert dat een Storm-cluster minimaal 99,9 procent van de tijd externe verbinding heeft.

Zie [Ondersteuning van Azure](https://azure.microsoft.com/support/options/) voor meer informatie.

## <a name="apache-storm-use-cases"></a>Use cases van Apache Storm

Hier volgen enkele veelvoorkomende scenario's waarvoor u Storm in HDInsight zou kunnen gebruiken:

* Internet der dingen (IoT)
* Fraudedetectie
* Sociale analyses
* Extractie, transformatie en laden (ETL)
* Netwerkbewaking
* Search
* Mobile Engagement

Zie voor meer informatie over praktijkscenario's de [hoe bedrijven gebruikmaken van Apache Storm](https://storm.apache.org/documentation/Powered-By.html) document.

## <a name="development"></a>Ontwikkeling

Met hulp van Data Lake-tools voor Visual Studio kunnen .NET-ontwikkelaars topologieën ontwerpen en implementeren voor Visual Studio. U kunt ook hybride topologieën maken die gebruikmaken van Java- en C#-onderdelen.

Zie [C#-topologieën met Visual Studio ontwikkelen voor Apache Storm op HDInsight](apache-storm-develop-csharp-visual-studio-topology.md) voor meer informatie.

U kunt ook Java-oplossingen ontwikkelen met behulp van de IDE van uw keuze. Zie voor meer informatie, [ontwikkelen van Java-topologieën voor Apache Storm op HDInsight](apache-storm-develop-java-topology.md).

Python kan ook worden gebruikt voor het ontwikkelen van Storm-onderdelen. Zie voor meer informatie, [ontwikkelen Apache Storm-topologieën met behulp van Python op HDInsight](apache-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Algemene ontwikkelingspatronen

### <a name="guaranteed-message-processing"></a>Gegarandeerde berichtverwerking

Apache Storm kan verschillende niveaus van gegarandeerde berichtverwerking bieden. Bijvoorbeeld, een eenvoudige Storm-toepassing op de minst keer worden verwerkt, kan garanderen en [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) kunt garanderen exact één keer worden verwerkt.

Zie [Guarantees on data processing](https://storm.apache.org/about/guarantees-data-processing.html) (Garanties met betrekking tot de gegevensverwerking) op apache.org voor meer informatie.

### <a name="ibasicbolt"></a>IBasicBolt

Het patroon voor het lezen van een invoer-tuple, verzenden van nul of meer tuples en vervolgens de invoer-tuple onmiddellijk aan het einde van de methode execute daar begrip voor tonen is gebruikelijk. Storm biedt de [IBasicBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/IBasicBolt.html)-interface voor het automatiseren van dit patroon.

### <a name="joins"></a>Samenvoegingen

Hoe gegevensstromen worden gekoppeld, varieert per toepassing. U kunt bijvoorbeeld elke tuple uit meerdere streams samenvoegen in één nieuwe stream, of u kunt alleen batches tuples voor een specifiek venster samenvoegen. In beide gevallen kunt u hiervoor [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) gebruiken. Veldgroepering is een manier om te definiëren hoe tuples worden gerouteerd naar bolts.

In het volgende Java-voorbeeld wordt fieldsGrouping gebruikt om tuples die afkomstig zijn uit de onderdelen 1, 2 en 3, te routeren naar de MyJoiner-bolt:

```java
builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));
```

### <a name="batches"></a>Batches

Apache Storm biedt een intern timingmechanisme dat bekend staat als een 'tick tuple'. U kunt instellen hoe vaak in uw topologie een tick tuple wordt verzonden.

Zie [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java) voor een voorbeeld van het gebruik van een tick tuple uit een C#-onderdeel.

### <a name="caches"></a>Caches

In-memory caching wordt vaak gebruikt als mechanisme om de verwerking te versnellen, omdat de veelgebruikte assets in het geheugen blijven staan. Omdat een topologie wordt verdeeld over meerdere knooppunten en meerdere processen binnen elk knooppunt, is het goed om het gebruik van [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) te overwegen. Gebruik `fieldsGrouping` om ervoor te zorgen dat tuples met velden die worden gebruikt voor het opzoeken van de cache, altijd naar hetzelfde proces worden gerouteerd. Met deze groeperingsfunctionaliteit voorkomt u dubbele cachevermeldingen voor verschillende processen.

### <a name="stream-top-n"></a>'Top N' van stromen

Wanneer uw topologie afhankelijk is van de berekening van de waarde van een 'top N', moet u die waarde in parallel berekenen. Vervolgens moet u de uitvoer van die berekeningen samenvoegen tot een globale waarde. Dit kan door met [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) op veld te routeren voor parallelle verwerking. Vervolgens kunt u de uitvoer doorsturen naar een bolt die globaal de top N-waarde bepaalt.

Zie het [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java)-voorbeeld voor een voorbeeld van een berekening van een top N-waarde.

## <a name="logging"></a>Logboekregistratie

Maakt gebruik van storm [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) om informatie te registreren. Standaard wordt een grote hoeveelheid gegevens geregistreerd en kan het lastig zijn om de informatie te doorzoeken. U kunt een configuratiebestand voor logboekregistratie opnemen als onderdeel van uw Storm-topologie om de werking van de logboekregistratie te bepalen.

Zie het voorbeeld van een [op Java gebaseerde woordentelling](apache-storm-develop-java-topology.md) voor Storm op HDInsight, voor een voorbeeldtopologie die aantoont hoe u de logboekregistratie moet configureren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over realtime analyseoplossingen met Apache Storm op HDInsight:

* [Aan de slag met Apache Storm op HDInsight](apache-storm-tutorial-get-started-linux.md)
* [Example Storm toplogies and components for Apache Storm on HDInsight](apache-storm-example-topology.md) (Voorbeelden van Storm-topologieën en -onderdelen in HDInsight)
