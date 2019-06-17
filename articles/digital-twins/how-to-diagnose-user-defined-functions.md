---
title: Fouten opsporen in UDF's in Azure, digitale dubbels | Microsoft Docs
description: Fouten opsporen in UDF's in Azure, digitale dubbels richtlijnen.
author: stefanmsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: stefanmsft
ms.custom: seodec18
ms.openlocfilehash: 4d772b8cad64f138d93d91e87f6e6364c5a5d602
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808887"
---
# <a name="how-to-debug-user-defined-functions-in-azure-digital-twins"></a>Fouten opsporen in door de gebruiker gedefinieerde functies in Azure, digitale dubbels

In dit artikel bevat een overzicht van hoe u kunt vaststellen en oplossen van de gebruiker gedefinieerde functies in Azure, digitale dubbels. Vervolgens wordt enkele van de meest voorkomende scenario's gevonden tijdens het opsporen van fouten in deze geïdentificeerd.

>[!TIP]
> Lezen [bewaking en logboekregistratie configureren](./how-to-configure-monitoring.md) voor meer informatie over het instellen van het foutopsporingsprogramma's in Azure digitale dubbels met activiteitenlogboeken, diagnostische logboeken en Azure Monitor.

## <a name="debug-issues"></a>Spoor fouten op

Weet hoe u problemen identificeren binnen Azure digitale Twins, kunt u effectief problemen wilt analyseren, de oorzaken van problemen identificeren en passende oplossingen leveren voor deze.

Tal van logboekregistratie, analyses en diagnostische hulpprogramma's vindt u wat dat betreft.

### <a name="enable-logging-for-your-instance"></a>Logboekregistratie inschakelen voor uw exemplaar

Azure van digitale dubbels biedt ondersteuning voor robuuste logboekregistratie, controle en analyse. Oplossingen voor ontwikkelaars kunnen Azure Monitor-Logboeken, diagnostische logboeken, activiteitenlogboeken en andere services gebruiken ter ondersteuning van de behoeften van complexe bewaking van een IoT-app. Opties voor logboekregistratie kunnen worden gecombineerd om opvragen of records weergeven binnen diverse services en om gedetailleerde logboekregistratie dekking voor veel services.

* Lees voor de configuratie van de logboekregistratie is specifiek voor Azure digitale dubbels, [bewaking en logboekregistratie configureren](./how-to-configure-monitoring.md).
* Raadpleeg de [Azure Monitor](../azure-monitor/overview.md) overzicht voor meer informatie over krachtige logboekinstellingen via Azure Monitor wordt ingeschakeld.
* Lees het artikel [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../azure-monitor/platform/diagnostic-logs-overview.md) voor het configureren van instellingen voor diagnostische logboeken in Azure van digitale dubbels via Azure Portal, Azure CLI of PowerShell.

Wanneer geconfigureerd, moet u mogelijk zijn voor het selecteren van alle logboekcategorieën, metrische gegevens en krachtige Azure Monitor log analytics-werkruimten gebruiken ter ondersteuning van uw foutopsporing.

### <a name="trace-sensor-telemetry"></a>Sensor-tracetelemetrie

Te traceren sensor telemetrie, controleert u of dat er diagnostische instellingen zijn ingeschakeld voor uw exemplaar van Azure digitale dubbels. Controleer vervolgens of dat alle gewenste logboekcategorieën zijn geselecteerd. Ten slotte of dat de gewenste logboeken worden verzonden naar de logboeken van Azure Monitor.

Om een bericht van de sensor telemetrie naar de respectieve Logboeken, kunt u een correlatie-ID opgeven op de gegevens van de gebeurtenis wordt verzonden. Om dit te doen, stel de `x-ms-client-request-id` eigenschap naar een GUID.

Na het verzenden van telemetrie, opent u log analytics-query voor logboeken met behulp van de set correlatie-ID:

```Kusto
AzureDiagnostics
| where CorrelationId == 'YOUR_CORRELATION_IDENTIFIER'
```

| Waarde opvragen | Vervangen door |
| --- | --- |
| YOUR_CORRELATION_IDENTIFIER | De correlatie-ID die is opgegeven in de gebeurtenisgegevens |

Als u logboekregistratie voor de gebruiker gedefinieerde functie inschakelt, deze logboeken worden weergegeven in uw log analytics-exemplaar met de categorie `UserDefinedFunction`. Om op te halen ze, voer de volgende queryvoorwaarde in log analytics:

```Kusto
AzureDiagnostics
| where Category == 'UserDefinedFunction'
```

Lees voor meer informatie over krachtige querybewerkingen [aan de slag met query's](../azure-monitor/log-query/get-started-queries.md).

## <a name="identify-common-issues"></a>Veelvoorkomende problemen identificeren

Zowel opsporen en identificeren van algemene problemen zijn belangrijk bij het oplossen van uw oplossing. Verschillende problemen die vaak optreden bij het ontwikkelen van de gebruiker gedefinieerde functies worden samengevat in de volgende subsecties.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

### <a name="check-if-a-role-assignment-was-created"></a>Controleer of er een roltoewijzing is gemaakt

De gebruiker gedefinieerde functie heeft geen toegang tot alle acties uitvoeren zoals het verzenden van meldingen, bij het ophalen van metagegevens, zonder een roltoewijzing gemaakt binnen de API Management, en instelling berekende waarden in de topologie.

Controleer of een roltoewijzing voor de gebruiker gedefinieerde functie door uw API Management:

```plaintext
GET YOUR_MANAGEMENT_API_URL/roleassignments?path=/&traverse=Down&objectId=YOUR_USER_DEFINED_FUNCTION_ID
```

| Parameterwaarde | Vervangen door |
| --- | --- |
| YOUR_USER_DEFINED_FUNCTION_ID | De ID van de gebruiker gedefinieerde functie om op te halen van roltoewijzingen voor|

Informatie over [over het maken van een roltoewijzing voor de gebruiker gedefinieerde functie](./how-to-user-defined-functions.md), als er geen roltoewijzingen bestaat.

### <a name="check-if-the-matcher-works-for-a-sensors-telemetry"></a>Controleer of de matcher voor de telemetrie van een sensor werkt

Met de volgende oproep verzenden op basis van uw digitale dubbels Azure-instanties Management-API u kunt bepalen of een bepaalde matcher voor de opgegeven sensor geldt.

```plaintext
GET YOUR_MANAGEMENT_API_URL/matchers/YOUR_MATCHER_IDENTIFIER/evaluate/YOUR_SENSOR_IDENTIFIER?enableLogging=true
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_MATCHER_IDENTIFIER* | De ID van de matcher die u wilt evalueren |
| *YOUR_SENSOR_IDENTIFIER* | De ID van de sensor die u wilt evalueren |

Reactie:

```JavaScript
{
    "success": true,
    "logs": [
        "$.dataType: \"Motion\" Equals \"Motion\" => True"
    ]
}
```

### <a name="check-what-a-sensor-triggers"></a>Controleer wat een sensor wordt geactiveerd

Met de volgende oproep verzenden op basis van de Azure digitale dubbels Management API's u kunt de id's van de gebruiker gedefinieerde functies geactiveerd door de opgegeven sensor binnenkomende telemetriegegevens te bepalen:

```plaintext
GET YOUR_MANAGEMENT_API_URL/sensors/YOUR_SENSOR_IDENTIFIER/matchers?includes=UserDefinedFunctions
```

| Parameter | Vervangen door |
| --- | --- |
| *YOUR_SENSOR_IDENTIFIER* | De ID van de sensor om telemetrie te verzenden |

Reactie:

```JavaScript
[
    {
        "id": "48a64768-797e-4832-86dd-de625f5f3fd9",
        "name": "MotionMatcher",
        "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
        "conditions": [
            {
                "id": "024a067a-414f-415b-8424-7df61392541e",
                "target": "Sensor",
                "path": "$.dataType",
                "value": "\"Motion\"",
                "comparison": "Equals"
            }
        ],
        "userDefinedFunctions": [
            {
                "id": "373d03c5-d567-4e24-a7dc-f993460120fc",
                "spaceId": "2117b3e1-b6ce-42c1-9b97-0158bef59eb7",
                "name": "Motion User-Defined Function",
                "disabled": false
            }
        ]
    }
]
```

### <a name="issue-with-receiving-notifications"></a>Probleem met het ontvangen van meldingen

Wanneer u geen meldingen van de geactiveerde door de gebruiker gedefinieerde functie ontvangt, bevestigt u dat uw topologie object typeparameter overeenkomt met het type-id die wordt gebruikt.

**Onjuiste** voorbeeld:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will not work'
};

sendNotification(telemetry.SensorId, "Space", JSON.stringify(customNotification));
```

In dit scenario optreedt, omdat de gebruikte id naar een sensor verwijst terwijl het objecttype topologie opgegeven `Space`.

**Juiste** voorbeeld:

```JavaScript
var customNotification = {
    Message: 'Custom notification that will work'
};

sendNotification(telemetry.SensorId, "Sensor", JSON.stringify(customNotification));
```

De eenvoudigste manier om niet ondervindt dit probleem is met de `Notify` methode voor het metagegevensobject.

Voorbeeld:

```JavaScript
function process(telemetry, executionContext) {
    var sensorMetadata = getSensorMetadata(telemetry.SensorId);

    var customNotification = {
        Message: 'Custom notification'
    };

    // Short-hand for above methods where object type is known from metadata.
    sensorMetadata.Notify(JSON.stringify(customNotification));
}
```

## <a name="common-diagnostic-exceptions"></a>Algemene diagnostische uitzonderingen

Als u diagnostische instellingen inschakelen, kunt u deze algemene uitzonderingen optreden:

1. **Beperking**: als de gebruiker gedefinieerde functie groter is dan de uitvoeringssnelheid van limieten die worden beschreven in de [Servicelimieten](./concepts-service-limits.md) artikel, worden beperkt. Verder geen bewerkingen worden uitgevoerd totdat de bandbreedteregeling limieten zijn verlopen.

1. **Gegevens niet gevonden**: als de gebruiker gedefinieerde functie probeert te krijgen tot metagegevens die niet bestaat, de bewerking mislukt.

1. **Niet gemachtigd**: als de gebruiker gedefinieerde functie beschikt niet over een roltoewijzing instellen of beschikt niet over voldoende machtigingen voor toegang tot bepaalde metagegevens van de topologie, de bewerking is mislukt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het inschakelen [bewaking en logboeken](./how-to-configure-monitoring.md) in digitale dubbels van Azure.

- Lees de [overzicht van Azure Activity log](../azure-monitor/platform/activity-logs-overview.md) artikel voor meer Azure opties voor logboekregistratie.
