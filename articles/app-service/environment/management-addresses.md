---
title: App Service-omgeving beheeradressen - Azure
description: Geeft een lijst van de beheeradressen die zijn gebruikt voor de opdracht een App Service Environment
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: f76dd423cb3f7fbae6cc88d064e49dc2d56f1a1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60766052"
---
# <a name="app-service-environment-management-addresses"></a>Beheeradressen van App Service-omgeving

De App Service Environment (ASE) is een implementatie met één tenant van de Azure App Service die wordt uitgevoerd in uw Azure Virtual Network (VNet).  Terwijl de as-omgeving wordt uitgevoerd in uw VNet, moet het nog steeds toegankelijk is vanaf een aantal toegewezen IP-adressen die worden gebruikt door de Azure App Service voor het beheren van de service zijn.  In het geval van een as-omgeving, over de gebruiker beheerde netwerk wordt het beheer van verkeer verzonden. Als dit verkeer wordt geblokkeerd of gerouteerde, wordt de as-omgeving worden onderbroken. Lees voor meer informatie over de afhankelijkheden van ASE-netwerken, [netwerken overwegingen en de App Service Environment][networking]. Voor algemene informatie over de as-omgeving, kunt u beginnen met [Inleiding tot App Service Environment][intro].

Alle as-omgevingen hebben een openbare VIP die beheer van verkeer binnenkomt. Het binnenkomende beheerverkeer van deze adressen komen binnen uit poorten 454 en 455 op het openbare VIP-adres van de as-omgeving. Dit document geeft een lijst van de App Service-bron-adressen voor beheer van verkeer naar de as-omgeving. Deze adressen zijn ook in de IP-Service-Tag met de naam AppServiceManagement.

De hieronder aangegeven adressen kunnen worden geconfigureerd in een routetabel om te voorkomen dat problemen met asymmetrische routering met het beheer van verkeer. Routes reageren op verkeer op het niveau van de IP- en ook geen een bewust te maken van de richting van verkeer of die het verkeer is een onderdeel van een TCP-antwoordbericht. Als het antwoordadres op dat voor een TCP-aanvraag anders dan het adres dat het is verzonden is, hebt u een probleem met asymmetrische routering. Om te voorkomen dat problemen met asymmetrische routering met uw ASE beheer van verkeer, moet u ervoor zorgen dat reacties terug vanaf hetzelfde adres die ze zijn verzonden om te worden verzonden. Lees voor meer informatie over het configureren van de as-omgeving werkt in een omgeving waar het uitgaande verkeer on-premises wordt verzonden, [uw ASE met geforceerde tunneling configureren][forcedtunnel]

## <a name="list-of-management-addresses"></a>Lijst met beheeradressen ##

| Regio | Adressen |
|--------|-----------|
| Alle openbare regio 's | 13.64.115.203, 13.75.127.117, 13.94.141.115, 13.94.143.126, 13.94.149.179, 23.102.135.246, 23.102.188.65, 40.83.120.64, 40.83.121.56, 40.83.125.161, 40.124.47.188, 52.151.25.45, 52.165.152.214, 52.165.153.122, 52.165.154.193, 52.165.158.140, 52.174.22.21, 52.178.177.147, 52.178.184.149, 52.178.190.65, 52.178.195.197, 52.187.56.50, 52.187.59.251, 52.187.63.19, 52.187.63.37, 52.224.105.172, 52.225.177.153, 65.52.14.230, 65.52.172.237, 65.52.193.203, 70.37.57.58, 70.37.89.222, 104.44.129.141, 104.44.129.243, 104.44.129.255, 104.44.134.255, 104.208.54.11, 157.55.176.93, 157.55.208.185, 191.236.154.88 |
| Microsoft Azure en de overheid | 23.97.29.209, 13.72.53.37, 13.72.180.105, 23.97.0.17, 23.97.16.184 |

## <a name="configuring-a-network-security-group"></a>Configureren van een Netwerkbeveiligingsgroep

Met Netwerkbeveiligingsgroepen hoeft u geen zorgen maken over de afzonderlijke adressen of het onderhouden van uw eigen configuratie. Er is een IP-servicetag AppServiceManagement die met alle adressen up-to-date wordt gehouden met de naam. Voor het gebruik van deze service IP-tag in uw NSG, gaat u naar de portal, opent u uw netwerk Security groepen-gebruikersinterface en selecteer inkomende beveiligingsregels. Hebt u een bestaande regel voor het van inkomend beheerverkeer, deze bewerken. Als deze NSG is niet gemaakt met de as-omgeving, of als allemaal nieuw is, en selecteer vervolgens **toevoegen**. Selecteer onder de bron vervolgkeuzelijst **servicetag**.  Selecteer onder de servicetag bron **AppServiceManagement**. De bron poortbereiken ingesteld op \*, doel om door te **eventuele**, poortbereiken van doel naar **454 455**, Protocol **TCP**, en de actie die moet worden **toestaan** . Als u de regel maakt, moet u de prioriteit instellen. 

![het maken van een NSG met de servicetag][1]

## <a name="configuring-a-route-table"></a>Een routetabel configureren

De beheeradressen kunnen worden geplaatst in een routetabel, met internet om ervoor te zorgen dat alle binnenkomend beheerverkeer in staat zijn om terug te gaan via hetzelfde pad is de volgende hop. Deze routes zijn nodig bij het configureren van geforceerde tunneling. U kunt de portal, PowerShell of Azure CLI gebruiken voor het maken van de routetabel.  De opdrachten voor het maken van een routetabel met behulp van Azure CLI uit een PowerShell-prompt staan hieronder. 

    $rg = "resource group name"
    $rt = "route table name"
    $location = "azure location"
    az network route-table create --name $rt --resource-group $rg --location $location
    az network route-table route create -g $rg --route-table-name $rt -n 13.64.115.203 --next-hop-type Internet --address-prefix 13.64.115.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.75.127.117 --next-hop-type Internet --address-prefix 13.75.127.117/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.141.115 --next-hop-type Internet --address-prefix 13.94.141.115/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.143.126 --next-hop-type Internet --address-prefix 13.94.143.126/32
    az network route-table route create -g $rg --route-table-name $rt -n 13.94.149.179 --next-hop-type Internet --address-prefix 13.94.149.179/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.135.246 --next-hop-type Internet --address-prefix 23.102.135.246/32
    az network route-table route create -g $rg --route-table-name $rt -n 23.102.188.65 --next-hop-type Internet --address-prefix 23.102.188.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.120.64 --next-hop-type Internet --address-prefix 40.83.120.64/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.121.56 --next-hop-type Internet --address-prefix 40.83.121.56/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.83.125.161 --next-hop-type Internet --address-prefix 40.83.125.161/32
    az network route-table route create -g $rg --route-table-name $rt -n 40.124.47.188 --next-hop-type Internet --address-prefix 40.124.47.188/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.151.25.45 --next-hop-type Internet --address-prefix 52.151.25.45/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.152.214 --next-hop-type Internet --address-prefix 52.165.152.214/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.153.122 --next-hop-type Internet --address-prefix 52.165.153.122/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.154.193 --next-hop-type Internet --address-prefix 52.165.154.193/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.165.158.140 --next-hop-type Internet --address-prefix 52.165.158.140/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.174.22.21 --next-hop-type Internet --address-prefix 52.174.22.21/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.177.147 --next-hop-type Internet --address-prefix 52.178.177.147/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.184.149 --next-hop-type Internet --address-prefix 52.178.184.149/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.190.65 --next-hop-type Internet --address-prefix 52.178.190.65/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.178.195.197 --next-hop-type Internet --address-prefix 52.178.195.197/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.56.50 --next-hop-type Internet --address-prefix 52.187.56.50/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.59.251 --next-hop-type Internet --address-prefix 52.187.59.251/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.19 --next-hop-type Internet --address-prefix 52.187.63.19/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.187.63.37 --next-hop-type Internet --address-prefix 52.187.63.37/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.224.105.172 --next-hop-type Internet --address-prefix 52.224.105.172/32
    az network route-table route create -g $rg --route-table-name $rt -n 52.225.177.153 --next-hop-type Internet --address-prefix 52.225.177.153/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.14.230 --next-hop-type Internet --address-prefix 65.52.14.230/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.172.237 --next-hop-type Internet --address-prefix 65.52.172.237/32
    az network route-table route create -g $rg --route-table-name $rt -n 65.52.193.203 --next-hop-type Internet --address-prefix 65.52.193.203/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.57.58 --next-hop-type Internet --address-prefix 70.37.57.58/32
    az network route-table route create -g $rg --route-table-name $rt -n 70.37.89.222 --next-hop-type Internet --address-prefix 70.37.89.222/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.141 --next-hop-type Internet --address-prefix 104.44.129.141/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.243 --next-hop-type Internet --address-prefix 104.44.129.243/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.129.255 --next-hop-type Internet --address-prefix 104.44.129.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.44.134.255 --next-hop-type Internet --address-prefix 104.44.134.255/32
    az network route-table route create -g $rg --route-table-name $rt -n 104.208.54.11 --next-hop-type Internet --address-prefix 104.208.54.11/32
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.176.93 --next-hop-type Internet --address-prefix 157.55.176.93/32
    az network route-table route create -g $rg --route-table-name $rt -n 157.55.208.185 --next-hop-type Internet --address-prefix 157.55.208.185/32
    az network route-table route create -g $rg --route-table-name $rt -n 191.236.154.88 --next-hop-type Internet --address-prefix 191.236.154.88/32

Nadat de routetabel is gemaakt, moet u deze in te stellen op uw ASE-subnet.  

## <a name="get-your-management-addresses-from-api"></a>Je beheeradressen ophalen uit-API ##

U kunt de beheeradressen die met uw ASE met de volgende API-aanroep overeenkomen kunt weergeven.

    get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01

De API retourneert een JSON-document met inbegrip van alle inkomende-adressen voor de as-omgeving. De lijst met adressen bevat de beheeradressen het VIP-adres gebruikt door de as-omgeving en de adresbereik van de ASE-subnet zelf.  

Voor het aanroepen van de API met de [armclient](https://github.com/projectkudu/ARMClient) gebruik de volgende opdrachten, maar vervang in uw abonnements-ID, resourcegroep en de naam van de as-omgeving.  

    armclient login
    armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01


<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
