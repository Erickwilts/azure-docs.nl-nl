---
title: 'Status van Azure Monitor v2 API-verwijzing: Instrumentatie-engine inschakelen | Microsoft Docs'
description: Status Monitor v2 InstrumentationEngine-API-verwijzing inschakelen. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: e993378634262de25449975431c0a9e3145ca9fb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255251"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>Status Monitor v2 API: Enable-InstrumentationEngine (v0.2.1-alfa)

Dit document beschrijft een cmdlet die wordt geleverd als een lid van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Description

Deze cmdlet wordt de Engine Instrumentation inschakelen door in te stellen bepaalde registersleutels.
Start IIS opnieuw om deze wijzigingen te laten treden.

De Engine instrumentatie kunt vormen een aanvulling op gegevens die zijn verzameld door de .NET-SDK's.
Gebeurtenissen en -berichten worden verzameld die de uitvoering van een beheerd proces wordt beschreven. Inclusief maar niet beperkt tot de resultaatcodes afhankelijkheid, HTTP-termen en SQL-opdrachttekst. 

De Engine Instrumentation inschakelen als:
- U hebt al ingeschakeld voor bewaking met de cmdlet Enable maar de InstrumentationEngine niet hebt ingeschakeld.
- U handmatig uw toepassing met de .NET-SDK's hebt geïnstrumenteerd en wilt u meer telemetrie verzamelen.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

> [!NOTE] 
> Deze cmdlet, moet u lees en accepteer de licentie- en privacy-instructie.

> [!NOTE] 
> De Engine Instrumentation voegt extra overhead en is standaard uitgeschakeld.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parameters 

### <a name="-acceptlicense"></a>-AcceptLicense
**Optioneel.** Gebruik deze switch te accepteren van de licentie- en privacy-instructie in ' headless '-installaties.

### <a name="-verbose"></a>-Verbose
**Common Parameter.** Gebruik deze switch naar gedetailleerde logboeken uitvoer.

## <a name="output"></a>Uitvoer


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Van voorbeelduitvoer van de instrumentatie-engine is ingeschakeld

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Volgende stappen

  Uw telemetrie weergeven:
 - [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) om de prestaties en het gebruik te bewaken
- [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren
- [Gebruik analyses](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's
- [Maak dashboards](../../azure-monitor/app/overview-dashboard.md)
 
 Meer telemetrie toevoegen:
 - [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
- [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) zien welke uitzonderingen webpaginacode en traceringsaanroepen in te voegen.
- [Application Insights SDK toevoegen aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
