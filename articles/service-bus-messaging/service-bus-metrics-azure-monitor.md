---
title: Metrische gegevens van Azure Service Bus in Azure Monitor | Microsoft Docs
description: Azure Monitor gebruiken voor het bewaken van Service Bus-entiteiten
services: service-bus-messaging
documentationcenter: .NET
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 11/06/2018
ms.author: aschhab
ms.openlocfilehash: 80a4b1e60202b88f6ed3c1574bd4684575a9b153
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538074"
---
# <a name="azure-service-bus-metrics-in-azure-monitor"></a>Metrische gegevens van Azure Service Bus in Azure Monitor

Service Bus metrische gegevens geven u de status van resources in uw Azure-abonnement. Met een uitgebreide set metrische gegevens, kunt u de algemene status van uw Service Bus-resources, niet alleen op het niveau van de naamruimte, maar ook op het entiteitsniveau van de beoordelen. Deze statistische gegevens is belangrijk, omdat ze u houden op de status van Service Bus. Metrische gegevens kunnen ook helpen problemen hoofdoorzaak zonder contact opnemen met ondersteuning van Azure.

Azure Monitor biedt een uniforme gebruikersinterfaces voor bewaking over de verschillende Azure-services. Zie voor meer informatie, [bewaken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) en de [ophalen van Azure Monitor metrics met .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) op GitHub.

> [!IMPORTANT]
> Wanneer er interactie met een entiteit gedurende 2 uur niet is, kan de metrische gegevens wordt gestart "0" als een waarde weergegeven tot de entiteit niet meer actief is.

## <a name="access-metrics"></a>Toegang tot metrische gegevens

Azure Monitor biedt meerdere manieren voor toegang tot metrische gegevens. U kunt de toegang tot metrische gegevens via de [Azure-portal](https://portal.azure.com), of gebruik de Azure Monitor API's (REST en .NET) en oplossingen voor gegevensanalyse, zoals Azure Monitor-logboeken en Event Hubs. Zie voor meer informatie, [metrische gegevens in Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).

Metrische gegevens zijn standaard ingeschakeld en u hebt toegang tot gegevens van de meest recente 30 dagen. Als u behouden van gegevens voor een langere periode wilt, kunt u metrische gegevens om een Azure Storage-account te archiveren. Deze waarde is geconfigureerd in [diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings) in Azure Monitor.

## <a name="access-metrics-in-the-portal"></a>Toegang tot metrische gegevens in de portal

U kunt metrische gegevens controleren na verloop van tijd in de [Azure-portal](https://portal.azure.com). Het volgende voorbeeld laat zien hoe om binnenkomende aanvragen op accountniveau en geslaagde aanvragen weer te geven:

![][1]

U kunt ook toegang tot metrische gegevens rechtstreeks via de naamruimte. Om dit te doen, selecteert u uw naamruimte en klik vervolgens op **metrische gegevens**. Als u wilt weergeven van metrische gegevens die zijn gefilterd op het bereik van de entiteit, selecteer de entiteit en klik vervolgens op **metrische gegevens**.

![][2]

Voor metrische gegevens voor ondersteuning van dimensies, moet u filteren met de gewenste dimensiewaarde.

## <a name="billing"></a>Billing

Metrische gegevens en waarschuwingen op Azure Monitor worden in rekening gebracht op basis van per waarschuwing. Deze kosten moeten beschikbaar zijn op de portal wanneer de waarschuwing ingesteld is en voordat deze wordt opgeslagen. 

Aanvullende oplossingen die opnemen van metrische gegevens worden in rekening gebracht direct door deze oplossingen. U wordt bijvoorbeeld gefactureerd door Azure Storage als u metrische gegevens om een Azure Storage-account te archiveren. U wordt ook gefactureerd door Log Analytics, als u metrische gegevens naar Log Analytics voor geavanceerde analyse streamen.

De volgende metrische gegevens geven u een overzicht van de status van uw service. 

> [!NOTE]
> Er zijn verschillende metrische gegevens niet meer ondersteund als ze worden verplaatst onder een andere naam. Dit moet u mogelijk uw referenties bijwerken. Metrische gegevens die zijn gemarkeerd met het sleutelwoord 'afgeschaft' wordt niet ondersteund voortaan.

Alle metrische waarden worden verzonden naar Azure Monitor elke minuut. De tijdgranulatie definieert het tijdsinterval waarvoor metrische waarden worden weergegeven. Het ondersteunde tijdsinterval voor alle Service Bus-metrische gegevens is 1 minuut.

## <a name="request-metrics"></a>Aanvraag voor metrische gegevens

Telt het aantal aanvragen voor beheer van gegevens en bewerkingen.

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
| Binnenkomende aanvragen| Het aantal aanvragen voor de Service Bus-service gedurende een bepaalde periode. <br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Geslaagde aanvragen|Het aantal geslaagde aanvragen voor de Service Bus-service gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Server-fouten|Het aantal aanvragen die niet worden verwerkt vanwege een fout in de Service Bus-service gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Gebruikersfouten (Zie de volgende subsectie)|Het aantal aanvragen die niet worden verwerkt wegens gebruikersfouten gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Beperkte aanvragen|Het aantal aanvragen die zijn beperkt omdat het gebruik is overschreden.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

### <a name="user-errors"></a>Gebruikersfouten

De volgende twee typen fouten zijn geclassificeerd als gebruikersfouten:

1. Client-side '-fouten (In HTTP die 400 fouten).
2. Fouten die tijdens het verwerken van berichten, zoals optreden [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception).


## <a name="message-metrics"></a>Bericht metrische gegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|Binnenkomende berichten|Het aantal gebeurtenissen of berichten die naar Service Bus wordt verzonden in een opgegeven periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
|Uitgaande berichten|Het aantal gebeurtenissen of berichten ontvangen van Service Bus gedurende een bepaalde periode.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|
| Berichten| Het aantal berichten in een wachtrij/onderwerp. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Dimensie: EntityName |
| ActiveMessages| Het aantal actieve berichten in een wachtrij/onderwerp. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/> Dimensie: EntityName |
| Dead lettered berichten| Het aantal dead lettered berichten in een wachtrij/onderwerp. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld <br/>Dimensie: EntityName |
| Geplande berichten| Het aantal geplande berichten in een wachtrij/onderwerp. <br/><br/> Eenheid: Count <br/> Aggregatietype: Gemiddeld  <br/> Dimensie: EntityName |

## <a name="connection-metrics"></a>Metrische verbindingsgegevens

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|ActiveConnections|Het aantal actieve verbindingen voor een naamruimte, maar ook op een entiteit.<br/><br/> Eenheid: Count <br/> Aggregatietype: Totaal <br/> Dimensie: EntityName|

## <a name="resource-usage-metrics"></a>Metrische gegevens voor het gebruik van resources

> [!NOTE] 
> De volgende metrische gegevens zijn alleen beschikbaar bij de **premium** laag. 

| Naam van meetwaarde | Description |
| ------------------- | ----------------- |
|CPU-gebruik per naamruimte|Het percentage CPU-gebruik van de naamruimte.<br/><br/> Eenheid: Procent <br/> Aggregatietype: Maximum <br/> Dimensie: EntityName|
|Geheugengebruik per naamruimte|Het geheugengebruik van het percentage van de naamruimte.<br/><br/> Eenheid: Procent <br/> Aggregatietype: Maximum <br/> Dimensie: EntityName|

## <a name="metrics-dimensions"></a>Metrische gegevens over dimensies

Azure Service Bus ondersteunt de volgende dimensies voor metrische gegevens in Azure Monitor. Dimensies toevoegen aan uw metrische gegevens is optioneel. Als u dimensies niet toevoegt, worden de metrische gegevens opgegeven op het niveau van de naamruimte. 

|Dimensienaam|Description|
| ------------------- | ----------------- |
|EntityName| Service Bus ondersteunt berichtentiteiten onder de naamruimte.|

## <a name="set-up-alerts-on-metrics"></a>Instellen van waarschuwingen over metrische gegevens

1. Op de **metrische gegevens** tabblad van de **Service Bus Namespace** weergeeft, schakelt **waarschuwingen configureren**. 

    ![Metrische gegevens over pagina - waarschuwingenmenu configureren](./media/service-bus-metrics-azure-monitor/metrics-page-configure-alerts-menu.png)
2. Selecteer de **doel selecteren** optie en voer de volgende acties op de **selecteert u een resource** pagina: 
    1. Selecteer **Service Bus-naamruimten** voor de **filteren op resourcetype** veld. 
    2. Selecteer uw abonnement voor de **filteren op abonnement** veld.
    3. Selecteer de **service bus-naamruimte** in de lijst. 
    4. Selecteer **Done**. 
    
        ![Naamruimte selecteren](./media/service-bus-metrics-azure-monitor/select-namespace.png)
1. Selecteer **criteria toevoegen**, en de volgende acties uitvoeren op de **signaallogica configureren** pagina:
    1. Selecteer **metrische gegevens** voor **signaal type**. 
    2. Selecteer een signaal. Bijvoorbeeld: **Service-fouten**. 

        ![Selecteer de server-fouten](./media/service-bus-metrics-azure-monitor/select-server-errors.png)
    1. Selecteer **groter is dan** voor **voorwaarde**.
    2. Selecteer **totale** voor **Tijdaggregatie**. 
    3. Voer **5** voor **drempelwaarde**. 
    4. Selecteer **Done**.    

        ![Voorwaarde opgeven](./media/service-bus-metrics-azure-monitor/specify-condition.png)    
1. Op de **maken regel** pagina uit, vouw **Waarschuwingsdetails definiëren**, en voer de volgende acties:
    1. Voer een **naam** voor de waarschuwing. 
    2. Voer een **beschrijving** voor de waarschuwing.
    3. Selecteer **ernst** voor de waarschuwing. 

        ![Meldingsdetails](./media/service-bus-metrics-azure-monitor/alert-details.png)
1. Op de **maken regel** pagina uit, vouw **actiegroep definiëren**, selecteer **nieuwe actiegroep**, en de volgende acties uitvoeren op de **pagina van de groep toevoegen actie**. 
    1. Voer een naam voor de actiegroep.
    2. Voer een korte naam voor de actiegroep. 
    3. Selecteer uw abonnement. 
    4. Selecteer een resourcegroep. 
    5. Voor dit scenario voert u **e-mailbericht verzenden** voor **ACTIENAAM**.
    6. Selecteer **e-mailadres/SMS/Push/stem** voor **ACTIETYPE**. 
    7. Selecteer **details bewerken**. 
    8. Op de **e-mailadres/SMS/Push/stem** pagina, de volgende acties uitvoeren:
        1. Selecteer **e**. 
        2. Type de **e-mailadres**. 
        3. Selecteer **OK**.

            ![Meldingsdetails](./media/service-bus-metrics-azure-monitor/add-action-group.png)
        4. Op de **actiegroep toevoegen** weergeeft, schakelt **OK**. 
1. Op de **maken regel** weergeeft, schakelt **waarschuwingsregel maken**. 

    ![Maken van waarschuwingsregel knop](./media/service-bus-metrics-azure-monitor/create-alert-rule.png)

## <a name="next-steps"></a>Volgende stappen

Zie de [overzicht van Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


