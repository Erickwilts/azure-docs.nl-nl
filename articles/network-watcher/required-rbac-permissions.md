---
title: Vereiste machtigingen voor het gebruik van de mogelijkheden van Azure Network Watcher | Microsoft Docs
description: Meer informatie over welke machtigingen voor het beheer van toegang van Azure op basis van rollen zijn vereist om te werken met Network Watcher-mogelijkheden.
services: network-watcher
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: kumud
ms.openlocfilehash: 8c8fe6125d9c638fedadc3d299ff0ac0d601fd61
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64685701"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Machtigingen voor beheer op basis van de rol vereist voor het gebruik van Network Watcher-mogelijkheden

Op rollen gebaseerd toegangsbeheer in Azure (RBAC) kunt u alleen de specifieke acties toewijzen aan leden van uw organisatie die zij nodig hebben om hun toegewezen taken te voltooien. Voor het gebruik van Network Watcher-mogelijkheden, het account dat u zich aanmeldt bij Azure, moeten worden toegewezen aan de [eigenaar](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [Inzender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), of [Inzender voor netwerken](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) ingebouwde rollen, of toegewezen aan een [aangepaste rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) die de acties die worden weergegeven voor elke functie Network Watcher in de volgende secties is toegewezen. Zie voor meer informatie over de mogelijkheden van Network Watcher [wat is Network Watcher?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Network Watcher

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Een network watcher ophalen                                          |
| Microsoft.Network/networkWatchers/write                             | Maken of bijwerken van een network watcher                             |
| Microsoft.Network/networkWatchers/delete                            | Een netwerk-watcher verwijderen                                       |

## <a name="nsg-flow-logs"></a>NSG-stroomlogboeken

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Een stroom logboek configureren                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Querystatus van de voor een stroomlogboek                                    |

## <a name="connection-troubleshoot"></a>Probleemoplossing voor verbindingen

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectivityCheck/action          | Een verbinding initiëren test oplossen
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | De resultaten van de query van een verbinding oplossen testen                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Voer een verbinding testen oplossen                             |

## <a name="connection-monitor"></a>Verbindingsmonitor

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Start een nieuwe Verbindingscontrole                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Een nieuwe Verbindingscontrole te stoppen                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Query uitvoeren op een nieuwe Verbindingscontrole                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Een nieuwe Verbindingscontrole ophalen                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Een verbindingsmonitor maken                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Een verbindingsmonitor verwijderen                                    |

## <a name="packet-capture"></a>Pakketopname

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | De status van een pakketopname opvragen                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Een pakketopname stoppen                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Een pakketopname ophalen                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | een pakketopname maken                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Een pakketopname verwijderen                                        |

## <a name="ip-flow-verify"></a>IP-stroomverificatie

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Een IP-stroom controleren                                              |

## <a name="next-hop"></a>Volgende hop

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | De volgende hop van een virtuele machine ophalen                                     |

## <a name="network-security-group-view"></a>Netwerkbeveiligingsgroep weergeven

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Weergave-beveiligingsgroepen                                           |

## <a name="topology"></a>Topologie

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topologie ophalen                                                   |

## <a name="reachability-report"></a>Bereikbaarheid rapport

| Bewerking                                                              | Name                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Een Azure bereikbaarheid rapport ophalen                               |

## <a name="additional-actions"></a>Aanvullende acties

Functionaliteit van Network Watcher is ook vereist voor de volgende acties:

- Microsoft.Authorization/\*/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachines/extensions/Read
- Microsoft.Compute/virtualMachines/extensions/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Compute/virtualMachineScaleSets/extensions/Read
- Microsoft.Compute/virtualMachineScaleSets/extensions/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*
