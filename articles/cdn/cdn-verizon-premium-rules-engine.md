---
title: Met behulp van de Azure CDN van Verizon Premium regels-engine HTTP-gedrag negeren | Microsoft Docs
description: De regelengine kunt u aanpassen hoe HTTP-aanvragen worden verwerkt door Azure CDN van Verizon Premium, zoals het blokkeren van de levering van bepaalde soorten inhoud, het definiëren van een cachebeleid en het wijzigen van HTTP-headers.
services: cdn
author: mdgattuso
ms.service: cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 81af3073d64e4379972568a57907a7fb2f82356d
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481727"
---
# <a name="override-http-behavior-using-the-azure-cdn-from-verizon-premium-rules-engine"></a>Met behulp van de Azure CDN van Verizon Premium regels-engine HTTP-gedrag negeren

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht

De regels-engine van Azure CDN kunt u aanpassen hoe HTTP-aanvragen worden verwerkt. Bijvoorbeeld, blokkeert de levering van bepaalde typen inhoud, het definiëren van een cachebeleid of het wijzigen van een HTTP-header. Deze zelfstudie wordt gedemonstreerd hoe u een regel maken waarmee het cachegedrag van CDN activa wordt gewijzigd. Zie voor meer informatie over de syntaxis voor regels-engine [naslaginformatie over Azure CDN de regelengine](cdn-verizon-premium-rules-engine-reference.md).

## <a name="access"></a>Access

Voor toegang tot de regelengine, moet u eerst selecteren **beheren** vanaf de bovenkant van de **CDN-profiel** pagina voor toegang tot de Azure CDN beheren pagina. Afhankelijk van of het eindpunt is geoptimaliseerd voor dynamic site acceleration (DSA), moet u vervolgens de regelengine met de set regels die geschikt is voor uw type eindpunt openen:

- Eindpunten die zijn geoptimaliseerd voor algemene webweergave of andere niet-DSA-optimalisatie:
    
    Selecteer de **HTTP grote** tabblad, en selecteer vervolgens **regels-Engine**.

    ![Regels-engine voor HTTP](./media/cdn-rules-engine/cdn-http-rules-engine.png)

- Eindpunten die zijn geoptimaliseerd voor DSA:
    
    Selecteer de **ADN** tabblad, en selecteer vervolgens **regels-Engine**.
    
    ADN is een term die wordt gebruikt door Verizon DSA-inhoud opgeven. Alle regels die u hier maakt, worden genegeerd door alle eindpunten in uw profiel die niet zijn geoptimaliseerd voor DSA.

    ![Regels-engine voor DSA](./media/cdn-rules-engine/cdn-dsa-rules-engine.png)

## <a name="tutorial"></a>Zelfstudie

1. Uit de **CDN-profiel** weergeeft, schakelt **beheren**.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-rules-engine/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.

2. Selecteer de **HTTP grote** tabblad, en selecteer vervolgens **regels-Engine**.
   
    De opties voor een nieuwe regel worden weergegeven.
   
    ![Opties voor de CDN nieuwe regel](./media/cdn-rules-engine/cdn-new-rule.png)
   
   > [!IMPORTANT]
   > De volgorde waarin meerdere regels worden weergegeven is van invloed op hoe ze worden verwerkt. Een volgende regel kan de handelingen die zijn opgegeven door een vorige regel negeren.
   >

3. Voer een naam in de **naam / beschrijving** tekstvak.

4. Identificeer het type van de regel geldt voor aanvragen. De voorwaarde van de overeenkomst standaard gebruiken **altijd**.
   
   ![Voorwaarde voor overeenkomst CDN-regel](./media/cdn-rules-engine/cdn-request-type.png)
   
   > [!NOTE]
   > Meerdere criteria voor overeenkomst zijn beschikbaar in de vervolgkeuzelijst. Voor informatie over de geselecteerde voorwaarde, selecteer het blauwe informatief pictogram aan de linkerkant.
   >
   >  Zie voor een gedetailleerde lijst met voorwaardelijke expressies, [voorwaardelijke expressies regelengine](cdn-verizon-premium-rules-engine-reference-match-conditions.md).
   >  
   > Zie voor een gedetailleerde lijst van criteria voor overeenkomst, [regels criteria voor overeenkomst regelengine](cdn-verizon-premium-rules-engine-reference-match-conditions.md).
   >
   >

5. Selecteren om toe te voegen een nieuwe functie, de **+** naast **functies**.  Selecteer in de vervolgkeuzelijst aan de linkerkant, **Force interne Max-Age**.  Voer in het tekstvak dat wordt weergegeven, **300**. De resterende standaardwaarden niet gewijzigd.
   
   ![Functie voor CDN-regel](./media/cdn-rules-engine/cdn-new-feature.png)
   
   > [!NOTE]
   > Meerdere functies zijn beschikbaar in de vervolgkeuzelijst. Voor informatie over de momenteel geselecteerde functie, selecteer het blauwe informatief pictogram aan de linkerkant.
   >
   > Voor **Force interne Max-Age**, van de asset `Cache-Control` en `Expires` headers worden overschreven om te bepalen wanneer de activa van de oorsprong in het CDN edge-knooppunt wordt vernieuwd. In dit voorbeeld Slaat het CDN edge-knooppunt de asset 300 seconden, of 5 minuten, voordat het de activa van de oorsprong wordt vernieuwd.
   >
   > Zie voor een gedetailleerd overzicht van functies, [functies regelengine](cdn-verizon-premium-rules-engine-reference-features.md).
   >
   >

6. Selecteer **toevoegen** naar de nieuwe regel niet opslaan.  De nieuwe regel is nu in afwachting van goedkeuring. Nadat deze is goedgekeurd, wordt de status verandert van **in behandeling XML** naar **Active XML**.
   
   > [!IMPORTANT]
   > Regels wijzigingen kunnen maximaal tien minuten worden doorgegeven via de Azure CDN duren.
   >
   >

## <a name="see-also"></a>Zie ook

- [Overzicht van Azure CDN](cdn-overview.md)
- [Verwijzing regelengine](cdn-verizon-premium-rules-engine-reference.md)
- [Criteria voor overeenkomst regelengine](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Voorwaardelijke expressies regelengine](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Functies regelengine](cdn-verizon-premium-rules-engine-reference-features.md)
- [Azure Fridays: Azure CDN krachtige nieuwe premium-functies](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)