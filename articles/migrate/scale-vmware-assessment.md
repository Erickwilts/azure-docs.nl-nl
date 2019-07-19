---
title: Evalueer een groot aantal virtuele VMware-machines voor migratie naar Azure met Azure Migrate | Microsoft Docs
description: Hierin wordt beschreven hoe u een groot aantal virtuele VMware-machines kunt beoordelen voor migratie naar Azure met behulp van de Azure Migrate-service.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: c9c57a07100f2ea6db86408826bf74d05c8df5aa
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868684"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Een groot aantal virtuele VMware-machines beoordelen voor migratie naar Azure


In dit artikel wordt beschreven hoe u grote aantallen (1000-35000) van on-premises virtuele VMware-machines kunt beoordelen voor migratie naar Azure, met behulp van het hulp programma Azure Migrate server Assessment

[Azure migrate](migrate-services-overview.md) biedt een hub aan hulpprogram ma's waarmee u apps, infra structuur en werk belastingen op Microsoft Azure kunt detecteren, evalueren en migreren. De hub bevat Azure Migrate-hulpprogram ma's en ISV-aanbiedingen (Independent Software Vendor) van derden. 

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Plan voor evaluatie op schaal.
> * Configureer Azure-machtigingen en bereid VMware voor evaluatie voor.
> * Maak een Azure Migrate project en maak een evaluatie.
> * Controleer de evaluatie tijdens het plannen van de migratie.


> [!NOTE]
> Als u een haalbaarheids test wilt uitproberen om een aantal Vm's te beoordelen voordat u op schaal controleert, volgt u de [reeks zelf](tutorial-prepare-vmware.md) studies

## <a name="plan-for-assessment"></a>Beoordeling plannen

Bij het plannen van de beoordeling van een groot aantal virtuele VMware-machines, zijn er een aantal dingen die u moet nadenken:

- **Azure migrate projecten plannen**: Bepaal hoe u Azure Migrate projecten implementeert. Als uw data centers zich bijvoorbeeld in verschillende geografische grafieken bevinden, of als u de meta gegevens voor detectie, analyses of migratie wilt opslaan in een andere geografie, hebt u mogelijk meerdere projecten nodig. 
- **Apparaten plannen**: Azure Migrate gebruikt een on-premises Azure Migrate apparaat, geïmplementeerd als een VMware-VM, om Vm's voortdurend te detecteren. Het apparaat bewaakt omgevings wijzigingen, zoals het toevoegen van Vm's, schijven of netwerk adapters. Ook worden er meta gegevens en prestatie gegevens naar Azure verzonden. U moet bepalen hoeveel apparaten er moeten worden geïmplementeerd.
- **Accounts voor detectie plannen**: Het Azure Migrate-apparaat maakt gebruik van een account met toegang tot vCenter Server om Vm's te ontdekken voor evaluatie en migratie. Als u meer dan 10.000 Vm's wilt detecteren, stelt u meerdere accounts in.


## <a name="planning-limits"></a>Plannings limieten
 
Gebruik de limieten in deze tabel voor de planning.

**Planning** | **Limieten**
--- | --- 
**Azure Migrate projecten** | Evalueer Maxi maal 35.000 Vm's in een project.
**Azure Migrate apparaat** | Een apparaat kan alleen verbinding maken met één vCenter Server.<br/><br/> Een apparaat kan alleen worden gekoppeld aan één Azure Migrate project.<br/> Een apparaat kan Maxi maal 10.000 Vm's op een vCenter Server detecteren.
**Azure Migrate beoordeling** | U kunt Maxi maal 35.000 Vm's in één evaluatie evalueren.

Hieronder volgen enkele voor beelden van implementaties:


**vCenter-server** | **Vm's op server** | **Aanbeveling** | **Actie**
---|---|---
Een' | < 10.000 | Eén Azure Migrate-project.<br/> Eén apparaat.<br/> Een vCenter-account voor detectie. | Apparaat instellen, verbinding maken met vCenter Server met een account.
Een' | > 10.000 | Eén Azure Migrate-project.<br/> Meerdere apparaten.<br/> Meerdere vCenter-accounts. | Stel het apparaat in voor elke 10.000 Vm's.<br/><br/> Stel vCenter-accounts in en Splits de inventaris om de toegang voor een account te beperken tot Maxi maal 10.000 Vm's.<br/> Elk apparaat verbinden met een vCenter-Server met een account.<br/> U kunt afhankelijkheden analyseren tussen computers die worden gedetecteerd met verschillende apparaten.
Meerdere | < 10.000 |  Eén Azure Migrate-project.<br/> Meerdere apparaten.<br/> Een vCenter-account voor detectie. | Toestellen instellen, verbinding maken met vCenter Server met een account.<br/> U kunt afhankelijkheden analyseren tussen computers die worden gedetecteerd met verschillende apparaten.
Meerdere | > 10.000 | Eén Azure Migrate-project.<br/> Meerdere apparaten.<br/> Meerdere vCenter-accounts. | Als vCenter Server detectie < 10.000 Vm's, moet u voor elke vCenter Server een apparaat instellen.<br/><br/> Als vCenter Server detectie > 10.000 Vm's, stelt u een apparaat in voor elke 10.000 Vm's.<br/> Stel vCenter-accounts in en Splits de inventaris om de toegang voor een account te beperken tot Maxi maal 10.000 Vm's.<br/> Elk apparaat verbinden met een vCenter-Server met een account.<br/> U kunt afhankelijkheden analyseren tussen computers die worden gedetecteerd met verschillende apparaten.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Detectie plannen in een omgeving met meerdere tenants

Als u van plan bent een omgeving met meerdere tenants te plannen, kunt u de detectie op de vCenter Server bereiken.

- U kunt het apparaat detectie bereik instellen op een vCenter Server Data Centers, clusters of map met clusters, hosts of de map hosts of afzonderlijke Vm's.
- Als uw omgeving wordt gedeeld door tenants en u elke Tenant afzonderlijk wilt detecteren, kunt u de toegang tot het vCenter-account beperken dat het apparaat voor detectie gebruikt. 
    - U kunt het bereik van VM-mappen beperken als de tenants hosts delen. Azure Migrate kan geen Vm's detecteren als het vCenter-account toegang heeft verleend op het niveau van de vCenter-VM-map. Als u uw detectie op basis van VM-mappen wilt beperken, kunt u dit doen door ervoor te zorgen dat het vCenter-account alleen-lezen toegang heeft dat is toegewezen op een VM-niveau. Meer informatie over het detecteren van scopes [vindt u hier](tutorial-assess-vmware.md#scoping-discovery).

## <a name="prepare-for-assessment"></a>Evaluatie voorbereiden

Azure en VMware voorbereiden voor Server evaluatie. 

1. Controleer de [vereisten en beperkingen voor VMware-ondersteuning](migrate-support-matrix-vmware.md).
2. Stel machtigingen in voor uw Azure-account om te communiceren met Azure Migrate.
3. VMware voorbereiden voor evaluatie.

Volg de instructies in [deze zelf studie](tutorial-prepare-vmware.md) om deze instellingen te configureren.


## <a name="create-a-project"></a>Een project maken

In overeenstemming met uw plannings vereisten gaat u als volgt te werk:

1. Een Azure Migrate projecten maken.
2. Voeg het hulp programma voor het evalueren van Azure Migrate-servers toe aan de projecten.

[Meer informatie](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Een evaluatie maken en bekijken

1. Maak evaluaties voor virtuele VMware-machines.
1. Bekijk de evaluaties in de voor bereiding op de migratie planning.


Volg de instructies in [deze zelf studie](tutorial-assess-vmware.md) om deze instellingen te configureren.
    

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u het volgende:
 
> [!div class="checklist"] 
> * Gepland om Azure Migrate Beoordelingen voor virtuele VMware-machines te schalen
> * Voor bereiding van Azure en VMware voor evaluatie
> * Een Azure Migrate project gemaakt en evaluaties uitgevoerd
> * Gereviseerde evaluaties in de voor bereiding voor de migratie.

Nu [leert u hoe](concepts-assessment-calculation.md) beoordelingen worden berekend en hoe u beoordelingen kunt [wijzigen](how-to-modify-assessment.md).
