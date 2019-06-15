---
title: Metrische gegevens van Azure Relay in Azure Monitor (preview) | Microsoft Docs
description: Bewaking van Azure gebruiken voor het bewaken van Azure Relay
services: service-bus-relay
documentationcenter: .NET
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2018
ms.author: spelluru
ms.openlocfilehash: bd62624406adb006fdcd7d59f72db3fb5e1848a0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60421795"
---
# <a name="azure-relay-metrics-in-azure-monitor-preview"></a>Metrische gegevens van Azure Relay in Azure Monitor (preview)
Metrische gegevens van Azure Relay bieden u de status van resources in uw Azure-abonnement. Met een uitgebreide set metrische gegevens, kunt u de algemene status van de Relay-resources, niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze helpen u de status van Azure Relay bewaken. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure Monitor biedt een uniforme gebruikersinterfaces voor bewaking over de verschillende Azure-services. Zie voor meer informatie, [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

> [!IMPORTANT]
> In dit artikel geldt alleen voor de functie hybride verbindingen van Azure Relay, niet naar de WCF-Relay. 

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure Monitor API's (REST en .NET) en oplossingen voor gegevensanalyse, zoals operations Management Suite en Event Hubs. Zie voor meer informatie, [door gegevens te controleren die worden verzameld door Azure Monitor](../azure-monitor/platform/data-platform.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de meest recente 30 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Dit is geconfigureerd in [diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) in Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld laat zien hoe om binnenkomende aanvragen op accountniveau en geslaagde aanvragen weer te geven:

![][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteert u uw naamruimte en klik vervolgens op **metrische gegevens (Preview)** . 

Voor metrische gegevens voor ondersteuning van dimensies, moet u filteren met de gewenste dimensiewaarde.

## <a name="billing"></a>Billing

Met metrische gegevens in Azure Monitor is momenteel gratis tijdens Preview-versie. Echter, als u aanvullende oplossingen die metrische gegevens opnemen, u mogelijk worden kosten in rekening gebracht door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als u metrische gegevens om een Azure Storage-account te archiveren. U wordt ook gefactureerd door Azure Monitor-Logboeken, als u metrische gegevens naar Azure Monitor-logboeken voor geavanceerde analyse streamen.

De volgende metrische gegevens geven u een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens niet meer ondersteund als ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle metrische waarden worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle Azure Relay-metrische gegevens is 1 minuut.

## <a name="connection-metrics"></a>Metrische verbindingsgegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| ListenerConnections-Success (preview) | Het aantal geslaagde listener verbindingen met Azure Relay gedurende een bepaalde periode. <br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ListenerConnections-ClientError (preview)|Het aantal clientfouten voor listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ListenerConnections-ServerError (preview)|Het nummer van de server-fouten op de listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|SenderConnections-Success (preview)|Het aantal geslaagde afzender verbindingen die zijn gemaakt in een opgegeven periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|SenderConnections-ClientError (preview)|Het aantal clientfouten op de afzender-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|SenderConnections-ServerError (preview)|Het aantal serverfouten voor verbindingen van de afzender gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ListenerConnections-TotalRequests (preview)|Het totale aantal listener-verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|SenderConnections-TotalRequests (preview)|De verbindingsaanvragen van de afzenders gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ActiveConnections (preview)|Het aantal actieve verbindingen gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ActiveListeners (preview)|Het aantal actieve listeners gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|ListenerDisconnects (preview)|Het aantal niet-verbonden listeners gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|SenderDisconnects (preview)|Het aantal niet-verbonden afzenders gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="memory-usage-metrics"></a>Geheugen-metrische gegevens over gebruik

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|BytesTransferred (preview)|Het aantal bytes overgedragen gedurende een bepaalde periode.<br/><br/> Eenheid: Bytes <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Relay biedt ondersteuning voor de volgende dimensies voor metrische gegevens in Azure Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

|Dimensienaam|Description|
| ------------------- | ----------------- |
|EntityName| Azure Relay biedt ondersteuning voor messaging-entiteiten in de naamruimte.|

## <a name="next-steps"></a>Volgende stappen

Zie de [overzicht Azure Monitoring](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




