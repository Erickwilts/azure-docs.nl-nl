---
ms.topic: include
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.date: 07/09/2020
ms.openlocfilehash: de5d3f8f32e928c77ffd6028ec764793ab7229ac
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86495331"
---
## <a name="business-disaster-recovery"></a>Bedrijfs nood herstel

In deze sectie worden de functies van Azure Time Series Insights beschreven waarmee u apps en services kunt blijven gebruiken, zelfs als er sprake is van een nood *herstel*.

### <a name="high-availability"></a>Hoge beschikbaarheid

Als Azure-service biedt Azure Time Series Insights bepaalde functies met *hoge Beschik baarheid* door gebruik te maken van redundantie op het niveau van de Azure-regio. Azure biedt bijvoorbeeld ondersteuning voor herstel na nood gevallen via de functionaliteit voor de *Beschik baarheid van meerdere regio's* van Azure.

Meer functies voor hoge Beschik baarheid die worden geleverd via Azure (en ook beschikbaar voor elk Azure Time Series Insights exemplaar) zijn:

- **Failover**: Azure biedt [geo-replicatie en taak verdeling](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- Herstel van **gegevens** en **opslag herstel**: Azure biedt [verschillende opties om gegevens te bewaren en te herstellen](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Azure site Recovery**: Azure biedt functies voor site herstel via [Azure site Recovery](https://docs.microsoft.com/azure/site-recovery/).
- **Azure backup**: [Azure backup](https://docs.microsoft.com/azure/backup/backup-architecture) ondersteunt zowel on-premises als in-Cloud back-ups van virtuele Azure-machines.

Zorg ervoor dat u de relevante Azure-functies inschakelt om wereld wijd een hoge Beschik baarheid te bieden voor uw apparaten en gebruikers.

> [!NOTE]
> Als Azure is geconfigureerd om de beschik baarheid van meerdere regio's mogelijk te maken, is er geen aanvullende beschikbaarheids configuratie voor meerdere regio's vereist in Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT en Event hubs

Sommige Azure IoT-services bevatten ook ingebouwde functies voor nood herstel van het bedrijf:

- [Azure IOT hub herstel na nood geval met hoge Beschik baarheid](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), met inbegrip van de redundantie binnen de regio
- [Azure Event Hubs-beleid](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Azure Storage redundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Het integreren van Azure Time Series Insights met de andere services biedt extra mogelijkheden voor herstel na nood gevallen. Bijvoorbeeld, telemetrie die naar uw Event Hub is verzonden, kan worden opgeslagen in een back-up van de Azure Blob Storage-Data Base.

### <a name="azure-time-series-insights"></a>Azure Time Series Insights

Er zijn verschillende manieren om uw Azure Time Series Insights-gegevens,-apps en-services te houden, zelfs als deze worden onderbroken. 

U kunt echter wel bepalen dat een volledige back-up van uw Azure time series-omgeving ook vereist is voor de volgende doel einden:

- Als een *failover-exemplaar* specifiek voor Azure time series Insights om gegevens en verkeer om te leiden naar
- Gegevens en controle gegevens behouden

Over het algemeen is het een goed idee om een Azure Time Series Insights omgeving te dupliceren, het maken van een tweede Azure Time Series Insights omgeving in een Azure-regio voor back-ups. Gebeurtenissen worden ook naar deze secundaire omgeving verzonden vanuit de bron van uw primaire gebeurtenis. Zorg ervoor dat u een tweede toegewezen consumenten groep gebruikt. Volg de richt lijnen voor bedrijfs nood herstel van de bron, zoals eerder beschreven.

Een dubbele omgeving maken:

1. Maak een omgeving in een tweede regio. Lees [een nieuwe Azure time series Insights-omgeving maken in de Azure Portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)voor meer informatie.
1. Maak een tweede toegewezen consumenten groep voor uw gebeurtenis bron.
1. Verbind de bron van de gebeurtenis met de nieuwe omgeving. Zorg ervoor dat u de tweede toegewijde Consumer groep opgeeft.
1. Bekijk de Azure Time Series Insights- [IOT hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) en [Event hubs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentatie.

Als er zich een gebeurtenis voordoet:

1. Als uw primaire regio wordt beïnvloed tijdens een nood geval, moet u de bewerkingen opnieuw door sturen naar de back-upAzure Time Series Insights omgeving.
1. Gebruik uw tweede regio om back-ups te maken en alle Azure Time Series Insights telemetrie en query gegevens te herstellen.

> [!IMPORTANT]
> Als er een failover optreedt:
> 
> * Er kan ook een vertraging optreden.
> * Er kan een Momente piek in bericht verwerking optreden, omdat bewerkingen worden omgeleid.
> 
> Lees [beperkende latentie in azure time series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency)voor meer informatie.

