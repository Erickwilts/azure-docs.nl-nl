---
title: Taal ondersteuning-spraak service
titleSuffix: Azure Cognitive Services
description: De spraak service ondersteunt talloze talen voor conversie van spraak naar tekst en tekst naar spraak, samen met spraak omzetting. In dit artikel vindt u een uitgebreide lijst met taal ondersteuning per service functie.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 031367a8a05defad475ae077f9b38b7294837460
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559403"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Taal-en regio ondersteuning voor de spraak Services

Verschillende talen worden ondersteund voor verschillende functies van spraak Services. De volgende tabellen geven een overzicht van ondersteuning voor talen.

## <a name="speech-to-text"></a>Spraak-naar-tekst

Zowel de micro soft Speech Recognition SDK als de REST API ondersteunen de volgende talen (land instellingen). Verschillende niveaus van aanpassingen zijn beschikbaar voor elke taal.

  Code | Taal | [Akoestische aanpassing](how-to-customize-acoustic-models.md) | [Aanpassing van taal](how-to-customize-language-model.md) | [Uitspraak van aanpassing](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-bijvoorbeeld | Arabisch (Egypte), moderne standard | Nee | Ja | Nee
 CA-ES | Catalaans | Nee | Nee | Nee
 da-DK | Deens (Denemarken) | Nee | Nee | Nee
 de-DE | Duits (Duitsland) | Ja | Ja | Nee
 en-AU | Engels (Australië) | Nee | Ja | Ja
 NL-CA | Engels (Canada) | Nee | Ja | Ja
 en-GB | Engels (Verenigd Koninkrijk) | Nee | Ja | Ja
 NL-IN | English (India) | Ja | Ja | Ja
 NL-NZ | Engels (Nieuw-Zeeland) | Nee | Ja | Ja  
 en-US | Engels (Verenigde Staten) | Ja | Ja | Ja
 es-ES | Spaans (Spanje) | Ja | Ja | Nee
 es-MX | Spaans (Mexico) | Nee | Ja | Nee
 fi-FI | Fins (Finland) | Nee | Nee | Nee
 fr-CA | Frans (Canada) | Nee | Ja | Nee
 fr-FR | Frans (Frankrijk) | Ja | Ja | Nee
 hi-IN | Hindi (India) | Nee | Ja | Nee
 IT-IT | Italiaans (Italië) | Ja | Ja | Nee
 ja-JP | Japans (Japan) | Nee | Ja | Nee
 ko-KR | Koreaans (Korea) | Nee | Ja | Nee
 nb-NO | Noors (Bokmål) (Noorwegen) | Nee | Nee | Nee
 NL-NL | Nederlands (Nederland) | Nee | Ja | Nee
 pl-PL | Pools (Polen) | Nee | Nee | Nee
 pt-BR | Portugees (Brazilië) | Ja | Ja | Nee
 pt-PT | Portugees (Portugal) | Nee | Ja | Nee
 ru-RU | Russisch (Rusland) | Ja | Ja | Nee
 SV-SE | Zweeds (Zweden) | Nee | Nee | Nee
 zh-CN | Chinees (Mandarijn, vereenvoudigd) | Ja | Ja | Nee
 zh-HK | Chinees (Kantonees, traditioneel) | Nee | Ja | Nee
 zh-TW | Chinees (Taiwan Mandarijn) | Nee | Ja | Nee
 e-e | Thai (Thailand) | Nee | Nee | Nee


## <a name="text-to-speech"></a>Tekst naar spraak

De Text to Speech REST-API biedt ondersteuning voor deze stemmen, die ondersteuning biedt voor een specifieke taal en een regionale taal, geïdentificeerd door de landinstelling.

> [!IMPORTANT]
> De prijzen zijn afhankelijk van de standaard, aangepaste en Neural stemmen. Ga naar de pagina met [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) voor meer informatie.

### <a name="neural-voices"></a>Neural stemmen

Neural text-to-speech is een nieuw type spraak synthese dat wordt aangedreven door diepe Neural-netwerken. Wanneer u een Neural-stem gebruikt, is de gesynthesizerde spraak bijna niet van de menselijke opnamen onderscheiden.

Neural stemmen kunnen worden gebruikt om interacties uit te voeren met chat bots uitbreiden en virtuele assistenten die natuurlijk en aantrekkelijker zijn, en om digitale teksten, zoals e-books, te converteren naar Audiobooks en in-car navigatie systemen te verbeteren. Met het menselijke net zoals natuurlijke prosody en heldere afbakening van woorden, verlaagt Neural stemmen aanzienlijk op het Luis teren van vermoeidheid wanneer gebruikers met AI-systemen communiceren.

Zie [regio's](regions.md#standard-and-neural-voices)voor een volledige lijst met Neural stemmen en regionale Beschik baarheid.

Landinstelling | Taal | Geslacht | Volledige toewijzing van service naam | Korte spraak naam
--------|----------|--------|---------|------------
de-DE | Duits (Duitsland) | Vrouw | "Micro soft server Speech Text to Speech Voice (de-DE, KatjaNeural)" | "de-DE-KatjaNeural"
en-US | English (US) | Man | "Micro soft server Speech Text to Speech Voice (nl-nl, GuyNeural)" | "en-US-GuyNeural"
en-US | English (US) | Vrouw | "Micro soft server Speech Text to Speech Voice (nl-nl, JessaNeural)" | "en-US-JessaNeural"
IT-IT | Italiaans (Italië) | Vrouw |"Micro soft server Speech Text to Speech Voice (IT-IT, ElsaNeural)" | ' it-IT-ElsaNeural '
zh-CN | Chinees (vasteland) | Vrouw | "Micro soft server Speech Text to Speech Voice (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

> [!NOTE]
> U kunt de volledige service naam toewijzing of de naam van de korte spraak gebruiken in uw verzoeken voor spraak synthese.

### <a name="standard-voices"></a>Standaard stemmen

Meer dan 75 standaard stemmen zijn beschikbaar in meer dan 45 talen en land instellingen, waarmee u tekst kunt converteren naar gesynthesizerde spraak. Zie [regio's](regions.md#standard-and-neural-voices)voor meer informatie over regionale Beschik baarheid.

Landinstelling | Taal | Geslacht | Volledige toewijzing van service naam | Korte spraak naam
-------|----------|---------|----------|----------
ar-bijvoorbeeld\* | Arabisch (Egypte) | Vrouw | "Microsoft Server spraak tekst en spraak spraak (ar-bijvoorbeeld Hoda)" | "ar-EG-Hoda"
ar-SA | Arabisch (Saoedi-Arabië) | Man | "Microsoft Server spraak tekst en spraak, spraak (ar-SA, Naayf)" | "ar-SA-Naayf"
bg-BG | Bulgaars | Man | "Microsoft Server tekst naar spraak stem (bg-BG, Ivan)" | "bg-BG-Ivan"
CA-ES | Catalaans | Vrouw | "Microsoft Server tekst naar spraak stem (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS"
cs-CZ | Tsjechisch | Man | "Microsoft Server spraak tekst en spraak spraak (cs-CZ, Jakub)" | "cs-CZ-Jakub"
da-DK | Deens | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (da-DK, HelleRUS)" | "da-DK-HelleRUS"
de-AT | Duits (Oostenrijk) | Man | "Microsoft Server spraak tekst en spraak, spraak (de-AT, Michael)" | "de-Michael"
de CH | Duits (Zwitserland) | Man | "Microsoft Server spraak tekst en spraak, spraak (de-h, Karsten)" | "de-CH-Karsten"
de-DE | Duits (Duitsland) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Hedda)" | "de-DE-Hedda"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, HeddaRUS)" | "de-DE-HeddaRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Stefan, Apollo)" | "de-DE-Stefan-Apollo"
el GR | Grieks | Man | "Microsoft Server spraak tekst en spraak, spraak (el-GR, Stefanos)" | "El-GR-Stefanos"
en-AU | Engels (Australië) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, Catherine)" | "en-AU-Catherine"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, HayleyRUS)" | "en-AU-HayleyRUS"
NL-CA | Engels (Canada) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, Linda)" | "en-CA-Linda"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, HeatherRUS)" | "en-CA-HeatherRUS"
en-GB | English (UK) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (en-GB, George, Apollo)" | "en-GB-George-Apollo"
NL-Internet Explorer | Engels (Ierland) | Man | "Microsoft Server spraak tekst en spraak, spraak (en Internet Explorer, Jan)" | "en-IE-Johan"
NL-IN | English (India) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Heera, Apollo)" | "en-IN-heera-Apollo"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, PriyaRUS)" | "en-IN-PriyaRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Ravi, Apollo)" | "en-IN-Ravi-Apollo"
en-US | English (US) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
es-ES | Spaans (Spanje) |Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, Laura, Apollo)" | "es-ES-Laura-Apollo"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, HelenaRUS)" | "es-ES-HelenaRUS"
| | | Man | "Microsoft Server tekst naar spraak stem (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo"
es-MX | Spaans (Mexico) | Vrouw | "Microsoft Server-stem tekst naar spraak (es-MX, HildaRUS)" | "es-MX-HildaRUS"
| | | Man | "Microsoft Server-stem tekst naar spraak (es-MX, Raul, Apollo)" | "es-MX-Raul-Apollo"
fi-FI | Fins | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (fi-FI, HeidiRUS)" | "fi-FI-HeidiRUS"
fr-CA | Frans (Canada) |Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, Caroline)" | "FR-CA-Caroline"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, HarmonieRUS)" | "FR-CA-HarmonieRUS"
FR-h | Frans (Zwitserland)| Man | "Microsoft Server tekst naar spraak stem (fr-h, Guillaume)" | "FR-CH-Guillaume"
fr-FR | Frans (Frankrijk)| Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, Julia, Apollo)" | "fr-FR-Julie-Apollo"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Man | "Microsoft Server tekst naar spraak stem (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo"
hij IL| Hebreeuws (Israël) | Man| "Microsoft Server spraak tekst en spraak, spraak (hij-IL, Asaf)" | "he IL-Asaf"
hi-IN | Hindi (India) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana, Apollo)" | "Hi-IN-Kalpana-Apollo"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana)" | "Hi-IN-Kalpana"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Hemant)" | "Hi-IN-Hemant"
hr-HR | Kroatisch | Man | "Microsoft Server tekst naar spraak stem (hr-HR, Matej)" | "hr-HR-Matej"
hu-HU | Hongaars | Man | "Microsoft Server spraak tekst en spraak, spraak (hu-HU, Szabolcs)" | "hu-HU-Szabolcs"
id-ID | Indonesisch| Man | "Microsoft Server spraak tekst en spraak, spraak (id-ID, Andika)" | "id-ID-andika"
IT-IT | Italiaans | Man | "Microsoft Server spraak tekst en spraak, spraak (it-IT, Cosimo, Apollo)" | "it-IT-Cosimo-Apollo"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (it-IT, LuciaRUS)" | "it-IT-LuciaRUS"
ja-JP | Japans | Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Man | "Microsoft Server tekst naar spraak stem (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
ko-KR | Koreaans | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS"
ms-MY | Maleis | Man | "Microsoft Server-stem tekst naar spraak (ms-mijn Rizwan)" | ' MS-MY-Rizwan '
nb-NO | Noors | Vrouw | "Microsoft Server-stem tekst naar spraak (nb-NO HuldaRUS)" | "nb-NO-HuldaRUS"
NL-NL | Nederlands | Vrouw | "Microsoft Server tekst naar spraak stem (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
pl-PL | Pools | Vrouw | "Microsoft Server tekst naar spraak stem (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
pt-BR | Portugees (Brazilië) | Vrouw | "Microsoft Server tekst naar spraak stem (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS"
| | | Man |"Microsoft Server tekst naar spraak stem (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo"
pt-PT | Portugees (Portugal) | Vrouw | "Microsoft Server tekst naar spraak stem (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS"
ro-RO | Roemeens | Man | "Microsoft Server spraak tekst en spraak, spraak (ro-RO, Andrei)" | "ro-RO-Andrei"
ru-RU |Russisch| Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Man | "Microsoft Server tekst naar spraak stem (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, EkaterinaRUS)" | ru-RU-EkaterinaRUS
sk-SK | Slowaaks | Man | "Microsoft Server tekst naar spraak stem (sk-SK, Filip)" | "sk-SK-Filip"
sl-SI | Sloveens | Man | "Microsoft Server tekst naar spraak stem (sl-SI, Lado)" | "sl-SI-Lado"
SV-SE | Zweeds | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS"
TA-IN | Tamil (India) | Man | "Microsoft Server spraak tekst en spraak, spraak (ta-IN, Valluvar)" | "TA-IN-Valluvar"
te-IN | Telugu (India) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (te-IN, Chitra)" | "te-IN-Chitra"
e-e | Thais | Man | "Microsoft Server tekst naar spraak stem (th-TH, Pattara)" | ' th-do-Pattara '
tr-TR | Turks | Vrouw | "Microsoft Server tekst naar spraak stem (tr-TR, SedaRUS)" | "tr-TR-SedaRUS"
vi-VN | Vietnamees | Man | "Microsoft Server-stem tekst naar spraak (vi-VN, een)" | "VI-VN-a"
zh-CN | Chinees (vasteland) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo"
zh-HK | Chinees (Hongkong SAR) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Tracy, Apollo)" | "zh-HK-Tracy-Apollo"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK TracyRUS)" | "zh-HK-TracyRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Danny, Apollo)" | "zh-HK-Danny-Apollo"
zh-TW | Chinees (Taiwan) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Yating, Apollo)" | "zh-TW-Yating-Apollo"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo"

\* *ar bijvoorbeeld ondersteunt moderne Standard Arabisch (MSA).*

> [!NOTE]
> U kunt de volledige service naam toewijzing of de naam van de korte spraak gebruiken in uw verzoeken voor spraak synthese.

### <a name="customization"></a>Aanpassing

Stem aanpassing is beschikbaar voor de-DE, en-GB, en-IN, en-US, es-MX, fr-FR, it-IT, pt-BR en zh-CN. Selecteer de juiste land instelling die overeenkomt met de trainings gegevens die u nodig hebt om een aangepast spraak model te trainen. Als de opname gegevens die u hebt gesp roken in het Engels met een Britse accent, selecteert u en-GB.  

> [!NOTE]
> We bieden geen ondersteuning voor bidirectionele model trainingen in aangepaste spraak, met uitzonde ring van het Chinese Engelse bi-taal gebruik. Selecteer ' Chinees-Engels-tweetalig ' als u een Chinese stem wilt trainen die ook Engels kan spreken. Spraak training in alle land instellingen begint met een gegevensset van 2000 + uitingen, met uitzonde ring van de en-US en zh-CN, waar u kunt beginnen met elke grootte van de trainings gegevens.

## <a name="speech-translation"></a>Spraakomzetting

De **Spraakomzetting** API biedt ondersteuning voor verschillende talen voor spraak-naar-spraak- en spraak naar tekst converteren. De bron taal moet altijd afkomstig zijn uit de tabel met spraak-naar-tekst taal. De beschikbare talen voor doel, is afhankelijk van of het doel van de vertaling spraak of tekst is. U mag vertalen binnenkomende spraak in meer dan [60 talen](https://www.microsoft.com/translator/business/languages/). Een subset van deze talen zijn beschikbaar voor [spraaksynthese](language-support.md#text-languages).

### <a name="text-languages"></a>Tekst-talen

| Taal van de tekst    | Taalcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalees      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Kantonees (Traditioneel)      | `yue`          |
| Catalaans      | `ca`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
| Traditioneel Chinees      | `zh-Hant`          |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Nederlands      | `en`          |
| Estisch      | `et`          |
| Fiji      | `fj`          |
| Filipijns      | `fil`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Haïtiaans      | `ht`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Hongaars      | `hu`          |
| Indonesisch      | `id`          |
| Italiaans      | `it`          |
| Japans      | `ja`          |
| Swahili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
| Malagassische      | `mg`          |
| Maleis      | `ms`          |
| Maltees      | `mt`          |
| Noors      | `nb`          |
| Perzisch      | `fa`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Queretaro Otomi      | `otq`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
| Samoaanse      | `sm`          |
| Servisch (Cyrillisch)      | `sr-Cyrl`          |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Tahitian      | `ty`          |
| Tamil      | `ta`          |
| Thais      | `th`          |
| Tongan      | `to`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Urdu      | `ur`          |
| Vietnamees      | `vi`          |
| Welsh      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech Services ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
