---
title: Apache Hive gebruiken met PowerShell in HDInsight - Azure
description: PowerShell gebruiken voor het uitvoeren van Hive-query's in Apache Hadoop op HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 243713d7961c911cdda93d3d680a952d424da22b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078377"
---
# <a name="run-apache-hive-queries-using-powershell"></a>Apache Hive-query's uitvoeren met behulp van PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell in de modus Azure Resource Group Hive-query's uitvoeren in een Apache Hadoop op HDInsight-cluster.

> [!NOTE]  
> Dit document biedt geen een gedetailleerde beschrijving van wat de HiveQL-instructies die worden gebruikt in de voorbeelden doen. Zie voor informatie over de HiveQL die wordt gebruikt in dit voorbeeld, [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Een Linux-gebaseerde Apache Hadoop op HDInsight-clusterversie 3.4 of hoger.

* Een client met Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-a-hive-query"></a>Een Hive-query uitvoeren

Azure PowerShell biedt *cmdlets* waarmee u op afstand uitvoeren van Hive-query's op HDInsight. Intern, de REST-aanroepen naar voor het maken van de cmdlets [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van Hive-query's in een extern HDInsight-cluster:

* `Connect-AzAccount`: Azure PowerShell geverifieerd bij uw Azure-abonnement.
* `New-AzHDInsightHiveJobDefinition`: Hiermee maakt u een *taak definitie* met behulp van de opgegeven HiveQL-instructies.
* `Start-AzHDInsightJob`: De taakdefinitie verzendt naar HDInsight en wordt de taak wordt gestart. Een *taak* object wordt geretourneerd.
* `Wait-AzHDInsightJob`: Maakt gebruik van het taakobject om de status van de taak te controleren. Wacht totdat de taak is voltooid of de wachttijd is overschreden.
* `Get-AzHDInsightJobOutput`: Gebruikt voor het ophalen van de uitvoer van de taak.
* `Invoke-AzHDInsightHiveJob`: Gebruikt voor het uitvoeren van HiveQL-instructies. Deze cmdlet-blokken de query is voltooid en vervolgens worden de resultaten geretourneerd.
* `Use-AzHDInsightCluster`: Hiermee stelt u het huidige cluster moet worden gebruikt voor de `Invoke-AzHDInsightHiveJob` opdracht.

De volgende stappen laten zien hoe u deze cmdlets gebruiken om een taak uitvoeren in uw HDInsight-cluster:

1. Met behulp van een editor, slaat u de volgende code als `hivejob.ps1`.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen in de locatie van de `hivejob.ps1` bestand en gebruik vervolgens de volgende opdracht uit het script uit te voeren:

        .\hivejob.ps1

    Wanneer het script wordt uitgevoerd, wordt u gevraagd de naam van het cluster en de referenties van het HTTPS-/ Cluster-Admin-account in te voeren. U kunt ook gevraagd zich aanmeldt bij uw Azure-abonnement.

3. Wanneer de taak is voltooid, retourneert deze informatie die vergelijkbaar is met de volgende tekst:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Zoals eerder aangegeven, `Invoke-Hive` kan worden gebruikt om een query uitvoeren en wacht tot het antwoord. Het volgende script gebruiken om te zien hoe Invoke-Hive werkt:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    De uitvoer lijkt op de volgende tekst:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]  
   > Voor meer HiveQL-query's, kunt u de Azure PowerShell **hier tekenreeksen** cmdlet of HiveQL scriptbestanden. Het volgende fragment ziet u hoe u de `Invoke-Hive` cmdlet een bestand HiveQL-script uit te voeren. Het bestand HiveQL-script moet worden geüpload naar wasb: / /.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Voor meer informatie over **hier tekenreeksen**, Zie <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">met behulp van Windows PowerShell hier-tekenreeksen</a>.

## <a name="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, moet u de foutenlogboeken weergeven. Als u wilt bekijken foutgegevens voor deze taak, Voeg het volgende toe aan het einde van de `hivejob.ps1` bestand, opslaan en voer het vervolgens opnieuw uit.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de gegevens die worden geschreven naar STDERR tijdens de taakverwerking van de.

## <a name="summary"></a>Samenvatting

Zoals u ziet, biedt Azure PowerShell een eenvoudige manier om Hive-query's uitvoeren in een HDInsight-cluster, de taakstatus controleren en ophalen van de uitvoer.

## <a name="next-steps"></a>Volgende stappen

Voor algemene informatie over Hive in HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)
