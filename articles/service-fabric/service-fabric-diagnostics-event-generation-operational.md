---
title: Lijst met gebeurtenissen van Azure Service Fabric | Microsoft Docs
description: Uitgebreide lijst van gebeurtenissen die worden geleverd door Azure Service Fabric bewaken-clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: cde0464985f756132c60453c4e79ffefd4a1dd2c
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788597"
---
# <a name="list-of-service-fabric-events"></a>Lijst met Service Fabric-gebeurtenissen 

Service Fabric wordt aangegeven dat een primaire set Clustergebeurtenissen om te informeren over de status van het cluster als [Service Fabric-gebeurtenissen](service-fabric-diagnostics-events.md). Deze zijn gebaseerd op acties die door Service Fabric wordt uitgevoerd op uw knooppunten als uw cluster of management beslissingen door een cluster eigenaar/operator. Deze gebeurtenissen kunnen worden geopend door te configureren op een aantal manieren, inclusief het configureren van [Azure Monitor-logboeken met uw cluster](service-fabric-diagnostics-oms-setup.md), of het uitvoeren van query's de [EventStore](service-fabric-diagnostics-eventstore.md). Deze gebeurtenissen zijn opgenomen in het gebeurtenislogboek - op Windows-machines, zodat u Service Fabric-gebeurtenissen in Logboeken ziet. 

Hier volgen enkele kenmerken van deze gebeurtenissen
* Elke gebeurtenis is gekoppeld aan een bepaalde entiteit in het cluster bijvoorbeeld toepassing, Service, knooppunt, Replica.
* Elke gebeurtenis bevat een set algemene velden: EventInstanceId, EventName en categorie.
* Elke gebeurtenis bevat de velden die de gebeurtenis terug naar de entiteit die is gekoppeld. De gebeurtenis ApplicationCreated hoeft bijvoorbeeld velden die de naam van de toepassing gemaakt.
* Gebeurtenissen zijn gestructureerd zodanig dat ze kunnen worden gebruikt in een aantal hulpprogramma's om u te doen verdere analyse. Bovendien zijn relevante details van een gebeurtenis gedefinieerd als afzonderlijke eigenschappen in plaats van een lange tekenreeks. 
* Gebeurtenissen worden geschreven door verschillende subsystemen weer in Service Fabric worden aangeduid met Source(Task) hieronder. Meer informatie vindt u op deze subsystemen in [Service Fabric-architectuur](service-fabric-architecture.md) en [technische overzicht van Service Fabric](service-fabric-technical-overview.md).

Hier volgt een lijst van deze Service Fabric-gebeurtenissen geordend op entiteit.

## <a name="cluster-events"></a>Clustergebeurtenissen

**Upgrade Clustergebeurtenissen**

Meer informatie over het upgraden van clusters kunnen worden gevonden [hier](service-fabric-cluster-upgrade-windows-server.md).

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau | 
| --- | --- | --- | --- | --- | --- | 
| 29627 | ClusterUpgradeStarted | Upgrade | Een clusterupgrade van een is gestart | CM | Informatief |
| 29628 | ClusterUpgradeCompleted | Upgrade | Een clusterupgrade is voltooid | CM | Informatief | 
| 29629 | ClusterUpgradeRollbackStarted | Upgrade | Een clusterupgrade van een is begonnen met het ongedaan maken  | CM | Waarschuwing | 
| 29630 | ClusterUpgradeRollbackCompleted | Upgrade | Een clusterupgrade van een is voltooid, terug te draaien | CM | Waarschuwing | 
| 29631 | ClusterUpgradeDomainCompleted | Upgrade | Een upgradedomein is voltooid tijdens de upgrade van een cluster upgraden | CM | Informatief | 

## <a name="node-events"></a>Knooppunt-gebeurtenissen

**Levenscyclusgebeurtenissen voor het knooppunt** 

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau |
| --- | --- | ---| --- | --- | --- | 
| 18602 | NodeDeactivateCompleted | StateTransition | Deactivering van een knooppunt is voltooid | FM | Informatief | 
| 18603 | NodeUp | StateTransition | Het cluster is gedetecteerd op dat een knooppunt is gestart | FM | Informatief | 
| 18604 | NodeDown | StateTransition | Het cluster heeft gedetecteerd dat een knooppunt is afgesloten. Tijdens een knooppunt opnieuw wordt opgestart ziet u een gebeurtenis NodeDown, gevolgd door een gebeurtenis NodeUp |  FM | Fout | 
| 18605 | NodeAddedToCluster | StateTransition |  Een nieuw knooppunt is toegevoegd aan het cluster en Service Fabric kunt toepassingen implementeren op dit knooppunt | FM | Informatief | 
| 18606 | NodeRemovedFromCluster | StateTransition |  Een knooppunt is verwijderd uit het cluster. Service Fabric wordt niet meer toepassingen implementeren op dit knooppunt | FM | Informatief | 
| 18607 | NodeDeactivateStarted | StateTransition |  Deactivering van een knooppunt is gestart | FM | Informatief | 
| 25621 | NodeOpenSucceeded | StateTransition |  Een knooppunt is gestart | FabricNode | Informatief | 
| 25622 | NodeOpenFailed | StateTransition |  Een knooppunt kan niet starten en deelnemen aan de ring | FabricNode | Fout | 
| 25624 | NodeClosed | StateTransition |  Een knooppunt is afgesloten | FabricNode | Informatief | 
| 25626 | NodeAborted | StateTransition |  Een knooppunt is ungracefully afgesloten | FabricNode | Fout | 

## <a name="application-events"></a>Toepassingsgebeurtenissen

**Levenscyclus van toepassingsgebeurtenissen**

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau | 
| --- | --- | --- | --- | --- | --- | 
| 29620 | ApplicationCreated | LifeCycle | Een nieuwe toepassing is gemaakt | CM | Informatief | 
| 29625 | ApplicationDeleted | LifeCycle | Een bestaande toepassing is verwijderd | CM | Informatief | 
| 23083 | ApplicationProcessExited | LifeCycle | Een proces in een toepassing is afgesloten | Hosten | Informatief | 

**Upgrade van toepassingsgebeurtenissen**

Meer informatie over upgrades van toepassingen vindt [hier](service-fabric-application-upgrade.md).

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- | 
| 29621 | ApplicationUpgradeStarted | Upgrade | Een upgrade van de toepassing is gestart | CM | Informatief | 
| 29622 | ApplicationUpgradeCompleted | Upgrade | Een upgrade van de toepassing is voltooid | CM | Informatief | 
| 29623 | ApplicationUpgradeRollbackStarted | Upgrade | Een upgrade van de toepassing is begonnen met het ongedaan maken |CM | Waarschuwing | 
| 29624 | ApplicationUpgradeRollbackCompleted | Upgrade | Een upgrade van de toepassing is voltooid, terug te draaien | CM | Waarschuwing | 
| 29626 | ApplicationUpgradeDomainCompleted | Upgrade | Een upgradedomein is voltooid tijdens de upgrade van een toepassing upgraden | CM | Informatief | 

## <a name="service-events"></a>Gebeurtenissen van de service

**Gebeurtenissen van de levenscyclus van service**

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | LifeCycle | Een nieuwe service is gemaakt | FM | Informatief | 
| 18658 | ServiceDeleted | LifeCycle | Een bestaande service is verwijderd | FM | Informatief | 

## <a name="partition-events"></a>Partitie-gebeurtenissen

**Partitie verplaatsen gebeurtenissen**

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau | 
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | LifeCycle | Een herconfiguratie van de partitie is voltooid | RA | Informatief | 

## <a name="replica-events"></a>Replica-gebeurtenissen

**Levenscyclusgebeurtenissen voor replica**

| Gebeurtenis-id | Name | Category | Description |Bron (taak) | Niveau |
| --- | --- | ---| --- | --- | --- |
| 61701 | ReliableDictionaryOpened | LifeCycle | Betrouwbare woordenlijst is geopend | DistributedDictionary | Informatief |
| 61702 | ReliableDictionaryClosed | LifeCycle | Betrouwbare woordenlijst is verbroken | DistributedDictionary | Informatief |
| 61703 | ReliableDictionaryCheckpointRecovered | LifeCycle | Betrouwbare dictionary is het controlepunt hersteld | DistributedDictionary | Informatief |
| 61704 | ReliableDictionaryCheckpointFilesSent | LifeCycle | Replica heeft betrouwbare dictionary controlepuntbestanden verzonden | DistributedDictionary | Informatief |
| 61705 | ReliableDictionaryCheckpointFilesReceived | LifeCycle | Replica heeft ontvangen van betrouwbare dictionary controlepuntbestanden | DistributedDictionary | Informatief |
| 61963 | ReliableQueueOpened | LifeCycle | Betrouwbare wachtrij is geopend | DistributedQueue | Informatief |
| 61964 | ReliableQueueClosed | LifeCycle | Betrouwbare wachtrij is gesloten | DistributedQueue | Informatief |
| 61965 | ReliableQueueCheckpointRecovered | LifeCycle | Betrouwbare wachtrij is het controlepunt hersteld | DistributedQueue | Informatief |
| 61966 | ReliableQueueCheckpointFilesSent | LifeCycle | Replica is controlepuntbestanden betrouwbare wachtrij verzonden | DistributedQueue | Informatief |
| 63647 | ReliableQueueCheckpointFilesReceived | LifeCycle | Replica heeft de controlepuntbestanden betrouwbare wachtrij ontvangen | DistributedQueue | Informatief |
| 63648 | ReliableConcurrentQueueOpened | LifeCycle | Betrouwbare gelijktijdige wachtrij is geopend | ReliableConcurrentQueue | Informatief |
| 63649 | ReliableConcurrentQueueClosed | LifeCycle | Betrouwbare gelijktijdige wachtrij is gesloten | ReliableConcurrentQueue | Informatief |
| 63650 | ReliableConcurrentQueueCheckpointRecovered | LifeCycle | Betrouwbare gelijktijdige wachtrij is het controlepunt hersteld | ReliableConcurrentQueue | Informatief |
| 61687 | TStoreError | Fout | Betrouwbare verzameling heeft een onverwachte fout ontvangen | TStore | Fout |
| 63831 | PrimaryFullCopyInitiated | LifeCycle | Primaire replica heeft geprobeerd een volledige kopie | TReplicator | Informatief |
| 63832 | PrimaryPartialCopyInitiated | LifeCycle | Primaire replica heeft een gedeeltelijke kopie gestart | TReplicator | Informatief |
| 16831 | BuildIdleReplicaStarted | LifeCycle | Primaire replica is gestart met het bouwen van een niet-actieve replica | Replicatie | Informatief |
| 16832 | BuildIdleReplicaCompleted | LifeCycle | Primaire replica is voltooid met het bouwen van een niet-actieve replica | Replicatie | Informatief |
| 16833 | BuildIdleReplicaFailed | LifeCycle | Primaire replica is mislukt met het bouwen van een niet-actieve replica | Replicatie | Waarschuwing |
| 16834 | PrimaryReplicationQueueFull | Gezondheidszorg | Primaire replica de replicatiewachtrij is vol. | Replicatie | Waarschuwing |
| 16835 | PrimaryReplicationQueueWarning | Gezondheidszorg | Primaire replica de replicatiewachtrij is bijna volledig | Replicatie | Waarschuwing |
| 16836 | PrimaryReplicationQueueWarningMitigated | Gezondheidszorg | Primaire replica de replicatiewachtrij is OK | Replicatie | Informatief |
| 16837 | SecondaryReplicationQueueFull | Gezondheidszorg | Secundaire replica replicatiewachtrij is vol. | Replicatie | Waarschuwing |
| 16838 | SecondaryReplicationQueueWarning | Gezondheidszorg | Secundaire replica de replicatiewachtrij is bijna volledig | Replicatie | Waarschuwing |
| 16839 | SecondaryReplicationQueueWarningMitigated | Gezondheidszorg | Secundaire replica de replicatiewachtrij is OK | Replicatie | Informatief |
| 16840 | PrimaryFaultedSlowSecondary | Gezondheidszorg | Primaire replica is een trage secundaire replica mislukt | Replicatie | Waarschuwing |
| 16841 | ReplicatorFaulted | Gezondheidszorg | Replica is mislukt | Replicatie | Waarschuwing |

## <a name="container-events"></a>Containergebeurtenissen

**Gebeurtenissen in de container levensduur** 

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Een container is gestart | Hosten | Informatief | 1 |
| 23075 | ContainerDeactivated | Een container is gestopt | Hosten | Informatief | 1 |
| 23082 | ContainerExited | Een container is afgesloten: Controleer de vlag UnexpectedTermination | Hosten | Informatief | 1 |

## <a name="health-reports"></a>Statusrapporten

De [Service Fabric-statusmodel](service-fabric-health-introduction.md) biedt een uitgebreide, flexibele en uitbreidbare evalueren en rapportage. Starten van Service Fabric versie 6.2 worden health-gegevens geschreven als Platform-gebeurtenissen voor historische records van de gezondheid van. Om te voorkomen dat het volume van de health-gebeurtenissen lage, schrijven we alleen de volgende Service Fabric-gebeurtenissen:

* Alle `Error` of `Warning` statusrapporten
* `Ok` statusrapporten tijdens overgangen
* Wanneer een `Error` of `Warning` statusgebeurtenis is verlopen. Dit kan worden gebruikt om te bepalen hoe lang een entiteit is niet in orde

**Cluster-rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Een nieuw cluster health rapport is beschikbaar | HM | Informatief | 1 |
| 54437 | ClusterHealthReportExpired | Een bestaand cluster health rapport is verlopen | HM | Informatief | 1 |

**Knooppunt rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Een nieuw knooppunt health rapport is beschikbaar | HM | Informatief | 1 |
| 54432 | NodeHealthReportExpired | Een bestaand knooppunt health rapport is verlopen | HM | Informatief | 1 |

**Toepassing rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Een nieuwe statusrapport van toepassing is gemaakt. Dit is voor niet-geïmplementeerde toepassingen. | HM | Informatief | 1 |
| 54426 | DeployedApplicationNewHealthReport | Een nieuw geïmplementeerde toepassing health rapport is gemaakt | HM | Informatief | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Een nieuw geïmplementeerde service health-rapport is gemaakt | HM | Informatief | 1 |
| 54434 | ApplicationHealthReportExpired | Een bestaande toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Een bestaande gedistribueerde toepassing statusrapport is verlopen | HM | Informatief | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Een bestaand geïmplementeerde service health-rapport is verlopen | HM | Informatief | 1 |

**Service health rapport-gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Een nieuwe service health-rapport is gemaakt | HM | Informatief | 1 |
| 54433 | ServiceHealthReportExpired | Een bestaande service-statusrapport is verlopen | HM | Informatief | 1 |

**Partitie rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Een nieuwe statusrapport van de partitie is gemaakt | HM | Informatief | 1 |
| 54431 | PartitionHealthReportExpired | Een bestaande partitie-statusrapport is verlopen | HM | Informatief | 1 |

**Replica rapport statusgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Een statusrapport stateful replica is gemaakt | HM | Informatief | 1 |
| 54430 | StatelessInstanceNewHealthReport | Een nieuw exemplaar van staatloze health rapport is gemaakt | HM | Informatief | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Een bestaand stateful replica health rapport is verlopen | HM | Informatief | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Een bestaand exemplaar van staatloze health rapport is verlopen | HM | Informatief | 1 |

## <a name="chaos-testing-events"></a>Testen chaos-gebeurtenissen 

**Chaos-sessiegebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Een Chaos testen van de sessie is gestart | Testbaarheid | Informatief | 1 |
| 50023 | ChaosStopped | Een sessie testen Chaos is gestopt | Testbaarheid | Informatief | 1 |

**Gebeurtenissen van chaos-knooppunt**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Een knooppunt is gepland om opnieuw te starten als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50087 | ChaosNodeRestartCompleted | Een knooppunt is opnieuw op te starten als onderdeel van een Chaos sessie testen voltooid | Testbaarheid | Informatief | 1 |

**Chaos-toepassingsgebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Een code-pakket opnieuw opstarten is gepland tijdens een sessie testen Chaos | Testbaarheid | Informatief | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Code-pakket opnieuw worden opgestart is tijdens een sessie testen Chaos voltooid | Testbaarheid | Informatief | 1 |

**Chaos-partitie gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Een primaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Een secundaire partitie is ingesteld om te verplaatsen als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | De diepgaande analyse van de primaire partitie verplaatsing is beschikbaar | Testbaarheid | Informatief | 1 |

**Chaos replica gebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Replica opnieuw is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50051 | ChaosReplicaRemovalScheduled | De verwijdering van een replica is gepland als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Een replica verwijderen is voltooid als onderdeel van een Chaos sessie testen | Testbaarheid | Informatief | 1 |

## <a name="other-events"></a>Andere gebeurtenissen

**Correlatiegebeurtenissen**

| Gebeurtenis-id | Name | Description |Bron (taak) | Niveau | Versie |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Een correlatie er is aangetroffen | Testbaarheid | Informatief | 1 |

## <a name="events-prior-to-version-62"></a>Gebeurtenissen voorafgaand aan versie 6.2

Hier volgt een uitgebreide lijst van gebeurtenissen die worden geleverd door de Service Fabric voorafgaand aan versie 6.2.

| Gebeurtenis-id | Name | Bron (taak) | Niveau |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informatief |
| 25621 | NodeOpenedSuccess | FabricNode | Informatief |
| 25622 | NodeOpenedFailed | FabricNode | Informatief |
| 25623 | NodeClosing | FabricNode | Informatief |
| 25624 | NodeClosed | FabricNode | Informatief |
| 25625 | NodeAborting | FabricNode | Informatief |
| 25626 | NodeAborted | FabricNode | Informatief |
| 29627 | ClusterUpgradeStart | CM | Informatief |
| 29628 | ClusterUpgradeComplete | CM | Informatief |
| 29629 | ClusterUpgradeRollback | CM | Informatief |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informatief |
| 29631 | ClusterUpgradeDomainComplete | CM | Informatief |
| 23074 | ContainerActivated | Hosten | Informatief |
| 23075 | ContainerDeactivated | Hosten | Informatief |
| 29620 | ApplicationCreated | CM | Informatief |
| 29621 | ApplicationUpgradeStart | CM | Informatief |
| 29622 | ApplicationUpgradeComplete | CM | Informatief |
| 29623 | ApplicationUpgradeRollback | CM | Informatief |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informatief |
| 29625 | ApplicationDeleted | CM | Informatief |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informatief |
| 18566 | ServiceCreated | FM | Informatief |
| 18567 | ServiceDeleted | FM | Informatief |

## <a name="next-steps"></a>Volgende stappen

* Bekijk een overzicht van [diagnostische gegevens in Service Fabric](service-fabric-diagnostics-overview.md)
* Meer informatie over de EventStore in [Eventstore-overzicht van Service Fabric](service-fabric-diagnostics-eventstore.md)
* Wijzigen van uw [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) configuratie om meer logboeken te verzamelen
* [Instellen van Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) om te zien van uw operationele kanaal-Logboeken
