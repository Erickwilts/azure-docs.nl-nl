---
title: Wat is Azure Monitor voor virtuele machines (preview)? | Microsoft Docs
description: Azure Monitor voor virtuele machines is een functie van Azure Monitor die de status en prestaties bewaken van het besturingssysteem van de virtuele machine van Azure, evenals automatisch detecteren van onderdelen van de toepassing en afhankelijkheden met andere resources worden gecombineerd en wijst de communicatie tussen beide. In dit artikel bevat een overzicht.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2019
ms.author: magoedte
ms.openlocfilehash: 7d86b3fe9aeddd603d0c40b1c760cabdee42e396
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522114"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>Wat is Azure Monitor voor virtuele machines (preview)?

Azure Monitor voor virtuele machines bewaakt uw Azure virtual machines (VM) en virtuele-machineschaalsets op schaal. De service analyseert de prestaties en status van uw Windows- en Linux-VM's en bewaakt hun processen en afhankelijkheden van andere resources en externe processen. 

Azure Monitor voor virtuele machines bevat als een oplossing, ondersteuning voor het bewaken van prestatie- en afhankelijkheden voor virtuele machines die zich on-premises gehost of in een andere cloudprovider. Drie belangrijke functies bieden een diepgaande informatie:

* **Logische onderdelen van Azure-VM's met Windows en Linux**: Ten opzichte van de van vooraf geconfigureerde gezondheidscriteria worden gemeten en ze een melding wanneer de geëvalueerde voorwaarde wordt voldaan.  

* **Vooraf gedefinieerde trending prestatiegrafieken**: Core maatstaven voor prestaties van het gastbesturingssysteem van de virtuele machine weergegeven.

* **Kaart van afhankelijkheden**: Geeft de onderling verbonden onderdelen met de virtuele machine vanuit de verschillende resourcegroepen en abonnementen.  

De functies zijn ingedeeld in drie perspectieven:

* Status
* Prestaties
* Kaart

>[!NOTE]
>De Health-functie wordt momenteel aangeboden alleen voor virtuele machines van Azure. Prestaties en functies van de kaart ondersteuning voor Azure-VM's, Azure VM-schaalsets en virtuele machines die worden gehost in uw omgeving of andere cloudprovider.

Integratie met Azure Monitor Logboeken biedt krachtige aggregatie en filteren, en deze gegevenstrends na verloop van tijd kunt analyseren. Dergelijke uitgebreide werkbelasting bewaking kan niet worden bereikt met Azure Monitor of alleen Serviceoverzicht.  

U kunt deze gegevens weergeven in een enkele virtuele machine van de virtuele machine rechtstreeks of u kunt Azure Monitor gebruiken voor het leveren van een samengevoegde weergave van uw virtuele machines. In deze weergave is gebaseerd op van elke functie perspectief:

* **Status**: De virtuele machines zijn gekoppeld aan een resourcegroep.
* **Kaart** en **prestaties**: De virtuele machines zijn geconfigureerd voor rapportage aan een specifieke Log Analytics-werkruimte.

![Perspectief van de virtuele machine inzicht in de Azure-portal](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

Azure Monitor voor virtuele machines kan bieden voorspelbare prestaties en beschikbaarheid van essentiële toepassingen. Het identificeert kritieke besturingssysteemgebeurtenissen, knelpunten in de prestaties en netwerkproblemen. Azure Monitor voor virtuele machines kunt u inzicht in of een probleem is gerelateerd aan andere afhankelijkheden.  

## <a name="data-usage"></a>Gegevensgebruik 

Wanneer u Azure Monitor voor virtuele machines implementeert, wordt de gegevens die worden verzameld door uw virtuele machines die zijn opgenomen en opgeslagen in Azure Monitor. Metrische gegevens voor servicestatus criteria worden opgeslagen in Azure Monitor in een tijdreeksdatabase, prestaties en afhankelijkheid van de verzamelde gegevens worden opgeslagen in een Log Analytics-werkruimte. Op de prijzen die gepubliceerd op basis van de [Azure Monitor-pagina met prijzen](https://azure.microsoft.com/pricing/details/monitor/), Azure-Monitor voor virtuele machines wordt in rekening gebracht voor:

* De gegevens die is opgenomen en opgeslagen.
* Het nummer van de gezondheid van criteria metriek time series die worden bewaakt.
* De regels voor waarschuwingen die zijn gemaakt.
* De meldingen die worden verzonden. 

Grootte van het logboekbestand is afhankelijk van de lengte van de tekenreeks van prestatiemeteritems en deze kunt verhogen met het aantal logische schijven en netwerkadapters die zijn toegewezen aan de virtuele machine. Als u al een werkruimte hebt en deze prestatiemeteritems zijn verzameld, worden geen kosten verbonden aan dubbele toegepast. Als u al Serviceoverzicht, is de enige wijziging u ziet de extra verbinding-gegevens die worden verzonden naar Azure Monitor.

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over de vereisten en de methoden die u helpen bij uw virtuele machines controleren, [implementeert Azure Monitor voor virtuele machines](vminsights-enable-overview.md).
