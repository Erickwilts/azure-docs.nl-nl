---
title: Gegevens verwijderen uit Azure Data Explorer
description: Dit artikel wordt beschreven scenario's voor bulksgewijs verwijderen in Azure gegevens verkennen, met inbegrip van opschonen van Logboeken en retentie op basis van wordt verwijderd.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 9c1b21e119a38c6d306b9c564ab7958ba21a1c41
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60445665"
---
# <a name="delete-data-from-azure-data-explorer"></a>Gegevens verwijderen uit Azure Data Explorer

Azure Data Explorer biedt ondersteuning voor verschillende bulksgewijs verwijderen benaderingen, die we in dit artikel behandelen. Het biedt geen ondersteuning voor per record verwijderen in realtime, omdat deze geoptimaliseerd voor snelle toegang voor lezen.

* Als een of meer tabellen niet meer nodig is, verwijdert u deze met behulp van de tabel verwijderen of tabellen opdracht verwijderen.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Als de oude gegevens is niet meer nodig hebt, kunt u deze door het veranderen van de bewaarperiode op het niveau van de database of tabel verwijderen.

    Houd rekening met een database of tabel die is ingesteld op 90 dagen bewaard. Zakelijke moet wijzigen, zodat alleen 60 dagen aan gegevens nodig is. In dit geval verwijderen van de oudere gegevens in een van de volgende manieren.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Zie voor meer informatie, [bewaarbeleid](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

Als u hulp nodig hebt bij problemen met het verwijderen van gegevens, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
