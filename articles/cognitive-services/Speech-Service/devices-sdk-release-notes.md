---
title: Spraak apparaten SDK-documentatie
titleSuffix: Azure Cognitive Services
description: Opmerkingen bij de release - wat is gewijzigd in de meest recente versies
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: wellsi
ms.openlocfilehash: 1ca6b5265cf97ef551ec0b13b46ac934a372d2c7
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797912"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Opmerkingen bij de release van Cognitive Services spraak Devices SDK
De volgende secties lijst met wijzigingen in de meest recente versies.

## <a name="speech-devices-sdk-160"></a>Spraak apparaten SDK 1.6.0:

*   Ondersteuning voor [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) in Windows en Linux met gemeenschappelijke [voorbeeldtoepassing](https://aka.ms/sdsdk-download)
*   Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.6.0. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>Spraak apparaten SDK 1.5.1:

*   Opnemen [conversatie transcriptie](conversation-transcription-service.md) in de voorbeeld-app.
*   Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.5.1. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-150-2019-may-release"></a>Cognitive Services spraak apparaten SDK 1.5.0: 2019 mei release

*   Spraak Devices SDK is nu algemeen beschikbaar en niet langer een gated Preview-versie.
*   Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.5.0. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).
*   Nieuwe wake word technologie biedt aanzienlijke kwaliteitsverbeteringen op het gebied, kunt u belangrijke wijzigingen.
*   Nieuwe pijplijn audio verwerking voor opname van verbeterde ver-veld.

**Belangrijke wijzigingen**

*   Vanwege de nieuwe wake word technologie moet alle wake woorden opnieuw worden gemaakt van onze verbeterde wake word-portal. Oude trefwoorden van het apparaat verwijderen om volledig te verwijderen van de oude app.
    - ADB com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp verwijderen

## <a name="cognitive-services-speech-devices-sdk-140-2019-apr-release"></a>Cognitive Services spraak apparaten SDK 1.4.0: 2019 Apr release

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.4.0. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>Cognitive Services spraak apparaten SDK 1.3.1: 2019 maart-release

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.3.1. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).
*   Bijgewerkte wake woord verwerken, belangrijke wijzigingen zien.
*   Voorbeeld van een toepassing wordt toegevoegd keuze van taal spraakherkenning en voor omzetting.

**Belangrijke wijzigingen**

*   [Installeren van een woord wake](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) is vereenvoudigd, het is nu onderdeel van de app en hiervoor hoeft geen afzonderlijke installatie op het apparaat.
*   De opname van wake word is gewijzigd en twee gebeurtenissen worden ondersteund.
    - RecognizingKeyword, geeft aan dat het resultaat van spraak (niet-geverifieerde) sleutelwoord tekst bevat.
    - RecognizedKeyword, geeft aan dat sleutelwoord de opname voltooid herkennen van het opgegeven trefwoord.


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>Cognitive Services spraak apparaten SDK 1.1.0: November 2018-release

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.1.0. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).
* Uiterst veld spraakherkenning is, verbeterd met onze verbeterde verwerking van audio-algoritme.
* Voorbeeld van een toepassing Chinese spraakherkenning ondersteuning toegevoegd.

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>Cognitive Services spraak apparaten SDK 1.0.1: Release van oktober 2018

* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 1.0.1. Zie voor meer informatie de [opmerkingen bij de release](https://aka.ms/csspeech/whatsnew).
* Nauwkeurigheid van de spraakherkenning zal worden verbeterd met onze verbeterde verwerking van audio-algoritme  
* Een continue erkenning audio-sessie-fout is verholpen.

**Belangrijke wijzigingen**

* Met deze release zijn een aantal belangrijke wijzigingen worden geïntroduceerd. Controleer of [deze pagina](https://aka.ms/csspeech/breakingchanges_1_0_0) voor meer informatie met betrekking tot de API's.
* Het model KWS bestanden zijn niet compatibel met spraak Devices SDK 1.0.1. De bestaande Wake Word-bestanden worden verwijderd nadat de nieuwe Wake Word-bestanden worden geschreven naar het apparaat.

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>Cognitive Services spraak apparaten SDK 0.5.0: 2018-Aug release

* De nauwkeurigheid van de spraakherkenning verbeterd door een bug in de van audio-verwerkingscode op te lossen.
* Bijgewerkt de [spraak SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) onderdeel naar versie 0.5.0. Zie voor meer informatie de [opmerkingen bij de release](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>Cognitive Services Speech Devices SDK 0.2.12733: 2018-mei release

De eerste openbare preview-versie van de Cognitive Services spraak apparaten SDK.
