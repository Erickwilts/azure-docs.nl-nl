---
title: Aggregatie van gebeurtenissen
description: Meer informatie over Defender voor IoT-gebeurtenis aggregatie.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2019
ms.author: mlottner
ms.openlocfilehash: aec750d246ce99fa65431e23ef68e70418db0017
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/22/2020
ms.locfileid: "90936875"
---
# <a name="defender-for-iot-event-aggregation"></a>Defender voor IoT-gebeurtenis aggregatie

Defender voor IoT-beveiligings agenten verzamelt gegevens en systeem gebeurtenissen van uw lokale apparaat en verzenden deze gegevens naar de Azure-Cloud voor verwerking en analyse. De beveiligings agent verzamelt veel soorten apparaatfuncties, waaronder nieuw proces en nieuwe verbindings gebeurtenissen. Het nieuwe proces en de nieuwe verbindings gebeurtenissen kunnen zich binnen een tweede keer regel matig voordoen op een apparaat, en hoewel belang rijk voor de robuuste en uitgebreide beveiliging, kan het aantal berichten dat beveiligings agenten voor verzen ding geforceerd snel worden bereikt of groter zijn dan uw IoT Hub quotum en kosten limieten. Deze gebeurtenissen bevatten echter zeer waardevolle beveiligings informatie die essentieel is voor het beveiligen van uw apparaat.

Om het extra quotum en de kosten te verminderen terwijl uw apparaten beveiligd blijven, kunnen Defender voor IoT-agents dit type gebeurtenissen samen stellen.

Gebeurtenis aggregatie is standaard **ingeschakeld** en hoewel dit niet wordt aanbevolen, kan op elk gewenst moment hand matig **worden uitgeschakeld.**

Aggregatie is momenteel beschikbaar voor de volgende typen gebeurtenissen:

* ProcessCreate
* ConnectionCreate
* ProcessTerminate (alleen Windows)

## <a name="how-does-event-aggregation-work"></a>Hoe werkt gebeurtenis aggregatie?

Wanneer gebeurtenis aggregatie is **ingeschakeld**, verzamelen Defender voor IOT-agents gebeurtenissen voor de interval periode of het tijd venster.
Zodra de interval periode is verstreken, verzendt de agent de geaggregeerde gebeurtenissen naar de Azure-Cloud voor verdere analyse.
De geaggregeerde gebeurtenissen worden in het geheugen opgeslagen totdat ze naar de Azure-Cloud worden verzonden.

De agent verhoogt het aantal treffers van deze specifieke gebeurtenis, wanneer de agent een identieke gebeurtenis verzamelt die al in het geheugen wordt bewaard, om het geheugen gebruik van de agent te verminderen. Wanneer het aggregatie tijd venster wordt door gegeven, verzendt de agent het aantal treffers van elk specifiek type gebeurtenis dat heeft plaatsgevonden. Gebeurtenis aggregatie is simpelweg de aggregatie van de treffer aantallen van elk verzameld type gebeurtenis.

Gebeurtenissen worden alleen als identiek beschouwd wanneer aan de volgende voor waarden wordt voldaan:

* ProcessCreate-gebeurtenissen: wanneer **commandline**, **uitvoer bare bestand**, **gebruikers naam**en **GebruikersID** identiek zijn
* ConnectionCreate-gebeurtenissen: wanneer **commandline**, **GebruikersID**, **Direction**, **lokaal adres**, **extern adres**, * * protocol en **doel poort** identiek zijn
* ProcessTerminate-gebeurtenissen: wanneer het **uitvoer bare bestand** en de status van het **Afsluiten** identiek zijn

### <a name="working-with-aggregated-events"></a>Werken met geaggregeerde gebeurtenissen

Tijdens aggregatie worden gebeurtenis eigenschappen die niet worden geaggregeerd, verwijderd en weer gegeven in log Analytics met de waarde 0.

* ProcessCreate-gebeurtenissen- **processId**en **parentProcessId** zijn ingesteld op 0
* ConnectionCreate-gebeurtenissen- **processId**en **bron poort** zijn ingesteld op 0

## <a name="event-aggregation-based-alerts"></a>Waarschuwingen op basis van gebeurtenis aggregatie

Na analyse maakt Defender voor IoT beveiligings waarschuwingen voor verdachte geaggregeerde gebeurtenissen. Waarschuwingen die zijn gemaakt op basis van geaggregeerde gebeurtenissen, worden slechts één keer voor elke geaggregeerde gebeurtenis weer gegeven.

De begin tijd van de aggregatie, de eind tijd en het aantal treffers voor elke gebeurtenis worden vastgelegd in het veld gebeurtenis **ExtraDetails** binnen log Analytics voor gebruik tijdens het onderzoek.

Elke samengevoegde gebeurtenis vertegenwoordigt een periode van 24 uur verzamelde waarschuwingen. Met het menu gebeurtenis opties in de linkerbovenhoek van elke gebeurtenis kunt u elke afzonderlijke statistische gebeurtenis **sluiten** .

## <a name="event-aggregation-twin-configuration"></a>Dubbele configuratie gebeurtenis aggregatie

Breng wijzigingen aan in de configuratie van Defender voor IoT-gebeurtenis aggregatie in het [object agent configuratie](how-to-agent-configuration.md) van de module dubbele identiteit van de module **azureiotsecurity** .

| Configuratie naam | Mogelijke waarden | Details | Opmerkingen |
|:-----------|:---------------|:--------|:--------|
| aggregationEnabledProcessCreate | boolean | Gebeurtenis aggregatie voor het maken van proces gebeurtenissen in-of uitschakelen |
| aggregationIntervalProcessCreate | ISO8601 time span-teken reeks | Aggregatie-interval voor het maken van proces gebeurtenissen |
| aggregationEnabledConnectionCreate | boolean| Aggregatie van gebeurtenissen in-of uitschakelen voor het maken van verbinding |
| aggregationIntervalConnectionCreate | ISO8601 time span-teken reeks | Aggregatie-interval voor het maken van verbindings gebeurtenissen |
| aggregationEnabledProcessTerminate | boolean | Gebeurtenis aggregatie voor proces beëindigings gebeurtenissen in-/uitschakelen | Alleen in Windows|
| aggregationIntervalProcessTerminate | ISO8601 time span-teken reeks | Aggregatie-interval voor gebeurtenissen voor proces beëindigen | Alleen in Windows|
|

## <a name="default-configurations-settings"></a>Standaard configuratie-instellingen

| Configuratie naam | Standaardwaarden |
|:-----------|:---------------|
| aggregationEnabledProcessCreate | true |
| aggregationIntervalProcessCreate | PT1H|
| aggregationEnabledConnectionCreate | true |
| aggregationIntervalConnectionCreate | PT1H|
| aggregationEnabledProcessTerminate | true |
| aggregationIntervalProcessTerminate | PT1H|
|

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de samen voeging van Defender voor IoT-beveiligings agent en de beschik bare opties voor gebeurtenis configuratie.

Als u aan de slag wilt gaan met Defender voor IoT-implementatie, gebruikt u de volgende artikelen:

- Meer informatie over [verificatie methoden voor beveiligings agenten](concept-security-agent-authentication-methods.md)
- Een [beveiligings agent](how-to-deploy-agent.md) selecteren en implementeren
- Bekijk de vereisten voor Defender voor IoT- [Services](service-prerequisites.md)
- Meer informatie over het [inschakelen van Defender voor IOT-service in uw IOT hub](quickstart-onboard-iot-hub.md)
- Meer informatie over de service van de [Veelgestelde vragen over Defender voor IOT](resources-frequently-asked-questions.md)
