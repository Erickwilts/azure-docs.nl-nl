---
title: Polymorfisme in het Reliable Actors Framework
description: Bouw hiërarchieën van .NET-interfaces en-typen in het Reliable Actors Framework om functionaliteit en API-definities opnieuw te gebruiken.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.custom: devx-track-csharp
ms.openlocfilehash: 7552609fc4c85cdd5033e67b1bab7fb58cb6ebf1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89016560"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfisme in het Reliable Actors Framework
Met het Reliable Actors-Framework kunt u actors bouwen met veel van de technieken die u zou gebruiken in een object georiënteerd ontwerp. Een van deze technieken is polymorfisme, waarmee typen en interfaces van meer algemene ouders kunnen worden overgenomen. Overname in het Reliable Actors Framework volgt in het algemeen het .NET-model met enkele extra beperkingen. In het geval van Java/Linux wordt het Java-model gevolgd.

## <a name="interfaces"></a>Interfaces
Het Reliable Actors-Framework vereist dat u ten minste één interface definieert die moet worden geïmplementeerd door het type actor. Deze interface wordt gebruikt voor het genereren van een proxy klasse die door clients kan worden gebruikt om te communiceren met uw actors. Interfaces kunnen van andere interfaces worden overgenomen, zolang elke interface die wordt geïmplementeerd door een actor-type en alle bovenliggende elementen uiteindelijk worden afgeleid van IActor (C#) of actor (Java). IActor (C#) en Actor (Java) zijn de door het platform gedefinieerde basis interfaces voor actors in respectievelijk de frameworks .NET en Java. Zo kan het klassieke polymorfisme-voor beeld met behulp van vormen er ongeveer als volgt uitzien:

![Interface hiërarchie voor actors van vorm][shapes-interface-hierarchy]

## <a name="types"></a>Typen
U kunt ook een hiërarchie van actor types maken die zijn afgeleid van de basis actor klasse die door het platform wordt verschaft. In het geval van vormen is het mogelijk dat u een basis `Shape` -(C#) of `ShapeImpl` (Java) type hebt:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

Subtypen van `Shape` (C#) of `ShapeImpl` (Java) kunnen methoden van de basis onderdrukken.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Noteer het `ActorService` kenmerk van het type actor. Dit kenmerk vertelt het reliable actor Framework dat automatisch een service moet maken voor het hosten van actors van dit type. In sommige gevallen wilt u mogelijk een basis type maken dat alleen is bedoeld voor het delen van functionaliteit met subtypen en nooit wordt gebruikt voor het instantiëren van beton actors. In die gevallen moet u het `abstract` sleutel woord gebruiken om aan te geven dat u nooit een actor wilt maken op basis van dat type.

## <a name="next-steps"></a>Volgende stappen
* Bekijk [hoe het reliable actors-Framework gebruikmaakt van het service Fabric platform](service-fabric-reliable-actors-platform.md) om betrouw baarheid, schaal baarheid en consistente status te bieden.
* Meer informatie over de [actor-levens cyclus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
