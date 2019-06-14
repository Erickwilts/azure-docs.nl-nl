---
title: Gegevens laden in omgevingen met Azure storage - Team Data Science Process
description: Gegevens verplaatsen van en naar Azure Blob-opslag
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 56c45bf6db79ded64574a44399712951e82c1c3e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303584"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Gegevens voor analysedoeleinden in opslagomgevingen laden

Het Team Data Science Process is vereist dat gegevens worden opgenomen of geladen in tal van verschillende opslagomgevingen worden verwerkt of geanalyseerd in de meest geschikte manier in elke fase van het proces. Gegevensbestemmingen gebruikte voor de verwerking van bevatten Azure Blob Storage, SQL Azure-databases, SQL Server op virtuele machine van Azure, HDInsight (Hadoop) en Azure Machine Learning. 

De volgende artikelen wordt beschreven hoe u gegevens opnemen in verschillende doelomgevingen waar de gegevens worden opgeslagen en verwerkt.

* Naar/van [Azure Blob-opslag](move-azure-blob.md)
* Naar [SQL Server op Azure VM](move-sql-server-virtual-machine.md)
* Naar [Azure SQL-database](move-sql-azure.md)
* Naar [Hive-tabellen](move-hive-tables.md)
* Naar [SQL gepartitioneerde tabellen](parallel-load-sql-partitioned-tables.md)
* Van [On-premises SQL Server](move-sql-azure-adf.md)

Formatteer technische en zakelijke behoeften, evenals de oorspronkelijke locatie en grootte van uw gegevens, bepaalt de doelomgevingen waarin de gegevens worden opgenomen moeten voor het bereiken van de doelstellingen van uw analyse. Het is niet ongebruikelijk dat voor een scenario dat gegevens worden verplaatst tussen verschillende omgevingen voor de verschillende taken die zijn vereist om te maken van een Voorspellend model. Deze reeks taken kan bijvoorbeeld bevatten gegevensverkenning, vooraf verwerken, opruimen, down-sampling en modeltraining.
