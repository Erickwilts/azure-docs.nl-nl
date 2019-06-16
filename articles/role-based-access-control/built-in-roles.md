---
title: Ingebouwde rollen voor Azure-resources | Microsoft Docs
description: Beschrijving van de ingebouwde functies voor op rollen gebaseerd toegangsbeheer (RBAC) en Azure-resources. De acties, NotActions, DataActions en NotDataActions bevat.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/16/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: 427c4615fcbb036ffff56a8fc592f258fb98845e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755121"
---
# <a name="built-in-roles-for-azure-resources"></a>Ingebouwde rollen voor Azure-resources

[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) heeft diverse ingebouwde rollen voor Azure-resources die u aan gebruikers, groepen, service-principals en beheerde identiteiten toewijzen kunt. Roltoewijzingen zijn de manier waarop u de toegang tot Azure-resources beheren. Als de ingebouwde rollen niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen [aangepaste rollen maken voor Azure-resources](custom-roles.md).

In dit artikel geeft een lijst van de ingebouwde rollen voor Azure-resources, die altijd zijn nog in ontwikkeling. Als u de nieuwste functies, gebruikt [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) of [az role definitielijst](/cli/azure/role/definition#az-role-definition-list). Als u op zoek bent voor de beheerdersrollen voor Azure Active Directory, Zie [rol beheerdersmachtigingen in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="built-in-role-descriptions"></a>Beschrijvingen van de ingebouwde functies

De volgende tabel bevat een korte beschrijving van de ingebouwde rol. Klik op de naam van de rol voor een overzicht van `Actions`, `NotActions`, `DataActions`, en `NotDataActions` voor elke rol. Zie voor meer informatie over de betekenis van deze acties en hoe ze van toepassing op het beheer en vlakken [roldefinities voor Azure-resources begrijpen](role-definitions.md).


| Ingebouwde rol | Description |
| --- | --- |
| [Eigenaar](#owner) | Hiermee kunt u alles beheren, inclusief toegang tot bronnen. |
| [Inzender](#contributor) | Hiermee kunt u alles beheren behalve toegang tot bronnen. |
| [Lezer](#reader) | Hiermee kunt u Alles weergeven, maar geen wijzigingen aanbrengen. |
| [AcrDelete](#acrdelete) | ACR delete |
| [AcrImageSigner](#acrimagesigner) | ACR-afbeeldingsondertekenaar |
| [AcrPull](#acrpull) | ACR pull |
| [AcrPush](#acrpush) | ACR-push |
| [AcrQuarantineReader](#acrquarantinereader) | gegevenslezer voor ACR in quarantaine plaatsen |
| [AcrQuarantineWriter](#acrquarantinewriter) | ACR-quarantainegegevensschrijver |
| [Inzender voor API Management-Services](#api-management-service-contributor) | Service en de API's kunt beheren |
| [Rol Operator API Management-Service](#api-management-service-operator-role) | Kan service beheren, maar niet de API 's |
| [Rol Lezer API Management-Service](#api-management-service-reader-role) | Alleen-lezen toegang tot de service en API 's |
| [Application Insights-Onderdeelinzender](#application-insights-component-contributor) | Application Insights-onderdelen kunt beheren |
| [Application Insights Snapshot Debugger](#application-insights-snapshot-debugger) | Biedt de gebruiker toestemming voor het weergeven en downloaden van momentopnamen voor foutopsporing die zijn verzameld met Application Insights Snapshot Debugger. Houd er rekening mee dat deze machtigingen zijn niet opgenomen in de [eigenaar](#owner) of [Inzender](#contributor) rollen. |
| [Operator voor Automation](#automation-job-operator) | Maken en beheren van taken met behulp van Automation-Runbooks. |
| [Automation-Operator](#automation-operator) | Operators voor Automation kunnen starten, stoppen, onderbreken en hervatten van taken |
| [Operator voor Automation-Runbook](#automation-runbook-operator) | Eigenschappen van Runbook - om taken van het runbook te kunnen lezen. |
| [Avere Inzender](#avere-contributor) | Kan maken en beheren van een Avere vFXT-cluster. |
| [Avere Operator](#avere-operator) | Gebruikt door het Avere vFXT-cluster voor het beheren van het cluster |
| [Beheerdersrol voor Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-admin-role) | Lijst met cluster admin credential actie. |
| [Azure Kubernetes Service-Cluster-gebruikersrol](#azure-kubernetes-service-cluster-user-role) | Lijst met cluster referentie actie van de gebruiker. |
| [Azure kaarten-gegevenslezer (Preview)](#azure-maps-data-reader-preview) | Verleent toegang tot het lezen gerelateerde gegevens uit een Azure kaarten-account worden toegewezen. |
| [De eigenaar van de Azure Stack-registratie](#azure-stack-registration-owner) | U kunt Azure Stack-registraties beheren. |
| [Back-Inzender](#backup-contributor) | Kunt die u beheren kunnen geen Backup-service, maar kluizen maken en anderen toegang verlenen |
| [Back-upoperator](#backup-operator) | Hiermee kunt u back-services, behalve het verwijderen van back-up, het maken van kluizen en toegang geven tot andere beheren |
| [Back-Uplezer](#backup-reader) | Kan back-upservices weergeven, maar kan geen wijzigingen aanbrengen |
| [Facturering voor lezer](#billing-reader) | Hiermee staat u leestoegang gegeven tot factureringsgegevens |
| [BizTalk Contributor](#biztalk-contributor) | Hiermee kunt u beheren van BizTalk services, maar niet de toegang tot. |
| [Blockchain lid knooppunt toegang (Preview)](#blockchain-member-node-access-preview) | Toegang tot Blockchain knooppunten |
| [Inzender voor CDN-eindpunt](#cdn-endpoint-contributor) | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [Lezer voor CDN-eindpunt](#cdn-endpoint-reader) | CDN-eindpunten kunt weergeven, maar kan geen wijzigingen aanbrengen. |
| [Inzender voor CDN-profiel](#cdn-profile-contributor) | CDN-profielen en de bijbehorende eindpunten kunt beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [Lezer voor CDN-profiel](#cdn-profile-reader) | CDN-profielen en de bijbehorende eindpunten kunt weergeven, maar kan geen wijzigingen aanbrengen. |
| [Inzender voor klassieke netwerken](#classic-network-contributor) | Kunt u klassieke netwerken, maar niet de toegang tot beheren. |
| [Inzender voor klassieke Opslagaccounts](#classic-storage-account-contributor) | Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang tot. |
| [Klassieke opslag Account servicerol Sleuteloperator](#classic-storage-account-key-operator-service-role) | Klassieke Storage-Account sleuteloperators sleutels voor klassieke Opslagaccounts opnieuw genereren en weergeven |
| [Inzender voor klassieke virtuele machines](#classic-virtual-machine-contributor) | Hiermee kunt u beheren van klassieke virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
| [Inzender voor cognitive Services](#cognitive-services-contributor) | Hiermee kunt u maken, lezen, bijwerken, verwijderen en sleutels van Cognitive Services beheren. |
| [Gegevenslezer voor cognitive Services (Preview)](#cognitive-services-data-reader-preview) | Hiermee kunt u gegevens van de Cognitive Services lezen. |
| [Cognitive Services User](#cognitive-services-user) | Hiermee kunt u lees- en sleutels van Cognitive Services weergeven. |
| [Rol van lezer voor cosmos DB-Account](#cosmos-db-account-reader-role) | Kan Azure Cosmos DB-accountgegevens lezen. Zie [Inzender voor DocumentDB-Account](#documentdb-account-contributor) voor het beheren van Azure Cosmos DB-accounts. |
| [Cosmos DB-Operator](#cosmos-db-operator) | U kunt Azure Cosmos DB-accounts, maar geen toegang tot gegevens in deze beheren. Voorkomt toegang tot sleutels en verbindingsreeksen. |
| [CosmosBackupOperator](#cosmosbackupoperator) | Restore-aanvraag voor een Cosmos DB-database of een container voor een account kunt indienen |
| [Inzender voor kostenbeheer](#cost-management-contributor) | Kunt u kosten wilt weergeven en beheren van kosten configuratie (bijvoorbeeld budgetten, uitvoer) |
| [Kostenbeheer-lezer](#cost-management-reader) | Kan gegevens van cost en configuratie (bijvoorbeeld budgetten, uitvoer) weergeven |
| [Inzender voor Data Box](#data-box-contributor) | Hiermee beheert u alles onder de Data Box-Service, behalve het verlenen van toegang aan anderen. |
| [Data Box-lezer](#data-box-reader) | U kunt Data Box-Service met uitzondering van het maken van de volgorde of ordergegevens bewerken en toegang geven tot andere beheren. |
| [Inzender Data Factory](#data-factory-contributor) | Maken en beheren van data factory's, evenals de onderliggende resources hierin. |
| [Data Lake Analytics-ontwikkelaar](#data-lake-analytics-developer) | Hiermee kunt u indienen, controleren, en uw eigen taken beheren, maar niet maken of verwijderen van Data Lake Analytics-accounts. |
| [Data Purger](#data-purger) | Analytics-gegevens kunt opschonen |
| [DevTest Labs User](#devtest-labs-user) | Hiermee kunt u verbinding maakt, starten, opnieuw opstarten en afsluiten van uw virtuele machines in Azure DevTest Labs. |
| [Inzender voor DNS-Zone](#dns-zone-contributor) | Hiermee kunt u DNS-zones en -recordsets in Azure DNS beheren, maar kunt u bepalen wie toegang heeft tot deze niet. |
| [Inzender voor het DocumentDB-Account](#documentdb-account-contributor) | Kan Azure Cosmos DB-accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
| [Event Hubs Data Owner](#event-hubs-data-owner) | Volledige toegang tot de Azure Event Hubs-bronnen | 
| [EventGrid EventSubscription Inzender](#eventgrid-eventsubscription-contributor) | Kunt u bewerkingen in het abonnement EventGrid gebeurtenis beheren. |
| [EventGrid EventSubscription lezer](#eventgrid-eventsubscription-reader) | Hiermee kunt u gebeurtenisabonnementen EventGrid lezen. |
| [HDInsight-Cluster-Operator](#hdinsight-cluster-operator) | Hiermee kunt u lezen en wijzigen van configuraties van clusters op HDInsight. |
| [HDInsight Domain Services-Inzender](#hdinsight-domain-services-contributor) | Kan lezen, maken, wijzigen en verwijderen van domeinservices gerelateerde bewerkingen die nodig zijn voor HDInsight Enterprise-beveiligingspakket |
| [Inzender voor het Account van de intelligente systemen](#intelligent-systems-account-contributor) | Kunt u Intelligent Systems-accounts, maar niet de toegang tot beheren. |
| [Inzender voor Key Vault](#key-vault-contributor) | Hiermee kunt u beheren van sleutelkluizen, maar niet de toegang tot. |
| [Labmaker](#lab-creator) | Hiermee kunt u maken, beheren en verwijderen van uw beheerde labs onder uw Azure Lab-Accounts. |
| [Inzender van log Analytics](#log-analytics-contributor) | Inzender van log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources. |
| [Lezer van log Analytics](#log-analytics-reader) | Lezer van log Analytics kunt bekijken en zoeken van alle bewakingsgegevens en de controle-instellingen, inclusief het weergeven van de configuratie van Azure diagnostics op alle Azure-resources weergeven. |
| [Logische App-bijdrager](#logic-app-contributor) | U kunt logische app, maar niet de toegang tot beheren. |
| [Logische App-Operator](#logic-app-operator) | Hiermee kunt u lezen, inschakelen en uitschakelen van de logische app. |
| [De Operatorrol beheerde toepassing](#managed-application-operator-role) | Hiermee kunt u lees- en acties uitvoeren op resources van beheerde toepassingen |
| [Beheerde toepassingen-lezer](#managed-applications-reader) | Hiermee kunt u lezen van resources in een beheerde app en de aanvraag voor JIT-toegang. |
| [Inzender beheerde identiteit](#managed-identity-contributor) | Maken, lezen, bijwerken en verwijderen van door gebruiker toegewezen identiteit |
| [Operator beheerde identiteit](#managed-identity-operator) | Maken en toewijzen van door gebruiker toegewezen identiteit |
| [Beheergroep-Inzender](#management-group-contributor) | De rol Inzender beheergroep |
| [Lezer van de beheergroep](#management-group-reader) | Lezerrol voor de beheergroep |
| [Controlebijdrager](#monitoring-contributor) | Kan alle controlegegevens lezen en bewerken van instellingen voor controle. Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
| [Uitgever van de metrische gegevens controleren](#monitoring-metrics-publisher) | Hiermee schakelt het publiceren van metrische gegevens bij Azure-resources |
| [Controlelezer](#monitoring-reader) | Kan alle controlegegevens lezen (metrische gegevens, Logboeken, enz.). Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
| [Inzender voor netwerken](#network-contributor) | Kunt u netwerken, maar niet de toegang tot beheren. |
| [Nieuwe Relic APM-Account Inzender](#new-relic-apm-account-contributor) | U kunt nieuwe Relic Application Performance Management-accounts en -toepassingen, maar niet de toegang tot beheren. |
| [Lezer en toegang tot gegevens](#reader-and-data-access) | Hiermee kunt u Alles weergeven, maar kunt u verwijderen of te maken van een storage-account of een ingesloten resource niet. Er kunnen ook toegang tot alle gegevens in een opslagaccount verleend via toegang tot opslagaccountsleutels lezen/schrijven. |
| [Redis-Cache-Inzender](#redis-cache-contributor) | U kunt Redis-caches, maar niet de toegang tot beheren. |
| [Inzender voor Resourcebeleid (Preview)](#resource-policy-contributor-preview) | (Preview) Supportticket gebruikers gevuld vanuit EA, met machtigingen voor het maken/wijzigen voor resourcebeleid, maken en het lezen van resources/hiërarchie. |
| [Inzender voor Scheduler-taak](#scheduler-job-collections-contributor) | Hiermee kunt u beheren Scheduler-taakverzamelingen, maar niet de toegang tot. |
| [Inzender voor Search-Services](#search-service-contributor) | U kunt Search-services, maar niet de toegang tot beheren. |
| [Beveiligingsbeheerder](#security-admin) | In Security Center: Kan weergeven beveiligingsbeleid, security-status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen |
| [Beveiligingsbeheer (verouderd)](#security-manager-legacy) | Dit is een verouderde rol. Gebruik in plaats hiervan beveiligingsbeheerder |
| [Beveiligingslezer](#security-reader) | In Security Center: Aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven |
| [De eigenaar van een service Bus-gegevens](#service-bus-data-owner) | Volledige toegang tot Azure Service Bus-bronnen |
| [Site Recovery-Inzender](#site-recovery-contributor) | Hiermee kunt u Site Recovery-service, met uitzondering van het maken van kluizen en roltoewijzing beheren |
| [Site Recovery-Operator](#site-recovery-operator) | Hiermee kunt u failover en failback maar geen andere beheerbewerkingen voor Site Recovery uitvoeren |
| [Site Recovery-lezer](#site-recovery-reader) | Hiermee kunt u Site Recovery-status weergeven maar geen andere beheerbewerkingen uitvoeren |
| [Inzender voor het Account van de ruimtelijke ankers](#spatial-anchors-account-contributor) | Hiermee kunt u ruimtelijke ankers beheren in uw account, maar ze niet verwijderen |
| [De accounteigenaar ruimtelijke ankers](#spatial-anchors-account-owner) | Hiermee kunt u beheren ruimtelijke ankers in uw account, met inbegrip van deze worden verwijderd |
| [Ruimtelijke ankers Account Reader](#spatial-anchors-account-reader) | U kunt zoeken en lezen van de eigenschappen van ruimtelijke ankers in uw account |
| [' SQL DB Contributor '](#sql-db-contributor) | Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beveiligingsbeleid of de bovenliggende SQL-servers. |
| [SQL beheerd exemplaar Inzender](#sql-managed-instance-contributor) | Kunt u beheerde SQL-instanties beheren en de vereiste netwerkconfiguratie, maar kunnen geen toegang verlenen aan anderen. |
| [SQL Security Manager](#sql-security-manager) | U kunt het beveiligingsbeleid van SQL-servers en databases, maar niet de toegang tot beheren. |
| [Inzender voor SQL Server](#sql-server-contributor) | Hiermee kunt u SQL-servers en databases beheren, maar niet de toegang tot en het beveiligingsbeleid-beleidsregels die betrekking hebben. |
| [Inzender voor opslagaccounts](#storage-account-contributor) | Hiermee kunt u opslagaccounts beheren, maar niet de toegang tot. |
| [Storage-Account servicerol Sleuteloperator](#storage-account-key-operator-service-role) | Storage-Account sleuteloperators sleutels van Opslagaccounts opnieuw genereren en weergeven |
| [Gegevensbijdrager voor Blob](#storage-blob-data-contributor) | Kan voor lezen, schrijven en toegang tot Azure Storage-blobcontainers en -gegevens verwijderen |
| [De eigenaar van een opslag-Blob-gegevens](#storage-blob-data-owner) | Kunt u volledige toegang tot Azure Storage-blobcontainers en gegevens, zoals het toewijzen van POSIX-toegangsbeheer. |
| [Gegevenslezer voor Opslagblob](#storage-blob-data-reader) | Kunt u leestoegang tot Azure Storage-blobcontainers en -gegevens |
| [Gegevensbijdrager voor wachtrij](#storage-queue-data-contributor) | Kunt u lees-, schrijf- en verwijdertoegang tot Azure Storage-wachtrijen en -Wachtrijberichten |
| [Storage Queue Gegevensverwerker bericht](#storage-queue-data-message-processor) | Kan voor peek, ontvangen en verwijdertoegang tot Azure Storage-berichtenwachtrij-berichten |
| [Storage Queue gegevens afzender](#storage-queue-data-message-sender) | Staat het verzenden van berichten van Azure Storage-wachtrij |
| [Gegevenslezer voor Opslagwachtrij](#storage-queue-data-reader) | Kunt u leestoegang tot Azure Storage-wachtrijen en -Wachtrijberichten |
| [Inzender voor ondersteuningsaanvragen](#support-request-contributor) | U kunt maken en ondersteuningsaanvragen beheren |
| [Inzender voor Traffic Manager](#traffic-manager-contributor) | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u bepalen wie toegang heeft tot deze niet. |
| [Beheerder van gebruikerstoegang](#user-access-administrator) | Hiermee kunt u gebruikerstoegang tot Azure-resources beheren. |
| [Beheerdersaanmelding bij virtuele Machine](#virtual-machine-administrator-login) | Virtuele Machines weergeven in de portal en meld u aan als beheerder |
| [Inzender voor virtuele machines](#virtual-machine-contributor) | Hiermee kunt u beheren van virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
| [Gebruikersaanmelding bij virtuele Machine](#virtual-machine-user-login) | Virtuele Machines in de portal en meld u aan als een gewone gebruiker weergeven. |
| [Inzender voor webabonnementen](#web-plan-contributor) | Kunt u de webabonnementen voor websites, maar niet de toegang om deze te beheren. |
| [Inzender voor websites](#website-contributor) | Kunt u websites (niet webabonnementen), maar niet de toegang tot beheren. |


## <a name="owner"></a>Eigenaar
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u alles beheren, inclusief toegang tot bronnen. |
> | **Id** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Acties** |  |
> | * | Maken en beheren van resources van alle typen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="contributor"></a>Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u alles beheren behalve toegang tot bronnen. |
> | **Id** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Acties** |  |
> | * | Maken en beheren van resources van alle typen |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Rollen en roltoewijzingen verwijderen |
> | Microsoft.Authorization/*/Write | Rollen en roltoewijzingen maken |
> | Microsoft.Authorization/elevateAccess/Action | De oproepende functie verleent toegang bij het tenantbereik Administrator voor gebruikerstoegang |
> | Microsoft.Blueprint/blueprintAssignments/write | Maken of bijwerken van alle blauwdrukartefacten |
> | Microsoft.Blueprint/blueprintAssignments/delete | Alle blauwdrukartefacten verwijderen |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="reader"></a>Lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Alles weergeven, maar geen wijzigingen aanbrengen. |
> | **Id** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrdelete"></a>AcrDelete
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | ACR delete |
> | **Id** | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/artifacts/delete | Verwijder artefact in een containerregister. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | ACR-afbeeldingsondertekenaar |
> | **Id** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | Push of eruit te halen de metagegevens van inhoud vertrouwensrelatie voor een container registry. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrpull"></a>AcrPull
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | ACR pull |
> | **Id** | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Pull- of installatiekopieën ophalen uit een containerregister. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrpush"></a>AcrPush
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | ACR-push |
> | **Id** | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Pull- of installatiekopieën ophalen uit een containerregister. |
> | Microsoft.ContainerRegistry/registries/push/write | Push- of installatiekopieën schrijven naar een containerregister. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | gegevenslezer voor ACR in quarantaine plaatsen |
> | **Id** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/quarantineRead/read | Pull- of in quarantaine geplaatste afbeeldingen kunt verkrijgen van containerregister |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | ACR-quarantainegegevensschrijver |
> | **Id** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/quarantineRead/read | Pull- of in quarantaine geplaatste afbeeldingen kunt verkrijgen van containerregister |
> | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Status van de in quarantaine plaatsen van in quarantaine geplaatste afbeeldingen schrijven/wijzigen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="api-management-service-contributor"></a>Inzender voor API Management-Services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Service en de API's kunt beheren |
> | **Id** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/* | Maken en beheren van API Management-service |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="api-management-service-operator-role"></a>Rol Operator API Management-Service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan service beheren, maar niet de API 's |
> | **Id** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/*/read | Lezen API Management Service-exemplaren |
> | Microsoft.ApiManagement/service/backup/action | Back-API Management-Service voor de opgegeven container in een gebruiker opgegeven opslagaccount |
> | Microsoft.ApiManagement/service/delete | API Management-Service-exemplaar verwijderen |
> | Microsoft.ApiManagement/service/managedeployments/action | Wijzigen van SKU /-eenheden, regionale implementaties van API Management-Service toevoegen/verwijderen |
> | Microsoft.ApiManagement/service/read | Metagegevens voor een exemplaar van API Management-Service lezen |
> | Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in een door de gebruiker opgegeven storage-account herstellen |
> | Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
> | Microsoft.ApiManagement/service/updatehostname/action | Installeren, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
> | Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Sleutels die zijn gekoppeld aan een gebruiker ophalen |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="api-management-service-reader-role"></a>Rol Lezer API Management-Service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Alleen-lezen toegang tot de service en API 's |
> | **Id** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/*/read | Lezen API Management Service-exemplaren |
> | Microsoft.ApiManagement/service/read | Metagegevens voor een exemplaar van API Management-Service lezen |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Sleutels die zijn gekoppeld aan een gebruiker ophalen |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="application-insights-component-contributor"></a>Application Insights-Onderdeelinzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Application Insights-onderdelen kunt beheren |
> | **Id** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Insights/components/* | Maken en beheren van inzicht onderdelen |
> | Microsoft.Insights/webtests/* | Maken en beheren van webtests |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Biedt de gebruiker toestemming voor het weergeven en downloaden van momentopnamen voor foutopsporing die zijn verzameld met Application Insights Snapshot Debugger. Houd er rekening mee dat deze machtigingen zijn niet opgenomen in de [eigenaar](#owner) of [Inzender](#contributor) rollen. |
> | **Id** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="automation-job-operator"></a>Operator voor Automation
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Maken en beheren van taken met behulp van Automation-Runbooks. |
> | **Id** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak wordt hervat |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Een Azure Automation-taak gestopt |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation-taak onderbroken |
> | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="automation-operator"></a>Automation-operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Operators voor Automation kunnen starten, stoppen, onderbreken en hervatten van taken |
> | **Id** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak wordt hervat |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Een Azure Automation-taak gestopt |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation-taak onderbroken |
> | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Een Azure Automation-taakschema opgehaald |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation-taakschema |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
> | Microsoft.Automation/automationAccounts/read | Een Azure Automation-account opgehaald |
> | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Microsoft.Automation/automationAccounts/schedules/read | Een Azure Automation-planningsasset opgehaald |
> | Microsoft.Automation/automationAccounts/schedules/write | Hiermee maakt of een Azure Automation-planningsasset werkt |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="automation-runbook-operator"></a>Operator voor Automation-Runbook
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Eigenschappen van Runbook - om taken van het runbook te kunnen lezen. |
> | **Id** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="avere-contributor"></a>Avere Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan maken en beheren van een Avere vFXT-cluster. |
> | **Id** | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Compute/*/read |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft.Compute/disks/* |  |
> | Microsoft.Network/*/read |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/virtualNetworks/subnets/read | De definitie van een virtueel netwerk subnet opgehaald |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk. Niet Signaleerbare. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. Niet Signaleerbare. |
> | Microsoft.Network/networkSecurityGroups/join/action | Een netwerkbeveiligingsgroep koppelt. Niet Signaleerbare. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/*/read |  |
> | Microsoft.Storage/storageAccounts/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Hiermee haalt de resources voor de resourcegroep. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Retourneert het resultaat van het verwijderen van een blob |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Retourneert een blob of een lijst met blobs |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Retourneert het resultaat van het schrijven van een blob |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="avere-operator"></a>Avere Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Gebruikt door het Avere vFXT-cluster voor het beheren van het cluster |
> | **Id** | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | **Acties** |  |
> | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Network/networkInterfaces/write | Hiermee maakt u een netwerkinterface gemaakt of bijgewerkt van een bestaande netwerkinterface.  |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/virtualNetworks/subnets/read | De definitie van een virtueel netwerk subnet opgehaald |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk. Niet Signaleerbare. |
> | Microsoft.Network/networkSecurityGroups/join/action | Een netwerkbeveiligingsgroep koppelt. Niet Signaleerbare. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Retourneert het resultaat van het verwijderen van een container |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Retourneert lijst met containers |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Retourneert het resultaat van put blob-container |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Retourneert het resultaat van het verwijderen van een blob |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Retourneert een blob of een lijst met blobs |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Retourneert het resultaat van het schrijven van een blob |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="azure-kubernetes-service-cluster-admin-role"></a>Beheerdersrol voor Azure Kubernetes Service-Cluster
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lijst met cluster admin credential actie. |
> | **Id** | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | **Acties** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Lijst van de referentie clusterAdmin van een beheerde cluster |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes Service-Cluster-gebruikersrol
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lijst met cluster referentie actie van de gebruiker. |
> | **Id** | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **Acties** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Lijst van de referentie clusterUser van een beheerde cluster |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="azure-maps-data-reader-preview"></a>Azure kaarten-gegevenslezer (Preview)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Verleent toegang tot het lezen gerelateerde gegevens uit een Azure kaarten-account worden toegewezen. |
> | **Id** | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Maps/accounts/data/read | Verleent toegang tot het lezen van gegevens naar een maps-account. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="azure-stack-registration-owner"></a>De eigenaar van de Azure Stack-registratie
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Azure Stack-registraties beheren. |
> | **Id** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Acties** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Details voor een Azure Stack Marketplace-product uitgebreid opgehaald |
> | Microsoft.AzureStack/registrations/products/read | Haalt u de eigenschappen van een Azure Stack Marketplace-product |
> | Microsoft.AzureStack/registrations/read | Haalt u de eigenschappen van een Azure Stack-registratie |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="backup-contributor"></a>Back-Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt die u beheren kunnen geen Backup-service, maar kluizen maken en anderen toegang verlenen |
> | **Id** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Resultaten van de bewerking op de back-upbeheer beheren |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Back-containers in de back-fabrics van Recovery Services-kluis maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de containerlijst |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Maken en beheren van meta-gegevens met betrekking tot back-upbeheer |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van back-upbeheer bewerkingen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Back-beleid maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Artikelen die u kunnen een back-up maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Maken en beheren van back-ups van items |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Maken en beheren van containers met back-upitems |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Maken en beheren van certificaten met betrekking tot back-up in Recovery Services-kluis |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie met betrekking tot de kluis |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van geregistreerde identiteiten |
> | Microsoft.RecoveryServices/Vaults/usages/* | Maken en beheren van het gebruik van Recovery Services-kluis |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Bewerking op de beveiligde Item valideren |
> | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Retourneert de back-upbewerking Status voor de Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Alle beveiligbare containers ophalen |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Controleer back-upstatus voor Recovery Services-kluizen |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Functies valideren |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Oplossing voor de waarschuwing. |
> | Microsoft.RecoveryServices/operations/read | Bewerking wordt de lijst met bewerkingen voor een Resourceprovider geretourneerd |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Status van de bewerking voor een bepaalde bewerking opgehaald |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Lijst van alle back-up Protection Intents |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="backup-operator"></a>Back-upoperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u back-services, behalve het verwijderen van back-up, het maken van kluizen en toegang geven tot andere beheren |
> | **Id** | 00c29273-979b-4161-815c-10b084fb9324 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Status van de bewerking geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hiermee wordt een back-up van het beveiligde item gemaakt. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Direct Itemherstel inrichten voor beveiligd Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Herstelpunten voor beveiligde items herstellen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Direct Itemherstel intrekken voor beveiligd Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de containerlijst |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read |  |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van back-upbeheer bewerkingen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Artikelen die u kunnen een back-up maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers van het abonnement |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking servicecontainer registreren kan worden gebruikt voor het registreren van een container met de Recovery-Service. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Bewerking op de beveiligde Item valideren |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Retourneert de back-upbewerking Status voor de Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Status van de beleidsbewerking ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Hiermee maakt u een geregistreerde container |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Workloads in een container navragen |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Een back-Upbeveiligingsintentie maken |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Een back-Upbeveiligingsintentie ophalen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Alle beveiligbare containers ophalen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Alle items in een container ophalen |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Controleer back-upstatus voor Recovery Services-kluizen |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Functies valideren |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Oplossing voor de waarschuwing. |
> | Microsoft.RecoveryServices/operations/read | Bewerking wordt de lijst met bewerkingen voor een Resourceprovider geretourneerd |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Status van de bewerking voor een bepaalde bewerking opgehaald |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Lijst van alle back-up Protection Intents |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="backup-reader"></a>Back-Uplezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan back-upservices weergeven, maar kan geen wijzigingen aanbrengen |
> | **Id** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Status van de bewerking geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Alle taakobjecten geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read |  |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read |  |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers van het abonnement |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Retourneert de opslagconfiguratie voor Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Retourneert-configuratie voor de Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Retourneert de back-upbewerking Status voor de Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Status van de beleidsbewerking ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Hiermee worden alle servers voor back-upbeheer geretourneerd die in de kluis zijn geregistreerd. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Een back-Upbeveiligingsintentie ophalen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Alle items in een container ophalen |
> | Microsoft.RecoveryServices/locations/backupStatus/action | Controleer back-upstatus voor Recovery Services-kluizen |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Oplossing voor de waarschuwing. |
> | Microsoft.RecoveryServices/operations/read | Bewerking wordt de lijst met bewerkingen voor een Resourceprovider geretourneerd |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Status van de bewerking voor een bepaalde bewerking opgehaald |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Lijst van alle back-up Protection Intents |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="billing-reader"></a>Lezer voor facturering
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee staat u leestoegang gegeven tot factureringsgegevens |
> | **Id** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Billing/*/read | Facturering-gegevens lezen |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="biztalk-contributor"></a>BizTalk Contributor
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren van BizTalk services, maar niet de toegang tot. |
> | **Id** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.BizTalkServices/BizTalk/* | BizTalk services maken en beheren |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="blockchain-member-node-access-preview"></a>Blockchain lid knooppunt toegang (Preview)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Toegang tot Blockchain knooppunten |
> | **Id** | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **Acties** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/read | Opgehaald of een lijst met bestaande Blockchain lid transactie knooppunten. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action | Maakt verbinding met een lid van de Blockchain-transactie-knooppunt. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cdn-endpoint-contributor"></a>Inzender voor CDN-eindpunt
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
> | **Id** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cdn-endpoint-reader"></a>Lezer voor CDN-eindpunt
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | CDN-eindpunten kunt weergeven, maar kan geen wijzigingen aanbrengen. |
> | **Id** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cdn-profile-contributor"></a>Inzender voor CDN-profiel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | CDN-profielen en de bijbehorende eindpunten kunt beheren, maar kan geen toegang verlenen aan andere gebruikers. |
> | **Id** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cdn-profile-reader"></a>Lezer voor CDN-profiel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | CDN-profielen en de bijbehorende eindpunten kunt weergeven, maar kan geen wijzigingen aanbrengen. |
> | **Id** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="classic-network-contributor"></a>Inzender voor klassieke netwerken
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u klassieke netwerken, maar niet de toegang tot beheren. |
> | **Id** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicNetwork/* | Maken en beheren van klassieke netwerken |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="classic-storage-account-contributor"></a>Inzender voor klassieke Opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang tot. |
> | **Id** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicStorage/storageAccounts/* | Opslagaccounts maken en beheren |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="classic-storage-account-key-operator-service-role"></a>Klassieke opslag Account servicerol Sleuteloperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Klassieke Storage-Account sleuteloperators sleutels voor klassieke Opslagaccounts opnieuw genereren en weergeven |
> | **Id** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Acties** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Geeft een lijst van de toegangssleutels voor de storage-accounts. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Genereert opnieuw de bestaande toegangssleutels voor het opslagaccount. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="classic-virtual-machine-contributor"></a>Inzender voor klassieke virtuele machines
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren van klassieke virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
> | **Id** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicCompute/domainNames/* | Maken en beheren van domeinnamen van klassieke berekening |
> | Microsoft.ClassicCompute/virtualMachines/* | Virtuele machines maken en beheren |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde Ip koppelen |
> | Microsoft.ClassicNetwork/reservedIps/read | De gereserveerde IP-adressen opgehaald |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Lid wordt van het virtuele netwerk. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Ophalen van het virtuele netwerk. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Retourneert de installatiekopie van het opslagaccount. (Afgeschaft. Use 'Microsoft.ClassicStorage/storageAccounts/vmImages') |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Geeft een lijst van de toegangssleutels voor de storage-accounts. |
> | Microsoft.ClassicStorage/storageAccounts/read | Retourneert de storage-account met het opgegeven account. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cognitive-services-contributor"></a>Inzender voor cognitive Services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u maken, lezen, bijwerken, verwijderen en sleutels van Cognitive Services beheren. |
> | **Id** | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Features/features/read | Hiermee haalt u de functies van een abonnement. |
> | Microsoft.Features/providers/features/read | Hiermee haalt de functie van een abonnement in een opgegeven resourceprovider. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of de diagnostische instelling voor analyseserver lezen |
> | Microsoft.Insights/logDefinitions/read | Logboekdefinities lezen |
> | Microsoft.Insights/metricdefinitions/read | Metrische definities lezen |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Microsoft.Resources/subscriptions/operationresults/read | Resultaten van de bewerking voor het abonnement ophalen. |
> | Microsoft.Resources/subscriptions/read | Hiermee haalt u de lijst met abonnementen. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cognitive-services-data-reader-preview"></a>Gegevenslezer voor cognitive Services (Preview)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u gegevens van de Cognitive Services lezen. |
> | **Id** | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cognitive-services-user"></a>Cognitive Services-gebruiker
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lees- en sleutels van Cognitive Services weergeven. |
> | **Id** | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Acties** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Een lijst met sleutels |
> | Microsoft.Insights/alertRules/read | Een klassieke waarschuwing voor metrische gegevens lezen |
> | Microsoft.Insights/diagnosticSettings/read | De diagnostische instelling van een resource lezen |
> | Microsoft.Insights/logDefinitions/read | Logboekdefinities lezen |
> | Microsoft.Insights/metricdefinitions/read | Metrische definities lezen |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Microsoft.Resources/subscriptions/operationresults/read | Resultaten van de bewerking voor het abonnement ophalen. |
> | Microsoft.Resources/subscriptions/read | Hiermee haalt u de lijst met abonnementen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cosmos-db-account-reader-role"></a>Rol van lezer voor cosmos DB-Account
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan Azure Cosmos DB-accountgegevens lezen. Zie [Inzender voor DocumentDB-Account](#documentdb-account-contributor) voor het beheren van Azure Cosmos DB-accounts. |
> | **Id** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen zijn toegewezen, kan machtigingen die aan elke gebruiker lezen |
> | Microsoft.DocumentDB/*/read | Een verzameling lezen |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Leest de database alleen-lezen-accountsleutels. |
> | Microsoft.Insights/MetricDefinitions/read | Metrische definities lezen |
> | Microsoft.Insights/Metrics/read | De metrische gegevens lezen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cosmos-db-operator"></a>Cosmos DB-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Azure Cosmos DB-accounts, maar geen toegang tot gegevens in deze beheren. Voorkomt toegang tot sleutels en verbindingsreeksen. |
> | **Id** | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | **Acties** |  |
> | Microsoft.DocumentDb/databaseAccounts/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cosmosbackupoperator"></a>CosmosBackupOperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Restore-aanvraag voor een Cosmos DB-database of een container voor een account kunt indienen |
> | **Id** | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | **Acties** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | Een aanvraag indient bij de back-up configureren |
> | Microsoft.DocumentDB/databaseAccounts/restore/action | Een restore-aanvraag verzenden |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cost-management-contributor"></a>Inzender voor kostenbeheer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u kosten wilt weergeven en beheren van kosten configuratie (bijvoorbeeld budgetten, uitvoer) |
> | **Id** | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | **Acties** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingPeriods/read | Een lijst met beschikbare factureringsperioden |
> | Microsoft.Resources/subscriptions/read | Hiermee haalt u de lijst met abonnementen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Advisor/configurations/read | Configuraties ophalen |
> | Microsoft.Advisor/recommendations/read | Aanbevelingen voor leesbewerkingen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="cost-management-reader"></a>Kostenbeheer-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan gegevens van cost en configuratie (bijvoorbeeld budgetten, uitvoer) weergeven |
> | **Id** | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | **Acties** |  |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.CostManagement/*/read |  |
> | Microsoft.Billing/billingPeriods/read | Een lijst met beschikbare factureringsperioden |
> | Microsoft.Resources/subscriptions/read | Hiermee haalt u de lijst met abonnementen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Advisor/configurations/read | Configuraties ophalen |
> | Microsoft.Advisor/recommendations/read | Aanbevelingen voor leesbewerkingen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="data-box-contributor"></a>Inzender voor Data Box
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u alles onder de Data Box-Service, behalve het verlenen van toegang aan anderen. |
> | **Id** | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="data-box-reader"></a>Data Box-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Data Box-Service met uitzondering van het maken van de volgorde of ordergegevens bewerken en toegang geven tot andere beheren. |
> | **Id** | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | Geeft een lijst van de niet-versleutelde referenties met betrekking tot de order. |
> | Microsoft.Databox/locations/availableSkus/action | Deze methode retourneert de lijst met beschikbare SKU's. |
> | Microsoft.Databox/locations/validateAddress/action | Hiermee wordt het verzendadres gevalideerd en biedt alternatieve adressen indien van toepassing. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="data-factory-contributor"></a>Inzender Data Factory
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Maken en beheren van data factory's, evenals de onderliggende resources hierin. |
> | **Id** | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.DataFactory/dataFactories/* | Maken en beheren van data factory's en onderliggende resources hierin. |
> | Microsoft.DataFactory/factories/* | Maken en beheren van data factory's en onderliggende resources hierin. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="data-lake-analytics-developer"></a>Data Lake Analytics-ontwikkelaar
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u indienen, controleren, en uw eigen taken beheren, maar niet maken of verwijderen van Data Lake Analytics-accounts. |
> | **Id** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Een account DataLakeAnalytics verwijderen. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen voor het annuleren van de taken die worden ingediend door andere gebruikers. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Maken of bijwerken van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Een DataLakeStore-account loskoppelen van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Een Storage-account loskoppelen van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Maken of bijwerken van een firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Een firewallregel verwijderen. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Maken of bijwerken van een compute-beleid. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Een compute-beleid niet verwijderen. |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="data-purger"></a>Gegevens Purger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Analytics-gegevens kunt opschonen |
> | **Id** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Acties** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Opschonen van gegevens uit Application Insights |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Opgegeven gegevens uit de werkruimte verwijderen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="devtest-labs-user"></a>DevTest Labs User
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u verbinding maakt, starten, opnieuw opstarten en afsluiten van uw virtuele machines in Azure DevTest Labs. |
> | **Id** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset |
> | Microsoft.Compute/virtualMachines/*/read | Lezen van de eigenschappen van een virtuele machine (VM-grootten, runtimestatus, VM-extensies, enz.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine en worden de rekenresources vrijgegeven uitgeschakeld |
> | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine |
> | Microsoft.Compute/virtualMachines/restart/action | De virtuele machine opnieuw wordt opgestart |
> | Microsoft.Compute/virtualMachines/start/action | De virtuele machine gestart |
> | Microsoft.DevTestLab/*/read | De eigenschappen van een lab lezen |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Claim een willekeurige claimbare virtuele machine in het lab. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Virtuele machines in een lab maken. |
> | Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action | Zorg ervoor dat de huidige gebruiker heeft een geldig profiel in het lab. |
> | Microsoft.DevTestLab/labs/formulas/delete | Verwijderen van formules. |
> | Microsoft.DevTestLab/labs/formulas/read | Formules worden gelezen. |
> | Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Lab-beleid evalueert. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Eigenaar worden van een bestaande virtuele machine |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | Geeft een lijst van de planning van toepassing starten/stoppen, indien van toepassing. |
> | Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action | Een tekenreeks waarmee de inhoud van het RDP-bestand voor de virtuele machine opgehaald |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een load balancer-back-end-adresgroep. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel. Niet Signaleerbare. |
> | Microsoft.Network/networkInterfaces/*/read | Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle load balancers die de netwerkinterface een deel van is) |
> | Microsoft.Network/networkInterfaces/join/action | Een virtuele Machine koppelt aan een netwerkinterface. Niet Signaleerbare. |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Network/networkInterfaces/write | Hiermee maakt u een netwerkinterface gemaakt of bijgewerkt van een bestaande netwerkinterface.  |
> | Microsoft.Network/publicIPAddresses/*/read | Lezen van de eigenschappen van een openbaar IP-adres |
> | Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare ip-adres. Niet Signaleerbare. |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk. Niet Signaleerbare. |
> | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Microsoft.Resources/deployments/read | Opgehaald of een lijst met implementaties. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Een lijst met beschikbare grootten die de virtuele machine kan worden bijgewerkt |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="dns-zone-contributor"></a>Inzender voor DNS-Zone
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u DNS-zones en -recordsets in Azure DNS beheren, maar kunt u bepalen wie toegang heeft tot deze niet. |
> | **Id** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/dnsZones/* | DNS-zones en records maken en beheren |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="documentdb-account-contributor"></a>Inzender voor het DocumentDB-Account
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan Azure Cosmos DB-accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
> | **Id** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.DocumentDb/databaseAccounts/* | Maken en beheren van Azure Cosmos DB-accounts |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="event-hubs-data-owner"></a>Eigenaar van gegevens van Event Hubs

> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u volledige toegang tot Azure Event Hubs-bronnen. |
> | **Id** | f526a384-b230-433a-b45c-95f59c4a2dec |
> | **Acties** |  |
> | Microsoft.EventHubs/* | Volledig beheer toegang tot de Event Hubs-naamruimte |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.EventHubs/* | Volledige gegevens toegang tot de Event Hubs-naamruimte |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u bewerkingen in het abonnement EventGrid gebeurtenis beheren. |
> | **Id** | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Lijst met algemene gebeurtenisabonnementen op Onderwerptype |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen door topictype |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u gebeurtenisabonnementen EventGrid lezen. |
> | **Id** | 2414bbcf-6497-4faf-8c65-045460748405 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.EventGrid/eventSubscriptions/read | Een eventSubscription lezen |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Lijst met algemene gebeurtenisabonnementen op Onderwerptype |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Lijst met regionale gebeurtenisabonnementen door topictype |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="hdinsight-cluster-operator"></a>HDInsight-Cluster-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lezen en wijzigen van configuraties van clusters op HDInsight. |
> | **Id** | 61ed4efc-fab3-44fd-b111-e24485cc132a |
> | **Acties** |  |
> | Microsoft.HDInsight/*/read |  |
> | Microsoft.HDInsight/clusters/getGatewaySettings/action | Gatewayinstellingen voor HDInsight-Cluster ophalen |
> | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Gatewayinstellingen bijwerken voor HDInsight-Cluster |
> | Microsoft.HDInsight/clusters/configurations/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="hdinsight-domain-services-contributor"></a>HDInsight Domain Services-Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan lezen, maken, wijzigen en verwijderen van domeinservices gerelateerde bewerkingen die nodig zijn voor HDInsight Enterprise-beveiligingspakket |
> | **Id** | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | **Acties** |  |
> | Microsoft.AAD/*/read |  |
> | Microsoft.AAD/domainServices/*/read |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="intelligent-systems-account-contributor"></a>Inzender voor het Account van de intelligente systemen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u Intelligent Systems-accounts, maar niet de toegang tot beheren. |
> | **Id** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.IntelligentSystems/accounts/* | Bouw intelligente systemen rekeningen maken en beheren |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="key-vault-contributor"></a>Inzender voor Key Vault
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren van sleutelkluizen, maar niet de toegang tot. |
> | **Id** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sleutelkluis leegmaken |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="lab-creator"></a>Labmaker
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u maken, beheren en verwijderen van uw beheerde labs onder uw Azure Lab-Accounts. |
> | **Id** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Een lab maken in een lab-account. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Informatie over de regionale beschikbaarheid voor elke grootte categorie geconfigureerd onder een lab-account ophalen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="log-analytics-contributor"></a>Inzender van Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Inzender van log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources. |
> | **Id** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Geeft een lijst van de toegangssleutels voor de storage-accounts. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of de diagnostische instelling voor analyseserver lezen |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="log-analytics-reader"></a>Lezer van Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lezer van log Analytics kunt bekijken en zoeken van alle bewakingsgegevens en de controle-instellingen, inclusief het weergeven van de configuratie van Azure diagnostics op alle Azure-resources weergeven. |
> | **Id** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoeken met de nieuwe engine. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="logic-app-contributor"></a>Logische App-bijdrager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt logische app, maar niet de toegang tot beheren. |
> | **Id** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Geeft een lijst van de toegangssleutels voor de storage-accounts. |
> | Microsoft.ClassicStorage/storageAccounts/read | Retourneert de storage-account met het opgegeven account. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of de diagnostische instelling voor analyseserver lezen |
> | Microsoft.Insights/logdefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Logboekcategorieën lijst in het activiteitenlogboek. |
> | Microsoft.Insights/metricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
> | Microsoft.Logic/* | Logic Apps-resources beheert. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/operationresults/read | Resultaten van de bewerking voor het abonnement ophalen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/connectionGateways/* | Maken en beheren van een Gateway-verbinding. |
> | Microsoft.Web/connections/* | Maken en beheren van een verbinding. |
> | Microsoft.Web/customApis/* | Maakt en beheert een aangepaste API. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |
> | Microsoft.Web/sites/functions/listSecrets/action | Lijst met geheimen Web Apps-functies. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="logic-app-operator"></a>Logische App-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lezen, inschakelen en uitschakelen van de logische app. |
> | **Id** | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/*/read | Lezen Insights-waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/*/read | Diagnostische instellingen voor logische Apps worden opgehaald |
> | Microsoft.Insights/metricDefinitions/*/read | Hiermee haalt de beschikbare metrische gegevens voor Logic Apps. |
> | Microsoft.Logic/*/read | Hiermee leest u Logic Apps-resources. |
> | Microsoft.Logic/workflows/disable/action | Hiermee schakelt u de werkstroom. |
> | Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
> | Microsoft.Logic/workflows/validate/action | Hiermee valideert u de werkstroom. |
> | Microsoft.Resources/deployments/operations/read | Opgehaald of een lijst met implementatiebewerkingen. |
> | Microsoft.Resources/subscriptions/operationresults/read | Resultaten van de bewerking voor het abonnement ophalen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/connectionGateways/*/read | Lees de Gateways verbinding. |
> | Microsoft.Web/connections/*/read | Verbindingen lezen. |
> | Microsoft.Web/customApis/*/read | Lees de aangepaste API. |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="managed-application-operator-role"></a>De Operatorrol beheerde toepassing
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lees- en acties uitvoeren op resources van beheerde toepassingen |
> | **Id** | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Solutions/applications/read | Hiermee haalt een lijst met toepassingen. |
> | Microsoft.Solutions/*/action |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="managed-applications-reader"></a>Beheerde toepassingen-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lezen van resources in een beheerde app en de aanvraag voor JIT-toegang. |
> | **Id** | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="managed-identity-contributor"></a>Inzender beheerde identiteit
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Maken, lezen, bijwerken en verwijderen van door gebruiker toegewezen identiteit |
> | **Id** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Acties** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="managed-identity-operator"></a>Operator beheerde identiteit
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Maken en toewijzen van door gebruiker toegewezen identiteit |
> | **Id** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Acties** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="management-group-contributor"></a>Beheergroep-Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | De rol Inzender beheergroep |
> | **Id** | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | **Acties** |  |
> | Microsoft.Management/managementGroups/delete | Beheergroep verwijderen. |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Hiermee koppelt u abonnement uit de beheergroep ongedaan maken. |
> | Microsoft.Management/managementGroups/subscriptions/write | Collega's het bestaande abonnement met de beheergroep. |
> | Microsoft.Management/managementGroups/write | Maken of bijwerken van een beheergroep. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="management-group-reader"></a>Lezer van de beheergroep
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lezerrol voor de beheergroep |
> | **Id** | ac63b705-f282-497d-ac71-919bf39d939d |
> | **Acties** |  |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="monitoring-contributor"></a>Controlebijdrager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan alle controlegegevens lezen en bewerken van instellingen voor controle. Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | Waarschuwingsregels voor lezen/schrijven/verwijderen. |
> | Microsoft.Insights/components/* | Onderdelen van de Application Insights lezen, schrijven en verwijderen. |
> | Microsoft.Insights/DiagnosticSettings/* | Diagnostische instellingen lezen, schrijven en verwijderen. |
> | Microsoft.Insights/eventtypes/* | Lijst met gebeurtenissen in activiteitenlogboeken (van Beheergebeurtenissen) in een abonnement. Deze machtiging is van toepassing op zowel programmatische als portal toegang tot het activiteitenlogboek. |
> | Microsoft.Insights/LogDefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Logboekcategorieën lijst in het activiteitenlogboek. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
> | Microsoft.Insights/Metrics/* | De metrische gegevens voor een resource lezen. |
> | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registreren |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Lezen/schrijven/verwijderen Application Insights-webtests. |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Oplossingspakketten voor lezen/schrijven/verwijderen log analytics. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Lezen, schrijven en verwijderen met log analytics opgeslagen zoekopdrachten. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lezen/schrijven/verwijderen log analytics insight opslagconfiguraties. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="monitoring-metrics-publisher"></a>Uitgever van de metrische gegevens controleren
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee schakelt het publiceren van metrische gegevens bij Azure-resources |
> | **Id** | 3913510d-42f4-4e42-8a64-420c390055eb |
> | **Acties** |  |
> | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registreren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Metrische gegevens schrijven |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="monitoring-reader"></a>Controlelezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan alle controlegegevens lezen (metrische gegevens, Logboeken, enz.). Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../azure-monitor/platform/roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="network-contributor"></a>Inzender voor netwerken
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u netwerken, maar niet de toegang tot beheren. |
> | **Id** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/* | Maken en beheren van netwerken |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="new-relic-apm-account-contributor"></a>Nieuwe Relic APM-Account Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt nieuwe Relic Application Performance Management-accounts en -toepassingen, maar niet de toegang tot beheren. |
> | **Id** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="reader-and-data-access"></a>Lezer en toegang tot gegevens
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Alles weergeven, maar kunt u verwijderen of te maken van een storage-account of een ingesloten resource niet. Er kunnen ook toegang tot alle gegevens in een opslagaccount verleend via toegang tot opslagaccountsleutels lezen/schrijven. |
> | **Id** | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/ListAccountSas/action | Retourneert de Account-SAS-token voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="redis-cache-contributor"></a>Redis-Cache-Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Redis-caches, maar niet de toegang tot beheren. |
> | **Id** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Cache/redis/* | Maken en beheren van Redis-caches |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="resource-policy-contributor-preview"></a>Inzender voor Resourcebeleid (Preview)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | (Preview) Supportticket gebruikers gevuld vanuit EA, met machtigingen voor het maken/wijzigen voor resourcebeleid, maken en het lezen van resources/hiërarchie. |
> | **Id** | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Authorization/policyassignments/* | Maken en beheren van de toewijzingen van beleid |
> | Microsoft.Authorization/policydefinitions/* | Maken en beheren van beleidsdefinities |
> | Microsoft.Authorization/policysetdefinitions/* | Maken en beheren van beleid instellen |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="scheduler-job-collections-contributor"></a>Inzender voor Scheduler-taak
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren Scheduler-taakverzamelingen, maar niet de toegang tot. |
> | **Id** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Scheduler/jobcollections/* | Maken en beheren van taakcollecties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="search-service-contributor"></a>Inzender voor Search-Services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Search-services, maar niet de toegang tot beheren. |
> | **Id** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Search/searchServices/* | Search-services maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="security-admin"></a>Beveiligingsbeheerder
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | In Security Center: Kan weergeven beveiligingsbeleid, security-status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen |
> | **Id** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Authorization/policyAssignments/* | Maken en beheren van de toewijzingen van beleid |
> | Microsoft.Authorization/policyDefinitions/* | Maken en beheren van beleidsdefinities |
> | Microsoft.Authorization/policySetDefinitions/* | Maken en beheren van beleid instellen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics-gegevens weergeven |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Security/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="security-manager-legacy"></a>Beveiligingsbeheer (verouderd)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Dit is een verouderde rol. Gebruik in plaats hiervan beveiligingsbeheerder |
> | **Id** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ClassicCompute/*/read | Configuratie-informatie lezen klassieke virtuele machines |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor klassieke virtuele machines schrijven |
> | Microsoft.ClassicNetwork/*/read | Configuratie-informatie lezen over klassieke netwerk |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Security/* | Security-onderdelen en -beleid maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="security-reader"></a>Beveiligingslezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | In Security Center: Aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven |
> | **Id** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.operationalInsights/workspaces/*/read | Log analytics-gegevens weergeven |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Security/*/read | Onderdelen van de veiligheid lezen en het beleid |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="service-bus-data-owner"></a>De eigenaar van een service Bus-gegevens

> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u volledige toegang tot Azure Service Bus-resources. |
> | **Id** | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | **Acties** |  |
> | Microsoft.ServiceBus/* | Volledig beheer toegang tot de Service Bus-naamruimte |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/* | Volledige gegevens toegang tot de Service Bus-naamruimte |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="site-recovery-contributor"></a>Site Recovery-Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Site Recovery-service, met uitzondering van het maken van kluizen en roltoewijzing beheren |
> | **Id** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie met betrekking tot de kluis |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van geregistreerde identiteiten |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Maken of bijwerken van de replicatie-instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Maken en replicatie-infrastructuren beheren |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Replicatietaken maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Replicatiebeleid maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Maken en beheren van herstelplannen |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Maken en beheren van configuratie van de opslag van Recovery Services-kluis |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="site-recovery-operator"></a>Site Recovery-Operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u failover en failback maar geen andere beheerbewerkingen voor Site Recovery uitvoeren |
> | **Id** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lezen van alle instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat vernieuwen voor de infrastructuur |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare Items lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lezen van alle herstelpunten voor replicatie |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Replicatie herstellen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligd Item opnieuw beveiligen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Failovertest opschonen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility-Service bijwerken |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Provider vernieuwen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenters lezen |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Replicatietaken maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Elk beleid lezen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstelplan voor failover doorvoeren |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Herstelplan voor geplande Failover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lees elk plannen voor herstel |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Herstelplan voor opnieuw beveiligen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Herstelplan voor Failover testen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plan voor herstel van test voor de failovertest |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Herstelplan voor failover |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="site-recovery-reader"></a>Site Recovery-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Site Recovery-status weergeven maar geen andere beheerbewerkingen uitvoeren |
> | **Id** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lezen van alle instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare Items lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lezen van alle herstelpunten voor replicatie |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenters lezen |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Alle taken lezen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Elk beleid lezen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lees elk plannen voor herstel |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="spatial-anchors-account-contributor"></a>Inzender voor het Account van de ruimtelijke ankers
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u ruimtelijke ankers beheren in uw account, maar ze niet verwijderen |
> | **Id** | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Ruimtelijke ankers maken |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | In de buurt ruimtelijke ankers detecteren |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermee worden eigenschappen van ruimtelijke ankers |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ruimtelijke ankers zoeken |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Verzenden van diagnostische gegevens over ter verbetering van de kwaliteit van de service Azure ruimtelijke ankers |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Eigenschappen van ruimtelijke ankers bijwerken |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="spatial-anchors-account-owner"></a>De accounteigenaar ruimtelijke ankers
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren ruimtelijke ankers in uw account, met inbegrip van deze worden verwijderd |
> | **Id** | 70bbe301-9835-447d-afdd-19eb3167307c |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Ruimtelijke ankers maken |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Ruimtelijke ankers verwijderen |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | In de buurt ruimtelijke ankers detecteren |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermee worden eigenschappen van ruimtelijke ankers |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ruimtelijke ankers zoeken |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Verzenden van diagnostische gegevens over ter verbetering van de kwaliteit van de service Azure ruimtelijke ankers |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Eigenschappen van ruimtelijke ankers bijwerken |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="spatial-anchors-account-reader"></a>Ruimtelijke ankers Account Reader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt zoeken en lezen van de eigenschappen van ruimtelijke ankers in uw account |
> | **Id** | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | In de buurt ruimtelijke ankers detecteren |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Hiermee worden eigenschappen van ruimtelijke ankers |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Ruimtelijke ankers zoeken |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Verzenden van diagnostische gegevens over ter verbetering van de kwaliteit van de service Azure ruimtelijke ankers |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="sql-db-contributor"></a>' SQL DB Contributor '
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beveiligingsbeleid of de bovenliggende SQL-servers. |
> | **Id** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | SQL-databases maken en beheren |
> | Microsoft.Sql/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.Insights/metricDefinitions/read | Metrische definities lezen |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Auditbeleid bewerken |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen bewerken |
> | Microsoft.Sql/servers/databases/auditRecords/read | De controlerecords voor database-blob ophalen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Verbinding beleidsregels kunt bewerken |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Gegevens maskeren beleid bewerken |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid bewerken |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Over de beveiliging bewerken |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="sql-managed-instance-contributor"></a>SQL beheerd exemplaar Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u beheerde SQL-instanties beheren en de vereiste netwerkconfiguratie, maar kunnen geen toegang verlenen aan anderen. |
> | **Id** | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | **Acties** |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Network/networkSecurityGroups/* |  |
> | Microsoft.Network/routeTables/* |  |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/managedInstances/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Network/virtualNetworks/subnets/* |  |
> | Microsoft.Network/virtualNetworks/* |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.Insights/metricDefinitions/read | Metrische definities lezen |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="sql-security-manager"></a>SQL-beveiligingsbeheer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt het beveiligingsbeleid van SQL-servers en databases, maar niet de toegang tot beheren. |
> | **Id** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Alleen Microsoft-authorisatie |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. Niet Signaleerbare. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server audit beleid maken en beheren |
> | Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van SQL server-controle-instellingen |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | Gegevens van de uitgebreide server blob auditing dat is geconfigureerd op een bepaalde server niet ophalen |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en beheren van controlebeleid van de SQL server-database |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
> | Microsoft.Sql/servers/databases/auditRecords/read | Controlerecords lezen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server-database verbinding beleid maken en beheren |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens maskeren beleid |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | De gegevens van de uitgebreide blob controleren dat is geconfigureerd op een bepaalde database ophalen |
> | Microsoft.Sql/servers/databases/read | Retourneert de lijst met databases of haalt de eigenschappen voor de opgegeven database. |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | Het schema van een database ophalen. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Een databasekolom ophalen. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Een databasetabel ophalen. |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van waarschuwingen beveiligingsbeleid van SQL server-database |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van SQL server-database security metrische gegevens |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server security waarschuwing beleid maken en beheren |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="sql-server-contributor"></a>Inzender voor SQL Server
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u SQL-servers en databases beheren, maar niet de toegang tot en het beveiligingsbeleid-beleidsregels die betrekking hebben. |
> | **Id** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Maken en beheren van de SQL-servers |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.Insights/metricDefinitions/read | Metrische definities lezen |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server-controlebeleid bewerken |
> | Microsoft.Sql/servers/auditingSettings/* | SQL server-controle-instellingen bewerken |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server-database controlebeleid bewerken |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL server-database controle-instellingen bewerken |
> | Microsoft.Sql/servers/databases/auditRecords/read | Controlerecords lezen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server-database verbinding beleidsregels kunt bewerken |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-databasegegevens maskeren beleid bewerken |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid van SQL server-database bewerken |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Metrische gegevens van SQL server-database security bewerken |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server waarschuwing beveiligingsbeleid bewerken |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-account-contributor"></a>Inzender voor Opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan beheer van de storage-accounts. Biedt geen toegang tot gegevens in de storage-account. |
> | **Id** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Alle autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. Niet Signaleerbare. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Storage/storageAccounts/* | Opslagaccounts maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-account-key-operator-service-role"></a>Storage-Account servicerol Sleuteloperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunnen inhoud weergeven en opnieuw genereren van toegangssleutels voor opslag-account. |
> | **Id** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Genereer een nieuwe de toegangssleutels voor het opgegeven opslagaccount. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-blob-data-contributor"></a>Gegevensbijdrager voor Blob
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lezen, schrijven en verwijderen van Azure Storage-containers en blobs. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Verwijderen van een container. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Retourneert een container of een lijst met containers. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | De metagegevens van een container of de eigenschappen wijzigen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Een blob verwijderen. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Een blob of een lijst met blobs geretourneerd. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Schrijven naar een blob. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-blob-data-owner"></a>De eigenaar van een opslag-Blob-gegevens
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Biedt volledige toegang tot Azure Storage-blobcontainers en gegevens, zoals het toewijzen van POSIX-toegangsbeheer. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | Volledige machtigingen voor containers. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Volledige machtigingen voor blobs. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-blob-data-reader"></a>Gegevenslezer voor Opslagblob
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lezen en Azure Storage-containers en blobs te vermelden. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Retourneert een container of een lijst met containers. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Een blob of een lijst met blobs geretourneerd. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-queue-data-contributor"></a>Gegevensbijdrager voor wachtrij
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lezen, schrijven en verwijderen van Azure Storage-wachtrijen en -Wachtrijberichten verleend. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Een wachtrij verwijderen. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Een wachtrij of een lijst met wachtrijen geretourneerd. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Eigenschappen van metagegevens in de wachtrij of wijzigen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Een of meer berichten uit een wachtrij verwijderen. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bekijken of een of meer berichten ophalen uit een wachtrij. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Een bericht toevoegen aan een wachtrij. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-queue-data-message-processor"></a>Storage Queue Gegevensverwerker bericht
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Bekijken, ophalen en verwijderen van berichten uit een Azure Storage-wachtrij. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Een bericht bekijken. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Ophalen en verwijderen van een bericht. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-queue-data-message-sender"></a>Storage Queue gegevens afzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Berichten aan een Azure Storage-wachtrij toevoegen. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | **Acties** |  |
> | *none* |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Een bericht toevoegen aan een wachtrij. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="storage-queue-data-reader"></a>Gegevenslezer voor Opslagwachtrij
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lees- en Azure Storage-wachtrijen en-Wachtrijberichten. Zie voor meer acties zijn vereist voor een bepaalde gegevensbewerking [machtigingen voor het aanroepen van blob- en wachtrijservices gegevensbewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). |
> | **Id** | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Retourneert een wachtrij of een lijst met wachtrijen. |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bekijken of een of meer berichten ophalen uit een wachtrij. |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="support-request-contributor"></a>Inzender voor ondersteuningsaanvragen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt maken en ondersteuningsaanvragen beheren |
> | **Id** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="traffic-manager-contributor"></a>Inzender voor Traffic Manager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u bepalen wie toegang heeft tot deze niet. |
> | **Id** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u gebruikerstoegang tot Azure-resources beheren. |
> | **Id** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Acties** |  |
> | \* / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Authorization/* | Machtigingen beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="virtual-machine-administrator-login"></a>Beheerdersaanmelding bij virtuele Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Virtuele Machines weergeven in de portal en meld u aan als beheerder |
> | **Id** | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | **Acties** |  |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Meld u aan bij een virtuele machine met Windows-beheerders- of Linux rootgebruikersbevoegdheden |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="virtual-machine-contributor"></a>Inzender voor virtuele machines
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren van virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
> | **Id** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Compute/availabilitySets/* | Maken en beheren van compute-beschikbaarheidssets |
> | Microsoft.Compute/locations/* | Maken en beheren van compute-locaties |
> | Microsoft.Compute/virtualMachines/* | Virtuele machines maken en beheren |
> | Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van virtuele-machineschaalsets |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Lid wordt van een application gateway back-end-adresgroep. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een load balancer-back-end-adresgroep. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid wordt van een load balancer binnenkomende NAT-pool. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van tests van een load balancer. Bijvoorbeeld: met deze machtiging Statustest-eigenschap van de VM-schaalset set kan verwijzen naar de test. Niet Signaleerbare. |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
> | Microsoft.Network/networkInterfaces/* | Maken en beheren van netwerkinterfaces |
> | Microsoft.Network/networkSecurityGroups/join/action | Een netwerkbeveiligingsgroep koppelt. Niet Signaleerbare. |
> | Microsoft.Network/networkSecurityGroups/read | De definitie van een network security opgehaald |
> | Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare ip-adres. Niet Signaleerbare. |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk. Niet Signaleerbare. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Een back-Upbeveiligingsintentie maken |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Hiermee maakt u beleid voor beveiliging |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Retourneert de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Retourneert de lijst met storage-accounts of haalt u de eigenschappen voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="virtual-machine-user-login"></a>Gebruikersaanmelding bij virtuele Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Virtuele Machines in de portal en meld u aan als een gewone gebruiker weergeven. |
> | **Id** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Acties** |  |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="web-plan-contributor"></a>Inzender voor webabonnementen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u de webabonnementen voor websites, maar niet de toegang om deze te beheren. |
> | **Id** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/serverFarms/* | Maken en beheren van server-farms |
> | Microsoft.Web/hostingEnvironments/Join/Action | Lid wordt van een App Service Environment |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="website-contributor"></a>Inzender voor websites
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u websites (niet webabonnementen), maar niet de toegang tot beheren. |
> | **Id** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/components/* | Maken en beheren van inzicht onderdelen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Hiermee haalt u de beschikbaarheidsstatus ophalen voor alle resources in het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Opgehaald of een lijst met resourcegroepen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/certificates/* | Maken en beheren van Websitecertificaten |
> | Microsoft.Web/listSitesAssignedToHostName/read | Namen van sites die zijn toegewezen aan de hostnaam ophalen. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |
> | Microsoft.Web/sites/* | Websites (het maken van site ook moeten schrijfmachtigingen voor de bijbehorende App Service-Plan) maken en beheren |
> | **NotActions** |  |
> | *none* |  |
> | **DataActions** |  |
> | *none* |  |
> | **NotDataActions** |  |
> | *none* |  |

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen voor Azure-resources](custom-roles.md)
- [Toegang tot Azure-resources beheren met op rollen gebaseerd toegangsbeheer en de Azure-portal](role-assignments-portal.md)
- [Machtigingen in Azure Security Center](../security-center/security-center-permissions.md)
