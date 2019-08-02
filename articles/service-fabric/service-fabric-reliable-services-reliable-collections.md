---
title: Inleiding tot betrouw bare verzamelingen in azure Service Fabric stateful-Services | Microsoft Docs
description: Service Fabric stateful services bieden betrouw bare verzamelingen waarmee u Maxi maal beschik bare Cloud toepassingen kunt schrijven die Maxi maal beschikbaar zijn.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 1/3/2019
ms.author: atsenthi
ms.openlocfilehash: a7b30003fd02f8ab2e367311cdb3f56c80dbb4b2
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599276"
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Inleiding tot betrouw bare verzamelingen in azure Service Fabric stateful Services

Met betrouw bare verzamelingen kunt u met hoge beschik bare, schaal bare en lage latentie Cloud toepassingen schrijven alsof u toepassingen met één computer schrijft. De klassen in de naam ruimte **micro soft. ServiceFabric. data. Collections** bieden een set verzamelingen die uw status automatisch Maxi maal beschikbaar maken. Ontwikkel aars moeten alleen Program meren met de betrouw bare verzamelings-Api's en kunnen beschikken over betrouw bare verzamelingen die de gerepliceerde en lokale status beheren.

Het belangrijkste verschil tussen betrouw bare verzamelingen en andere technologieën voor hoge Beschik baarheid (zoals redis, Azure Table service en Azure Queue-service) is dat de status lokaal in het service-exemplaar wordt bewaard en ook Maxi maal beschikbaar wordt gemaakt. Dit betekent dat:

* Alle Lees bewerkingen zijn lokaal, wat resulteert in Lees bewerkingen met lage latentie en hoge door voer.
* Bij alle schrijf bewerkingen wordt het minimale aantal netwerk-IOs-apparaten in rekening gebracht, wat leidt tot lage latentie en schrijf bewerkingen met hoge door voer.

![Afbeelding van de ontwikkeling van verzamelingen.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Betrouw bare verzamelingen kunnen worden beschouwd als de natuurlijke evolutie van de klassen **System. Collections** : een nieuwe set verzamelingen die zijn ontworpen voor de Cloud en toepassingen voor meerdere computers zonder de complexiteit van de ontwikkelaar te verg Roten. De betrouw bare verzamelingen zijn als volgt:

* Bijgewerkt Status wijzigingen worden gerepliceerd voor hoge Beschik baarheid.
* Persistente Gegevens worden bewaard op schijf voor duurzaamheid tegen grootschalige storingen (bijvoorbeeld een stroom storing in Data Centers).
* Omdat schrijf bewerkingen persistent zijn en worden gerepliceerd, kunt u geen vluchtige ReliableDictionary, ReliableQueue of andere betrouw bare verzameling maken die alleen gegevens in het geheugen persistent maakt.
* Asynchrone Api's zijn asynchroon om ervoor te zorgen dat threads niet worden geblokkeerd wanneer er IO wordt gemaakt.
* Transactionele Api's gebruiken de abstractie van trans acties, zodat u meerdere betrouw bare verzamelingen binnen een service eenvoudig kunt beheren.

Betrouw bare verzamelingen bieden sterke consistentie garanties van de doos om redenen in de status van toepassingen gemakkelijker te maken.
Sterke consistentie wordt bereikt door trans acties alleen te volt ooien nadat de volledige trans actie is geregistreerd op een meerderheid van de replica's, inclusief de primaire.
Om ervoor te zorgen dat de consistentie zwakkerer is, kunnen toepassingen terugsturen naar de client/aanvrager voordat de asynchrone door Voer retourneert.

De betrouw bare verzamelingen-Api's zijn een evolutie van gelijktijdige verzamelingen Api's (gevonden in de naam ruimte **System. Collections. gelijktijdige** ):

* Asynchrone Retourneert een taak omdat de bewerkingen, in tegens telling tot gelijktijdige verzamelingen, worden gerepliceerd en bewaard.
* Geen para meters: Hiermee `ConditionalValue<T>` wordt een `bool` en een waarde geretourneerd in plaats van out-para meters. `ConditionalValue<T>`is net `Nullable<T>` als geen struct vereist.
* Transacties Maakt gebruik van een transactie object om de gebruiker in staat te stellen acties op meerdere betrouw bare verzamelingen in een trans actie te groeperen.

Vandaag, **micro soft. ServiceFabric. data. Collections** bevat drie verzamelingen:

* [Betrouw bare woorden lijst](https://msdn.microsoft.com/library/azure/dn971511.aspx): Dit object vertegenwoordigt een gerepliceerde, transactionele en asynchrone verzameling sleutel-waardeparen. Net als bij **ConcurrentDictionary**kunnen zowel de sleutel als de waarde van elk type zijn.
* [Betrouw bare wachtrij](https://msdn.microsoft.com/library/azure/dn971527.aspx): Vertegenwoordigt een gerepliceerde, transactionele en asynchrone strikte first-out (FIFO)-wachtrij. Net als bij **ConcurrentQueue**kan de waarde van elk type zijn.
* [Betrouw bare gelijktijdige wachtrij](service-fabric-reliable-services-reliable-concurrent-queue.md): Hiermee wordt een gerepliceerde, transactionele en asynchrone bestel wachtrij voor Best effort voor hoge door Voer aangeduid. Net als bij de **ConcurrentQueue**kan de waarde van elk type zijn.

## <a name="next-steps"></a>Volgende stappen

* [Betrouw bare verzamelings richtlijnen & aanbevelingen](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Werken met betrouwbare verzamelingen](service-fabric-work-with-reliable-collections.md)
* [Trans acties en vergren delingen](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Gegevens beheren
  * [Back-up en herstel](service-fabric-reliable-services-backup-restore.md)
  * [Meldingen](service-fabric-reliable-services-notifications.md)
  * [Betrouwbare verzamelingserialisatie](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serialisatie en upgrade](service-fabric-application-upgrade-data-serialization.md)
  * [Configuratie van betrouw bare status Manager](service-fabric-reliable-services-configuration.md)
* Andere
  * [Reliable Services snel starten](service-fabric-reliable-services-quick-start.md)
  * [Naslag informatie voor ontwikkel aars voor betrouw bare verzamelingen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
