---
title: Azure VMware-oplossing door CloudSimple-vSAN-versleuteling configureren voor Privécloud
description: Hierin wordt beschreven hoe u de functie voor vSAN-software versleuteling configureert, zodat uw persoonlijke cloud van CloudSimple kan worden gebruikt in combi natie met een sleutel beheer server die wordt uitgevoerd in uw virtuele Azure-netwerk.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a00cce852878d65ca08927d937f5a0f1f37c1b87
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544472"
---
# <a name="configure-vsan-encryption-for-your-cloudsimple-private-cloud"></a>VSAN-versleuteling voor uw CloudSimple-Privécloud configureren

U kunt de functie voor het versleutelen van vSAN-software configureren zodat uw persoonlijke cloud van CloudSimple kan worden gebruikt in combi natie met een sleutel beheer server die wordt uitgevoerd in uw virtuele Azure-netwerk.

VMware vereist een extern KMIP 1,1-compatibel KMS-hulp programma (Key Management Server) van derden bij het gebruik van vSAN-versleuteling. U kunt elke ondersteunde KMS gebruiken die door VMware is gecertificeerd en beschikbaar is voor Azure.

U gaat als volgt te werk om HyTrust-besturings element KMS uit te voeren in een virtueel Azure-netwerk. Een soort gelijke benadering kan worden gebruikt voor elke andere certificerings oplossing van een derde van derden voor vSAN.

Voor deze KMS-oplossing moet u het volgende doen:

* Installeren, configureren en beheren van een VMware-gecertificeerd KMS-hulp programma van derden in uw virtuele Azure-netwerk.
* Geef uw eigen licenties voor het KMS-hulp programma op.
* Configureer en beheer vSAN-versleuteling in uw Privécloud met behulp van het KMS-hulp programma van derden dat wordt uitgevoerd in uw virtuele Azure-netwerk.

## <a name="kms-deployment-scenario"></a>Implementatie scenario voor KMS

Het KMS-server cluster wordt uitgevoerd in uw virtuele Azure-netwerk en is een IP-adres dat bereikbaar is via de Privécloud-vCenter via de geconfigureerde Azure ExpressRoute-verbinding.

![Het KMS-cluster in het virtuele Azure-netwerk](media/vsan-kms-cluster.png)

## <a name="how-to-deploy-the-solution"></a>De oplossing implementeren

Het implementatie proces bestaat uit de volgende stappen:

1. [Controleren of aan de vereisten wordt voldaan](#verify-that-prerequisites-are-met)
2. [CloudSimple-portal: Informatie over ExpressRoute-peering verkrijgen](#cloudsimple-portal-obtain-expressroute-peering-information)
3. [Azure Portal: Uw virtuele netwerk verbinden met de Privécloud](#azure-portal-connect-your-virtual-network-to-your-private-cloud)
4. [Azure Portal: Implementeer een HyTrust-besturings cluster in uw virtuele netwerk](#azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network)
5. [HyTrust WebUI: KMIP-server configureren](#hytrust-webui-configure-the-kmip-server)
6. [vCenter-gebruikers interface: VSAN-versleuteling configureren voor het gebruik van het KMS-cluster in uw virtuele Azure-netwerk](#vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network)

### <a name="verify-that-prerequisites-are-met"></a>Controleren of aan de vereisten wordt voldaan

Controleer het volgende voordat u de implementatie implementeert:

* De geselecteerde KMS-leverancier, het hulp programma en de versie bevinden zich in de compatibiliteits lijst vSAN.
* De geselecteerde leverancier ondersteunt een versie van het hulp programma om uit te voeren in Azure.
* De Azure-versie van het hulp programma KMS is KMIP 1,1-compatibel.
* Er zijn al een Azure Resource Manager en een virtueel netwerk gemaakt.
* Er is al een CloudSimple-Privécloud gemaakt.

### <a name="cloudsimple-portal-obtain-expressroute-peering-information"></a>CloudSimple-portal: Informatie over ExpressRoute-peering verkrijgen

Als u wilt door gaan met de installatie, hebt u de verificatie sleutel en peer-circuit-URI nodig voor ExpressRoute plus toegang tot uw Azure-abonnement. Deze informatie is beschikbaar op de pagina Virtual Network verbinding in de CloudSimple-Portal. Zie [een virtuele netwerk verbinding instellen op de privécloud](virtual-network-connection.md)voor instructies. Als u problemen hebt met het verkrijgen van de informatie, opent u een ondersteunings <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">aanvraag</a>.

### <a name="azure-portal-connect-your-virtual-network-to-your-private-cloud"></a>Azure Portal: Uw virtuele netwerk verbinden met uw Privécloud

1. Maak een virtuele netwerk gateway voor uw virtuele netwerk door de instructies in [een virtuele netwerk gateway configureren voor ExpressRoute te volgen met behulp van de Azure Portal](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).
2. Koppel uw virtuele netwerk aan het CloudSimple ExpressRoute-circuit door de instructies in [een virtueel netwerk verbinden met een ExpressRoute-circuit te volgen met behulp van de portal](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
3. Gebruik de CloudSimple ExpressRoute-circuit informatie ontvangen in uw welkomst bericht van CloudSimple om uw virtuele netwerk te koppelen aan het CloudSimple ExpressRoute-circuit in Azure.
4. Voer de autorisatie sleutel en de URI van het peer circuit in, geef de verbinding een naam en klik op **OK**.

![De URI van het CS-peer circuit opgeven tijdens het maken van het virtuele netwerk](media/vsan-azureportal01.png)

### <a name="azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network"></a>Azure Portal: Implementeer een HyTrust-besturings cluster in de Azure Resource Manager in uw virtuele netwerk

Als u een HyTrust-besturings cluster wilt implementeren in de Azure Resource Manager in uw virtuele netwerk, voert u de volgende taken uit. Raadpleeg de <a href="https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0" target="_blank">HyTrust-documentatie</a> voor meer informatie.

1. Maak een Azure-netwerk beveiligings groep (NSG-hytrust) met de opgegeven regels voor binnenkomende verbindingen door de instructies in de HyTrust-documentatie te volgen.
2. Genereer een SSH-sleutel paar in Azure.
3. Implementeer het eerste knoop punt voor besturings elementen uit de installatie kopie in azure Marketplace.  Gebruik de open bare sleutel van het sleutel paar dat is gegenereerd en selecteer **NSG-hytrust** als de netwerk beveiligings groep voor het besturings element Besturingselementbron.
4. Converteer het persoonlijke IP-adres van de besturings elementen naar een statisch IP-adres.
5. SSH naar de virtuele machine voor besturings elementen met behulp van het open bare IP-adres en de persoonlijke sleutel van het eerder genoemde sleutel paar.
6. Wanneer u hierom wordt gevraagd in de SSH ```No``` -shell, selecteert u het knoop punt instellen als het eerste knoop punt voor het besturings element.
7. Voeg extra Stuur knooppunten voor besturings elementen toe door stap 3-5 van deze procedure ```Yes``` te herhalen en te selecteren wanneer u wordt gevraagd of u wilt toevoegen aan een bestaand cluster.

### <a name="hytrust-webui-configure-the-kmip-server"></a>HyTrust WebUI: De KMIP-server configureren

Ga naar https://*Public-IP*, waarbij *openbaar IP* het open bare IP-adres is van de VM van het besturings element voor de Besturingselementbron. Volg deze stappen in de <a href="https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0" target="_blank">HyTrust-documentatie</a>.

1. <a href="https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2" target="_blank">Een KMIP-server configureren</a>
2. <a href="https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3" target="_blank">Een certificaat bundel maken voor VMware-versleuteling</a>

### <a name="vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network"></a>vCenter-gebruikers interface: VSAN-versleuteling configureren voor het gebruik van het KMS-cluster in uw virtuele Azure-netwerk

Volg de HyTrust-instructies voor het <a href="https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4" target="_blank">maken van een KMS-cluster in vCenter</a>.

![Details van KMS-cluster toevoegen in vCenter](media/vsan-config01.png)

Ga in vCenter naar **Cluster > configureren** en selecteer **algemene** optie voor vSAN. Schakel versleuteling in en selecteer het KMS-cluster dat eerder aan vCenter is toegevoegd.

![VSAN-versleuteling inschakelen en KMS-cluster configureren in vCenter](media/vsan-config02.png)

## <a name="references"></a>Verwijzingen

### <a name="azure"></a>Azure

[Een virtuele netwerk gateway configureren voor ExpressRoute met behulp van de Azure Portal](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)

[Een virtueel netwerk verbinden aan een ExpressRoute-circuit met behulp van de portal](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)

### <a name="hytrust"></a>HyTrust

[HyTrust DataControl en Microsoft Azure](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0)

[Een KMPI-server configureren](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2)

[Een certificaat bundel maken voor VMware-versleuteling](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3)

[Het KMS-cluster maken in vSphere](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4)
