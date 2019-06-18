---
title: Communicatie voor rollen in Cloudservices | Microsoft Docs
description: Rolinstanties in Cloud Services kunnen eindpunten (http, https, tcp, udp) gedefinieerd voor deze die met de buitenwereld of tussen andere rolinstanties communiceren hebben.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: jeconnoc
ms.openlocfilehash: 4fa3885f9c273cf6aaf9173ebd3fee3d4499be34
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808091"
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Inschakelen van de communicatie voor rolinstanties in azure
Cloud service-rollen communiceren via de interne en externe verbindingen. Externe verbindingen worden genoemd **invoer eindpunten** tijdens interne verbindingen heten **interne eindpunten**. Dit onderwerp wordt beschreven hoe u wijzigt de [servicedefinition](cloud-services-model-and-package.md#csdef) om eindpunten te maken.

## <a name="input-endpoint"></a>Invoereindpunt
Het eindpunt van de invoer wordt gebruikt als u wilt om een poort voor de buitenwereld zichtbaar te maken. U geeft het protocoltype en de poort van het eindpunt dat wordt toegepast voor de interne en externe poorten voor het eindpunt. Als u wilt, kunt u een andere interne poort voor het eindpunt met de [localPort](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) kenmerk.

Het invoereindpunt kunt de volgende protocollen: **http, https, tcp, udp**.

Voor het maken van een invoereindpunt toevoegen de **Invoereindpunt** onderliggend element aan de **eindpunten** element van een web-of werkrol.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Invoereindpunt exemplaar
Invoereindpunten exemplaar zijn vergelijkbaar met het invoeren van eindpunten, maar kunt u bepaalde openbare poorten voor elke afzonderlijke rolinstantie toewijzen met behulp van doorsturen via poort op de load balancer. U kunt één openbare poort of een poortbereik opgeven.

De exemplaar-invoereindpunt kan alleen worden gebruikt **tcp** of **udp** als het protocol.

Voor het maken van een invoereindpunt exemplaar toevoegen de **InstanceInputEndpoint** onderliggend element aan de **eindpunten** element van een web-of werkrol.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interne eindpunt
Interne eindpunten zijn beschikbaar voor exemplaar-naar-instance-communicatie. De poort is optioneel en als u dit weglaat, een dynamische poort is toegewezen aan het eindpunt. Een poortbereik kan worden gebruikt. Er is een limiet van vijf interne eindpunten per rol.

Het interne eindpunt kunt de volgende protocollen: **http, tcp, udp, eventuele**.

Voor het maken van een intern eindpunt dat invoer toevoegen de **InternalEndpoint** onderliggend element aan de **eindpunten** element van een web-of werkrol.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

U kunt ook een poortbereik gebruiken.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8999" min="8995" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Worker-rollen vs. Webrollen
Er is een klein verschil met eindpunten bij het werken met zowel werknemers als webrollen. De Webrol moet er op een enkele invoereindpunt met de **HTTP** protocol.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Met behulp van de .NET SDK voor toegang tot een eindpunt
De beheerde bibliotheek van Azure biedt methoden voor rolinstanties om te communiceren tijdens runtime. In de code die wordt uitgevoerd binnen een rolinstantie, kunt u informatie over de aanwezigheid van andere rolinstanties en de bijbehorende eindpunten, evenals informatie over de huidige rolinstantie ophalen.

> [!NOTE]
> U kunt alleen informatie over rolexemplaren die in uw cloudservice worden uitgevoerd en die ten minste één interne eindpunt definiëren ophalen. U kunt gegevens over instanties die worden uitgevoerd in een andere service kan niet verkrijgen.
> 
> 

U kunt de [exemplaren](/previous-versions/azure/reference/ee741904(v=azure.100)) eigenschap voor het ophalen van exemplaren van een rol. Voor het eerst gebruiken de [CurrentRoleInstance](/previous-versions/azure/reference/ee741907(v=azure.100)) retourneert een verwijzing naar het huidige exemplaar en gebruik daarna de [rol](/previous-versions/azure/reference/ee741918(v=azure.100)) eigenschap als resultaat een verwijzing naar de rol zelf.

Wanneer u verbinding met een rolinstantie programmatisch via de .NET SDK maakt, is het relatief eenvoudig toegang krijgen tot informatie over het eindpunt. Nadat u al hebt verbonden met een specifieke rol-omgeving, kunt u bijvoorbeeld de poort van een bepaald eindpunt met deze code krijgen:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

De **exemplaren** eigenschap retourneert een verzameling **RoleInstance** objecten. Deze verzameling bevat altijd de huidige sessie. Als u de rol geen een intern eindpunt gedefinieerd, wordt de verzameling bevat het huidige exemplaar, maar er zijn geen andere exemplaren. Het aantal rolinstanties in de verzameling is altijd 1 in het geval waarbij er geen interne eindpunt voor de rol is gedefinieerd. Als de rol van een intern eindpunt dat is gedefinieerd, kunnen worden gedetecteerd tijdens runtime versies ervan permanent en het aantal exemplaren in de verzameling komt overeen met het aantal exemplaren dat is opgegeven voor de rol in het configuratiebestand van de service.

> [!NOTE]
> De beheerde bibliotheek van Azure biedt geen een manier voor het bepalen van de status van andere rolinstanties, maar u kunt implementeren om deze health-evaluaties zelf als uw service nodig heeft voor deze functionaliteit. U kunt [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) om informatie over het uitvoeren van de rolinstanties te verkrijgen.
> 
> 

Om te bepalen het poortnummer voor een intern eindpunt op een rolinstantie, kunt u de [InstanceEndpoints](/previous-versions/azure/reference/ee741917(v=azure.100)) eigenschap om terug te keren een Dictionary-object met namen van eindpunten en hun bijbehorende IP-adressen en poorten. De [IPEndpoint](/previous-versions/azure/reference/ee741919(v=azure.100)) eigenschap retourneert de IP-adres en poort voor een opgegeven eindpunt. De **PublicIPEndpoint** eigenschap retourneert de poort voor een eindpunt met gelijke. Het gedeelte van de IP-adres van de **PublicIPEndpoint** eigenschap wordt niet gebruikt.

Hier volgt een voorbeeld van gegevensbrontabellen, rolinstanties loopt.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Hier volgt een voorbeeld van een werkrol die de opgehaald van het eindpunt beschikbaar gemaakt via de definitie van de service en start luistert voor verbindingen.

> [!WARNING]
> Deze code werkt alleen voor een geïmplementeerde service. Bij uitvoering in de Rekenemulator van Azure, service-configuratie-elementen die directe poort eindpunten maken (**InstanceInputEndpoint** elementen), worden genegeerd.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Regels voor netwerkverkeer voor het beheren van communicatie rollen
Nadat u interne eindpunten hebt gedefinieerd, kunt u regels voor netwerkverkeer (op basis van de eindpunten die u hebt gemaakt) toevoegen om te bepalen hoe rolinstanties met elkaar kunnen communiceren. Het volgende diagram ziet u enkele algemene scenario's voor het beheren van communicatie rollen:

![Verkeer regels scenario's](./media/cloud-services-enable-communication-role-instances/scenarios.png "verkeer regels scenario's")

Het volgende codevoorbeeld toont roldefinities voor de rollen weergegeven in het vorige diagram. De roldefinitie bevat ten minste één interne eindpunt dat is gedefinieerd:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Beperking van de communicatie tussen rollen kan optreden met interne eindpunten van zowel vaste als automatisch toegewezen poorten.
> 
> 

Standaard nadat een intern eindpunt dat is gedefinieerd, kan communicatie stromen van alle rollen naar het interne eindpunt van een rol zonder beperkingen. Als u wilt beperken de communicatie, moet u toevoegen een **NetworkTrafficRules** element op de **ServiceDefinition** -element in het servicedefinitiebestand.

### <a name="scenario-1"></a>Scenario 1
Alleen toestaan netwerkverkeer van **WebRole1** naar **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Scenario 2
Kunt u alleen netwerkverkeer van **WebRole1** naar **WorkerRole1** en **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Scenario 3
Kunt u alleen netwerkverkeer van **WebRole1** naar **WorkerRole1**, en **WorkerRole1** naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Scenario 4
Kunt u alleen netwerkverkeer van **WebRole1** naar **WorkerRole1**, **WebRole1** naar **WorkerRole2**, en **WorkerRole1**  naar **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Een XML-schema-referentie voor de elementen die hierboven hebt gebruikt, kan worden gevonden [hier](/previous-versions/azure/reference/gg557551(v=azure.100)).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de Cloudservice [model](cloud-services-model-and-package.md).

