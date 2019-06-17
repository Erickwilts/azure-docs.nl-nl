---
title: MapReduce gebruiken en Curl met Apache Hadoop in HDInsight - Azure
description: Leer hoe u MapReduce-taken op afstand met Apache Hadoop op HDInsight met Curl uitgevoerd.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: e4968310459097fc6a00f7c453846fe61726c3d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64716112"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-rest"></a>MapReduce-taken uitvoeren met Apache Hadoop op HDInsight met behulp van REST

Informatie over het gebruik van de REST API Apache Hive-WebHCat MapReduce-taken uitvoeren op een Apache Hadoop op HDInsight-cluster. CURL is gebruikt om te demonstreren hoe u met HDInsight kunt werken met behulp van onbewerkte HTTP-aanvragen MapReduce-taken uit te voeren.

> [!NOTE]  
> Als u al bekend bent met behulp van Hadoop op basis van Linux-servers, maar u niet bekend bent met HDInsight, raadpleegt u de [wat u moet weten over Apache Hadoop op basis van Linux in HDInsight](../hdinsight-hadoop-linux-information.md) document.


## <a id="prereq"></a>Vereisten

* Een Hadoop op HDInsight-cluster
* Windows PowerShell of [Curl](https://curl.haxx.se/) en [jq](https://stedolan.github.io/jq/)

## <a id="curl"></a>Voer een MapReduce-taak

> [!NOTE]  
> Wanneer u Curl of een andere REST-communicatie met WebHCat gebruikt, moet u de aanvragen verifiëren door te geven van de gebruikersnaam van beheerder van HDInsight-cluster en het wachtwoord. U moet de clusternaam gebruiken als onderdeel van de URI die wordt gebruikt voor het verzenden van de aanvragen naar de server.
>
> De REST-API is beveiligd met behulp van [eenvoudige verificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren met behulp van HTTPS om ervoor te zorgen dat uw referenties veilig worden verzonden naar de server.

1. Om in te stellen de aanmelding bij cluster dat wordt gebruikt door de scripts in dit document, gebruikt u een van de volgende opdrachten:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Om in te stellen de clusternaam, gebruikt u een van de volgende opdrachten:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. Gebruik een opdrachtregel met de volgende opdracht om te controleren of u verbinding met uw HDInsight-cluster kunt maken:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    U ontvangt een reactie vergelijkbaar met de volgende JSON:

        {"status":"ok","version":"v1"}

    In deze opdracht worden de volgende parameters gebruikt:

   * **-u**: Geeft aan dat de gebruikersnaam en wachtwoord voor het verifiëren van de aanvraag
   * **-G**: Geeft aan dat deze bewerking een GET-aanvraag is

   Het begin van de URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1** , is hetzelfde voor alle aanvragen.

4. Als u wilt een MapReduce-taak, gebruik de volgende opdracht:

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Het einde van de URI (/ mapreduce/jar) geeft WebHCat dat deze aanvraag wordt gestart voor een MapReduce-taak van een klasse in een jar-bestand. In deze opdracht worden de volgende parameters gebruikt:

   * **-d**: `-G` wordt niet gebruikt, zodat de aanvraag standaard ingesteld op de POST-methode. `-d` Hiermee geeft u de waarden die worden verzonden met de aanvraag.
     * **User.name**: De gebruiker die de opdracht wordt uitgevoerd
     * **JAR**: De locatie van de jar-bestand met de klasse die moet worden uitgevoerd
     * **klasse**: De klasse die de MapReduce-logica bevat
     * **func**: De argumenten worden doorgegeven aan de MapReduce-taak. In dit geval, de invoertekst-bestand en de map die worden gebruikt voor de uitvoer

   Met deze opdracht moet een taak-ID die kan worden gebruikt om te controleren of de status van de taak retourneren:

       job_1415651640909_0026

5. Om te controleren of de status van de taak, gebruikt u de volgende opdracht:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    Als de taak voltooid is, wordt de status geretourneerd is `SUCCEEDED`.

   > [!NOTE]  
   > Deze aanvraag Curl retourneert een JSON-document met informatie over de taak. Jq wordt gebruikt om op te halen, alleen de statuswaarde.

6. Wanneer de status van de taak is gewijzigd in `SUCCEEDED`, kunt u de resultaten van de taak ophalen uit Azure Blob-opslag. De `statusdir` parameter die wordt doorgegeven aan de query bevat de locatie van het uitvoerbestand. In dit voorbeeld is de locatie `/example/curl`. Dit adres slaat de uitvoer van de taak in de standaardopslag clusters op `/example/curl`.

U kunt de lijst en deze bestanden downloaden met behulp van de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Zie voor meer informatie over het werken met blobs uit de Azure CLI de [met de Azure CLI met Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) document.

## <a id="nextsteps"></a>Volgende stappen

Voor algemene informatie over MapReduce-taken in HDInsight:

* [MapReduce gebruiken met Apache Hadoop op HDInsight](hdinsight-use-mapreduce.md)

Voor meer informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Apache Hive gebruiken met Apache Hadoop op HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met Apache Hadoop op HDInsight](hdinsight-use-pig.md)

Zie voor meer informatie over de REST-interface die wordt gebruikt in dit artikel, de [WebHCat verwijzing](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
