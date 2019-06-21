---
title: 'Zelfstudie: Azure verlagen met aanbevelingen voor optimalisatie | Microsoft Docs'
description: Deze zelfstudie helpt u om Azure-kosten verlagen wanneer u actie op optimalisatie aanbevelingen ondernemen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 9306e44655bd172343f20ac4fda2b2c56afcfb88
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164495"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Zelfstudie: De kosten van aanbevelingen optimaliseren

Kostenbeheer van Azure werkt met Azure Advisor, aanbevelingen voor kosten voor optimalisatie. Azure Advisor helpt u te optimaliseren en de efficiëntie door een niet-actieve en onderbenutte resources te identificeren. Deze zelfstudie leert u een voorbeeld waarin u weinig gebruikte Azure-resources identificeren en vervolgens het ondernemen van actie om kosten te verlagen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Optimalisatie van aanbevelingen om weer te geven inefficiënt mogelijke gebruik weergeven
> * Reageren op een aanbeveling klikken om het formaat van een virtuele machine naar een meer voordelige optie zijn
> * Controleer of de actie om ervoor te zorgen dat de virtuele machine met succes is gewijzigd

## <a name="prerequisites"></a>Vereisten
Aanbevelingen zijn beschikbaar voor verschillende bereiken en typen van de Azure-account, inclusief [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) klanten. Zie voor de volledige lijst met ondersteunde accounttypen [Gegevens van Azure Cost Management begrijpen](understand-cost-mgt-data.md). U moet minimaal leestoegang hebben tot een of meer van de volgende bereiken om kostengegevens te kunnen bekijken. Zie voor meer informatie over bereiken [begrijpen en werk met een bereik](understand-work-scopes.md).

- Abonnement
- Resourcegroep

U moet actieve virtuele machines met ten minste 14 dagen van de activiteit hebben.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com/).

## <a name="view-cost-optimization-recommendations"></a>Optimalisatie aanbevelingen weergeven

Voor aanbevelingen voor kosten optimalisatie voor een abonnement, opent u het gewenste bereik in de Azure portal en selecteer **aanbevelingen van Advisor**.

Voor aanbevelingen voor een beheergroep, opent u het gewenste bereik in de Azure portal en selecteer **Cost analysis** in het menu. Gebruik de **bereik** pill om over te schakelen naar een ander bereik, zoals een beheergroep. Selecteer **aanbevelingen van Advisor** in het menu. Zie voor meer informatie over bereiken [begrijpen en werk met een bereik](understand-work-scopes.md).

![Aanbevelingen voor kosten Management Advisor wordt weergegeven in de Azure-portal](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

De lijst met aanbevelingen inefficiënt gebruik Hiermee wordt aangegeven of aanschafaanbevelingen die u kunnen helpen extra geld besparen. De berekend totaal **mogelijke jaarlijkse besparingen** toont de totale hoeveelheid die u besparen kunt als u afsluiten of de toewijzing van al uw VM's die voldoen aan de regels van de aanbeveling. Als u niet dat ze uitschakelen wilt, kunt u overwegen ze naar een minder dure VM-SKU vergroten of verkleinen.

De **Impact** categorie, samen met de **mogelijke jaarlijkse besparingen**, zijn ontworpen om u te helpen bij het identificeren van de aanbevelingen die u hebt de mogelijkheid om op te slaan zo veel mogelijk.

Hoge impact aanbevelingen zijn onder andere:
- [Gereserveerde virtuele machine-instanties om geld te besparen ten opzichte van betalen per gebruik kopen](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs)
- [Optimaliseer virtuele machine uitgaven per vergroten of verkleinen of weinig gebruikte exemplaren afsluiten](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances)
- [Standard-opslag gebruikt voor het opslaan van momentopnamen van Managed Disks](../advisor/advisor-cost-recommendations.md#use-standard-snapshots-for-managed-disks)

Normale impact aanbevelingen zijn onder andere:
- [Azure Data Factory-pijplijnen die niet verwijderen](../advisor/advisor-cost-recommendations.md#delete-azure-data-factory-pipelines-that-are-failing)
- [Kosten verlagen door het elimineren van niet-ingerichte ExpressRoute-circuits](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits)
- [Kosten verlagen door te verwijderen of opnieuw configureren van niet-actieve virtuele netwerkgateways](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways)

## <a name="act-on-a-recommendation"></a>Reageren op een aanbeveling

Azure Advisor bewaakt uw gebruik van virtuele machines gedurende 14 dagen en identificeert het vervolgens weinig gebruikte virtuele machines. Virtuele machines waarvan CPU-gebruik is vijf procent of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

De 5% of minder CPU-gebruik instelling is de standaardinstelling, maar u kunt de instellingen aanpassen. Zie voor meer informatie over het aanpassen van de instelling van de [configureren van de gemiddelde CPU-gebruik regel of de aanbeveling van de virtuele machine beperkt gebruikte](../advisor/advisor-get-started.md#configure-low-usage-vm-recommendation).

Hoewel sommige scenario's in laag gebruik standaard resulteren kunnen, kunt u vaak geld besparen door de grootte van uw virtuele machines te wijzigen naar minder dure grootten. De daadwerkelijke besparingen kunnen variëren als u ervoor een actie formaat wijzigen kiest. We nemen een voorbeeld van het formaat van een virtuele machine.

In de lijst met aanbevelingen, klikt u op de **afsluiten of de grootte ervan optimaal werden benut virtuele machines** aanbeveling. Kies in de lijst met kandidaten voor virtuele machine, een virtuele machine vergroten of verkleinen en klik vervolgens op de virtuele machine. Details van de virtuele machine worden weergegeven zodat u kunt controleren of de metrische gegevens over het Resourcegebruik. De **mogelijke jaarlijkse besparingen** waarde is wat u kunt besparen als u afgesloten of verwijder de virtuele machine. Formaat van een virtuele machine waarschijnlijk bespaart u geld, maar u het volledige bedrag van de mogelijke jaarlijkse besparingen worden niet opgeslagen.

![Voorbeeld van details van de aanbeveling](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

Controleer in de virtuele machine, het gebruik van de virtuele machine om te bevestigen dat het een kandidaat een geschikte grootte.

![Voorbeeld van de virtuele machine details historische gebruik weergeven](./media/tutorial-acm-opt-recommendations/vm-details.png)

Houd er rekening mee grootte van de huidige virtuele machine. Nadat u hebt gecontroleerd dat de virtuele machine moet worden gewijzigd, sluit u de details van de virtuele machine, zodat u de lijst met virtuele machines.

Selecteer in de lijst met kandidaten af te sluiten of vergroten of verkleinen, **grootte van de virtuele machine**.
![Voorbeeld van de aanbeveling met de optie voor de grootte van de virtuele machine](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Vervolgens krijgt u een lijst met beschikbare formaat opties. Kies de methode die de beste prestaties en kosteneffectiviteit voor uw scenario krijgt. In het volgende voorbeeld wordt de gekozen optie vergroot/verkleint uit een **DS14\_V2** naar een **DS13\_V2**. De aanbeveling volgen slaat $551.30/ maand of $6,615.60/ jaar.

![Voorbeeld van de lijst met beschikbare VM-grootten, waar u kunt een grootte kiezen](./media/tutorial-acm-opt-recommendations/choose-size.png)

Nadat u een geschikte grootte kiezen, klikt u op **Selecteer** de actie formaat te beginnen.

Formaat wijzigen, moet een actief actieve virtuele machine opnieuw op te starten. Als de virtuele machine zich in een productieomgeving, wordt u aangeraden de bewerking formaat wijzigen na kantooruren uit te voeren. Het opstarten van de planning inkorten onderbrekingen veroorzaakt door tijdelijk niet beschikbaar zijn.

## <a name="verify-the-action"></a>Controleer of de actie

Wanneer de VM-grootte voltooid is, ziet u een melding van Azure.

![Melding van de virtuele machine van de geslaagde gewijzigd](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Optimalisatie van aanbevelingen om weer te geven inefficiënt mogelijke gebruik weergeven
> * Reageren op een aanbeveling klikken om het formaat van een virtuele machine naar een meer voordelige optie zijn
> * Controleer of de actie om ervoor te zorgen dat de virtuele machine met succes is gewijzigd

Als u de aanbevolen procedures voor kostenbeheer al nog niet hebt gelezen en biedt uitgebreide hulp en principes rekening houden met het beheer van kosten.

> [!div class="nextstepaction"]
> [Best practices voor kosten](cost-mgt-best-practices.md)
