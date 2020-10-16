---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 362c13771e7382ead1ba5aebd99a69fd86cd718c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "84793315"
---
<!-- operation names for the diag logs for IoT Hub -->

|Naam van bewerking|Niveau|Beschrijving|
|------------- |-----|-----------|
|UndefinedRouteEvaluation|Informatie|Het bericht kan niet worden geëvalueerd met een opgegeven voor waarde. Als bijvoorbeeld een eigenschap in de route query voorwaarde ontbreekt in het bericht. Meer informatie over de [syntaxis van routerings query's](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).|
|RouteEvaluationError|Fout|Er is een fout opgetreden bij het evalueren van het bericht vanwege een probleem met de bericht indeling. Deze fout wordt bijvoorbeeld vastgelegd als de inhouds codering niet is opgegeven of als het inhouds type niet geldig is in het bericht. Deze moeten worden ingesteld in de [systeem eigenschappen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#system-properties).|
|DroppedMessage|Fout|Het bericht is verwijderd en niet doorgestuurd. Dit kan worden veroorzaakt doordat het bericht niet overeenkomt met een routerings query of het eind punt onbestelbaar is en het bericht niet kan worden bezorgd na verschillende pogingen. Het is raadzaam om meer details op het eind punt op te halen met behulp van de REST API [eind punt status ophalen](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointUnhealthy|Fout|Het eind punt heeft geen berichten geaccepteerd van IoT Hub en IoT Hub probeert de berichten opnieuw te verzenden. We raden u aan de laatste bekende fout te bestuderen via de REST API [eind punt status ophalen](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointDead|Fout|Het eind punt heeft gedurende een uur geen berichten geaccepteerd van IoT Hub. We raden u aan de laatste bekende fout te bestuderen via de REST API [eind punt status ophalen](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|EndpointHealthy|Informatie|Het eind punt is in orde en ontvangt berichten van IoT Hub. Dit bericht wordt niet doorlopend geregistreerd, maar alleen vastgelegd wanneer het eind punt weer in orde wordt. Dit bericht betekent dat IoT Hub geen berichten naar het eind punt kon verzenden, maar het eind punt nu in orde is.|
|OrphanedMessage|Informatie|Het bericht komt niet overeen met een route.|
|InvalidMessage|Fout|Het bericht is ongeldig vanwege incompatibiliteit met het eind punt. U wordt aangeraden de configuraties van het eind punt te controleren.|


De bewerkingen *UndefinedRouteEvaluation*, *RouteEvaluationError* en *OrphanedMessage* worden beperkt en niet meer dan één keer per IOT hub vastgelegd.