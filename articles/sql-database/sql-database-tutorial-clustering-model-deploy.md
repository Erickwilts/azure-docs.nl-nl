---
title: 'Zelf studie: een cluster model implementeren in R'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In deel drie van deze serie met drie delen zelf studies implementeert u een cluster model in R met Azure SQL Database Machine Learning Services (preview).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ef478246108d40a0c97d7dab03ecf1e5b474410b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81452890"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Zelf studie: een cluster model implementeren in R met Azure SQL Database Machine Learning Services (preview)

In deel drie van deze reeks met drie zelf studies implementeert u een cluster model dat is ontwikkeld in R, in een SQL database met behulp van Azure SQL Database Machine Learning Services (preview).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

U maakt een opgeslagen procedure met een Inge sloten R-script dat clustering uitvoert. Omdat uw model wordt uitgevoerd in Azure SQL database, kan het eenvoudig worden getraind op basis van gegevens die zijn opgeslagen in de data base.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een opgeslagen procedure maken waarmee het model wordt gegenereerd
> * Clustering in SQL Database uitvoeren
> * De cluster informatie gebruiken

In [deel één](sql-database-tutorial-clustering-model-prepare-data.md)hebt u geleerd hoe u de gegevens van een Azure-SQL database voorbereidt voor het uitvoeren van een cluster.

In [deel twee](sql-database-tutorial-clustering-model-build.md)hebt u geleerd hoe u een K-cluster model maakt en traint in R.

## <a name="prerequisites"></a>Vereisten

* In deel drie van deze zelf studie wordt ervan uitgegaan dat u [**deel één**](sql-database-tutorial-clustering-model-prepare-data.md) en [**deel twee**](sql-database-tutorial-clustering-model-build.md)hebt voltooid.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Een opgeslagen procedure maken waarmee het model wordt gegenereerd

Voer het volgende T-SQL-script uit om de opgeslagen procedure te maken. Met de procedure worden de stappen die u in delen één en twee van deze zelfstudie reeks hebt ontwikkeld, opnieuw gemaakt:

* klanten classificeren op basis van hun aankoop-en retour geschiedenis
* vier klanten clusters genereren met behulp van een K-betekent-algoritme

In de procedure worden de resulterende klant cluster toewijzingen opgeslagen in de database tabel **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string
connStr <- paste("Driver=SQL Server; Server=", instance_name,
               "; Database=", database_name,
               "; Trusted_Connection=true; ",
                  sep="" );

# Input customer data that needs to be classified.
# This is the result we get from the query.
customer_returns <- RxSqlServerData(
                     sqlQuery=input_query,
                     colClasses=c(customer ="numeric",
                                  orderRatio="numeric",
                                  itemsRatio="numeric",
                                  monetaryRatio="numeric",
                                  frequency="numeric" ),
                     connectionString=connStr);
# Output table to hold the customer cluster mappings
return_cluster = RxSqlServerData(table = "customer_return_clusters",
                                 connectionString = connStr);

# Set seed for random number generator for predictability
set.seed(10);

# Generate clusters using rxKmeans and output clusters to a table
# called "customer_return_clusters"
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters = 4,
                   outFile = return_cluster,
                   outColName = "cluster",
                   writeModelVars = TRUE ,
                   extraVarsToWrite = c("customer"),
                   overwrite = TRUE);
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering-in-sql-database"></a>Clustering in SQL Database uitvoeren

Nu u de opgeslagen procedure hebt gemaakt, voert u het volgende script uit om clustering uit te voeren.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Controleer of het werkt en of we de lijst met klanten en hun cluster toewijzingen werkelijk hebben.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>De cluster informatie gebruiken

Omdat u de clustering procedure in de-Data Base hebt opgeslagen, kan deze efficiënt worden geclusterd op basis van klant gegevens die zijn opgeslagen in dezelfde data base. U kunt de procedure uitvoeren wanneer uw klant gegevens worden bijgewerkt en de bijgewerkte cluster informatie gebruiken.

Stel dat u een promotie-e-mail wilt verzenden naar klanten in cluster 3, de groep met meer actief retour gedrag (u kunt zien hoe de vier clusters in [deel twee](sql-database-tutorial-clustering-model-build.md#analyze-the-results)zijn beschreven). Met de volgende code worden de e-mail adressen van klanten in cluster 3 geselecteerd.

```sql
USE [tpcxbb_1gb]

SELECT customer.[c_email_address],
    customer.c_customer_sk
FROM dbo.customer
JOIN [dbo].[customer_return_clusters] AS r ON r.customer = customer.c_customer_sk
WHERE r.cluster = 3
```

U kunt de waarde **r. cluster** wijzigen om e-mail adressen voor klanten in andere clusters te retour neren.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze zelf studie, kunt u de tpcxbb_1gb-data base verwijderen van de Azure SQL Database-Server.

Voer de volgende stappen uit op de Azure Portal:

1. Selecteer in het menu aan de linkerkant in het Azure Portal **alle resources** of **SQL-data bases**.
1. In het veld **filteren op naam...** voert u **tpcxbb_1gb**in en selecteert u uw abonnement.
1. Selecteer uw **tpcxbb_1gb** -data base.
1. Selecteer **Verwijderen** op de pagina **Overzicht**.

## <a name="next-steps"></a>Volgende stappen

In deel drie van deze reeks zelf studies hebt u de volgende stappen uitgevoerd:

* Een opgeslagen procedure maken waarmee het model wordt gegenereerd
* Clustering in SQL Database uitvoeren
* De cluster informatie gebruiken

Zie voor meer informatie over het gebruik van R in Azure SQL Database Machine Learning Services (preview):

* [Zelf studie: gegevens voorbereiden om een voorspellend model in R te trainen met Azure SQL Database Machine Learning Services (preview-versie)](sql-database-tutorial-predictive-model-prepare-data.md)
* [Geavanceerde R-functies schrijven in Azure SQL Database met behulp van Machine Learning Services (preview)](sql-database-machine-learning-services-functions.md)
* [Werken met R-en SQL-gegevens in Azure SQL Database Machine Learning Services (preview-versie)](sql-database-machine-learning-services-data-issues.md)
* [Een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview-versie)](sql-database-machine-learning-services-add-r-packages.md)
