---
title: Incrementeel gegevens kopiëren met behulp van Azure Data Factory | Microsoft Docs
description: Deze zelfstudies tonen hoe u stapsgewijs gegevens kunt kopiëren van een brongegevensarchief naar een doelgegevensarchief. De eerste kopieert gegevens uit één tabel.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: 87b5b30738451800da21736d7f139c4ba85ff998
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68233699"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Incrementeel laden van gegevens van een brongegevensarchief naar een doelgegevensarchief

In een oplossing voor gegevensintegratie is incrementeel (of delta) laden van gegevens na een eerste volledige laadhandeling een veelgebruikt scenario. De zelfstudies in deze sectie tonen u de verschillende manieren van het incrementeel laden van gegevens met behulp van Azure Data Factory.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Delta-gegevens laden uit de database met behulp van een watermerk
In dit geval definieert u een watermerk in de brondatabase. Een watermerk is een kolom die het laatst bijgewerkte tijdstempel of een ophogende sleutel heeft. Bij delta-laden worden de gewijzigde gegevens tussen een oud watermerk en een nieuw watermerk geladen. De werkstroom voor deze benadering wordt verduidelijkt in het volgende diagram: 

![Werkstroom voor het gebruik van een watermerk](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Zie de volgende zelfstudies voor stapsgewijze instructies: 
- [Incrementeel gegevens uit een tabel in Azure SQL Database kopiëren naar Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Incrementeel gegevens uit meerdere tabellen in een lokale SQL Server naar een Azure SQL Database kopiëren](tutorial-incremental-copy-multiple-tables-powershell.md)

Voor sjablonen, Zie de volgende:
- [Delta-kopie met controle-tabel](solution-template-delta-copy-with-control-table.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Delta-gegevens laden uit SQL DB met behulp van de technologie voor wijzigingen bijhouden
Technologie voor het bijhouden van wijzigingen is een lichtgewicht oplossing in SQL Server en Azure SQL Database waarmee een efficiënt mechanisme wordt geboden voor het bijhouden van wijzigingen in toepassingen. Hiermee kan een toepassing eenvoudig gegevens herkennen die zijn toegevoegd, bijgewerkt of verwijderd. 

De werkstroom voor deze benadering wordt verduidelijkt in het volgende diagram:

![Werkstroom voor het gebruik van Wijzigingen bijhouden](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Zie de volgende zelfstudies voor stapsgewijze instructies: <br/>
- [Incrementeel gegevens kopiëren van Azure SQL Database naar Azure Blob Storage met behulp van technologie voor wijzigingen bijhouden](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Alleen nieuwe en gewijzigde bestanden laden met behulp van LastModifiedDate
U kunt de nieuwe en gewijzigde bestanden kopiëren alleen met behulp van LastModifiedDate naar de doel-store. ADF scant alle bestanden uit de brongegevensopslag, toepassen van het filter door hun LastModifiedDate, en alleen de nieuwe en bijgewerkte bestand sinds de laatste keer kopiëren naar het doelarchief.  Houd er rekening mee dat als u grote hoeveelheden van ADF scannen van bestanden, kunnen maar alleen enkele bestanden naar de bestemming kopiëren, u nog steeds de lange duur van de vanwege de bestandscontrole is tijd in beslag nemen ook zou verwachten.   

Zie de volgende zelfstudies voor stapsgewijze instructies: <br/>
- [Nieuwe bestanden stapsgewijs kopiëren van Azure Blob-opslag naar Azure Blob-opslag op basis van LastModifiedDate](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

Voor sjablonen, Zie de volgende:
- [Nieuwe bestanden door LastModifiedDate kopiëren](solution-template-copy-new-files-lastmodifieddate.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Alleen nieuwe bestanden laden met behulp van de op tijdsbasis gepartitioneerde map- of bestandsnaam.
U kunt alleen nieuwe bestanden kopiëren als bestanden of mappen al op basis van tijd zijn gepartitioneerd met tijdsdeelinformatie die onderdeel is van de bestands- of mapnaam (bijvoorbeeld /yyyy/mm/dd/file.csv). Het is de meeste prestaties benadering voor het laden van incrementele nieuwe bestanden. 

Zie de volgende zelfstudies voor stapsgewijze instructies: <br/>
- [Nieuwe bestanden stapsgewijs kopiëren van Azure Blob-opslag naar Azure Blob-opslag op basis van de op tijdsbasis gepartitioneerde map- of bestandsnaam](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Volgende stappen
Ga naar de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Incrementeel gegevens uit een tabel in Azure SQL Database kopiëren naar Azure Blob Storage](tutorial-incremental-copy-powershell.md)
