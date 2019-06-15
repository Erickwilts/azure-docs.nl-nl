---
title: Waarschuwing voor problemen in Azure Cloud Services met behulp van de Azure Diagnostics-integratie met Azure Application Insights | Microsoft Docs
description: Monitor voor problemen zoals storingen bij het opstarten, crashes en rol lussen in Azure Cloud Services met Azure Application Insights recyclen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/07/2018
ms.reviewer: harelbr
ms.author: mbullwin
ms.openlocfilehash: 219ba632d7688f1a428378309828b689698d2fe5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60409500"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Waarschuwing bij problemen in Azure Cloud Services met behulp van de Azure diagnostics-integratie met Azure Application Insights

In dit artikel wordt wordt beschreven hoe u waarschuwingsregels die op problemen controleren zoals storingen bij het opstarten, crashes en rol lussen in Azure Cloud Services (web- en werkrollen rollen recyclen) instellen.

De methode die wordt beschreven in dit artikel is gebaseerd op de [Azure Diagnostics-integratie met Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/), en de onlangs uitgebrachte [Logboekwaarschuwingen voor Application Insights](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) mogelijkheid.

## <a name="define-a-base-query"></a>Een basis query definiëren

Om te beginnen, definieert we een basisquery waarmee de Windows-gebeurtenislogboek gebeurtenissen worden opgehaald uit het kanaal in Windows Azure, die zijn vastgelegd in Application Insights als trace-records.
Deze records kunnen worden gebruikt voor het detecteren van tal van problemen in Azure Cloud Services, zoals storingen bij het opstarten, runtime-fouten en lussen recyclen.

> [!NOTE]
> De onderstaande basisquery problemen in een tijdvenster van 30 minuten wordt gecontroleerd en wordt ervan uitgegaan dat een latentie van 10 minuten in de records telemetrie opnemen. Deze standaardinstellingen kunnen worden geconfigureerd wens naar.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Controleren op specifieke gebeurtenis-id 's

Bij het ophalen van de Windows-gebeurtenislogboek gebeurtenissen, kunnen de specifieke problemen worden gedetecteerd door te zoeken naar hun respectieve gebeurtenis-ID en het bericht eigenschappen zijn (Zie de onderstaande voorbeelden).
Combineer gewoon de basisquery hierboven met een van de query's hieronder, en gebruikt die gecombineerd query bij het definiëren van de waarschuwingsregel.

> [!NOTE]
> In de onderstaande voorbeelden wordt een probleem worden gedetecteerd als meer dan drie gebeurtenissen zijn gevonden tijdens de geanalyseerde-periode. Deze standaardinstelling kan worden geconfigureerd voor het wijzigen van de vertrouwelijkheid van de waarschuwingsregel.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Een waarschuwing maken

In het navigatiemenu in uw Application Insights-resource, gaat u naar **waarschuwingen**, en selecteer vervolgens **nieuwe waarschuwingsregel**.

![Schermafbeelding van Create rule](./media/proactive-cloud-services/001.png)

In de **maken regel** venster onder de **waarschuwingsvoorwaarde definiëren** sectie, klikt u op **criteria toevoegen**, en selecteer vervolgens **aangepast zoeken in logboeken**.

![Schermafbeelding van de voorwaarde criteria definiëren voor waarschuwing](./media/proactive-cloud-services/002.png)

In de **zoekquery** vak, plak de gecombineerde query die u hebt voorbereid in de vorige stap.

Ga vervolgens naar de **drempelwaarde** vak en stel de waarde op 0. U kunt eventueel aanpassen de **periode** en frequentie **velden**.
Klik op **Gereed**.

![Schermopname van het signaal logische query configureren](./media/proactive-cloud-services/003.png)

Onder de **Waarschuwingsdetails definiëren** sectie, bieden een **naam** en **beschrijving** naar de waarschuwingsregel en stel de **ernst**.
Zorg er ook voor dat de **regel inschakelen bij het maken van** knop is ingesteld op **Ja**.

![Schermafbeelding van de details van waarschuwing](./media/proactive-cloud-services/004.png)

Onder de **actiegroep definiëren** sectie, kunt u een bestaande **actiegroep** of maak een nieuwe.
U kunt kiezen om de actiegroep meerdere acties van verschillende typen bevatten.

![Schermafbeelding van actiegroep](./media/proactive-cloud-services/005.png)

Zodra u de actiegroep die u hebt gedefinieerd, Controleer uw wijzigingen en klikt u **waarschuwingsregel maken**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatisch detecteren:

[Afwijkende fouten](../../azure-monitor/app/proactive-failure-diagnostics.md)
[geheugenlekken](../../azure-monitor/app/proactive-potential-memory-leak.md)
[prestatieafwijkingen](../../azure-monitor/app/proactive-performance-diagnostics.md)

