---
title: Zoomniveaus en tegelraster van Azure Maps | Microsoft Docs
description: Meer informatie over zoomniveaus en tegelraster in Azure-kaarten
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: e7dcdb960fbd9196aca8b667269a4c6e5a1fb8f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60794950"
---
# <a name="zoom-levels-and-tile-grid"></a>Zoomniveaus en tegelraster
Azure kaarten gebruiken het coördinatensysteem van de bolvormige Mercator-projectie (EPSG: 3857).

De hele wereld is onderverdeeld in vierkante tegels. Azure kaarten biedt raster en vector tegels voor 23 zoomniveaus, 0 tot en met 22 genummerd. Op inzoomen op het niveau van 0, wordt de hele wereld op één tegel past:

![Tegel van de hele wereld](./media/zoom-levels-and-tile-grid/world0.png)

Zoomniveau 1 maakt gebruik van vier tegels om weer te geven van de hele wereld: een vierkante 2 x 2

![World tegel linksboven](media/zoom-levels-and-tile-grid/world1a.png)     ![World tegel rechtsboven](media/zoom-levels-and-tile-grid/world1c.png) 

![World tegel linksonder](media/zoom-levels-and-tile-grid/world1b.png)     ![World tegel rechtsonder](media/zoom-levels-and-tile-grid/world1d.png) 

Elke extra zoomniveau quad-deelt de tegels van de vorige versie, het maken van een raster met 2<sup>zoomen</sup> x 2<sup>zoomen</sup>. Zoomniveau 22 is een raster 2<sup>22</sup> x 2<sup>22</sup>, of 4,194,304 x 4,194,304 tegels (17,592,186,044,416 tegels in totaal).

De Azure-kaarten interactieve kaart-besturingselementen voor web- en Android, ondersteunen zoom niveaus 25 zoomniveaus genummerd van 0 tot en met 24 uur per dag. Hoewel weg gegevens alleen beschikbaar op het niveau van zoom zijn wordt in wanneer de tegels beschikbaar zijn.

De volgende tabel bevat de volledige lijst met waarden voor zoomniveaus:

|Zoomniveau|Meters/pixel|Meters/tegel aan clientzijde|
|--- |--- |--- |
|0|156543|40075008|
|1|78271.5|20037504|
|2|39135.8|10018764.8|
|3|19567.9|5009382.4|
|4|9783.9|2504678.4|
|5|4892|1252352|
|6|2446|626176|
|7|1223|313088|
|8|611.5|156544|
|9|305.7|78259.2|
|10|152.9|39142.4|
|11|76.4|19558.4|
|12|38.2|9779.2|
|13|19.1|4889.6|
|14|9.6|2457.6|
|15|4.8|1228.8|
|16|2.4|614.4|
|17|1.2|307.2|
|18|0,6|152.8|
|19|0.3|76.4|
|20|0.15|38.2|
|21|0.075|19.1|
|22|0.0375|9.55|
|23|0.01875|4.775|
|24|0.009375|2.3875|

Tegels worden aangeroepen door zoomen niveau en de x en y-coördinaten overeenkomt met de positie van de tegel op het raster voor dat niveau inzoomen.

Wanneer u te bepalen welke zoomniveau te gebruiken, moet u dat elke locatie bevindt zich in een vaste positie op de tegel. Dit betekent dat het aantal tegels die nodig zijn om weer te geven van een bepaalde expanse van gebied afhankelijk van de specifieke plaatsing van raster Inzoomen op de hele wereld is. Bijvoorbeeld, als er twee zijn 900 meters uit elkaar gehaald, verwijst deze *kan* duurt maar drie tegels om weer te geven van een route tussen hen op inzoomen op het niveau 17. Echter, als de westerse punt aan de rechterkant van de tegel en het eastern punt aan de linkerkant van de tegel, duurt het vier tegels:

![Demo-/ uitzoomen](media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Nadat het zoomniveau is vastgesteld, de x- en y waarden kunnen worden berekend. De tegel linksboven in elk raster zoomen is x = 0, y = 0. de tegel rechtsonder is x 2 =<sup>zoom -1</sup>, y = 2<sup>Inzoomen op-1</sup>.

Dit is het raster zoomen voor zoomniveau 1:

![Inzoomen op raster voor zoomniveau 1](media/zoom-levels-and-tile-grid/api_x_y.png)
