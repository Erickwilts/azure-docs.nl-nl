---
title: Aanbevolen procedures voor het maken van evaluatie met Azure Migrate server beoordeling | Microsoft Docs
description: Biedt tips voor het maken van evaluaties met Azure Migrate server-evaluatie.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 18b82b5553f7045c38c9de532199c2a0fd815ee1
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234300"
---
# <a name="best-practices-for-creating-assessments"></a>Aanbevolen procedures voor het maken van evaluaties

[Azure migrate](migrate-overview.md) biedt een hub aan hulpprogram ma's waarmee u apps, infra structuur en werk belastingen op Microsoft Azure kunt detecteren, evalueren en migreren. De hub bevat Azure Migrate-hulpprogram ma's en ISV-aanbiedingen (Independent Software Vendor) van derden.

Dit artikel bevat een overzicht van de aanbevolen procedures voor het maken van evaluaties met behulp van het hulp programma Azure Migrate server Assessment.

## <a name="about-assessments"></a>Over evaluaties

Evaluaties die u maakt met Azure Migrate server-evaluatie zijn een tijdgebonden moment opname van gegevens. Er zijn twee soorten evaluaties in Azure Migrate.

**Beoordelings type** | **Details** | **Gegevens**
--- | --- | ---
**Op basis van prestaties** | Beoordelingen die aanbevelingen doen op basis van verzamelde prestatie gegevens | Aanbeveling voor VM-grootte is gebaseerd op gegevens van CPU en geheugen gebruik.<br/><br/> De aanbeveling van het schijf type (standaard HDD/SSD of Premium-Managed disks) is gebaseerd op de IOPS en door Voer van de on-premises schijven.
**As-is on-premises** | Evaluaties die geen gebruik maken van prestatie gegevens voor aanbevelingen. | Aanbeveling voor VM-grootte is gebaseerd op de on-premises VM-grootte<br/><br> Het aanbevolen schijf type is gebaseerd op wat u selecteert in de instelling opslag type voor de evaluatie.

### <a name="example"></a>Voorbeeld
Een voor beeld: als u een on-premises VM hebt met vier kernen van 20% gebruik en geheugen van 8 GB met 10% gebruik, zijn de evaluaties als volgt:

- **Evaluatie op basis van prestaties**:
    - Identificeert effectief kernen en geheugen op basis van core (4 x 0,20 = 0,8) en geheugen (8 GB x 0,10 = 0,8) gebruik.
    - Past de comfort factor toe die is opgegeven in de eigenschappen van de beoordeling (le'ts zeg 1,3 x) om de waarden op te halen die moeten worden gebruikt voor het aanpassen van de grootte. 
    - Hiermee wordt de dichtstbijzijnde VM-grootte in azure aanbevolen die ondersteuning biedt voor meer dan ~ 1,4 kernen (0,8 x 1,3) en ~ 1,4 GB (0,8 x 1,3) geheugen.

- **As-is (zoals on-premises) evaluatie**:
    -  Raadt een VM aan met vier kernen; 8 GB geheugen.

## <a name="best-practices-for-creating-assessments"></a>Aanbevolen procedures voor het maken van evaluaties

Het Azure Migrate apparaat doorlopend uw on-premises omgeving profileert en verstuurt meta gegevens en prestatie gegevens naar Azure. Volg deze aanbevolen procedures voor het maken van evaluaties:

- **Evaluaties maken als**: U kunt onmiddellijk evaluaties maken als uw computers in de Azure Migrate Portal worden weer gegeven.
- Een **evaluatie op basis van prestaties maken**: Na het instellen van de detectie wordt u aangeraden minstens een dag te wachten voordat u een evaluatie op basis van prestaties uitvoert:
    - Het verzamelen van prestatie gegevens kost tijd. Als u op minstens een dag wacht, zorgt u ervoor dat er voldoende prestatie gegevens punten zijn voordat u de evaluatie uitvoert.
    - Wanneer u evaluaties op basis van prestaties uitvoert, moet u ervoor zorgen dat u uw omgeving voor de evaluatie duur profileert. Als u bijvoorbeeld een beoordeling maakt waarbij de duur van de prestatie is ingesteld op één week, moet u na het starten van de detectie worden gewacht totdat alle gegevens punten zijn verzameld. Als u dit niet doet, krijgt de evaluatie van de beoordeling geen vijf sterren.
- **Evaluaties opnieuw berekenen**: Omdat beoordelingen moment opnamen van een bepaald moment zijn, worden ze niet automatisch bijgewerkt met de meest recente gegevens. Als u een evaluatie met de meest recente gegevens wilt bijwerken, moet u deze opnieuw berekenen.

## <a name="best-practices-for-confidence-ratings"></a>Aanbevolen procedures voor betrouwbaarheids classificaties

Wanneer u evaluaties op basis van prestaties uitvoert, wordt een betrouwbaarheids classificatie van 1 ster (laagste) tot 5 sterren (hoogst) toegekend aan de evaluatie. Vertrouwens classificaties effectief gebruiken:
- Voor de analyse van Azure Migrate server zijn de gebruiks gegevens voor de CPU/het geheugen van de virtuele machine nodig.
- Voor elke schijf die aan de on-premises VM is gekoppeld, is de IOPS/doorvoer gegevens lezen/schrijven vereist.
- Voor elke netwerk adapter die aan de VM is gekoppeld, is het netwerk in/uit-gegevens nodig.

Afhankelijk van het percentage beschik bare gegevens punten voor de geselecteerde duur, wordt de betrouwbaarheids classificatie voor een evaluatie gegeven, zoals beschreven in de volgende tabel.

   **Beschik baarheid van gegevens punt** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren


## <a name="common-assessment-issues"></a>Algemene evaluatie problemen

Hier vindt u informatie over het oplossen van enkele veelvoorkomende omgevings problemen die van invloed zijn op evaluaties.

###  <a name="out-of-sync-assessments"></a>Evaluaties buiten de synchronisatie

Als u computers toevoegt aan of verwijdert uit een groep nadat u een evaluatie hebt gemaakt, wordt de door u gemaakte evaluatie gemarkeerd als **out-of-Sync**. Voer de evaluatie opnieuw uit(herberekening) om de groeps wijzigingen weer te geven.

### <a name="outdated-assessments"></a>Verouderde evaluaties

Als er on-premises wijzigingen zijn aangebracht aan virtuele machines die zich in een groep bevinden die is geëvalueerd, wordt de evaluatie gemarkeerd als verouderd. Voer de evaluatie opnieuw uit om de wijzigingen weer te geven.

### <a name="low-confidence-rating"></a>Beoordeling met lage betrouw baarheid

Een evaluatie heeft mogelijk niet alle gegevens punten om een aantal redenen:

- U hebt uw omgeving niet geprofileerd gedurende de periode waarvoor u de evaluatie maakt. Als u bijvoorbeeld een *evaluatie op basis van prestaties* maakt waarbij de duur van de prestaties is ingesteld op een week, moet u een periode van minstens een week wachten nadat u de detectie hebt gestart voor alle gegevens punten die u wilt verzamelen. U kunt altijd op **herberekenen** klikken om de meest recente beoordeling van het vertrouwen te bekijken. De vertrouwens classificatie is alleen van toepassing wanneer u een evaluatie *op basis van prestaties* maakt.

- Er zijn enkele VM's uitgeschakeld geweest in de periode waarover de evaluatie wordt berekend. Als sommige Vm's al enige tijd zijn uitgeschakeld, kan de server evaluatie de prestatie gegevens voor die periode niet verzamelen.

- Er zijn enkele Vm's gemaakt nadat de detectie in de server evaluatie is gestart. Als u bijvoorbeeld een evaluatie maakt voor de prestatiegeschiedenis van de laatste maand, maar er een week geleden enkele VM's in de omgeving zijn gemaakt. In dit geval zijn de prestatie gegevens voor de nieuwe Vm's niet beschikbaar voor de hele duur en is de betrouwbaarheids classificatie laag.


## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen worden berekend.
- [Meer informatie](how-to-modify-assessment.md) over het aanpassen van een evaluatie.
