---
title: Problemen met Synapse SQL oplossen
description: Problemen met Synapse SQL in azure Synapse Analytics oplossen.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: bce5e14db133ad55a2035c0c8074486ed1b8a6ec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89460504"
---
# <a name="troubleshooting-synapse-sql-in-azure-synapse-analytics"></a>Problemen met Synapse SQL oplossen in azure Synapse Analytics

In dit artikel worden veelvoorkomende problemen met het oplossen van problemen in Synapse SQL beschreven.

## <a name="connecting"></a>Verbinding maken

| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Aanmelding mislukt voor gebruiker 'NT AUTHORITY\ANONYMOUS LOGON'. (Microsoft SQL Server, fout: 18456) | Deze fout treedt op wanneer een Azure AD-gebruiker probeert verbinding te maken met de hoofd database, maar geen gebruiker heeft in de master-data base.  Als u dit probleem wilt corrigeren, geeft u de SQL-groep op waarmee u verbinding wilt maken op het moment van de verbinding of voegt u de gebruiker toe aan de hoofd database.  Zie het artikel over [beveiligings overzicht](sql-data-warehouse-overview-manage-security.md) voor meer informatie. |
| De server-principal 'MijnGebruikersnaam' heeft in de huidige beveiligingscontext geen toegang tot de hoofddatabase. Kan de standaarddatabase van de gebruiker niet openen. Aanmelden mislukt. Aanmelden is mislukt voor gebruiker 'MijnGebruikersnaam'. (Microsoft SQL Server, fout: 916) | Deze fout treedt op wanneer een Azure AD-gebruiker probeert verbinding te maken met de hoofd database, maar geen gebruiker heeft in de master-data base.  Als u dit probleem wilt corrigeren, geeft u de SQL-groep op waarmee u verbinding wilt maken op het moment van de verbinding of voegt u de gebruiker toe aan de hoofd database.  Zie het artikel over [beveiligings overzicht](sql-data-warehouse-overview-manage-security.md) voor meer informatie. |
| CTAIP-fout                                                  | Deze fout kan optreden als er een aanmelding is gemaakt voor de SQL Database hoofd database, maar niet in de specifieke SQL database.  Als deze fout optreedt, bekijkt u het artikel overzicht van de [beveiliging](sql-data-warehouse-overview-manage-security.md) .  In dit artikel wordt uitgelegd hoe u een aanmelding en gebruiker in de hoofd database maakt en hoe u een gebruiker maakt in een SQL database. |
| Geblokkeerd door de firewall                                          | SQL-groepen worden beveiligd door firewalls om ervoor te zorgen dat alleen bekende IP-adressen toegang hebben tot een Data Base. De firewalls zijn standaard beveiligd. Dit betekent dat u een expliciete en een IP-adres of bereik van adressen moet inschakelen voordat u verbinding kunt maken.  Als u uw firewall voor toegang wilt configureren, volgt u de stappen in de [Server firewall toegang configureren voor uw client-IP](create-data-warehouse-portal.md) in de instructies voor het [inrichten](create-data-warehouse-portal.md). |
| Kan geen verbinding maken met het hulp programma of stuur programma                           | Synapse SQL pool raadt u aan om [SSMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), [SSDT voor Visual Studio](sql-data-warehouse-install-visual-studio.md)of [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) te gebruiken om uw gegevens op te vragen. Zie [Stuur Programma's voor Azure Synapse](sql-data-warehouse-connection-strings.md) en [verbinding maken met Azure Synapse](sql-data-warehouse-connect-overview.md) -artikelen voor meer informatie over Stuur Programma's en het maken van verbinding met Azure Synapse. |

## <a name="tools"></a>Hulpprogramma's

| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Er ontbreken Azure AD-gebruikers in Visual Studio object Explorer           | Dit is een bekend probleem.  Als tijdelijke oplossing kunt u de gebruikers weergeven in [sys.database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  Zie [verificatie voor Azure Synapse](sql-data-warehouse-authentication.md) voor meer informatie over het gebruik van Azure Active Directory met Synapse SQL-pool. |
| Hand matige scripting, met behulp van de wizard scripting of het maken van verbinding via SSMS is traag, reageert niet of levert fouten op | Zorg ervoor dat gebruikers zijn gemaakt in de hoofd database. In script opties moet u er ook voor zorgen dat de engine Edition is ingesteld op ' Microsoft Azure Synapse Analytics Edition ' en Engine type is ' Microsoft Azure SQL Database '. |
| Genereren van scripts mislukt in SSMS                               | Het genereren van een script voor de SQL-groep Synapse mislukt als de optie script voor afhankelijke objecten genereren is ingesteld op ' True '. Als tijdelijke oplossing moeten gebruikers hand matig naar **Hulpprogram ma's > opties->SQL Server-objectverkenner-> script genereren voor afhankelijke opties en ingesteld op ONWAAR** |

## <a name="performance"></a>Prestaties

| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Problemen met query prestaties oplossen                            | Als u probeert een bepaalde query op te lossen, begint u met [leren hoe u uw query's kunt bewaken](sql-data-warehouse-manage-monitor.md#monitor-query-execution). |
| Problemen met TempDB-ruimte | [Controleer](sql-data-warehouse-manage-monitor.md#monitor-tempdb) het gebruik van TempDB-ruimte.  Veelvoorkomende oorzaken voor het uitvoeren van de TempDB-ruimte zijn:<br>-Onvoldoende resources toegewezen aan de query waardoor gegevens worden overgelopen naar TempDB.  Zie [workload Management](resource-classes-for-workload-management.md) <br>-Statistieken ontbreken of zijn verouderd en veroorzaken een buitensporige verplaatsing van gegevens.  Zie [tabel statistieken onderhouden](sql-data-warehouse-tables-statistics.md) voor meer informatie over het maken van statistieken<br>-TempDB-ruimte wordt per service niveau toegewezen.  Als u [de SQL-groep](sql-data-warehouse-manage-compute-overview.md#scaling-compute) naar een hogere DWU-instelling schaalt, wordt er meer TempDB-ruimte toegewezen.|
| Slechte query prestaties en-plannen zijn vaak het resultaat van ontbrekende statistieken | De meest voorkomende oorzaak van slechte prestaties zijn geen statistieken voor uw tabellen.  Zie beheer van [tabel statistieken](sql-data-warehouse-tables-statistics.md) voor meer informatie over het maken van statistieken en waarom ze essentieel zijn voor uw prestaties. |
| Weinig gelijktijdigheid/query's in wachtrij                             | Inzicht in het [beheer van werk belastingen](resource-classes-for-workload-management.md) is belang rijk om te begrijpen hoe geheugen toewijzing met gelijktijdigheid kan worden gebalanceerd. |
| Aanbevolen procedures implementeren                              | De beste manier om te beginnen met het verbeteren van de query prestaties is het artikel [Synapse SQL-pool best practices](sql-data-warehouse-best-practices.md) . |
| Prestaties verbeteren met schalen                      | Soms is de oplossing voor het verbeteren van de prestaties het eenvoudig om meer reken kracht aan uw query's toe te voegen door [de SQL-groep te schalen](sql-data-warehouse-manage-compute-overview.md). |
| Slechte query prestaties als gevolg van slechte index kwaliteit     | Sommige keer dat query's kunnen vertragen vanwege [slechte kwaliteit van Column Store-indexen](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality).  Raadpleeg dit artikel voor meer informatie en hoe u [indexen opnieuw bouwt om de segment kwaliteit te verbeteren](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality). |

## <a name="system-management"></a>Systeem beheer

| Probleem                                                        | Oplossing                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Msg 40847: de bewerking kan niet worden uitgevoerd omdat de server het toegestane quotum van de data base-transactie eenheid van 45000 zou overschrijden. | Verminder de [DWU](what-is-a-data-warehouse-unit-dwu-cdwu.md) van de data base die u probeert te maken of [vraag een quotum toename](sql-data-warehouse-get-started-create-support-ticket.md)aan. |
| Ruimte gebruik onderzoeken                              | Zie [tabel grootten](sql-data-warehouse-tables-overview.md#table-size-queries) om inzicht te krijgen in het ruimte gebruik van uw systeem. |
| Hulp bij het beheren van tabellen                                    | Zie het artikel [overzicht](sql-data-warehouse-tables-overview.md) voor meer informatie over het beheren van tabellen.  Dit artikel bevat ook koppelingen naar gedetailleerde onderwerpen, zoals [tabel gegevens typen](sql-data-warehouse-tables-data-types.md), [het distribueren van een tabel](sql-data-warehouse-tables-distribute.md), [het indexeren](sql-data-warehouse-tables-index.md)van een tabel, het  [partitioneren van een](sql-data-warehouse-tables-partition.md)tabel, het [onderhouden van tabel statistieken](sql-data-warehouse-tables-statistics.md) en [tijdelijke tabellen](sql-data-warehouse-tables-temporary.md). |
| De voortgangs balk voor transparent Data Encryption (TDE) wordt niet bijgewerkt in de Azure Portal | U kunt de status van TDE bekijken via [Power shell](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). |

## <a name="differences-from-sql-database"></a>Verschillen van SQL Database

| Probleem                                 | Oplossing                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| Niet-ondersteunde SQL Database functies     | Zie [niet-ondersteunde tabel functies](sql-data-warehouse-tables-overview.md#unsupported-table-features). |
| Niet-ondersteunde SQL Database gegevens typen   | Zie [niet-ondersteunde gegevens typen](sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types).        |
| Beperkingen voor verwijderen en bijwerken         | Zie [oplossingen bijwerken](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements), [tijdelijke oplossingen verwijderen](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements) en [CTAS gebruiken om niet-ondersteunde syntaxis voor bijwerken en verwijderen te](sql-data-warehouse-develop-ctas.md)omzeilen. |
| De instructie MERGe wordt niet ondersteund      | Zie [tijdelijke oplossingen voor samen voegen](sql-data-warehouse-develop-ctas.md#replace-merge-statements).                  |
| Beperkingen voor opgeslagen procedures          | Zie [beperkingen van opgeslagen](sql-data-warehouse-develop-stored-procedures.md#limitations) procedures voor een overzicht van de beperkingen van opgeslagen procedure. |
| Udf's bieden geen ondersteuning voor SELECT-instructies | Dit is een huidige beperking van onze Udf's.  Zie [Create function](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) voor de syntaxis die we ondersteunen. |

## <a name="next-steps"></a>Volgende stappen

Als u meer informatie wilt over het vinden van oplossingen voor uw probleem, kunt u het volgende proberen.

* [Blogs](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [Functieverzoeken](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [Ondersteuningsticket maken](sql-data-warehouse-get-started-create-support-ticket.md)
* [Microsoft Q&A-vragenpagina](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html)
* [Stack Overflow forum](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)
