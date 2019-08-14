---
title: Richtingen met Azure Maps weer geven | Microsoft Docs
description: Instructies weer geven tussen twee locaties op een kaart met de Azure Maps Web-SDK.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: cf997d4ae120f3e9309892b112f9954bde97bc76
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976478"
---
# <a name="show-directions-from-a-to-b"></a>Routebeschrijving van A naar B

In dit artikel leest u hoe u een route aanvraag maakt en hoe de route op de kaart wordt weer gegeven.

Er zijn twee manieren om dit te doen. De eerste manier is om de [Azure Maps route-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) via een service module op te vragen. De tweede manier is de [Fetch API](https://fetch.spec.whatwg.org/) te gebruiken om een zoek opdracht naar de [Azure Maps route-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)te maken. Beide manieren worden hieronder besproken.

## <a name="query-the-route-via-service-module"></a>Een query uitvoeren op de route via service module

<iframe height='500' scrolling='no' title='Route beschrijving van A naar B weer geven op een kaart (Service module)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/RBZbep/'>route beschrijving van a naar B op een kaart (Service module)</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object en stelt het verificatie mechanisme in voor gebruik van het toegangs token. U kunt [een overzicht maken](./map-create.md) voor instructies.

Het tweede code blok maakt een `TokenCredential` om HTTP-aanvragen te verifiëren voor Azure Maps met het toegangs token. Vervolgens wordt door gegeven `TokenCredential` aan `atlas.service.MapsURL.newPipeline()` en wordt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) instantie gemaakt. De `routeURL` vertegenwoordigt een URL om [route](https://docs.microsoft.com/rest/api/maps/route) bewerkingen te Azure Maps.

Met het derde code blok wordt een [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object gemaakt en toegevoegd aan de kaart.

Het vierde code blok maakt objecten van het begin-en eind [punt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) en voegt deze toe aan het data source-object.

Een regel is een [functie](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) van Lines Tring. Met een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) worden regel objecten die zijn ingepakt in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) , weer gegeven als lijnen op de kaart. Het vierde code blok maakt een laag en voegt deze toe aan de kaart. Zie Eigenschappen van een laag op [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen voor het weer geven van op punten gebaseerde gegevens die in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) zijn verpakt als symbolen op de kaart. Met het vijfde code blok wordt een symbool laag gemaakt en toegevoegd aan de kaart.

Het zesde code blok voert een query uit op de Azure Maps Routing-service, die deel uitmaakt van de [service module](how-to-use-services-module.md). De methode [calculateRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) van de RouteURL wordt gebruikt om een route tussen de begin-en eind punten op te halen. Een verzameling geojson-functies uit het antwoord wordt vervolgens geëxtraheerd met de `geojson.getFeatures()` methode en aan de gegevens bron toegevoegd. Vervolgens wordt het antwoord weer gegeven als een route op de kaart. Zie [een regel toevoegen op de kaart](map-add-line-layer.md)voor meer informatie over het toevoegen van een regel aan de kaart.

Met het laatste code blok worden de grenzen van de kaart ingesteld met behulp van de eigenschap [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) van de kaart.

De route query, de gegevens bron, het symbool en de lijn lagen en de grenzen van de camera worden gemaakt en in de [](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) gebeurtenislistener van de kaart ingesteld om ervoor te zorgen dat de resultaten worden weer gegeven nadat de kaart volledig is geladen.

## <a name="query-the-route-via-fetch-api"></a>De route opvragen via de API voor ophalen

<iframe height='500' scrolling='no' title='Route beschrijving van A naar B op een kaart weer geven' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>richting van de pen weer geven van a naar B op een kaart</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste code blok een kaart object en stelt het verificatie mechanisme in voor gebruik van het toegangs token. U kunt [een overzicht maken](./map-create.md) voor instructies.

Met het tweede code blok wordt een [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) object gemaakt en toegevoegd aan de kaart.

Het derde code blok maakt de begin-en doel punten voor de route en voegt deze toe aan de gegevens bron. U kunt [een pincode toevoegen op de kaart](map-add-pin.md) zien voor instructies over het gebruik van [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest).

Met een [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) worden regel objecten die zijn ingepakt in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) , weer gegeven als lijnen op de kaart. Het vierde code blok maakt een laag en voegt deze toe aan de kaart. Zie Eigenschappen van een laag op [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Een [symbool laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) maakt gebruik van tekst of pictogrammen voor het weer geven van op punten gebaseerde gegevens die in de [gegevens bron](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) zijn verpakt als symbolen op de kaart. Met het vijfde code blok wordt een symbool laag gemaakt en toegevoegd aan de kaart. Zie Eigenschappen van een Symbol-laag op [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

In het volgende code blok `SouthWest` worden `NorthEast` de begin-en doel punten gemaakt en worden de grenzen van de kaart ingesteld met behulp van de eigenschap [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) van de kaart.

In het laatste code blok wordt de [API voor ophalen](https://fetch.spec.whatwg.org/) gebruikt om een zoek opdracht naar de [Azure Maps route-API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)te maken. Het antwoord wordt vervolgens geparseerd. Als het antwoord is geslaagd, worden de gegevens over de breedte graad en lengte graad gebruikt voor het maken van een lijn door deze punten te verbinden. De regel gegevens worden vervolgens toegevoegd aan de gegevens bron om de route op de kaart weer te geven. Zie [een regel toevoegen op de kaart](map-add-line-layer.md) voor instructies.

De route query, de gegevens bron, het symbool en de lijn lagen en de grenzen van de camera worden gemaakt en in de [](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) gebeurtenislistener van de kaart ingesteld om ervoor te zorgen dat de resultaten worden weer gegeven nadat de kaart volledig is geladen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Diagram](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor voor beelden van volledige code:

> [!div class="nextstepaction"]
> [Verkeer op de kaart weer geven](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interactie met de gebeurtenissen van de kaart muis](./map-events.md)
