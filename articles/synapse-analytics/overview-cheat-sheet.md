---
title: 'Cheatsheet: Azure Synapse Analytics (preview van werkruimten)'
description: Naslaggids waarmee de gebruiker Azure Synapse Analytics leert kennen
services: synapse-analytics
author: saveenr
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: saveenr
ms.reviewer: jrasnick
ms.openlocfilehash: c8ae47f89dcddd2253abd21239eb1a9bffee27c7
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2020
ms.locfileid: "93307819"
---
# <a name="azure-synapse-analytics-cheat-sheet"></a>Cheatsheet voor Azure Synapse Analytics

[!INCLUDE [preview](includes/note-preview.md)]

Het cheatsheet voor Azure Synapse Analytics bevat een overzicht van de basisconcepten en belangrijke opdrachten van de service. Dit artikel is nuttig voor zowel nieuwe gebruikers als mensen die meer over belangrijke Azure Synapse-onderwerpen willen weten.

## <a name="basics"></a>Basisbeginselen

Een **Synapse-werkruimte** is een beveiligbare samenwerkingsgrens voor het uitvoeren van zakelijke cloudanalyses in Azure. Een werkruimte wordt geïmplementeerd in een specifieke regio en heeft een gekoppeld ADLS Gen2-account en -bestandssysteem (voor het opslaan van tijdelijke gegevens). Een werkruimte bevindt zich onder een resourcegroep.

Met een werkruimte kunt u analyses uitvoeren met SQL en Apache Spark. Resources die beschikbaar zijn voor SQL en Spark Analytics, zijn georganiseerd in SQL- en Spark- **pools**. 

## <a name="synapse-sql"></a>Synapse SQL

**Synapse SQL** is de mogelijkheid om op T-SQL gebaseerde analyses uit te voeren in een Synapse-werkruimte. Synapse SQL heeft twee verbruiksmodellen: toegewezen en serverloos.  Gebruik **toegewezen SQL-pools** voor het toegewezen model. Een werkruimte kan elk gewenst aantal pools bevatten. Gebruik de **serverloze SQL-pools** als u het serverloze model wilt gebruiken. Elke werkruimte heeft een van deze pools.

## <a name="apache-spark-for-synapse"></a>Apache Spark for Synapse

Maak en gebruik **serverloze Apache Spark-pools** in uw Synapse-werkruimte als u Spark-analyses wilt gebruiken.

## <a name="sql-terminology"></a>SQL-terminologie
| Termijn                         | Definitie      |
|:---                                 |:---                 |
| **SQL-aanvraag**  |   Bewerking zoals een queryuitvoering via een toegewezen SQL-pool of serverloze SQL-pool. |

## <a name="spark-terminology"></a>Spark-terminologie
| Termijn                         | Definitie      |
|:---                                 |:---                 |
|**Apache Spark for Synapse** | Spark-runtime die wordt gebruikt in een serverloze Spark-pool. De huidige versie die wordt ondersteund is Spark 2.4 met python 3.6.1, scala 2.11.12, .NET-ondersteuning voor Apache Spark 0.5 en Delta Lake 0.3.  | 
| **Apache Spark-pool**  | 0-to-N in Spark ingerichte resources en de bijbehorende databases kunnen in een werkruimte worden geïmplementeerd. Een Spark-pool kan automatisch worden onderbroken, hervat en geschaald.  |
| **Spark-toepassing**  |   Het bestaat uit een stuurprogrammaproces en een reeks uitvoeringsprocedures. Een Spark-toepassing wordt uitgevoerd in een serverloze Spark-pool.            |
| **Spark-sessie**  |   Uniform invoerpunt van een Spark-toepassing. Een sessie biedt een manier om te communiceren met de verschillende functies van Spark en met een kleiner aantal constructs. Als u een notebook wilt uitvoeren, moet u een sessie maken. Een sessie kan worden geconfigureerd om te worden uitgevoerd op een specifiek aantal uitvoerders van een specifiek formaat. De standaardconfiguratie voor een notebook-sessie is om op twee middelgrote uitvoerders te worden uitgevoerd. |
|**Gegevensintegratie**| Biedt de mogelijkheid om gegevens op te nemen in verschillende bronnen en activiteiten te organiseren die worden uitgevoerd binnen of buiten een werkruimte.| 
|**Artefacten**| Een concept dat alle objecten omvat die een gebruiker nodig heeft om gegevensbronnen te beheren, te ontwikkelen, te organiseren en te visualiseren.|
|**Notebook**| Interactieve en reactieve interface voor gegevenswetenschap en engineering die ondersteuning biedt voor Scala, PySpark, C# en SparkSQL. |
|**Spark-taakdefinitie**|Interface voor het verzenden van een Spark-taak met een assembly-JAR die de code en de bijbehorende afhankelijkheden bevat.|
|**Gegevensstroom**|  Biedt een volledig visuele ervaring zonder codering om big data-transformatie uit te voeren. Alle optimalisatie- en uitvoeringstaken worden op serverloze wijze afgehandeld. |
|**SQL-script**| Een set met SQL-opdrachten die zijn opgeslagen in een bestand. Een SQL-script kan een of meer SQL-instructies bevatten. Het kan worden gebruikt om SQL-aanvragen uit te voeren via een toegewezen SQL-pool of een serverloze SQL-pool.|
|**Pijplijn**| Een logische groep activiteiten die samen een taak uitvoeren.|
|**Activiteit**| Staat voor acties die moeten worden uitgevoerd op gegevens, zoals het kopiëren van gegevens of het uitvoeren van een notebook of een SQL-script.|
|**Trigger**| Voert een pijplijn uit. Deze kan handmatig of automatisch worden uitgevoerd (planning, tumblingvenster of op gebeurtenis gebaseerd).|
|**Gekoppelde service**| Verbindingsreeksen waarmee de verbindingsgegevens worden gedefinieerd die nodig zijn om de werkruimte aan externe resources te koppelen.|
|**Gegevensset**|  Een weergave van gegevens met een naam die simpelweg verwijst naar de gegevens die in een activiteit moeten worden gebruikt als invoer en uitvoer. Deze hoort bij een gekoppelde service.|

## <a name="next-steps"></a>Volgende stappen

- [Een werkruimte maken](quickstart-create-workspace.md)
- [Synapse Studio gebruiken](quickstart-synapse-studio.md)
- [Een toegewezen SQL-pool maken](quickstart-create-sql-pool-portal.md)
- [Een serverloze Apache Spark-pool maken](quickstart-create-apache-spark-pool-portal.md)
- [Serverloze SQL-pools gebruiken](quickstart-sql-on-demand.md)

