---
title: On-premises Hyper-V-servers voorbereiden voor herstel na noodgevallen van Hyper-V-machines naar Azure | Microsoft Docs
description: Informatie over het voorbereiden van on-premises Hyper-V-VM's voor herstel na noodgevallen naar Azure met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: aec5632b5ea29d52426c5d065ca41e18573fd5b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399389"
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>On-premises Hyper-V-servers voorbereiden op herstel na noodgevallen naar Azure

In dit artikel wordt beschreven hoe u uw on-premises Hyper-V-infrastructuur voorbereidt wanneer u wilt instellen van herstel na noodgevallen van Hyper-VM's naar Azure, met behulp van [Azure Site Recovery](site-recovery-overview.md).


Dit is de tweede zelfstudie in een serie die laat zien u hoe u herstel na noodgevallen naar Azure instelt voor on-premises Hyper-V-machines. In de eerste zelfstudie we [instellen van de Azure-onderdelen](tutorial-prepare-azure.md) die nodig zijn voor Hyper-V-noodherstel.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten voor Hyper-V en VMM-vereisten controleren als de Hyper-V-hosts worden beheerd door System Center VMM.
> * Bereid VMM indien van toepassing.
> * Controleer of u toegang tot internet tot Azure-locaties.
> * Virtuele machines voorbereiden, zodat u ze na een failover naar Azure.

> [!NOTE]
> Zelfstudies ziet u het meest eenvoudige implementatie-pad voor een scenario. Waar mogelijk wordt gebruikgemaakt van standaardopties en niet alle mogelijke instellingen en paden worden weergegeven. Lees het artikel in de sectie How To van de Site Recovery inhoudsopgave voor gedetailleerde instructies.

## <a name="before-you-start"></a>Voordat u begint

Zorg ervoor dat u hebt voorbereid Azure zoals wordt beschreven in de [eerste zelfstudie in deze reeks](tutorial-prepare-azure.md).

## <a name="review-requirements-and-prerequisites"></a>Vereisten controleren en vereisten

Zorg ervoor dat Hyper-V-hosts en virtuele machines te voldoen aan de vereisten.

1. [Controleer of](hyper-v-azure-support-matrix.md#on-premises-servers) on-premises serververeisten.
2. [Controleer de vereisten](hyper-v-azure-support-matrix.md#replicated-vms) voor Hyper-V-machines die u wilt repliceren naar Azure.
3. Controleer de Hyper-V-host [netwerken](hyper-v-azure-support-matrix.md#hyper-v-network-configuration); en de host en de Gast [opslag](hyper-v-azure-support-matrix.md#hyper-v-host-storage) ondersteuning voor on-premises Hyper-V-hosts.
4. Controleer wat er na failover wordt ondersteund voor [Azure-netwerken](hyper-v-azure-support-matrix.md#azure-vm-network-configuration-after-failover), [-opslag](hyper-v-azure-support-matrix.md#azure-storage) en [-rekenkracht](hyper-v-azure-support-matrix.md#azure-compute-features).
5. De on-premises VM's die u voor Azure repliceert, moeten overeenstemmen met [Azure VM-vereisten](hyper-v-azure-support-matrix.md#azure-vm-requirements).


## <a name="prepare-vmm-optional"></a>Voorbereiden van VMM (optioneel)

Als Hyper-V-hosts worden beheerd door VMM, moet u de on-premises VMM-server voorbereiden. 

- Zorg ervoor dat de VMM-server is een minimaal één cloud, met een of meer hostgroepen. De Hyper-V-host waarop de virtuele machines worden uitgevoerd, moet zich bevinden in de cloud.
- De VMM-server voorbereiden op netwerktoewijzing.

### <a name="prepare-vmm-for-network-mapping"></a>VMM voorbereiden op netwerktoewijzing

Als u VMM, [netwerktoewijzing](site-recovery-network-mapping.md) toewijzingen tussen on-premises VMM-VM-netwerken en virtuele netwerken van Azure. Toewijzing zorgt ervoor dat de Azure-VM's zijn verbonden met het juiste netwerk wanneer ze na een failover worden gemaakt.

Voorbereiden op VMM netwerktoewijzing als volgt:

1. Zorg ervoor dat u hebt een [logisch netwerk van VMM](https://docs.microsoft.com/system-center/vmm/network-logical) die is gekoppeld aan de cloud waarin de Hyper-V-hosts zich bevinden.
2. Zorg ervoor dat u hebt een [VM-netwerk](https://docs.microsoft.com/system-center/vmm/network-virtual) gekoppeld aan het logische netwerk.
3. In VMM, verbinding maken met de virtuele machines met VM-netwerk.

## <a name="verify-internet-access"></a>Controleer of u toegang tot internet

1. Voor de doeleinden van de zelfstudie is de eenvoudigste configuratie voor de Hyper-V-hosts en VMM-server rechtstreeks toegang tot internet hebben zonder een proxy. 
2. Zorg ervoor dat Hyper-V-hosts en de VMM-server, indien van toepassing, toegang tot de vereiste URL's die hieronder.   
3. Als u bent toegangsbeheer op basis van IP-adres, controleert u of:
    - IP-adressen gebaseerde firewallregels kunnen verbinding maken met [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en de poort HTTPS (443).
    - Toestaan dat IP-adresbereiken voor de Azure-regio van uw abonnement.
    
### <a name="required-urls"></a>Vereiste URL 's


[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Tijdens een failover-scenario kunt u verbinding maken met uw gerepliceerde on-premises netwerk.

Voor verbinding met virtuele Windows-machines na een failover met RDP, als volgt toegang toestaan:

1. Om toegang te krijgen via het internet, schakelt u voor de failover RDP in op de on-premises VM. Zorg ervoor dat TCP- en UDP-regels zijn toegevoegd voor het profiel **Openbaar** en dat RDP is toegestaan in **Windows Firewall** > **Toegestane apps** voor alle profielen.
2. Voor toegang via VPN tussen sites schakelt u RDP in op de on-premises computer. RDP moet toegestaan zijn in de **Windows Firewall** -> **Toegestane apps en onderdelen** voor **Domein en Privé**-netwerken.
   Controleer of het SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135). Er mogen geen Windows-updates in behandeling zijn op de VM wanneer u een failover activeert. Als er, kunt u zich niet aanmelden bij de virtuele machine totdat de update is voltooid.
3. Controleer na een failover **Diagnostische gegevens over opstarten** op de Windows Azure VM om een schermopname van de VM weer te geven. Als u geen verbinding kunt maken, controleer dan of de VM actief is en bekijk deze [tips voor het oplossen van problemen](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Na een failover, kunt u toegang tot Azure-VM's met behulp van hetzelfde IP-adres als de gerepliceerde on-premises VM, of een ander IP-adres. [Meer informatie](concepts-on-premises-to-azure-networking.md) over het instellen van het IP-adressering voor failover.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Instellen van herstel na noodgevallen naar Azure voor Hyper-V-machines](tutorial-hyper-v-to-azure.md)
> [instellen van herstel na noodgevallen naar Azure voor Hyper-V-machines in VMM-clouds](tutorial-hyper-v-vmm-to-azure.md)
