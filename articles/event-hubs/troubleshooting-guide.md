---
title: Gids voor probleem oplossing-Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een lijst met uitzonde ringen en voorgestelde acties van Azure Event Hubs Messa ging.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 01/16/2020
ms.author: shvija
ms.openlocfilehash: ab3cbdf938409f4eacffa10ae5cb20f8c36b5856
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124666"
---
# <a name="azure-event-hubs---troubleshooting-guide"></a>Azure-Event Hubs-probleemoplossings gids
In dit artikel vindt u tips en aanbevelingen voor het oplossen van problemen die kunnen optreden bij het gebruik van Azure Event hubs.

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemen met connectiviteit, certificaten of time-outs
De volgende stappen kunnen u helpen bij het oplossen van problemen met connectiviteit/certificaat/time-out voor alle services onder *. servicebus.windows.net. 

- Blader naar of [wget](https://www.gnu.org/software/wget/) `https://<yournamespacename>.servicebus.windows.net/` . Het helpt te controleren of er problemen zijn met de IP-filtering of het virtuele netwerk of de certificaat keten (het meest gebruikelijk wanneer u Java SDK gebruikt).

    Een voor beeld van een geslaagd bericht:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Een voor beeld van een fout bericht:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Voer de volgende opdracht uit om te controleren of een poort is geblokkeerd op de firewall. De gebruikte poorten zijn 443 (HTTPS), 5671 (AMQP) en 9093 (Kafka). Afhankelijk van de bibliotheek die u gebruikt, worden ook andere poorten gebruikt. Hier volgt een voor beeld van de opdracht waarmee wordt gecontroleerd of de 5671-poort is geblokkeerd.

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Op Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Als er onregelmatige verbindings problemen zijn, voert u de volgende opdracht uit om te controleren of er verloren pakketten zijn. Met deze opdracht wordt geprobeerd 25 verschillende TCP-verbindingen elke 1 seconde te maken met de service. Vervolgens kunt u controleren of het aantal geslaagde/mislukte en ook TCP-verbindings latentie wordt weer geven. U kunt het `psping` hulp programma [hier](/sysinternals/downloads/psping)downloaden.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespacename>.servicebus.windows.net:5671 -nobanner     
    ```
    U kunt gelijkwaardige opdrachten gebruiken als u andere hulp middelen gebruikt, zoals `tnc` , `ping` , enzovoort. 
- Verkrijg een netwerk tracering als de vorige stappen niet helpen en analyseren met behulp van hulpprogram ma's zoals [wireshark](https://www.wireshark.org/). Neem zo nodig contact op met [Microsoft ondersteuning](https://support.microsoft.com/) . 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Problemen die zich kunnen voordoen met Service-upgrades/opnieuw opstarten
Het upgraden van de back-end-service en opnieuw opstarten kan de volgende invloed hebben op uw toepassingen:

- Aanvragen kunnen tijdelijk worden beperkt.
- Er is mogelijk een neerzetten in inkomende berichten/aanvragen.
- Het logboek bestand kan fout berichten bevatten.
- De verbinding van de toepassingen kan enkele seconden worden verbroken met de service.

Als de toepassings code gebruikmaakt van SDK, is het beleid voor opnieuw proberen al ingebouwd en actief. De toepassing zal opnieuw verbinding maken zonder belang rijke gevolgen voor de toepassing/werk stroom.


## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Overzicht van Event Hubs](event-hubs-what-is-event-hubs.md)
* [Een event hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
