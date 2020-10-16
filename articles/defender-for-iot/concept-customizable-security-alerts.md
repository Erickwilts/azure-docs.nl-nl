---
title: Aanpasbare beveiligingswaarschuwingen
description: Meer informatie over aanpas bare beveiligings waarschuwingen en aanbevolen herstel met Defender voor IoT-functies en-services.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2020
ms.author: mlottner
ms.openlocfilehash: 5a5b9182081e267f8e20cb0818202b9cb5ecd904
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90936295"
---
# <a name="defender-for-iot-customizable-security-alerts"></a>Aangepaste beveiligings waarschuwingen voor Defender voor IoT

Defender voor IoT analyseert uw IoT-oplossing voortdurend met geavanceerde analyses en bedreigings informatie om u te waarschuwen voor schadelijke activiteiten.

We raden u aan om aangepaste waarschuwingen te maken op basis van uw kennis van het verwachte gedrag van het apparaat om te zorgen dat waarschuwingen fungeren als de meest efficiënte indica toren van mogelijke inbreuk in uw unieke organisatie-implementatie en landschap.

De volgende lijst met Defender voor IoT-waarschuwingen kunt u definiëren op basis van de verwachte IoT Hub en/of het gedrag van het apparaat. Zie [aangepaste waarschuwingen maken](quickstart-create-custom-alerts.md)voor meer informatie over het aanpassen van elke waarschuwing.

## <a name="iot-hub-alerts-available-for-customization"></a>IoT Hub waarschuwingen beschikbaar voor aanpassing

| Ernst | Naam waarschuwing | Gegevensbron | Beschrijving | Voorgestelde herstel|
|---|---|---|---|---|
| Beperkt      | Aangepaste waarschuwing: aantal berichten van de Cloud naar het apparaat in het AMQP-protocol valt buiten het toegestane bereik          | IoT Hub     | Het aantal Cloud-naar-apparaat-berichten (AMQP-Protocol) binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.||
| Beperkt      | Aangepaste waarschuwing: aantal afgewezen Cloud-naar-apparaat-berichten in het AMQP-protocol valt buiten het toegestane bereik | IoT Hub     | Het aantal Cloud-naar-apparaat-berichten (AMQP-Protocol) dat door het apparaat is afgewezen, binnen een bepaald tijd venster valt buiten het huidige geconfigureerde en toegestane bereik.||
| Beperkt      | Aangepaste waarschuwing: aantal Cloud berichten in AMQP-protocol valt buiten het toegestane bereik      | IoT Hub     | De hoeveelheid apparaat naar Cloud berichten (AMQP-Protocol) binnen een bepaald tijd venster valt buiten het huidige geconfigureerde en toegestane bereik.|   |
| Beperkt      | Aangepaste waarschuwing: aantal aanroepen van directe methode valt buiten het toegestane bereik | IoT Hub     | De hoeveelheid directe methode aanroepen binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.||
| Beperkt      | Aangepaste waarschuwing: aantal uploads van bestanden valt buiten het toegestane bereik | IoT Hub     | De hoeveelheid uploads van bestanden binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.| |
| Beperkt      | Aangepaste waarschuwing: aantal berichten van de Cloud naar het apparaat in het HTTP-protocol ligt buiten het toegestane bereik | IoT Hub     | De hoeveelheid Cloud-naar-apparaat-berichten (HTTP-protocol) in een tijd venster bevindt zich niet in het geconfigureerde toegestane bereik                                  |
| Beperkt      | Aangepaste waarschuwing: aantal geweigerde Cloud-naar-apparaat-berichten in HTTP-protocol bevindt zich niet in het toegestane bereik | IoT Hub     | De hoeveelheid Cloud-naar-apparaat-berichten (HTTP-protocol) binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik. |
| Beperkt      | Aangepaste waarschuwing: aantal van het apparaat naar Cloud berichten in het HTTP-protocol valt buiten het toegestane bereik | IoT Hub| De hoeveelheid apparaat naar Cloud berichten (HTTP-protocol) binnen een bepaald tijd venster valt buiten het huidige geconfigureerde en toegestane bereik.|    |
| Beperkt      | Aangepaste waarschuwing: aantal berichten van de Cloud naar het apparaat in het MQTT-protocol valt buiten het toegestane bereik | IoT Hub     | De hoeveelheid Cloud-naar-apparaat-berichten (MQTT-Protocol) binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.|   |
| Beperkt      | Aangepaste waarschuwing: aantal afgewezen Cloud-naar-apparaat-berichten in het MQTT-protocol valt buiten het toegestane bereik | IoT Hub     | De hoeveelheid Cloud-naar-apparaat-berichten (MQTT-Protocol) die binnen een bepaald tijd venster van het apparaat worden afgewezen, ligt buiten het huidige geconfigureerde en toegestane bereik. |
| Beperkt      | Aangepaste waarschuwing: aantal Cloud berichten in MQTT-protocol valt buiten het toegestane bereik          | IoT Hub     | De hoeveelheid apparaat naar Cloud berichten (MQTT-Protocol) binnen een bepaald tijd venster valt buiten het huidige geconfigureerde en toegestane bereik.|
| Beperkt      | Aangepaste waarschuwing: het aantal opschoningen van de opdracht wachtrij valt buiten het toegestane bereik                               | IoT Hub     | De hoeveelheid leegmaak opdrachten van de opdracht wachtrij binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.||
| Beperkt      | Aangepaste waarschuwing: aantal dubbele updates van module valt buiten het toegestane bereik                                       | IoT Hub     | De hoeveelheid module dubbele updates binnen een specifiek tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.|
| Beperkt      | Aangepaste waarschuwing: aantal niet-geautoriseerde bewerkingen valt buiten het toegestane bereik  | IoT Hub     | De hoeveelheid niet-geautoriseerde bewerkingen binnen een bepaald tijd venster bevindt zich buiten het huidige geconfigureerde en toegestane bereik.|
|

## <a name="agent-alerts-available-for-customization"></a>Agent waarschuwingen beschikbaar voor aanpassing

| Ernst | Naam waarschuwing | Gegevensbron | Beschrijving | Voorgestelde herstel|
|---|---|---|---|---|
| Beperkt      | Aangepaste waarschuwing: aantal actieve verbindingen valt buiten het toegestane bereik  | Agent       | Het aantal actieve verbindingen binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik.|  De logboeken van het apparaat onderzoeken. Ontdek waar de verbinding vandaan komt en bepaal of het goed is of kwaad aardig is. Als dat schadelijk is, verwijdert u mogelijke malware en begrijpt u de bron. Voeg, indien goed aardig, de bron toe aan de lijst met toegestane verbindingen.  |
| Beperkt      | Aangepaste waarschuwing: uitgaande verbinding die is gemaakt met een IP-adres dat niet is toegestaan                             | Agent       | Er is een uitgaande verbinding gemaakt met een IP-adres buiten de lijst met toegestane IP-adressen. |De logboeken van het apparaat onderzoeken. Ontdek waar de verbinding vandaan komt en bepaal of het goed is of kwaad aardig is. Als dat schadelijk is, verwijdert u mogelijke malware en begrijpt u de bron. Voeg, indien goed aardig, de bron toe aan de lijst met toegestane IP-adressen.                        |
| Beperkt      | Aangepaste waarschuwing: aantal mislukte lokale aanmeldingen valt buiten het toegestane bereik                               | Agent       | De hoeveelheid mislukte lokale aanmeldingen binnen een bepaald tijd venster ligt buiten het huidige geconfigureerde en toegestane bereik. |   |
| Beperkt      | Aangepaste waarschuwing: aanmelding van een gebruiker die niet voor komt in de lijst met toegestane gebruikers | Agent       | Een lokale gebruiker buiten de lijst met toegestane gebruikers die is aangemeld bij het apparaat.|  Als u onbewerkte gegevens opslaat, navigeert u naar uw log Analytics-account en gebruikt u de gegevens om het apparaat te onderzoeken, identificeert u de bron en verhelpt u de lijst voor toestaan/blok keren voor deze instellingen. Als u momenteel geen onbewerkte gegevens opslaat, gaat u naar het apparaat en herstelt u de lijst met toegestane en geblokkeerde websites.|
| Beperkt      | Aangepaste waarschuwing: er is een proces uitgevoerd dat niet is toegestaan | Agent       | Een proces dat niet is toegestaan, is uitgevoerd op het apparaat. |Als u onbewerkte gegevens opslaat, navigeert u naar uw log Analytics-account en gebruikt u de gegevens om het apparaat te onderzoeken, identificeert u de bron en verhelpt u de lijst voor toestaan/blok keren voor deze instellingen. Als u momenteel geen onbewerkte gegevens opslaat, gaat u naar het apparaat en herstelt u de lijst met toegestane en geblokkeerde websites.  |
|

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [aanpassen van een waarschuwing](quickstart-create-custom-alerts.md)
- [Overzicht](overview.md) van Defender voor IOT-service
- Meer informatie over [het verkrijgen van toegang tot uw beveiligings gegevens](how-to-security-data-access.md)
- Meer informatie over [het onderzoeken van een apparaat](how-to-investigate-device.md)
