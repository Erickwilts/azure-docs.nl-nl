---
title: 'Azure Defender voor containerregister: de voordelen en functies'
description: Meer informatie over de voordelen en functies van Azure Defender voor containerregisters.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 05d7316ceccd67a8eeb1d081843307162ff210c1
ms.sourcegitcommit: d3c3f2ded72bfcf2f552e635dc4eb4010491eb75
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92558632"
---
# <a name="introduction-to-azure-defender-for-container-registries"></a>Inleiding tot Azure Defender voor containerregisters

Azure Container Registry (ACR) is een beheerde, privé-Docker-registerservice die uw containerinstallatiekopieën voor Azure-implementaties in een centraal register opslaat en beheert. Het is gebaseerd op het opensource Docker Registry 2.0.

Om alle registers op basis van Azure Resource Manager in uw abonnement te beschermen, schakelt u **Azure Defender voor containerregisters** in op abonnementsniveau. Security Center scant vervolgens installatiekopieën die naar het register worden gepusht, of installatiekopieën die in de laatste 30 dagen zijn opgehaald. Deze functie wordt in rekening gebracht per installatiekopie.

## <a name="what-are-the-benefits-of-azure-defender-for-container-registries"></a>Wat zijn de voordelen van Azure Defender voor containerregisters?

Security Center identificeert op Azure Resource Manager gebaseerde ACR-registers in uw abonnement en zorgt voor naadloze Azure-systeemeigen beoordeling van beveiligingsproblemen en beheer voor de installatiekopieën van uw register.

**Azure Defender voor containerregisters** bevat een scanner voor beveiligingsproblemen die de installatiekopieën in uw op Azure Resource Manager gebaseerde Azure Container Registry-registers scant en dieper inzicht biedt in de beveiligingsproblemen van uw installatiekopieën. De geïntegreerde scanner wordt aangestuurd door Qualys, de toonaangevende leverancier voor het scannen op beveiligingsproblemen.

Als er problemen worden gevonden - door Qualys of Security Center - krijgt u een melding in het dashboard van Security Center. Security Center geeft voor elk beveiligingsprobleem praktische aanbevelingen, een classificatie van de ernst en richtlijnen voor het oplossen van het probleem. Zie voor meer informatie over de aanbevelingen van Security Center voor containers de [Verwijzingenlijst met aanbevelingen](recommendations-reference.md#recs-containers).

Security Center filtert en classificeert de resultaten van de scanner. Wanneer een installatiekopie in orde is, wordt deze als zodanig gemarkeerd door Security Center. Security Center genereert alleen aanbevelingen voor de beveiliging ten aanzien van installatiekopieën waarvoor problemen moeten worden opgelost. Security Center geeft details en een classificatie van de ernst voor elk gemeld beveiligingsprobleem. Daarnaast geeft het richtlijnen voor het herstellen van de specifieke beveiligingsproblemen die in elke installatiekopie worden gevonden.

Door alleen een melding te geven als er problemen zijn, vermindert Security Center het potentieel voor ongewenste informatieve waarschuwingen.


## <a name="when-are-images-scanned"></a>Wanneer worden installatiekopieën gescand?

Er zijn drie triggers voor het scannen van installatiekopieën:

- **Bij push** : wanneer een installatiekopie naar het register wordt gepusht, wordt deze automatisch door Security Center gescand. U kunt het scannen van een installatiekopie activeren door deze naar uw opslagplaats te pushen.

- **Recent opgehaald** : aangezien er elke dag nieuwe beveiligingsproblemen worden ontdekt, scant **Azure Defender voor containerregisters** ook alle installatiekopieën die de afgelopen 30 dagen zijn opgehaald. Er zijn geen extra kosten verbonden aan het opnieuw scannen. Zoals hierboven gezegd, wordt u één keer per installatiekopie gefactureerd.

- **Bij import** : Azure Container Registry heeft functies voor het importeren van installatiekopieën in uw register vanuit Docker Hub, Microsoft-containerregister of een ander Azure-containerregister. **Azure Defender voor containerregisters** scant alle ondersteunde installatiekopieën die u importeert. Zie [Containerinstallatiekopieën importeren in een containerregister](../container-registry/container-registry-import-images.md) voor meer informatie.
 
De scan wordt doorgaans binnen 2 minuten voltooid, maar kan tot 15 minuten duren. Bevindingen worden beschikbaar gesteld als Security Center-aanbevelingen zoals deze:

[![Voorbeeldaanbeveling van Azure Security Center over beveiligingsproblemen die zijn gedetecteerd in een gehoste installatiekopie in Azure Container Registry (ACR)](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)


## <a name="how-does-security-center-work-with-azure-container-registry"></a>Hoe werkt Security Center met Azure Container Registry?

Hieronder vindt u een diagram op hoog niveau van de onderdelen en voordelen van het beveiligen van uw registers met Security Center.

![Overzicht op hoog niveau van Azure Security Center en Azure Container Registry (ACR)](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)




## <a name="faq-for-azure-container-registry-image-scanning"></a>Veelgestelde vragen over het scannen van Azure Container Registry-installatiekopieën

### <a name="how-does-security-center-scan-an-image"></a>Hoe scant Security Center een installatiekopie?
De installatiekopie wordt opgehaald uit het register. Vervolgens wordt hij uitgevoerd in een geïsoleerde sandbox met de Qualys-scanner, die een lijst met bekende beveiligingsproblemen extraheert.

Security Center filtert en classificeert de resultaten van de scanner. Wanneer een installatiekopie in orde is, wordt deze als zodanig gemarkeerd door Security Center. Security Center genereert alleen aanbevelingen voor de beveiliging ten aanzien van installatiekopieën waarvoor problemen moeten worden opgelost. Door alleen een melding te geven als er problemen zijn, vermindert Security Center het potentieel voor ongewenste informatieve waarschuwingen.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Kan ik de scanresultaten verkrijgen via REST API?
Ja. De resultaten staan onder [Subevaluaties REST API](/rest/api/securitycenter/subassessments/list/). U kunt ook gebruikmaken van Azure Resource Graph (ARG), de Kusto-achtige API voor al uw resources: een query kan een specifieke scan ophalen.

### <a name="what-registry-types-are-scanned-what-types-are-billed"></a>Welke registertypen worden gescand? Welke typen worden gefactureerd?
Zie [Beschikbaarheid](defender-for-container-registries-usage.md#availability)voor een lijst met de typen containerregisters die worden ondersteund door Azure Defender voor containerregisters.

Als u niet-ondersteunde registers verbindt met uw Azure-abonnement, worden deze niet gescand en worden er geen kosten in rekening gebracht.

### <a name="can-i-customize-the-findings-from-the-vulnerability-scanner"></a>Kan ik de bevindingen van de kwetsbaarheidsscanner aanpassen?
Ja. Als u een organisatorische behoefte hebt om een resultaat te negeren in plaats van dit te herstellen, kunt u het eventueel uitschakelen. Uitgeschakelde resultaten hebben geen invloed op uw beveiligingsscore en genereren geen ongewenste ruis.

[Meer informatie over het maken van rollen om bevindingen uit te schakelen uit het geïntegreerde evaluatiehulpprogramma voor beveiligingsproblemen](defender-for-container-registries-usage.md#disable-specific-findings-preview).



## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over beveiligingsfuncties voor containers van Security Center:

- [Azure Security Center en containerbeveiliging](container-security.md)

- [Inleiding tot Azure Defender for Kubernetes](defender-for-kubernetes-introduction.md)


