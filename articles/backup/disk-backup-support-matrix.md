---
title: Ondersteunings matrix voor Azure Disk Backup
description: Hierin wordt een overzicht gegeven van de ondersteunings instellingen en beperkingen voor Azure-schijf back-ups.
ms.topic: conceptual
ms.date: 01/07/2021
ms.custom: references_regions
ms.openlocfilehash: 950651148237c7b9374c378e27ef5cd76697ae9e
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2021
ms.locfileid: "98557872"
---
# <a name="azure-disk-backup-support-matrix-in-preview"></a>Ondersteunings matrix voor Azure Disk Backup (in Preview)

>[!IMPORTANT]
>Azure Disk Backup is in Preview zonder service level agreement en wordt niet aanbevolen voor productie werkbelastingen. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
>[Vul dit formulier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u) in om u aan te melden voor de preview-versie.

U kunt [Azure backup](https://docs.microsoft.com/azure/backup/backup-overview) gebruiken om Azure-schijven te beveiligen. In dit artikel vindt u een overzicht van de beschik baarheid van regio's, ondersteunde scenario's en beperkingen.

## <a name="supported-regions"></a>Ondersteunde regio’s

Azure Disk Backup is beschikbaar als preview-versie in de volgende regio's: West-Centraal vs. 

Er worden meer regio's aangekondigd wanneer deze beschikbaar komen.

## <a name="limitations"></a>Beperkingen

- Azure Disk Backup wordt ondersteund voor Azure Managed Disks, inclusief gedeelde schijven (gedeelde Premium Ssd's). Niet-beheerde schijven worden niet ondersteund. Deze oplossing biedt momenteel geen ondersteuning voor Ultra-disks, met inbegrip van gedeelde Ultra schijven vanwege een gebrek aan momentopname mogelijkheden.

- Azure Disk Backup ondersteunt back-ups van Write Accelerator schijf. Tijdens het herstellen wordt de schijf echter teruggezet als een normale schijf. Het schrijven van de versnelde cache kan worden ingeschakeld op de schijf nadat deze is gekoppeld aan een virtuele machine.

- Azure Backup biedt operationele (moment opname) laag back-up van Azure Managed disks met ondersteuning voor meerdere back-ups per dag. De back-ups worden niet gekopieerd naar de back-upkluis.

- Op dit moment wordt de optie voor het Original-Location herstel (herstellen) hersteld door bestaande bron schijven te vervangen van waaruit de back-ups zijn gemaakt niet ondersteund. U kunt herstellen vanaf herstel punt om een nieuwe schijf te maken in dezelfde resource groep als die van de bron schijf van waaruit de back-ups zijn gemaakt of in een andere resource groep. Dit wordt Alternate-Location herstel (ALR) genoemd.

- Azure Backup voor Managed Disks gebruikt incrementele moment opnamen, die zijn beperkt tot 200 moment opnamen per schijf. Als u back-ups op aanvraag wilt maken van geplande back-ups, beperkt het back-upbeleid de totale back-ups tot 180. Meer informatie over [incrementele moment opnamen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal#restrictions) voor beheerde schijven.

- De [limieten](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-machine-disk-limits) voor Azure-abonnementen en-services zijn van toepassing op het totale aantal moment opnamen per regio per abonnement.

- Punt-in-time moment opnamen van meerdere schijven die zijn gekoppeld aan een virtuele machine worden niet ondersteund.

- De back-upkluis en de schijven waarvan een back-up moet worden gemaakt, kunnen zich in dezelfde of verschillende abonnementen bevindt. Zowel de back-upkluis als de schijf waarvan een back-up moet worden gemaakt, moeten zich echter in dezelfde regio bevinden.

- Schijven waarvan een back-up moet worden gemaakt en de resource groep met moment opnamen waarvan de moment opnamen lokaal worden opgeslagen, moeten zich in hetzelfde abonnement bevinden.

- Het herstellen van een schijf van een back-up naar hetzelfde of een ander abonnement wordt ondersteund. De herstelde schijf wordt echter gemaakt in dezelfde regio als die van de moment opname.

- Versleutelde ADE-schijven kunnen niet worden beveiligd.

- Schijven die zijn versleuteld met door het platform beheerde sleutels of door de klant beheerde sleutels worden ondersteund. De herstel punten van een schijf die is versleuteld met door de klant beheerde sleutels (CMK), kunnen echter niet meer worden hersteld als de sleutel voor het instellen van een schijf versleuteling is uitgeschakeld of verwijderd.

- Het back-upbeleid kan momenteel niet worden gewijzigd en de resource groep van de moment opname die is toegewezen aan een back-upexemplaar wanneer u de back-up van een schijf configureert, kan niet worden gewijzigd.

- Op dit moment is de Azure Portal-ervaring voor het configureren van de back-up van schijven beperkt tot Maxi maal 20 schijven van hetzelfde abonnement.

- Bij het configureren van de back-up moet de schijf die u hebt geselecteerd om een back-up te maken en de resource groep met moment opnamen waarvan de moment opnamen moeten worden opgeslagen, deel uitmaken van hetzelfde abonnement. U kunt geen incrementele moment opname maken voor een bepaalde schijf buiten het abonnement van de schijf. Meer informatie over [incrementele moment opnamen](https://docs.microsoft.com/azure/virtual-machines/windows/disks-incremental-snapshots-portal#restrictions) voor beheerde schijven. Zie  [back-up configureren](backup-managed-disks.md#configure-backup)voor meer informatie over het kiezen van een resource groep voor moment opnamen.

- Voor geslaagde back-up-en herstel bewerkingen zijn roltoewijzingen vereist voor de beheerde identiteit van de back-upkluis. Gebruik alleen de roldefinities die in de documentatie zijn opgenomen. Het gebruik van andere rollen, zoals owner, Inzender, enzovoort, wordt niet ondersteund. U kunt machtigings problemen ondervinden als u na het toewijzen van rollen begint met het configureren van back-up-of herstel bewerkingen. Dit komt doordat het enkele minuten duren voordat de roltoewijzingen van kracht worden.

- Met Managed disks kunt u de prestatie-laag wijzigen tijdens de implementatie of daarna zonder de grootte van de schijf te wijzigen. De oplossing voor de back-up van Azure-schijven ondersteunt de laag van de prestatie tier van de bron schijf waarvan een back-up wordt gemaakt. Tijdens het herstellen is de prestatie tier van de herstelde schijf hetzelfde als die van de bron schijf op het moment van de back-up. Volg de documentatie [hier](https://docs.microsoft.com/azure/virtual-machines/disks-performance-tiers-portal) om de prestatie-laag van uw schijf te wijzigen nadat de herstel bewerking is uitgevoerd.

- Met de ondersteuning voor [persoonlijke koppelingen](https://docs.microsoft.com/azure/virtual-machines/disks-enable-private-links-for-import-export-portal) voor Managed disks kunt u het exporteren en importeren van beheerde schijven beperken zodat deze alleen plaatsvindt in uw virtuele Azure-netwerk. Azure Disk Backup ondersteunt back-ups van schijven waarop privé-eind punten zijn ingeschakeld. Dit omvat niet de back-upgegevens of moment opnamen die toegankelijk moeten zijn via het persoonlijke eind punt.

## <a name="next-steps"></a>Volgende stappen

- [Back-up maken van Azure Managed Disks](backup-managed-disks.md)
