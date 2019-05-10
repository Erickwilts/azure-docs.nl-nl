---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 4/30/2019
ms.author: shants
ms.custom: include file
ms.openlocfilehash: adf99b941a775f105d8c65da3ac6c11dc7257120
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416282"
---
Azure werkt periodiek een platform voor het verbeteren van de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur voor virtuele machines. Deze updates kunnen variëren van het patchen van software-onderdelen in de hosting-omgeving, netwerkonderdelen, upgraden naar het buiten gebruik stellen van hardware. De meeste van deze updates bevatten geen invloed hebben op de gehoste virtuele machines. Er zijn echter gevallen waar updates hebben invloed en Azure kiest de minste krachtige methode voor updates:

- Als een niet-rebootful-update mogelijk is, dat de virtuele machine wordt onderbroken terwijl de host is bijgewerkt of ze actief is gemigreerd naar een reeds bijgewerkte host.

- Als u onderhoud moet worden opgestart, krijgt u een kennisgeving van wanneer het onderhoud is gepland. Azure biedt ook een bepaalde periode waar u kunt beginnen het onderhoud zelf op een tijdstip die bij u past. Tijdvenster voor automatisch onderhoud is doorgaans 30 dagen, tenzij het wordt dringend worden onderhoud uit te voeren. Azure investeert ook in om te beperken van de gevallen wanneer de virtuele machines moeten opnieuw worden opgestart voor het onderhoud van de geplande platform. 

Deze pagina wordt beschreven hoe Azure presteert voor beide typen onderhoud. Zie voor meer informatie over niet-geplande gebeurtenissen (uitval), de beschikbaarheid van virtuele machines voor beheren [Windows](../articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).

Ontvangt u in de VM-melding over toekomstig onderhoud met behulp van de geplande gebeurtenissen voor [Windows](../articles/virtual-machines/windows/scheduled-events.md) of [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Zie voor 'procedures' informatie over het beheren van gepland onderhoud 'Meldingen gepland onderhoud verwerken' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-not-requiring-a-reboot"></a>Onderhoud niet opnieuw opstarten vereist

Zoals hierboven, moeten de meeste platform-updates worden uitgevoerd zonder gevolgen voor de klant-VM's. Bij nul impact-update niet mogelijk is kiest Azure het mechanisme voor updates die minimaal impactful klant virtuele machines. Meerderheid van deze gevolgen voor niet-nul-onderhoud zorgt ervoor dat kleiner is dan 10 seconden onderbreken voor de virtuele machine. In bepaalde gevallen worden geheugen behouden onderhoud mechanismen gebruikt die de virtuele machine gedurende maximaal 30 seconden onderbroken en behoudt u het geheugen in RAM-geheugen. De virtuele machine wordt vervolgens hervat en wordt de klok automatisch gesynchroniseerd. Onderhoud met statusbehoud geheugen werkt voor meer dan 90% Azure VM's met uitzondering van G, M, N en H-serie. Azure is steeds met behulp van technologieën voor livemigratie en verbeteren van geheugen onderhoud mechanisme voor het verminderen van de onderbrekingsduur te behouden.  

Deze onderhoudsbewerkingen niet rebootful zijn toegepast foutdomein door foutdomein en voortgang is gestopt als er health waarschuwingssignalen worden ontvangen. 

Sommige toepassingen wordt mogelijk beïnvloed door deze typen updates. Als de virtuele machine actief gemigreerd naar een andere host is, kunnen een lichte prestatievermindering in de aanloop naar de virtuele machine onderbreken minuten ziet u enkele gevoelige werkbelastingen. Dergelijke toepassingen kunnen profiteren van geplande gebeurtenissen voor [Windows](../articles/virtual-machines/windows/scheduled-events.md) of [Linux](../articles/virtual-machines/linux/scheduled-events.md) voorbereiden voor onderhoud van VM's en hebben geen invloed tijdens het onderhoud van Azure. Azure is ook Bezig met onderhoud beheren van functies voor zeer gevoelige toepassingen. 

### <a name="live-migration"></a>Live migratie

Livemigratie is een niet-rebootful-bewerking geheugen voor de virtuele machine de bestandsservergegevens en resulteert in een beperkte onderbreken of te blokkeren langdurig is doorgaans niet meer dan 5 seconden. Vandaag de dag alle infrastructuur als een Service (IaaS) virtuele Machines, afgezien van G, M, N en H-serie, komen in aanmerking voor livemigratie. Dit komt overeen met meer dan 90% van de IaaS-VM's geïmplementeerd op de Azure-vloot. 

Livemigratie wordt geïnitieerd door de Azure-infrastructuur in de volgende scenario's:
- Gepland onderhoud
- Hardware-uitval
- Optimalisaties van toewijzing

Livemigratie wordt gebruikt in sommige scenario's voor gepland onderhoud en geplande gebeurtenissen kunnen worden gebruikt om te weten vooraf wanneer de Live migratie operations start.

Livemigratie wordt ook gebruikt voor het verplaatsen van virtuele Machines af bij de hardware met een aanstaande voorspelde fout wanneer gedetecteerd door onze Machine Learning-algoritmen en toewijzingen van virtuele Machine te optimaliseren. Voor meer informatie over onze voorspellende modellering die exemplaren van verminderde hardware detecteert, raadpleegt u onze blogpost recht [tolerantie verbeteren van de virtuele Azure-Machine met voorspellende ML en livemigratie](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Klanten ontvangen een kennisgeving livemigratie altijd in hun Azure-portal in de Monitor / servicestatus-Logboeken, evenals als via geplande gebeurtenissen, als deze worden gebruikt.

## <a name="maintenance-requiring-a-reboot"></a>Onderhoud vereisen van opnieuw opstarten

In het zeldzame geval wanneer virtuele machines opnieuw worden opgestart voor gepland onderhoud, u krijgt een melding van tevoren. Gepland onderhoud heeft twee fasen: het selfservice-venster en een geplande onderhoudsvenster.

De **selfservice venster** kunt u het onderhoud starten op uw virtuele machines. Gedurende deze tijd die meestal vier weken, kunt u een query elke virtuele machine om te zien wat de status en controleer het resultaat van de laatste aanvraag voor onderhoud.

Als u selfservice-onderhoud start, wordt uw virtuele machine opnieuw geïmplementeerd naar een reeds bijgewerkte knooppunt. Omdat de virtuele machine opnieuw is opgestart, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt.

Als u selfservice-onderhoud starten en er een fout opgetreden tijdens het proces is, de bewerking is gestopt, de virtuele machine is niet bijgewerkt en krijgt u de mogelijkheid om opnieuw te proberen de selfservice-onderhoud. 

Wanneer het venster selfservice is verstreken, de **geplande onderhoudsvenster** begint. Tijdens deze periode, kunt u kunt nog steeds een query voor het onderhoudsvenster, maar kan niet worden gestart onderhoud zelf.

Zie voor meer informatie over het beheren van onderhoud vereisen van opnieuw opstarten 'Meldingen gepland onderhoud verwerken' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Beschikbaarheidsoverwegingen tijdens gepland onderhoud 

Als u besluit om te wachten tot het geplande onderhoudsvenster, zijn er enkele aandachtspunten voor het onderhouden van de hoogst mogelijke beschikbaarheid van uw VM's. 

#### <a name="paired-regions"></a>Gekoppelde regio's

Elke Azure-regio is gekoppeld aan een andere regio binnen hetzelfde geografische gebied en samen ze een regionaal paar maken. Azure in de fase gepland onderhoud, worden alleen de virtuele machines in één regio van een paar regio bijgewerkt. Bijvoorbeeld bij het bijwerken van de virtuele machine in Noord-centraal VS, niet Azure alle virtuele machines in Zuid-centraal VS bijgewerkt op hetzelfde moment. Tegelijkertijd met US - oost kan er echter wel onderhoud plaatsvinden in andere regio's, zoals Europa - noord. Uw VM's inzicht in de werking van gekoppelde regio's, kunt u beter verdelen over regio's. Zie voor meer informatie, [Azure regioparen](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Beschikbaarheidssets en schaalsets

Bij het implementeren van een workload op virtuele Azure-machines, kunt u de virtuele machines in een beschikbaarheidsset voor maximale beschikbaarheid voor uw toepassing kunt maken. Dit zorgt ervoor dat tijdens een storing of rebootful onderhoud, ten minste één virtuele machine beschikbaar is.

Afzonderlijke virtuele machines worden in een beschikbaarheidsset verdeeld over maximaal 20 updatedomeinen (ud's). Tijdens het geplande onderhoud is op een bepaald moment slechts één updatedomein bijgewerkt. De volgorde van update-domeinen wordt bijgewerkt gebeurt niet altijd sequentieel worden verwerkt. 

Virtuele-machineschaalsets vormen een Azure compute-resource waarmee u kunt implementeren en beheren van een set identieke VM's als één bron. De schaalset wordt automatisch geïmplementeerd in meerdere updatedomeinen, zoals virtuele machines in een beschikbaarheidsset. Net als met beschikbaarheidssets, wordt met schaalsets voor slechts één updatedomein bijgewerkt op een bepaald moment tijdens gepland onderhoud.

Zie voor meer informatie over het configureren van uw VM's voor hoge beschikbaarheid, de beschikbaarheid van uw virtuele machines beheren voor [Windows](../articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).
