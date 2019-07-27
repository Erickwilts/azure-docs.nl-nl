---
title: Gegevens wijziging-LUIS
titleSuffix: Azure Cognitive Services
description: Informatie over hoe gegevens kunnen worden gewijzigd voordat voorspellingen in Language Understanding (LUIS)
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 67b56f09663aca35ed0843f50e2420b531c82833
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560827"
---
# <a name="alter-utterance-data-before-or-during-prediction"></a>ALTER utterance gegevens vóór of tijdens de voorspelling
LUIS biedt methoden voor het bewerken van de utterance vóór of tijdens de voorspelling. Het gaat hierbij spelling, en het verhelpen van problemen met de tijdzone voor prebuild datetimeV2 oplossen. 

## <a name="correct-spelling-errors-in-utterance"></a>Corrigeren van spelfouten in utterance
Maakt gebruik van LUIS [Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) te corrigeren van spelfouten in de utterance. LUIS moet de sleutel die is gekoppeld aan die service. De sleutel maken en vervolgens de sleutel toevoegen als een queryreeksparameter aan de [eindpunt](https://go.microsoft.com/fwlink/?linkid=2092356). 

U kunt ook corrigeren van spelfouten in de **Test** door het deelvenster [invoeren van de sleutel](luis-interactive-test.md#view-bing-spell-check-corrections-in-test-panel). De sleutel wordt opgeslagen als een variabele van sessie in de browser voor het deelvenster Test. De sleutel toevoegen aan het deelvenster Test in elke spelling gecorrigeerd gewenste browsersessie. 

Gebruik van de sleutel in het deelvenster testen en op het eindpunt tellen mee voor de [sleutelgebruik](https://azure.microsoft.com/pricing/details/cognitive-services/spellcheck-api/) quotum. LUIS implementeert Bing Spell Check-beperkingen voor tekstlengte. 

Het eindpunt moet twee parameters voor spellingcorrecties om te werken:

|Param|Waarde|
|--|--|
|`spellCheck`|booleaans|
|`bing-spell-check-subscription-key`|[Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) eindpuntsleutel|

Wanneer [Bing spellingcontrole controleren-API-versie 7](https://azure.microsoft.com/services/cognitive-services/spell-check/) detecteert een fout, de oorspronkelijke utterance en de gecorrigeerde utterance worden geretourneerd, samen met de voorspellingen van het eindpunt.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```
 
### <a name="whitelist-words"></a>Lijst met toegestane adressen woorden
De Bing spell check-API die wordt gebruikt in LUIS biedt geen ondersteuning voor een wit-lijst met woorden worden genegeerd tijdens de spelling controleren op wijzigingen. Als u acceptatielijst woorden of afkortingen wilt, verwerkt de utterance in de clienttoepassing met een acceptatielijst voordat de utterance worden verzonden naar LUIS voor intentie voorspelling.

## <a name="change-time-zone-of-prebuilt-datetimev2-entity"></a>Tijdzone van de vooraf gedefinieerde datetimeV2 entiteit wijzigen
Wanneer een LUIS-app maakt gebruik van de vooraf gedefinieerde datetimeV2 entiteit, kan een datum / tijdwaarde in het antwoord voorspelling worden geretourneerd. De tijdzone van de aanvraag wordt gebruikt om te bepalen van de juiste datum/tijd om terug te keren. Als de aanvraag afkomstig is van een bot of een andere gecentraliseerde toepassing voorafgaand aan LUIS, corrigeer dan de tijdzone die LUIS gebruikt. 

### <a name="endpoint-querystring-parameter"></a>Eindpunt querystring-parameter
De tijdzone wordt verholpen door het toevoegen van de tijdzone van de gebruiker naar de [eindpunt](https://go.microsoft.com/fwlink/?linkid=2092356) met behulp van de `timezoneOffset` param. De waarde van `timezoneOffset` moet de positief of negatief getal in minuten, voor het wijzigen van de tijd.  

|Param|Waarde|
|--|--|
|`timezoneOffset`|positief of negatief getal, in minuten|

### <a name="daylight-savings-example"></a>Voorbeeld van de zomer-en besparingen
Als u de geretourneerde vooraf gedefinieerde datetimeV2 om aan te passen voor zomer-en wintertijd, moet u de `timezoneOffset` querystring-parameter met een +/-waarde in minuten voor de [eindpunt](https://go.microsoft.com/fwlink/?linkid=2092356) query.

60 minuten toevoegen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de verlichting op? **timezoneOffset = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & staging = {Booleaanse waarde} & bing-spellingcontrole-controle-subscription-key = {tekenreeks} & logboek = {Booleaanse waarde}

60 minuten verwijderen: 

https://{Region}.API.cognitive.Microsoft.com/Luis/v2.0/Apps/{appId}?q=TURN de verlichting op? **timezoneOffset = 60**& uitgebreide = {Booleaanse waarde} & spellingcontrole = {Booleaanse waarde} & staging = {Booleaanse waarde} & bing-spellingcontrole-controle-subscription-key = {tekenreeks} & logboek = {Booleaanse waarde}

## <a name="c-code-determines-correct-value-of-timezoneoffset"></a>C#-code bepaalt de juiste waarde van timezoneOffset
De volgende C#-code gebruikt de [TimeZoneInfo](https://docs.microsoft.com/dotnet/api/system.timezoneinfo) van de klasse [FindSystemTimeZoneById](https://docs.microsoft.com/dotnet/api/system.timezoneinfo.findsystemtimezonebyid#examples) methode om te bepalen van de juiste `timezoneOffset` op basis van de tijd van systeem:

```CSharp
// Get CST zone id
TimeZoneInfo targetZone = TimeZoneInfo.FindSystemTimeZoneById("Central Standard Time");

// Get local machine's value of Now
DateTime utcDatetime = DateTime.UtcNow;

// Get Central Standard Time value of Now
DateTime cstDatetime = TimeZoneInfo.ConvertTimeFromUtc(utcDatetime, targetZone);

// Find timezoneOffset
int timezoneOffset = (int)((cstDatetime - utcDatetime).TotalMinutes);
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Juiste spelfouten in deze zelfstudie](luis-tutorial-bing-spellcheck.md)
