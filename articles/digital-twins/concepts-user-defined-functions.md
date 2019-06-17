---
title: Gegevensverwerking en de gebruiker gedefinieerde functies met Azure digitale dubbels | Microsoft Docs
description: Overzicht van de gegevensverwerking en vergelijkingsprogramma voor door de gebruiker gedefinieerde functies met digitale dubbels van Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: alinast
ms.openlocfilehash: 4db515a931bc7f423eb11ae31b7304a602f0da46
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60925882"
---
# <a name="data-processing-and-user-defined-functions"></a>Gegevensverwerking en door gebruikers gedefinieerde functies

Azure van digitale dubbels biedt geavanceerde Reken-mogelijkheden. Ontwikkelaars kunnen definiëren en aangepaste functies uitvoeren in binnenkomende berichten over telemetrie te verzenden van gebeurtenissen naar vooraf gedefinieerde eindpunten.

## <a name="data-processing-flow"></a>Gegevensverwerking stroom

Nadat apparaten telemetrische gegevens naar Azure digitale Twins verzenden, ontwikkelaars gegevens kunnen verwerken in vier fasen: *valideren*, *overeenkomen met*, *compute*, en *verzending* .

![Azure digitale dubbels gegevensverwerking stroom][1]

1. De fase valideren transformeert het binnenkomende telemetrie-bericht naar een algemeen begrepen [data transfer object](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) indeling. Deze fase voert ook validatie van het apparaat en de sensor.
1. De fase van de overeenkomst wordt gezocht naar de juiste door de gebruiker gedefinieerde functies om uit te voeren. Vooraf gedefinieerde vergelijkingsprogramma voor vinden de gebruiker gedefinieerde functies op basis van het apparaat, sensor en ruimte gegevens uit de binnenkomende telemetrie-bericht.
1. De compute-fase wordt de gebruiker gedefinieerde functies overeenkomen met in de vorige fase uitgevoerd. Deze functies kunnen lezen en bijwerken van de berekende waarden op ruimtelijke knooppunten in een grafiek en aangepaste meldingen kan verzenden.
1. De fase van de verzending van de routes aangepaste meldingen van de compute-fase naar eindpunten zijn gedefinieerd in de grafiek.

## <a name="data-processing-objects"></a>Gegevensverwerking objecten

Gegevens worden verwerkt in digitale dubbels Azure bestaat uit drie objecten te definiëren: *vergelijkingsprogramma voor*, *door gebruiker gedefinieerde functies*, en *roltoewijzingen*.

![Azure digitale dubbels gegevensverwerking objecten][2]

<div id="matcher"></div>

### <a name="matchers"></a>Vergelijkingsprogramma voor

Vergelijkingsprogramma voor definiëren een set voorwaarden evalueren welke acties worden uitgevoerd op basis van binnenkomende telemetriegegevens van de sensor. Voorwaarden voor het bepalen van de overeenkomst kunnen eigenschappen van de sensor, van de sensor bovenliggende apparaat en van de sensor bovenliggende ruimte bevatten. De voorwaarden worden uitgedrukt als vergelijkingen op basis van een [JSON-pad](https://jsonpath.com/) zoals wordt beschreven in dit voorbeeld:

- Alle sensoren van gegevenstype **temperatuur** vertegenwoordigd door de tekenreekswaarde met escape-teken `\"Temperature\"`
- Met `01` in hun poort
- Die deel uitmaken van apparaten met de sleutel van de uitgebreide eigenschap **fabrikant** ingesteld op de tekenreekswaarde met escape-teken `\"GoodCorp\"`
- Die behoren tot de adresruimte van het type dat is opgegeven door de tekenreeks met escape-teken `\"Venue\"`
- Welke descendanten van bovenliggende zijn **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON-paden zijn hoofdlettergevoelig.
> - De JSON-nettolading is hetzelfde als de nettolading die wordt geretourneerd door:
>   - `/sensors/{id}?includes=properties,types` voor de sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` voor de sensor van bovenliggende apparaat.
>   - `/spaces/{id}?includes=properties,types,location,timezone` voor de sensor van bovenliggende ruimte.
> - De vergelijkingen zijn niet hoofdlettergevoelig.

### <a name="user-defined-functions"></a>De gebruiker gedefinieerde functies

Een door de gebruiker gedefinieerde functie is een aangepaste functie uitgevoerd binnen een geïsoleerde digitale dubbels Azure-omgeving. De gebruiker gedefinieerde functies hebben toegang tot onbewerkte sensor telemetrieberichten zoals deze wordt ontvangen. De gebruiker gedefinieerde functies hebben ook toegang tot de ruimtelijke graph- en verzender-service. Nadat de gebruiker gedefinieerde functie is geregistreerd in een grafiek, een matcher (gedetailleerde [hierboven](#matcher)) moet worden gemaakt om op te geven wanneer de functie wordt uitgevoerd. Wanneer Azure digitale dubbels nieuwe telemetrie van een bepaalde sensor ontvangt, kunt de overeenkomende gebruiker gedefinieerde functie bijvoorbeeld een zwevend gemiddelde van de laatste paar sensormetingen berekenen.

De gebruiker gedefinieerde functies kunnen worden geschreven in JavaScript. Help-methoden werken met de grafiek in de uitvoeringsomgeving die door de gebruiker gedefinieerd. Ontwikkelaars kunnen aangepaste fragmenten van code op basis van de sensor telemetrieberichten uitvoeren. Voorbeelden zijn:

- Stel de sensor lezen rechtstreeks op de sensor-object in de grafiek.
- Een actie op basis van verschillende sensorwaarden, serverlogs, binnen een ruimte in de grafiek.
- Maak een melding wanneer bepaalde voorwaarden wordt voldaan voor een binnenkomende sensor lezen.
- De metagegevens van de grafiek koppelen aan de sensor lezen voordat u verstuurt een melding.

Zie voor meer informatie, [over het gebruik van de gebruiker gedefinieerde functies](./how-to-user-defined-functions.md).


#### <a name="examples"></a>Voorbeelden

De [GitHub-opslagplaats voor de digitale dubbels C# voorbeeld](https://github.com/Azure-Samples/digital-twins-samples-csharp/) bevat enkele voorbeelden van de gebruiker gedefinieerde functies:
- [Deze functie](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) zoekt uitstoot, beweging en temperatuurwaarden om te bepalen of een lokaal beschikbaar zijn met deze waarden binnen het bereik. De [-zelfstudies voor digitale dubbels](tutorial-facilities-udf.md) verkennen van deze functie in meer informatie. 
- [Deze functie](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) zoekt naar gegevens van meerdere beweging sensors en bepaalt dat de ruimte beschikbaar is als geen van beide een beweging detecteren. U kunt de gebruiker gedefinieerde functie wordt gebruikt in een eenvoudig vervangen de [snelstartgids](quickstart-view-occupancy-dotnet.md), of de [zelfstudies](tutorial-facilities-setup.md), door de wijzigingen die worden vermeld in de sectie met opmerkingen van het bestand. 



### <a name="role-assignment"></a>Roltoewijzing

Een gebruiker gedefinieerde functie acties zijn afhankelijk van Azure digitale dubbels [op rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md) voor het beveiligen van gegevens in de service. Roltoewijzingen definiëren welke door de gebruiker gedefinieerde functies zijn de juiste machtigingen om te communiceren met de ruimtelijke graph en de entiteiten daarvan. Bijvoorbeeld, een door de gebruiker gedefinieerde functie wellicht de mogelijkheid en de machtiging voor *maken*, *lezen*, *UPDATE*, of *verwijderen* grafiekgegevens onder de beschikbare ruimte. Het toegangsniveau van een gebruiker gedefinieerde functie is ingeschakeld wanneer de gebruiker gedefinieerde functie wordt gevraagd de grafiek voor gegevens of een actie probeert. Zie voor meer informatie, [Role-based access control van](./security-create-manage-role-assignments.md).

Het is mogelijk dat een matcher voor het activeren van een gebruiker gedefinieerde functie waarvoor geen roltoewijzingen. In dit geval de gebruiker gedefinieerde functie niet kan gegevens lezen uit de grafiek.

## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over het doorsturen van gebeurtenissen en berichten over telemetrie naar andere Azure-services, [gebeurtenissen en -berichten routeren](./concepts-events-routing.md).

- Lees voor meer informatie over het maken van vergelijkingsprogramma voor de gebruiker gedefinieerde functies en roltoewijzingen [handleiding voor het gebruik van de gebruiker gedefinieerde functies](./how-to-user-defined-functions.md).

- Controleer de [referentiedocumentatie voor de gebruiker gedefinieerde functie client-bibliotheek](./reference-user-defined-functions-client-library.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
