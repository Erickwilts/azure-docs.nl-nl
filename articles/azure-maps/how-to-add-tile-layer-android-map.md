---
title: Een tegel laag toevoegen aan Android-kaarten | Microsoft Azure kaarten
description: Meer informatie over het toevoegen van een tegel laag aan een kaart. Bekijk een voor beeld waarin de Azure Maps Android-SDK wordt gebruikt om een weers radar-overlay toe te voegen aan een kaart.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 8ea6f44c47c5cd4d223b053640f65827f46db482
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2020
ms.locfileid: "97679305"
---
# <a name="add-a-tile-layer-to-a-map-android-sdk"></a>Een laag voor een tegel toevoegen aan een kaart (Android SDK)

In dit artikel wordt beschreven hoe u een tegel laag op een kaart kunt weer geven met behulp van de Azure Maps Android SDK. Met tegel lagen kunt u afbeeldingen boven op Azure Maps basis kaart tegels plaatsen. Meer informatie over Azure Maps tegel systeem vindt u in de documentatie over het [Zoom niveau en het tegel raster](zoom-levels-and-tile-grid.md) .

Een tegel laag wordt in tegels van een server geladen. Deze installatie kopieën kunnen vooraf worden weer gegeven en opgeslagen, zoals elke andere installatie kopie op een server, met behulp van een naamgevings Conventie die de laag van de tegel begrijpt. Het is ook mogelijk dat deze installatie kopieën worden weer gegeven met een dynamische service die de afbeeldingen bijna in real time genereert. Er worden drie verschillende naamgevings conventies voor tegel Services ondersteund door Azure Maps klasse TileLayer:

* X, Y, zoom notatie: gebaseerd op het zoom niveau, x is de kolom en Y de rijpositie van de tegel in het tegel raster.
* Quadkey notatie: combi natie x, y en zoom informatie in een enkele teken reeks waarde die een unieke id voor een tegel is.
* Omsluitende Box-coördinaten kunnen worden gebruikt voor het opgeven van een afbeelding in de indeling `{west},{south},{east},{north}` , die meestal wordt gebruikt door [Web Mapping Services (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Een TileLayer is een uitstekende manier om grote gegevens sets op de kaart te visualiseren. U kunt niet alleen een tegel laag genereren op basis van een afbeelding, maar u kunt ook vector gegevens weer geven als een tegel laag. Door vector gegevens als een tegel laag te renderen, hoeft het kaart besturings element alleen de tegels te laden. Dit kan veel kleiner zijn dan de vector gegevens die ze vertegenwoordigen. Deze techniek wordt gebruikt door veel die miljoenen rijen met gegevens op de kaart moeten weer geven.

De tegel-URL die wordt door gegeven aan een tegel laag moet een HTTP/HTTPS-URL zijn naar een TileJSON-resource of een tegel-URL-sjabloon die gebruikmaakt van de volgende para meters: 

* `{x}` -X-positie van de tegel. Ook nodig `{y}` en `{z}` .
* `{y}` -Y-positie van de tegel. Ook nodig `{x}` en `{z}` .
* `{z}` -Zoom niveau van de tegel. Ook nodig `{x}` en `{y}` .
* `{quadkey}` -Tegel quadkey-id gebaseerd op de naam Conventie voor Bing Maps-tegel systeem.
* `{bbox-epsg-3857}` -Een teken reeks voor selectie kader met de indeling `{west},{south},{east},{north}` in het EPSG 3857 Spatial Reference System.
* `{subdomain}` -Een tijdelijke aanduiding voor de waarden van het subdomein als de subdomeinwaarde is opgegeven.

## <a name="prerequisites"></a>Vereisten

Om het proces in dit artikel te volt ooien, moet u [Azure Maps ANDROID SDK](how-to-use-android-map-control-library.md) installeren om een kaart te laden.

## <a name="add-a-tile-layer-to-the-map"></a>Een tegel laag aan de kaart toevoegen

Dit voor beeld laat zien hoe u een tegel laag maakt die verwijst naar een set tegels. In dit voor beeld wordt het tegel systeem x, y, Zoom gebruikt. De bron van deze laag voor tegels is het [OpenSeaMap-project](https://openseamap.org/index.php), dat prosourced zeemijl-grafieken bevat. Bij het weer geven van Tegel lagen is het wenselijk om de labels van steden op de kaart duidelijk zichtbaar te maken. Dit gedrag kan worden bereikt door de laag van de tegel onder de kaart label lagen in te voegen.

```java
TileLayer layer = new TileLayer(
    tileUrl("https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png"),
    opacity(0.8f),
    tileSize(256),
    minSourceZoom(7),
    maxSourceZoom(17)
);

map.layers.add(layer, "labels");
```

Op de volgende scherm afbeelding ziet u de bovenstaande code met een tegel laag met zeemijl over een kaart met een donkere grijs waarde.

![Android-kaart met weer gave van laag van Tegel](media/how-to-add-tile-layer-android-map/xyz-tile-layer-android.png)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het volgende artikel voor meer informatie over manieren om kaart stijlen in te stellen

> [!div class="nextstepaction"]
> [De kaart stijl wijzigen](set-android-map-styles.md)

> [!div class="nextstepaction"]
> [Een heatmap toevoegen](map-add-heat-map-layer-android.md)
