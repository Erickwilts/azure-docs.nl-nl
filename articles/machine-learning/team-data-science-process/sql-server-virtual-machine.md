---
title: Gegevens verkennen in een computer met SQL Server-machine - Team Data Science Process
description: Gegevens verkennen en genereren van functies in een SQL Server-machine op Azure
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/23/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6adc5dfa740d440e78bf2f276447c4585503d7c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606466"
---
# <a name="heading"></a>Gegevens verwerken in SQL Server-Machine op Azure
Dit document wordt uitgelegd hoe u gegevens verkennen en functies voor gegevens die zijn opgeslagen in een SQL Server-VM op Azure te genereren. Dit kan worden gedaan door data wrangling met behulp van SQL of met behulp van een programmeertaal zoals Python.

> [!NOTE]
> De voorbeeld-SQL-instructies in dit document wordt ervan uitgegaan dat gegevens in SQL Server. Als dit niet is, kunt u verwijzen naar de cloud data science process kaart voor informatie over het verplaatsen van uw gegevens naar SQL Server.
> 
> 

## <a name="SQL"></a>Met behulp van SQL
We beschrijven de volgende data wrangling taken in deze sectie met behulp van SQL:

1. [Gegevens verkennen](#sql-dataexploration)
2. [Functie genereren](#sql-featuregen)

### <a name="sql-dataexploration"></a>Gegevens verkennen
Hier volgen enkele voorbeelden SQL-scripts die kunnen worden gebruikt voor het verkennen van SQL Server voor gegevensopslag.

> [!NOTE]
> Voor een voorbeeld, kunt u de [NYC Taxi gegevensset](https://www.andresmh.com/nyctaxitrips/) en verwijzen naar de met de titel IPNB [NYC Data wrangling met behulp van IPython Notebook en SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) voor een overzicht van end-to-end.
> 
> 

1. Het aantal opmerkingen per dag
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. De niveaus in een categorische kolom ophalen
   
    `select  distinct <column_name> from <databasename>`
3. Het aantal niveaus in de combinatie van twee categorische kolommen ophalen 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. De verdeling van de numerieke kolommen ophalen
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="sql-featuregen"></a>Functie genereren
In deze sectie wordt beschreven manieren voor het genereren van functies met behulp van SQL:  

1. [Aantal op basis van functie genereren](#sql-countfeature)
2. [Genereren van de functie binning](#sql-binningfeature)
3. [Implementeren van de functies van één kolom](#sql-featurerollout)

> [!NOTE]
> Nadat u extra functies genereert, kunt u ze als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die kan worden samengevoegd met de oorspronkelijke tabel. 
> 
> 

### <a name="sql-countfeature"></a>Aantal op basis van functie genereren
De volgende voorbeelden ziet twee manieren voor het genereren van het aantal functies. De eerste methode maakt gebruik van Voorwaardelijke som en de tweede methode maakt gebruik van de component 'where'. Deze kunnen vervolgens worden samengevoegd met de oorspronkelijke tabel (met behulp van de primaire-sleutelkolommen) als u wilt dat het aantal functies samen met de oorspronkelijke gegevens.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="sql-binningfeature"></a>Genereren van de functie binning
Het volgende voorbeeld laat zien hoe voor het genereren van binned functies door binning (met behulp van vijf opslaglocaties) een numerieke kolom die kan worden gebruikt als een functie in plaats daarvan:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Implementeren van de functies van één kolom
In deze sectie laten we zien hoe u één kolom in een tabel voor het genereren van extra functies worden uitgerold. Het voorbeeld wordt ervan uitgegaan dat er een kolom voor breedtegraad of lengtegraad in de tabel waaruit u wilt genereren van functies.

Hier volgt een korte uitleg van breedtegraad/lengtegraad locatiegegevens (resources voorzien van stackoverflow [hoe u voor het meten van de nauwkeurigheid van de breedtegraad en lengtegraad?](https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dit is handig om te begrijpen voordat featurizing het locatieveld:

* De aanmelding kan worden achterhaald of we Noord of -Zuid, Oost- of -west op de hele wereld.
* Een andere waarde dan nul honderden cijfers kan worden achterhaald dat we niet breedtegraad lengtegraad gebruiken!
* De tientallen cijfer biedt een positie naar ongeveer 1000 kilometer zijn verwijderd. Deze geeft ons voor nuttige informatie over welke continent of in de Indische Oceaan worden op.
* De eenheden cijfers (één decimaal graad) biedt een positie tot 111 kilometer (60 zeemijl, ongeveer 69 mijl). Het kan ons ongeveer welke grote staat of land/regio, zijn we in.
* De eerste decimaal de moeite waard tot 11.1 km is: het de positie van een grote plaats van een nabijgelegen grote plaats kunt onderscheiden.
* De tweede decimaalpositie de moeite waard tot 1.1 km is: kan het één village scheiden van de volgende.
* De derde decimaal de moeite waard is maximaal 110 m: dat het een grote agricultural veld of institutionele campus kunt identificeren.
* De vierde decimaal de moeite waard is maximaal 11 m: dat er een pakket van de grond kunt identificeren. Het is vergelijkbaar met de typische nauwkeurigheid van een niet-gecorrigeerde GPS-eenheid zonder storing.
* De vijfde decimaal de moeite waard is tot 1.1 m: dat deze structuren van elkaar worden onderscheiden. Nauwkeurigheid van de gegevens op dit niveau met commerciële GPS-eenheden kan alleen worden bereikt met differentiële correctie.
* De zesde decimaal is waard tot 0.11 m: die u kunt deze gebruiken voor het opmaken van structuren in detail voor het ontwerpen van landschappen, het bouwen van wegen. Dit moet meer dan goed genoeg voor het bijhouden van bewegingen van glaciers en rivieren. Dit kan worden bereikt door middel van hele maatregelen met GPS, zoals differentially gecorrigeerde GPS.

De locatie-informatie kan worden boommodel als volgt scheiden van regio, locatie en plaats. Houd er rekening mee dat u ook een REST-eindpunt, zoals Bing kaarten-API beschikbaar kunt aanroepen [zoeken naar een locatie met punt](https://msdn.microsoft.com/library/ff701710.aspx) voor de regio/district-informatie.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Deze functies op basis van locatie kunnen verder worden gebruikt voor het genereren van aantal extra functies, zoals eerder beschreven. 

> [!TIP]
> U kunt de records die met de taal van keuze programmatisch invoegen. U moet mogelijk om in te voegen van de gegevens in segmenten schrijven efficiëntie te verbeteren (Zie voor een voorbeeld van hoe u dit doet met pyodbc [een Hallo wereld-voorbeeld voor toegang tot SQL Server met python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Een ander alternatief is om in te voegen van gegevens in de database met de [hulpprogramma BCP](https://msdn.microsoft.com/library/ms162802.aspx).
> 
> 

### <a name="sql-aml"></a>Verbinding maken met Azure Machine Learning
De nieuwe functie kan worden toegevoegd als een kolom aan een bestaande tabel of die zijn opgeslagen in een nieuwe tabel en samengevoegd met de oorspronkelijke tabel voor machine learning. Functies kunnen worden gegenereerd of geopend als u al hebt gemaakt, met behulp van de [importgegevens] [ import-data] module in Azure Machine Learning, zoals hieronder weergegeven:

![lezers van azureml][1] 

## <a name="python"></a>Met behulp van een programmeertaal zoals Python
Met behulp van Python voor gegevens verkennen en het genereren van functies als de gegevens in SQL Server is vergelijkbaar met het verwerken van gegevens in Azure blob met behulp van Python, zoals beschreven in [proces Azure Blob-gegevens in uw data science-omgeving](data-blob.md). De gegevens moeten worden geladen uit de database in een gegevensframe pandas en vervolgens verder kunnen worden verwerkt. We leggen het proces van verbinding met de database en het laden van de gegevens in het kader van de gegevens in deze sectie.

De volgende indeling van de verbindingsreeks kan worden gebruikt voor het verbinding maken met een SQL Server-database vanuit Python met behulp van pyodbc (vervang servername, dbname, gebruikersnaam en wachtwoord met uw specifieke waarden):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

De [Pandas bibliotheek](https://pandas.pydata.org/) in Python biedt u een uitgebreide set gegevensstructuren en hulpprogramma's voor gegevensanalyse voor gegevensmanipulatie voor Python programmeren. De onderstaande code leest de resultaten van een SQL Server-database in een gegevensframe Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Nu u met het Pandas dataframe werken kunt zoals beschreven in het artikel [proces Azure Blob-gegevens in uw data science-omgeving](data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure Data Science in actie voorbeeld
Zie voor een overzicht van end-to-end-voorbeeld van het Azure Data Science Process met behulp van een openbare gegevensset, [Azure Data Science Process in actie](sql-walkthrough.md).

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

