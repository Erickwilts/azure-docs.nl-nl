---
title: Voor beelden van Translator Text-API Dictionary-methode
titleSuffix: Azure Cognitive Services
description: Gebruik de methode Translator Text-API Dictionary-voor beelden.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: swmachan
ms.openlocfilehash: 15d23016df9b0c85b9d252b4c4a9ea48d3608f75
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595050"
---
# <a name="translator-text-api-30-dictionary-examples"></a>Translator Text-API 3,0: Voorbeelden in woordenlijst

Biedt voor beelden die laten zien hoe termen in de woorden lijst in de context worden gebruikt. Deze bewerking wordt samen gebruikt bij het [opzoeken van woorden lijsten](./v3-0-dictionary-lookup.md).

## <a name="request-url"></a>Aanvraag-URL

Een `POST` aanvraag verzenden naar:

```HTTP
https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Aanvraag parameters die zijn door gegeven voor de query reeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Description</th>
  <tr>
    <td>api-version</td>
    <td>*Vereiste para meter*.<br/>De versie van de API die door de client is aangevraagd. Waarde moet zijn `3.0`.</td>
  </tr>
  <tr>
    <td>from</td>
    <td>*Vereiste para meter*.<br/>Geeft de taal van de invoer tekst aan. De bron taal moet een van de [ondersteunde talen](./v3-0-languages.md) zijn die in het `dictionary` bereik zijn opgenomen.</td>
  </tr>
  <tr>
    <td>to</td>
    <td>*Vereiste para meter*.<br/>Hiermee geeft u de taal van de uitvoer tekst op. De doel taal moet een van de [ondersteunde talen](./v3-0-languages.md) zijn die in het `dictionary` bereik zijn opgenomen.</td>
  </tr>
</table>

Aanvraag headers zijn onder andere:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>Verificatie header (s)</td>
    <td>De <em>vereiste aanvraag header</em>.<br/>Bekijk de <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">beschik bare opties voor authenticatie</a>.</td>
  </tr>
  <tr>
    <td>Inhoudstype</td>
    <td>De *vereiste aanvraag header*.<br/>Hiermee geeft u het inhouds type van de payload op. Mogelijke waarden zijn: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-length</td>
    <td>De *vereiste aanvraag header*.<br/>De lengte van de aanvraag tekst.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Optioneel*.<br/>Een door de client gegenereerde GUID om de aanvraag uniek te identificeren. U kunt deze koptekst weglaten als u de tracerings-ID in de query reeks opneemt `ClientTraceId`met behulp van een query parameter met de naam.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagbody

De hoofd tekst van de aanvraag is een JSON-matrix. Elk matrix element is een JSON-object met de volgende eigenschappen:

  * `Text`: Een teken reeks die de term aangeeft die moet worden gezocht. Dit moet de waarde zijn van een `normalizedText` veld uit de back-vertalingen van een eerdere lookup-aanvraag voor een [woorden lijst](./v3-0-dictionary-lookup.md) . Dit kan ook de waarde van het `normalizedSource` veld zijn.

  * `Translation`: Een teken reeks waarmee de vertaalde tekst wordt opgegeven die eerder door de zoek bewerking van de [woorden lijst](./v3-0-dictionary-lookup.md) is geretourneerd. Dit moet de waarde zijn uit het `normalizedTarget` veld in de `translations` lijst van het antwoord op de [woorden lijst opzoeken](./v3-0-dictionary-lookup.md) . Met de service worden voor beelden geretourneerd voor het specifieke woord paar van de doel groep.

Een voor beeld is:

```json
[
    {"Text":"fly", "Translation":"volar"}
]
```

De volgende beperkingen zijn van toepassing:

* De matrix kan Maxi maal 10 elementen bevatten.
* De tekst waarde van een matrix element mag niet langer zijn dan 100 tekens, inclusief spaties.

## <a name="response-body"></a>Antwoord tekst

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke teken reeks in de invoer matrix. Een resultaat object bevat de volgende eigenschappen:

  * `normalizedSource`: Een teken reeks met de genormaliseerde vorm van de bron term. Over het algemeen moet dit gelijk zijn aan de waarde van `Text` het veld in de hoofd tekst van de aanvraag.
    
  * `normalizedTarget`: Een teken reeks met de genormaliseerde vorm van de doel term. Over het algemeen moet dit gelijk zijn aan de waarde van `Translation` het veld in de hoofd tekst van de aanvraag.
  
  * `examples`: Een lijst met voor beelden voor het paar (bron term, doel term). Elk element van de lijst is een object met de volgende eigenschappen:

    * `sourcePrefix`: De teken reeks die moet worden samengevoegd _vóór_ de `sourceTerm` waarde van om een volledig voor beeld te vormen. Voeg geen spatie toe, omdat deze al aanwezig is wanneer dit moet zijn. Deze waarde kan een lege teken reeks zijn.

    * `sourceTerm`: Een teken reeks die gelijk is aan de werkelijke term die wordt opgezocht. De teken reeks wordt toegevoegd `sourcePrefix` aan `sourceSuffix` en voor het volledige voor beeld. De waarde ervan wordt gescheiden, zodat deze kan worden gemarkeerd in een gebruikers interface, bijvoorbeeld door het vet te maken.

    * `sourceSuffix`: De teken reeks die moet worden samengevoegd _na_ de `sourceTerm` waarde van om een volledig voor beeld te vormen. Voeg geen spatie toe, omdat deze al aanwezig is wanneer dit moet zijn. Deze waarde kan een lege teken reeks zijn.

    * `targetPrefix`: Een teken reeks die `sourcePrefix` vergelijkbaar is met maar voor het doel.

    * `targetTerm`: Een teken reeks die `sourceTerm` vergelijkbaar is met maar voor het doel.

    * `targetSuffix`: Een teken reeks die `sourceSuffix` vergelijkbaar is met maar voor het doel.

    > [!NOTE]
    > Als er geen voor beelden in de woorden lijst staan, is de reactie 200 (OK) `examples` , maar is de lijst een lege lijst.

## <a name="examples"></a>Voorbeelden

In dit voor beeld ziet u hoe u voor beelden kunt opzoeken voor het paar dat `fly` is gemaakt met de `volar`Engelse term en de Spaanse vertaling.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/dictionary/examples?api-version=3.0&from=en&to=es" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'fly', 'Translation':'volar'}]"
```

---

De antwoord tekst (afgekort voor de duidelijkheid) is:

```
[
    {
        "normalizedSource":"fly",
        "normalizedTarget":"volar",
        "examples":[
            {
                "sourcePrefix":"They need machines to ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Necesitan máquinas para ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },      
            {
                "sourcePrefix":"That should really ",
                "sourceTerm":"fly",
                "sourceSuffix":".",
                "targetPrefix":"Eso realmente debe ",
                "targetTerm":"volar",
                "targetSuffix":"."
            },
            //
            // ...list abbreviated for documentation clarity
            //
        ]
    }
]
```
