---
title: Resource gebeurtenissen in Azure App Service ophalen
description: Meer informatie over het ophalen van bron gebeurtenissen via activiteiten logboeken en Event Grid op uw App Service-app.
ms.topic: article
ms.date: 04/24/2020
ms.author: msangapu
ms.openlocfilehash: c5c84891187c540c0b24162cf5c8c7f96e9e731a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88962295"
---
# <a name="get-resource-events-in-azure-app-service"></a>Resource gebeurtenissen in Azure App Service ophalen

Azure App Service biedt ingebouwde hulpprogram ma's voor het bewaken van de status en status van uw resources. Resource gebeurtenissen helpt u bij het begrijpen van alle wijzigingen die zijn aangebracht in uw onderliggende web-app-resources en om indien nodig actie te ondernemen. Voor beelden van gebeurtenissen zijn: schalen van instanties, updates van toepassings instellingen, opnieuw starten van de web-app en nog veel meer. In dit artikel leert u hoe u [Azure-activiteiten logboeken](../azure-monitor/platform/activity-log.md#view-the-activity-log) kunt weer geven en [Event grid](../event-grid/index.yml) om bron gebeurtenissen te bewaken die betrekking hebben op uw app service Web-app.

> [!NOTE]
> App Service integratie met Event Grid is in **Preview**. [Bekijk de aankondiging voor meer informatie.](https://aka.ms/app-service-event-grid-announcement)
>

## <a name="view-azure-activity-logs"></a>Activiteiten logboeken van Azure weer geven
Activiteiten logboeken van Azure bevatten bron gebeurtenissen die worden verzonden door bewerkingen die zijn uitgevoerd op de resources in uw abonnement. Zowel de gebruikers acties in de Azure Portal als Azure Resource Manager sjablonen dragen bij aan de gebeurtenissen die worden vastgelegd door het activiteiten logboek. 

Azure-activiteiten logboeken voor App Service Details, zoals:
- welke bewerkingen zijn uitgevoerd voor de resources (bijvoorbeeld: App Service-abonnementen)
- wie de bewerking heeft gestart
- Wanneer de bewerking is uitgevoerd
- de status van de bewerking
- eigenschaps waarden die u helpen bij het onderzoeken van de bewerking

### <a name="what-can-you-do-with-azure-activity-logs"></a>Wat kunt u doen met Azure-activiteiten logboeken?

De activiteiten logboeken van Azure kunnen worden opgevraagd met behulp van de Azure Portal, Power shell, REST API of CLI. U kunt de logboeken verzenden naar een opslag account, Event hub en Log Analytics. U kunt ze ook analyseren in Power BI of waarschuwingen maken om de resource gebeurtenissen bijgewerkt te houden.

[Activiteiten logboek gebeurtenissen van Azure weer geven en ophalen.](../azure-monitor/platform/activity-log.md#view-the-activity-log)

## <a name="ship-activity-logs-to-event-grid"></a>Activiteiten logboeken verzenden naar Event Grid

Hoewel activiteiten logboeken op basis van een gebruiker zijn gebaseerd, is er een nieuwe [Event grid](../event-grid/index.yml) integratie met app service (preview) waarmee zowel gebruikers acties als geautomatiseerde gebeurtenissen worden geregistreerd. Met Event Grid kunt u een handler configureren om te reageren op de genoemde gebeurtenissen. Gebruik Event Grid bijvoorbeeld om direct een serverloze functie te triggeren voor het uitvoeren van beeldanalyse zodra er een nieuwe foto wordt toegevoegd aan de container voor blob-opslag.

U kunt Event Grid ook gebruiken met Logic Apps om op elke locatie gegevens te verwerken, zonder dat u hiervoor code hoeft te schrijven. Event Grid verbindt gegevensbronnen en gebeurtenis-handlers. Gebruik Event Grid bijvoorbeeld om direct een serverloze functie te triggeren voor het uitvoeren van beeldanalyse zodra er een nieuwe foto wordt toegevoegd aan de container voor blob-opslag.

[De eigenschappen en het schema voor Azure App Service gebeurtenissen weer geven.](../event-grid/event-schema-app-service.md)

## <a name="next-steps"></a><a name="nextsteps"></a> Volgende stappen
* [Logboeken doorzoeken met Azure Monitor](../azure-monitor/log-query/log-query-overview.md)
* [Azure App Service bewaken](web-sites-monitor.md)
* [Problemen met Azure App Service oplossen in Visual Studio](troubleshoot-dotnet-visual-studio.md)
* [App-logboeken in HDInsight analyseren](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)