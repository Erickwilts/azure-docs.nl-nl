---
title: Problemen met toewijzing van Cloud Services oplossen | Microsoft Docs
description: Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
services: azure-service-management, cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 529157eb-e4a1-4388-aa2b-09e8b923af74
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 7830b2a5d065f54029839d250e35f3e1b3da2200
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945478"
---
# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Toewijzingsfouten oplossen die zijn opgetreden bij het implementeren van Cloud Services in Azure
## <a name="summary"></a>Samenvatting
Bij het implementeren van exemplaren in een Cloud service of het toevoegen van nieuwe web-of worker-rolinstanties, Microsoft Azure het toewijzen van reken resources. U kunt af en toe fouten ontvangen tijdens het uitvoeren van deze bewerkingen, zelfs voordat u de limieten voor Azure-abonnementen bereikt. In dit artikel worden de oorzaken beschreven van enkele algemene toewijzings fouten en suggesties voor mogelijke herstel bewerkingen. De informatie kan ook nuttig zijn bij het plannen van de implementatie van uw services.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Achtergrond: hoe de toewijzing werkt
De servers in Azure-datacenters worden in clusters gepartitioneerd. Er is een nieuwe aanvraag voor Cloud Service toewijzing in meerdere clusters geprobeerd. Wanneer de eerste instantie wordt geïmplementeerd in een Cloud service (in staging of productie), wordt die Cloud service vastgemaakt aan een cluster. Eventuele verdere implementaties voor de Cloud service worden uitgevoerd in hetzelfde cluster. In dit artikel wordt dit als ' vastgemaakt aan een cluster ' genoemd. Diagram 1 hieronder illustreert het geval van een normale toewijzing die wordt geprobeerd in meerdere clusters; Diagram 2 illustreert het geval van een toewijzing die is vastgemaakt aan cluster 2 omdat de CS_1 van de bestaande Cloud service wordt gehost.

![Toewijzings diagram](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Waarom toewijzings fout optreedt
Wanneer een toewijzings aanvraag is vastgemaakt aan een cluster, is er een hogere kans op het vinden van vrije bronnen omdat de beschik bare resource groep beperkt is tot een cluster. Als uw toewijzings aanvraag is vastgemaakt aan een cluster maar het type van de aangevraagde resource niet wordt ondersteund door dat cluster, mislukt de aanvraag zelfs als het cluster een gratis bron heeft. Diagram 3 hieronder illustreert het geval waarin een vastgemaakte toewijzing mislukt omdat het enige kandidaat-cluster geen vrije resources heeft. Diagram 4 illustreert het geval dat een vastgemaakte toewijzing mislukt omdat het enige kandidaat-cluster de aangevraagde VM-grootte niet ondersteunt, zelfs als het cluster vrije bronnen heeft.

![Fout bij vastgemaakte toewijzing](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Toewijzings fouten voor Cloud Services oplossen
### <a name="error-message"></a>Foutbericht
Mogelijk wordt het volgende fout bericht weer gegeven:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Algemene problemen
Hier volgen de algemene toewijzings scenario's die ervoor zorgen dat een toewijzings aanvraag wordt vastgemaakt aan één cluster.

* Implementeren naar Faserings sleuf: als een Cloud service in een van de sleuven een implementatie heeft, wordt de volledige Cloud service vastgemaakt aan een specifiek cluster.  Dit betekent dat als een implementatie al in de productiesite bestaat, dan kan een nieuwe faseringsimplementatie alleen worden toegewezen in hetzelfde cluster als de productiesite. Als de capaciteit van het cluster bijna is bereikt, kan de aanvraag mislukken.
* Schalen: het toevoegen van nieuwe exemplaren aan een bestaande Cloud service moet in hetzelfde cluster worden toegewezen.  Kleine schaalaanvragen kunnen doorgaans worden toegewezen, maar niet altijd. Als de capaciteit van het cluster bijna is bereikt, kan de aanvraag mislukken.
* Affiniteits groep: een nieuwe implementatie naar een lege Cloud service kan worden toegewezen door de infra structuur in een cluster in die regio, tenzij de Cloud service is vastgemaakt aan een affiniteits groep. Implementaties voor dezelfde affiniteits groep worden geprobeerd op hetzelfde cluster. Als de capaciteit van het cluster bijna is bereikt, kan de aanvraag mislukken.
* Het vNet-oudere virtuele netwerken van de affiniteits groep zijn gekoppeld aan affiniteits groepen in plaats van regio's, en Cloud Services in deze virtuele netwerken worden vastgemaakt aan het affiniteits groeps cluster. Implementaties van dit type virtuele netwerk worden geprobeerd op het vastgemaakte cluster. Als de capaciteit van het cluster bijna is bereikt, kan de aanvraag mislukken.

## <a name="solutions"></a>Oplossingen
1. Opnieuw implementeren naar een nieuwe Cloud service: deze oplossing is waarschijnlijk het meest succesvol omdat het platform kan kiezen uit alle clusters in die regio.

   * De werk belasting implementeren in een nieuwe Cloud service  
   * De CNAME-of A-record bijwerken om verkeer naar de nieuwe Cloud service te verwijzen
   * Als er geen verkeer naar de oude site wordt verzonden, kunt u de oude Cloud service verwijderen. Deze oplossing moet een downtime van nul tot gevolg hebben.
2. Productie-en staging-sleuven verwijderen: met deze oplossing blijft uw bestaande DNS-naam behouden, maar wordt uw toepassing downtime.

   * Verwijder de productie-en staging-sleuven van een bestaande Cloud service, zodat de Cloud service leeg is en
   * Maak een nieuwe implementatie in de bestaande Cloud service. Hiermee wordt opnieuw geprobeerd de toewijzing uit te voeren voor alle clusters in de regio. Zorg ervoor dat de Cloud service niet is gekoppeld aan een affiniteits groep.
3. Gereserveerd IP: deze oplossing behoudt uw bestaande IP-adres, maar veroorzaakt uitval tijd voor uw toepassing.  

   * Een ReservedIP maken voor uw bestaande implementatie met behulp van Power shell

     ```
     New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
     ```
   * Volg de bovenstaande #2 en zorg ervoor dat u de nieuwe ReservedIP opgeeft in het CSCFG-service.
4. Affiniteits groep voor nieuwe implementaties verwijderen-Affiniteits groepen worden niet meer aanbevolen. Volg de stappen voor #1 hierboven om een nieuwe Cloud service te implementeren. Zorg ervoor dat de Cloud service zich niet in een affiniteits groep bevindt.
5. Converteren naar een regionaal Virtual Network-Zie [How to migrate from Affinity groups to a regional Virtual Network (VNet) (Engelstalig)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
