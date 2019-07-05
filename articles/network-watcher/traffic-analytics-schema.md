---
title: Azure traffic analytics schema | Microsoft Docs
description: Krijg inzicht in het schema van Traffic Analytics voor het analyseren van stroomlogboeken van Azure-netwerk.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: vinigam
ms.openlocfilehash: 9a02a56df85c5c6aa9fd177ad42a2f9bfb303e44
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491953"
---
# <a name="schema-and-data-aggregation-in-traffic-analytics"></a>Schema en voor gegevensaggregatie in Traffic Analytics

Traffic Analytics is een cloud-gebaseerde oplossing waarmee u inzicht in gebruikers en toepassingen in cloudnetwerken. Traffic Analytics analyseert Network Watcher network security group (NSG)-stroomlogboeken voor inzicht in de verkeersstroom in uw Azure-cloud. Met traffic analytics, kunt u het volgende doen:

- Netwerkactiviteit visualiseren voor uw Azure-abonnementen en hotspots identificeren.
- Beveiligingsrisico's te identificeren en Beveilig uw netwerk, met informatie zoals open-poorten, toepassingen die toegang tot internet en virtuele machines (VM) maken van verbinding met netwerken rogue proberen.
- Patronen van verkeersstromen begrijpen tussen Azure-regio's en het internet naar het optimaliseren van uw netwerkimplementatie voor de prestaties en capaciteit.
- Identificeer netwerk configuratiefouten leiden tot mislukte verbindingen in uw netwerk.
- Netwerkgebruik in bytes, pakketten of stromen op de hoogte.

### <a name="data-aggregation"></a>De gegevens worden verzameld

1. Alle logboeken van de stroom op een NSG tussen "FlowIntervalStartTime_t" en "FlowIntervalEndTime_t" worden vastgelegd met tussenpozen van één minuut in de storage-account als blobs voordat deze wordt verwerkt door Traffic Analytics.
2. Verwerking van standaardintervaltijd van Traffic Analytics is 60 minuten. Dit betekent dat elke 60 minuten die Traffic Analytics blobs uit de opslag voor aggregatie kiest.
3. Stromen met de dezelfde bron-IP, doel-IP, doelpoort, naam van NSG, NSG-regel, Flow richting en Transport layer-protocol (TCP of UDP) (Opmerking: Bronpoort is uitgesloten voor aggregatie) in één stroom worden geregeld door Traffic Analytics
4. Deze één record is versierde (Details in de sectie hieronder) en opgenomen in Log Analytics door verkeer Analytics.This proces kan maximaal een uur duren max.
5. FlowStartTime_t veld geeft aan dat het eerste exemplaar van dergelijke een samengevoegde stroom (dezelfde 4-tuple) in de stroom van interval tussen "FlowIntervalStartTime_t" en "FlowIntervalEndTime_t" de logboekverwerking.
6. Voor elke resource in TA, de stromen die zijn aangegeven in de gebruikersinterface voor het totaal aantal stromen gezien door de Netwerkbeveiligingsgroep zijn, maar in logboek Anlaytics gebruiker alleen de record één, verlaagde ziet. Als alle stromen weergeven, gebruikt u het veld blob_id, die uit de opslag kan worden verwezen. De totale stroom telt voor die record komt overeen met de eigen stromen weergegeven in de blob.

De onderstaande query helpt u ziet er uit op alle stromen logboeken van on-premises in de afgelopen 30 dagen.
```
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s  
```
Als u wilt het blobpad voor de stromen in de bovenstaande query weergeven, gebruikt u de onderstaande query:

```
let TableWithBlobId =
(AzureNetworkAnalytics_CL
   | where SubType_s == "Topology" and ResourceType == "NetworkSecurityGroup" and DiscoveryRegion_s == Region_s and IsFlowEnabled_b
   | extend binTime = bin(TimeProcessed_t, 6h),
            nsgId = strcat(Subscription_g, "/", Name_s),
            saNameSplit = split(FlowLogStorageAccount_s, "/")
   | extend saName = iif(arraylength(saNameSplit) == 3, saNameSplit[2], '')
   | distinct nsgId, saName, binTime)
| join kind = rightouter (
   AzureNetworkAnalytics_CL
   | where SubType_s == "FlowLog"  
   | extend binTime = bin(FlowEndTime_t, 6h)
) on binTime, $left.nsgId == $right.NSGList_s  
| extend blobTime = format_datetime(todatetime(FlowIntervalStartTime_t), "yyyy MM dd hh")
| extend nsgComponents = split(toupper(NSGList_s), "/"), dateTimeComponents = split(blobTime, " ")
| extend BlobPath = strcat("https://", saName,
                        "@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/", nsgComponents[0],
                        "/RESOURCEGROUPS/", nsgComponents[1],
                        "/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/", nsgComponents[2],
                        "/y=", dateTimeComponents[0], "/m=", dateTimeComponents[1], "/d=", dateTimeComponents[2], "/h=", dateTimeComponents[3],
                        "/m=00/macAddress=", replace(@"-", "", MACAddress_s),
                        "/PT1H.json")
| project-away nsgId, saName, binTime, blobTime, nsgComponents, dateTimeComponents;

TableWithBlobId
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s , BlobPath
```

De bovenstaande query vormt een URL waarmee u rechtstreeks toegang hebben tot de blob. URL van de met plaatsaanduidingen lager is dan:

```
https://{saName}@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroup}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json

```

### <a name="fields-used-in-traffic-analytics-schema"></a>Velden die worden gebruikt in het schema voor Traffic Analytics

Traffic Analytics is gebaseerd op Log Analytics, zodat u kunt aangepaste query's uitvoeren op gegevens die door Traffic Analytics versierde en waarschuwingen instellen voor hetzelfde.

Hieronder ziet u de velden in het schema en wat ze geven

| Veld | Indeling | Opmerkingen |
|:---   |:---    |:---  |
| TableName | AzureNetworkAnalytics_CL | Tabel voor verkeer Anlaytics gegevens
| SubType_s | FlowLog | Subtype voor de logboeken van de stroom. Alleen 'FlowLog' gebruikt, zijn andere waarden van SubType_s voor interne werking van het product |
| FASchemaVersion_s |   1   | Scehma versie. Komt niet overeen met NSG Flow-Log-versie |
| TimeProcessed_t   | Datum en tijd in UTC  | Tijdstip waarop de Traffic Analytics de onbewerkte stroomlogboeken van het opslagaccount verwerkt |
| FlowIntervalStartTime_t | Datum en tijd in UTC |  Begintijd van de flow-log interval verwerken. Dit is het tijd van waaruit de stroom interval wordt gemeten |
| FlowIntervalEndTime_t | Datum en tijd in UTC | Eindtijd van de stroom log verwerkingsinterval |
| FlowStartTime_t | Datum en tijd in UTC |  Eerste exemplaar van de stroom (die wordt ophalen samengevoegd) in de stroom log verwerkingsinterval tussen "FlowIntervalStartTime_t" en 'FlowIntervalEndTime_t'. Deze stroom wordt samengevoegd op basis van de aggregatie logica |
| FlowEndTime_t | Datum en tijd in UTC | Laatste exemplaar van de stroom (die wordt ophalen samengevoegd) in de stroom log verwerkingsinterval tussen "FlowIntervalStartTime_t" en 'FlowIntervalEndTime_t'. In termen van flow log v2 bevat dit veld het tijdstip waarop de laatste stroom met de dezelfde 4-tuple gestart (gemarkeerd als "B" in de onbewerkte stroom-record) |
| FlowType_s |  * IntraVNet <br> * InterVNet <br> * S2S <br> * P2S <br> * AzurePublic <br> * ExternalPublic <br> * MaliciousFlow <br> * Onbekend privé <br> * Onbekend | Definitie van de opmerkingen bij de onder de tabel |
| SrcIP_s | IP-adres van bron | In het geval van AzurePublic leeg en ExternalPublic stromen |
| DestIP_s | Doel-IP-adres | In het geval van AzurePublic leeg en ExternalPublic stromen |
| VMIP_s | IP-adres van de virtuele machine | Gebruikt voor AzurePublic en ExternalPublic stromen |
| PublicIP_s | Openbare IP-adressen | Gebruikt voor AzurePublic en ExternalPublic stromen |
| DestPort_d | Doelpoort | Poort waarmee verkeer inkomend is |
| L4Protocol_s  | * T <br> * U  | Transport Protocol. T = TCP <br> U = UDP |
| L7Protocol_s  | Protocolnaam | Afgeleid van doelpoort |
| FlowDirection_s | * Ik = inkomend<br> * O = uitgaande | Richting van de stroom in/uit NSG aan de hand van stroomlogboek |
| FlowStatus_s  | * Een = toegestaan door NSG-regel <br> * D = geweigerd door NSG-regel  | Status van stroom toegestaan/nblocked door NSG aan de hand van stroomlogboek |
| NSGList_s | \<SUBSCRIPTIONID>\/<RESOURCEGROUP_NAME>\/<NSG_NAME> | Netwerkbeveiligingsgroep (NSG) die zijn gekoppeld aan de stroom |
| NSGRules_s | \<Index waarde 0) >< NSG_RULENAME >\<Flow richting >\<Flow Status >\<FlowCount ProcessedByRule > |  NSG-regel die is toegestaan of geweigerd met deze stroom |
| NSGRuleType_s | * De gebruiker gedefinieerde * standaard |   Het type van de NSG-regel die wordt gebruikt door de stroom |
| MACAddress_s | MAC-adres | MAC-adres van de NIC waarop de stroom is opgenomen |
| Subscription_s | Abonnement van de Azure-netwerk / netwerkinterface / virtuele machines in dit veld wordt ingevuld | Alleen van toepassing op FlowType = S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow en UnknownPrivate flow types (typen gebruikersstromen waar slechts één kant azure is) |
| Subscription1_s | Abonnements-id | Abonnements-ID van het virtuele netwerk / netwerkinterface / virtuele machine waarop het bron-IP-adres in de stroom behoort |
| Subscription2_s | Abonnements-id | Abonnements-ID van het virtuele netwerk / netwerkinterface / virtuele machine waarop de doel-IP-adres in de stroom behoort |
| Region_s | Azure-regio van het virtuele netwerk / netwerkinterface / virtuele machine waarop het IP-adres in de stroom behoort | Alleen van toepassing op FlowType = S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow en UnknownPrivate flow types (typen gebruikersstromen waar slechts één kant azure is) |
| Region1_s | Azure-regio | Azure-regio van het virtuele netwerk / netwerkinterface / virtuele machine waarop het bron-IP-adres in de stroom behoort |
| Region2_s | Azure-regio | Azure-regio van het virtuele netwerk waarvoor het doel-IP-adres in de stroom behoort |
| NIC_s | \<resourcegroup_Name>\/\<NetworkInterfaceName> |  NIC die is gekoppeld aan de virtuele machine verzenden of ontvangen van het verkeer |
| NIC1_s | <resourcegroup_Name>/\<NetworkInterfaceName> | NIC die is gekoppeld aan de bron-IP-adres in de stroom |
| NIC2_s | <resourcegroup_Name>/\<NetworkInterfaceName> | NIC die is gekoppeld aan de doel-IP-adres in de stroom |
| VM_s | <resourcegroup_Name>\/\<NetworkInterfaceName> | Virtuele Machine die is gekoppeld aan de netwerkinterface NIC_s |
| VM1_s | <resourcegroup_Name>/\<VirtualMachineName> | Virtuele Machine die is gekoppeld aan de bron-IP-adres in de stroom |
| VM2_s | <resourcegroup_Name>/\<VirtualMachineName> | Virtuele Machine die is gekoppeld aan de doel-IP-adres in de stroom |
| Subnet_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Subnet dat is gekoppeld aan de NIC_s |
| Subnet1_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName> | Subnet dat is gekoppeld aan de bron-IP-adres in de stroom |
| Subnet2_s | <ResourceGroup_Name>/<VNET_Name>/\<SubnetName>    | Subnet dat is gekoppeld aan de doel-IP-adres in de stroom |
| ApplicationGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Application gateway die is gekoppeld aan de bron-IP-adres in de stroom |
| ApplicationGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<ApplicationGatewayName> | Application gateway die is gekoppeld aan de doel-IP-adres in de stroom |
| LoadBalancer1_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Load balancer die zijn gekoppeld aan de bron-IP-adres in de stroom |
| LoadBalancer2_s | \<SubscriptionID>/\<ResourceGroupName>/\<LoadBalancerName> | Load balancer gekoppeld met de doel-IP-adres in de stroom |
| LocalNetworkGateway1_s | \<SubscriptionID>/\<ResourceGroupName>/\<LocalNetworkGatewayName> | Lokale netwerkgateway die is gekoppeld aan de bron-IP-adres in de stroom |
| LocalNetworkGateway2_s | \<SubscriptionID>/\<ResourceGroupName>/\<LocalNetworkGatewayName> | Lokale netwerkgateway die is gekoppeld aan de doel-IP-adres in de stroom |
| ConnectionType_s | Mogelijke waarden zijn VNetPeering, VPN-gateway en ExpressRoute |    Verbindingstype |
| ConnectionName_s | \<SubscriptionID>/\<ResourceGroupName>/\<ConnectionName> | Verbindingsnaam |
| ConnectingVNets_s | Ruimte gescheiden lijst met namen van virtueel netwerk | In het geval van hub en spoke-topologie, worden virtuele netwerken hub hier ingevuld |
| Country_s | Landcode (ISO 3166-1-alfa-2) van twee letters | Voor stroomtype ExternalPublic wordt gevuld. Alle IP-adressen in het veld PublicIPs_s delen de dezelfde landcode |
| AzureRegion_s | Locaties van de Azure-regio | Voor stroomtype AzurePublic wordt gevuld. Alle IP-adressen in het veld PublicIPs_s deelt de Azure-regio |
| AllowedInFlows_d | | Het aantal inkomende stromen die zijn toegestaan. Hiermee wordt het aantal stromen die de dezelfde 4-tuple gedeeld inkomende verbinding met de netweork-interface waarmee de stroom is vastgelegd |
| DeniedInFlows_d |  | Het aantal inkomende stromen die zijn geweigerd. (Binnenkomend aan de netwerkinterface waarmee de stroom is vastgelegd) |
| AllowedOutFlows_d | | Het aantal uitgaande stromen die zijn toegestaan (uitgaand aan de netwerkinterface waarmee de stroom is vastgelegd) |
| DeniedOutFlows_d  | | Het aantal uitgaande stromen die zijn geweigerd (uitgaand aan de netwerkinterface waarmee de stroom is vastgelegd) |
| FlowCount_d | Afgeschaft. Totaal aantal stromen die overeenkomen met de dezelfde 4-tuple. In het geval van de typen gebruikersstromen ExternalPublic en AzurePublic bevat aantal het stromen van ook verschillende PublicIP-adressen.
| InboundPackets_d | Pakketten die zijn ontvangen als vastgelegd op de netwerkinterface waar de NSG-regel is toegepast | Dit is ingevuld alleen voor de versie 2 van NSG-schema voor het logboek van flow |
| OutboundPackets_d  | Pakketten die worden verzonden als vastgelegd op de netwerkinterface waar de NSG-regel is toegepast | Dit is ingevuld alleen voor de versie 2 van NSG-schema voor het logboek van flow |
| InboundBytes_d |  Bytes ontvangen zoals vastgelegd op de netwerkinterface waar de NSG-regel is toegepast | Dit is ingevuld alleen voor de versie 2 van NSG-schema voor het logboek van flow |
| OutboundBytes_d | Bytes verzonden zoals vastgelegd op de netwerkinterface waar de NSG-regel is toegepast | Dit is ingevuld alleen voor de versie 2 van NSG-schema voor het logboek van flow |
| CompletedFlows_d  |  | Dit is gevuld met andere waarde dan nul alleen voor de versie 2 van NSG-schema voor het logboek van flow |
| PublicIPs_s | < PUBLIC_IP >\|\<FLOW_STARTED_COUNT >\|\<FLOW_ENDED_COUNT >\|\<OUTBOUND_PACKETS >\|\<INBOUND_PACKETS >\| \<OUTBOUND_BYTES >\|\<INBOUND_BYTES > | Items gescheiden door balken |

### <a name="notes"></a>Opmerkingen

1. In het geval van AzurePublic en ExternalPublic stromen, de klant die eigendom zijn dat IP-Adressen van Azure VM is ingevuld in VMIP_s veld, terwijl de openbare IP-adressen worden ingevuld in het veld PublicIPs_s. Voor deze stroomtypen twee, moeten we VMIP_s en PublicIPs_s gebruiken in plaats van SrcIP_s en DestIP_s velden. Voor AzurePublic en ExternalPublicIP-adressen, we statistische-meer, zodat het aantal records die worden opgenomen in log analytics-werkruimte van de klant minimaal is. (Dit veld wordt binnenkort worden afgeschaft en we moeten worden met behulp van SrcIP_ en DestIP_s, afhankelijk van of virtuele machines van azure de bron of de bestemming in de stroom is)
1. Details voor flow typen: Op basis van de IP-adressen die betrokken zijn bij de stroom, categoriseren we van de stromen in met de volgende stroomtypen:
1. IntraVNet: zowel de IP-adressen in de stroom zich bevinden in hetzelfde Azure-netwerk.
1. InterVNet - IP-adressen in de stroom zich bevinden in de twee virtuele netwerken van verschillende Azure.
1. S2S-(Site-naar-Site) een van de IP-adressen behoort tot Azure Virtual Network terwijl de andere IP-adres tot klantnetwerk (Site behoort) die zijn verbonden met het Azure-netwerk via VPN-gateway of Express Route.
1. P2S - (punt-naar-Site) een van de IP-adressen met Azure-netwerk behoort, terwijl de andere IP-adres tot klantnetwerk (Site behoort) die zijn verbonden met het Azure-netwerk via VPN-gateway.
1. AzurePublic - een van de IP-adressen met Azure Virtual Network bij hoort terwijl de andere IP-adres tot Azure interne openbare IP-adressen die zijn eigendom van Microsoft behoort. Klanten die eigendom zijn openbare IP-adressen niet deel uit van dit stroomtype. Bijvoorbeeld, eigendom elke klant met een VM die verkeer verzenden naar een Azure-Service (opslageindpunt) onder dit stroomtype zou worden gecategoriseerd.
1. ExternalPublic - behoort een van de IP-adressen met Azure-netwerk terwijl de andere IP-adres een openbare IP-adres dat zich niet in Azure is, wordt niet gerapporteerd als in de ASC-feeds die Traffic Analytics voor het verwerkingsinterval voor tussen verbruikt schadelijke " FlowIntervalStartTime_t' en 'FlowIntervalEndTime_t'.
1. MaliciousFlow - een van de IP-adressen deel uitmaken van azure-netwerk terwijl de andere IP-adres is een openbare IP-adres dat zich niet in Azure en wordt gerapporteerd als in de ASC-feeds die Traffic Analytics voor het verwerkingsinterval voor tussen verbruikt schadelijke" FlowIntervalStartTime_t' en 'FlowIntervalEndTime_t'.
1. UnknownPrivate - een van de IP-adressen deel uitmaken van Azure Virtual Network terwijl de andere IP-adres behoort tot de privé IP-adresbereik, zoals gedefinieerd in RFC 1918 en kan niet door Traffic Analytics worden toegewezen aan een site of Azure Virtual Network van klanten.
1. Onbekend: kan geen om toe te wijzen op een van de IP-adressen in de stromen met de klant-topologie in Azure, evenals on-premises (site).
1. De namen van sommige velden worden toegevoegd met _K of _d. Deze niet geven aan bron- en doelserver.

### <a name="next-steps"></a>Volgende stappen
Vindt u antwoorden op veelgestelde vragen, [Traffic analytics Veelgestelde vragen over](traffic-analytics-faq.md) Zie voor meer informatie over de functionaliteit van [Traffic analytics-documentatie](traffic-analytics.md)
