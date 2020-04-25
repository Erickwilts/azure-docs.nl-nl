---
title: Naslag informatie over spraak naar tekst-API (REST)-Speech Service
titleSuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van de spraak-naar-tekst REST API. In dit artikel vindt u meer informatie over autorisatie opties, query opties, het structureren van een aanvraag en het ontvangen van een reactie.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: yinhew
ms.openlocfilehash: 005824b0953be741f47c027d121dbe073adca3ba
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131286"
---
# <a name="speech-to-text-rest-api"></a>REST API voor spraak-naar-tekst

Als alternatief voor de [Speech-SDK](speech-sdk.md)kunt u met de speech-service spraak-naar-tekst converteren met behulp van een rest API. Elk toegankelijk eind punt is gekoppeld aan een regio. Voor uw toepassing is een abonnements sleutel vereist voor het eind punt dat u wilt gebruiken. De REST API is zeer beperkt en mag alleen worden gebruikt in gevallen van de spraak- [SDK](speech-sdk.md) .

Voordat u de REST API voor spraak naar tekst gebruikt, moet u het volgende weten:

* Aanvragen die de REST API gebruiken en audio rechtstreeks verzenden, kunnen Maxi maal 60 seconden aan audio bevatten.
* De REST API voor spraak naar tekst retourneert alleen eind resultaten. Er zijn geen gedeeltelijke resultaten gegeven.

Als het verzenden van meer audio een vereiste is voor uw toepassing, kunt u overwegen om de [spraak-SDK](speech-sdk.md) of een rest API op basis van een bestand te gebruiken, zoals [batch transcriptie](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Regio's en eind punten

Het eind punt voor de REST API heeft de volgende indeling:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Vervang `<REGION_IDENTIFIER>` door de id die overeenkomt met de regio van uw abonnement uit deze tabel:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> De para meter language moet worden toegevoegd aan de URL om te voor komen dat er een 4xx HTTP-fout wordt ontvangen. Bijvoorbeeld, de taal die is ingesteld op Amerikaans-Engels met het eind punt VS `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`West is:.

## <a name="query-parameters"></a>Queryparameters

Deze para meters kunnen worden opgenomen in de query reeks van de REST-aanvraag.

| Parameter | Beschrijving | Vereist/optioneel |
|-----------|-------------|---------------------|
| `language` | Hiermee wordt de gesp roken taal aangegeven die wordt herkend. Zie [ondersteunde talen](language-support.md#speech-to-text). | Vereist |
| `format` | Geeft de resultaat indeling aan. Geaccepteerde waarden `simple` zijn `detailed`en. Eenvoudige resultaten zijn `RecognitionStatus`onder `DisplayText`andere `Offset`,, `Duration`en. Gedetailleerde antwoorden omvatten meerdere resultaten met betrouw bare waarden en vier verschillende representaties. De standaardinstelling is `simple`. | Optioneel |
| `profanity` | Hiermee geeft u op hoe scheld woorden in de herkennings resultaten moet worden afgehandeld. Geaccepteerde waarden `masked`zijn, waarbij de woorden met sterretjes `removed`worden vervangen, waardoor alle woorden van het resultaat worden verwijderd of `raw`, waarbij de scheld woorden in het resultaat wordt opgenomen. De standaardinstelling is `masked`. | Optioneel |
| `pronunciationScoreParams` | Hiermee geeft u de para meters op voor het weer geven van uitspraak cijfers in de herkennings resultaten, waarmee de uitspraak kwaliteit van spraak invoer wordt beoordeeld, met indica toren van nauw keurigheid, Fluency, volledigheid, enzovoort. Deze para meter is een base64-gecodeerde JSON met meerdere gedetailleerde para meters. Zie [para meters voor uitspraak beoordeling](#pronunciation-assessment-parameters) voor het maken van deze para meter. | Optioneel |
| `cid` | Wanneer u de [Custom speech Portal](how-to-custom-speech.md) gebruikt om aangepaste modellen te maken, kunt u aangepaste modellen gebruiken via hun **eind punt-id** op de pagina **implementatie** . Gebruik de **eind punt-id** als argument voor `cid` de query teken reeks parameter. | Optioneel |

## <a name="request-headers"></a>Aanvraagheaders

Deze tabel bevat de vereiste en optionele kopteksten voor aanvragen voor spraak naar tekst.

|Header| Beschrijving | Vereist/optioneel |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Uw abonnements sleutel voor spraak Services. | Deze header of `Authorization` is vereist. |
| `Authorization` | Een autorisatie token dat wordt voorafgegaan door `Bearer`het woord. Zie [Verificatie](#authentication) voor meer informatie. | Deze header of `Ocp-Apim-Subscription-Key` is vereist. |
| `Content-type` | Hierin worden de indeling en codec van de gegeven audio gegevens beschreven. Geaccepteerde waarden `audio/wav; codecs=audio/pcm; samplerate=16000` zijn `audio/ogg; codecs=opus`en. | Vereist |
| `Transfer-Encoding` | Hiermee geeft u op dat gesegmenteerde audio gegevens worden verzonden in plaats van één bestand. Gebruik deze header alleen als de audio gegevens worden gesegmenteerd. | Optioneel |
| `Expect` | Als u gesegmenteerde overdracht gebruikt `Expect: 100-continue`, verzendt u deze. De speech-service erkent de eerste aanvraag en wacht op aanvullende gegevens.| Vereist als gesegmenteerde audio gegevens worden verzonden. |
| `Accept` | Indien aanwezig, moet dit zijn `application/json`. De speech-service biedt resultaten in JSON. Sommige aanvraag raamwerken bieden een incompatibele standaard waarde. Het is raadzaam altijd in te voegen `Accept`. | Optioneel, maar aanbevolen. |

## <a name="audio-formats"></a>Audio-indelingen

Audio wordt verzonden in de hoofd tekst van de `POST` HTTP-aanvraag. Deze moet een van de volgende indelingen hebben:

| Indeling | Videocodec | Bitsnelheid | Sample frequentie  |
|--------|-------|---------|--------------|
| WAV    | PCM   | 16-bits  | 16 kHz, mono |
| OGG    | OPUS  | 16-bits  | 16 kHz, mono |

>[!NOTE]
>De bovenstaande indelingen worden ondersteund via REST API en WebSocket in de speech-service. De [Speech SDK](speech-sdk.md) ondersteunt momenteel de WAV-indeling met PCM-codec en [andere indelingen](how-to-use-codec-compressed-audio-input-streams.md).

## <a name="pronunciation-assessment-parameters"></a>Beoordelings parameters voor uitspraak

In deze tabel worden de vereiste en optionele para meters voor de uitspraak beoordeling weer gegeven.

| Parameter | Beschrijving | Vereist/optioneel |
|-----------|-------------|---------------------|
| ReferenceText | De tekst waarmee de uitspraak wordt geëvalueerd. | Vereist |
| GradingSystem | Het punt systeem voor de Score kalibratie. Geaccepteerde waarden `FivePoint` zijn `HundredMark`en. De standaardinstelling is `FivePoint`. | Optioneel |
| Granulariteit | De granulatie van de evaluatie. Geaccepteerde waarden `Phoneme`zijn, waarmee de score wordt weer gegeven op de volledige tekst, het woord `Word`-en foneem-niveau, waarin de Score van het volledige tekst `FullText`-en woord niveau wordt weer gegeven, waarin de score op het volledige tekst niveau wordt weer geven. De standaardinstelling is `Phoneme`. | Optioneel |
| Dimensie | Hiermee worden de uitvoer criteria gedefinieerd. Geaccepteerde waarden `Basic`zijn, waarbij alleen de nauw keurigheid `Comprehensive` wordt weer gegeven. hier worden de scores voor meer dimensies weer gegeven (bijvoorbeeld fluency Score en volledige score op het volledige tekst niveau, fout type op woord niveau). Controleer de [antwoord parameters](#response-parameters) om de definities van verschillende Score dimensies en Word-fout typen weer te geven. De standaardinstelling is `Basic`. | Optioneel |
| EnableMiscue | Hiermee wordt de miscue-berekening ingeschakeld. Als deze functie is ingeschakeld, worden de uitgesp roken woorden vergeleken met de verwijzings tekst en worden deze gemarkeerd met weglating/invoegen op basis van de vergelijking. Geaccepteerde waarden `False` zijn `True`en. De standaardinstelling is `False`. | Optioneel |
| ScenarioId | Een GUID die een aangepast punt systeem aangeeft. | Optioneel |

Hieronder ziet u een voor beeld van een JSON met de para meters voor uitspraak beoordeling:

```json
{
  "ReferenceText": "Good morning.",
  "GradingSystem": "HundredMark",
  "Granularity": "FullText",
  "Dimension": "Comprehensive"
}
```

De volgende voorbeeld code laat zien hoe u de para meters voor de uitspraak beoordeling opbouwt in de URL-query parameter:

```csharp
var pronunciationScoreParamsJson = $"{{\"ReferenceText\":\"Good morning.\",\"GradingSystem\":\"HundredMark\",\"Granularity\":\"FullText\",\"Dimension\":\"Comprehensive\"}}";
var pronunciationScoreParamsBytes = Encoding.UTF8.GetBytes(pronunciationScoreParamsJson);
var pronunciationScoreParams = Convert.ToBase64String(pronunciationScoreParamsBytes);
```

## <a name="sample-request"></a>Voorbeeld aanvraag

In het onderstaande voor beeld zijn de hostname en de vereiste headers opgenomen. Het is belang rijk te weten dat de service ook audio gegevens verwacht, die niet is opgenomen in dit voor beeld. Zoals eerder vermeld, wordt Chunking aanbevolen, maar dit is niet vereist.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP-statuscode

De HTTP-status code voor elke reactie wijst op geslaagde of veelvoorkomende fouten.

| HTTP-statuscode | Beschrijving | Mogelijke reden |
|------------------|-------------|-----------------|
| `100` | Doorgaan | De eerste aanvraag is geaccepteerd. Ga door met het verzenden van de rest van de gegevens. (Gebruikt met gesegmenteerde overdracht) |
| `200` | OK | De aanvraag is voltooid. de antwoord tekst is een JSON-object. |
| `400` | Ongeldige aanvraag | Er is geen taal code gegeven, geen ondersteunde taal, ongeldig audio bestand, enzovoort. |
| `401` | Niet geautoriseerd | De abonnements sleutel of het autorisatie token is ongeldig in de opgegeven regio of het ongeldige eind punt. |
| `403` | Verboden | De abonnements sleutel of het autorisatie token ontbreekt. |

## <a name="chunked-transfer"></a>Gesegmenteerde overdracht

Gesegmenteerde overdracht`Transfer-Encoding: chunked`() kan helpen de latentie van herkenning te verminderen. Hiermee kan de spraak service de verwerking van het audio bestand starten tijdens de verzen ding. De REST API biedt geen gedeeltelijke of tussentijdse resultaten.

Dit code voorbeeld laat zien hoe u audio kunt verzenden in segmenten. Alleen de eerste chunk moet de header van het audio bestand bevatten. `request`is een `HttpWebRequest` object dat is verbonden met het juiste rest-eind punt. `audioFile`het pad is naar een audio bestand op schijf.

```csharp
var request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = host;
request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";
request.AllowWriteStreamBuffering = false;

using (var fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    // Open a request stream and write 1024 byte chunks in the stream one at a time.
    byte[] buffer = null;
    int bytesRead = 0;
    using (var requestStream = request.GetRequestStream())
    {
        // Read 1024 raw bytes from the input audio file.
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Antwoord parameters

De resultaten worden als JSON geleverd. De `simple` notatie bevat deze velden op het hoogste niveau.

| Parameter | Beschrijving  |
|-----------|--------------|
|`RecognitionStatus`|Status, zoals `Success` voor geslaagde herkenning. Zie de volgende tabel.|
|`DisplayText`|De herkende tekst na hoofdletter gebruik, interpunctie, inverse tekst normalisatie (conversie van gesp roken tekst naar kortere formulieren, zoals 200 voor "200" of "Dr. Smit" voor "Doctor Smith") en grove maskers. Alleen aanwezig op geslaagd.|
|`Offset`|De tijd (in 100 eenheden) waarbij de herkende spraak wordt gestart in de audio stroom.|
|`Duration`|De duur (in 100-nano seconden eenheden) van de herkende spraak in de audio stroom.|

Het `RecognitionStatus` veld kan deze waarden bevatten:

| Status | Beschrijving |
|--------|-------------|
| `Success` | De herkenning is gelukt en `DisplayText` het veld is aanwezig. |
| `NoMatch` | Er is een spraak gedetecteerd in de audio stroom, maar er zijn geen woorden van de doel taal gevonden. Dit betekent meestal dat de herkennings taal een andere taal is dan die waarmee de gebruiker spreekt. |
| `InitialSilenceTimeout` | Het begin van de audio stroom bevat alleen stilte en er is een time-out opgetreden voor de service tijdens het wachten op spraak. |
| `BabbleTimeout` | Het begin van de audio stroom bevat alleen ruis en er is een time-out opgetreden voor de service tijdens het wachten op spraak. |
| `Error` | De herkennings service heeft een interne fout aangetroffen en kan niet worden voortgezet. Probeer het opnieuw als dat mogelijk is. |

> [!NOTE]
> Als de audio alleen uit scheld woorden bestaat en de `profanity` query parameter is ingesteld op `remove`, wordt door de service geen gesp roken resultaat geretourneerd.

De `detailed` indeling bevat dezelfde gegevens als de `simple` indeling, samen met `NBest`een lijst met alternatieve interpretaties van hetzelfde herkennings resultaat. Deze resultaten worden geclassificeerd op basis van de meest waarschijnlijke kans op minst. De eerste vermelding is hetzelfde als het belangrijkste herkennings resultaat.  Wanneer u de `detailed` indeling gebruikt `DisplayText` , wordt de `Display` voor elk resultaat in de `NBest` lijst weer gegeven.

Elk object in de `NBest` lijst bevat:

| Parameter | Beschrijving |
|-----------|-------------|
| `Confidence` | De betrouwbaarheids Score van de vermelding van 0,0 (geen betrouw baarheid) tot 1,0 (volledig vertrouwen) |
| `Lexical` | De lexicale vorm van de herkende tekst: de werkelijke woorden die worden herkend. |
| `ITN` | De inverse-text-genormaliseerde ("canonieke") vorm van de herkende tekst, met telefoon nummers, cijfers, afkortingen ("Doctor Smith" naar "Dr Smith") en andere toegepaste trans formaties. |
| `MaskedITN` | Het ITN-formulier waarbij de maskering voor scheld woorden is toegepast, indien aangevraagd. |
| `Display` | De weergave vorm van de herkende tekst, met lees tekens en hoofdletter gebruik. Deze para meter is hetzelfde als `DisplayText` de waarde die is opgegeven als `simple`de notatie is ingesteld op. |
| `AccuracyScore` | De score die de nauw keurigheid van de uitspraak van de opgegeven spraak aangeeft. |
| `FluencyScore` | De Score waarmee de fluency van de opgegeven spraak wordt aangegeven. |
| `CompletenessScore` | De Score waarmee de volledigheid van de opgegeven spraak wordt aangegeven door de verhouding van uitgesp roken woorden naar de volledige invoer te berekenen. |
| `PronScore` | De totale score die de uitspraak kwaliteit van de opgegeven spraak aangeeft. Dit wordt berekend op `AccuracyScore`basis `FluencyScore` van `CompletenessScore` en met gewicht. |
| `ErrorType` | Deze waarde geeft aan of een woord wordt wegge laten, wordt ingevoegd of verkeerd is uitgesp `ReferenceText`roken, vergeleken met. Mogelijke waarden zijn `None` (wat betekent dat er geen fout is in `Omission`dit `Insertion` woord `Mispronunciation`), en. |

## <a name="sample-responses"></a>Voorbeeld reacties

Een typisch antwoord voor `simple` herkenning:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Een typisch antwoord voor `detailed` herkenning:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

Een typische reactie op erkenning met beoordeling van de uitspraak:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "400000",
  "Duration": "11000000",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "good morning",
        "ITN" : "good morning",
        "MaskedITN" : "good morning",
        "Display" : "Good morning.",
        "PronScore" : 84.4,
        "AccuracyScore" : 100.0,
        "FluencyScore" : 74.0,
        "CompletenessScore" : 100.0,
        "Words": [
            {
              "Word" : "Good",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 500000,
              "Duration" : 2700000
            },
            {
              "Word" : "morning",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 5300000,
              "Duration" : 900000
            }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
