---
title: Door de gebruiker gedefinieerde functies maken in azure Digital Apparaatdubbels | Microsoft Docs
description: Het maken van door de gebruiker gedefinieerde functies, treffers en roltoewijzingen in azure Digital Apparaatdubbels.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: alinast
ms.custom: seodec18
ms.openlocfilehash: 6853ebf16c1a9d6b0d363277b22c7dd2583d37e5
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69013973"
---
# <a name="how-to-create-user-defined-functions-in-azure-digital-twins"></a>Door de gebruiker gedefinieerde functies maken in azure Digital Apparaatdubbels

Door de [gebruiker gedefinieerde functies](./concepts-user-defined-functions.md) bieden gebruikers de mogelijkheid om aangepaste logica te configureren voor het uitvoeren van binnenkomende telemetriegegevens en meta gegevens van ruimtelijke grafieken. Gebruikers kunnen ook gebeurtenissen naar vooraf gedefinieerde [eind punten](./how-to-egress-endpoints.md)verzenden.

In deze hand leiding vindt u een voor beeld waarin wordt uitgelegd hoe u een lees-en waarschuwings signalen kunt detecteren die een bepaalde Tempe ratuur van ontvangen temperatuur gebeurtenissen overschrijdt.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

## <a name="client-library-reference"></a>Naslag informatie voor de client bibliotheek

Beschik bare functies als Help-methoden in de door de gebruiker gedefinieerde functions-runtime worden weer gegeven in het referentie document van de [client bibliotheek](./reference-user-defined-functions-client-library.md) .

## <a name="create-a-matcher"></a>Een overeenkomst maken

Matchers zijn grafiek objecten die bepalen welke door de gebruiker gedefinieerde functies worden uitgevoerd voor een gegeven telemetrie-bericht.

- Vergelijking van de voor waarde voor een geldige Matcher:

  - `Equals`
  - `NotEquals`
  - `Contains`

- Geldige doelen voor voor waarden van Matcher:

  - `Sensor`
  - `SensorDevice`
  - `SensorSpace`

In het volgende voor beeld wordt ' True ' geëvalueerd als een wille keurige `"Temperature"` telemetrie-gebeurtenis van een sensor met als waarde voor het gegevens type. U kunt meerdere matchers maken op basis van een door de gebruiker gedefinieerde functie door een geverifieerde HTTP POST-aanvraag in te stellen:

```plaintext
YOUR_MANAGEMENT_API_URL/matchers
```

Met JSON-hoofd tekst:

```JSON
{
  "Name": "Temperature Matcher",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    }
  ],
  "SpaceId": "YOUR_SPACE_IDENTIFIER"
}
```

| Value | Vervangen door |
| --- | --- |
| YOUR_SPACE_IDENTIFIER | De serverregio waarin het exemplaar wordt gehost |

## <a name="create-a-user-defined-function"></a>Een door de gebruiker gedefinieerde functie maken

Door een door de gebruiker gedefinieerde functie te maken, moet u een multi part HTTP-aanvraag indienen bij de Azure Digital Apparaatdubbels Management-Api's.

[!INCLUDE [Digital Twins multipart requests](../../includes/digital-twins-multipart.md)]

Nadat de matchers zijn gemaakt, uploadt u het functie fragment met de volgende Authenticated multi-HTTP POST-aanvraag:

```plaintext
YOUR_MANAGEMENT_API_URL/userdefinedfunctions
```

Gebruik de volgende hoofd tekst:

```plaintext
--USER_DEFINED_BOUNDARY
Content-Type: application/json; charset=utf-8
Content-Disposition: form-data; name="metadata"

{
  "spaceId": "YOUR_SPACE_IDENTIFIER",
  "name": "User Defined Function",
  "description": "The contents of this udf will be executed when matched against incoming telemetry.",
  "matchers": ["YOUR_MATCHER_IDENTIFIER"]
}
--USER_DEFINED_BOUNDARY
Content-Disposition: form-data; name="contents"; filename="userDefinedFunction.js"
Content-Type: text/javascript

function process(telemetry, executionContext) {
  // Code goes here.
}

--USER_DEFINED_BOUNDARY--
```

| Value | Vervangen door |
| --- | --- |
| USER_DEFINED_BOUNDARY | Een grens naam voor meerdelige inhoud |
| YOUR_SPACE_IDENTIFIER | De ruimte-id  |
| YOUR_MATCHER_IDENTIFIER | De ID van de overeenkomst die u wilt gebruiken |

1. Controleer of de headers het volgende `Content-Type: multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`bevatten:.
1. Controleer of de hoofd tekst meerdelige is:

   - Het eerste deel bevat de vereiste meta gegevens van de door de gebruiker gedefinieerde functie.
   - Het tweede deel bevat de Java script-Compute-logica.

1. Vervang de waarden voor **spaceId** `YOUR_SPACE_IDENTIFIER`() en matchers ( `YOUR_MATCHER_IDENTIFIER`) in de sectie **USER_DEFINED_BOUNDARY** .
1. Controleer of de door de gebruiker gedefinieerde Java script-functie `Content-Type: text/javascript`is opgegeven als.

### <a name="example-functions"></a>Voorbeeld functies

Stel de telemetrie van de sensor zo in dat deze rechtstreekswordt `sensor.DataType`gelezen voor de sensor met gegevens type temperatuur:

```JavaScript
function process(telemetry, executionContext) {

  // Get sensor metadata
  var sensor = getSensorMetadata(telemetry.SensorId);

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Set the sensor reading as the current value for the sensor.
  setSensorValue(telemetry.SensorId, sensor.DataType, parseReading.SensorValue);
}
```

De telemetrie-para meter bevat de **SensorId** -en **bericht** kenmerken die overeenkomen met een bericht dat door een sensor wordt verzonden. De para meter **executionContext** stelt de volgende kenmerken beschikbaar:

```csharp
var executionContext = new UdfExecutionContext
{
    EnqueuedTime = request.HubEnqueuedTime,
    ProcessorReceivedTime = request.ProcessorReceivedTime,
    UserDefinedFunctionId = request.UserDefinedFunctionId,
    CorrelationId = correlationId.ToString(),
};
```

In het volgende voor beeld wordt een bericht geregistreerd als de telemetrie van de sensor een vooraf gedefinieerde drempel overschrijdt. Als uw Diagnostische instellingen zijn ingeschakeld voor het Azure Digital Apparaatdubbels-exemplaar, worden er ook logboeken van door de gebruiker gedefinieerde functies doorgestuurd:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Example sensor telemetry reading range is greater than 0.5
  if(parseFloat(parseReading.SensorValue) > 0.5) {
    log(`Alert: Sensor with ID: ${telemetry.SensorId} detected an anomaly!`);
  }
}
```

Met de volgende code wordt een melding gegenereerd als het temperatuur niveau hoger is dan de vooraf gedefinieerde constante:

```JavaScript
function process(telemetry, executionContext) {

  // Retrieve the sensor value
  var parseReading = JSON.parse(telemetry.Message);

  // Define threshold
  var threshold = 70;

  // Trigger notification 
  if(parseInt(parseReading) > threshold) {
    var alert = {
      message: 'Temperature reading has surpassed threshold',
      sensorId: telemetry.SensorId,
      reading: parseReading
    };

    sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(alert));
  }
}
```

Zie voor een complexere door de gebruiker gedefinieerde functie code voor beeld van de [Snelstartgids](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availability.js).

## <a name="create-a-role-assignment"></a>Een roltoewijzing maken

Maak een roltoewijzing voor de door de gebruiker gedefinieerde functie die u wilt uitvoeren. Als er geen roltoewijzing voor de door de gebruiker gedefinieerde functie bestaat, beschikt u niet over de juiste machtigingen om te communiceren met de beheer-API of kunt u geen acties uitvoeren op Graph-objecten. Acties die door een door de gebruiker gedefinieerde functie kunnen worden uitgevoerd, worden opgegeven en gedefinieerd via op rollen gebaseerd toegangs beheer binnen de Azure Digital Apparaatdubbels Management-Api's. Door de gebruiker gedefinieerde functies kunnen bijvoorbeeld worden beperkt in het bereik door bepaalde rollen of bepaalde toegangscontrole paden op te geven. Zie de documentatie [op basis van op rollen gebaseerde toegangs beheer](./security-role-based-access-control.md) voor meer informatie.

1. [Vraag de systeem-API](./security-create-manage-role-assignments.md#all) voor alle rollen op om de rol-id op te halen die u wilt toewijzen aan uw door de gebruiker gedefinieerde functie. Doe dit door een geverifieerde HTTP GET-aanvraag in te stellen voor:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/system/roles
    ```
   Behoud de gewenste rol-ID. Het wordt door gegeven als het JSON Body- kenmerk roleId`YOUR_DESIRED_ROLE_IDENTIFIER`() hieronder.

1. **objectId** (`YOUR_USER_DEFINED_FUNCTION_ID`) is de door de gebruiker gedefinieerde functie-id die eerder is gemaakt.
1. Zoek de waarde van **Path** (`YOUR_ACCESS_CONTROL_PATH`) door uw Spaces te doorzoeken `fullpath`met.
1. Kopieer de geretourneerde `spacePaths` waarde. U gaat hiervoor het volgende gebruiken. Een geverifieerde HTTP GET-aanvraag indienen voor:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/spaces?name=YOUR_SPACE_NAME&includes=fullpath
    ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_SPACE_NAME | De naam van de ruimte die u wilt gebruiken |

1. Plak de geretourneerde `spacePaths` waarde in **pad** om een door de gebruiker gedefinieerde functie toewijzing te maken door een geverifieerde http post-aanvraag in te stellen:

    ```plaintext
    YOUR_MANAGEMENT_API_URL/roleassignments
    ```
    Met JSON-hoofd tekst:

    ```JSON
    {
      "roleId": "YOUR_DESIRED_ROLE_IDENTIFIER",
      "objectId": "YOUR_USER_DEFINED_FUNCTION_ID",
      "objectIdType": "YOUR_USER_DEFINED_FUNCTION_TYPE_ID",
      "path": "YOUR_ACCESS_CONTROL_PATH"
    }
    ```

    | Value | Vervangen door |
    | --- | --- |
    | YOUR_DESIRED_ROLE_IDENTIFIER | De id voor de gewenste rol |
    | YOUR_USER_DEFINED_FUNCTION_ID | De ID voor de door de gebruiker gedefinieerde functie die u wilt gebruiken |
    | YOUR_USER_DEFINED_FUNCTION_TYPE_ID | De ID waarmee het door de gebruiker gedefinieerde functie type wordt opgegeven |
    | YOUR_ACCESS_CONTROL_PATH | Het Access Control-pad |

>[!TIP]
> Lees het artikel [over het maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md) voor meer informatie over door de gebruiker gedefinieerde functie beheer-API-bewerkingen en-eind punten.

## <a name="send-telemetry-to-be-processed"></a>Telemetrie verzenden die moeten worden verwerkt

De sensor die in de ruimtelijke Intelligence-grafiek is gedefinieerd, verzendt telemetrie. Op zijn beurt wordt de uitvoering geactiveerd van de door de gebruiker gedefinieerde functie die is geüpload. De gegevens processor haalt de telemetrie op. Vervolgens wordt er een uitvoerings plan gemaakt voor het aanroepen van de door de gebruiker gedefinieerde functie.

1. Haal de vergelijkings op voor de sensor waarvan de Lees bewerking is gegenereerd.
1. Afhankelijk van wat overeenkomende treffers zijn geëvalueerd, haalt u de gekoppelde door de gebruiker gedefinieerde functies op.
1. Elke door de gebruiker gedefinieerde functie uitvoeren.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van Azure Digital apparaatdubbels-eind punten](./how-to-egress-endpoints.md) voor het verzenden van gebeurtenissen naar.

- Lees [routerings gebeurtenissen en berichten](./concepts-events-routing.md)voor meer informatie over route ring in azure Digital apparaatdubbels.

- Raadpleeg de [documentatie van de client bibliotheek referentie](./reference-user-defined-functions-client-library.md).
