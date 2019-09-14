---
title: 'Quickstart: Translator Speech-API Node.js'
titlesuffix: Azure Cognitive Services
description: Ontdek informatie en codevoorbeelden om u te helpen snel aan de slag te gaan met de Translator Speech-API.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: quickstart
ms.date: 3/5/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: fc5c6fe4040a8f4140cd18ff41373d50f74a6a55
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965397"
---
# <a name="quickstart-translator-speech-api-with-nodejs"></a>Quickstart: Translator Speech-API met Node.js
<a name="HOLTop"></a>

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

In dit artikel leest u hoe de Translator Speech-API gebruikt voor het vertalen van gesproken woorden in een WAV-bestand.

## <a name="prerequisites"></a>Vereisten

U hebt [Node.js 6](https://nodejs.org/en/download/) nodig om deze code uit te voeren.

U moet het [Websocket-pakket](https://www.npmjs.com/package/websocket) voor Node.js installeren.

In de map met het uitvoerbare bestand dat u hebt gemaakt op basis van de volgende code moet ook een WAV-bestand staan met de naam 'speak.wav'. Dit WAV-bestand moet de standaardindeling hebben: PCM, 16-bits, 16 kHz, mono. U kunt dit WAV-bestand verkrijgen via de [Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

U moet beschikken over een [account voor de Cognitive Services-API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) en de **Microsoft Translator Speech-API**. U hebt een betaalde abonnementssleutel nodig uit uw [Azure-dashboard](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Spraak vertalen

Met de volgende code wordt spraak van de ene naar de andere taal vertaald.

1. Maak een nieuw Node.js-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```nodejs
/* To install this dependency, run:
npm install websocket
*/
var wsClient = require('websocket').client;
var fs = require('fs');
/* To install this dependency, run:
npm install stream-buffers
*/
var streamBuffers = require('stream-buffers');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let key = 'ENTER KEY HERE';

let host = 'wss://dev.microsofttranslator.com';
let path = '/speech/translate';
let params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa';
let uri = host + path + params;

/* The input .wav file is in PCM 16bit, 16kHz, mono format.
You can obtain such a .wav file using the Text to Speech API. See:
https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis
*/
let input_path = 'speak.wav';

let output_path = 'speak2.wav';

function receive(message) {
    console.log ("Received message of type: " + message.type + ".");

    if (message.type == 'utf8') {
        var result = JSON.parse(message.utf8Data)
        console.log("Response type: " + result.type + ".");
        console.log("Recognized input as: " + result.recognition);
        console.log("Translation: " + result.translation);
    }
    else if (message.type == 'binary') {
/* This is needed to make sure message.binaryData is defined. See:
https://stackoverflow.com/questions/17546953/cant-access-object-property-even-though-it-exists-returns-undefined
*/
        let binary_data = JSON.parse (JSON.stringify (message.binaryData));

        if (binary_data.type == 'Buffer') {
/* Put the binary data in a Buffer - do not write it directly to the file. */
            let buffer = new Buffer(binary_data.data, 'binary');
/* Write the contents of the Buffer to the file. */
            fs.writeFile (output_path, buffer, 'binary', function (error) {
/* fs.writeFile calls the error-handling function even if there is no error, in which case error === null. See:
https://stackoverflow.com/questions/44473868/node-js-fs-writefile-err-returns-null
*/
                if (error !== null) {
                    console.log ("Error: " + error);
                }
            });
        }
    }
}

function send(connection, filename) {

    var myReadableStreamBuffer = new streamBuffers.ReadableStreamBuffer({
        frequency: 100,
        chunkSize: 32000
    });

/* Make sure the audio file is followed by silence.
This lets the service know that the audio file is finished.
At 32 bytes per millisecond, this is 100 seconds of silence. */
    myReadableStreamBuffer.put(fs.readFileSync(filename));
    myReadableStreamBuffer.put(new Buffer(3200000));
    myReadableStreamBuffer.stop();

    myReadableStreamBuffer.on('data', function (data) {
        connection.sendBytes(data);
    });

    myReadableStreamBuffer.on('end', function () {
        console.log('Closing connection.');
        connection.close(1000);
    });
}

function connect() {
    var ws = new wsClient();

    ws.on('connectFailed', function (error) {
        console.log('Connection error: ' + error.toString());
    });

    ws.on('connect', function (connection) {
        console.log('Connected.');

        connection.on('message', receive);

        connection.on('close', function (reasonCode, description) {
            console.log('Connection closed: ' + reasonCode);
        });

        connection.on('error', function (error) {
            console.log('Connection error: ' + error.toString());
        });

        send(connection, input_path);
    });

    ws.connect(uri, null, null, { 'Ocp-Apim-Subscription-Key' : key });
}

connect();
```

**Gesproken antwoord vertalen**

Deze handeling is geslaagd als het bestand 'speak2.wav' is gemaakt. Het bestand bevat de vertaling van de woorden die te horen zijn in 'speak.wav'.

[Terug naar boven](#HOLTop)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Translator Speech-zelfstudie](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Zie ook

[Translator Speech-overzicht](../overview.md)
[API-naslagmateriaal](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
