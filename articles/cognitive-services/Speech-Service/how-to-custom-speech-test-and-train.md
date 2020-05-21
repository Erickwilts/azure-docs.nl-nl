---
title: Gegevens voorbereiden voor de Custom Speech-spraak service
titleSuffix: Azure Cognitive Services
description: Wanneer u de nauw keurigheid van micro soft-spraak herkenning wilt testen of uw aangepaste modellen wilt trainen, hebt u audio-en tekst gegevens nodig. Op deze pagina hebben we betrekking op de typen gegevens, het gebruik en het beheer ervan.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: bc79dabe82ab02166e3aa60a378ff394bca25028
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725547"
---
# <a name="prepare-data-for-custom-speech"></a>Gegevens voorbereiden voor Custom Speech

Wanneer u de nauw keurigheid van micro soft-spraak herkenning wilt testen of uw aangepaste modellen wilt trainen, hebt u audio-en tekst gegevens nodig. Op deze pagina hebben we betrekking op de typen gegevens, het gebruik en het beheer ervan.

## <a name="data-types"></a>Gegevenstypen

In deze tabel worden de geaccepteerde gegevens typen vermeld, wanneer elk gegevens type moet worden gebruikt en de aanbevolen hoeveelheid. Niet elk gegevens type is vereist voor het maken van een model. De gegevens vereisten variëren, afhankelijk van het feit of u een model maakt of een training uitvoert.

| Gegevenstype | Gebruikt voor testen | Aanbevolen aantal | Gebruikt voor training | Aanbevolen aantal |
|-----------|-----------------|----------|-------------------|----------|
| [Audio](#audio-data-for-testing) | Ja<br>Gebruikt voor visuele inspectie | 5 + audio bestanden | Nee | N.v.t. |
| [Audio en Transcripten met menselijke labels](#audio--human-labeled-transcript-data-for-testingtraining) | Ja<br>Wordt gebruikt om de nauw keurigheid te evalueren | 0,5-5 uur audio | Ja | 1-1000 uur audio |
| [Gerelateerde tekst](#related-text-data-for-training) | Nee | N.v.t. | Ja | 1-200 MB aan Verwante tekst |

Bestanden moeten worden gegroepeerd op type in een gegevensset en worden geüpload als zip-bestand. Elke gegevensset kan slechts één gegevens type bevatten.

> [!TIP]
> Overweeg om voorbeeld gegevens te gebruiken om snel aan de slag te gaan. Bekijk deze GitHub-opslag plaats voor <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">voorbeeld <span class="docon docon-navigate-external x-hidden-focus"></span> Custom speech gegevens</a>

## <a name="upload-data"></a>Gegevens uploaden

Als u uw gegevens wilt uploaden, gaat u naar de <a href="https://speech.microsoft.com/customspeech" target="_blank">Custom speech Portal <span class="docon docon-navigate-external x-hidden-focus"></span> </a>. Klik in de portal op **gegevens uploaden** om de wizard te starten en uw eerste gegevensset te maken. U wordt gevraagd een type spraak gegevens voor uw gegevensset te selecteren voordat u uw gegevens kunt uploaden.

![Audio selecteren in de spraak Portal](./media/custom-speech/custom-speech-select-audio.png)

Elke gegevensset die u uploadt, moet voldoen aan de vereisten voor het gegevens type dat u kiest. Uw gegevens moeten correct zijn geformatteerd voordat ze worden geüpload. Correct opgemaakte gegevens zorgen ervoor dat deze nauw keurig worden verwerkt door de Custom Speech-Service. De vereisten worden weer gegeven in de volgende secties.

Nadat uw gegevensset is geüpload, hebt u een aantal opties:

* U kunt naar het tabblad **testen** navigeren en alleen audio visueel controleren of audio + Transcriptie-gegevens met menselijke labels.
* U kunt naar het tabblad **training** gaan en audio en gegevens van menselijk transcriptie of gerelateerde tekst gegevens gebruiken om een aangepast model te trainen.

## <a name="audio-data-for-testing"></a>Audio gegevens voor testen

Audio gegevens zijn optimaal voor het testen van de nauw keurigheid van het spraak-naar-tekst model van micro soft of een aangepast model. Houd er rekening mee dat audio gegevens worden gebruikt voor het inspecteren van de nauw keurigheid van spraak met betrekking tot de prestaties van een specifiek model. Als u wilt weten wat de nauw keurigheid van een model is, gebruik dan [audio + met Human labels transcriptie-gegevens](#audio--human-labeled-transcript-data-for-testingtraining).

Gebruik deze tabel om ervoor te zorgen dat uw audio bestanden correct zijn ingedeeld voor gebruik met Custom Speech:

| Eigenschap                 | Waarde                 |
|--------------------------|-----------------------|
| Bestands indeling              | RIFF (WAV)            |
| Sample frequentie              | 8.000 Hz of 16.000 Hz |
| Kanalen                 | 1 (mono)              |
| Maximum lengte per audio | 2 uur               |
| Voorbeeld indeling            | PCM, 16-bits           |
| Archief indeling           | .zip                  |
| Maximale archief grootte     | 2 GB                  |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!TIP]
> Bij het uploaden van training en het testen van gegevens, mag de zip-bestand niet groter zijn dan 2 GB. Als u meer gegevens nodig hebt voor de training, splitst u deze op in verschillende zip-bestanden en uploadt u deze afzonderlijk. Later kunt u kiezen voor het trainen van *meerdere* gegevens sets. U kunt echter alleen testen vanuit *één* gegevensset.

Gebruik <a href="http://sox.sourceforge.net" target="_blank" rel="noopener">Sox <span class="docon docon-navigate-external x-hidden-focus"></span> </a> om audio-eigenschappen te controleren of bestaande audio om te zetten in de juiste notaties. Hieronder ziet u enkele voor beelden van de manier waarop elk van deze activiteiten kan worden uitgevoerd via de SoX-opdracht regel:

| Activiteit | Beschrijving | SoX-opdracht |
|----------|-------------|-------------|
| Audio-indeling controleren | Gebruik deze opdracht om te controleren<br>de indeling van het audio bestand. | `sox --i <filename>` |
| Audio-indeling converteren | Gebruik deze opdracht om te converteren<br>het audio bestand op één kanaal, 16-bits, 16 KHz. | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>Audio en transcriptie gegevens met menselijke labels voor testen/training

Als u de nauw keurigheid van de spraak-naar-tekst nauwkeurigheid van micro soft tijdens het verwerken van uw audio bestanden wilt meten, moet u transcripties (woord voor woord) van de mens voorzien voor vergelijking. Hoewel menselijke labels transcriptie vaak tijdrovend zijn, is het nood zakelijk om nauw keurigheid te evalueren en het model te trainen voor uw gebruiks voorbeelden. Houd er rekening mee dat de verbeteringen in de herkenning alleen van belang zijn voor de gegevens. Daarom is het belang rijk dat alleen transcripten van hoogwaardige kwaliteit worden geüpload.

Audio bestanden kunnen stilte aan het begin en het einde van de opname hebben. Indien mogelijk moet u ten minste een halve seconde van stilte voor en na de spraak in elk voorbeeld bestand toevoegen. Hoewel audio met een laag opname volume of storende achtergrond ruis niet nuttig is, kan het niet goed zijn om uw aangepaste model te zien. Denk altijd aan het upgraden van uw hardware en signaal verwerkings apparatuur voordat u audio voorbeelden gaat verzamelen.

| Eigenschap                 | Waarde                               |
|--------------------------|-------------------------------------|
| Bestands indeling              | RIFF (WAV)                          |
| Sample frequentie              | 8.000 Hz of 16.000 Hz               |
| Kanalen                 | 1 (mono)                            |
| Maximum lengte per audio | 2 uur (testen)/60 s (training) |
| Voorbeeld indeling            | PCM, 16-bits                         |
| Archief indeling           | .zip                                |
| Maximale grootte van zip         | 2 GB                                |

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

> [!NOTE]
> Bij het uploaden van training en het testen van gegevens, mag de zip-bestand niet groter zijn dan 2 GB. U kunt alleen testen vanuit *één* gegevensset, zorg ervoor dat deze binnen de juiste bestands grootte blijft. Daarnaast kan elk trainings bestand niet groter zijn dan 60 seconden, anders wordt er een fout opgetreden.

Voor het oplossen van problemen zoals het verwijderen of vervangen van woorden, is een aanzienlijke hoeveelheid gegevens vereist om de herkenning te verbeteren. Over het algemeen is het raadzaam om per woord transcripties te bieden voor ongeveer 10 tot 1.000 uur aan audio. De transcripties voor alle WAV-bestanden moeten worden opgenomen in één bestand met tekst zonder opmaak. Elke regel van het transcriptiebestand moet de naam van een van de audiobestanden bevatten, gevolgd door de bijbehorende transcriptie. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t).

  Bijvoorbeeld:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```

> [!IMPORTANT]
> Transcriptie moet worden gecodeerd als UTF-8 BOM (Byte Order Mark).

De tekst van de transcripties wordt genormaliseerd zodat ze door het systeem kunnen worden verwerkt. Er zijn echter enkele belang rijke normalisaties die moeten worden uitgevoerd voordat u de gegevens naar de speech Studio uploadt. Voor de juiste taal die moet worden gebruikt wanneer u uw transcripties voorbereidt, Zie [How to Create a Human-gelabeld transcriptie](how-to-custom-speech-human-labeled-transcriptions.md)

Nadat u uw audio bestanden en bijbehorende transcripties hebt verzameld, pakt u deze als één ZIP-bestand in voordat u het uploadt naar de <a href="https://speech.microsoft.com/customspeech" target="_blank">Custom speech Portal <span class="docon docon-navigate-external x-hidden-focus"></span> </a>. Hieronder ziet u een voor beeld van een gegevensset met drie audio bestanden en een transcriptie-bestand met menselijke Labels:

> [!div class="mx-imgBorder"]
> ![Audio selecteren in de spraak Portal](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>Gerelateerde tekst gegevens voor training

Product namen of-onderdelen die uniek zijn, moeten gerelateerde tekst gegevens bevatten voor training. Gerelateerde tekst zorgt voor een juiste herkenning. Er kunnen twee typen gerelateerde tekst gegevens worden gegeven om de herkenning te verbeteren:

| Gegevenstype | Hoe deze gegevens de herkenning verbeteren |
|-----------|------------------------------------|
| Zinnen (uitingen) | Verbeter de nauw keurigheid bij het herkennen van product namen of branchespecifieke vocabulaire in de context van een zin. |
| Uitspraak | De uitspraak van ongebruikelijke termen, acroniemen of andere woorden met niet-gedefinieerde uitspraaken verbeteren. |

Zinnen kunnen worden gegeven als één tekst bestand of meerdere tekst bestanden. Gebruik voor het verbeteren van de nauw keurigheid tekst gegevens die zich dichter bij de verwachte gesp roken uitingen bevindt. Uitspraak moet worden gegeven als één tekst bestand. Alles kan worden verpakt als één ZIP-bestand en worden geüpload naar de <a href="https://speech.microsoft.com/customspeech" target="_blank">Custom speech <span class="docon docon-navigate-external x-hidden-focus"></span> Portal </a>.

### <a name="guidelines-to-create-a-sentences-file"></a>Richt lijnen voor het maken van een sentence-bestand

Als u een aangepast model met behulp van zinnen wilt maken, moet u een lijst met voorbeeld uitingen opgeven. Uitingen hoeft _niet_ volledig of grammaticaal correct te zijn, maar ze moeten de gesp roken invoer die u verwacht in productie nauw keurig weer spie gelen. Als u wilt dat bepaalde voor waarden een verhoogd gewicht hebben, voegt u meerdere zinnen toe die deze specifieke voor waarden bevatten.

Als algemene richt lijn is aanpassing van modellen het meest effectief wanneer de tekst van de training zo dicht mogelijk bij de werkelijke tekst van de productie wordt verwacht. Specifiek jargon en zinsdelen van het domein dat u als doel hebt gericht, moeten worden opgenomen in de trainings tekst. Als dat mogelijk is, kunt u op een afzonderlijke regel proberen om één zin of sleutel woord te beheren. Voor tref woorden en zinsdelen die belang rijk voor u zijn (bijvoorbeeld product namen), kunt u ze een paar keer kopiëren. Maar houd er rekening mee dat u niet te veel kunt kopiëren. Dit kan van invloed zijn op het algemene herkennings aantal.

Gebruik deze tabel om ervoor te zorgen dat het gerelateerde gegevens bestand voor uitingen correct is ingedeeld:

| Eigenschap | Waarde |
|----------|-------|
| Tekstcodering | UTF-8 BOM |
| Aantal utterances per regel | 1 |
| Maximale bestandsgrootte | 200 MB |

Daarnaast wilt u rekening met de volgende beperkingen:

* Vermijd meer dan vier keer herhaalde tekens. Bijvoorbeeld: ' AAAA ' of ' UUUU '.
* Gebruik geen speciale tekens of UTF-8-tekens hierboven `U+00A1` .
* Uri's worden geweigerd.

### <a name="guidelines-to-create-a-pronunciation-file"></a>Richt lijnen voor het maken van een uitspraak bestand

Als er ongebruikelijke voor waarden zijn zonder standaard uitspraak dat uw gebruikers zich kunnen voordoen of gebruiken, kunt u een aangepast uitspraak bestand opgeven om de herkenning te verbeteren.

> [!IMPORTANT]
> Het is niet raadzaam om aangepaste uitspraak bestanden te gebruiken om de uitspraak van veelvoorkomende woorden te wijzigen.

Dit omvat voor beelden van een gesp roken utterance en een aangepaste uitspraak voor elk:

| Herkend/weer gegeven formulier | Gesp roken formulier |
|--------------|--------------------------|
| 3CPO | drie c p |
| CNTK | c n t k |
| IEEE | drievoudige e |

Het gesp roken formulier is de fonetische volg orde die is gespeld. Het kan bestaan uit letter, woorden, letter grepen of een combi natie van alle drie.

Aangepaste uitspraak is beschikbaar in het Engels ( `en-US` ) en Duits ( `de-DE` ). In deze tabel worden de ondersteunde tekens per taal weer gegeven:

| Taal | Landinstelling | Aantal |
|----------|--------|------------|
| Engels | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| Duits | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

Gebruik de volgende tabel om ervoor te zorgen dat het gerelateerde gegevens bestand voor uitspraak de juiste indeling heeft. Uitspraak bestanden zijn klein en mogen slechts enkele kilo bytes groot zijn.

| Eigenschap | Waarde |
|----------|-------|
| Tekstcodering | UTF-8-stuk lijst (ANSI wordt ook ondersteund voor Engels) |
| aantal uitspraakingen per regel | 1 |
| Maximale bestandsgrootte | 1 MB (1 KB voor gratis laag) |

## <a name="next-steps"></a>Volgende stappen

* [Uw gegevens controleren](how-to-custom-speech-inspect-data.md)
* [Uw gegevens evalueren](how-to-custom-speech-evaluate-data.md)
* [Uw model trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
