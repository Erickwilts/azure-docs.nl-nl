---
title: Aanbevelingen voor beveiliging in Azure Security Center
description: In dit document wordt uitgelegd hoe aanbevelingen in Azure Security Center u helpen uw Azure-resources te beveiligen en te blijven voldoen aan het beveiligings beleid.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: 7f6c0f2a311590219fb59bfe1ec63831c03e8af2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91314433"
---
# <a name="security-recommendations-in-azure-security-center"></a>Aanbevelingen voor beveiliging in Azure Security Center 
In dit onderwerp wordt uitgelegd hoe u de aanbevelingen in Azure Security Center kunt bekijken en begrijpen om u te helpen uw Azure-resources te beveiligen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze hand leiding.
>

## <a name="what-are-security-recommendations"></a>Wat zijn beveiligings aanbevelingen?

Aanbevelingen zijn acties die u kunt uitvoeren om uw resources te beveiligen.

Security Center analyseert periodiek de beveiligingsstatus van uw Azure-resources om mogelijke beveiligingsproblemen op te sporen. Vervolgens krijgt u aanbevelingen voor het oplossen van deze beveiligingsproblemen.

Elke aanbeveling biedt u het volgende:

- Een korte beschrijving van het probleem.
- De herstels tappen die u moet uitvoeren om de aanbeveling te implementeren.
- De betrokken resources.

## <a name="monitor-recommendations"></a>Aanbevelingen controleren <a name="monitor-recommendations"></a>

Security Center analyseert de beveiligings status van uw resources om mogelijke beveiligings problemen te identificeren. De tegel **aanbevelingen** onder **overzicht** toont het totale aantal aanbevelingen dat is geïdentificeerd door Security Center.

![Overzicht van Security Center](./media/security-center-recommendations/asc-overview.png)

1. Selecteer de **tegel aanbevelingen** onder **overzicht**. De lijst met **aanbevelingen** wordt geopend.

1. Aanbevelingen zijn onderverdeeld in beveiligings controles.

      ![Aanbevelingen gegroepeerd op beveiligings beheer](./media/security-center-recommendations/view-recommendations.png)

1. Vouw een besturings element uit en selecteer een specifieke aanbeveling om de pagina aanbeveling weer te geven.

    :::image type="content" source="./media/security-center-recommendations/recommendation-details-page.png" alt-text="Pagina aanbevelings Details." lightbox="./media/security-center-recommendations/recommendation-details-page.png":::

    De pagina bevat:

    - Knoppen op ondersteunde aanbevelingen **afdwingen** en **weigeren** (Zie [geen onjuiste configuraties met afdwingen/weigeren](prevent-misconfigurations.md))
    - **Ernst indicator**
    - **Interval voor versheid**  (indien van toepassing) 
    - **Beschrijving** : een korte beschrijving van het probleem
    - Herstels **tappen** : een beschrijving van de hand matige stappen die nodig zijn om het beveiligings probleem op de betrokken resources op te lossen. Voor aanbevelingen met snelle oplossing kunt u **herstel logica weer geven** selecteren voordat u de voorgestelde oplossing toepast op uw resources. 
    - **Betrokken resources** : uw resources worden in tabbladen gegroepeerd.
        - **Goede resources** : relevante bronnen die niet van invloed zijn op of waarop u het probleem al hebt opgelost.
        - **Slechte bronnen** : bronnen die nog steeds worden beïnvloed door het geïdentificeerde probleem.
        - **Niet-toepasselijke resources** : resources waarvoor de aanbeveling geen definitief antwoord kan geven. Het tabblad niet van toepassing bevat ook de redenen voor elke resource. 

            :::image type="content" source="./media/security-center-recommendations/recommendations-not-applicable-reasons.png" alt-text="Pagina aanbevelings Details.":::

## <a name="preview-recommendations"></a>Preview-aanbevelingen

Aanbevelingen die als **Preview** zijn gemarkeerd, zijn niet opgenomen in de berekeningen van uw beveiligde Score.

Ze moeten, waar mogelijk, nog steeds worden hersteld. Wanneer de preview-periode is afgelopen, worden ze dus meegerekend in uw score.

Een voorbeeld van een preview-aanbeveling:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Pagina aanbevelings Details.":::
 
## <a name="next-steps"></a>Volgende stappen

In dit document hebt u kennis gemaakt met beveiligings aanbevelingen in Security Center. Voor meer informatie over het oplossen van de aanbevelingen:

- [Aanbevelingen herstellen](security-center-remediate-recommendations.md) : informatie over het configureren van beveiligings beleid voor uw Azure-abonnementen en resource groepen.
- [Voorkom onjuiste configuraties met de aanbevelingen afdwingen/weigeren](prevent-misconfigurations.md).
