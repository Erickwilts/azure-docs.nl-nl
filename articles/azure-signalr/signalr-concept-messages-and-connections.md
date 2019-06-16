---
title: Berichten en verbindingen in Azure SignalR Service
description: Een overzicht van de belangrijkste concepten over berichten en verbindingen in Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: e82ce8f5c97aed7e2cb832d8e808ff84691f7c9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61401199"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Berichten en verbindingen in Azure SignalR Service

Het factureringsmodel voor Azure SignalR Service is gebaseerd op het aantal verbindingen en het aantal berichten. In dit artikel wordt uitgelegd hoe berichten en verbindingen worden gedefinieerd en geteld voor factureringsdoeleinden.


## <a name="message-formats"></a>Berichtindelingen 

Azure SignalR Service ondersteunt dezelfde indelingen als ASP.NET Core SignalR: [JSON](https://www.json.org/) en [MessagePack](/aspnet/core/signalr/messagepackhubprotocol).

## <a name="message-size"></a>Berichtgrootte

Azure SignalR Service heeft geen groottelimiet voor berichten.

Grote berichten worden opgesplitst in kleinere berichten van elke maximaal 2 kB, die als afzonderlijke berichten worden verzonden. Het splitsen en samenvoegen van berichten wordt afgehandeld via SDK's. Ontwikkelaars hoeven er niets voor te doen.

Grote berichten hebben een negatieve invloed op de prestaties van de berichtenafhandeling. Gebruik, indien mogelijk, kleinere berichten en voer tests uit om de optimale berichtgrootte voor elk gebruiksscenario te bepalen.

## <a name="how-messages-are-counted-for-billing"></a>Hoe berichten worden meegeteld voor de facturering

Voor de facturering worden alleen uitgaande berichten van Azure SignalR Service meegeteld. Ping-berichten tussen clients en servers worden genegeerd.

Een bericht dat groter is dan 2 kB, wordt beschouwd als meerdere berichten van elk 2 kB. De grafiek met het aantal berichten in de Azure-portal wordt elke 100 berichten per hub bijgewerkt.

Stel u hebt drie clients en één toepassingsserver. Vanaf één client wordt een bericht van 4 kB verzonden dat via de server moet worden uitgezonden naar alle clients. Het aantal berichten is acht: één bericht van de service naar de toepassingsserver, en drie berichten van de service naar de clients. Elk bericht wordt geteld als twee berichten van 2 kB.

Het aantal berichten dat in de Azure-portal wordt weergegeven, blijft 0 totdat het aantal boven de 100 komt.

## <a name="how-connections-are-counted"></a>Hoe verbindingen worden geteld

Er zijn serververbindingen en clientverbindingen. Standaard heeft elke toepassingsserver per hub vijf verbindingen met Azure SignalR Service en heeft elke client één clientverbinding met Azure SignalR Service.

Het aantal verbindingen dat in de Azure-portal wordt weergegeven, betreft zowel server- als clientverbindingen.

Stel dat u twee toepassingsservers hebt en dat u vijf hubs in code definieert. Het aantal serververbindingen is dan 50: 2 app-servers * 5 hubs * 5 verbindingen per hub.

ASP.NET SignalR berekent serververbindingen op een andere manier. Het bevat één standaardhub, naast de hubs die u definieert. Elke toepassingsserver moet standaard vijf extra serververbindingen hebben. Het aantal verbindingen voor de standaardhub blijft in overeenstemming met de andere hubs.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Hoe binnenkomend/uitgaand verkeer wordt geteld

Het verschil tussen binnenkomend en uitgaand verkeer is gebaseerd op het perspectief van Azure SignalR Service. Verkeer wordt geteld in bytes. Net zoals het aantal berichten, heeft het verkeer ook een samplefrequentie. De grafiek met binnenkomend/uitgaand verkeer in de Azure-portal wordt elke 100 kB per hub bijgewerkt.

## <a name="related-resources"></a>Gerelateerde resources

- [Aggregatietypen in Azure Monitor](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR-configuratie](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)