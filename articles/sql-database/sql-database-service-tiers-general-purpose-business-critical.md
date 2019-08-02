---
title: Azure SQL Database-algemeen gebruik en bedrijfs kritiek | Microsoft Docs
description: In het artikel worden de algemene doel stellingen en bedrijfskritische service lagen in het op vCore gebaseerde inkoop model besproken.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 02/23/2019
ms.openlocfilehash: decb4428321d5083d6ba7af134e223eb2fa5a912
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566708"
---
# <a name="azure-sql-database-service-tiers"></a>Service lagen Azure SQL Database

Azure SQL Database is gebaseerd op SQL Server data base engine-architectuur die is aangepast voor de cloud omgeving om 99,99 procent Beschik baarheid te garanderen, zelfs als er een infrastructuur fout is opgetreden. Er worden drie service lagen gebruikt in Azure SQL Database, elk met een ander architectuur model. Deze service lagen zijn:

- [Algemeen gebruik](sql-database-service-tier-general-purpose.md), dat is ontworpen voor de meeste algemene werk belastingen.
- [Bedrijfs kritiek](sql-database-service-tier-business-critical.md), die is ontworpen voor workloads met lage latentie met één Lees bare replica.
- [Grootschalige](sql-database-service-tier-hyperscale.md), dat is ontworpen voor zeer grote data bases (tot 100 TB) met meerdere Lees bare replica's.

In dit artikel worden de overwegingen voor opslag en back-ups beschreven voor de laag algemeen en bedrijfskritische service lagen in het op vCore gebaseerde aankoop model.

> [!NOTE]
> Zie [grootschalige service tier](sql-database-service-tier-hyperscale.md)(grootschalige) voor meer informatie over de servicelaag in het op vCore gebaseerde aankoop model. Zie [Azure SQL database-inkoop modellen en-resources](sql-database-purchase-models.md)voor een vergelijking van het op vCore gebaseerde aankoop model met het DTU-gebaseerde aankoop model.

## <a name="data-and-log-storage"></a>Gegevens-en logboek opslag

De volgende factoren zijn van invloed op de hoeveelheid opslag die wordt gebruikt voor gegevens-en logboek bestanden:

- De toegewezen opslag wordt gebruikt door gegevens bestanden (MDF) en logboek bestanden (LDF).
- Elke reken grootte van één data base ondersteunt een maximale database grootte, met een standaard maximale grootte van 32 GB.
- Wanneer u de vereiste grootte voor één data base (de grootte van het MDF-bestand) configureert, wordt er automatisch 30 procent extra opslag toegevoegd ter ondersteuning van LDF-bestanden.
- De opslag grootte voor een SQL Database beheerd exemplaar moet worden opgegeven in veelvouden van 32 GB.
- U kunt de grootte van één data base tussen 10 GB en het ondersteunde maximum selecteren.
  - Verg root of verklein de grootte in stappen van 10 GB voor opslag in de service lagen standaard of algemeen.
  - Verg root of verklein de grootte in stappen van 250 GB voor opslag in de essentiële of bedrijfs kritieke service lagen.
- In de servicelaag `tempdb` voor algemeen gebruik wordt een bijgevoegde SSD gebruikt. deze opslag kosten worden opgenomen in de vCore prijs.
- In de laag `tempdb` bedrijfskritische service deelt u de gekoppelde SSD met de MDF-en LDF-bestanden en `tempdb` worden de opslag kosten opgenomen in de vCore-prijs.

> [!IMPORTANT]
> Er worden kosten in rekening gebracht voor de totale opslag ruimte die is toegewezen voor MDF-en LDF-bestanden.

Gebruik [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql)om de huidige totale grootte van uw MDF-en LDF-bestanden te controleren. Gebruik [sys. database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql)om de huidige grootte van de afzonderlijke MDF-en LDF-bestanden te bewaken.

> [!IMPORTANT]
> In sommige gevallen is het wellicht voor het verkleinen van een database voor het vrijmaken van ongebruikte ruimte. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Back-ups en opslag

Opslag voor back-ups van data bases wordt toegewezen ter ondersteuning van de mogelijkheden voor PITR (Point-in-time Restore) en [v.l.n.r. (Long-term retention)](sql-database-long-term-retention.md) van SQL database. Deze opslag wordt afzonderlijk voor elke Data Base toegewezen en gefactureerd als twee afzonderlijke kosten per data base.

- **PITR**: Afzonderlijke database back-ups worden automatisch naar [geografisch redundante opslag met lees toegang (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md) gekopieerd. De opslag grootte neemt dynamisch toe wanneer er nieuwe back-ups worden gemaakt. De opslag wordt gebruikt door wekelijkse volledige back-ups, dagelijkse differentiële back-ups en back-ups van transactie logboeken, die elke vijf minuten worden gekopieerd. Het opslag verbruik is afhankelijk van de frequentie waarmee de data base wordt gewijzigd en de retentie periode voor back-ups. U kunt tussen 7 en 35 dagen een afzonderlijke Bewaar periode configureren voor elke Data Base. Er wordt geen extra kosten in rekening gebracht voor een minimale opslag hoeveelheid die gelijk is aan 100 procent (1x) van de grootte van de data base. Voor de meeste data bases is deze hoeveelheid voldoende om zeven dagen back-ups op te slaan.
- **LTR**: SQL Database biedt u de mogelijkheid om lange termijn retentie van volledige back-ups te configureren gedurende Maxi maal tien jaar. Als u een LTR-beleid instelt, worden deze back-ups automatisch opgeslagen in RA-GRS-opslag, maar u kunt bepalen hoe vaak de back-ups worden gekopieerd. Als u wilt voldoen aan verschillende nalevings vereisten, kunt u verschillende Bewaar perioden selecteren voor wekelijkse, maandelijkse en/of jaarlijkse back-ups. De configuratie die u kiest, bepaalt hoeveel opslag ruimte wordt gebruikt voor V.L.N.R.-back-ups. U kunt de LTR-prijs calculator gebruiken om de kosten van V.L.N.R.-opslag te schatten. Zie [SQL database lange termijn](sql-database-long-term-retention.md)retentie voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL database resource limieten op basis van vCore voor afzonderlijke](sql-database-vcore-resource-limits-single-databases.md)data bases voor meer informatie over de specifieke berekenings grootten en opslag grootten die beschikbaar zijn voor één data base in de laag algemeen en bedrijfskritische service lagen.
- Zie [SQL database resource limieten op basis van vCore voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)voor meer informatie over de specifieke berekenings grootten en opslag grootten die beschikbaar zijn voor elastische Pools in de categorie Algemeen en bedrijfskritische service.
