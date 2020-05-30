---
title: Veelgestelde vragen over Azure Red Hat open Shift
description: Hier vindt u antwoorden op veelgestelde vragen over Microsoft Azure Red Hat open Shift
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: conceptual
ms.date: 05/29/2020
ms.openlocfilehash: 0c4c5ddfebe9e2b5b37a2c28ec4941f6c38668f1
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2020
ms.locfileid: "84219225"
---
# <a name="azure-red-hat-openshift-faq"></a>Veelgestelde vragen over Azure Red Hat open Shift

In dit artikel vindt u antwoorden op veelgestelde vragen over Microsoft Azure Red Hat open SHIFT.

## <a name="installation-and-upgrade"></a>Installatie en upgrade

### <a name="which-azure-regions-are-supported"></a>Welke Azure-regio's worden ondersteund?

Zie [beschik bare regio's](https://docs.openshift.com/aro/4/welcome/index.html#available-regions)voor een lijst met ondersteunde regio's voor Azure Red Hat open Shift 4. x.

Zie [producten beschikbaar per regio](supported-resources.md#azure-regions)voor een lijst met ondersteunde regio's voor Azure Red Hat open Shift 3,11.

### <a name="what-virtual-machine-sizes-can-i-use"></a>Welke groottes voor virtuele machines kan ik gebruiken?

Zie voor een lijst met ondersteunde virtuele-machine grootten voor Azure Red Hat open Shift 4 [ondersteunde resources voor Azure Red Hat open Shift 4](support-policies-v4.md).

Voor een lijst met ondersteunde virtuele-machine grootten voor Azure Red Hat open Shift 3,11 raadpleegt u [ondersteunde bronnen voor Azure Red Hat open shift 3,11](supported-resources.md).

### <a name="what-is-the-maximum-number-of-pods-in-an-azure-red-hat-openshift-cluster--what-is-the-maximum-number-of-pods-per-node-in-azure-red-hat-openshift"></a>Wat is het maximum aantal van een Azure Red Hat open Shift-cluster?  Wat is het maximum aantal peulen per knoop punt in azure Red Hat open Shift?

Het werkelijke aantal ondersteunde peulen is afhankelijk van het geheugen van de toepassing, de CPU en de opslag vereisten.

Azure Red Hat open Shift 4. x heeft een limiet van 250 pod per knoop punt en een limiet van 100 Compute-knoop punten. Dit is het maximum aantal dat wordt ondersteund in een cluster tot 250 &times; 100 = 25.000.

Azure Red Hat open Shift 3,11 heeft een limiet van 50 pod per knoop punt en een limiet van 20 reken knooppunten. Dit is het maximum aantal dat wordt ondersteund in een cluster tot 50 &times; 20 = 1.000.

### <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Kan een cluster reken knooppunten hebben in meerdere Azure-regio's?

Nee. Alle knoop punten in een open Shift-cluster van Azure Red Hat moeten afkomstig zijn uit dezelfde Azure-regio.

### <a name="can-a-cluster-be-deployed-across-multiple-availability-zones"></a>Kan een cluster in meerdere beschikbaarheids zones worden geïmplementeerd?

Ja. Dit gebeurt automatisch als uw cluster wordt geïmplementeerd in een Azure-regio die beschikbaarheids zones ondersteunt. Zie [beschikbaarheids zones](../availability-zones/az-overview.md#availability-zones)voor meer informatie.

### <a name="are-control-plane-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>Zijn er knoop punten van het besturings element in de buurt van de Azure Kubernetes service (AKS)?

Nee. Alle resources, inclusief de cluster hoofd knooppunten, worden uitgevoerd in uw klant abonnement. Deze typen resources worden in een alleen-lezen resource groep geplaatst.

### <a name="does-the-cluster-reside-in-a-customer-subscription"></a>Bevindt het cluster zich in een klant abonnement? 

De door Azure beheerde toepassing bevindt zich in een vergrendelde resource groep met het abonnement van de klant. Klanten kunnen objecten in die resource groep weer geven, maar niet wijzigen.

### <a name="is-there-any-element-in-azure-red-hat-openshift-shared-with-other-customers-or-is-everything-independent"></a>Is er een element in azure Red Hat open Shift gedeeld met andere klanten? Of is alles onafhankelijk?

Elk Azure Red Hat open Shift-cluster is toegewezen aan een bepaalde klant en valt binnen het abonnement van de klant. 

### <a name="are-infrastructure-nodes-available"></a>Zijn er infra structuur knooppunten beschikbaar?

In azure Red Hat open Shift 4. x-clusters zijn er momenteel geen infrastructuur knooppunten beschikbaar.

In azure Red Hat open Shift 3,11-clusters zijn standaard infrastructuur knooppunten opgenomen.

## <a name="upgrades"></a>Uitvoeren

###  <a name="what-is-the-general-upgrade-process"></a>Wat is het algemene upgrade proces?

Patches worden automatisch toegepast op uw cluster. U hoeft geen actie te ondernemen voor het ontvangen van patch upgrades voor uw cluster.

Het uitvoeren van een upgrade is een veilig proces dat kan worden uitgevoerd en die Cluster Services niet kan verstoren. Het gezamenlijke micro soft-Red Hat-team kan het upgrade proces activeren wanneer er nieuwe versies beschikbaar zijn of veelvoorkomende beveiligings lekken en bloot stellingen worden verstaan. Beschik bare updates worden getest in een faserings omgeving en vervolgens toegepast op productie clusters. Volg de aanbevolen procedures om ervoor te zorgen dat er mini maal geen uitval tijd is.

Gepland onderhoud is niet voorgepland met de klant. Meldingen met betrekking tot onderhoud kunnen via e-mail worden verzonden.

### <a name="what-is-the-azure-red-hat-openshift-maintenance-process"></a>Wat is het onderhouds proces van Azure Red Hat open Shift?

Er zijn twee soorten onderhoud voor Azure Red Hat open Shift: upgrades en door de Cloud provider geïnitieerd onderhoud.
- Upgrades omvatten software-upgrades en veelvoorkomende beveiligings lekken en bloot stellingen.
- Door de Cloud provider geïnitieerd onderhoud omvat netwerk-, opslag-en regionale storingen. Het onderhoud is afhankelijk van de Cloud provider en is afhankelijk van door de provider geleverde updates.

### <a name="what-about-emergency-vs-planned-maintenance-windows"></a>Hoe zit het met nood en geplande onderhouds Vensters?

Er wordt geen onderscheid gemaakt tussen de twee soorten onderhoud. Onze teams zijn beschikbaar 24/7/365 en gebruiken geen traditionele, geplande ' out-of-uren ' onderhouds Vensters.

### <a name="how-will-the-host-operating-system-and-openshift-software-be-updated"></a>Hoe worden het besturings systeem van de host en open Shift-software bijgewerkt?

De gast besturingssystemen en open Shift-software worden bijgewerkt omdat Azure Red Hat open Shift gebruikmaakt van kleine release versies en patches van upstream open Shift container platform.

### <a name="whats-the-process-to-reboot-the-updated-node"></a>Wat is het proces om het bijgewerkte knoop punt opnieuw op te starten?

Knoop punten worden opnieuw opgestart als onderdeel van een upgrade.

## <a name="cluster-operations"></a>Clusterbewerkingen

### <a name="can-i-use-prometheus-to-monitor-my-applications"></a>Kan ik Prometheus gebruiken voor het bewaken van mijn toepassingen?

Prometheus is vooraf geïnstalleerd en geconfigureerd voor Azure Red Hat open Shift 4. x-clusters. Meer informatie over [cluster bewaking](https://docs.openshift.com/container-platform/3.11/install_config/prometheus_cluster_monitoring.html).

Voor Azure Red Hat open Shift 3,11-clusters kunt u Prometheus in uw naam ruimte implementeren en toepassingen in uw naam ruimte controleren. Zie [Prometheus-exemplaar implementeren in azure Red Hat open Shift cluster](howto-deploy-prometheus.md)voor meer informatie.

### <a name="can-i-use-prometheus-to-monitor-metrics-related-to-cluster-health-and-capacity"></a>Kan ik Prometheus gebruiken voor het bewaken van metrische gegevens met betrekking tot de status en capaciteit van het cluster?

In azure Red Hat open Shift 4. x: Ja.

In azure Red Hat open Shift 3,11: Nee.

### <a name="can-logs-of-underlying-vms-be-streamed-out-to-a-customer-log-analysis-system"></a>Kunnen Logboeken van onderliggende Vm's worden gestreamd naar een logboek analyse systeem van klanten?

Logboeken van onderliggende Vm's worden verwerkt door de beheerde service en worden niet blootgesteld aan klanten.

### <a name="how-can-a-customer-get-access-to-metrics-like-cpumemory-at-the-node-level-to-take-action-to-scale-debug-issues-etc-i-cannot-seem-to-run-kubectl-top-on-an-azure-red-hat-openshift-cluster"></a>Hoe kan een klant toegang krijgen tot metrische gegevens, zoals CPU/geheugen op knooppunt niveau om actie te ondernemen om te schalen, problemen op te lossen, enzovoort? Het is niet mogelijk om kubectl top uit te voeren op een Azure Red Hat open Shift-cluster.

Voor Azure Red Hat open Shift 4. x-clusters bevat de webconsole open Shift alle metrische gegevens op knooppunt niveau. Zie voor meer informatie de documentatie voor Red Hat over het [weer geven van cluster informatie](https://docs.openshift.com/aro/4/web_console/using-dashboard-to-get-cluster-information.html).

Voor Azure Red Hat open Shift 3,11-clusters hebben klanten toegang tot de metrische gegevens van de CPU/het geheugen op knooppunt niveau met behulp van de opdracht `oc adm top nodes` of `kubectl top nodes` met de cluster functie klant-beheerder. Klanten hebben ook toegang tot de metrische gegevens van de CPU/het geheugen van `pods` met de opdracht `oc adm top pods` of `kubectl top pods` .

### <a name="if-we-scale-up-the-deployment-how-do-azure-fault-domains-map-into-pod-placement-to-ensure-all-pods-for-a-service-do-not-get-knocked-out-by-a-failure-in-a-single-fault-domain"></a>Als de implementatie wordt geschaald, hoe kunnen Azure-fout domeinen worden toegewezen aan de pod-plaatsing om ervoor te zorgen dat alle peulen voor een service niet worden uitgesp aard door een storing in één fout domein?

Er zijn standaard vijf fout domeinen bij het gebruik van schaal sets voor virtuele machines in Azure. Elk exemplaar van de virtuele machine in een schaalset wordt in een van deze fout domeinen geplaatst. Dit zorgt ervoor dat toepassingen die zijn geïmplementeerd op de reken knooppunten in een cluster, in afzonderlijke fout domeinen worden geplaatst.

Zie [het juiste aantal fout domeinen voor schaal sets voor virtuele machines kiezen](../virtual-machine-scale-sets/virtual-machine-scale-sets-manage-fault-domains.md)voor meer informatie.

### <a name="is-there-a-way-to-manage-pod-placement"></a>Is er een manier om pod-plaatsing te beheren?

Klanten hebben de mogelijkheid om knoop punten op te halen en labels te bekijken als de beheerder van de klant. Dit biedt een manier om een virtuele machine in de schaalset te richten.

Let op wanneer u specifieke labels gebruikt:

- De hostnaam mag niet worden gebruikt. De hostnaam wordt vaak met upgrades en updates gedraaid en wordt gegarandeerd gewijzigd.
- Als de klant een aanvraag voor specifieke labels of een implementatie strategie heeft, kan dit worden bereikt, maar is er technische inspanningen nodig en wordt deze niet ondersteund.

Zie [pod-plaatsing beheren](https://docs.openshift.com/aro/4/nodes/scheduling/nodes-scheduler-about.html)voor meer informatie.

### <a name="is-the-image-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>Is het installatie kopie register extern beschikbaar, zodat ik hulpprogram ma's zoals Jenkins kan gebruiken?

Voor 4. x-clusters moet u een beveiligd REGI ster beschikbaar maken en verificatie configureren. Zie de volgende Red Hat-documentatie voor meer informatie:

- [Een REGI ster beschikbaar maken](https://docs.openshift.com/aro/4/registry/securing-exposing-registry.html)
- [Toegang tot het REGI ster](https://docs.openshift.com/aro/4/registry/accessing-the-registry.html)

Voor 3,11 clusters is het REGI ster van de docker-installatie kopie beschikbaar. Het docker-REGI ster is beschikbaar via `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` . U kunt ook Azure Container Registry gebruiken.

## <a name="networking"></a>Netwerken

### <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Kan ik een cluster implementeren in een bestaand virtueel netwerk?

In 4. x-clusters kunt u een cluster implementeren in een bestaand VNet.

In 3,11 clusters kunt u geen cluster implementeren in een bestaand VNet. U kunt een Azure Red Hat open Shift 3,11-cluster verbinden met een bestaand VNet via peering.

### <a name="is-cross-namespace-networking-supported"></a>Wordt meerdere naam ruimte netwerken ondersteund?

Klanten en afzonderlijke project beheerders kunnen meerdere naam ruimte netwerken (inclusief het weigeren) aanpassen per project met behulp van `NetworkPolicy` objecten.

### <a name="i-am-trying-to-peer-into-a-virtual-network-in-a-different-subscription-but-getting-failed-to-get-vnet-cidr-error"></a>Ik probeer te koppelen aan een virtueel netwerk in een ander abonnement, maar kan geen VNet CIDR-fout ophalen.

Zorg er in het abonnement met het virtuele netwerk voor dat u `Microsoft.ContainerService` de provider registreert met de volgende opdracht:`az provider register -n Microsoft.ContainerService --wait`

### <a name="can-we-specify-ip-ranges-for-deployment-on-the-private-vnet-avoiding-clashes-with-other-corporate-vnets-once-peered"></a>Kunnen we IP-bereiken voor implementatie op het privé-VNet opgeven, waardoor er geen conflict is met andere bedrijfs VNets zodra het peerend is?

In 4. x-clusters kunt u uw eigen IP-bereiken opgeven.

In 3,11-clusters biedt Azure Red Hat open Shift ondersteuning voor VNet-peering en kan de klant een VNet naar de peer bieden en een VNet-CIDR waarin het open Shift-netwerk werkt.

Het VNet dat door Azure Red Hat open Shift is gemaakt, wordt beveiligd en accepteert geen configuratie wijzigingen. Het VNet dat wordt beheerd door de klant en zich in het abonnement bevindt.

### <a name="is-the-software-defined-network-module-configurable"></a>Is de door software gedefinieerde netwerk module configureerbaar?

Het door software gedefinieerde netwerk is `openshift-ovs-networkpolicy` en kan niet worden geconfigureerd.

### <a name="what-azure-load-balancer-is-used-by-azure-red-hat-openshift--is-it-standard-or-basic-and-is-it-configurable"></a>Welke Azure Load Balancer wordt gebruikt door Azure Red Hat open Shift?  Is it Standard of Basic en kan het worden geconfigureerd?

Azure Red Hat open SHIFT gebruikt standaard Azure Load Balancer en kan niet worden geconfigureerd.

## <a name="permissions"></a>Machtigingen

### <a name="can-an-admin-manage-users-and-quotas"></a>Kan een beheerder gebruikers en quota's beheren?

Ja. Een Azure Red Hat open Shift-beheerder kan gebruikers en quota's beheren naast toegang tot alle door de gebruiker gemaakte projecten.

### <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Kan ik een cluster beperken tot bepaalde Azure AD-gebruikers?

Ja. U kunt beperken welke Azure AD-gebruikers zich kunnen aanmelden bij een cluster door de Azure AD-toepassing te configureren. Zie [How to: uw app beperken tot een set gebruikers](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md)voor meer informatie.

### <a name="can-i-restrict-users-from-creating-projects"></a>Kan ik voor komen dat gebruikers projecten maken?

Ja. Meld u aan bij uw cluster als beheerder en voer deze opdracht uit:

```
oc adm policy \
    remove-cluster-role-from-group self-provisioner \
    system:authenticated:oauth
```

Zie voor meer informatie de open Shift-documentatie over het uitschakelen van zelf inrichting voor uw cluster versie:

- [Zelf inrichting in 4,3 clusters uitschakelen](https://docs.openshift.com/aro/4/applications/projects/configuring-project-creation.html#disabling-project-self-provisioning_configuring-project-creation)
- [Zelf inrichting in 3,11 clusters uitschakelen](https://docs.openshift.com/container-platform/3.11/admin_guide/managing_projects.html#disabling-self-provisioning)

### <a name="which-unix-rights-in-iaas-are-available-for-mastersinfraapp-nodes"></a>Welke UNIX-rechten (in IaaS) zijn beschikbaar voor modellen/infra structuur/app-knoop punten?

Voor 4. x-clusters is toegang tot knoop punten beschikbaar via de rol cluster beheerder. Zie voor meer informatie [RBAC-overzicht](https://docs.openshift.com/container-platform/4.3/authentication/using-rbac.html).

Voor 3,11-clusters is toegang tot knoop punten verboden.

### <a name="which-ocp-rights-do-we-have-cluster-admin-project-admin"></a>Welke OCP-rechten hebben we? Cluster-beheerder? Project-beheerder?

Voor 4. x-clusters is de rol cluster-beheerder beschikbaar. Zie voor meer informatie [RBAC-overzicht](https://docs.openshift.com/container-platform/4.3/authentication/using-rbac.html).

Voor 3,11-clusters raadpleegt u het [overzicht van Cluster beheer](https://docs.openshift.com/aro/admin_guide/index.html) voor meer informatie.

### <a name="which-identity-providers-are-available"></a>Welke id-providers zijn beschikbaar?

Voor 4. x-clusters configureert u uw eigen id-provider. Zie voor meer informatie de documentatie van Red Hat over het [configureren van identiteits-uitduikers](https://docs.openshift.com/aro/4/authentication/identity_providers/configuring-ldap-identity-provider.html).

Voor 3,11-clusters kunt u gebruikmaken van de Azure AD-integratie. 

## <a name="storage"></a>Storage

### <a name="is-data-on-my-cluster-encrypted"></a>Versleutelde gegevens op mijn cluster?

Standaard worden gegevens in rust versleuteld. Op het Azure Storage-platform worden uw gegevens automatisch versleuteld voordat deze persistent worden gemaakt en worden de gegevens gedecodeerd voordat ze worden opgehaald. Zie [Azure Storage-service versleuteling voor Data-at-rest](../storage/common/storage-service-encryption.md)voor meer informatie.

### <a name="is-data-stored-in-etcd-encrypted-on-azure-red-hat-openshift"></a>Worden gegevens die zijn opgeslagen in etcd versleuteld op Azure Red Hat open Shift?

Voor Azure Red Hat open Shift 4-clusters worden gegevens niet standaard versleuteld, maar u hebt de mogelijkheid om versleuteling in te scha kelen. Zie de hand leiding voor het [versleutelen van etcd](https://docs.openshift.com/container-platform/4.3/authentication/encrypting-etcd.html)voor meer informatie.

Voor 3,11-clusters worden gegevens niet versleuteld op het niveau van de etcd. De optie om versleuteling in te scha kelen, wordt momenteel niet ondersteund. Open Shift ondersteunt deze functie, maar technische inspanningen zijn vereist om deze op de weg kaart te maken. De gegevens worden versleuteld op schijf niveau. Zie [gegevens versleutelen in de Data Store-laag](https://docs.openshift.com/container-platform/3.11/admin_guide/encrypting_data.html) voor meer informatie.

### <a name="can-we-choose-any-persistent-storage-solution-like-ocs"></a>Kunnen we een permanente opslag oplossing kiezen, zoals OCS? 

Voor 4. x-clusters wordt Azure-schijf (Premium_LRS) geconfigureerd als de standaard-opslag klasse. Zie de documentatie van Red Hat over [permanente opslag](https://docs.openshift.com/aro/4/storage/understanding-persistent-storage.html)voor extra opslag providers en voor meer informatie over de configuratie (inclusief Azure file).

Voor 3,11 clusters worden standaard twee opslag klassen gegeven: één voor Azure-schijf (Premium_LRS) en één voor Azure-bestand.
