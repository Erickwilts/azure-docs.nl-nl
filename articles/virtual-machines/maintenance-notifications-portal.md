---
title: De portal gebruiken voor onderhouds meldingen voor Azure-Vm's
description: Bekijk onderhouds meldingen voor virtuele machines die in Azure worden uitgevoerd en start self-service onderhoud met behulp van de portal.
services: virtual-machines
author: shants123
tags: azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 759fbc5ba3c5eaa78fec1045bcf41969108d39b1
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535819"
---
# <a name="handling-planned-maintenance-notifications-using-the-portal"></a>Geplande onderhouds meldingen verwerken met behulp van de portal

**Dit artikel is van toepassing op virtuele machines met Linux en Windows.**

Zodra een [geplande onderhouds](maintenance-notifications.md) Golf is gepland, kunt u controleren op een lijst met virtuele machines die worden beïnvloed. 

U kunt de Azure Portal gebruiken en zoeken naar Vm's die zijn gepland voor onderhoud.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in de linkernavigatiebalk op **virtual machines**.

3. Selecteer in het deel venster Virtual Machines de knop **kolommen bewerken** om de lijst met beschik bare kolommen te openen.

4. Selecteer en voeg de volgende kolommen toe:

   **Onderhouds status**: hier wordt de onderhouds status voor de virtuele machine weer gegeven. Hier volgen de mogelijke waarden:
      
      | Waarde | Beschrijving |
      |-------|-------------|
      | Aan de slag | De virtuele machine bevindt zich in het self-service-onderhouds venster waarmee u het onderhoud zelf kunt initiëren. Hieronder vindt u informatie over het starten van onderhoud op uw virtuele machine. | 
      | Gepland | Er is onderhoud gepland voor uw virtuele machine, zonder mogelijkheid dit onderhoud zelf te starten. U kunt het onderhouds venster leren door het venster onderhoud gepland te selecteren in deze weer gave of door te klikken op de virtuele machine. | 
      | Al bijgewerkt | De virtuele machine is al bijgewerkt en er is op dit moment geen verdere actie vereist. | 
      | Later opnieuw proberen | U hebt onderhoud gestart zonder slagen. U kunt de optie voor selfservice onderhoud op een later tijdstip gebruiken. | 
      | Nu opnieuw proberen | U kunt een eerder geslaagde, zelf-onderhouds werkzaamheden opnieuw proberen. | 
      | - | Uw VM maakt geen deel uit van een geplande onderhouds Golf. |
      

   **Onderhoud-self-service**: hier wordt het tijd venster weer gegeven wanneer u het onderhoud op uw virtuele machines zelf kunt starten.
   
   **Onderhouds venster**: toont het tijd venster wanneer Azure uw VM onderhoudt om de onderhouds werkzaamheden te volt ooien. 



## <a name="notification-and-alerts-in-the-portal"></a>Meldingen en waarschuwingen in de portal

Azure communiceert een planning voor gepland onderhoud door een e-mail bericht te verzenden naar de eigenaar van het abonnement en de groep mede-eigen aars. U kunt extra ontvangers en kanalen aan deze communicatie toevoegen door Azure-activiteiten logboek waarschuwingen te maken. Zie [waarschuwingen voor activiteiten logboek maken op service meldingen](../azure-monitor/platform/alerts-activity-log-service-notifications.md)voor meer informatie.

Zorg ervoor dat u het **gebeurtenis type** instelt als **gepland onderhoud**en **Services** als **Virtual Machine Scale sets** en/of **virtual machines**.

## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Onderhoud starten op uw virtuele machine vanuit de portal

Bij het bekijken van de details van de virtuele machine ziet u meer informatie over onderhoud.  
Boven in de weer gave VM-Details wordt een nieuw meldings lint toegevoegd als uw virtuele machine is opgenomen in een ' gepland onderhouds Golf '. Daarnaast wordt een nieuwe optie toegevoegd om onderhoud te starten wanneer dit mogelijk is. 


Klik op het onderhouds bericht om de onderhouds pagina te bekijken met meer informatie over het geplande onderhoud. Vanaf daar kunt u het onderhoud op uw virtuele machine **starten** .

Zodra u het onderhoud start, wordt uw virtuele machine behouden en wordt de onderhouds status bijgewerkt om het resultaat binnen enkele minuten weer te geven.

Als u het self-service venster hebt gemist, kunt u het venster nog steeds zien wanneer de virtuele machine wordt beheerd door Azure. 


## <a name="next-steps"></a>Volgende stappen

U kunt gepland onderhoud ook afhandelen met behulp van [Azure cli](maintenance-notifications-cli.md) of [Power shell](maintenance-notifications-powershell.md).