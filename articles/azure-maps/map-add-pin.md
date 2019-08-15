---
title: Een symbool laag toevoegen aan Azure Maps | Microsoft Docs
description: Symbolen toevoegen aan de Azure Maps Web-SDK.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 10f6a7ef92bfd6558ed93e7fb40df9e48e1b92f5
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976177"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Een symbool laag aan een kaart toevoegen

Een symbool kan worden verbonden met een gegevens bron en wordt gebruikt om een pictogram en/of tekst op een bepaald punt weer te geven. Symbool lagen worden gerenderd met behulp van WebGL en kunnen worden gebruikt om grote verzamelingen van punten op de kaart weer te geven. Met deze laag kunnen veel meer punt gegevens op de kaart worden weer gegeven, met goede prestaties dan wat er met HTML-markeringen kan worden behaald. De laag Symbol ondersteunt echter geen traditionele CSS-en HTML-elementen voor opmaak.  

> [!TIP]
> Met symbool lagen worden standaard de coördinaten van alle geometrieën in een gegevens bron weer gegeven. Als u de laag zo wilt beperken dat alleen de functies van de punt geometrie `filter` worden weer gegeven, stelt `['==', ['geometry-type'], 'Point']` u `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` de eigenschap van de laag in of als u ook multi point-functies wilt toevoegen.

## <a name="add-a-symbol-layer"></a>Een symboollaag toevoegen

Als u een symbool laag wilt toevoegen aan de kaart en gegevens wilt weer geven, moet u eerst een gegevens bron maken en de kaart toevoegen. Vervolgens kan een symbool laag worden gemaakt en door gegeven in de gegevens bron waaruit de gegevens moeten worden opgehaald. Ten slotte moeten gegevens worden toegevoegd aan de gegevens bron, zodat er iets kan worden gerenderd. De volgende code toont de code die moet worden toegevoegd aan de kaart nadat deze is geladen om één punt op de kaart weer te geven met behulp van een symbool laag. 

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a symbol layer to render icons and/or text at points on the map.
var layer = new atlas.layer.SymbolLayer(dataSource);

//Add the layer to the map.
map.layers.add(layer);

//Create a point and add it to the data source.
dataSource.add(new atlas.data.Point([0, 0]));
```

Er zijn vier verschillende typen punt gegevens die aan de kaart kunnen worden toegevoegd:

- Geometrie van geojson-punt: dit object bevat alleen een coördinaat van een punt en niets anders. De `atlas.data.Point` helperklasse kan worden gebruikt om deze objecten eenvoudig te maken.
- Geojson multi point Geometry: dit object bevat de coördinaten van meerdere punten, maar niets anders. De `atlas.data.MultiPoint` helperklasse kan worden gebruikt om deze objecten eenvoudig te maken.
- Geojson-functie: dit object bestaat uit een geojson-geometrie en een set eigenschappen die meta gegevens bevatten die aan de geometrie zijn gekoppeld. De `atlas.data.Feature` helperklasse kan worden gebruikt om deze objecten eenvoudig te maken.
- `atlas.Shape`de klasse is vergelijkbaar met de geojson-functie in dat deze bestaat uit een geojson-geometrie en een set eigenschappen die meta gegevens bevatten die aan de geometrie zijn gekoppeld. Als een geojson-object wordt toegevoegd aan een gegevens bron, kan het eenvoudig worden gerenderd in een laag, maar als de eigenschap coördinaten van het geojson-object wordt bijgewerkt, worden de gegevens bron en de kaart niet gewijzigd omdat er geen mechanisme is in het JSON-object om een update te activeren. De klasse Shape biedt functies voor het bijwerken van de gegevens die deze bevat en wanneer een wijziging wordt aangebracht, worden de gegevens bron en de kaart automatisch gewaarschuwd en bijgewerkt. 

In het volgende code voorbeeld wordt een geometrie voor een geojson-punt gemaakt `atlas.Shape` en door gegeven aan de klasse, zodat deze gemakkelijk kan worden bijgewerkt. Het middel punt van de kaart wordt in eerste instantie gebruikt om een symbool weer te geven. Een gebeurtenis Click wordt toegevoegd aan de kaart, zodat de coördinaten van waar de muis werd geklikt, worden gebruikt met de functie shapes `setCoordinates` die de locatie van het symbool op de kaart bijwerkt.

<br/>

<iframe height='500' scrolling='no' title='Switch pincode locatie' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>pincode locatie</a> van de pen van Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>().
</iframe>

> [!TIP]
> Bij prestaties kunnen symbool lagen standaard de rendering van symbolen optimaliseren door symbolen te verbergen die elkaar overlappen. Wanneer u inzoomt, worden de verborgen symbolen zichtbaar. Als u deze functie wilt uitschakelen en alle symbolen wilt renderen, stelt `allowOverlap` u de eigenschap `iconOptions` van de `true`opties in op.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Een aangepast pictogram toevoegen aan een symbool-laag

Symbool lagen worden gerenderd met behulp van WebGL. Alle resources, zoals pictogram afbeeldingen, moeten worden geladen in de WebGL-context. In dit voor beeld ziet u hoe u een aangepast pictogram kunt toevoegen aan de kaart resources en hoe u dit vervolgens kunt gebruiken om punt gegevens weer te geven met een aangepast symbool op de kaart. Voor `textField` de eigenschap van de Symbol-laag moet een expressie worden opgegeven. In dit geval willen we de eigenschap Tempe ratuur weer geven, maar omdat het een getal is, moet dit worden geconverteerd naar een teken reeks. Daarnaast willen we het ' °F ' aan het bestand toevoegen. Een expressie kan worden gebruikt om dit te doen; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Pictogram voor aangepaste symbool afbeelding' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie het <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>pictogram afbeelding voor aangepast symbool</a> voor pen door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De Azure Maps Web-SDK biedt verschillende aanpas bare afbeeldings sjablonen die u kunt gebruiken met de Symbol-laag. Zie het document [Image-sjablonen gebruiken](how-to-use-image-templates-web-sdk.md) voor meer informatie.

## <a name="customize-a-symbol-layer"></a>Een symbool-laag aanpassen 

De Symbol-laag heeft veel stijl opties beschikbaar. Hier volgt een hulp programma om deze verschillende opmaak opties te testen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor symbool lagen' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/PxVXje/'>laag opties</a> van het pen-symbool<a href='https://codepen.io/azuremaps'>@azuremaps</a>per Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Als u alleen tekst wilt weer geven met een symbool-laag, kunt u het pictogram verbergen door de `image` eigenschap van de pictogram opties in `'none'`te stellen op.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een gegevens bron maken](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Een pop-upvenster toevoegen](map-add-popup.md)

> [!div class="nextstepaction"]
> [Op gegevens gebaseerde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Afbeeldings sjablonen gebruiken](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Een line laag toevoegen](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Een polygoon laag toevoegen](map-add-shape.md)

> [!div class="nextstepaction"]
> [Een Bubble laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML-makers toevoegen](map-add-bubble-layer.md)
