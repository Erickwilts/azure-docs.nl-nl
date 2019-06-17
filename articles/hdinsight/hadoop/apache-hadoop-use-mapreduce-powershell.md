---
title: MapReduce en PowerShell gebruiken met Apache Hadoop - Azure HDInsight
description: Informatie over het gebruik van PowerShell op afstand MapReduce-taken uitvoeren met Apache Hadoop op HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: hrasheed
ms.openlocfilehash: 2ba8ab07edc4fd036b82c97f0ae3fb565d5eed72
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078398"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>MapReduce-taken uitvoeren met Apache Hadoop op HDInsight met behulp van PowerShell

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell een MapReduce-taak uitvoeren in een Hadoop op HDInsight-cluster.

## <a id="prereq"></a>Vereisten

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* **Een Azure HDInsight (Hadoop op HDInsight)-cluster**

* **Een werkstation met Azure PowerShell**.

## <a id="powershell"></a>Voer een MapReduce-taak

Azure PowerShell biedt *cmdlets* waarmee u op afstand MapReduce-taken uitvoeren op HDInsight. Intern, kunt u PowerShell REST-aanroepen naar [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (voorheen Templeton) uitgevoerd op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van MapReduce-taken in een extern HDInsight-cluster.

* **Connect-AzAccount**: Azure PowerShell geverifieerd bij uw Azure-abonnement.

* **New-AzHDInsightMapReduceJobDefinition**: Maakt u een nieuwe *taak definitie* met behulp van de opgegeven MapReduce-informatie.

* **Start-AzHDInsightJob**: De taakdefinitie verzendt naar HDInsight en wordt de taak wordt gestart. Een *taak* object wordt geretourneerd.

* **Wait-AzHDInsightJob**: Maakt gebruik van het taakobject om de status van de taak te controleren. Wacht totdat de taak is voltooid of de wachttijd is overschreden.

* **Get-AzHDInsightJobOutput**: Gebruikt voor het ophalen van de uitvoer van de taak.

De volgende stappen laten zien hoe u deze cmdlets gebruikt voor het uitvoeren van een taak in uw HDInsight-cluster.

1. Met behulp van een editor, slaat u de volgende code als **mapreducejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen in de locatie van de **mapreducejob.ps1** bestand en gebruik vervolgens de volgende opdracht uit het script uit te voeren:

        .\mapreducejob.ps1

    Wanneer u het script uitvoert, wordt u gevraagd om de naam van het HDInsight-cluster en de aanmelding bij cluster. U kunt ook gevraagd om te verifiëren bij uw Azure-abonnement.

3. Wanneer de taak is voltooid, ontvangt u uitvoer die vergelijkbaar is met de volgende tekst:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Deze uitvoer geeft aan dat de taak is voltooid.

    > [!NOTE]  
    > Als de **ExitCode** is een waarde dan 0, Zie [probleemoplossing](#troubleshooting).

    In dit voorbeeld slaat ook de gedownloade bestanden naar een **uitvoer.txt** bestand in de map met het script uit.

### <a name="view-output"></a>Uitvoer weergeven

Als u wilt zien van de woorden en aantallen die worden geproduceerd door de taak, opent u de **uitvoer.txt** bestand in een teksteditor.

> [!NOTE]  
> De uitvoerbestanden van een MapReduce-taak zijn onveranderd. Dus als u dit voorbeeld opnieuw uitvoeren, moet u de naam van het uitvoerbestand te wijzigen.

## <a id="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, kunt u fouten voor de taak weergeven. Toevoegen als foutinformatie voor deze taak, de volgende opdracht aan het einde van de **mapreducejob.ps1** bestand, opslaan en voer het vervolgens opnieuw uit.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de informatie die is geschreven naar STDERR terwijl de taak wordt uitgevoerd.

## <a id="summary"></a>Samenvatting

Zoals u ziet, biedt Azure PowerShell een eenvoudige manier MapReduce-taken uitvoeren op een HDInsight-cluster, het bewaken van de taak de status en het ophalen van de uitvoer.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met HDInsight Hadoop](hdinsight-use-mapreduce.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)
