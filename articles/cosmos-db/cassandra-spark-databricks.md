---
title: Toegang tot Azure Cosmos DB Cassandra-API vanuit Azure Databricks
description: In dit artikel wordt beschreven hoe u kunt werken met Azure Cosmos DB Cassandra-API van Azure Databricks.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 30bd3187973de204f27a3be3862351550d6a56f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85260471"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Toegang tot Azure Cosmos DB Cassandra-API gegevens vanuit Azure Databricks

In dit artikel wordt beschreven hoe u Azure Cosmos DB Cassandra-API van Spark kunt workwith op [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks).

## <a name="prerequisites"></a>Vereisten

* [Een Azure Cosmos DB Cassandra-API-account inrichten](create-cassandra-dotnet.md#create-a-database-account)

* [Bekijk de basis beginselen van het maken van verbinding met Azure Cosmos DB Cassandra-API](cassandra-spark-generic.md)

* [Een Azure Databricks cluster inrichten](../azure-databricks/quickstart-create-databricks-workspace-portal.md)

* [De code voorbeelden voor het werken met Cassandra-API bekijken](cassandra-spark-generic.md#next-steps)

* [Gebruik cqlsh voor validatie als u dat wilt](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Configuratie van Cassandra-API-exemplaar voor Cassandra-connector:**

  De connector voor Cassandra-API vereist dat de Cassandra-verbindings Details worden geïnitialiseerd als onderdeel van de Spark-context. Wanneer u een Databricks-notebook start, is de Spark-context al geïnitialiseerd en is het niet raadzaam deze te stoppen en opnieuw te initialiseren. Een oplossing is het toevoegen van de Cassandra-API-exemplaar configuratie op een cluster niveau, in de configuratie van het cluster Spark. Dit is een eenmalige activiteit per cluster. Voeg de volgende code toe aan de Spark-configuratie als een door spaties gescheiden paar sleutel waarden:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

* **Cassandra Spark-connector:** -om Azure Cosmos DB Cassandra-API te integreren met Spark, moet de Cassandra-connector zijn gekoppeld aan het Azure Databricks cluster. Het cluster koppelen:

  * Controleer de runtime versie van Databricks, de Spark-versie. Zoek vervolgens de [maven-coördinaten](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) die compatibel zijn met de Cassandra Spark-connector en koppel deze aan het cluster. Zie het artikel [een Maven-pakket uploaden of Spark-pakket](https://docs.databricks.com/user-guide/libraries.html) om de connector bibliotheek aan het cluster te koppelen. Bijvoorbeeld maven-coördinaat voor ' Databricks Runtime versie 4,3 ', ' Spark 2.3.1 ' en ' scala 2,11 ' is`spark-cassandra-connector_2.11-2.3.1`

* **Azure Cosmos DB Cassandra-API-specifieke bibliotheek:** -er is een aangepaste Connection Factory vereist voor het configureren van het beleid voor opnieuw proberen van de Cassandra Spark-connector tot Azure Cosmos DB Cassandra-API. Voeg de `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` [maven-coördinaten](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) toe om de bibliotheek aan het cluster te koppelen.

## <a name="sample-notebooks"></a>Voorbeelden van notebooks

Er is een lijst met Azure Databricks [voorbeeld notitieblokken](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/notebooks/scala) beschikbaar in github opslag plaats die u kunt downloaden. Deze voor beelden bevatten informatie over het maken van verbinding met Azure Cosmos DB Cassandra-API van Spark en het uitvoeren van verschillende ruwe bewerkingen op de gegevens. U kunt ook [alle notitie blokken](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/dbc) in uw Databricks cluster-werk ruimte importeren en uitvoeren. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Toegang tot Azure Cosmos DB Cassandra-API vanuit Spark scala Program ma's

Spark-Program ma's die moeten worden uitgevoerd als geautomatiseerde processen op Azure Databricks, worden verzonden naar het cluster door gebruik te maken van [Spark-verzen](https://spark.apache.org/docs/latest/submitting-applications.html)ding en volgens de Azure Databricks taken worden uitgevoerd.

Hieronder vindt u koppelingen waarmee u aan de slag kunt gaan met het bouwen van Spark scala-Program ma's om te communiceren met Azure Cosmos DB Cassandra-API.
* [Verbinding maken met Azure Cosmos DB Cassandra-API vanuit een Spark scala-programma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Een Spark scala-programma uitvoeren als een geautomatiseerde taak op Azure Databricks](https://docs.azuredatabricks.net/user-guide/jobs.html)
* [Volledige lijst met code voorbeelden voor het werken met Cassandra-API](cassandra-spark-generic.md#next-steps)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met het [maken van een Cassandra-API-account,-data base en een tabel](create-cassandra-api-account-java.md) met behulp van een Java-toepassing.
