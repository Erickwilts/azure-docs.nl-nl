---
title: Inleiding tot Azure Advisor | Microsoft Docs
description: Azure Advisor gebruiken om te optimaliseren van uw Azure-implementaties.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: kasparks
ms.openlocfilehash: 2ccac3bf9a882dc021c6c969946ad9d439a7cf5d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069690"
---
# <a name="introduction-to-azure-advisor"></a>Inleiding tot Azure Advisor

Meer informatie over de belangrijkste mogelijkheden van Azure Advisor en krijg antwoorden op veelgestelde vragen.

## <a name="what-is-advisor"></a>Wat is Advisor?
Advisor is een persoonlijke cloudconsultant waarmee u Volg de aanbevolen procedures voor het optimaliseren van uw Azure-implementaties. Het analyseert uw resourceconfiguratie en gebruikstelemetrie, en raadt vervolgens oplossingen aan die u kunnen helpen de kosteneffectiviteit, prestaties, hoge beschikbaarheid en beveiliging van uw Azure-resources te verbeteren.

Met Advisor, kunt u het volgende doen:
* Proactieve, bruikbare ophalen en voorkeuren aangepaste best practices. 
* Verbeteren de prestaties, beveiliging en hoge beschikbaarheid van uw resources, zoals het identificeren van kansen om te beperken van uw totale Azure besteden.
* Ontvang aanbevelingen met voorgestelde acties inline.

U kunt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), zoek **Advisor** in het navigatiemenu of zoek het in de **alle services** menu.

De Advisor-dashboard toont de persoonlijke aanbevelingen voor al uw abonnementen.  U kunt toepassen van filters om aanbevelingen voor specifieke abonnementen en -resourcetypen weer te geven.  De aanbevelingen zijn onderverdeeld in vier categorieën: 

* **Hoge beschikbaarheid**: Om te controleren en de continuïteit van uw essentiële toepassingen verbeteren. Zie voor meer informatie, [aanbevelingen voor hoge beschikbaarheid van Advisor](advisor-high-availability-recommendations.md).
* **Beveiliging**: Voor het detecteren van bedreigingen en zwakke plekken die tot beveiligingsschendingen kunnen leiden. Zie voor meer informatie, [Advisor beveiligingsaanbevelingen](advisor-security-recommendations.md).
* **Prestaties**: Voor het verbeteren van de snelheid van uw toepassingen. Zie voor meer informatie, [Advisor prestatieaanbevelingen](advisor-performance-recommendations.md).
* **Kosten**: Om uw algehele Azure-uitgaven te optimaliseren en te verminderen. Zie voor meer informatie, [kosten van de Advisor-aanbevelingen](advisor-cost-recommendations.md).

  ![Typen van Advisor-aanbevelingen](./media/advisor-overview/advisor-dashboard.png)

U kunt op een categorie om de lijst met aanbevelingen binnen die categorie weer te geven en selecteer een aanbeveling voor meer informatie over het.  U kunt ook meer informatie over acties die u uitvoeren kunt om te profiteren van een verkoopkans of een probleem wordt opgelost.

![Categorie van Advisor-aanbevelingen](./media/advisor-overview/advisor-ha-category-example.png) 

Selecteer de aanbevolen actie voor een aanbeveling de aanbeveling kunt implementeren.  Een eenvoudige interface wordt geopend waarmee u de aanbeveling voor het implementeren of raadpleegt u documentatie die u met implementatie helpt.  Wanneer u een aanbeveling kunt implementeren, kan deze op een dag voor de Advisor voor het herkennen van die duren.

Als u niet onmiddellijk actie ondernemen op een aanbeveling wilt, kunt u het uitstellen gedurende een opgegeven periode of deze sluiten.  Als u niet ontvangen van aanbevelingen voor een bepaald abonnement of resourcegroep wilt, kunt u Advisor voor het genereren van alleen aanbevelingen voor de opgegeven abonnementen en resourcegroepen configureren.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-access-advisor"></a>Hoe krijg ik toegang tot Advisor?
U kunt toegang tot Advisor via de [Azure-portal](https://aka.ms/azureadvisordashboard). Aanmelden bij de [portal](https://portal.azure.com), zoek **Advisor** in het navigatiemenu of zoek het in de **alle services** menu.

U kunt ook de aanbevelingen van Advisor weergeven via de interface van de bron virtuele machine. Kies een virtuele machine en schuif vervolgens naar de Advisor-aanbevelingen in het menu. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Welke machtigingen heb ik nodig voor toegang tot Advisor?
 
U hebt toegang tot de aanbevelingen van Advisor als *eigenaar*, *Inzender*, of *lezer* van een abonnement.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Welke bronnen biedt Advisor aanbevelingen voor?

Advisor-aanbevelingen voor Application Gateway, App-Services, beschikbaarheidssets, Azure Cache, Azure Data Factory, Azure Database for MySQL, Azure Database for PostgreSQL, Azure Database voor MariaDB, Azure ExpressRoute, Azure Cosmos DB, Azure openbaar IP-adressen, SQL Data Warehouse, SQL-servers, opslagaccounts, Traffic Manager-profielen en virtuele machines.

Azure Advisor bevat ook de aanbevelingen van [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations) waaronder aanbevelingen voor extra resourcetypen.

### <a name="can-i-postpone-or-dismiss-a-recommendation"></a>Kan ik uitstellen of een aanbeveling negeren?

Als u wilt stellen of te verwijderen van een aanbeveling, klikt u op de **uitstellen** koppeling. U kunt opgeven dat een uitstellen, periode of selecteer **nooit** voor het verwijderen van de aanbeveling.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:

* [Aan de slag met Advisor](advisor-get-started.md)
* [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-high-availability-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)
* [Advisor-aanbevelingen voor prestaties](advisor-performance-recommendations.md)
* [Aanbevelingen van Advisor-kosten](advisor-cost-recommendations.md)
