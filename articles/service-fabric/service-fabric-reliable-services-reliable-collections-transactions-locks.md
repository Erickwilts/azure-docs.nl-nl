---
title: Trans acties en vergrendelings modi in azure Service Fabric reliable-verzamelingen | Microsoft Docs
description: Azure Service Fabric betrouw bare status Manager en betrouw bare incasso transacties en vergren deling.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: atsenthi
ms.openlocfilehash: 8e77e488a3c0a40a714a0e8efffba0a2947454bf
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599327"
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Trans acties en vergrendelings modi in azure Service Fabric reliable-verzamelingen

## <a name="transaction"></a>Transactie
Een trans actie is een opeenvolging van bewerkingen die worden uitgevoerd als één logische werk eenheid.
Een trans actie moet de volgende ACID-eigenschappen hebben. kijken https://technet.microsoft.com/library/ms190612)
* **Atomiciteit**: Een trans actie moet een atoom werk eenheid zijn. Met andere woorden, alle wijzigingen in de gegevens worden uitgevoerd, of ze worden niet uitgevoerd.
* **Consistentie**: Wanneer u klaar bent, moet een trans actie alle gegevens in een consistente status laten staan. Alle interne gegevens structuren moeten aan het einde van de trans actie juist zijn.
* **Isolatie**: Wijzigingen die zijn aangebracht door gelijktijdige trans acties, moeten worden geïsoleerd van de wijzigingen die worden aangebracht door andere gelijktijdige trans acties. Het isolatie niveau dat voor een bewerking binnen een ITransaction wordt gebruikt, wordt bepaald door de IReliableState die de bewerking uitvoert.
* **Duurzaamheid**: Nadat een trans actie is voltooid, zijn de gevolgen ervan permanent aanwezig in het systeem. De wijzigingen blijven behouden, zelfs in het geval van een systeem fout.

### <a name="isolation-levels"></a>Isolatie niveaus
Het isolatie niveau bepaalt de mate waarin de trans actie moet worden geïsoleerd van wijzigingen die zijn aangebracht door andere trans acties.
Er zijn twee isolatie niveaus die in betrouw bare verzamelingen worden ondersteund:

* **Herhaal bare Lees bewerking**: Hiermee geeft u op dat instructies geen gegevens kunnen lezen die zijn gewijzigd maar nog niet zijn doorgevoerd door andere trans acties en dat geen andere trans acties gegevens kunnen wijzigen die door de huidige trans actie zijn gelezen totdat de huidige trans actie is voltooid. Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.
* **Moment opname**: Hiermee geeft u op dat de gegevens die worden gelezen door een wille keurige instructie in een trans actie, de transactionele, consistente versie van de gegevens die bestonden aan het begin van de trans actie.
  De trans actie kan alleen gegevens wijzigingen herkennen die zijn vastgelegd vóór het begin van de trans actie.
  Gegevens wijzigingen die zijn aangebracht door andere trans acties na het begin van de huidige trans actie, zijn niet zichtbaar voor instructies die in de huidige trans actie worden uitgevoerd.
  Het effect is alsof de instructies in een trans actie een moment opname van de doorgevoerde gegevens ophalen die aan het begin van de trans actie bestonden.
  Moment opnamen zijn consistent in betrouw bare verzamelingen.
  Zie [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)voor meer informatie.

Betrouw bare verzamelingen kiezen automatisch het isolatie niveau dat moet worden gebruikt voor een bepaalde Lees bewerking, afhankelijk van de bewerking en de rol van de replica op het moment dat de trans actie wordt gemaakt.
Hieronder ziet u de tabel waarin de standaard waarden voor het isolatie niveau worden weer gegeven voor betrouw bare dictionary-en wachtrij bewerkingen.

| Bewerking \ rol | Primair | Secundair |
| --- |:--- |:--- |
| Enkele entiteit gelezen |Herhaal bare Lees bewerking |Momentopname |
| Opsomming, aantal |Momentopname |Momentopname |

> [!NOTE]
> Algemene voor beelden voor bewerkingen met één `IReliableDictionary.TryGetValueAsync`entiteit `IReliableQueue.TryPeekAsync`zijn,.
> 

Zowel de betrouw bare woorden lijst als de betrouw bare wachtrij ondersteunen uw schrijf bewerkingen.
Met andere woorden, elke schrijf bewerking binnen een trans actie is zichtbaar voor een lees bewerking die tot dezelfde trans actie behoort.

## <a name="locks"></a>Vergrendelingen
In betrouw bare verzamelingen implementeren alle trans acties strikte twee fase vergrendeling: een trans actie geeft de vergrendelde vergren deling niet vrij totdat de trans actie wordt beëindigd met een afbreken of door voeren.

Een betrouw bare woorden lijst maakt gebruik van vergren deling op rijniveau voor alle bewerkingen van één entiteit.
Betrouw bare wachtrij verhoudingen voor een strikte transactionele FIFO-eigenschap.
Bij een betrouw bare wachtrij wordt het bewerkings niveau vergren delen, waarbij `EnqueueAsync` een trans actie met `TryPeekAsync` en/of `TryDequeueAsync` en één trans actie per keer wordt uitgevoerd.
Houd er rekening mee dat u FIFO kunt `TryPeekAsync` behouden `TryDequeueAsync` als er een of meer is dat de betrouw bare wachtrij leeg is `EnqueueAsync`, wordt deze ook vergrendeld.

Schrijf bewerkingen maken altijd exclusieve vergren delingen.
Voor lees bewerkingen is de vergren deling afhankelijk van een aantal factoren.
Een lees bewerking die is uitgevoerd met snap shot-isolatie, is gratis vergrendeld.
Elke Herhaal bare Lees bewerking maakt standaard gebruik van gedeelde vergren delingen.
Voor elke Lees bewerking die Herhaal bare Lees bewerkingen ondersteunt, kan de gebruiker echter om een update vergrendeling vragen in plaats van de gedeelde vergren deling.
Een update vergrendeling is een asymmetrische vergren deling die wordt gebruikt om te voor komen dat een gemeen schappelijke vorm van deadlock optreedt wanneer meerdere trans acties op een later tijdstip worden vergren delen voor mogelijke updates.

De vergrendelings compatibiliteits matrix vindt u in de volgende tabel:

| Aanvraag-\ verleend | Geen | Gedeeld | Update | Exclusive |
| --- |:--- |:--- |:--- |:--- |
| Gedeeld |Geen conflict |Geen conflict |Conflict |Conflict |
| Update |Geen conflict |Geen conflict |Conflict |Conflict |
| Exclusive |Geen conflict |Conflict |Conflict |Conflict |

Het argument time-out in de betrouw bare verzamelingen-Api's wordt gebruikt voor de deadlock-detectie.
Bijvoorbeeld, twee trans acties (T1 en T2) proberen K1 te lezen en bij te werken.
Het is mogelijk om deadlock, omdat deze beide uiteindelijk de gedeelde vergren deling hebben.
In dit geval is er een time-out opgestaan voor een of beide bewerkingen.

Dit deadlock scenario is een goed voor beeld van hoe een update vergrendeling deadlocks kan voor komen.

## <a name="next-steps"></a>Volgende stappen
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Reliable Services meldingen](service-fabric-reliable-services-notifications.md)
* [Back-up en herstel (nood herstel) Reliable Services](service-fabric-reliable-services-backup-restore.md)
* [Configuratie van betrouw bare status Manager](service-fabric-reliable-services-configuration.md)
* [Naslag informatie voor ontwikkel aars voor betrouw bare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

