---
title: Verzamelen en analyseren van Windows-gebeurtenislogboeken in Azure Monitor | Microsoft Docs
description: Beschrijft hoe u configureert de verzameling van Windows-gebeurtenislogboeken door Azure Monitor en details van de records die zij maken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: 8fcab1ead4ab6135e715dc173829178e43f8af2a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236929"
---
# <a name="windows-event-log-data-sources-in-azure-monitor"></a>Windows-gebeurtenislogboek gegevensbronnen in Azure Monitor
Windows-gebeurtenislogboeken zijn een van de meest voorkomende [gegevensbronnen](agent-data-sources.md) voor het verzamelen van gegevens met behulp van Windows-agents, omdat veel toepassingen naar het Windows-gebeurtenislogboek schrijven.  U kunt gebeurtenissen verzamelen van standard logboeken zoals systeem- en naast het opgeven van een aangepaste logboeken die zijn gemaakt door toepassingen die u nodig hebt om te controleren.

![Windows-gebeurtenissen](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Configuratie Windows gebeurtenislogboeken
Configureren van Windows-gebeurtenislogboeken vanuit de [menu van de gegevens in de geavanceerde instellingen](agent-data-sources.md#configuring-data-sources).

Azure Monitor verzamelt alleen gebeurtenissen uit de Windows-gebeurtenislogboeken die zijn opgegeven in de instellingen.  U kunt een gebeurtenislogboek toevoegen door te klikken en naam van het logboek typen **+** .  Voor elk logboek, worden alleen de gebeurtenissen met de geselecteerde ernstcategorieën verzameld.  Raadpleeg de ernstcategorieën voor het logboek dat u wenst te verzamelen.  U kunt aanvullende criteria om te filteren van gebeurtenissen kan niet opgeven.

Terwijl u de naam van een gebeurtenislogboek typt, bevat Azure Monitor suggesties van algemene namen van het gebeurtenislogboek. Als het logboek dat u wilt toevoegen niet wordt weergegeven in de lijst, kunt u deze nog steeds toevoegen door in de volledige naam van het logboek te typen. U vindt de volledige naam van het logboek met behulp van Logboeken. Open in de logboeken de *eigenschappen* pagina voor het logboek en kopieer de tekenreeks van de *volledige naam* veld.

![Windows-gebeurtenissen configureren](media/data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Gegevensverzameling
Azure Monitor worden verzameld van elke gebeurtenis die overeenkomt met een geselecteerde ernst van een bewaakte gebeurtenislogboek als de gebeurtenis wordt gemaakt.  De agent registreert plaats daarvan in elk logboek voor systeemgebeurtenissen die het verzamelt uit.  Als de agent voor een bepaalde periode offline gaat, klikt u vervolgens verzamelt het gebeurtenissen van waar het laatste afgebroken, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent offline was.  Er is het mogelijk om deze gebeurtenissen niet worden verzameld als het gebeurtenislogboek wordt verpakt met niet-geïnde gebeurtenissen wordt overschreven terwijl de agent offline is.

>[!NOTE]
>Azure Monitor verzamelt geen controlegebeurtenissen die zijn gemaakt door SQL Server vanuit de bron *MSSQLSERVER* met gebeurtenis-ID 18453 met trefwoorden - *klassieke* of *controle geslaagd* en sleutelwoord *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows-gebeurtenis registreert eigenschappen
Windows-gebeurtenis legt vast zijn een type **gebeurtenis** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |De naam van de computer waarop de gebeurtenis is verzameld. |
| Culture |Categorie van de gebeurtenis. |
| EventData |Alle gebeurtenisgegevens in een onbewerkte indeling. |
| EventID |Nummer van de gebeurtenis. |
| EventLevel |Ernst van de gebeurtenis in numerieke vorm. |
| EventLevelName |Ernst van de gebeurtenis in de tekstvorm. |
| EventLog |De naam van het gebeurtenislogboek dat de gebeurtenis is verzameld. |
| ParameterXml |Gebeurtenis parameterwaarden in XML-indeling. |
| ManagementGroupName |Naam van de beheergroep van System Center Operations Manager-agents.  Voor andere agents is deze waarde `AOI-<workspace ID>` |
| RenderedDescription |Beschrijving van gebeurtenis met parameterwaarden |
| source |Bron van de gebeurtenis. |
| SourceSystem |Het type van de agent die de gebeurtenis is verzameld. <br> OpsManager – Windows-agent, rechtstreeks verbinding maken of Operations Manager worden beheerd <br> Linux: alle Linux-agents  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |De datum en tijd waarop die de gebeurtenis is gemaakt in Windows. |
| UserName |De naam van de gebruiker van het account waarmee de gebeurtenis is vastgelegd. |

## <a name="log-queries-with-windows-events"></a>Logboeken-query's met Windows-gebeurtenissen
De volgende tabel bevat voorbeelden van Logboeken-query's waarmee Windows-gebeurtenis legt vast worden opgehaald.

| Query’s uitvoeren | Description |
|:---|:---|
| Gebeurtenis |Alle Windows-gebeurtenissen. |
| Event &#124; where EventLevelName == "error" |Alle Windows-gebeurtenissen met de ernst van fout. |
| Gebeurtenis &#124; count() by Source samenvatten |Aantal van de Windows-gebeurtenissen op bron. |
| Event &#124; where EventLevelName == "error" &#124; summarize count() by Source |Aantal van de Windows-foutgebeurtenissen door bron. |


## <a name="next-steps"></a>Volgende stappen
* Log Analytics voor het verzamelen van andere configureren [gegevensbronnen](agent-data-sources.md) voor analyse.
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  
* Configureer [verzamelen van prestatiemeteritems](data-sources-performance-counters.md) van uw Windows-agents.