---
title: Overzicht van virtuele netwerk isolatie en privacy
titleSuffix: Azure Machine Learning
description: Gebruik een geïsoleerd Azure-Virtual Network met Azure Machine Learning voor het beveiligen van werkruimte bronnen en reken omgevingen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: peterlu
author: peterclu
ms.date: 09/25/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, references_regions, contperfq1
ms.openlocfilehash: c1d351cf7a3d66f6051bf5c9d17672b054c2d862
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/29/2020
ms.locfileid: "91538379"
---
# <a name="virtual-network-isolation-and-privacy-overview"></a>Overzicht van virtuele netwerk isolatie en privacy

In dit artikel leert u hoe u virtuele netwerken (VNets) kunt gebruiken voor het beveiligen van netwerk communicatie in Azure Machine Learning. In dit artikel wordt een voorbeeld scenario gebruikt om te laten zien hoe u een volledig virtueel netwerk configureert.

Dit artikel maakt deel uit van een reeks van vijf delen die u begeleidt bij het beveiligen van een Azure Machine Learning werk stroom. We raden u ten zeerste aan dit overzichts artikel te lezen om de concepten eerst te begrijpen. 

Dit zijn de andere artikelen in deze serie:

**1. VNet-overzicht**  >  [2. Beveilig de werk ruimte](how-to-secure-workspace-vnet.md)  >  [3. De trainings omgeving](how-to-secure-training-vnet.md)  >  [4 beveiligen. Beveilig de vergoedings omgeving](how-to-secure-inferencing-vnet.md)  >  [5. De functionaliteit van Studio inschakelen](how-to-enable-studio-virtual-network.md)

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend bent met de volgende onderwerpen:
+ [Virtuele netwerken van Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
+ [IP-netwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
+ [Azure Private Link](how-to-configure-private-link.md)
+ [Netwerk beveiligings groepen (NSG)](../virtual-network/security-overview.md)
+ [Netwerk firewalls](../firewall/overview.md)

## <a name="example-scenario"></a>Voorbeeldscenario

In deze sectie leert u hoe een gemeen schappelijk netwerk scenario is ingesteld om Azure Machine Learning communicatie met persoonlijke IP-adressen te beveiligen.

De volgende tabel vergelijkt de manier waarop Services toegang hebben tot verschillende onderdelen van een Azure Machine Learning netwerk met een VNet en zonder een VNet.

| Scenario | Werkruimte | Gekoppelde resources | Trainings Compute-omgeving | Reken omgeving afwijzen |
|-|-|-|-|-|-|
|**Geen virtueel netwerk**| Openbare IP | Openbare IP | Openbare IP | Openbare IP |
|**Resources in een virtueel netwerk beveiligen**| Privé-IP (persoonlijk eind punt) | Openbaar IP-adres (Service-eind punt) <br> **of** <br> Privé-IP (persoonlijk eind punt) | Privé IP-adres | Privé IP-adres  | 

* **Werk ruimte** : Maak een persoonlijk eind punt van uw VNet om verbinding te maken met een privé-koppeling in de werk ruimte. Het persoonlijke eind punt verbindt de werk ruimte met het vnet via verschillende privé IP-adressen.
* Gekoppelde service-eind punten of privé-eind punten **van resources** gebruiken om verbinding te maken met werkruimte bronnen zoals Azure storage, Azure Key Vault en Azure container Services.
    * **Service-eind punten** bieden de identiteit van uw virtuele netwerk aan de Azure-service. Wanneer u service-eind punten in uw virtuele netwerk hebt ingeschakeld, kunt u een regel voor het virtuele netwerk toevoegen om de Azure-service bronnen te beveiligen in uw virtuele netwerk. Service-eind punten gebruiken open bare IP-adressen.
    * **Privé-eind punten** zijn netwerk interfaces waarmee u een beveiligde verbinding kunt maken met een service van een persoonlijke Azure-koppeling. Privé-eind punt gebruikt een privé-IP-adres uit uw VNet, waardoor de service effectief in uw VNet wordt gezet.
* **Training Compute Access** -Access-Compute-doelen zoals Azure machine learning Compute-instantie en Azure machine learning reken clusters veilig met privé-IP-adressen. 
* **Berekenen van Compute Access** -Access Azure Kubernetes Services (AKS)-reken clusters met persoonlijke IP-adressen.


In de volgende vijf secties ziet u hoe u het netwerk scenario kunt beveiligen dat hierboven wordt beschreven. Als u uw netwerk wilt beveiligen, moet u het volgende doen:

1. Beveilig de [**werk ruimte en de bijbehorende resources**](#secure-the-workspace-and-associated-resources).
1. Beveilig de [**trainings omgeving**](#secure-the-training-environment).
1. Beveilig de [**omgeving**](#secure-the-inferencing-environment)voor het afwijzen van interferentie.
1. Optioneel: [**Schakel de Studio-functionaliteit in**](#optional-enable-studio-functionality).
1. [ **Firewall instellingen** configureren](#configure-firewall-settings)

## <a name="secure-the-workspace-and-associated-resources"></a>De werk ruimte en de bijbehorende resources beveiligen

Gebruik de volgende stappen om uw werk ruimte en de bijbehorende resources te beveiligen. Met deze stappen kunnen uw services communiceren in het virtuele netwerk.

1. Maak een [persoonlijke werk ruimte voor koppelen](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint) om communicatie tussen uw VNet en de werk ruimte mogelijk te maken.
1. Voeg Azure Key Vault toe aan het virtuele netwerk met een [service-eind punt](../key-vault/general/overview-vnet-service-endpoints.md) of een [persoonlijk eind punt](../key-vault/general/private-link-service.md). Stel Key Vault in op [' vertrouwde micro soft-services mogen deze firewall overs Laan '](how-to-secure-workspace-vnet.md#secure-azure-key-vault).
1. Een Azure Storage-account toevoegen aan het virtuele netwerk met een [service-eind punt](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts) of een [persoonlijk eind punt](../storage/common/storage-private-endpoints.md)
1. [Configureer Azure container Registry voor het gebruik van een persoonlijk eind punt](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr) en [Schakel subnet delegering in azure container instances in](how-to-secure-inferencing-vnet.md#enable-azure-container-instances-aci).

![Architectuur diagram dat laat zien hoe de werk ruimte en gekoppelde resources met elkaar communiceren via service-eind punten of particuliere eind punten binnen een VNet](./media/how-to-network-security-overview/secure-workspace-resources.png)

Zie [een Azure machine learning-werk ruimte beveiligen](how-to-secure-workspace-vnet.md)voor gedetailleerde instructies voor het uitvoeren van deze stappen. 

### <a name="limitations"></a>Beperkingen

Voor het beveiligen van uw werk ruimte en de bijbehorende resources binnen een virtueel netwerk gelden de volgende beperkingen:
- De persoonlijke werkruimte koppeling is alleen beschikbaar in de volgende regio's:
    - **VS - oost**
    - **South Central US**
    - **VS - west**
    - **West US 2**
    - **Centraal-Canada**
    - **Azië - zuidoost**
    - **Japan East**
    - **Europa - noord**
    - **Oost-Australië**
    - **Verenigd Koninkrijk Zuid**
    
    Deze beperking is niet van toepassing op de bijbehorende resources. U kunt bijvoorbeeld VNet inschakelen voor opslag in een Azure Machine Learning regio.
- Alle resources moeten zich achter hetzelfde VNet bevindt. Subnetten binnen hetzelfde VNet zijn echter toegestaan.

## <a name="secure-the-training-environment"></a>De trainings omgeving beveiligen

In deze sectie leert u hoe u de trainings omgeving in Azure Machine Learning kunt beveiligen. U leert ook hoe Azure Machine Learning een trainings taak voltooit om te begrijpen hoe de netwerk configuraties samen werken.

Gebruik de volgende stappen om de trainings omgeving te beveiligen:

1. Maak een Azure Machine Learning [Compute-exemplaar en computer cluster in het virtuele netwerk](how-to-secure-training-vnet.md#compute-instance) om de trainings taak uit te voeren.
1. [Sta binnenkomende communicatie van Azure batch service toe](how-to-secure-training-vnet.md#mlcports) zodat de batch-service taken kan verzenden naar uw reken resources. 

![Architectuur diagram dat laat zien hoe u beheerde reken clusters en instanties kunt beveiligen](./media/how-to-network-security-overview/secure-training-environment.png)

Zie [een trainings omgeving beveiligen](how-to-secure-training-vnet.md)voor meer informatie over het uitvoeren van deze stappen. 

### <a name="example-training-job-submission"></a>Voor beeld van het verzenden van trainings taken 

In deze sectie leert u hoe Azure Machine Learning veilig communiceert tussen services om een trainings taak te verzenden. Hier ziet u hoe alle configuraties samen werken om de communicatie te beveiligen.

1. De-client uploadt trainings scripts en trainings gegevens naar opslag accounts die zijn beveiligd met een service of persoonlijk eind punt.

1. De client verzendt een trainings taak naar de Azure Machine Learning-werk ruimte via het persoonlijke eind punt.

1. Azure Batch Services ontvangt de taak vanuit de werk ruimte en verzendt de trainings taak naar de reken omgeving via de open bare load balancer die is ingericht met de reken resource. 

1. De reken resource ontvangt de taak en begint met de training. De reken resources hebben toegang tot beveiligde opslag accounts voor het downloaden van trainings bestanden en het uploaden van uitvoer. 

![Architectuur diagram waarin wordt getoond hoe een Azure Machine Learning trainings taak wordt verzonden met behulp van een VNet](./media/how-to-network-security-overview/secure-training-job-submission.png)


### <a name="limitations"></a>Beperkingen

- Azure Compute-exemplaren en Azure compute-clusters moeten zich in hetzelfde VNet, dezelfde regio en hetzelfde abonnement bevinden als de werk ruimte en de bijbehorende resources. 

## <a name="secure-the-inferencing-environment"></a>De omgeving voor het afwijzen beveiligen

In deze sectie vindt u informatie over de beschik bare opties voor het beveiligen van een omgeving voor het afwijzen van een interferentie. U wordt aangeraden Azure Kubernetes Services-clusters (AKS) te gebruiken voor productie-implementaties met een hoge schaal.

U hebt twee opties voor AKS-clusters in een virtueel netwerk:

- Implementeer of koppel een standaard AKS-cluster aan uw VNet.
- Koppel een persoonlijk AKS-cluster aan uw VNet.

**Standaard AKS-clusters** hebben een besturings vlak met open bare IP-adressen. U kunt tijdens de implementatie een standaard AKS-cluster toevoegen aan uw VNet of een cluster koppelen nadat het is gemaakt.

**Persoonlijke AKS-clusters** hebben een besturings vlak, dat alleen toegankelijk is via privé ip's. Persoonlijke AKS-clusters moeten worden gekoppeld nadat het cluster is gemaakt.

Voor gedetailleerde instructies over het toevoegen van standaard-en privé clusters, zie een omgeving voor het afwijzen van [een interferentie beveiligen](how-to-secure-inferencing-vnet.md). 

In het volgende netwerk diagram ziet u een beveiligde Azure Machine Learning-werk ruimte met een persoonlijk AKS-cluster dat is gekoppeld aan het virtuele netwerk.

![Architectuur diagram waarin wordt getoond hoe een persoonlijk AKS-cluster moet worden gekoppeld aan het virtuele netwerk. Het AKS-besturings vlak wordt buiten het klant VNet geplaatst](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>Beperkingen
- AKS-clusters moeten deel uitmaken van hetzelfde VNet als de werk ruimte en de bijbehorende resources. 

## <a name="optional-enable-studio-functionality"></a>Optioneel: de functionaliteit van Studio inschakelen

[De werk ruimte beveiligen](#secure-the-workspace-and-associated-resources)  >  [De trainings omgeving beveiligen](#secure-the-training-environment)  >  [De omgeving](#secure-the-inferencing-environment)  >  voor het afwijzen beveiligen De **functionaliteit**  >  van Studio inschakelen [Firewall instellingen configureren](#configure-firewall-settings)

Hoewel de Studio toegang kan krijgen tot gegevens in een opslag account dat is geconfigureerd met een service-eind punt, zijn sommige functies standaard uitgeschakeld:

* Bekijk de gegevens in de Studio.
* Gegevens visualiseren in de ontwerp functie.
* Een AutoML-experiment verzenden.
* Een label project starten.

Zie [Azure machine learning Studio gebruiken in een virtueel netwerk](how-to-enable-studio-virtual-network.md#access-data-using-the-studio)als u de volledige functionaliteit wilt inschakelen wanneer u een service-eind punt gebruikt. De Studio ondersteunt zowel service-eind punten als persoonlijke eind punten voor opslag accounts.

### <a name="limitations"></a>Beperkingen
- De studio heeft geen toegang tot gegevens in opslag accounts die zijn geconfigureerd voor het gebruik van privé-eind punten. Voor volledige functionaliteit moet u service-eind punten voor opslag gebruiken en beheerde identiteit gebruiken.

## <a name="configure-firewall-settings"></a>Firewallinstellingen configureren

Configureer uw firewall om de toegang tot uw Azure Machine Learning werkruimte bronnen en het open bare Internet te beheren. We raden u aan Azure Firewall te kunnen gebruiken voor het beveiligen van uw netwerk. Als u vragen hebt over het toestaan van communicatie via uw firewall, raadpleegt u de documentatie voor de firewall die u gebruikt.

Zie [werk ruimte gebruiken achter een firewall](how-to-access-azureml-behind-firewall.md)voor meer informatie over Firewall instellingen.

## <a name="next-steps"></a>Volgende stappen

Dit artikel maakt deel uit van een virtuele netwerk reeks van vier delen. Raadpleeg de rest van de artikelen voor meer informatie over het beveiligen van een virtueel netwerk:

* [Deel 2: overzicht van virtuele netwerken](how-to-secure-workspace-vnet.md)
* [Deel 3: de trainings omgeving beveiligen](how-to-secure-training-vnet.md)
* [Deel 4: de omgeving voor het afwijzen van interferentie beveiligen](how-to-secure-inferencing-vnet.md)
* [Deel 5: de functionaliteit van Studio inschakelen](how-to-enable-studio-virtual-network.md)
