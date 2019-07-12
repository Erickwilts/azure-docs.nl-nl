---
title: Meer informatie over Azure Security Center voor aanbevelingen voor beveiliging van IoT Preview | Microsoft Docs
description: Meer informatie over het concept van aanbevelingen voor beveiliging en hoe ze worden gebruikt in Azure Security Center voor IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 1ee71bbacdba7a14e94de41563a04be9c0f00d13
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618412"
---
# <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Security Center (ASC) voor IoT scant uw Azure-resources en IoT-apparaten en aanbevelingen voor beveiliging te verminderen van uw kwetsbaarheid voor biedt. Aanbevelingen voor beveiliging zijn bruikbare en richt u op het helpen van klanten in die voldoet aan best practices voor beveiliging.

In dit artikel vindt u een lijst met aanbevelingen die kan worden geactiveerd op uw IoT-Hub en/of IoT-apparaten.

## <a name="recommendations-for-iot-devices"></a>Aanbevelingen voor IoT-apparaten

Apparaat aanbevelingen bieden inzicht en suggesties voor het apparaat het beveiligingspostuur verbeteren. 

| Severity | Name                                                      | Gegevensbron | Description                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gemiddeld   | Poorten openen op apparaat                                      | Agent       | Een eindpunt luisteren is op het apparaat gevonden                                                                                                                                                          |
| Gemiddeld   | Strikte firewall-beleid gevonden in een van de ketens. | Agent       | Firewall-beleid gevonden (INPUT/OUTPUT) toegestaan. Firewall-beleid moet al het verkeer standaard weigeren en definieer regels voor het vereiste communicatie naar/van het apparaat toestaan.                               |
| Gemiddeld   | Strikte firewallregel in de invoer keten is gevonden     | Agent       | Een regel in de firewall is gebleken dat een ruime patroon voor een breed scala aan IP-adressen of -poorten bevat.                                                                                    |
| Gemiddeld   | Strikte firewallregel in de uitvoer-keten is gevonden    | Agent       | Een regel in de firewall is gebleken dat een ruime patroon voor een breed scala aan IP-adressen of -poorten bevat.                                                                                   |
| Gemiddeld   | Bewerking basislijn systeemvalidatie is mislukt           | Agent       | Apparaat voldoet niet aan [benchmarks van CIS-Linux](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendations-for-iot-devices"></a>Operationele aanbevelingen voor IoT-apparaten

Operationele aanbevelingen bieden inzicht en suggesties voor het verbeteren van de beveiligingsconfiguratie voor de agent.

| Severity | Name                                    | Gegevensbron | Description                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Laag      | Agent verzendt unutilized berichten          | Agent       | 10% of meer van de beveiligingsberichten zijn kleiner is dan 4kb tijdens de afgelopen 24 uur.  |
| Laag      | Dubbele beveiligingsconfiguratie niet optimaal | Agent       | Dubbele beveiligingsconfiguratie is niet optimaal.                                        |
| Laag      | Beveiliging dubbele configuratieconflict    | Agent       | Conflicten zijn gevonden in de beveiligingsconfiguratie van de apparaatdubbel.                           |


## <a name="recommendations-for-iot-hub"></a>Aanbevelingen voor IoT-Hub

Aanbeveling waarschuwingen bieden inzicht en suggesties voor bewerkingen voor het verbeteren van de beveiligingsstatus van uw omgeving.  

| Severity | Name                                                     | Gegevensbron | Description                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hoog     | Referenties voor dezelfde verificatie wordt gebruikt door meerdere apparaten | IoT Hub     | Referenties voor IoT Hub-verificatie worden gebruikt door meerdere apparaten. Dit kan duiden op een illegale apparaat door een legitieme apparaat te imiteren. Gebruik van dubbele referenties verhoogt het risico op imitatie bestaat apparaat door een kwaadwillende actor. |
| Gemiddeld   | Standaardbeleid voor IP-filter moet weigeren                  | IoT Hub     | Filter-IP-configuratie moet hebben regels gedefinieerd voor verkeer toegestaan, en standaard moet, alle andere verkeer standaard weigeren.                                                                                                     |
| Gemiddeld   | IP-filterregel bevat grote IP-adresbereik                   | IoT Hub     | Een IP-adresbereik voor toestaan IP-filter regel bron is te groot. Te ruime regels kunnen beschikbaar maken voor uw IoT-hub voor kwaadwillende actoren.                                                                                       |
| Laag      | Diagnostische logboeken in IoT Hub inschakelen                       | IoT Hub     | Logboeken inschakelen en behouden voor maximaal een jaar. Behoud van Logboeken, kunt u activiteit sporen onderzoek opnieuw maken wanneer een beveiligingsincident voordoet of uw netwerk is gecompromitteerd.                                       |
|