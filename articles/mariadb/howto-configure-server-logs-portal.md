---
title: Configureren en toegang tot van serverlogboeken voor Azure Database voor MariaDB in Azure portal
description: Dit artikel wordt beschreven hoe u configureert en toegang krijgen tot de serverlogboeken in Azure Database voor MariaDB vanuit Azure portal.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 3dbf7064e409230916668e62ef861c0ce149fdbb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065640"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configureren en access-server beschikbaar zijn in Azure portal

U kunt configureren, weergeven en downloaden de [Azure Database voor MariaDB langzame-querylogboeken](concepts-server-logs.md) vanuit Azure portal.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- [Azure Database voor MariaDB-server](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Logboekregistratie configureren
Configureer de toegang tot het logboek voor langzame query's. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer uw Azure Database voor MariaDB-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. 
   ![Selecteer serverlogboeken, klik om te configureren](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Selecteer de kop **Klik hier om Logboeken inschakelen en configureren van logboekparameters** om te zien van de parameters van de server.

5. Wijzigt u de parameters die u nodig hebt om aan te passen, waaronder het inschakelen van de 'slow_query_log' op' aan'. Alle wijzigingen die u in deze sessie aanbrengt worden in paars gemarkeerd. 

   Nadat u de parameters hebt gewijzigd, kunt u klikken op **opslaan**. U kunt ook **negeren** uw wijzigingen.

   ![Klik op opslaan of negeren](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Ga terug naar de lijst met Logboeken door te klikken op de **knop Sluiten** (het pictogram X) op de **serverparameters** pagina.

## <a name="view-list-and-download-logs"></a>Weergeven en logboeken downloaden
Zodra logboekregistratie wordt gestart, kunt u een lijst met beschikbare langzame-querylogboeken weergeven en downloaden van afzonderlijke logboekbestanden op de Server Logs-deelvenster. 

1. Open Azure Portal.

2. Selecteer uw Azure Database voor MariaDB-server.

3. Onder de **bewaking** sectie in de zijbalk, selecteer **serverlogboeken**. De pagina bevat een overzicht van uw logboekbestanden, zoals wordt weergegeven:

   ![Lijst van Logboeken](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > De naamgevingsconventie van het logboek is **mysql - traag - < naam van uw server >-yyyymmddhh.log**. De datum en tijd die wordt gebruikt in de bestandsnaam is het tijd is wanneer het logboek is uitgegeven. Logboekbestanden worden gedraaid elke 24 uur of 7,5 GB, afhankelijk van wat eerst komt.

4. Als het nodig is, gebruikt u de **zoekvak** snel verfijnen naar een bepaald op basis van datum/tijd. De zoekopdracht is op de naam van het logboek.

5. Downloaden van afzonderlijke logboekbestanden met behulp van de **downloaden** knop (omlaag pijlpictogram) naast elke log-bestand in een rij in de tabel, zoals wordt weergegeven:

   ![Klik op het downloadpictogram](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [langzame-querylogboeken](concepts-server-logs.md) in Azure Database voor MariaDB.
- Zie de MariaDB-documentatie voor meer informatie over de parameterdefinities en logboekregistratie op [logboeken](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!--- See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->
