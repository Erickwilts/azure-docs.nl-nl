---
title: Prijzen van Azure Security Center
description: Azure Security Center wordt met en zonder Azure Defender aangeboden.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 01d7d29e9fffff3be2c98c0ec52c01774439e3f7
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2020
ms.locfileid: "92341185"
---
# <a name="pricing-of-azure-security-center"></a>Prijzen van Azure Security Center
Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor werkbelastingen die worden uitgevoerd in Azure, on-premises en in andere clouds. Het biedt zichtbaarheid en controle over hybride cloudwerkbelastingen, actieve beveiligingen die uw blootstelling aan bedreigingen verminderen en intelligente detectie om u te helpen snel ontwikkelende cyberaanvallen voor te blijven.


## <a name="free-option-vs-azure-defender-enabled"></a>Gratis optie vs Azure Defender

Security Center wordt op twee manieren aangeboden:

- **Azure Defender UIT** (gratis). Security Center zonder Azure Defender is gratis ingeschakeld voor al uw huidige Azure-abonnementen wanneer u het Azure Security Center-dashboard voor het eerst bezoekt in de Azure-portal of als u programmatisch via API hebt ingeschakeld. Deze gratis modus biedt beveiligingsbeleid, continue beveiligingsevaluatie en praktische beveiligingsaanbevelingen voor het beveiligen van uw Azure-resources.

- **Azure Defender AAN** - Als u Azure Defender inschakelt, breidt dat de mogelijkheden van de gratis versie uit voor workloads die worden uitgevoerd in privé- en andere openbare clouds, met geïntegreerd beveiligingsbeheer en beveiliging tegen bedreigingen voor uw hybride cloudworkloads. Enkele belangrijke functies van Azure Defender:

    - **Microsoft Defender for Endpoint** - Azure Defender voor servers bevat [Microsoft Defender for Endpoint](https://www.microsoft.com/microsoft-365/security/endpoint-defender) voor uitgebreide eindpuntdetectie en -respons (EDR). Lees meer over de voordelen van het gebruik van Microsoft Defender for Endpoint met Azure Defender in [De geïntegreerde EDR-oplossing van Security Center gebruiken](security-center-wdatp.md).
    - **Beveiligingsproblemen voor virtuele machines en containerregisters** : implementeer eenvoudig een scanner op al uw virtuele machines met de meest geavanceerde oplossing voor het beheer van beveiligingsproblemen. Bekijk onderzoek en herstel de bevinden rechtstreeks binnen Security Center. 
    - **Hybride beveiliging** : krijg een uniforme weergave van beveiliging in al uw on-premises en cloudwerkbelastingen. Pas beveiligingsbeleid voor uw hybride-cloudworkloads toe en beoordeel de beveiliging van uw ervan om de naleving van beveiligingsstandaarden te waarborgen. Verzamel, zoek en analyseer beveiligingsgegevens van meerdere bronnen, waaronder firewalls en oplossingen van andere partners.
    - **Bedreigingswaarschuwingen** : geavanceerde gedragsanalyses en de Microsoft Intelligent Security Graph bieden een voordeel tegen ontwikkelende cyberaanvallen. Maak gebruik van gedragsanalyses en machine learning om aanvallen en zero-day-aanvallen te identificeren. Bewaak netwerken, machines en cloudservices voor inkomende aanvallen en activiteiten na inbreuk. Stroomlijn onderzoek met interactieve hulpprogramma's en bedreigingsinformatie in context.
    - **Toegang- en toepassingsbeheer** : blokkeer malware en andere ongewenste toepassingen door machine learning op basis van aanbevelingen toe te passen die is toegespitst op uw specifieke workloads om goedkeurings- en weigeringslijsten te maken. Verklein het oppervlak voor netwerkaanvallen met beheerde JIT-toegang tot beheerpoorten op Azure-VM's. Dit vermindert blootstelling aan brute kracht en andere netwerk aanvallen.
    - **Beveiligingsfuncties voor containers** : profiteer van beiligingsprobleembeheer en realtime bedreigingsbescherming in uw containeromgevingen. Wanneer u de **Azure Defender voor containerregisters** inschakelt, kan het tot 12 uur duren totdat alle functies zijn ingeschakeld. Kosten zijn gebaseerd op het aantal unieke containerinstallatiekopieën dat naar uw gekoppelde register wordt gepusht. Nadat een installatiekopie is gescand, worden er geen kosten meer in rekening voor gebracht, tenzij deze wordt bewerkt en nogmaals wordt gepusht. 

## <a name="try-azure-defender-free-for-30-days"></a>Azure Defender gratis uitproberen gedurende 30 dagen

Azure Defender is gratis gedurende de eerste 30 dagen. Als u aan het einde van 30 dagen wilt door gaan met het gebruik van de service, worden er automatisch kosten voor gebruik in rekening gebracht.

## <a name="enable-azure-defender"></a>Azure Defender inschakelen

U kunt een volledig Azure-abonnement beschermen met Azure Defender en de beschermingen worden overgenomen door alle resources binnen het abonnement.

Azure Defender inschakelen:

1. Selecteer **Prijzen en instellingen** in het hoofdmenu van Security Center.
1. Selecteer het abonnement dat u wilt upgraden.
1. Selecteer **Azure Defender aan** als u een upgrade wilt uitvoeren.
1. Selecteer **Opslaan** .

Hieronder volgt de prijspagina voor een voorbeeldabonnement. U ziet dat elk abonnement in Azure Defender afzonderlijk wordt geprijsd en afzonderlijk op aan of uit kan worden ingesteld.

:::image type="content" source="./media/security-center-pricing/pricing-tier-page.png" alt-text="De pagina met prijzen van Security Center in de portal":::

> [!NOTE]
> Als u de alle bedreigingsbeschermingsfuncties van Security Center wilt inschakelen, moet u Azure Defender inschakelen voor het abonnement dat de van toepassing zijnde workloads bevat. Als u dit op werkruimteniveau inschakelt, betekent dat niet dat just-in-time toegang tot VM's wordt ingeschakeld, evenmin als adaptieve toepassingscontroles en netwerkdetecties voor Azure-resources. Daarnaast zijn de enige Azure Defender-abonnementen die op werkruimteniveau beschikbaar zijn Azure Defender voor servers en Azure Defender voor SQL-servers op computers.
>
> U kunt **Azure Defender for Storage-accounts** inschakelen op abonnementsniveau of op resourceniveau.
> U kunt **Azure Defender for SQL** inschakelen op abonnementsniveau of op resourceniveau.
> U kunt bedreigingsbeveiliging voor **Azure Database for MariaDB/MySQL/PostgreSQL** alleen inschakelen op resourceniveau.


## <a name="next-steps"></a>Volgende stappen
In dit artikel zijn de prijzen voor Security Center geïntroduceerd. Zie voor gerelateerd materiaal:

- [De kosten van uw Azure-werkbelasting optimaliseren](https://azure.microsoft.com/blog/how-to-optimize-your-azure-workload-costs/)
- [Prijsinformatie in de gewenste valuta en op basis van uw regio](https://azure.microsoft.com/pricing/details/security-center/)
- Mogelijk wilt u uw kosten beheren en het aantal verzamelde gegevens voor oplossing beperken door deze te beperken tot een bepaalde set agents. Met [gerichte oplossingen](../azure-monitor/insights/solution-targeting.md) kunt u een bereik instellen voor de oplossing en u richten op een subset computers in de werkruimte. Als u gerichte oplossingen gebruikt, vermeldt Security Center dat de werkruimte geen oplossing heeft.