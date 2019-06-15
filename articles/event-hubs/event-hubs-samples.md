---
title: Voorbeelden - Azure Eventhubs | Microsoft Docs
description: Dit artikel bevat een lijst met voorbeelden voor Azure Event Hubs die zich op GitHub.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 1c1198733fb56303d328ee97152442d25dbe945a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60343474"
---
# <a name="git-repositories-with-samples-for-azure-event-hubs"></a>GIT-opslagplaatsen met voorbeelden voor Azure Event Hubs 
U kunt voorbeelden voor Event Hubs vinden op [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples). Deze voorbeelden demonstreren belangrijke functies in [Azure Event Hubs](/azure/event-hubs/). In dit artikel worden gecategoriseerd en een beschrijving van de voorbeelden die beschikbaar zijn, met koppelingen naar elk.

## <a name="net-samples"></a>.NET-voorbeelden

| Voorbeeldnaam | Description | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Dit voorbeeld laat zien hoe u een .NET Core-consoletoepassing die een reeks gebeurtenissen naar een event hub verzendt. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | Dit voorbeeld laat zien hoe u een .NET Core-consoletoepassing die een reeks gebeurtenissen vanaf een event hub ontvangt met behulp van de Event Processor Host-bibliotheek.  | 

## <a name="java-samples"></a>Java-voorbeelden

| Voorbeeldnaam | Description | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | In dit voorbeeld ziet hoe u voor het opnemen van batches van gebeurtenissen in uw event hub. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | In dit voorbeeld ziet hoe u voor het opnemen van gebeurtenissen in uw event hub. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | In dit voorbeeld ziet de verschillende opties die beschikbaar zijn met Event Hubs om gebeurtenissen te nemen. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub-partitie met behulp van een specifieke datum / tijd offset. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub-partitie met behulp van een offset van het specifieke gegevens. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | In dit voorbeeld laat zien hoe kunnen ontvangen van een event hub-partities met behulp van een volgnummer. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |In dit voorbeeld ziet hoe u gebeurtenissen ontvangen van een event hub met behulp van de event processor host, waarmee automatische partitie selectie en failover over meerdere gelijktijdige ontvangers. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | In dit voorbeeld ziet hoe een event hub kan automatisch opschalen op hoge belasting. Het voorbeeld verzendt gebeurtenissen met een snelheid die alleen groter zijn dan de geconfigureerde snelheid van een event hub, waardoor de event hub om omhoog te schalen. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | In dit voorbeeld kunt meten van het tarief voor inkomend verkeer. | 

## <a name="python-samples"></a>Python-voorbeelden
Vindt u voorbeelden van Python voor Azure Event Hubs in de [azure-event-hubs-python](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) GitHub-opslagplaats.

## <a name="nodejs-samples"></a>Node.js-voorbeelden
U vindt Node.js-voorbeelden voor Azure Event Hubs in de [azure sdk voor js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) GitHub-opslagplaats.

## <a name="go-samples"></a>Go-voorbeelden
U vindt Go-voorbeelden voor Azure Event Hubs in de [azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples) GitHub-opslagplaats.

## <a name="azure-cli-samples"></a>Azure CLI-voorbeelden
U kunt Azure CLI-voorbeelden zoeken voor Azure Event Hubs in de [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/CLI) GitHub-opslagplaats.

## <a name="azure-powershell-samples"></a>Azure PowerShell-voorbeelden
Vindt u voorbeelden van Azure PowerShell voor Azure Event Hubs in de [azure-event-hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples/Management/PowerShell) GitHub-opslagplaats.
 
## <a name="apache-kafka-samples"></a>Apache Kafka-voorbeelden
U kunt ook voorbeelden vinden voor de Event Hubs voor Apache Kafka-functie in de [azure-event-hubs-voor-kafka](https://github.com/Azure/azure-event-hubs-for-kafka) GitHub-opslagplaats.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs in de volgende artikelen:

- [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
- [Functies van Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)