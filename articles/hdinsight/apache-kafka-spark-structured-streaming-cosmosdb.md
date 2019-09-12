---
title: Gegevens Apache Spark van Apache Kafka naar Azure Cosmos DB-Azure HDInsight
description: Leer hoe u Apache Spark Structured streaming kunt gebruiken om gegevens uit Apache Kafka te lezen en vervolgens op te slaan in Azure Cosmos DB. In dit voorbeeld gaat u met behulp van een Jupyter Notebook gegevens streamen van Spark naar HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: hrasheed
ms.openlocfilehash: b95af36ed8f8b6c9749711e2ebbf6ef8aa88f001
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2019
ms.locfileid: "70885882"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Apache Spark Structured streaming gebruiken met Apache Kafka en Azure Cosmos DB

Meer informatie over het gebruik van [Apache Spark](https://spark.apache.org/) [Structured streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) om gegevens te lezen van [Apache Kafka](https://kafka.apache.org/) in azure HDInsight en de gegevens vervolgens op te slaan in azure Cosmos db.

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is een wereld wijd gedistribueerde data base met meerdere modellen. In dit voor beeld wordt een SQL API-database model gebruikt. Zie de [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md) -document voor meer informatie.

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op Spark SQL. Hiermee kunt u streamingberekeningen op dezelfde manier weergeven als batchberekeningen van statische gegevens. Meer informatie over Structured streaming vindt u in de [hand leiding Structured streaming Programming](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) op Apache.org.

> [!IMPORTANT]  
> In dit voor beeld wordt Spark 2,2 op HDInsight 3,6 gebruikt.
>
> Met de stappen in dit document wordt een Azure-resourcegroep gemaakt die zowel een Spark in HDInsight- als een Kafka in HDInsight-cluster bevat. Deze clusters bevinden zich beide binnen een Azure Virtual Network, waardoor het Spark-cluster rechtstreeks kan communiceren met het Kafka-cluster.
>
> Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

## <a name="create-the-clusters"></a>De clusters maken

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-brokers via het openbare internet. Alles wat er naar Kafka praat, moet zich in hetzelfde virtuele Azure-netwerk bevindt als de knoop punten in het Kafka-cluster. In dit voor beeld bevinden zowel de Kafka-als Spark-clusters zich in een virtueel Azure-netwerk. In het volgende diagram ziet u hoe communicatie stromen tussen de clusters:

![Diagram van Spark- en Kafka-clusters in een Azure Virtual Network](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> De Kafka-service blijft beperkt tot communicatie binnen het virtuele netwerk. Andere services in het cluster, zoals SSH en Ambari, zijn toegankelijk via internet. Zie [Poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md) voor meer informatie over de openbare poorten die beschikbaar zijn voor HDInsight.

Hoewel u hand matig een Azure Virtual Network-, Kafka-en Spark-cluster kunt maken, is het eenvoudiger om een Azure Resource Manager sjabloon te gebruiken. Gebruik de volgende stappen om een Azure Virtual Network-, Kafka-en Spark-cluster te implementeren in uw Azure-abonnement.

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="./media/apache-kafka-spark-structured-streaming-cosmosdb/resource-manager-deploy.png" alt="Deploy to Azure"/>
    </a>

    De Azure Resource Manager sjabloon bevindt zich in de GitHub-opslag plaats[https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)voor dit project ().

    Met deze sjabloon maakt u de volgende bronnen:

   * Een Kafka in HDInsight 3.6-cluster.

   * Een Spark in HDInsight 3,6-cluster.

   * Een Azure Virtual Network dat de HDInsight-clusters bevat.

       > [!NOTE]  
       > Het virtuele netwerk dat door de sjabloon wordt gemaakt, maakt gebruik van de adres ruimte 10.0.0.0/16.

   * Een Azure Cosmos DB SQL API-data base.

     > [!IMPORTANT]  
     > Voor de Structured Streaming-notebook die in dit voorbeeld wordt gebruikt, is Spark in HDInsight 3.6 vereist. Als u een eerdere versie van Spark in HDInsight gebruikt, worden er fouten gegenereerd bij gebruik van de notebook.

2. Gebruik de volgende informatie om de vermeldingen in de sectie **aangepaste implementatie** in te vullen:
   
    ![Aangepaste HDInsight-implementatie](./media/apache-kafka-spark-structured-streaming-cosmosdb/hdi-custom-parameters.png)

    * **Abonnement**: Selecteer uw Azure-abonnement.
   
    * **Resourcegroep**: Een groep maken of een bestaande selecteren. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie die geografisch dicht bij u ligt.

    * **Cosmos DB account naam**: Deze waarde wordt gebruikt als de naam voor het Cosmos DB-account.

    * **Basis cluster naam**: Deze waarde wordt gebruikt als de basis naam voor de Spark-en Kafka-clusters. Als u bijvoorbeeld **myhdi** invoert, wordt een Spark-cluster met de naam __Spark-Myhdi__ en een Kafka-cluster met de naam **Kafka-myhdi**gemaakt.

    * **Cluster versie**: De versie van het HDInsight-cluster.

        > [!IMPORTANT]  
        > Dit voor beeld is getest met HDInsight 3,6 en werkt mogelijk niet met andere cluster typen.

    * **Gebruikers naam**voor het aanmelden bij een cluster: De gebruikers naam van de beheerder voor de Spark-en Kafka-clusters.

    * **Wacht woord voor cluster aanmelding**: Het gebruikers wachtwoord van de beheerder voor de Spark-en Kafka-clusters.

    * **SSH-gebruikers naam**: De SSH-gebruiker die moet worden gemaakt voor de Spark-en Kafka-clusters.

    * **SSH-wacht woord**: Het wacht woord voor de SSH-gebruiker voor de Spark-en Kafka-clusters.

3. Lees de **voorwaarden** en schakel vervolgens het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden** in.

4. Selecteer ten slotte **Inkoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

> [!IMPORTANT]  
> Het kan 45 minuten duren om de clusters, het virtuele netwerk en het Cosmos DB-account te maken.

## <a name="create-the-cosmos-db-database-and-collection"></a>De Cosmos DB-Data Base en-verzameling maken

Het project dat in dit document wordt gebruikt, slaat gegevens op in Cosmos DB. Voordat u de code uitvoert, moet u eerst een _Data Base_ en _verzameling_ maken in uw Cosmos DB-exemplaar. U moet ook het document eindpunt en de _sleutel_ ophalen die wordt gebruikt voor het verifiëren van aanvragen voor Cosmos db. 

Een manier om dit te doen is met behulp van de [Azure cli](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). `kafkadata` Met het volgende script wordt een Data Base gemaakt met de naam `kafkacollection`en een verzameling met de naam. Vervolgens wordt de primaire sleutel geretourneerd.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb database create --name $name --db-name $databaseName --resource-group $resourceGroupName
# Create the collection
az cosmosdb collection create --collection-name $collectionName --name $name --db-name $databaseName --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb list-keys --name $name --resource-group $resourceGroupName --query primaryMasterKey
```

De gegevens van het document eindpunt en de primaire sleutel zijn vergelijkbaar met de volgende tekst:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]  
> Sla het eind punt en de sleutel waarden op, zoals ze nodig zijn in de Jupyter-notebooks.

## <a name="get-the-apache-kafka-brokers"></a>De Apache Kafka Brokers ophalen

De code in dit voor beeld maakt verbinding met Kafka Broker-hosts in het Kafka-cluster. Als u de adressen van de twee Kafka Broker-hosts wilt vinden, gebruikt u het volgende Power shell-of bash-voor beeld:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds `
    -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

> [!NOTE]  
> Het bash-voor `$CLUSTERNAME` beeld verwacht de naam van het Kafka-cluster te bevatten.
>
> In dit voor beeld wordt het [JQ](https://stedolan.github.io/jq/) -hulp programma gebruikt voor het parseren van gegevens uit het JSON-document.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Wanneer u hierom wordt gevraagd, voert u het wacht woord voor het account voor cluster aanmelding (admin) in

De uitvoer lijkt op het volgende:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Sla deze informatie op, zoals deze wordt gebruikt in de volgende secties van dit document.

## <a name="get-the-notebooks"></a>De notitie blokken ophalen

De code voor het voorbeeld in dit document is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>De notebooks uploaden

Gebruik de volgende stappen om de notitie blokken van het project te uploaden naar uw Spark in HDInsight-cluster:

1. Maak vanuit uw webbrowser verbinding met de Jupyter-notebook in uw Spark-cluster. In de volgende URL vervangt u `CLUSTERNAME` door de naam van uw __Spark__-cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Typ desgevraagd de cluster-aanmelding (beheerder) en het cluster-wachtwoord die u hebt gebruikt bij het maken van het cluster.

2. Klik in de rechter bovenhoek van de pagina op de knop __uploaden__ om het bestand __-taxi-data-to-Kafka. ipynb__ te uploaden naar het cluster. Selecteer __Openen__ om te beginnen met uploaden.

3. Zoek de vermelding __Stream-taxi-data-to-Kafka. ipynb__ in de lijst met notitie blokken en Selecteer naast de knop __uploaden__ .

4. Herhaal stap 1-3 voor het laden __van de stream-data-from-Kafka-to-Cosmos-db. ipynb__ -notebook.

## <a name="load-taxi-data-into-kafka"></a>Taxi gegevens in Kafka laden

Zodra de bestanden zijn geüpload, selecteert u de vermelding __Stream-taxi-naar-Kafka. ipynb__ om het notitie blok te openen. Volg de stappen in het notitie blok om gegevens te laden in Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>De taxi-gegevens verwerken met behulp van Spark Structured streaming

Selecteer op de start pagina van [Jupyter notebook](https://jupyter.org/) de vermelding __Stream-data-from-Kafka-to-Cosmos-data base. ipynb__ . Volg de stappen in de notebook voor het streamen van gegevens van Kafka en in Azure Cosmos DB met behulp van Spark Structured streaming.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u Apache Spark Structured streaming kunt gebruiken, raadpleegt u de volgende documenten voor meer informatie over het werken met Apache Spark, Apache Kafka en Azure Cosmos DB:

* [Apache Spark streaming (DStream) gebruiken met Apache Kafka](hdinsight-apache-spark-with-kafka.md).
* [Beginnen met Jupyter Notebook en Apache Spark op HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md)
