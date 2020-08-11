---
title: Regels voor het verzamelen van gegevens in Azure Monitor (preview-versie)
description: Overzicht van regels voor gegevens verzameling (Dcr's) in Azure Monitor met inbegrip van de inhoud en structuur en hoe u deze kunt maken en gebruiken.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/10/2020
ms.openlocfilehash: af5324373359cea643a3e31b6bb94e614ddb7e36
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88082975"
---
# <a name="data-collection-rules-in-azure-monitor-preview"></a>Regels voor het verzamelen van gegevens in Azure Monitor (preview-versie)
Met regels voor gegevens verzameling (DCR) worden gegevens in Azure Monitor gedefinieerd en wordt aangegeven waar de gegevens moeten worden verzonden of opgeslagen. Dit artikel bevat een overzicht van regels voor het verzamelen van gegevens, inclusief de inhoud en structuur en hoe u deze kunt maken en gebruiken.

## <a name="input-sources"></a>Invoer bronnen
Regels voor het verzamelen van gegevens ondersteunen momenteel de volgende invoer bronnen:

- Virtuele machine van Azure met de Azure Monitor-agent. Zie [gegevens verzameling configureren voor de Azure monitor-agent (preview)](data-collection-rule-azure-monitor-agent.md).



## <a name="components-of-a-data-collection-rule"></a>Onderdelen van een regel voor gegevens verzameling
Een regel voor het verzamelen van gegevens bevat de volgende onderdelen.

| Onderdeel | Beschrijving |
|:---|:---|
| Gegevensbronnen | Unieke bron van bewakings gegevens met een eigen indeling en methode waarmee de gegevens worden weer gegeven. Voor beelden van een gegevens bron zijn Windows-gebeurtenis logboeken, prestatie meter items en syslog. Elke gegevens bron komt overeen met een bepaald gegevens bron type, zoals hieronder wordt beschreven. |
| Stromen | Unieke koppeling die een set gegevens bronnen beschrijft die worden getransformeerd en geschematiseerde als één type. Elke gegevens bron vereist een of meer streams en één stroom kan door meerdere gegevens bronnen worden gebruikt. Alle gegevens bronnen in een stroom delen een gemeen schappelijk schema. Gebruik meerdere streams bijvoorbeeld wanneer u een bepaalde gegevens bron wilt verzenden naar meerdere tabellen in dezelfde Log Analytics-werk ruimte. |
| Bestemmingen | De set doelen waarnaar de gegevens moeten worden verzonden. Voor beelden zijn Log Analytics-werk ruimte, Azure Monitor metrische gegevens en Azure Event Hubs. | 
| Gegevensstromen | Definitie van de verzen ding naar welke bestemming moet worden verzonden. | 

In het volgende diagram ziet u de onderdelen van een gegevens verzamelings regel en de bijbehorende relatie

[![Diagram van DCR](media/data-collection-rule/data-collection-rule-components.png)](media/data-collection-rule/data-collection-rule-components.png#lightbox)

### <a name="data-source-types"></a>Typen gegevensbronnen
Elke gegevens bron heeft een gegevens bron type. Elk type definieert een unieke set eigenschappen die voor elke gegevens bron moet worden opgegeven. De gegevens bron typen die momenteel beschikbaar zijn, worden weer gegeven in de volgende tabel.

| Gegevens bron type | Beschrijving | 
|:---|:---|
| switch | Gegevens bron op basis van VM-extensie |
| Performance Counters | Prestatie meter items voor Windows en Linux |
| syslog | Syslog-gebeurtenissen op virtuele Linux-machine |
| windowsEventLogs | Windows-gebeurtenis logboek |


## <a name="limits"></a>Limieten
De volgende tabel bevat de limieten die momenteel van toepassing zijn op elke regel voor het verzamelen van gegevens.

| Limiet | Waarde |
|:---|:---|
| Maximum aantal gegevens bronnen | 10 |
| Maximale tellers voor prestatie meter items | 100 |
| Maximale namen van faciliteiten in SysLog | 20 |
| Maximum aantal XPath-query's in EventLog | 100 |
| Maximum aantal gegevens stromen | 10 |
| Maximum aantal gegevens stromen | 10 |
| Maximum aantal uitbrei dingen | 10 |
| Maximale grootte van extensie-instellingen | 32 KB |
| Maximum aantal Log Analytics-werk ruimten | 10 |


## <a name="create-a-dcr"></a>Een DCR maken
Er zijn momenteel twee beschik bare methoden voor het maken van een DCR:

- [Gebruik de Azure Portal](data-collection-rule-azure-monitor-agent.md) om een regel voor het verzamelen van gegevens te maken en deze te koppelen aan een of meer virtuele machines.
- Bewerk de regel voor het verzamelen van gegevens rechtstreeks in JSON en verzend deze met behulp van de REST API.

## <a name="sample-data-collection-rule"></a>Regel voor het verzamelen van voorbeeld gegevens
De onderstaande regel voor het verzamelen van gegevens is voor virtuele machines met Azure Management Agent en bevat de volgende details:

- Prestatiegegevens
  - Verzamelt elke 15 seconden een specifieke processor, het geheugen, de logische schijf en de fysieke schijf en uploadt elke minuut.
  - Verzamelt specifieke proces tellers elke 30 seconden en uploadt elke vijf minuten.
- Windows-gebeurtenissen
  - Verzamelt Windows-beveiligings gebeurtenissen en uploadt elke minuut.
  - Verzamelt Windows-toepassings-en systeem gebeurtenissen en uploadt elke vijf minuten.
- Syslog
  - Verzamelt gebeurtenissen voor fout opsporing, essentiële activiteiten en nood gevallen van cron-faciliteit.
  - Hiermee worden waarschuwings-, kritieke en nood gebeurtenissen van syslog-faciliteit verzameld.
- Bestemmingen
  - Hiermee worden alle gegevens verzonden naar een Log Analytics werk ruimte met de naam centralTeamWorkspace.
  - Hiermee worden prestatie gegevens naar Azure Monitor meet waarden in het huidige abonnement verzonden.

```json
{
    "location": "eastus",
    "properties": {
      "dataSources": {
        "performanceCounters": [
          {
            "name": "cloudTeamCoreCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT1M",
            "samplingFrequencyInSeconds": 15,
            "counterSpecifiers": [
              "\\Processor(_Total)\\% Processor Time",
              "\\Memory\\Committed Bytes",
              "\\LogicalDisk(_Total)\\Free Megabytes",
              "\\PhysicalDisk(_Total)\\Avg. Disk Queue Length"
            ]
          },
          {
            "name": "appTeamExtraCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT5M",
            "samplingFrequencyInSeconds": 30,
            "counterSpecifiers": [
              "\\Process(_Total)\\Thread Count"
            ]
          }
        ],
        "windowsEventLogs": [
          {
            "name": "cloudSecurityTeamEvents",
            "streams": [
              "Microsoft-WindowsEvent"
            ],
            "scheduledTransferPeriod": "PT1M",
            "xPathQueries": [
              "Security!*"
            ]
          },
          {
            "name": "appTeam1AppEvents",
            "streams": [
              "Microsoft-WindowsEvent"
            ],
            "scheduledTransferPeriod": "PT5M",
            "xPathQueries": [
              "System!*[System[(Level = 1 or Level = 2 or Level = 3)]]",
              "Application!*[System[(Level = 1 or Level = 2 or Level = 3)]]"
            ]
          }
        ],
        "syslog": [
          {
            "name": "cronSyslog",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "cron"
            ],
            "logLevels": [
              "Debug",
              "Critical",
              "Emergency"
            ]
          },
          {
            "name": "sylogBase",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "syslog"
            ],
            "logLevels": [
              "Alert",
              "Critical",
              "Emergency"
            ]
          }
        ]
      },
      "destinations": {
        "logAnalytics": [
          {
            "workspaceResourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.OperationalInsights/workspaces/my-workspace",
            "name": "centralWorkspace"
          }
        ]
      },
      "dataFlows": [
        {
          "streams": [
            "Microsoft-Perf",
            "Microsoft-Syslog",
            "Microsoft-WindowsEvent"
          ],
          "destinations": [
            "centralWorkspace"
          ]
        }
      ]
    }
  }
```


## <a name="next-steps"></a>Volgende stappen

- [Maak een gegevens verzamelings regel](data-collection-rule-azure-monitor-agent.md) en een koppeling naar deze van een virtuele machine met behulp van de Azure monitor-agent.