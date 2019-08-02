---
title: Gids voor het oplossen van problemen met Azure Security Center voor IoT Linux-beveiligings agent | Microsoft Docs
description: Problemen met het werken met Azure Security Center voor IoT-beveiligings agenten voor Linux oplossen.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2019
ms.author: mlottner
ms.openlocfilehash: 7f3bd4be3ef927f73643146a457bc551ef86a450
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600564"
---
# <a name="security-agent-troubleshoot-guide-linux"></a>Probleemoplossings gids voor beveiligings agenten (Linux)

In dit artikel wordt uitgelegd hoe u mogelijke problemen oplost in het opstart proces van de beveiligings agent.

Azure Security Center voor IoT-agent zelf wordt direct na de installatie gestart. Het opstart proces van de agent omvat het lezen van de lokale configuratie, het verbinden met Azure IoT Hub en het ophalen van de externe dubbele configuratie. Als een van deze stappen mislukt, kan dit ertoe leiden dat de beveiligings agent mislukt.

In deze hand leiding voor probleem oplossing leert u het volgende:
> [!div class="checklist"]
> * Controleren of de beveiligings agent wordt uitgevoerd
> * Fouten in de beveiligings agent ophalen
> * Fouten in de beveiligings agent begrijpen en oplossen 

## <a name="validate-if-the-security-agent-is-running"></a>Controleren of de beveiligings agent wordt uitgevoerd

1. Als u wilt valideren, wordt de beveiligings agent uitgevoerd, wacht u enkele minuten na de installatie van de agent en voert u de volgende opdracht uit. 
     <br>

    **C-agent**
    ```bash
    grep "ASC for IoT Agent initialized" /var/log/syslog
    ```
    **C#tussen**
    ```bash
    grep "Agent is initialized!" /var/log/syslog
    ```
2. Als de opdracht een lege regel retourneert, kan de beveiligings agent niet worden gestart.    

## <a name="force-stop-the-security-agent"></a>De beveiligings agent geforceerd stoppen 
In gevallen waarin de beveiligings agent niet kan worden gestart, stopt u de agent met de volgende opdracht en gaat u verder met de onderstaande fout tabel:

```bash
systemctl stop ASCIoTAgent.service
```
## <a name="get-security-agent-errors"></a>Fouten in de beveiligings agent ophalen
1. Haal de fout (en) van de beveiligings agent op door de volgende opdracht uit te voeren:
    ```bash
    grep ASCIoTAgent /var/log/syslog
    ```
2. De fout opdracht beveiligings agent ophalen haalt alle logboeken op die zijn gemaakt door de Azure Security Center voor IoT-agent. Gebruik de volgende tabel om de fouten te begrijpen en de juiste stappen te nemen voor herstel. 

> [!Note]
> Fouten logboeken worden in chronologische volg orde weer gegeven. Noteer de tijds tempel van elke fout om uw herstel te kunnen uitvoeren. 

## <a name="restart-the-agent"></a>De agent opnieuw starten

1. Nadat u een beveiligings agent fout hebt gevonden en gecorrigeerd, probeert u de agent opnieuw te starten door de volgende opdracht uit te voeren. 
    ```bash
    systemctl restart ASCIoTAgent.service
    ```
1. Herhaal het vorige proces om stoppen op te halen en de fouten op te halen als de agent het opstart proces blijft mislukken. 

## <a name="understand-security-agent-errors"></a>Beveiligings agent fouten begrijpen

De meeste fouten van de beveiligings agent worden weer gegeven in de volgende indeling: 
```
Azure Security Center for IoT agent encountered an error! Error in: {Error Code}, reason: {Error sub code}, extra details: {error specific details}
```
| Foutcode | Fout subcode | Details van fout | Herstellen C | HerstellenC# |
|:-----------|:---------------|:--------|:------------|:------------|
| Lokale configuratie | Ontbrekende configuratie | Er ontbreekt een configuratie in het lokale configuratie bestand. In het fout bericht moet worden vermeld welke sleutel ontbreekt. | Voeg de ontbrekende sleutel toe aan het/var/LocalConfiguration.json-bestand, Raadpleeg de [CS-localconfig-Reference](azure-iot-security-local-configuration-c.md) voor meer informatie.| Voeg de ontbrekende sleutel toe aan het bestand algemeen. config. Raadpleeg [c#-localconfig-Reference](azure-iot-security-local-configuration-csharp.md) voor meer informatie. |
| Lokale configuratie | Configuratie kan niet worden geparseerd | Een configuratie waarde kan niet worden geparseerd. In het fout bericht moet worden vermeld welke sleutel niet kan worden geparseerd. Een configuratie waarde kan niet worden geparseerd omdat de waarde zich niet in het verwachte type bevindt of omdat de waarde buiten het bereik valt. | Herstel de waarde van de sleutel in het bestand/var/LocalConfiguration.json zodat deze overeenkomt met het LocalConfiguration-schema, Raadpleeg de [c#-localconfig-Reference](azure-iot-security-local-configuration-csharp.md) voor meer informatie. |  Corrigeer de waarde van de sleutel in het bestand algemeen. config zodat deze overeenkomt met het schema, zie de [CS-localconfig-Reference](azure-iot-security-local-configuration-c.md) voor meer informatie.|
| Lokale configuratie | Bestandsindeling | Het parseren van het configuratie bestand is mislukt. | Het configuratie bestand is beschadigd, down load de agent en installeer deze opnieuw. | |
| Externe configuratie | Time-out | De agent kan de azureiotsecurity-module niet ophalen, dubbele binnen de time-outperiode. | Controleer of de verificatie configuratie juist is en probeer het opnieuw. | De agent kan de azureiotsecurity-module niet ophalen tijdens een time-outperiode. | Controleer of de verificatie configuratie juist is en probeer het opnieuw. |
| Authentication | Het bestand bestaat niet | Het bestand in het opgegeven pad bestaat niet. | Controleer of het bestand bestaat in het opgegeven pad of ga naar het bestand **LocalConfiguration. json** en wijzig de **filepath** -configuratie. | Controleer of het bestand bestaat in het opgegeven pad of ga naar het bestand **Authentication. config** en wijzig de **filepath** -configuratie.|
| Authentication | Bestands machtiging | De agent heeft onvoldoende machtigingen om het bestand te openen. | Geef de **asciotagent** -gebruiker lees machtigingen voor het bestand in het opgegeven pad. | Controleer of het bestand toegankelijk is. |
| Authentication | Bestandsindeling | Het opgegeven bestand heeft niet de juiste indeling. | Controleer of het bestand de juiste indeling heeft. De ondersteunde bestands typen zijn. pfx en. pem. | Controleer of het bestand een geldig certificaat bestand is. |
| Authentication | Niet geautoriseerd | De agent kan niet worden geverifieerd tegen IoT Hub met de opgegeven referenties. | Valideer de verificatie configuratie in het LocalConfiguration-bestand, ga door de verificatie configuratie en zorg ervoor dat alle details juist zijn. Controleer of het geheim in het bestand overeenkomt met de geverifieerde identiteit. | Valideer verificatie configuratie in Authentication. config, door loop de verificatie configuratie en zorg ervoor dat alle details juist zijn en controleer vervolgens of het geheim in het bestand overeenkomt met de geverifieerde identiteit.
| Authentication | Niet gevonden | Het apparaat/de module is gevonden. | Verificatie configuratie valideren: Controleer of de hostnaam juist is en of het apparaat bestaat in IoT Hub en een azureiotsecurity-dubbele module heeft. |  Verificatie configuratie valideren: Controleer of de hostnaam juist is en of het apparaat bestaat in IoT Hub en een azureiotsecurity-dubbele module heeft. |
| Authentication | Ontbrekende configuratie | Er ontbreekt een configuratie in het bestand *Authentication. config* . In het fout bericht moet worden vermeld welke sleutel ontbreekt. | Voeg de ontbrekende sleutel toe aan het bestand *LocalConfiguration. json* .| Voeg de ontbrekende sleutel toe aan het bestand *Authentication. config.* Raadpleeg [c#-localconfig-Reference](azure-iot-security-local-configuration-csharp.md) voor meer informatie. |
| Authentication | Configuratie kan niet worden geparseerd | Een configuratie waarde kan niet worden geparseerd. In het fout bericht moet worden vermeld welke sleutel niet kan worden geparseerd. Een configuratie waarde kan niet worden geparseerd omdat de waarde niet van het verwachte type is of omdat de waarde buiten het bereik valt. |Corrigeer de waarde van de sleutel in het bestand **LocalConfiguration. json** . |Herstel de waarde van de sleutel in het bestand **Authentication. config** zodat deze overeenkomt met het schema, zie de [CS-localconfig-Reference](azure-iot-security-local-configuration-c.md) voor meer informatie.|
|

## <a name="restart-the-agent"></a>De agent opnieuw starten
1. Nadat u een beveiligings agent fout hebt gevonden en opgelost, start u de agent opnieuw door de volgende opdracht uit te voeren:

    ```bash
    systemctl restart ASCIoTAgent.service
    ```
2. Herhaal indien nodig de vorige processen om de agent geforceerd te stoppen en de fouten op te halen als de agent het opstart proces blijft mislukken. 

## <a name="next-steps"></a>Volgende stappen
- Lees het [overzicht](overview.md) van de Azure Security Center voor IOT-service
- Meer informatie over Azure Security Center voor IoT- [architectuur](architecture.md)
- De Azure Security Center inschakelen voor IoT- [service](quickstart-onboard-iot-hub.md)
- Lees de [Veelgestelde vragen over](resources-frequently-asked-questions.md) de Azure Security Center voor IOT-service
- Meer informatie over het openen van onbewerkte [beveiligings gegevens](how-to-security-data-access.md)
- Meer informatie over [aanbevelingen](concept-recommendations.md)
- Beveiligings [waarschuwingen](concept-security-alerts.md) begrijpen
