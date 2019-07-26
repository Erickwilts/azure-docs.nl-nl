---
title: Upgrade Bing Spellingcontrole-API v5 naar v7
titleSuffix: Azure Cognitive Services
description: Identificeert de onderdelen van uw toepassing die u moet bijwerken om versie 7 te gebruiken.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: 545772a28a67310b12eb55cd1fb14e8d12a95a58
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68500822"
---
# <a name="spell-check-api-upgrade-guide"></a>Upgrade handleiding Spell Check-API

Deze upgrade handleiding bevat de wijzigingen tussen versie 5 en versie 7 van de Bing Spellingcontrole-API. Gebruik deze hand leiding om u te helpen bij het identificeren van de onderdelen van uw toepassing die u moet bijwerken om versie 7 te gebruiken.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versie nummer van het eind punt is gewijzigd van v5 naar v7. Bijvoorbeeld `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Fout bericht objecten en fout codes

- Alle mislukte aanvragen moeten nu een `ErrorResponse` object bevatten in de hoofd tekst van het antwoord.

- De volgende velden zijn toegevoegd aan `Error` het object.  
  - `subCode`&mdash;Partitioneert de fout code indien mogelijk naar discrete buckets
  - `moreDetails`&mdash;Aanvullende informatie over de fout die in het `message` veld wordt beschreven
   

- De V5-fout codes zijn vervangen door de `code` volgende `subCode` mogelijke en waarden.  
  
|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert server error wanneer een van de voor waarden van de subcode zich voordoet. Het antwoord bevat deze fouten als de HTTP-status code 500 is.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer een deel van de aanvraag niet geldig is. Een vereiste para meter ontbreekt bijvoorbeeld of een parameter waarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-status code 400.<br/><br/>Als de fout HttpNotAllowed is, wordt de HTTP-status code 410.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u het quotum voor query's per seconde (QPS) of query's per maand (QPM) overschrijdt.<br/><br/>Bing retourneert HTTP-status code 429 als u QPS en 403 hebt overschreden als u QPM hebt overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie niet kan verifiëren. De `Ocp-Apim-Subscription-Key` koptekst ontbreekt bijvoorbeeld of de abonnements sleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatie methode opgeeft.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-status code 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper geen machtigingen heeft voor toegang tot de resource. Dit kan gebeuren als de abonnements sleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-status code 403.

- De volgende fout codes worden toegewezen aan de nieuwe codes. Als u een afhankelijkheid van V5-fout codes hebt genomen, werkt u de code dienovereenkomstig bij.  
  
|Versie 5-code|Versie 7 code. subcode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Uitgeschakeld|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
Niet geïmplementeerd|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Geblokkeerd|InvalidRequest. blocked

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisten voor gebruik en weergave](./UseAndDisplayRequirements.md)
