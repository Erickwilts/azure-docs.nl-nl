---
title: .NET gebruiken met Hadoop-MapReduce op Linux gebaseerde HDInsight - Azure
description: Informatie over het gebruik van .NET-toepassingen voor het streamen van MapReduce in HDInsight op basis van Linux.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: a1d1488840ca2b17c83f380af4fa24105bb36202
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64729477"
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>.NET-oplossingen migreren voor HDInsight op Linux gebaseerde HDInsight op basis van Windows

HDInsight op basis van Linux-clusters gebruiken [Mono (https://mono-project.com) ](https://mono-project.com) .NET-toepassingen uit te voeren. Mono kunt u .NET-componenten zoals MapReduce-toepassingen gebruiken met HDInsight op basis van Linux. In dit document leert u hoe u .NET-oplossingen die zijn gemaakt voor HDInsight op basis van een Windows-clusters om te werken met Mono op Linux gebaseerde HDInsight migreren.

## <a name="mono-compatibility-with-net"></a>Mono-compatibiliteit met .NET

Mono versie 4.2.1 is opgenomen in HDInsight versie 3.6. Zie voor meer informatie over de versie van Mono geleverd bij HDInsight [HDInsight onderdeel versies](hdinsight-component-versioning.md).

Zie voor meer informatie over de compatibiliteit tussen Mono en .NET de [Mono-compatibiliteit (https://www.mono-project.com/docs/about-mono/compatibility/) ](https://www.mono-project.com/docs/about-mono/compatibility/) document.

> [!IMPORTANT]  
> Het SCP.NET-framework is compatibel met Mono. Zie voor meer informatie over het gebruik van SCP.NET met Mono [Gebruik Visual Studio om C#-topologieën ontwikkelen voor Apache Storm op HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Draagbaarheid van geautomatiseerde analyse

De [.NET draagbaarheid Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) kan worden gebruikt om een rapport van compatibiliteitsproblemen tussen uw toepassing en Mono te genereren. Gebruik de volgende stappen uit om te configureren van de analyzer om te controleren of uw toepassing voor Mono portabiliteit:

1. Installeer de [.NET draagbaarheid Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Tijdens de installatie, selecteert u de versie van Visual Studio te gebruiken.

2. Selecteer in Visual Studio 2015, __analyseren__ > __draagbaarheid Analyzer instellingen__, en zorg ervoor dat __4.5__ is ingeschakeld de __Mono__ sectie.

    ![4.5 ingecheckt Mono sectie voor de analyzer-instellingen](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Selecteer __OK__ aan de configuratie op te slaan.

3. Selecteer __analyseren__ > __analyseren Assembly draagbaarheid__. Selecteer de assembly die de oplossing bevat, en selecteer vervolgens __Open__ om te beginnen met de analyse.

4. Zodra de analyse is voltooid, selecteert u __analyseren__ > __analyserapporten weergeven__. In __draagbaarheid analyseresultaten__, selecteer __rapport openen__ om een rapport te openen.

    ![Draagbaarheid analyzer resultaten dialoogvenster](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]  
> De analyzer kan niet alle problemen met uw oplossing catch. Bijvoorbeeld, een bestandspad van `c:\temp\file.txt` wordt beschouwd als OK als Mono wordt uitgevoerd op Windows. Hetzelfde pad is niet geldig voor een Linux-platform.

## <a name="manual-portability-analysis"></a>Handmatige draagbaarheid analyse

Voer een handmatige controle van uw code met behulp van de informatie in de [draagbaarheid van toepassingen (https://www.mono-project.com/docs/getting-started/application-portability/) ](https://www.mono-project.com/docs/getting-started/application-portability/) document.

## <a name="modify-and-build"></a>Wijzig en bouwen

U kunt echter ook doorgaan met Visual Studio uw .NET-oplossingen bouwen voor HDInsight. Echter, moet u ervoor zorgen dat het project is geconfigureerd voor het gebruik van .NET Framework 4.5.

## <a name="deploy-and-test"></a>Implementeren en testen

Als u de oplossing met behulp van de aanbevelingen van de .NET draagbaarheid Analyzer of van een handmatige analyse hebt gewijzigd, moet u deze testen met HDInsight. Testen van de oplossing op een Linux gebaseerde HDInsight-cluster, waarschijnlijk subtiele problemen die moeten worden gecorrigeerd. U wordt aangeraden dat u aanvullende logboekregistratie inschakelen in uw toepassing bij het testen.

Zie de volgende documenten voor meer informatie over het verkrijgen van toegang tot logboeken:

* [Toegang tot Apache Hadoop YARN-toepassingslogboeken op Linux gebaseerde HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Volgende stappen

* [Gebruik C# met MapReduce in HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Gebruik C# door de gebruiker gedefinieerde functies met Apache Hive en Apache Pig](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Ontwikkel C# topologieën voor Apache Storm op HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md)
