---
title: Poorten boven 1433 voor SQL Database | Microsoft Docs
description: Clientverbindingen van ADO.NET naar Azure SQL Database kunnen de proxy overslaan en Communiceer rechtstreeks met de database met behulp van andere poorten dan 1433.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 04/03/2019
ms.openlocfilehash: ddb115370c62371e769ef98e0031f7e0379bafbf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61075492"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Poorten boven 1433 voor ADO.NET 4.5

Dit onderwerp beschrijft het gedrag van de Azure SQL Database-verbindingen voor clients die gebruikmaken van ADO.NET 4.5 of hoger.

> [!IMPORTANT]
> Zie voor meer informatie over de connectiviteitsarchitectuur van [Azure SQL Database-connectiviteitsarchitectuur](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Buiten vs binnen

Voor verbindingen met Azure SQL Database, moet eerst vragen we uw clientprogramma wordt uitgevoerd of *buiten* of *binnen* de grens van de Azure-cloud. De subsecties worden twee algemene scenario's besproken.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Buiten:* Client wordt uitgevoerd op uw desktopcomputer

Poort 1433 is de enige poort die moet worden geopend op uw computer die als host fungeert voor de clienttoepassing van uw SQL-Database.

### <a name="inside-client-runs-on-azure"></a>*In:* Client wordt uitgevoerd op Azure

Wanneer de client wordt uitgevoerd binnen de grenzen van de Azure-cloud, gebruikt deze kunt zogeheten een *direct route* om te communiceren met de SQL Database-server. Nadat een verbinding tot stand is gebracht, verdere interactie tussen de client en de database hebben betrekking op geen Azure SQL Database-Gateway.

De volgorde is als volgt:

1. ADO.NET 4.5 (of hoger) initieert een korte interactie met de Azure-cloud, en ontvangt een dynamisch geïdentificeerde poortnummer.

   * Het dynamisch geïdentificeerde poortnummer is in het bereik van 11000 11999.
2. ADO.NET maakt vervolgens verbinding met de SQL Database-server rechtstreeks met geen middleware tussenin.
3. Query's rechtstreeks naar de database worden verzonden en resultaten rechtstreeks naar de client worden geretourneerd.

Zorg ervoor dat de poort bereik van 11000-11999 op uw Azure-client-computer beschikbaar voor ADO.NET 4.5 client interacties met SQL Database blijven.

* Poorten in het bereik moet in het bijzonder vrij van andere uitgaande blokkeringen.
* Op uw Azure-VM, de **Windows Firewall met geavanceerde beveiliging** Hiermee bepaalt u de poortinstellingen.
  
  * Kunt u de [gebruikersinterface van de firewall](https://msdn.microsoft.com/library/cc646023.aspx) om toe te voegen een regel die u opgeeft de **TCP** protocol samen met een bereik met de syntaxis zoals **11000 11999**.

## <a name="version-clarifications"></a>Versie verduidelijkingen

In deze sectie wordt uitleg gegeven over de monikers die naar versies van het product verwijzen. Het bevat ook enkele testpunten versies van producten.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4.0 ondersteunt het TDS-7.3-protocol, maar niet 7.4.
* ADO.NET 4.5 en hoger ondersteunt het TDS-7.4-protocol.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 of hoger

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4.2 of hoger (JDBC 4.0 daadwerkelijk biedt ondersteuning voor TDS-7.4 maar implementeert geen "omleiding")

## <a name="related-links"></a>Verwante koppelingen

* ADO.NET 4.6 is uitgebracht op 20 juli 2015. Een aankondiging blog van het team van .NET is beschikbaar [hier](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 is uitgebracht op 15 augustus 2012. Een aankondiging blog van het team van .NET is beschikbaar [hier](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  * Er is een blogbericht over ADO.NET 4.5.1 beschikbaar [hier](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-net-framework-4-5-1-preview.aspx).

* Microsoft® ODBC Driver 17 voor SQL Server® - Windows, Linux en macOS https://www.microsoft.com/download/details.aspx?id=56567

* Verbinding maken met Azure SQL Database V12 via een omleiding https://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [Lijst met de versie van de TDS-protocol](http://www.freetds.org/userguide/tdshistory.htm)
* [Overzicht van de ontwikkeling van de SQL Database](sql-database-develop-overview.md)
* [Azure SQL Database-firewall](sql-database-firewall-configure.md)
* [Procedure: Firewallinstellingen voor SQL Database configureren](sql-database-configure-firewall-settings.md)


