---
title: Bewaking configureren in azure Digital Apparaatdubbels | Microsoft Docs
description: Bewaking in azure Digital Apparaatdubbels configureren.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 66a0b98e2cae14c3708cf8a6aa3f8bd498f41c9f
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68609633"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Bewaking in azure Digital Apparaatdubbels configureren

Azure Digital Apparaatdubbels ondersteunt robuuste logboek registratie, bewaking en analyses. Ontwikkel aars van oplossingen kunnen Azure Monitor logboeken, Diagnostische logboeken, activiteiten registreren en andere services gebruiken om de complexe bewakings behoeften van een IoT-app te ondersteunen. Opties voor logboek registratie kunnen worden gecombineerd om records op te vragen of weer te geven in verschillende services en om een gedetailleerde logboek registratie te bieden voor veel services.

Dit artikel bevat een overzicht van de opties voor logboek registratie en controle en hoe u deze kunt combi neren op manieren die specifiek zijn voor Azure Digital Apparaatdubbels.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Activiteiten logboeken controleren

Azure- [activiteiten logboeken](../azure-monitor/platform/activity-logs-overview.md) bieden snelle inzichten in gebeurtenis-en bewerkings geschiedenis voor elk Azure-service-exemplaar.

Gebeurtenissen op abonnements niveau zijn onder andere:

* Resources maken
* Resources verwijderen
* App-geheimen maken
* Integreren met andere services

Activiteiten logboek registratie voor Azure Digital Apparaatdubbels is standaard ingeschakeld en kan worden gevonden via de Azure Portal:

1. Uw Azure Digital Apparaatdubbels-exemplaar selecteren.
1. **Activiteiten logboek** kiezen om het weergave paneel weer te geven:

    [![Activiteiten logboek](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Voor geavanceerde activiteiten logboek registratie:

1. Selecteer de optie Logboeken om het **analyse van activiteitenlogboek overzicht**weer te geven:

    [![Item](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. In het overzicht van de **analyse van activiteitenlogboek** vindt u een overzicht van essentiële logboek gegevens voor activiteiten:

    [![Overzicht van activiteiten logboek analyse]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

>[!TIP]
>Gebruik **activiteiten logboeken** voor snelle inzichten in gebeurtenissen op abonnements niveau.

## <a name="enable-customer-diagnostic-logs"></a>Diagnostische logboeken van klanten inschakelen

De [Diagnostische instellingen](../azure-monitor/platform/diagnostic-logs-overview.md) van Azure kunnen voor elke Azure-instantie worden ingesteld op een aanvullende activiteiten logboek registratie. Hoewel activiteiten logboeken betrekking hebben op gebeurtenissen op abonnements niveau, biedt diagnostische logboek registratie inzicht in de operationele geschiedenis van de resources zelf.

Voor beelden van diagnostische logboek registratie zijn:

* De uitvoerings tijd voor een door de gebruiker gedefinieerde functie
* De antwoord status code van een geslaagde API-aanvraag
* Een app-sleutel ophalen uit een kluis

Diagnostische logboeken voor een exemplaar inschakelen:

1. Breng de resource in Azure Portal.
1. **Diagnostische instellingen**selecteren:

    [![Diagnostische instellingen één](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Selecteer **diagnostiek inschakelen** om gegevens te verzamelen (indien niet eerder ingeschakeld).
1. Vul de vereiste velden in en Selecteer hoe en waar de gegevens worden opgeslagen:

    [![Diagnostische instellingen twee](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Diagnostische logboeken worden vaak opgeslagen met [Azure File Storage](../storage/files/storage-files-deployment-guide.md) en worden gedeeld met [Azure monitor](../azure-monitor/log-query/get-started-portal.md)-Logboeken. U kunt beide opties selecteren.

>[!TIP]
>Gebruik **Diagnostische logboeken** voor inzichten in resource bewerkingen.

## <a name="azure-monitor-and-log-analytics"></a>Azure monitor en log Analytics

IoT-toepassingen kunnen verschillende resources, apparaten, locaties en gegevens samen voegen tot één. Met nauw keurige logboek registratie wordt gedetailleerde informatie gegeven over elk specifiek onderdeel, elke service of elk onderdeel van de algemene toepassings architectuur, maar een uniform overzicht is vaak vereist voor onderhoud en fout opsporing.

Azure Monitor bevat de krachtige log Analytics-service, waarmee logboek registratie bronnen op één locatie kunnen worden weer gegeven en geanalyseerd. Azure Monitor is daarom zeer nuttig voor het analyseren van Logboeken binnen geavanceerde IoT-apps.

Voor beelden van gebruik zijn:

* Query's uitvoeren op meerdere Diagnostische logboeken
* Logboeken bekijken voor verschillende door de gebruiker gedefinieerde functies
* Logboeken weer geven voor twee of meer services binnen een bepaald tijds bestek

Volledige logboek query's worden via Azure Monitor- [Logboeken](../azure-monitor/log-query/log-query-overview.md)verschaft. U kunt deze krachtige functies als volgt instellen:

1. Zoek naar **log Analytics** in het Azure Portal.
1. Uw beschik bare **log Analytics werkruimte** -instanties worden weer geven. Kies één en selecteer de logboeken die u wilt doorzoeken:

    [![Log Analytics](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Als u nog geen exemplaar van **log Analytics-werk ruimte** hebt, kunt u een werk ruimte maken door de knop **toevoegen** te selecteren:

    [![OMS maken](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Zodra uw **log Analytics werkruimte** -exemplaar is ingericht, kunt u krachtige query's gebruiken om vermeldingen in meerdere logboeken te vinden of om te zoeken met specifieke criteria met behulp van **logboek beheer**:

   [![Logboek beheer](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Zie aan de slag [met query's](../azure-monitor/log-query/get-started-queries.md)voor meer informatie over krachtige query bewerkingen.

> [!NOTE]
> Er kan een vertraging van 5 minuten optreden bij het verzenden van gebeurtenissen naar **log Analytics werk ruimte** voor de eerste keer.

Azure Monitor logboeken bieden ook krachtige services voor fout meldingen en waarschuwingen, die kunnen worden weer gegeven door het selecteren van **problemen vaststellen en oplossen**:

   [![Waarschuwings-en fout meldingen](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Gebruik **log Analytics werk ruimte** om de logboek geschiedenis te doorzoeken op meerdere app-functionaliteiten, abonnementen of services.

## <a name="other-options"></a>Andere opties

Azure Digital Apparaatdubbels biedt ook ondersteuning voor toepassingsspecifieke logboek registratie en beveiligings controles. Zie het artikel over [Azure-logboek controle](../security/fundamentals/log-audit.md) voor een uitgebreid overzicht van alle opties voor Azure-logboek registratie die beschikbaar zijn voor uw Azure Digital apparaatdubbels-exemplaar.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Azure- [activiteiten logboeken](../azure-monitor/platform/activity-logs-overview.md).

- Bedieping van de diagnostische instellingen van Azure door een [overzicht van Diagnostische logboeken](../azure-monitor/platform/diagnostic-logs-overview.md)te lezen.

- Lees meer over [Azure monitor](../azure-monitor/log-query/get-started-portal.md)-Logboeken.
