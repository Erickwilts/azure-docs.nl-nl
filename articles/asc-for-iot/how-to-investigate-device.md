---
title: Azure Security Center voor onderzoek voor Preview-versie voor IoT-apparaat | Microsoft Docs
description: Deze stapsgewijze handleiding wordt uitgelegd hoe u Azure Security Center voor IoT gebruiken voor het onderzoeken van een verdachte IoT-apparaat met de Log Analytics.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: b18b48ae-b445-48f8-9ac0-365d6e065b64
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/18/2019
ms.author: mlottner
ms.openlocfilehash: 15e65c155a98ae12c156587735d34a16ed2c9109
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65192662"
---
# <a name="investigate-a-suspicious-iot-device"></a>Onderzoeken van een verdachte IoT-apparaat

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Security Center (ASC) voor IoT-servicewaarschuwingen en bewijs bieden duidelijk aangegeven wanneer de IoT-apparaten wordt vermoed dat ze van de betrokkenheid bij verdachte activiteiten of wanneer vermeldingen bestaat dat een apparaat is geknoeid. 

Gebruik onderzoek suggesties om te bepalen van de mogelijke risico's voor uw organisatie, bepalen hoe u wilt herstellen, en de beste manieren zijn om in de toekomst te voorkomen dat dergelijke aanvallen te detecteren die in deze handleiding.  

> [!div class="checklist"]
> * De apparaatgegevens van uw zoeken
> * Met behulp van query's kql onderzoeken


## <a name="how-can-i-access-my-data"></a>Hoe krijg ik toegang tot mijn gegevens?

Standaard slaat ASC voor IoT uw beveiligingswaarschuwingen en aanbevelingen in uw Log Analytics-werkruimte. U kunt ook kiezen voor het opslaan van uw onbewerkte beveiligingsgegevens.

Om te zoeken de werkruimte voor logboekanalyse voor gegevensopslag:

1. Open uw IoT-hub. 
1. Onder **Security**, klikt u op **overzicht**, en selecteer vervolgens **instellingen**.
1. Wijzig de configuratiedetails van uw Log Analytics-werkruimte. 
1. Klik op **Opslaan**. 

De volgende configuratie, doe het volgende voor toegang tot gegevens die zijn opgeslagen in uw Log Analytics-werkruimte:

1. Selecteer en klik op een ASC voor IoT-waarschuwing in uw IoT-Hub. 
1. Klik op **nader onderzoek**. 
1. Selecteer **om te zien welke apparaten deze waarschuwing klikt u hier en weergeven van de apparaat-id-kolom**.

## <a name="investigation-steps-for-suspicious-iot-devices"></a>Onderzoeksvolgorde voor verdachte IoT-apparaten

Voor toegang tot inzichten en onbewerkte gegevens over uw IoT-apparaten, gaat u naar uw Log Analytics-werkruimte [toegang tot uw gegevens](#how-can-i-access-my-data).

Controleren en onderzoeken van de apparaatgegevens voor de volgende details en de activiteiten met behulp van de volgende kql-query's.

### <a name="related-alerts"></a>Gerelateerde waarschuwingen

Als u wilt weten of andere waarschuwingen zijn geactiveerd rond dezelfde tijd gebruikt de volgende kql-query:

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityAlert
  | where ExtendedProperties contains device and ResourceId contains tolower(hub)
  | project TimeGenerated, AlertName, AlertSeverity, Description, ExtendedProperties
  ~~~

### <a name="users-with-access"></a>Gebruikers met toegang

Als u wilt weten hebben welke gebruikers toegang tot dit apparaat gebruiken met de volgende kql-query: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "LocalUsers"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     GroupNames=extractjson("$.GroupNames", EventDetails, typeof(string)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string))
  | summarize FirstObserved=min(TimestampLocal) by GroupNames, UserName
  ~~~
Deze gegevens gebruiken om te detecteren: 
  1. Welke gebruikers hebben toegang tot het apparaat?
  2. Hebben de gebruikers met toegang tot de machtigingsniveaus zoals verwacht? 

### <a name="open-ports"></a>Poorten openen

Als u wilt weten welke poorten in het apparaat zijn momenteel in gebruik of zijn gebruikt, gebruikt u de volgende kql-query: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ListeningPorts"
     and extractjson("$.LocalPort", EventDetails, typeof(int)) <= 1024 // avoid short-lived TCP ports (Ephemeral)
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Protocol=extractjson("$.Protocol", EventDetails, typeof(string)),
     LocalAddress=extractjson("$.LocalAddress", EventDetails, typeof(string)),
     LocalPort=extractjson("$.LocalPort", EventDetails, typeof(int)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     RemotePort=extractjson("$.RemotePort", EventDetails, typeof(string))
  | summarize MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), AllowedRemoteIPAddress=makeset(RemoteAddress), AllowedRemotePort=makeset(RemotePort) by Protocol, LocalPort
  ~~~

    Use this data to discover:
  1. Welke luisterende sockets zijn momenteel actief is op het apparaat?
  2. De luisterende sockets die momenteel actief zijn toegestaan?
  3. Zijn er een verdachte remote-adressen die zijn verbonden met het apparaat?

### <a name="user-logins"></a>Gebruikersaanmeldingen

Als u wilt weten gebruik gebruikers dat aangemeld bij het apparaat de volgende kql-query: 
 
  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "Login"
     // filter out local, invalid and failed logins
     and EventDetails contains "RemoteAddress"
     and EventDetails !contains '"RemoteAddress":"127.0.0.1"'
     and EventDetails !contains '"UserName":"(invalid user)"'
     and EventDetails !contains '"UserName":"(unknown user)"'
     //and EventDetails !contains '"Result":"Fail"'
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string)),
     LoginHandler=extractjson("$.Executable", EventDetails, typeof(string)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     Result=extractjson("$.Result", EventDetails, typeof(string))
  | summarize CntLoginAttempts=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), CntIPAddress=dcount(RemoteAddress), IPAddress=makeset(RemoteAddress) by UserName, Result, LoginHandler
  ~~~

    Use the query results to discover:
  1. Welke gebruikers aangemeld op het apparaat?
  2. Zijn de gebruikers die aangemeld, moet zich aanmelden?
  3. De gebruikers die vastgelegd in verbinding maken vanaf de verwachte of onverwachte IP-adressen?
  
### <a name="process-list"></a>Proceslijst

Als u wilt weten als de lijst is zoals verwacht, gebruikt u de volgende kql-query: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ProcessCreate"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Executable=extractjson("$.Executable", EventDetails, typeof(string)),
     UserId=extractjson("$.UserId", EventDetails, typeof(string)),
     CommandLine=extractjson("$.CommandLine", EventDetails, typeof(string))
  | join kind=leftouter (
     // user UserId details
     SecurityIoTRawEvent
     | where
        DeviceId == device and AssociatedResourceId contains tolower(hub)
        and RawEventName == "LocalUsers"
     | project
        UserId=extractjson("$.UserId", EventDetails, typeof(string)),
        UserName=extractjson("$.UserName", EventDetails, typeof(string))
     | distinct UserId, UserName
  ) on UserId
  | extend UserIdName = strcat("Id:", UserId, ", Name:", UserName)
  | summarize CntExecutions=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), ExecutingUsers=makeset(UserIdName), ExecutionCommandLines=makeset(CommandLine) by Executable
  ~~~

    Use the query results to discover:

  1. Zijn er een verdachte processen worden uitgevoerd op het apparaat?
  2. Zijn er processen uitgevoerd door de juiste gebruikers?
  3. Bevat alle uitvoeringen vanaf de opdrachtregel de juiste en de verwachte argumenten?

## <a name="next-steps"></a>Volgende stappen

Neem na onderzoek van een apparaat en krijgen een beter begrip van de risico's, kunt u overwegen [configureren van aangepaste waarschuwingen](quickstart-create-custom-alerts.md) voor het verbeteren van de beveiligingsstatus van uw IoT-oplossing. Als u nog een agent apparaat hebt, kunt u overwegen [implementeren van een beveiligingsagent](how-to-deploy-agent.md) of [wijzigen van de configuratie van een bestaand apparaat agent](how-to-agent-configuration.md) voor het verbeteren van de resultaten. 