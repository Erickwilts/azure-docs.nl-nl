---
title: Het opnieuw instellen van de netwerkinterface voor virtuele Azure Windows-machine | Microsoft Docs
description: Laat zien hoe u de netwerkinterface voor Azure Windows VM opnieuw instellen
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 11/16/2018
ms.author: genli
ms.openlocfilehash: 3a8e005f8678deef9fc4aebd2d620619fe6074bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60307284"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Het opnieuw instellen van de netwerkinterface voor Azure Windows VM 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

In dit artikel laat zien hoe de netwerkinterface voor Azure Windows VM het oplossen van problemen wanneer u geen verbinding maken op Microsoft Azure Windows Virtual Machine (VM) na het opnieuw instellen:

* U uitschakelen de standaard Standaardnetwerkinterface (NIC). 
* Stel u handmatig een statisch IP-adres voor de NIC. 

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="reset-network-interface"></a>De netwerkinterface opnieuw instellen

### <a name="for-vms-deployed-in-resource-group-model"></a>Voor virtuele machines die worden geïmplementeerd in de groep resourcemodel

1.  Ga naar de [Azure Portal](https://ms.portal.azure.com).
2.  Selecteer de betreffende virtuele Machine.
3.  Selecteer **netwerken** en selecteer vervolgens de netwerkinterface van de virtuele machine.

    ![De locatie van de netwerk-interface](./media/reset-network-interface/select-network-interface-vm.png)
    
4.  Selecteer **IP-configuraties**.
5.  Selecteer het IP-adres. 
6.  Als de **privé-IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
7.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
8. De virtuele machine wordt opnieuw opgestart om te initialiseren van de nieuwe NIC in het systeem.
9.  Probeer via RDP verbinding naar uw computer. Als dit lukt, kunt u het particuliere IP-adres terug naar de oorspronkelijke wijzigen als u wilt. Anders kunt u deze. 

#### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

1. Zorg ervoor dat u hebt [de nieuwste Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) geïnstalleerd
2. Open een verhoogde Azure PowerShell-sessie (als administrator uitvoeren). Voer de volgende opdrachten uit:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $ResourceGroup = "<Resource Group>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzAccount
    Select-AzSubscription -SubscriptionId $SubscriptionId 
    
    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP

    #Add/Change static IP. This process will not change MAC address
    Get-AzVM -ServiceName $ResourceGroup -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP | Update-AzVM
    ```
3. Probeer via RDP verbinding naar uw computer.  Als dit lukt, kunt u het particuliere IP-adres terug naar de oorspronkelijke wijzigen als u wilt. Anders kunt u deze.

### <a name="for-classic-vms"></a>Voor klassieke VM 's

Als u wilt herstellen van de netwerkinterface, de volgende stappen uit:

#### <a name="use-azure-portal"></a>Azure Portal gebruiken

1.  Ga naar de [Azure Portal]( https://ms.portal.azure.com).
2.  Selecteer **virtuele Machines (klassiek)** .
3.  Selecteer de betreffende virtuele Machine.
4.  Selecteer **IP-adressen**.
5.  Als de **privé-IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
6.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
7.  Selecteer **Opslaan**.
8.  De virtuele machine wordt opnieuw opgestart om te initialiseren van de nieuwe NIC in het systeem.
9.  Probeer via RDP verbinding naar uw computer. Als dit lukt, kunt u kiezen om terug te zetten van de privé IP-adres terug naar de oorspronkelijke.  

#### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

1. Zorg ervoor dat u hebt [de nieuwste Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) geïnstalleerd.
2. Open een verhoogde Azure PowerShell-sessie (als administrator uitvoeren). Voer de volgende opdrachten uit:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $CloudService = "<Cloud Service>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId $SubscriptionId 

    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP
    
    #Add/Change static IP. This process will not change MAC address
    Get-AzureVM -ServiceName $CloudService -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP |Update-AzureVM
    ```
3. Probeer via RDP verbinding naar uw computer. Als dit lukt, kunt u het particuliere IP-adres terug naar de oorspronkelijke wijzigen als u wilt. Anders kunt u deze. 

## <a name="delete-the-unavailable-nics"></a>Het is niet beschikbaar NIC's verwijderen
Nadat u met de machine via Extern bureaublad kunt, moet u de oude NIC's om het potentiële probleem te voorkomen verwijderen:

1.  Open Apparaatbeheer.
2.  Selecteer **weergave** > **verborgen apparaten weergeven**.
3.  Selecteer **netwerkadapters**. 
4.  Controleer of u adapters met de naam als 'Microsoft Hyper-V-netwerkadapter'.
5.  U ziet mogelijk een niet-beschikbare adapter. Deze wordt grijs weergegeven. Met de rechtermuisknop op de adapter en selecteer verwijderen.

    ![de installatiekopie van de NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Verwijder alleen de niet beschikbaar-adapters die de naam 'Microsoft Hyper-V-netwerkadapter' hebben. Als u een van de andere verborgen adapters verwijdert, kan dit ertoe leiden dat andere problemen.
    >
    >

6.  Nu moeten alle niet beschikbaar netwerkadapters worden gewist van uw systeem.
