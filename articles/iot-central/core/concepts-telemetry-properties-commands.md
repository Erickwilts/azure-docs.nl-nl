---
title: De telemetrie-, Property-en Command-Payloads in azure IoT Central | Microsoft Docs
description: Met Azure IoT Central-Apparaatbeheer kunt u opgeven welke telemetrie, eigenschappen en opdrachten van een apparaat moeten worden geïmplementeerd. Inzicht in de indeling van de gegevens die een apparaat kan uitwisselen met IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 554079ddec3332ced2817d18ea55ce1260d68817
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "87291605"
---
# <a name="telemetry-property-and-command-payloads"></a>Payloads van telemetrie, eigenschappen en opdrachten

_Dit artikel is bedoeld voor ontwikkelaars van apparaten._

Een Device-sjabloon in azure IoT Central is een blauw druk die de volgende definieert:

* Telemetrie die een apparaat naar IoT Central verzendt.
* Eigenschappen die een apparaat synchroniseert met IoT Central.
* Opdrachten die op een apparaat worden IoT Central.

In dit artikel wordt beschreven hoe de JSON-nettoladingen van apparaten worden verzonden en ontvangen voor telemetrie, eigenschappen en opdrachten die in een apparaatprofiel zijn gedefinieerd, voor ontwikkel aars van apparaten.

In het artikel wordt niet elk mogelijk type telemetrie, eigenschap en opdracht Payload beschreven, maar de voor beelden illustreren alle sleutel typen.

Elk voor beeld toont een fragment van het apparaatcompatibiliteit (device capability model, DCM) dat het type en de voor beeld-JSON-nettoladingen definieert om te laten zien hoe het apparaat moet communiceren met de IoT Central-toepassing.

> [!NOTE]
> IoT Central accepteert een geldige JSON, maar kan alleen worden gebruikt voor visualisaties als deze overeenkomt met een definitie in de DCM. U kunt gegevens exporteren die niet overeenkomen met een definitie. Zie [IOT-gegevens exporteren naar bestemmingen in azure](howto-export-data.md).

Het JSON-bestand dat de DCM definieert, maakt gebruik van de [Digital-taal (DTDL) v1](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/dtdlv1.md). Deze specificatie bevat de definitie van de `@id` eigenschaps indeling.

Zie de [client toepassing maken en verbinden met uw azure IOT Central-toepassing (Node.js)](tutorial-connect-device-nodejs.md) en [een client toepassing maken en verbinden met uw python-zelf studies (Azure IOT Central Application)](tutorial-connect-device-python.md) voor een voor beeld van een apparaatcode dat enkele van deze nettoladingen bevat die in gebruik zijn.

## <a name="view-raw-data"></a>Onbewerkte gegevens weergeven

Met IoT Central kunt u de onbewerkte gegevens weer geven die een apparaat naar een toepassing verzendt. Deze weer gave is handig voor het oplossen van problemen met de payload die van een apparaat is verzonden. De onbewerkte gegevens weer geven die een apparaat verzendt:

1. Ga naar het apparaat op de pagina **apparaten** .

1. Selecteer het tabblad **onbewerkte gegevens** :

    :::image type="content" source="media/concepts-telemetry-properties-commands/raw-data.png" alt-text="Weergave Onbewerkte gegevens":::

    In deze weergave kunt u de kolommen selecteren die u wilt weergeven en een tijdsbereik instellen. In de kolom **Niet-gemodelleerde gegevens** ziet u gegevens van het apparaat die niet overeenkomen met een van de eigenschaps- of telemetriedefinities in de apparaatsjabloon.

## <a name="telemetry"></a>Telemetrie

### <a name="primitive-types"></a>Primitieve typen

In deze sectie ziet u voor beelden van primitieve telemetrie-typen die een apparaat streamt naar een IoT Central-toepassing.

Het volgende code fragment van een DCM toont de definitie van een `boolean` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "BooleanTelemetry"
  },
  "name": "BooleanTelemetry",
  "schema": "boolean"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{ "BooleanTelemetry": true }
```

Het volgende code fragment van een DCM toont de definitie van een `string` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "StringTelemetry"
  },
  "name": "StringTelemetry",
  "schema": "string"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{ "StringTelemetry": "A string value - could be a URL" }
```

Het volgende code fragment van een DCM toont de definitie van een `integer` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "IntegerTelemetry"
  },
  "name": "IntegerTelemetry",
  "schema": "integer"
}

```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{ "IntegerTelemetry": 23 }
```

Het volgende code fragment van een DCM toont de definitie van een `double` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DoubleTelemetry"
  },
  "name": "DoubleTelemetry",
  "schema": "double"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{ "DoubleTelemetry": 56.78 }
```

Het volgende code fragment van een DCM toont de definitie van een `dateTime` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DateTimeTelemetry"
  },
  "name": "DateTimeTelemetry",
  "schema": "dateTime"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die lijkt op het volgende voor beeld- `DateTime` typen moet voldoen aan ISO 8061-compatibel:

```json
{ "DateTimeTelemetry": "2020-08-30T19:16:13.853Z" }
```

Het volgende code fragment van een DCM toont de definitie van een `duration` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "DurationTelemetry"
  },
  "name": "DurationTelemetry",
  "schema": "duration"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die lijkt op het volgende voor beeld: de duur van de ISO 8601-duur moet voldoen aan de voor waarden.

```json
{ "DurationTelemetry": "PT10H24M6.169083011336625S" }
```

### <a name="complex-types"></a>Complexe typen

In deze sectie ziet u voor beelden van complexe telemetriegegevens die een apparaat streamt naar een IoT Central-toepassing.

Het volgende code fragment van een DCM toont de definitie van een `geopoint` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "GeopointTelemetry"
  },
  "name": "GeopointTelemetry",
  "schema": "geopoint"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld. IoT Central wordt de waarde weer gegeven als een pincode op een kaart:

```json
{
  "GeopointTelemetry": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Het volgende code fragment van een DCM toont de definitie van een `Enum` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "EnumTelemetry"
  },
  "name": "EnumTelemetry",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld. Mogelijke waarden zijn `0` , `1` en `2` die worden weer gegeven in IOT Central als `Item1` , `Item2` , `Item3` en:

```json
{ "EnumTelemetry": 1 }
```

Het volgende code fragment van een DCM toont de definitie van een `Object` type telemetrie. Dit object heeft drie velden met typen `dateTime` , `integer` en `Enum` :

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "ObjectTelemetry"
  },
  "name": "ObjectTelemetry",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property1"
        },
        "name": "Property1",
        "schema": "dateTime"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property2"
        },
        "name": "Property2",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Property3"
        },
        "name": "Property3",
        "schema": {
          "@id": "<element id>",
          "@type": "Enum",
          "displayName": {
            "en": "Enum"
          },
          "valueSchema": "integer",
          "enumValues": [
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item1"
              },
              "enumValue": 0,
              "name": "Item1"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item2"
              },
              "enumValue": 1,
              "name": "Item2"
            },
            {
              "@id": "<element id>",
              "@type": "EnumValue",
              "displayName": {
                "en": "Item3"
              },
              "enumValue": 2,
              "name": "Item3"
            }
          ]
        }
      }
    ]
  }
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld. `DateTime` typen moeten compatibel zijn met ISO 8061. Mogelijke waarden voor `Property3` zijn `0` , `1` en die worden weer gegeven in IOT Central als `Item1` , `Item2` , en `Item3` :

```json
{
  "ObjectTelemetry": {
      "Property1": "2020-09-09T03:36:46.195Z",
      "Property2": 37,
      "Property3": 2
  }
}
```

Het volgende code fragment van een DCM toont de definitie van een `vector` telemetrie-type:

```json
{
  "@id": "<element id>",
  "@type": "Telemetry",
  "displayName": {
    "en": "VectorTelemetry"
  },
  "name": "VectorTelemetry",
  "schema": "vector"
}
```

Een apparaatclient moet de telemetrie als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{
  "VectorTelemetry": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="event-and-state-types"></a>Gebeurtenis-en status typen

In deze sectie vindt u voor beelden van telemetriegegevens en statussen die een apparaat naar een IoT Central-toepassing verzendt.

Het volgende code fragment vanuit een DCM toont de definitie van een `integer` gebeurtenis type:

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/Event"
  ],
  "displayName": {
    "en": "IntegerEvent"
  },
  "name": "IntegerEvent",
  "schema": "integer"
}
```

Een apparaatclient moet de gebeurtenis gegevens als JSON verzenden die eruitziet als in het volgende voor beeld:

```json
{ "IntegerEvent": 74 }
```

Het volgende code fragment van een DCM toont de definitie van een `integer` status type:

```json
{
  "@id": "<element id>",
  "@type": [
    "Telemetry",
    "SemanticType/State"
  ],
  "displayName": {
    "en": "IntegerState"
  },
  "name": "IntegerState",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level1"
        },
        "enumValue": 1,
        "name": "Level1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level2"
        },
        "enumValue": 2,
        "name": "Level2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Level3"
        },
        "enumValue": 3,
        "name": "Level3"
      }
    ]
  }
}
```

Een apparaatclient moet de status als JSON verzenden die eruitziet als in het volgende voor beeld. Mogelijke waarden voor een geheel getal zijn `1` , `2` of `3` :

```json
{ "IntegerState": 2 }
```

## <a name="properties"></a>Eigenschappen

> [!NOTE]
> De payload-indelingen voor eigenschappen zijn van toepassing op toepassingen die zijn gemaakt op of na 07/14/2020.

### <a name="primitive-types"></a>Primitieve typen

In deze sectie vindt u voor beelden van primitieve eigenschaps typen die een apparaat naar een IoT Central-toepassing verzendt.

Het volgende code fragment vanuit een DCM toont de definitie van een `boolean` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "BooleanProperty"
  },
  "name": "BooleanProperty",
  "schema": "boolean"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{ "BooleanProperty": false }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `boolean` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "LongProperty"
  },
  "name": "LongProperty",
  "schema": "long"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{ "LongProperty": 439 }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `date` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DateProperty"
  },
  "name": "DateProperty",
  "schema": "date"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het dubbele apparaat. `Date` typen moeten compatibel zijn met ISO 8061:

```json
{ "DateProperty": "2020-05-17" }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `duration` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "DurationProperty"
  },
  "name": "DurationProperty",
  "schema": "duration"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in de dubbele duur van het apparaat moet voldoen aan de ISO 8601-duur:

```json
{ "DurationProperty": "PT10H24M6.169083011336625S" }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `float` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "FloatProperty"
  },
  "name": "FloatProperty",
  "schema": "float"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{ "FloatProperty": 1.9 }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `string` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringProperty"
  },
  "name": "StringProperty",
  "schema": "string"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{ "StringProperty": "A string value - could be a URL" }
```

### <a name="complex-types"></a>Complexe typen

In deze sectie vindt u voor beelden van complexe eigenschaps typen die een apparaat naar een IoT Central-toepassing verzendt.

Het volgende code fragment vanuit een DCM toont de definitie van een `geopoint` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "GeopointProperty"
  },
  "name": "GeopointProperty",
  "schema": "geopoint"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{
  "GeopointProperty": {
    "lat": 47.64263,
    "lon": -122.13035,
    "alt": 0
  }
}
```

Het volgende code fragment vanuit een DCM toont de definitie van een `Enum` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumProperty"
  },
  "name": "EnumProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het dubbele apparaat. Mogelijke waarden zijn `0` , `1` en die worden weer gegeven in IOT Central als `Item1` , `Item2` , en `Item3` :

```json
{ "EnumProperty": 1 }
```

Het volgende code fragment vanuit een DCM toont de definitie van een `Object` eigenschaps type. Dit object heeft twee velden met typen `string` en `integer` :

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "ObjectProperty"
  },
  "name": "ObjectProperty",
  "schema": {
    "@id": "<element id>",
    "@type": "Object",
    "displayName": {
      "en": "Object"
    },
    "fields": [
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field1"
        },
        "name": "Field1",
        "schema": "integer"
      },
      {
        "@id": "<element id>",
        "@type": "SchemaField",
        "displayName": {
          "en": "Field2"
        },
        "name": "Field2",
        "schema": "string"
      }
    ]
  }
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{
  "ObjectProperty": {
    "Field1": 37,
    "Field2": "A string value"
  }
}
```

Het volgende code fragment vanuit een DCM toont de definitie van een `vector` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "VectorProperty"
  },
  "name": "VectorProperty",
  "schema": "vector"
}
```

Een apparaatclient moet een JSON-nettolading verzenden die lijkt op het volgende voor beeld als een gerapporteerde eigenschap in het apparaat dubbele:

```json
{
  "VectorProperty": {
    "x": 74.72395045538597,
    "y": 74.72395045538597,
    "z": 74.72395045538597
  }
}
```

### <a name="writeable-property-types"></a>Beschrijf bare eigenschaps typen

In deze sectie vindt u voor beelden van Beschrijf bare eigenschaps typen die een apparaat ontvangt van een IoT Central-toepassing.

IoT Central verwacht een reactie van het apparaat naar updates van schrijf bare eigenschappen. Het antwoord bericht moet de `ac` velden en bevatten `av` . Het veld `ad` is optioneel. Raadpleeg de volgende fragmenten voor voor beelden.

`ac` is een numeriek veld dat gebruikmaakt van de waarden in de volgende tabel:

| Waarde | Label | Beschrijving |
| ----- | ----- | ----------- |
| `'ac': 200` | Voltooid | De bewerking voor het wijzigen van de eigenschap is voltooid. |
| `'ac': 202`  of `'ac': 201` | In behandeling | De bewerking voor het wijzigen van de eigenschap is in behandeling of wordt uitgevoerd |
| `'ac': 4xx` | Fout | De aangevraagde wijziging van de eigenschap is ongeldig of er is een fout opgetreden |
| `'ac': 5xx` | Fout | Het apparaat heeft een onverwachte fout aangetroffen bij het verwerken van de aangevraagde wijziging. |

`av` is het versie nummer dat naar het apparaat wordt verzonden.

`ad` is een beschrijving van een optie teken reeks.

Het volgende code fragment van een DCM toont de definitie van een Beschrijf bare `string` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "StringPropertyWritable"
  },
  "name": "StringPropertyWritable",
  "writable": true,
  "schema": "string"
}
```

Het apparaat ontvangt de volgende payload van IoT Central:

```json
{  
  "StringPropertyWritable": "A string from IoT Central", "$version": 7
}
```

Het apparaat moet de volgende JSON-Payload verzenden naar IoT Central nadat de update is verwerkt. Dit bericht bevat het versie nummer van de oorspronkelijke update die is ontvangen van IoT Central. Wanneer IoT Central dit bericht ontvangt, wordt de eigenschap gemarkeerd als **gesynchroniseerd** in de gebruikers interface:

```json
{
  "StringPropertyWritable": {
    "value": "A string from IoT Central",
    "ac": 200,
    "ad": "completed",
    "av": 7
  }
}
```

Het volgende code fragment van een DCM toont de definitie van een Beschrijf bare `Enum` eigenschaps type:

```json
{
  "@id": "<element id>",
  "@type": "Property",
  "displayName": {
    "en": "EnumPropertyWritable"
  },
  "name": "EnumPropertyWritable",
  "writable": true,
  "schema": {
    "@id": "<element id>",
    "@type": "Enum",
    "displayName": {
      "en": "Enum"
    },
    "valueSchema": "integer",
    "enumValues": [
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item1"
        },
        "enumValue": 0,
        "name": "Item1"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item2"
        },
        "enumValue": 1,
        "name": "Item2"
      },
      {
        "@id": "<element id>",
        "@type": "EnumValue",
        "displayName": {
          "en": "Item3"
        },
        "enumValue": 2,
        "name": "Item3"
      }
    ]
  }
}
```

Het apparaat ontvangt de volgende payload van IoT Central:

```json
{  
  "EnumPropertyWritable":  1 , "$version": 10
}
```

Het apparaat moet de volgende JSON-Payload verzenden naar IoT Central nadat de update is verwerkt. Dit bericht bevat het versie nummer van de oorspronkelijke update die is ontvangen van IoT Central. Wanneer IoT Central dit bericht ontvangt, wordt de eigenschap gemarkeerd als **gesynchroniseerd** in de gebruikers interface:

```json
{
  "EnumPropertyWritable": {
    "value": 1,
    "ac": 200,
    "ad": "completed",
    "av": 10
  }
}
```

## <a name="commands"></a>Opdracht

### <a name="synchronous-command-types"></a>Synchrone opdracht typen

Het volgende code fragment van een DCM toont de definitie van een synchrone opdracht die geen para meters heeft en waarvan niet wordt verwacht dat het apparaat iets retourneert:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "displayName": {
    "en": "SynchronousCommandBasic"
  },
  "name": "SynchronousCommandBasic"
}
```

Het apparaat ontvangt een lege Payload in de aanvraag en retourneert een lege nettolading in het antwoord met een `200` http-antwoord code om aan te geven dat dit is geslaagd.

Het volgende code fragment van een DCM toont de definitie van een synchrone opdracht met een gehele para meter en die het apparaat verwacht een geheel getal te retour neren:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "SynchronousCommandSimple"
  },
  "name": "SynchronousCommandSimple"
}
```

Het apparaat ontvangt een geheel getal als de aanvraag lading. Het apparaat moet een geheel getal retour neren als de reactie Payload met een `200` http-antwoord code om aan te geven dat dit is geslaagd.

Het volgende code fragment van een DCM toont de definitie van een synchrone opdracht die een object parameter heeft en die het apparaat verwacht een object te retour neren. In dit voor beeld hebben beide objecten de velden integer en string:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "synchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": {
      "@id": "<element id>",
      "@type": "Object",
      "displayName": {
        "en": "Object"
      },
      "fields": [
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field1"
          },
          "name": "Field1",
          "schema": "integer"
        },
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field2"
          },
          "name": "Field2",
          "schema": "string"
        }
      ]
    }
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": {
      "@id": "<element id>",
      "@type": "Object",
      "displayName": {
        "en": "Object"
      },
      "fields": [
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field1"
          },
          "name": "Field1",
          "schema": "integer"
        },
        {
          "@id": "<element id>",
          "@type": "SchemaField",
          "displayName": {
            "en": "Field2"
          },
          "name": "Field2",
          "schema": "string"
        }
      ]
    }
  },
  "displayName": {
    "en": "SynchronousCommandComplex"
  },
  "name": "SynchronousCommandComplex"
}
```

Het volgende code fragment toont een voor beeld van een Payload-aanvraag die naar het apparaat wordt verzonden:

```json
{ "Field1": 56, "Field2": "A string value" }
```

Het volgende code fragment toont een voor beeld van een nettolading van de reactie die wordt verzonden vanaf het apparaat. Gebruik een `200` http-antwoord code om het volgende te bepalen:

```json
{ "Field1": 87, "Field2": "Another string value" }
```

### <a name="asynchronous-command-types"></a>Asynchrone opdracht typen

Het volgende code fragment vanuit een DCM toont de definitie van een asynchrone opdracht. De opdracht heeft een gehele para meter en verwacht dat het apparaat een geheel getal retourneert:

```json
{
  "@id": "<element id>",
  "@type": "Command",
  "commandType": "asynchronous",
  "durable": false,
  "request": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "RequestParam"
    },
    "name": "RequestParam",
    "schema": "integer"
  },
  "response": {
    "@id": "<element id>",
    "@type": "SchemaField",
    "displayName": {
      "en": "ResponseParam"
    },
    "name": "ResponseParam",
    "schema": "integer"
  },
  "displayName": {
    "en": "AsynchronousCommandSimple"
  },
  "name": "AsynchronousCommandSimple"
}
```

Het apparaat ontvangt een geheel getal als de aanvraag lading. Het apparaat moet een lege nettolading retour neren met een `202` http-antwoord code om aan te geven dat het apparaat de aanvraag voor asynchrone verwerking heeft geaccepteerd.

Wanneer het apparaat klaar is met de verwerking van de aanvraag, moet er een eigenschap naar IoT Central worden verzonden die eruitziet als in het volgende voor beeld. De naam van de eigenschap moet hetzelfde zijn als de naam van de opdracht:

```json
{
  "AsynchronousCommandSimple": {
    "value": 87
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Als ontwikkel aars van apparaten weet u nu dat u de volgende stappen hebt doorstaan om een verbinding te maken met [Azure IOT Central](./concepts-get-connected.md) om meer te weten te komen over het registreren van apparaten met IOT Central en hoe IOT Central verbindingen met apparaten beveiligt.
