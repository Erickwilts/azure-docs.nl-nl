---
title: 'Veelgestelde vragen: Azure Synapse Analytics (preview-versie van werkruimten)'
description: Veelgestelde vragen over Azure Synapse Analytics (preview-versie van werkruimten)
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: ba6f79fffe5287be7574d422f026489d4da2795e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287507"
---
# <a name="azure-synapse-analytics-workspaces-preview-frequently-asked-questions"></a>Veelgestelde vragen over Azure Synapse Analytics (preview-versie van werkruimten)

In deze handleiding vindt u de veelgestelde vragen over Synapse Analytics.

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="general"></a>Algemeen

### <a name="q-what-is-azure-synapse-analytics"></a>V: Wat is Azure Synapse Analytics

A: Azure Synapse is een geïntegreerd gegevensplatform voor BI, AI en doorlopende informatie. Het verbindt verschillende analyse-runtimes, zoals SQL en Spark, via één platform dat een uniforme manier biedt om:

- Beveilig uw analyseresources, met inbegrip van het netwerk, het beheren van de toegang via eenmalige aanmelding voor de groeps-, gegevens-, en ontwikkelingsartefacten.
- U kunt eenvoudig en gebeurtenissen in uw werkruimte-activiteiten bewaken en snel optimaliseren, reageren en fouten opsporen die zich op elke laag voordoen.
- Uw metagegevens voor alle engines beheren. Maak een Spark-tabel en deze is automatisch beschikbaar in uw Azure Synapse-databases.
- Interactie met de gegevens via een uniforme gebruikerservaring. Synapse Studio brengt Big data-ontwikkelaars, gegevenstechnici, DBA's, gegevensanalisten en gegevenswetenschappers samen op hetzelfde platform.

Zie [Wat is Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/overview-what-is) voor meer informatie.

### <a name="q-what-are-the-main-components-of-azure-synapse-analytics"></a>V: Wat zijn de belangrijkste onderdelen van Azure Synapse Analytics

A: Azure Synapse biedt de volgende mogelijkheden:

- Analyse-mogelijkheden worden aangeboden via SQL-pool of SQL on-demand (preview) (Serverloos).
- Apache Spark-pool (preview) met volledige ondersteuning voor Scala, Python, SparkSQL en C#
- Gegevensstroom biedt een transformatie-ervaring voor big data zonder code
- Gegevensintegratie en -indeling om uw gegevens te integreren en al uw code-ontwikkeling operationeel te maken
- Studio om toegang te krijgen tot al deze mogelijkheden via één Web-UI

### <a name="q-how-does-azure-synapse-analytics-relate-to-azure-sql-data-warehouse"></a>V: Wat is de relatie tussen Azure Synapse Analytics en Azure SQL Data Warehouse

A: Azure Synapse Analytics is een evolutie van Azure SQL Data Warehouse naar een analyseplatform, waaronder SQL-pool als de Data Warehouse-oplossing. Dit platform combineert gegevensverkenning, opname, transformatie, voorbereiding en een analyse-laag voor services.

## <a name="use-cases"></a>Gebruiksvoorbeelden

### <a name="q-how-do-i-rename-a-published-artifact-dataset-notebook-sql-script-and-so-on-in-azure-synapse"></a>V: Hoe kan ik de naam wijzigen van een gepubliceerd artefact (gegevensset, notebook, SQL-script enzovoort) in Azure Synapse?

A: Als u de naam wilt wijzigen van een gepubliceerd artefactbestand, moet u eerst het bestand klonen en de naam van het nieuwe bestand wijzigen in de gewenste bestandsnaam. U moet alle verwijzingen van het artefact handmatig bijwerken naar de nieuwe bestandsnaam en de oude verwijderen.

### <a name="q-what-is-a-good-use-case-for-synapse-sql-pool"></a>V: Wat is een goed gebruiksvoorbeeld voor een Synapse SQL-pool

A: SQL-pool is de kern van uw datawarehouse-behoeften. Het is de toonaangevende datawarehouse-oplossing zowel in [prijs en prestatie](https://azure.microsoft.com/services/sql-data-warehouse/compare/). SQ-pool is de toonaangevende oplossing voor Cloud Data Warehouse, omdat u het volgende kunt doen:

- een grote en gemengde workload hebben zonder invloed op de prestaties dankzij een hoge isolatie van gelijktijdigheid- en workload
- Beveilig uw gegevens eenvoudig via geavanceerde functies, variërend van netwerkbeveiliging tot nauwkeurige toegang
- Profiteer van een breed scala aan eco-systemen

### <a name="q-what-is-a-good-use-case-for-spark-in-synapse"></a>V: Wat is een goed gebruiksvoorbeeld voor Spark in Synapse

A: Ons eerste doel is om een goede data engineering te bieden voor het transformeren van gegevens over de data lake in batch of stream. Dankzij de naadloze en eenvoudige integratie met onze gegevensindeling wordt de uitoefening van uw ontwikkelingswerk duidelijk.

Een ander gebruiksvoorbeeld voor Spark is voor een gegevenswetenschapper om het volgende te doen:

- een functie extraheren
- gegevens verkennen
- een model trainen

### <a name="q-what-is-a-good-use-case-for-sql-on-demand-in-synapse"></a>V: Wat is een goed gebruiksvoorbeeld voor SQL on-demand in Synapse

A: SQL on-demand is een queryservice over de gegevens in uw data lake. Hiermee kunt u democratische toegang tot al uw gegevens instellen door een bekende T-SQL-syntaxis te bieden voor het opvragen van gegevens, zonder dat u gegevens naar een gespecialiseerde store hoeft te kopiëren of te laden.

Gebruiksvoorbeelden zijn onder andere:

- basisdetectie en -ontdekking: biedt gegevensanalisten, opkomende gegevenswetenschappers en gegevenstechnici met een eenvoudig pad naar het eerste inzicht in gegevens in hun data lake met schema over lees-T-SQL-query's
- logische data warehouse: gegevensanalisten kunnen volledige duidelijkheid van de T-SQL-taal uitvoeren om de gegevens die zich in Azure Storage bevinden, rechtstreeks te doorzoeken en te analyseren en vertrouwde BI-hulpprogramma's (zoals Azure Analyses Services, Power BI Premium enz.) te gebruiken om dashboards te vernieuwen door Starlight Query-query's opnieuw uit te voeren
- "ETL van één query". Hiermee kunnen gegevenstechnici Azure Storage op basis van gegevens van de ene indeling naar een andere transformeren, filteren, aggregeren, enzovoort, op zeer parallelle verwerkingswijze de query resultaten persistent maken voor Azure Storage en deze onmiddellijk beschikbaar stellen voor verdere verwerking in Starlight Query of andere services

### <a name="q-what-is-a-good-use-case-for-data-flow-in-synapse"></a>V: Wat is een goed gebruiksvoorbeeld voor gegevensstroom in Synapse

A: Met gegevensstroom kunnen data engineers grafische gegevenstransformatie-logica ontwikkelen zonder code te schrijven. De resulterende gegevensstromen worden uitgevoerd als activiteiten binnen de gegevensintegratie en -indeling. Activiteiten voor gegevensstromen kunnen operationeel worden gemaakt via bestaande mogelijkheden voor planning, besturingselementen, stroom en bewaking.

## <a name="security-and-access"></a>Beveiliging en toegang

A: Een end-to-end-ervaring voor eenmalige aanmelding is een belangrijk verificatieproces in Synapse Analytics. Het beheren en doorgeven van de identiteit door middel van een volledige AAD-integratie is essentieel.

### <a name="q-how-do-i-get-access-to-files-and-folders-in-the-adls-gen2"></a>V: Hoe kan ik toegang krijgen tot bestanden en mappen in de ADLS Gen2

A: Toegang tot bestanden en mappen wordt momenteel beheerd via ADLS Gen2. Zie [Toegangsbeheer voor Data Lake Storage](../storage/blobs/data-lake-storage-access-control.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) voor meer informatie.

### <a name="q-can-i-use-third-party-business-intelligence-tools-to-access-azure-synapse-analytics"></a>V: Kan ik hulpprogramma's voor business intelligence van derden gebruiken om toegang te krijgen tot Azure Synapse Analytics

A: Ja, u kunt externe zakelijke toepassingen gebruiken, zoals Tableau en Power BI, om verbinding te maken met een SQL-pool en een on-demand SQL-database. Spark ondersteunt IntelliJ.

## <a name="next-steps"></a>Volgende stappen

- [Een werkruimte maken](quickstart-create-workspace.md)
- [Synapse Studio gebruiken](quickstart-synapse-studio.md)
- [Een SQL-pool maken](quickstart-create-sql-pool-portal.md)
- [SQL on-demand gebruiken](quickstart-sql-on-demand.md)
- [Een Apache Spark-pool maken](quickstart-create-apache-spark-pool-portal.md) 
