---
title: Azure Stream Analytics gebruiken met SQL datawarehouse | Microsoft Docs
description: Tips voor het gebruik van Azure Stream Analytics met Azure SQL Data Warehouse om oplossingen te ontwikkelen.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 03/22/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 94646c41d9894dd00018ff5ca44d76534d35e8c5
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873266"
---
# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure Stream Analytics gebruiken met SQL datawarehouse
Azure Stream Analytics is een volledig beheerde service met lage latentie, maximaal beschikbare, schaalbare complexe verwerking van gebeurtenissen die via het streamen van gegevens in de cloud. U leert de beginselen door te lezen [Inleiding tot Azure Stream Analytics][Introduction to Azure Stream Analytics]. U kunt vervolgens informatie over het maken van een end-to-end-oplossing met Stream Analytics door de volgende de [aan de slag met Azure Stream Analytics] [ Get started using Azure Stream Analytics] zelfstudie.

In dit artikel leert u hoe u uw Azure SQL Data Warehouse-database gebruikt als een uitvoerlocatie voor uw Stream Analytics-taken.

## <a name="prerequisites"></a>Vereisten
Voer eerst via de volgende stappen uit in de [aan de slag met Azure Stream Analytics] [ Get started using Azure Stream Analytics] zelfstudie.  

1. Maak een Event Hub
2. Configureren en gebeurtenisgenerator te starten
3. Inrichten van een Stream Analytics-taak
4. Taakinvoer en query opgeven

Vervolgens maakt u een Azure SQL Data Warehouse-database

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Geef de uitvoer: Azure SQL Data Warehouse database
### <a name="step-1"></a>Stap 1
Klik in de Stream Analytics-taak op **uitvoer** vanaf de bovenkant van de pagina en klik vervolgens op **toevoegen**.

### <a name="step-2"></a>Stap 2
Selecteer de SQL-Database.

### <a name="step-3"></a>Stap 3
Voer de volgende waarden op de volgende pagina:

* *Uitvoer van de Alias*: Voer een beschrijvende naam voor de taakuitvoer van deze.
* *Abonnement*:
  * Als uw SQL Data Warehouse-database zich in hetzelfde abonnement als de Stream Analytics-taak, selecteert u met SQL Database van het huidige abonnement.
  * Als uw database in een ander abonnement, selecteert u SQL-Database gebruiken vanuit een ander abonnement.
* *Database*: Geef de naam van een doeldatabase.
* *De naam van server*: Geef de naam van de server voor de database die u zojuist hebt opgegeven. De Azure-portal kunt u dit vinden.

![][server-name]

* *Gebruikersnaam*: Geef de gebruikersnaam van een account met schrijfmachtigingen heeft voor de database.
* *Wachtwoord*: Geef het wachtwoord voor het opgegeven gebruikersaccount.
* *tabel*: Geef de naam van de doeltabel in de database.

![][add-database]

### <a name="step-4"></a>Stap 4
Klik op de knop controleren om toe te voegen van de taakuitvoer van deze en te controleren dat Stream Analytics verbinding met de database maken kan.

Wanneer de verbinding met de database is gelukt, ziet u een melding in de portal. U kunt klikken op testen om te testen van de verbinding met de database.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de integratie van [overzicht van SQL Data Warehouse-integratie][SQL Data Warehouse integration overview].

Zie [Overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview] voor meer tips voor ontwikkelaars.

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introduction to Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Get started using Azure Stream Analytics]: ../stream-analytics/stream-analytics-real-time-fraud-detection.md
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: https://azure.microsoft.com/documentation/services/stream-analytics/
