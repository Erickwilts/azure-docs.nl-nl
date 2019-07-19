---
title: Diagnostische gegevens en bewaking voor Azure ServiceFabric | Microsoft Docs
description: In dit artikel worden de functies voor prestatie bewaking van de Service Fabric reliable ServiceRemoting-runtime beschreven, zoals de prestatie meter items die door de server worden gegenereerd.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: chackdan
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: chackdan
ms.openlocfilehash: 4e9aa2bbb99cac2ffc2b57ccb9299bf4ee7a729e
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876260"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnose en prestatie bewaking voor betrouw bare service voor externe communicatie
De betrouw bare ServiceRemoting-runtime verzendt [prestatie meter items](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze bieden inzicht in de manier waarop de ServiceRemoting werkt en helpt bij het oplossen van problemen en het controleren van de prestaties.


## <a name="performance-counters"></a>Prestatiemeteritems
De betrouw bare ServiceRemoting-runtime definieert de volgende categorieën voor prestatie meter items:

| Categorie | Description |
| --- | --- |
| Service Fabric-service |Tellers die specifiek zijn voor Azure Service Fabric service voor externe toegang, bijvoorbeeld de gemiddelde tijd die nodig is om de aanvraag te verwerken |
| Service Fabric service methode |Tellers die specifiek zijn voor methoden die zijn geïmplementeerd door Service Fabric externe service, bijvoorbeeld hoe vaak een service methode wordt aangeroepen |

Elk van de voor gaande categorieën heeft een of meer tellers.

De [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) -toepassing die standaard beschikbaar is in het Windows-besturings systeem, kan worden gebruikt voor het verzamelen en weer geven van gegevens van prestatie meter items. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens over prestatie meter items en het uploaden ervan naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Instantie namen van prestatie meter items
Een cluster met een groot aantal ServiceRemoting-Services of-partities beschikt over een groot aantal instanties voor prestatie meter items. De instantie namen van het prestatie meter item kunnen helpen bij het identificeren van de specifieke partitie en service methode (indien van toepassing) waaraan het prestatie meter exemplaar is gekoppeld.

#### <a name="service-fabric-service-category"></a>Categorie van Service Fabric-service
De namen van `Service Fabric Service`de tellers voor de categorie hebben de volgende indeling:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* is de teken reeks representatie van de service Fabric partitie-id waaraan het prestatie meter exemplaar is gekoppeld. De partitie-id is een GUID en de teken reeks representatie wordt gegenereerd via [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) de methode met de indelings specificatie "D".

*ServiceReplicaOrInstanceId* is de teken reeks representatie van de service Fabric replica/exemplaar-id waaraan het prestatie meter exemplaar is gekoppeld.

*ServiceRuntimeInternalID* is de teken reeks representatie van een 64-bits geheel getal dat wordt gegenereerd door de Fabric service-runtime voor intern gebruik. Dit is opgenomen in de exemplaar naam van het prestatie meter item om de uniekheid ervan te garanderen en te voor komen dat er conflicten ontstaan met andere instantie namen van prestatie meter items. Gebruikers moeten dit gedeelte van de exemplaar naam van het prestatie meter item niet interpreteren.

Hier volgt een voor beeld van de naam van een item exemplaar voor een teller die tot `Service Fabric Service` de categorie behoort:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

In het vorige voor beeld `2740af29-78aa-44bc-a20b-7e60fb783264` is de teken reeks representatie van de service Fabric partitie- `635650083799324046` id, een teken reeks representatie van replica `5008379932` /InstanceId en de 64-bits id die wordt gegenereerd voor intern gebruik door de runtime.

#### <a name="service-fabric-service-method-category"></a>Categorie Service Fabric service methode
De namen van `Service Fabric Service Method`de tellers voor de categorie hebben de volgende indeling:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* is de naam van de service methode waaraan het prestatie meter exemplaar is gekoppeld. De indeling van de methode naam wordt bepaald op basis van bepaalde logica in de runtime van de Fabric-service waarmee de Lees baarheid van de naam wordt gebalanceerd met beperkingen voor de maximum lengte van de instantie namen van prestatie meter items in Windows.

*ServiceRuntimeMethodId* is de teken reeks representatie van een 32-bits geheel getal dat wordt gegenereerd door de Fabric service-runtime voor intern gebruik. Dit is opgenomen in de exemplaar naam van het prestatie meter item om de uniekheid ervan te garanderen en te voor komen dat er conflicten ontstaan met andere instantie namen van prestatie meter items. Gebruikers moeten dit gedeelte van de exemplaar naam van het prestatie meter item niet interpreteren.

*ServiceFabricPartitionID* is de teken reeks representatie van de service Fabric partitie-id waaraan het prestatie meter exemplaar is gekoppeld. De partitie-id is een GUID en de teken reeks representatie wordt gegenereerd via [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) de methode met de indelings specificatie "D".

*ServiceReplicaOrInstanceId* is de teken reeks representatie van de service Fabric replica/exemplaar-id waaraan het prestatie meter exemplaar is gekoppeld.

*ServiceRuntimeInternalID* is de teken reeks representatie van een 64-bits geheel getal dat wordt gegenereerd door de Fabric service-runtime voor intern gebruik. Dit is opgenomen in de exemplaar naam van het prestatie meter item om de uniekheid ervan te garanderen en te voor komen dat er conflicten ontstaan met andere instantie namen van prestatie meter items. Gebruikers moeten dit gedeelte van de exemplaar naam van het prestatie meter item niet interpreteren.

Hier volgt een voor beeld van de naam van een item exemplaar voor een teller die tot `Service Fabric Service Method` de categorie behoort:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

In het voor gaande voor `ivoicemailboxservice.leavemessageasync` beeld is de naam van `2` de methode, de 32-bits id die is gegenereerd voor het interne `89383d32-e57e-4a9b-a6ad-57c6792aa521` gebruik van de runtime, is de teken reeks representatie`635650083804480486` van de service Fabric partitie-id, is de teken reeks representatie van de service Fabric replica/exemplaar-id `5008380` en is de 64-bits-id die wordt gegenereerd voor het interne gebruik van de runtime.

## <a name="list-of-performance-counters"></a>Lijst met prestatie meter items
### <a name="service-method-performance-counters"></a>Prestatie meter items service methode

De betrouw bare service-runtime publiceert de volgende prestatie meter items die betrekking hebben op de uitvoering van service methoden.

| Categorie naam | Naam van het prestatiemeteritem | Description |
| --- | --- | --- |
| Service Fabric service methode |Aanroepen per seconde |Aantal keren dat de service methode per seconde wordt aangeroepen |
| Service Fabric service methode |Gemiddeld aantal milliseconden per aanroep |Gebruikte tijd in milliseconden voor het uitvoeren van de service methode |
| Service Fabric service methode |Uitzonde ringen gegenereerd per seconde |Aantal keren dat de service methode een uitzonde ring heeft gegenereerd per seconde |

### <a name="service-request-processing-performance-counters"></a>Prestatie meter items voor verwerking van service aanvragen
Wanneer een client een methode aanroept via een Service proxy-object, resulteert dit in een aanvraag bericht dat via het netwerk naar de externe service wordt verzonden. De service verwerkt het aanvraag bericht en stuurt een antwoord terug naar de client. De betrouw bare ServiceRemoting-runtime publiceert de volgende prestatie meter items die betrekking hebben op de verwerking van service aanvragen.

| Categorie naam | Naam van het prestatiemeteritem | Description |
| --- | --- | --- |
| Service Fabric-service |aantal openstaande aanvragen |Aantal aanvragen dat wordt verwerkt in de service |
| Service Fabric-service |Gemiddeld aantal milliseconden per aanvraag |Gebruikte tijd (in milliseconden) van de service voor het verwerken van een aanvraag |
| Service Fabric-service |Gemiddeld aantal milliseconden voor het deserialiseren van aanvragen |Gebruikte tijd (in milliseconden) voor het deserialiseren van het bericht service aanvraag wanneer het wordt ontvangen bij de service |
| Service Fabric-service |Gemiddeld aantal milliseconden voor het serialiseren van antwoorden |Gebruikte tijd (in milliseconden) voor het serialiseren van het service-antwoord bericht bij de service voordat het antwoord naar de client wordt verzonden |

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeldcode](https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0)
* [Event source-providers in PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
