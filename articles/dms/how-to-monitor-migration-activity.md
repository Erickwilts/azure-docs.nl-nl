---
title: Azure Database Migration Service gebruiken voor het bewaken van migratieactiviteiten | Microsoft Docs
description: Leer hoe u met de Azure Database Migration Service voor het bewaken van migratieactiviteiten.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 325bbee3f3d5ad5097f710cb56fe03baff97388a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60532869"
---
# <a name="monitor-migration-activity"></a>Migratieactiviteit bewaken
In dit artikel leert u hoe u de voortgang van een migratie op het databaseniveau van een en het tabelniveau van een.

## <a name="monitor-at-the-database-level"></a>Controleren op het databaseniveau van de
Voor het bewaken van activiteiten op het databaseniveau van de, raadpleegt u de blade op databaseniveau:

![Blade op databaseniveau](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> Selecteren van de database-hyperlink ziet u de lijst met tabellen en de voortgang van de migratie.

De volgende tabel geeft een lijst van de velden op de blade op databaseniveau en beschrijft de verschillende statuswaarden die zijn gekoppeld aan elk.

<table id='overview' class='overview'>
  <thead>
    <tr>
      <th class="x-hidden-focus"><strong>Veldnaam</strong></th>
      <th><strong>Veld substatus</strong></th>
      <th><strong>Beschrijving</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" class="ActivityStatus"><strong>Activiteitsstatus</strong></td>
      <td>In uitvoering</td>
      <td>Migratieactiviteit wordt uitgevoerd.</td>
    </tr>
    <tr>
      <td>Geslaagd</td>
      <td>Migratieactiviteit is voltooid zonder problemen.</td>
    </tr>
    <tr>
      <td>Mislukt</td>
      <td>Migratie is mislukt. Selecteer de koppeling 'Zie foutdetails' onder de details van de migratie voor het volledige foutbericht.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Status</strong></td>
      <td>Tijdens de initialisatie van</td>
      <td>DMS is instellen van de migratie-pipeline.</td>
    </tr>
    <tr>
      <td>In uitvoering</td>
      <td>DMS-pijplijn wordt uitgevoerd en migratie uitvoert.</td>
    </tr>
    <tr>
      <td>Voltooien</td>
      <td>De migratie is voltooid.</td>
    </tr>
    <tr>
      <td>Mislukt</td>
      <td>Migratie is mislukt. Klik op details om te zien van fouten bij de migratie van de migratie.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Details van de migratie</strong></td>
      <td>Starten van de migratie-pipeline</td>
      <td>DMS is instellen van de migratie-pipeline.</td>
    </tr>
    <tr>
      <td>Volledige gegevens worden geladen</td>
      <td>DMS is bezig met laden.</td>
    </tr>
    <tr>
      <td>Gereed voor Cutover</td>
      <td>Nadat de initiële laden is voltooid, wordt DMS database markeren als gereed voor cutover. Gebruiker moet controleren als gegevens op de doorlopende synchronisatie is bijgewerkt.</td>
    </tr>
    <tr>
      <td>Alle wijzigingen worden toegepast</td>
      <td>Initiële laden en doorlopende synchronisatie zijn voltooid. Deze status wordt ook weergegeven nadat de database nog niet is cutover.</td>
    </tr>
    <tr>
      <td>Zie de details van fout</td>
      <td>Klik op de koppeling naar de Foutdetails weergeven.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Duur</strong></td>
      <td>N/A</td>
      <td>Totale tijd van de migratieactiviteit wordt geïnitialiseerd voor de migratie is voltooid of migratie is mislukt.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Controleren op het tabelniveau van – korte samenvatting
Voor het bewaken van activiteiten op het tabelniveau van de, de blade op tabelniveau weergeven Het bovenste gedeelte van de blade ziet u dat de gedetailleerde aantal rijen volledig laden en incrementele updates gemigreerd. 

Het onderste gedeelte van de blade geeft een lijst van de tabellen en geeft een kort overzicht van migratie wordt uitgevoerd.

![Tabel-niveau van de blade - snel overzicht](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

De volgende tabel beschrijft de velden die worden weergegeven in de details van de tabel op serverniveau.

| Veldnaam        | Description       |
| ------------- | ------------- |
| **Volledig geladen**      | Aantal tabellen voltooid alle gegevens worden geladen. |
| **Volledig laden in de wachtrij geplaatst**      | Het aantal tabellen in de wachtrij geplaatst voor volledig laden.      |
| **Volledig laden wordt uitgevoerd** | Aantal tabellen is mislukt.      |
| **Incrementele updates**      | Het aantal gegevens vastleggen (CDC)-updates in de rijen die worden toegepast op het doel. |
| **Incrementele ingevoegd**      | Aantal CDC wordt ingevoegd in de rijen die worden toegepast op het doel.      |
| **Incrementele verwijderen** | Aantal CDC wordt verwijderd in rijen toegepast op het doel.      |
| **Wijzigingen in behandeling**      | Aantal CDC in rijen die zijn nog niet toegepast op het doel. |
| **Toegepaste wijzigingen**      | Totaal van CDC-updates, invoegen, en wordt verwijderd in rijen toegepast op het doel.      |
| **Tabellen in de foutstatus** | Het aantal tabellen die status 'fout' tijdens de migratie zijn. Enkele voorbeelden die tabellen in de foutstatus raadplegen kunnen als er dubbele waarden vermeld in het doel zijn of gegevens is niet compatibel is geladen in de doeltabel.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Controleren op het tabelniveau van: gedetailleerd overzicht
Er zijn twee tabbladen die de migratievoortgang van in volledig laden en incrementele gegevenssynchronisatie weergeven.
    
![Tabblad volledig laden](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Tabblad van incrementele gegevens synchroniseren](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

De volgende tabel beschrijft de velden weergegeven in de tabel op migratie wordt uitgevoerd.

| Veldnaam        | Description       |
| ------------- | ------------- |
| **Status - synchronisatie**      | Doorlopende synchronisatie wordt uitgevoerd. |
| **Invoegen**      | Aantal CDC wordt ingevoegd in de rijen die worden toegepast op het doel.      |
| **Update** | Het aantal CDC-updatebewerkingen in rijen toegepast op het doel.      |
| **Verwijderen**      | Aantal CDC wordt verwijderd in rijen toegepast op het doel. |
| **Totaal toegewezen**      | Totaal van CDC-updates, invoegen, en wordt verwijderd in rijen toegepast op het doel. |
| **Fouten met gegevens** | Er is een aantal fouten opgetreden in deze tabel. Enkele voorbeelden van de fouten zijn *511: Kan geen rij van de grootte %d die groter dan de toegestane maximale rijgrootte van %d, 8114 is niet maken: Fout bij converteren van gegevenstype ls % naar %ls.*  Klant moet een query uit de tabel dms_apply_exceptions in Azure doel om de foutdetails te bekijken.    |

> [!NOTE]
> CDC-waarden van Insert, Update, Delete en totale toegepast kunnen afnemen wanneer de database cutover is of migratie opnieuw wordt opgestart.

## <a name="next-steps"></a>Volgende stappen
- Bekijk de hulp bij de migratie in de Microsoft [handleiding voor databasemigratie](https://datamigration.microsoft.com/).