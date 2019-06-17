---
title: IP-adressen in Azure Functions
description: Informatie over binnenkomende en uitgaande IP-adressen vinden voor functie-apps en waarom ze aan te passen.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: glenga
ms.openlocfilehash: 83e5a15d8a7f9c01f6a180ebceb715600b8a39db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61035850"
---
# <a name="ip-addresses-in-azure-functions"></a>IP-adressen in Azure Functions

Dit artikel wordt uitgelegd in de volgende onderwerpen met betrekking tot IP-adressen van de functie-apps:

* Over het vinden van de IP-adressen die momenteel in gebruik door een functie-app.
* Wat ervoor zorgt dat een functie van app-IP-adressen worden gewijzigd.
* Klik hier voor meer informatie over het beperken van de IP-adressen die toegang hebben tot een functie-app.
* Klik hier voor meer informatie over het ophalen van toegewezen IP-adressen voor een functie-app.

IP-adressen zijn gekoppeld met de functie-apps, niet met afzonderlijke functies. Binnenkomende HTTP-aanvragen kunnen geen inkomende IP-adres gebruiken om aan te roepen van afzonderlijke functies. ze moeten de standaarddomeinnaam (functionappname.azurewebsites.net) of een aangepaste domeinnaam gebruiken.

## <a name="function-app-inbound-ip-address"></a>Functie-app inkomende IP-adres

Elke functie-app heeft één inkomende IP-adres. Om te zoeken dat IP-adres:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar de functie-app.
3. Selecteer **platformfuncties**.
4. Selecteer **eigenschappen**, en de inkomende IP-adres wordt weergegeven onder **virtueel IP-adres**.

## <a name="find-outbound-ip-addresses"></a>Functie-app uitgaande IP-adressen

Elke functie-app heeft een reeks uitgaande IP-adressen beschikbaar. Alle uitgaande verbindingen van een functie, zoals het een back-end-database maakt gebruik van een van de uitgaande IP-adressen beschikbaar als het oorspronkelijke IP-adres. U weten niet vooraf welk IP-adres een bepaalde verbinding moet worden gebruikt. Om deze reden moet de back-end-service de firewall om alle uitgaande IP-adressen van de functie-app te openen.

Zoek de uitgaande IP-adressen beschikbaar zijn voor een functie-app:

1. Aanmelden bij de [Azure Resource Explorer](https://resources.azure.com).
2. Selecteer **abonnementen > {uw abonnement} > providers > Microsoft.Web > sites**.
3. In het deelvenster JSON vindt u de site met een `id` eigenschap die het einde van de naam van uw functie-app.
4. Zie `outboundIpAddresses` en `possibleOutboundIpAddresses`. 

De set `outboundIpAddresses` is momenteel beschikbaar voor de functie-app. De set `possibleOutboundIpAddresses` bevat IP-adressen die beschikbaar zijn als de functie-app [kan worden geschaald naar andere Prijscategorieën](#outbound-ip-address-changes).

Een alternatieve manier om te zoeken van de uitgaande IP-adressen beschikbaar is via de [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```
> [!NOTE]
> Wanneer een functie-app die wordt uitgevoerd op de [verbruiksabonnement](functions-scale.md#consumption-plan) wordt geschaald, kunnen een nieuwe reeks uitgaande IP-adressen worden toegewezen. Bij het uitvoeren van het plan verbruik, wellicht u whitelisten het hele datacenter.

## <a name="data-center-outbound-ip-addresses"></a>Uitgaande IP-adressen via datacenters

Als u wilt plaatsen de uitgaande IP-adressen die worden gebruikt door uw functie-apps, een andere optie is aan lijst met geaccepteerde datacenter van de functie-apps (Azure-regio). U kunt [een JSON-bestand met een lijst met IP-adressen voor alle Azure-datacenters downloaden](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Gaat u naar het JSON-fragment dat van toepassing op de regio waarin uw functie-app wordt uitgevoerd in.

Dit is bijvoorbeeld, wat de West-Europa JSON-fragment als volgt uitzien:

```
{
  "name": "AzureCloud.westeurope",
  "id": "AzureCloud.westeurope",
  "properties": {
    "changeNumber": 9,
    "region": "westeurope",
    "platform": "Azure",
    "systemService": "",
    "addressPrefixes": [
      "13.69.0.0/17",
      "13.73.128.0/18",
      ... Some IP addresses not shown here
     "213.199.180.192/27",
     "213.199.183.0/24"
    ]
  }
}
```

 Voor informatie over wanneer dit bestand wordt bijgewerkt en wanneer de wijziging voor de IP-adressen, vouw de **Details** sectie van de [Downloadcentrum](https://www.microsoft.com/en-us/download/details.aspx?id=56519).

## <a name="inbound-ip-address-changes"></a>Inkomende IP-adres verandert

De inkomende IP-adres **mogelijk** wijzigen wanneer u:

- Een functie-app verwijderen en opnieuw maken in een andere resourcegroep.
- De laatste functie-app in een combinatie van groep en de regio resource verwijderen en opnieuw maken.
- Een SSL-binding verwijderen zoals tijdens [verlenging van het certificaat](../app-service/app-service-web-tutorial-custom-ssl.md#renew-certificates)).

Wanneer uw functie-app wordt uitgevoerd een [verbruiksabonnement](functions-scale.md#consumption-plan), het inkomende IP-adres kan ook worden gewijzigd wanneer u dit nog niet hebt maatregelen genomen, zoals die worden vermeld.

## <a name="outbound-ip-address-changes"></a>Uitgaande IP-adres verandert

De set beschikbare uitgaande IP-adressen voor een functie-app wanneer veranderen kan u:

* Enkele actie ondernemen die het inkomende IP-adres kunt wijzigen.
* Uw App Service-plan prijscategorie wijzigen. De lijst met alle mogelijke uitgaande IP-adressen uw app voor alle Prijscategorieën gebruiken kunt is in de `possibleOutboundIPAddresses` eigenschap. Zie [uitgaande IP-adressen vinden](#find-outbound-ip-addresses).

Wanneer uw functie-app wordt uitgevoerd een [verbruiksabonnement](functions-scale.md#consumption-plan), de uitgaande IP-adres kan ook worden gewijzigd wanneer u dit nog niet hebt maatregelen genomen, zoals die worden vermeld.

Om af te dwingen opzettelijk een uitgaande IP-adres is gewijzigd:

1. Uw App Service-plan omhoog of omlaag schalen tussen Standard en Premium v2-Prijscategorieën.
2. Wacht tien minuten.
3. Schaal terug naar waar u bent begonnen.

## <a name="ip-address-restrictions"></a>IP-adresbeperkingen

U kunt een lijst met IP-adressen die u wilt toestaan of weigeren van toegang tot een functie-app configureren. Zie voor meer informatie, [Azure App Service statische IP-adresbeperkingen](../app-service/app-service-ip-restrictions.md).

## <a name="dedicated-ip-addresses"></a>Toegewezen IP-adressen

Als u statisch toegewezen IP-adressen, wordt aangeraden [App Service-omgevingen](../app-service/environment/intro.md) (de [geïsoleerde laag](https://azure.microsoft.com/pricing/details/app-service/) van App Service-plannen). Zie voor meer informatie, [App Service-omgeving IP-adressen](../app-service/environment/network-info.md#ase-ip-addresses) en [over het beheren van inkomend verkeer naar een App Service Environment](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md).

Als u wilt nagaan of uw functie app wordt uitgevoerd in een App Service-omgeving:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Navigeer naar de functie-app.
3. Selecteer de **overzicht** tabblad.
4. De App Service-plan-laag wordt weergegeven onder **App Service-plan/prijscategorie**. De prijscategorie van App Service Environment is **geïsoleerd**.
 
Als alternatief kunt u de [Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

De App Service Environment `sku` is `Isolated`.

## <a name="next-steps"></a>Volgende stappen

Een veelvoorkomende oorzaak van de IP-wijzigingen is schaalwijzigingen voor functie-app. [Meer informatie over het schalen van de functie-app](functions-scale.md).
