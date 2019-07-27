---
title: Beschik baarheid van scenario-spraak service
titleSuffix: Azure Cognitive Services
description: Verwijzing voor regio's van de Speech-Service.
services: cognitive-services
author: chrisbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cbasoglu
ms.openlocfilehash: 94fd415909e86a43916ee2f510732a6a6d9c5ed3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552955"
---
# <a name="scenario-availability"></a>Beschik baarheid scenario

De Speech Service SDK bevat verschillende scenario's voor een groot aantal programmeer talen en omgevingen.  Niet alle scenario's zijn momenteel beschikbaar in alle programmeer talen of in alle omgevingen.  Hieronder vindt u de beschik baarheid van elk scenario.

- **Spraak herkenning (SR), woordgroepen lijst, intentie, vertaling en on-premises containers**
  - Alle programmeer talen/omgevingen met een pijl koppeling <img src="media/index/link.jpg" height="15" width="15"></img> in de tabel Quick Start [hier](https://aka.ms/csspeech).
- **Tekst-naar-spraak (TTS)**
  - C++/Windows & Linux
  - C#/Windows & UWP & eenheid
  - TTS-REST API kunnen worden gebruikt in elke andere situatie.
- **Wake woord (Keyword Spotter/KWS)**
  - C++/Windows & Linux
  - C#/Windows & Linux
  - Python/Windows & Linux
  - Java/Windows & Linux & Android (Speech-apparaten SDK)
  - De functionaliteit van het Wake-woord (Keyword Spotter/KWS) kan worden gebruikt voor elk type microfoon, maar de ondersteuning van officiële KWS is momenteel beperkt tot de microfoon matrices die zijn gevonden in de Azure Kinect DK-hardware of de speech-apparaten SDK
- **De televoice-eerste virtuele assistent**
  - C++/Windows & Linux & macOS
  - C#/Windows
  - Java/Windows & Linux & macOS & Android (Speech apparaten SDK)
- **Gesprek transcriptie**
  - C++/Windows & Linux
  - C#(Framework & .NET core)/Windows & UWP & Linux
  - Java/Windows & Linux & Android (Speech-apparaten SDK)
- **Call Center transcriptie**
  - REST API en kunnen in elke situatie worden gebruikt
- **Door codec gecomprimeerde audio-invoer**
  - C++/Linux
  - C#/Linux
  - Java/Linux & Android
