---
title: Een virtuele machine netwerk routeringsprobleem vaststellen - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u een VM-netwerk routering probleem met behulp van de volgende hop-mogelijkheden van Azure Network Watcher op te sporen.
services: network-watcher
documentationcenter: network-watcher
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 08d273ce6e6ecb1b10d3c39a0954d430a3cb674a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66730739"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Een virtuele machine netwerk routeringsprobleem vaststellen - Azure PowerShell

In dit artikel, kunt u een virtuele machine (VM) implementeren, en controleert op communicatie naar een IP-adres en de URL. U stelt de oorzaak van mislukte communicatie vast en leert hoe u dit probleem kunt oplossen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel Azure PowerShell vereist `Az` module. Voer `Get-Module -ListAvailable Az` uit om na te gaan welke versie er is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.



## <a name="create-a-vm"></a>Een virtuele machine maken

Voordat u een VM kunt maken, maakt u eerst een resourcegroep die de VM bevat. Maak een resourcegroep met behulp van de opdracht [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

Maak de VM met [New-AzVM](/powershell/module/az.compute/new-azvm). Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

```azurepowershell-interactive
$vM = New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Het maken van de virtuele machine duurt een paar minuten. Ga niet door met de resterende stappen totdat de VM is gemaakt en uitvoer wordt geretourneerd met PowerShell.

## <a name="test-network-communication"></a>Netwerkcommunicatie testen

Als u wilt testen netwerkcommunicatie met Network Watcher, moet u eerst een netwerk-watcher in de regio voor de virtuele machine die u wilt testen inschakelen en vervolgens de volgende hop-mogelijkheden van Network Watcher gebruiken om communicatie te testen.

## <a name="enable-network-watcher"></a>Netwerk-watcher inschakelen

Als u al een netwerk-watcher ingeschakeld in de regio VS-Oost, gebruikt u [Get-AzNetworkWatcher](/powershell/module/az.network/get-aznetworkwatcher) om op te halen van de netwerk-watcher. In het volgende voorbeeld wordt een bestaande netwerk-watcher opgehaald met de naam *NetworkWatcher_eastus* die zich in de resourcegroep *NetworkWatcherRG* bevindt:

```azurepowershell-interactive
$networkWatcher = Get-AzNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Als u nog een network watcher ingeschakeld in de regio VS-Oost hebt, gebruikt u [New-AzNetworkWatcher](/powershell/module/az.network/new-aznetworkwatcher) te maken van een network watcher in de regio VS-Oost:

```azurepowershell-interactive
$networkWatcher = New-AzNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Volgende hop gebruiken

Azure maakt automatisch routes naar standaardbestemmingen. U kunt uw eigen, aangepaste routes maken om die standaardroutes te overschrijven. Soms hebben aangepaste routes tot gevolg dat de communicatie mislukt. Als u wilt testen van de routering van een virtuele machine, gebruikt u de [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop) opdracht om te bepalen van de volgende hop routering wanneer verkeer bestemd voor een specifiek adres is.

Uitgaande communicatie van de VM naar een van de IP-adressen testen voor www.bing.com:

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Na enkele seconden, de uitvoer wordt gemeld dat de **NextHopType** is **Internet**, en dat de **RouteTableId** is **Systeemroute**. Dit resultaat laat u weten dat er een geldige route naar de bestemming is.

Uitgaande communicatie van de VM naar 172.31.0.100 testen:

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

De uitvoer die wordt geretourneerd, informeert u die **geen** is de **NextHopType**, en dat de **RouteTableId** is ook **Systeemroute**. Hieruit kunt u afleiden dat er wel een geldige systeemroute is naar de bestemming, maar dat er geen volgende hop is voor het routeren van het verkeer naar de bestemming.

## <a name="view-details-of-a-route"></a>Details van een route weergeven

Voor het analyseren van verdere routering, Controleer de effectieve routes voor de netwerkinterface met de [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable) opdracht:

```azurepowershell-interactive
Get-AzEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Uitvoer met de volgende tekst is geretourneerd:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Zoals u in de vorige uitvoer, de route met ziet de **AddressPrefix** van **0.0.0.0/0** al het verkeer dat niet is bestemd voor adressen binnen de adresvoorvoegsels van andere route met de volgende hop van **Internet**. Zoals u ook in de uitvoer ziet, maar er is een standaardroute voor het voorvoegsel 172.16.0.0/12, waaronder de 172.31.0.100-adres, de **nextHopType** is **geen**. Azure maakt een standaardroute naar 172.16.0.0/12, maar stelt geen volgend hoptype in, tenzij daar een reden voor is. Als u bijvoorbeeld het adresbereik 172.16.0.0/12 toegevoegd aan de adresruimte van het virtuele netwerk, Azure verandert de **nextHopType** naar **virtueel netwerk** voor de route. Vervolgens ziet u een selectievakje **virtueel netwerk** als de **nextHopType**.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, kunt u [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een virtuele machine gemaakt en gediagnosticeerd de routering van de virtuele machine. U hebt geleerd dat Azure verschillende standaardroutes maakt en u hebt de routering naar twee verschillende bestemmingen getest. Lees hier meer over [routering in Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) en hoe u [aangepaste routes maakt](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Voor uitgaande verbindingen van de virtuele machine, u kunt ook bepalen de latentie en toegestane en geweigerde netwerkverkeer tussen de virtuele machine en een eindpunt met behulp van Network Watcher [probleemoplossing voor verbindingen](network-watcher-connectivity-powershell.md) mogelijkheid. U kunt de communicatie tussen een virtuele machine en een eindpunt, zoals een IP-adres of de URL, na verloop van tijd met behulp van de mogelijkheden van Network Watcher connection monitor bewaken. Voor meer informatie Zie [bewaken van een netwerkverbinding](connection-monitor.md).
