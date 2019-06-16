---
title: Configureren en logboeken van de access-server voor Azure Database voor PostgreSQL - één Server in Azure Portal
description: Dit artikel wordt beschreven hoe u configureert en toegang krijgen tot de serverlogboeken in Azure Database voor PostgreSQL - servergegevens uit het Azure-Portal.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 13a8c1ee4b7ca114211f93245d74866e5aa5d52c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067452"
---
# <a name="configure-and-access-azure-database-for-postgresql---single-server-logs-in-the-azure-portal"></a>Configureren en toegang tot Azure Database voor PostgreSQL - logboeken voor één Server in Azure portal

U kunt configureren, weergeven en downloaden de [Azure Database for PostgreSQL-logboeken](concepts-server-logs.md) vanuit Azure portal.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database for PostgreSQL-server](quickstart-create-server-database-portal.md)

## <a name="configure-logging"></a>Logboekregistratie configureren
Toegang tot de logboeken voor query's en de foutenlogboeken configureren. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer uw Azure Database for PostgreSQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. 

   ![Selecteer serverlogboeken en selecteer 'Klik hier om in te schakelen...'](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Selecteer de kop **Klik hier om Logboeken inschakelen en configureren van logboekparameters** om te zien van de parameters van de server.

5. Hiermee wijzigt u de parameters die u nodig hebt om aan te passen. Alle wijzigingen die u in deze sessie aanbrengt worden in paars gemarkeerd.

   Nadat u de parameters hebt gewijzigd, kunt u klikken op **opslaan**. U kunt ook **negeren** uw wijzigingen. 

   ![Lange lijst met parameters met de wijzigingen op te slaan of te negeren](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

6. Ga terug naar de lijst met Logboeken door te klikken op de **knop Sluiten** (het pictogram X) op de **serverparameters** pagina.

## <a name="view-list-and-download-logs"></a>Weergeven en logboeken downloaden
Zodra logboekregistratie wordt gestart, kunt u een lijst met beschikbare logboeken weergeven en downloaden van afzonderlijke logboekbestanden op de Server Logs-deelvenster. 

1. Open Azure Portal.

2. Selecteer uw Azure Database for PostgreSQL-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. De pagina bevat een overzicht van uw logboekbestanden, zoals wordt weergegeven:

   ![Lijst met servers-Logboeken](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > De naamgevingsconventie van het logboek is **postgresql-jjjj-mm-dd_hh0000.log**. De datum en tijd die wordt gebruikt in de bestandsnaam is het tijd is wanneer het logboek is uitgegeven. De logboekbestanden draaien om één uur of de grootte van 100 MB, afhankelijk van wat het eerste komt.

4. Als het nodig is, gebruikt u de **zoekvak** snel verfijnen naar een bepaald op basis van datum/tijd. De zoekopdracht is op de naam van het logboek.

   ![Voorbeeld van de zoekopdracht op logboeknamen](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Downloaden van afzonderlijke logboekbestanden met behulp van de **downloaden** knop (omlaag pijlpictogram) naast elke log-bestand in een rij in de tabel, zoals wordt weergegeven:

   ![Klik op het downloadpictogram](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Volgende stappen
- Zie [serverlogboeken openen in de CLI](howto-configure-server-logs-using-cli.md) voor meer informatie over hoe u kunt Logboeken downloaden die u via een programma.
- Meer informatie over [serverlogboeken](concepts-server-logs.md) in Azure database voor PostgreSQL. 
- Zie voor meer informatie over de parameterdefinities en PostgreSQL-logboekregistratie, de PostgreSQL-documentatie op [foutrapportage en logboekregistratie](https://www.postgresql.org/docs/current/static/runtime-config-logging.html).

