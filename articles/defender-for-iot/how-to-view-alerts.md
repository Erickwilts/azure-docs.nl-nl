---
title: Waarschuwingen weergeven
description: Bekijk waarschuwingen op basis van verschillende categorieën en gebruik zoek functies om u te helpen bij het vinden van belang rijke meldingen.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/02/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 4803691a82a97cd2be5fa3beafd4419010e7a9c9
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2020
ms.locfileid: "97840682"
---
# <a name="view-alerts"></a>Waarschuwingen weergeven

In dit artikel wordt beschreven hoe u waarschuwingen die door uw sensor worden geactiveerd, kunt bekijken en beheren met hulpprogram ma's voor waarschuwingen.

U kunt waarschuwingen weer geven op basis van verschillende categorieën, zoals waarschuwingen die zijn gearchiveerd of vastgemaakt. U kunt ook zoeken naar interessante meldingen, zoals waarschuwingen op basis van een IP-of MAC-adres.  

U kunt ook waarschuwingen van het sensor dashboard weer geven.

Ga als volgt te werk als u waarschuwingen wilt weergeven:

- Selecteer **waarschuwingen** in het menu aan de zijkant. Het venster **waarschuwingen** geeft de waarschuwingen weer die uw sensor heeft gedetecteerd.

  :::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-screen.png" alt-text="Weer gave van het scherm waarschuwingen.":::

## <a name="view-alerts-by-category"></a>Waarschuwingen per categorie weer geven

U kunt waarschuwingen weer geven op basis van verschillende categorieën in de hoofd weergave van **waarschuwingen** . Selecteer een waarschuwing om de details te bekijken en de gebeurtenis te beheren.

| Sorteren op type | Beschrijving |
|--|--|
| **Belang rijke waarschuwingen** | Waarschuwingen die op urgentie zijn gesorteerd. |
| **Vastgemaakte waarschuwingen** | Waarschuwingen die de gebruiker heeft vastgemaakt voor nader onderzoek. Vastgemaakte waarschuwingen worden niet gearchiveerd en worden 14 dagen opgeslagen in de vastgemaakte map. |
| **Recente waarschuwingen** | Waarschuwingen worden gesorteerd op tijd. |
| **Bevestigde waarschuwingen** | Waarschuwingen die zijn bevestigd en niet zijn verwerkt, of die zijn gedempt en niet-gedempt zijn. |
| **Gearchiveerde waarschuwingen** | Waarschuwingen die automatisch door het systeem worden gearchiveerd. Alleen de beheerder heeft toegang tot de gebruikers. |

## <a name="search-for-alerts-of-interest"></a>Zoeken naar interessante meldingen

In de hoofd weergave van **waarschuwingen** vindt u diverse zoek functies die u helpen bij het vinden van belang rijke meldingen.

:::image type="content" source="media/how-to-work-with-alerts-sensor/main-alerts-view.png" alt-text="Scherm afbeelding voor het leren van waarschuwingen.":::

### <a name="text-search"></a>Tekst zoeken 

Gebruik de **gratis Zoek** optie om waarschuwingen te zoeken op tekst, cijfers of tekens.

Zoeken:

- Typ de vereiste tekst in het veld **gratis zoek opdracht** en druk op ENTER op het toetsen bord.

De zoek opdracht wissen:

- Verwijder de tekst in het veld **gratis zoek opdracht** en druk op ENTER op het toetsen bord.

### <a name="device-group-or-device-ip-address-search"></a>IP-adres van apparaatgroep of apparaat zoeken

Zoek naar waarschuwingen die verwijzen naar specifieke IP-adressen of apparaatgroepen. Apparaatgroepen worden gemaakt in de apparaattoewijzing.

Geavanceerde filters gebruiken:

1. Selecteer **Geavanceerde filters** in de hoofd weergave van **waarschuwingen** .

2. Kies een apparaatgroep of een apparaat.

3. Selecteer **bevestigen**.

4. Als u de zoek opdracht wilt wissen, selecteert u **Alles wissen**.

### <a name="security-versus-operational-alert-search"></a>Beveiliging versus zoek actie naar operationele waarschuwingen

Scha kelen tussen het weer geven van operationele en beveiligings waarschuwingen:

- **Beveiligings** waarschuwingen vertegenwoordigen potentieel schadelijk verkeer, netwerk afwijkingen, beleids schendingen en schendingen van het protocol.

- **Operationele** waarschuwingen vertegenwoordigen netwerk afwijkingen, beleids schendingen en schendingen van het protocol.

Wanneer geen van de opties zijn geselecteerd, worden alle waarschuwingen weer gegeven.

:::image type="content" source="media/how-to-work-with-alerts-sensor/alerts-security.png" alt-text="Beveiliging in het scherm waarschuwingen.":::

## <a name="alert-window-options"></a>Opties waarschuwings venster

Waarschuwings berichten bieden de volgende acties:

- Selecteer deze optie :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-an-alert-icon.png" border="false"::: om een waarschuwing te bevestigen.

- Selecteren :::image type="icon" source="media/how-to-work-with-alerts-sensor/unacknowledge-an-alert-icon.png" border="false"::: om de bevestiging van een waarschuwing te bevestigen.

- Selecteer deze optie :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false"::: om een waarschuwing vast te maken.

- Selecteer deze optie :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false"::: om een waarschuwing te losmaken.

- Selecteer deze optie :::image type="icon" source="media/how-to-work-with-alerts-sensor/acknowledge-all-alerts-icon.png" border="false"::: om alle waarschuwingen te bevestigen.

- Selecteer deze optie :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-and-acknowledge-all-alerts.png" border="false"::: om alle waarschuwingen te leren en te bevestigen.

- Selecteer de :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-csv.png" border="false"::: lijst met waarschuwingen exporteren naar een CSV-bestand en selecteer de optie exporteren. Kies **waarschuwing exporteren** voor de reguliere optie exporteren naar CSV. Of kies **uitgebreide waarschuwing exporteren** voor de mogelijkheid om afzonderlijke rijen toe te voegen voor aanvullende informatie over een waarschuwing in het CSV-bestand.

- Selecteer het :::image type="icon" source="media/how-to-work-with-alerts-sensor/export-to-pdf.png" border="false"::: pictogram om een waarschuwings rapport te downloaden als een PDF-bestand.

- Selecteer het :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-pcap.png" border="false"::: pictogram om het pcap-bestand te downloaden. Het bestand kan worden weer gegeven met wireshark, de Free Network Protocol Analyzer.

- Selecteer :::image type="icon" source="media/how-to-work-with-alerts-sensor/download-filtered-pcap.png" border="false"::: deze optie om een gefilterd pcap-bestand te downloaden dat alleen de relevante waarschuwings pakketten bevat, waardoor de grootte van het uitvoer bestand wordt verkleind en een meer gerichte analyse mogelijk wordt. U kunt het bestand weer geven met behulp van wireshark.

- Selecteer het :::image type="icon" source="media/how-to-work-with-alerts-sensor/show-alert-in-timeline.png" border="false"::: pictogram om de waarschuwing in de tijd lijn van de gebeurtenis weer te geven.

- Selecteer het :::image type="icon" source="media/how-to-work-with-alerts-sensor/pin-an-alert-icon.png" border="false"::: pictogram om de waarschuwing vast te maken.

- Selecteer het :::image type="icon" source="media/how-to-work-with-alerts-sensor/unpin-an-alert-icon.png" border="false"::: pictogram om de waarschuwing te losmaken.

- Selecteren :::image type="icon" source="media/how-to-work-with-alerts-sensor/learn-icon.png" border="false"::: om het verkeer goed te keuren (alleen beveiligings analisten en beheerders).

- Selecteer een apparaat om dit weer te geven op de apparaattoewijzing.

## <a name="next-steps"></a>Volgende stappen

[De waarschuwings gebeurtenis beheren](how-to-manage-the-alert-event.md)

[Waarschuwings werk stromen versnellen](how-to-accelerate-alert-incident-response.md)
