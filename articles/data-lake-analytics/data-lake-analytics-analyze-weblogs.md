---
title: Websitelogboeken analyseren met Azure Data Lake Analytics
description: Leer hoe u websitelogboeken analyseren met Data Lake Analytics.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 83742a4f82fb4d67fd258ff0d242847eab634c78
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60334075"
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Websitelogboeken analyseren met Azure Data Lake Analytics
Leer hoe u websitelogboeken analyseren met Data Lake Analytics, met name op controleren welke verwijzende sites fouten wordt uitgevoerd wanneer er wordt geprobeerd de website te bezoeken.

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2015 of Visual Studio 2013**.
* **[Data Lake Tools voor Visual Studio](https://aka.ms/adltoolsvs)** .

    Wanneer Data Lake Tools voor Visual Studio is geïnstalleerd, ziet u een **Data Lake** item in de **extra** menu van Visual Studio:

    ![U-SQL Visual Studio-menu](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Basiskennis van Data Lake Analytics en Data Lake Tools voor Visual Studio**. Als u wilt beginnen, Zie:

  * [U-SQL-script met Data Lake tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md).
* **Een Data Lake Analytics-account.**  Zie [maken van een Azure Data Lake Analytics-account](data-lake-analytics-get-started-portal.md).
* **Installeer de voorbeeldgegevens.** In de Azure-Portal, opent u Data Lake Analytics-account en klikt u op **voorbeeldscripts** in het linkermenu en klik vervolgens op **Copy Sample Data**. 

## <a name="connect-to-azure"></a>Verbinding maken met Azure
Voordat u kunt bouwen en testen van een U-SQL-scripts, moet u eerst verbinding maken met Azure.

**Verbinding maken met Data Lake Analytics**

1. Open Visual Studio.
2. Klik op **Data Lake > Opties en instellingen**.
3. Klik op **aanmelden**, of **gebruiker wijzigen** als iemand anders heeft aangemeld en volg de instructies.
4. Klik op **OK** om het dialoogvenster Opties en instellingen te sluiten.

**Om te bladeren van uw Data Lake Analytics-accounts**

1. Open in Visual Studio, **Server Explorer** door press **CTRL + ALT + S**.
2. Vouw in **Server Explorer** achtereenvolgens **Azure** en **Data Lake Analytics** uit. U ziet een lijst met uw Data Lake Analytics-accounts, als u die hebt. U kunt geen Data Lake Analytics-accounts maken vanuit de studio. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) of [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md) voor meer informatie over het maken van een account.

## <a name="develop-u-sql-application"></a>U-SQL-toepassing ontwikkelen
Een U-SQL-toepassing is voornamelijk een U-SQL-script. Zie voor meer informatie over U-SQL, [aan de slag met U-SQL](data-lake-analytics-u-sql-get-started.md).

U kunt de gebruiker gedefinieerde operators toevoeging toevoegen aan de toepassing.  Zie voor meer informatie, [ontwikkelen van U-SQL-gebruiker gedefinieerde operators voor Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Een Data Lake Analytics-taak maken en verzenden**

1. Klik op de **bestand > Nieuw > Project**.
2. Selecteer het Project U-SQL-type.

    ![nieuw U-SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klik op **OK**. Visual studio maakt een oplossing met een bestand Script.usql.
4. Voer het volgende script in het bestand Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Zie voor meer informatie over de U-SQL, [aan de slag met Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md).    
5. Een nieuw U-SQL-script toevoegen aan uw project en voer het volgende:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Schakel terug naar het eerste U-SQL-script en klik bij de **indienen** knop, de Analytics-account opgeven.
7. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Build Script**. Controleer of de resultaten in het deelvenster Uitvoer.
8. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Submit Script**.
9. Controleer of de **Analytics-Account** waar u de taak uitvoeren en klik vervolgens op de is **indienen**. Het resultaat van het verzenden en de koppeling naar de taak zijn beschikbaar in het resultaatvenster van Data Lake Tools voor Visual Studio wanneer het verzenden is voltooid.
10. Wacht totdat de taak is voltooid.  Als de taak is mislukt, is het zeer waarschijnlijk het bronbestand ontbreekt.  Zie de sectie vereisten van deze zelfstudie. Zie voor meer informatie over probleemoplossing [Monitor en Azure Data Lake Analytics-taken oplossen](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Wanneer de taak is voltooid, ziet u het volgende scherm:

    ![Data lake analytics analyseren weblogs websitelogboeken](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Herhaal nu stap 7: 10 voor **Script1.usql**.

**De taakuitvoer weergeven**

1. Ga naar **Server Explorer**, vouw **Azure** uit, vouw **Data Lake Analytics** uit, vouw uw Data Lake Analytics-account uit, vouw **Storage Accounts** uit, klik met de rechtermuisknop op het Data Lake Storage-standaardaccount en klik vervolgens op **Explorer**.
2. Dubbelklik op **voorbeelden** naar de map wilt openen, en dubbelklik vervolgens op **uitvoer**.
3. Dubbelklik op **UnsuccessfulResponses.log**.
4. U kunt ook dubbelklikken op het uitvoerbestand in de diagramweergave van de taak om Ga rechtstreeks naar de uitvoer.

## <a name="see-also"></a>Zie ook
Om aan de slag te gaan met Data Lake Analytics met verschillende hulpprogramma's, zie:

* [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
* [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met .NET SDK](data-lake-analytics-get-started-net-sdk.md)
