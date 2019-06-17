---
title: Toegang tot Azure Cosmos DB Cassandra-API van Azure Databricks
description: In dit artikel bevat informatie over het werken met Azure Cosmos DB Cassandra-API van Azure Databricks.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 37a06b19285c1196b5d87830ea176d4bd0d4eade
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60894008"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Toegang tot Azure Cosmos DB Cassandra API gegevens uit Azure Databricks

Dit artikel wordt beschreven hoe u in combinatie met Azure Cosmos DB Cassandra-API uit Spark op [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks).

## <a name="prerequisites"></a>Vereisten

* [Inrichten van een Cassandra-API van Azure Cosmos DB-account](create-cassandra-dotnet.md#create-a-database-account)

* [Bekijk de basisbeginselen van het verbinden met Azure Cosmos DB Cassandra-API](cassandra-spark-generic.md)

* [Een Azure Databricks-cluster inrichten](../azure-databricks/quickstart-create-databricks-workspace-portal.md)

* [Bekijk de voorbeelden van code voor het werken met de Cassandra-API](cassandra-spark-generic.md#next-steps)

* [Cqlsh voor validatie gebruiken als u dus liever](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Configuratie van de Cassandra-API-exemplaar voor Cassandra-connector:**

  De connector voor Cassandra-API is vereist voor de details van de Cassandra-verbinding moet worden geïnitialiseerd als onderdeel van de spark-context. Wanneer u een Databricks-notebook start, wordt de spark-context is al geïnitialiseerd en wordt niet aangeraden om te stoppen en opnieuw initialiseren. Een mogelijke oplossing is het toevoegen van de configuratie van de Cassandra-API-exemplaren op het clusterniveau van een, in de configuratie van spark. Dit is een eenmalige activiteit per cluster. Voeg de volgende code aan de configuratie van Spark als een spatie sleutel-waardepaar gescheiden:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>De vereiste afhankelijkheden toevoegen

* **Cassandra-Spark-connector:** - Cassandra-API van Azure Cosmos DB worden geïntegreerd met Spark, de connector moet worden gekoppeld aan het Azure Databricks-cluster Cassandra. Koppelen van het cluster:

  * Bekijk de Databricks-runtimeversie de Spark-versie. Gaat u naar de [maven-coördinaten](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) die compatibel zijn met de Cassandra-Spark-connector, en deze koppelen aan het cluster. Zie ["Voor het uploaden van een Maven-pakket of een Spark-pakket"](https://docs.databricks.com/user-guide/libraries.html) artikel de connector-bibliotheek koppelen aan het cluster. Bijvoorbeeld, maven-coördinaat voor "Databricks Runtime versie 4.3", "2.3.1 Spark," en "Scala 2.11" is `spark-cassandra-connector_2.11-2.3.1`

* **Azure Cosmos DB Cassandra-API-specifieke bibliotheek:** -een aangepaste verbinding factory is vereist voor het configureren van het beleid voor opnieuw proberen van de Cassandra-Spark-connector met Azure Cosmos DB Cassandra-API. Voeg de `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` [maven-coördinaten](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) de bibliotheek koppelen aan het cluster.

## <a name="sample-notebooks"></a>Voorbeelden van notebooks

Een lijst met Azure Databricks [voorbeeld notitieblokken](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/notebooks/scala) zijn beschikbaar in de GitHub-opslagplaats voor u om te downloaden. Deze voorbeelden bevatten informatie over het verbinding maken met Azure Cosmos DB Cassandra-API van Spark en andere CRUD-bewerkingen op de gegevens uitvoeren. U kunt ook [importeren van alle notitieblokken](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/dbc) cluster naar uw Databricks-werkruimte en de App uitvoeren. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Toegang tot Azure Cosmos DB Cassandra-API van Spark Scala-programma 's

Spark-programma's om te worden uitgevoerd, zoals geautomatiseerde processen op Azure Databricks worden verzonden naar het cluster met behulp van [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html)) en geplande om uit te voeren via de Azure Databricks-taken.

Hieronder vindt u koppelingen om u te helpen aan de slag voor het bouwen van Spark Scala-programma's om te communiceren met de Cassandra-API van Azure Cosmos DB.
* [Verbinding maken met Azure Cosmos DB Cassandra-API van een Spark Scala-programma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Een Spark Scala-programma als een geautomatiseerde taak uitvoeren op Azure Databricks](https://docs.azuredatabricks.net/user-guide/jobs.html)
* [Volledige lijst met voorbeelden van code voor het werken met de Cassandra-API](cassandra-spark-generic.md#next-steps)

## <a name="next-steps"></a>Volgende stappen

Ga aan de slag met het [maken van een Cassandra-API-account, -database en een tabel](create-cassandra-api-account-java.md) met behulp van een Java-toepassing.
