---
title: Oplossen van problemen met Azure bijhouden
description: Dit artikel bevat informatie over het oplossen van wijzigingen bijhouden
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2a6610b5cb3f01fc70b1737fc4492e09d9a7637b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61085320"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Problemen oplossen met wijzigingen bijhouden en inventaris

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Scenario: Records voor wijzigingen bijhouden worden niet voor Windows-Machines weergegeven

#### <a name="issue"></a>Probleem

U kunt geen inventaris of bijhouden resultaten voor Windows-machines die toegevoegd voor het bijhouden zijn niet ziet.

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. De **Microsoft Monitoring Agent** niet wordt uitgevoerd
2. Communicatie terug naar het Automation-Account wordt geblokkeerd.
3. De Management Packs voor wijzigingen bijhouden niet worden gedownload.
4. De virtuele machine onboarding wordt uitgevoerd kan afkomstig zijn uit een gekloonde machine die niet Sysprep met de Microsoft Monitoring Agent is geïnstalleerd is.

#### <a name="resolution"></a>Oplossing

1. Controleer of de **Microsoft Monitoring Agent** (HealthService.exe) wordt uitgevoerd op de machine.
1. Controleer **logboeken** op de machine en het zoeken naar alle gebeurtenissen die het woord `changetracking` erin.
1. Ga naar, [netwerkplanning](../automation-hybrid-runbook-worker.md#network-planning) voor meer informatie over welke adressen en poorten moeten worden toegestaan voor wijzigingen bijhouden om te werken.
1. Controleer of de volgende wijzigingen bijhouden en inventaris management packs lokaal bestaan:
    * Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
    * Microsoft.IntelligencePacks.InventoryChangeTracking.*
    * Microsoft.IntelligencePacks.SingletonInventoryCollection.*
1. Als u met behulp van een gekloonde installatiekopie, sysprep de installatiekopie van het eerste en de MMA-agent installeren na de gebeurtenis.

Als deze oplossingen het probleem niet verhelpen, en u contact op met ondersteuning, kunt u de volgende opdrachten voor het verzamelen van de diagnose in de agent uitvoeren

Op de computer van de agent, gaat u naar `C:\Program Files\Microsoft Monitoring Agent\Agent\Tools` en voer de volgende opdrachten uit:

```cmd
set stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Door de standaardfout tracering is ingeschakeld, als u wilt inschakelen, uitgebreide foutberichten worden weergegeven als in het voorgaande voorbeeld, gebruik `VER` parameter. Voor informatie traceringen, gebruikt u `INF` bij het aanroepen van `StartTracing.cmd`.

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
