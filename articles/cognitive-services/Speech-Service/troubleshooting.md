---
title: De spraak-SDK - Speech Services oplossen
titleSuffix: Azure Cognitive Services
description: Dit artikel bevat informatie om u te helpen bij het oplossen van problemen die mogelijk optreden wanneer u de spraak-SDK gebruiken.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: a3741cb351b11b8cfd7c5d38713bb71232cf010e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515437"
---
# <a name="troubleshoot-the-speech-sdk"></a>Problemen met de Speech-SDK oplossen

Dit artikel bevat informatie om u te helpen bij het oplossen van problemen die mogelijk optreden wanneer u de spraak-SDK gebruiken.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Fout: WebSocket-Upgrade is mislukt met een verificatiefout (403)

Mogelijk hebt u het juiste eindpunt voor uw regio of de service. Controleer de URI om te controleren of dat deze juist is.

Bovendien kan er een probleem met uw abonnementssleutel of autorisatie token. Zie de volgende sectie voor meer informatie.

## <a name="error-http-403-forbidden-or-http-401-unauthorized"></a>Fout: HTTP 403-verboden of HTTP 401-niet toegestaan

Deze fout wordt vaak veroorzaakt door verificatieproblemen met. Verbindingsaanvragen zonder een geldig `Ocp-Apim-Subscription-Key` of `Authorization` header met de status 403 of 401 worden afgewezen.

* Als u een abonnementssleutel voor verificatie gebruikt, ziet u mogelijk de fout omdat:

    - de abonnementssleutel is ongeldig of ontbreekt
    - u hebt overschreden quotum voor het gebruik van uw abonnement

* Als u een verificatietoken voor de verificatie gebruikt, ziet u mogelijk de fout omdat:

    - het verificatietoken is ongeldig
    - het verificatietoken is verlopen

### <a name="validate-your-subscription-key"></a>Uw abonnementssleutel valideren

U kunt controleren of dat u beschikt over een sleutel geldig abonnement door het uitvoeren van een van de volgende opdrachten.

> [!NOTE]
> Vervang `YOUR_SUBSCRIPTION_KEY` en `YOUR_REGION` met uw eigen abonnementssleutel en een gekoppelde regio.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

Als u een geldig abonnement-sleutel hebt opgegeven, de opdracht retourneert een verificatietoken, anders wordt een fout geretourneerd.

### <a name="validate-an-authorization-token"></a>Een verificatietoken valideren

Als u een verificatietoken voor de verificatie gebruikt, voert u een van de volgende opdrachten om te verifiëren dat het Autorisatietoken nog geldig is. Tokens zijn geldig voor 10 minuten.

> [!NOTE]
> Vervang `YOUR_AUDIO_FILE` met het pad naar uw vooraf opgenomen audiobestand. Vervang `YOUR_ACCESS_TOKEN` geretourneerd door het Autorisatietoken in de vorige stap. Vervang `YOUR_REGION` met de juiste regio.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'

    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }

    # Read audio into byte array.
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

    # Show the result.
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

Als u een geldige Autorisatietoken hebt ingevoerd, wordt met de opdracht de transcriptie voor uw audio-bestand, anders wordt die er een fout retourneert.

---

## <a name="error-http-400-bad-request"></a>Fout: HTTP 400-Ongeldige aanvraag

Deze fout treedt meestal op wanneer de aanvraagtekst ongeldige audiogegevens bevat. Alleen WAV-indeling wordt ondersteund. Controleer ook of de aanvraagheaders om te controleren of u de juiste waarden voor opgeven `Content-Type` en `Content-Length`.

## <a name="error-http-408-request-timeout"></a>Fout: HTTP-408 time-out van aanvraag

De meest waarschijnlijke fout treedt op omdat er geen audiogegevens worden verzonden naar de service. Deze fout kan ook worden veroorzaakt door netwerkproblemen.

## <a name="recognitionstatus-in-the-response-is-initialsilencetimeout"></a>"RecognitionStatus' in het antwoord is"InitialSilenceTimeout"

Dit probleem wordt meestal veroorzaakt door audiogegevens. U kunt deze fout mogelijk ziet, omdat:

* Er is een lange stretch stilte aan het begin van de audio. In dat geval wordt de service stopt de opname na een paar seconden en retourneert `InitialSilenceTimeout`.

* Een niet-ondersteunde codec-indeling, die ervoor zorgt dat de gegevens worden behandeld als stilte maakt gebruik van de audio.

## <a name="next-steps"></a>Volgende stappen

* [Opmerkingen bij de release bekijken](releasenotes.md)
