---
title: Fouten opsporen in uw toepassing in Visual Studio | Microsoft Docs
description: De betrouwbaarheid en prestaties van uw services verbeteren door te ontwikkelen en ze in Visual Studio-foutopsporing op een lokaal ontwikkelcluster.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 682059914b5d86f5e670e373a4acf3e4ac6246ba
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428214"
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Fouten opsporen in uw Service Fabric-toepassing met behulp van Visual Studio
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Fouten opsporen in een lokale Service Fabric-toepassing
U kunt tijd en geld besparen door te implementeren en opsporen van fouten in uw Azure Service Fabric-toepassing in een cluster van de ontwikkeling van lokale computer. Visual Studio 2019 of 2015 kan de toepassing implementeren in het lokale cluster en het foutopsporingsprogramma automatisch verbinding te maken met alle exemplaren van uw toepassing. Visual Studio moet worden uitgevoerd als beheerder om het foutopsporingsprogramma verbinding te maken.

1. Een lokaal ontwikkelcluster starten met de volgende stappen in [instellen van uw Service Fabric-ontwikkelomgeving](service-fabric-get-started.md).
2. Druk op **F5** of klik op **Debug** > **Start Debugging**.
   
    ![Foutopsporing in een toepassing starten][startdebugging]
3. Onderbrekingspunten instellen in uw code en doorloop de toepassing door te klikken op de opdrachten in de **Debug** menu.
   
   > [!NOTE]
   > Visual Studio koppelt aan alle exemplaren van uw toepassing. Terwijl u de code hebt doorlopen, krijgen onderbrekingspunten door meerdere processen, wat resulteert in gelijktijdige sessies bereikt. Probeer de onderbrekingspunten uitschakelen nadat ze zijn bereikt, door middel van elke onderbrekingspunt afhankelijk van de thread-ID of met behulp van diagnostische gebeurtenissen.
   > 
   > 
4. De **diagnostische gebeurtenissen** venster wordt automatisch geopend zodat u kunt diagnostische gebeurtenissen in realtime bekijken.
   
    ![Diagnostische gebeurtenissen in realtime weergeven][diagnosticevents]
5. U kunt ook openen de **diagnostische gebeurtenissen** venster in de Cloud Explorer.  Onder **Service Fabric**, met de rechtermuisknop op een willekeurig knooppunt en kiest u **weergave Streaming traceringen**.
   
    ![Open het venster diagnostische gebeurtenissen][viewdiagnosticevents]
   
    Als u filteren van uw traces aan een bepaalde service of toepassing wilt, schakelt u streaming traceringen op die specifieke service of toepassing.
6. De diagnostische gebeurtenissen kunnen worden weergegeven in de automatisch gegenereerde **ServiceEventSource.cs** -bestand en worden aangeroepen vanuit de toepassingscode.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. De **diagnostische gebeurtenissen** venster biedt ondersteuning voor filteren, onderbreken, op te halen en gebeurtenissen in realtime.  Het filter is een eenvoudige tekenreeks zoeken in het gebeurtenisbericht, met inbegrip van de inhoud ervan.
   
    ![Filteren, onderbreken en hervatten of gebeurtenissen in realtime controleren][diagnosticeventsactions]
8. Foutopsporingsservices is vergelijkbaar met een andere toepassing voor foutopsporing. U zult normaal onderbrekingspunten via Visual Studio instellen voor eenvoudig foutopsporing. Hoewel betrouwbare verzamelingen op meerdere knooppunten repliceren, ze nog steeds als IEnumerable is geïmplementeerd. Deze implementatie betekent dat u de weergave van resultaten in Visual Studio gebruiken kunt terwijl u fouten opsporen in om te zien wat u hebt opgeslagen in. Stel een onderbrekingspunt overal in uw code om dit te doen.
   
    ![Foutopsporing in een toepassing starten][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Fouten opsporen in een externe Service Fabric-toepassing
Als uw Service Fabric-toepassingen worden uitgevoerd op een Service Fabric-cluster in Azure, kunt u deze toepassingen, rechtstreeks vanuit Visual Studio op afstand fouten opsporen.

> [!NOTE]
> De functie vereist [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2.9](https://azure.microsoft.com/downloads/).    
> 
> 

<!-- -->
> [!WARNING]
> Foutopsporing op afstand is bedoeld voor scenario's voor ontwikkelen/testen en niet moet worden gebruikt in een productieomgeving, vanwege de impact op de actieve toepassingen.
> 
> 

1. Navigeer naar het cluster in **Cloud Explorer**. Met de rechtermuisknop op en kies **opsporen van fouten inschakelen**
   
    ![Foutopsporing op afstand inschakelen][enableremotedebugging]
   
    Deze actie wordt het proces voor het inschakelen van de extensie voor externe foutopsporing op de clusterknooppunten en de vereiste netwerkconfiguraties Vliegende.
2. Met de rechtermuisknop op het clusterknooppunt in **Cloud Explorer**, en kies **foutopsporingsprogramma koppelen**
   
    ![Debugger koppelen][attachdebugger]
3. In de **koppelen aan proces** dialoogvenster, kiest u het proces dat u wilt, fouten opsporen en klikt u op **koppelen**
   
    ![Proces kiezen][chooseprocess]
   
    De naam van het proces dat u koppelen wilt aan, gelijk is aan de naam van het assembly-naam van uw service-project.
   
    Het foutopsporingsprogramma zal worden gekoppeld aan alle knooppunten waarop het proces wordt uitgevoerd.
   
   * In het geval waar u fouten in een staatloze service opspoort, zijn alle exemplaren van de service op alle knooppunten onderdeel van de sessie voor foutopsporing.
   * Als u foutopsporing op een stateful service, worden alleen de primaire replica van een partitie actief is en daarom onderschept door het foutopsporingsprogramma. Als de primaire replica wordt verplaatst tijdens de sessie voor foutopsporing, wordt de verwerking van die replica nog steeds deel uitmaken van de sessie voor foutopsporing.
   * Alleen het afvangen relevante partities of verschillende exemplaren van een bepaalde service, kunt u voorwaardelijke onderbrekingspunten verbreken alleen een specifieke partitie of het exemplaar.
     
     ![Voorwaardelijke onderbrekingspunt][conditionalbreakpoint]
     
     > [!NOTE]
     > We bieden momenteel geen ondersteuning opsporen van fouten in een Service Fabric-cluster met meerdere exemplaren van het uitvoerbare dezelfde service.
     > 
     > 
4. Wanneer u klaar bent met het opsporen van fouten in uw toepassing, kunt u de externe foutopsporing uitbreiding uitschakelen met de rechtermuisknop op het cluster in **Cloud Explorer** en kies **foutopsporing uitschakelen**
   
    ![Externe foutopsporing uitschakelen][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streaming traceringen vanuit een extern cluster-knooppunt
Ook u kunt naar stream traceringen rechtstreeks vanuit een extern cluster-knooppunt met Visual Studio. Deze functie kunt u naar stream ETW-traceringsgebeurtenissen, die worden geproduceerd op het knooppunt van een Service Fabric-cluster.

> [!NOTE]
> Deze functie moet [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) en [Azure SDK voor .NET 2.9](https://azure.microsoft.com/downloads/).
> Deze functie biedt alleen ondersteuning voor clusters die worden uitgevoerd in Azure.
> 
> 

<!-- -->
> [!WARNING]
> Streaming-traceringen is bedoeld voor scenario's voor ontwikkelen/testen en niet moet worden gebruikt in een productieomgeving, vanwege de impact op de actieve toepassingen.
> In een productiescenario, moet u zijn gebaseerd op het doorsturen van gebeurtenissen met Azure Diagnostics.
> 
> 

1. Navigeer naar het cluster in **Cloud Explorer**. Met de rechtermuisknop op en kies **Streaming traceringen inschakelen**
   
    ![Externe streaming traceringen inschakelen][enablestreamingtraces]
   
    Deze actie wordt een vliegende start het proces voor het inschakelen van de streaming traceringen-extensie op de clusterknooppunten, evenals de vereiste configuraties.
2. Vouw de **knooppunten** -element in **Cloud Explorer**, met de rechtermuisknop op het knooppunt dat u wilt streamen traceringen uit en kies **weergave Streaming traceringen**
   
    ![Externe streaming traceringen weergeven][viewremotestreamingtraces]
   
    Herhaal stap 2 voor zo veel knooppunten als u wilt zien van traceringen uit. Elke stroom knooppunten worden weergegeven in een specifieke venster.
   
    U bent nu de traceringen gegenereerd door de Service Fabric en uw services zien. Als u filteren van de gebeurtenissen om weer te geven alleen een specifieke toepassing wilt, gewoon Typ de naam van de toepassing in het filter.
   
    ![Streaming-traceringen weergeven][viewingstreamingtraces]
3. Wanneer u klaar bent streaming traceringen van het cluster, kunt u externe streaming traceringen, uitschakelen met de rechtermuisknop op het cluster in **Cloud Explorer** en kies **Streaming traceringen uitschakelen**
   
    ![Externe streaming traceringen uitschakelen][disablestreamingtraces]

## <a name="next-steps"></a>Volgende stappen
* [Een Service Fabric-service testen](service-fabric-testability-overview.md).
* [Beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
