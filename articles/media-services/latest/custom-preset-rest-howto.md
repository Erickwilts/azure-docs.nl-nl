---
title: Codeer aangepaste transformeren met behulp van Media Services v3 REST - Azure | Microsoft Docs
description: In dit onderwerp laat zien hoe Azure Media Services v3 gebruiken om een aangepaste transformeren met behulp van REST.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/14/2019
ms.author: juliako
ms.openlocfilehash: 30e22cb786e5dc2a667fe41ca8edf398cf0b7613
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65761798"
---
# <a name="how-to-encode-with-a-custom-transform---rest"></a>Coderen met een aangepaste transformatie - REST

Wanneer u met Azure Media Services encoding, u kunt snel aan de slag met een van de aanbevolen ingebouwde voorinstellingen, gebaseerd op best practices, zoals geïllustreerd in de [Streaming bestanden](stream-files-tutorial-with-rest.md#create-a-transform) zelfstudie. U kunt ook een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat maken.

## <a name="considerations"></a>Overwegingen

Bij het maken van aangepaste voorinstellingen, de volgende overwegingen zijn van toepassing:

* Alle waarden voor de hoogte en breedte in AVC inhoud moet een meervoud van 4.
* In Azure Media Services v3 zijn alle van de codering bitsnelheden in bits per seconde. Dit wijkt af van het voorbeelddiagram met onze v2 API's, dat kilobits per seconde als de eenheid gebruikt. Bijvoorbeeld, als de bitrate in v2 is opgegeven als 128 (kilobits per seconde), zou in v3 deze worden ingesteld op 128000 (bits per seconde).

## <a name="prerequisites"></a>Vereisten 

- [Een Azure Media Services-account maken](create-account-cli-how-to.md). <br/>Zorg ervoor dat u de naam van de resourcegroep en de naam van de Media Services-account. 
- [Postman configureren voor Azure Media Services REST API-aanroepen](media-rest-apis-with-postman.md).<br/>Zorg ervoor dat u de laatste stap in het onderwerp [Azure AD Token ophalen](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-custom-preset"></a>Een aangepaste voorinstelling definiëren

Het volgende voorbeeld wordt de hoofdtekst van de aanvraag van een nieuwe transformatie gedefinieerd. We definiëren van een groep van de uitvoer die we worden gegenereerd willen wanneer deze transformatie wordt gebruikt. 

In dit voorbeeld we eerst een AacAudio-laag voor de audio codering en twee H264Video lagen toevoegen voor de videocodering. In de video lagen toewijzen we labels, zodat ze kunnen worden gebruikt in de namen van de uitvoer. Vervolgens gaan we de uitvoer naar alle miniaturen. In het onderstaande voorbeeld geven we afbeeldingen in PNG-indeling, die worden gegenereerd op 50% van de omzetting van de invoervideo en op drie tijdstempels - {% 25, 50%, 75} van de lengte van de invoervideo. Tot slot geven we de indeling voor de uitvoerbestanden: één voor video en audio, en een andere voor de miniaturen. Aangezien we meerdere H264Layers hebben, hebben we het gebruik van macro's die unieke namen per laag produceren. Kunnen we gebruiken een `{Label}` of `{Bitrate}` macro, in het voorbeeld ziet u de vorige.

```json
{
    "properties": {
        "description": "Basic Transform using a custom encoding preset",
        "outputs": [
            {
                "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                "preset": {
                    "@odata.type": "#Microsoft.Media.StandardEncoderPreset",
                    "codecs": [
                        {
                            "@odata.type": "#Microsoft.Media.AacAudio",
                            "channels": 2,
                            "samplingRate": 48000,
                            "bitrate": 128000,
                            "profile": "AacLc"
                        },
                        {
                            "@odata.type": "#Microsoft.Media.H264Video",
                            "keyFrameInterval": "PT2S",
                            "stretchMode": "AutoSize",
                            "sceneChangeDetection": false,
                            "complexity": "Balanced",
                            "layers": [
                                {
                                    "width": "1280",
                                    "height": "720",
                                    "label": "HD",
                                    "bitrate": 3400000,
                                    "maxBitrate": 3400000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                },
                                {
                                    "width": "640",
                                    "height": "360",
                                    "label": "SD",
                                    "bitrate": 1000000,
                                    "maxBitrate": 1000000,
                                    "bFrames": 3,
                                    "slices": 0,
                                    "adaptiveBFrame": true,
                                    "profile": "Auto",
                                    "level": "auto",
                                    "bufferWindow": "PT5S",
                                    "referenceFrames": 3,
                                    "entropyMode": "Cabac"
                                }
                            ]
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngImage",
                            "stretchMode": "AutoSize",
                            "start": "25%",
                            "step": "25%",
                            "range": "80%",
                            "layers": [
                                {
                                    "width": "50%",
                                    "height": "50%"
                                }
                            ]
                        }
                    ],
                    "formats": [
                        {
                            "@odata.type": "#Microsoft.Media.Mp4Format",
                            "filenamePattern": "Video-{Basename}-{Label}-{Bitrate}{Extension}",
                            "outputFiles": []
                        },
                        {
                            "@odata.type": "#Microsoft.Media.PngFormat",
                            "filenamePattern": "Thumbnail-{Basename}-{Index}{Extension}"
                        }
                    ]
                }
            }
        ]
    }
}

```

## <a name="create-a-new-transform"></a>Maak een nieuwe transformatie  

In dit voorbeeld maken we een **transformeren** die is gebaseerd op de aangepaste voorinstelling die we eerder hebt gedefinieerd. Bij het maken van een transformatie, moet u eerst gebruiken [ophalen](https://docs.microsoft.com/rest/api/media/transforms/get) om te controleren als er al een bestaat. Als de transformatie bestaat, dit opnieuw gebruiken. 

Selecteer in van de Postman-verzameling die u hebt gedownload, **transformaties en taken**->**maken of bijwerken transformeren**.

De **plaatsen** HTTP-aanvraagmethode is vergelijkbaar met:

```
PUT https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
```

Selecteer de **hoofdtekst** tabblad en vervang de hoofdtekst met de json-code u [eerder hebt gedefinieerd](#define-a-custom-preset). Voor Media Services toe te passen van de transformatie op de opgegeven video of audio, moet u een taak onder deze transformatie verzenden.

Selecteer **Verzenden**. 

Voor Media Services toe te passen van de transformatie op de opgegeven video of audio, moet u een taak onder deze transformatie verzenden. Zie voor een compleet voorbeeld die laat zien hoe u een taak onder een transformatie verzenden, [zelfstudie: Stream videobestanden - REST](stream-files-tutorial-with-rest.md).

## <a name="next-steps"></a>Volgende stappen

Zie [andere REST-bewerkingen](https://docs.microsoft.com/rest/api/media/)
