---
title: 'Quickstart: een miniatuur maken - REST, JavaScript'
titleSuffix: Azure Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision-API en JavaScript.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 626ee2ca81967ab088ab4b5aceda15a99231d118
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604332"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-javascript"></a>Quickstart: Genereer een miniatuur met behulp van de Computer Vision REST-API en JavaScript

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de REST API van Computer Vision. U geeft de hoogte en breedte, hoogte-breedteverhouding van de afbeelding kan verschillen. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet beschikken over een abonnementssleutel voor Computer Vision. U krijgt een gratis proefversie sleutel van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Of, volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) abonneren op de Computer Vision en haal uw sleutel.

## <a name="create-and-run-the-sample"></a>Het voorbeeld maken en uitvoeren

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code in een teksteditor.
1. Breng waar nodig de volgende wijzigingen in code aan:
    1. Vervang de waarde van `subscriptionKey` door uw abonnementssleutel.
    1. Vervang de waarde van `uriBase` door de eindpunt-URL van de methode [Miniatuur ophalen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) uit de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
    1. Vervang eventueel de waarde van het `value`-kenmerk voor het `inputImage`-besturingselement door de URL van een andere afbeelding die u wilt analyseren.
1. Sla de code op als een bestand met de extensie `.html`. Bijvoorbeeld `get-thumbnail.html`.
1. Open een browservenster.
1. Sleep het bestand in de browser naar het browservenster.
1. Wanneer de webpagina wordt weergegeven in de browser, kiest u de knop **Miniatuur genereren**.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Thumbnail Sample</title>
</head>
<body>

<script type="text/javascript">
    function processImage() {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace <Subscription Key> with your valid subscription key.
        var subscriptionKey = "<Subscription Key>";

        // You must use the same Azure region in your REST API method as you used to
        // get your subscription keys. For example, if you got your subscription keys
        // from the West US region, replace "westcentralus" in the URL
        // below with "westus".
        //
        // Free trial subscription keys are generated in the "westus" region.
        // If you use a free trial subscription key, you shouldn't need to change
        // this region.
        var uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail";

        // Request parameters.
        var params = "?width=100&height=150&smartCropping=true";

        // Display the source image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Prepare the REST API call:

        // Create the HTTP Request object.
        var xhr = new XMLHttpRequest();

        // Identify the request as a POST, with the URL and parameters.
        xhr.open("POST", uriBase + params);

        // Add the request headers.
        xhr.setRequestHeader("Content-Type","application/json");
        xhr.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

        // Set the response type to "blob" for the thumbnail image data.
        xhr.responseType = "blob";

        // Process the result of the REST API call.
        xhr.onreadystatechange = function(e) {
            if(xhr.readyState === XMLHttpRequest.DONE) {

                // Thumbnail successfully created.
                if (xhr.status === 200) {
                    // Show response headers.
                    var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                    document.getElementById("responseTextArea").value =
                        JSON.stringify(xhr.getAllResponseHeaders(), null, 2);

                    // Show thumbnail image.
                    var urlCreator = window.URL || window.webkitURL;
                    var imageUrl = urlCreator.createObjectURL(this.response);
                    document.querySelector("#thumbnailImage").src = imageUrl;
                } else {
                    // Display the error message. The error message is the response
                    // body as a JSON string. The code in this code block extracts
                    // the JSON string from the blob response.
                    var reader = new FileReader();

                    // This event fires after the blob has been read.
                    reader.addEventListener('loadend', (e) => {
                        document.getElementById("responseTextArea").value =
                            JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                    });

                    // Start reading the blob as text.
                    reader.readAsText(xhr.response);
                }
            }
        }

        // Make the REST API call.
        xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
    };
</script>

<h1>Generate thumbnail image:</h1>
Enter the URL to an image to use in creating a thumbnail image,
then click the <strong>Generate thumbnail</strong> button.
<br><br>
Image for thumbnail:
<input type="text" name="inputImage" id="inputImage"
    value="https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg" />
<button onclick="processImage()">Generate thumbnail</button>
<br><br>
<div id="wrapper" style="width:1160px; display:table;">
    <div id="jsonOutput" style="width:600px; display:table-cell;">
        Response:
        <br><br>
        <textarea id="responseTextArea" class="UIInput"
                  style="width:580px; height:400px;"></textarea>
    </div>
    <div id="imageDiv" style="width:420px; display:table-cell;">
        Source image:
        <br><br>
        <img id="sourceImage" width="400" />
    </div>
    <div id="thumbnailDiv" style="width:140px; display:table-cell;">
        Thumbnail:
        <br><br>
        <img id="thumbnailImage" />
    </div>
</div>
</body>
</html>
```

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord wordt geretourneerd als binaire gegevens - deze staan voor de afbeeldingsgegevens van de miniatuur. Als de aanvraag is geslaagd, wordt er een miniatuur gegenereerd op basis van de binaire gegevens in het antwoord. De miniatuur wordt in een browservenster weergegeven. Als de aanvraag mislukt, wordt het antwoord weergegeven in het consolevenster. De reactie op de mislukte aanvraag bevat een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan.

## <a name="next-steps"></a>Volgende stappen

Een JavaScript-toepassing verkennen die Computer Vision gebruikt om optische tekenherkenning (OCR) uit te voeren; slim bijgesneden miniaturen maken; plus visuele kenmerken, inclusief gezichten, in een afbeelding detecteren, categoriseren, labelen en beschrijven. Als u snel wilt experimenteren met de Computer Vision-API, gebruikt u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Zelfstudie voor de Computer Vision-API met JavaScript](../Tutorials/javascript-tutorial.md)
