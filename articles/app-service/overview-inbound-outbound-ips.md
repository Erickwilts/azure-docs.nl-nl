---
title: Inkomend/uitgaand IP-adressen - Azure App Service | Microsoft Docs
description: Hierin wordt beschreven hoe binnenkomende en uitgaande IP-adressen worden gebruikt in App Service en hoe u informatie over deze voor uw app.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: de9ae8e5c0cbf0997811db9624f6c6b92e03a5df
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66742937"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Binnenkomende en uitgaande IP-adressen in Azure App Service

[Azure App Service](overview.md) is een multitenant-service, met uitzondering van [App Service-omgevingen](environment/intro.md). Apps die zich niet in een App Service environment (niet in de [geïsoleerde laag](https://azure.microsoft.com/pricing/details/app-service/)) de netwerkinfrastructuur delen met andere apps. Als gevolg hiervan de binnenkomende en uitgaande IP-adressen van een app kunnen afwijken en zelfs in bepaalde situaties kunnen wijzigen. 

[App Service-omgevingen](environment/intro.md) speciaal netwerk-infrastructuren gebruiken, zodat apps die worden uitgevoerd in een App Service environment statische, toegewezen IP-adressen voor binnenkomende en uitgaande verbindingen.

## <a name="when-inbound-ip-changes"></a>Wanneer de inkomende IP verandert

Elke app heeft, ongeacht het aantal exemplaren van de scale-out, één inkomende IP-adres. De inkomende IP-adres kan worden gewijzigd wanneer u een van de volgende acties uitvoeren:

- Een app verwijderen en opnieuw maken in een andere resourcegroep.
- De laatste app in een resourcegroep verwijderen _en_ regio combinatie en maak deze opnieuw.
- Verwijderen van een bestaand SSL-binding, zoals tijdens het vernieuwen van een certificaat (Zie [certificaten vernieuwen](app-service-web-tutorial-custom-ssl.md#renew-certificates)).

## <a name="find-the-inbound-ip"></a>Het inkomende IP-adres zoeken

Voer alleen de volgende opdracht in een lokale terminal:

```bash
nslookup <app-name>.azurewebsites.net
```

## <a name="get-a-static-inbound-ip"></a>Een statische inkomende IP-adres ophalen

Soms wilt u mogelijk een speciale, statische IP-adres voor uw app. Als u een statisch inkomende IP-adres, die u wilt configureren een [op basis van IP SSL-binding](app-service-web-tutorial-custom-ssl.md#secure-a-custom-domain). Als u niet echt nodig voor SSL-functionaliteit hebt voor het beveiligen van uw app, kunt u ook een zelfondertekend certificaat voor deze binding uploaden. In een IP-gebaseerd SSL-binding is het certificaat gebonden aan het IP-adres zelf, dit het geval is Appservice-bepalingen een statisch IP-adres te realiseren. 

## <a name="when-outbound-ips-change"></a>Als de uitgaande IP-adressen wijzigen

Ongeacht het aantal exemplaren van de scale-out heeft elke app een bepaald aantal uitgaande IP-adressen op een bepaald moment. Alle uitgaande verbindingen van de App Service-app, bijvoorbeeld bij een back-end-database maakt gebruik van een van de uitgaande IP-adressen als het oorspronkelijke IP-adres. U weten niet vooraf welk IP-adres een bepaalde app-exemplaar gebruikt de uitgaande om verbinding te maken, zodat uw back-end-service moet de firewall om de uitgaande IP-adressen van uw app te openen.

De set met uitgaande IP-adressen voor uw app verandert wanneer u uw app tussen de lagere lagen (**Basic**, **Standard**, en **Premium**) en de  **Premium V2** laag.

U vindt de set met alle mogelijke uitgaande IP-adressen uw app gebruiken kunt, ongeacht de Prijscategorieën, door te zoeken naar de `possibleOutboundIPAddresses` eigenschap of in de **aanvullende uitgaand IP-adressen** veld in de **eigenschappen**  blade in Azure portal. Zie [uitgaande IP-adressen vinden](#find-outbound-ips).

## <a name="find-outbound-ips"></a>Uitgaande IP-adressen zoeken

Als u wilt zoeken in de uitgaande IP-adressen momenteel gebruikt door uw app in Azure portal, klikt u op **eigenschappen** in de linkernavigatiebalk van uw app. Ze worden weergegeven in de **uitgaande IP-adressen** veld.

U kunt dezelfde informatie vinden door te voeren van de volgende opdracht uit in de [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Om te zoeken _alle_ mogelijk uitgaande IP-adressen voor uw app, ongeacht de Prijscategorieën, klikt u op **eigenschappen** in de linkernavigatiebalk van uw app. Ze worden weergegeven in de **aanvullende uitgaand IP-adressen** veld.

U kunt dezelfde informatie vinden door te voeren van de volgende opdracht uit in de [Cloud Shell](../cloud-shell/quickstart.md).

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Volgende stappen

Leer hoe u binnenkomend verkeer beperken door de bron-IP-adressen.

> [!div class="nextstepaction"]
> [Statische IP-adresbeperkingen](app-service-ip-restrictions.md)
