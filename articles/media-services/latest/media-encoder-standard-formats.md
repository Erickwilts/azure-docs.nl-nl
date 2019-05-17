---
title: Standard Encoder-indelingen en codecs voor - Azure
description: In dit onderwerp biedt een overzicht van de codering-standaard-indelingen en codecs voor.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako;anilmur
ms.openlocfilehash: 730ff68e70999307417eea276761d56f4a44046a
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65520016"
---
# <a name="standard-encoder-formats-and-codecs"></a>Standard Encoder-indelingen en codecs voor

In dit artikel bevat een lijst van de meest voorkomende importeren en exporteren-bestandsindelingen die u met gebruiken kunt [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset). Voor informatie over het maken van aangepaste voorinstellingen wilt met behulp van **StandardEncoderPreset**, Zie [een transformatie te maken met een aangepaste voorinstelling](customize-encoder-presets-how-to.md).

## <a name="input-containerfile-formats"></a>Invoer/bestandsindelingen

| Bestandsindelingen (bestandsextensies) | Ondersteund |
| --- | --- |
| FLV (met H.264- en AAC-codecs) (.flv) |Ja |
| MXF    (.mxf) |Ja |
| GXF (.gxf) |Ja |
| MPEG2-PS, MPEG2-TS 3GP (TS, PS, .3gp, .3GP, mpg) |Ja |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |Ja |
| AVI (niet-gecomprimeerde 8-bits/10 bits) (.avi) |Ja |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |Ja |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ja |
| Matroska/WebM (.mkv) |Ja |
| WAVE/WAV (.wav) |Ja |
| QuickTime (.mov) |Ja |

> [!NOTE]
> 
> 

### <a name="audio-formats-in-input-containers"></a>Audio-indelingen in invoer-containers

Codering-standaard ondersteunt de volgende audio-indelingen in invoer containers uitvoeren:

* MXF-, GXF- en QuickTime-bestanden, die met interleaved stereo of 5.1-voorbeelden audiosporen

of

* MXF-, GXF- en QuickTime-bestanden waar de audio wordt uitgevoerd als afzonderlijke PCM-sporen, maar de kanaaltoewijzing (in stereo of 5.1) kunnen worden afgeleid van de metagegevens van het bestand

## <a name="input-video-codecs"></a>Codecs invoervideo
| Codecs invoervideo | Ondersteund |
| --- | --- |
| AVC 8-bits/10 bits, maximaal 4:2:2, inclusief AVCIntra |8-bits 4:2:0 en 4:2:2 |
| Avid DNxHD (in MXF) |Ja |
| DVCPro/DVCProHD (in MXF) |Ja |
| Digitale video (DV) (in AVI-bestanden) |Ja |
| JPEG 2000 |Ja |
| MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10) |Maximaal 422-profiel |
| MPEG-1 |Ja |
| VC-1/WMV9 |Ja |
| Canopus hoofdkantoor/HQX |Nee |
| MPEG-4-deel 2 |Ja |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Ja |
| YUV420 ongecomprimeerd of mezzanine |Ja |
| Apple ProRes 422 |Ja |
| Apple ProRes 422 LT |Ja |
| Apple ProRes 422 hoofdkantoor |Ja |
| Apple ProRes Proxy |Ja |
| Apple ProRes 4444 |Ja |
| Apple ProRes 4444 XQ |Ja |
| HEVC/H.265| Belangrijkste profiel|

## <a name="input-audio-codecs"></a>Codecs audio-invoer
| Codecs Audio-invoer | Ondersteund |
| --- | --- |
| AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1) |Ja |
| MPEG-laag 2 |Ja |
| MP3 (laag 3 Audio MPEG-1) |Ja |
| Windows Media Audio |Ja |
| WAV/PCM |Ja |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Ja |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Ja |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Ja |
| AMR (adaptive multi-rate) |Ja |
| AES (SMPTE 331 M en 302 M, AES3-2003) |Nee |
| Dolby® E |Nee |
| Dolby® Digital (AC3) |Nee |
| Dolby® Digital Plus (E-AC3) |Nee |

## <a name="output-formats-and-codecs"></a>Output-indelingen en codecs voor
De volgende tabel bevat de codecs en bestandsindelingen die worden ondersteund voor exporteren.

| Bestandsindeling | Video-Codec | Audio Codec |
| --- | --- | --- |
| MP4 <br/><br/>(met inbegrip van multi-bitrate MP4-containers) |H.264 (hoog, Main en basislijn profielen) |AAC-LC, hij AAC v1, v2 HE-AAC |
| MPEG2-TS |H.264 (hoog, Main en basislijn profielen) |AAC-LC, hij AAC v1, v2 HE-AAC |

## <a name="next-steps"></a>Volgende stappen

[Maken van een transformatie met een aangepaste voorinstelling](customize-encoder-presets-how-to.md)
