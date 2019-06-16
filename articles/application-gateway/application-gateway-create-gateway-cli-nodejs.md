---
title: Een Azure Application Gateway - CLI van Azure classic maken
description: Informatie over het maken van een toepassingsgateway met behulp van de klassieke Azure CLI in Resource Manager
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 4/15/2019
ms.author: victorh
ms.openlocfilehash: 7107f45253c4f13b3378489726bf5034e104fa30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62095979"
---
# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Een toepassingsgateway maken met behulp van de Azure CLI

Azure Application Gateway is een load balancer in laag 7. De gateway biedt opties voor failovers en het routeren van HTTP-aanvragen tussen servers (on-premises en in de cloud). Application gateway biedt de volgende functies voor de levering van toepassing: HTTP-taakverdeling, cookies gebaseerde sessieaffiniteit en Secure Sockets Layer (SSL)-offload, aangepaste statustests en ondersteuning voor meerdere locaties.

## <a name="prerequisite-install-the-azure-cli"></a>Voorwaarde: Azure-CLI installeren

Als u wilt de stappen in dit artikel uitvoert, moet u [Azure CLI installeren](../xplat-cli-install.md) en u moet [Meld u aan Azure](/cli/azure/authenticate-azure-cli). 

> [!NOTE]
> Als u een Azure-account niet hebt, moet u een. U kunt zich [hier aanmelden voor een gratis proefversie](../active-directory/fundamentals/sign-up-organization.md).

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway met behulp van de Azure portal maken.

In dit scenario wordt:

* Een middelgrote toepassingsgateway maken met twee exemplaren.
* Maak een virtueel netwerk met de naam ContosoVNET met een gereserveerde CIDR-blok van 10.0.0.0/16.
* Maak een subnet met de naam subnet01 dat gebruikmaakt van 10.0.0.0/28 als CIDR-blok.

> [!NOTE]
> Aanvullende configuratie van de toepassingsgateway, met inbegrip van de gezondheid van aangepaste tests, adressen van de back-end-groep en extra regels worden geconfigureerd nadat de application gateway is geconfigureerd en niet tijdens de initiële implementatie.

## <a name="before-you-begin"></a>Voordat u begint

Azure Application Gateway is een eigen subnet vereist. Bij het maken van een virtueel netwerk, zorg ervoor dat u onvoldoende adresruimte voor meerdere subnetten hebt verlaten. Wanneer u een toepassingsgateway met een subnet implementeert, kunnen alleen andere Toepassingsgateways worden toegevoegd aan het subnet.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Open de **Microsoft Azure-opdrachtprompt**, en meld u aan.

```azurecli-interactive
az login
```

Wanneer u in het voorgaande voorbeeld typt, krijgt u een code. Navigeer naar https://aka.ms/devicelogin in een browser om door te gaan het teken proces.

![apparaataanmelding cmd-weergeven][1]

Voer de code die u hebt ontvangen in de browser. U bent omgeleid naar een aanmeldingspagina.

![browser-code invoeren][2]

Zodra de code is ingevoerd. u bent aangemeld, sluit de browser om door te gaan met het scenario.

![aangemeld][3]

## <a name="switch-to-resource-manager-mode"></a>Schakel over naar Resource Manager-modus

```azurecli-interactive
azure config mode arm
```

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de toepassingsgateway maakt, een resourcegroep gemaakt waartoe de toepassingsgateway. Hieronder ziet u de opdracht.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Nadat de resourcegroep is gemaakt, wordt een virtueel netwerk gemaakt voor de toepassingsgateway.  In het volgende voorbeeld is de adresruimte als 10.0.0.0/16 zoals gedefinieerd in de opmerkingen bij de voorgaande scenario.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Een subnet maken

Nadat het virtuele netwerk is gemaakt, wordt een subnet voor application gateway toegevoegd.  Als u van plan bent voor het gebruik van application gateway met een web-app die wordt gehost in hetzelfde virtuele netwerk bevinden als de toepassingsgateway, zorg er dan voor dat laat voldoende ruimte heeft voor een ander subnet.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Als het virtuele netwerk en subnet worden gemaakt, zijn de vereisten voor de toepassingsgateway zijn voltooid. Een eerder geëxporteerd pfx-certificaat en het wachtwoord voor het certificaat zijn ook vereist voor de volgende stap: De IP-adressen die worden gebruikt voor de back-end zijn de IP-adressen voor uw back-endserver. Deze waarden kunnen worden persoonlijke IP-adressen in het virtuele netwerk, de openbare IP-adressen of de volledig gekwalificeerde domeinnamen voor uw back-endservers.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> Voor een lijst met parameters die kan worden opgegeven tijdens het maken van de volgende opdracht uitvoeren: **azure network application-gateway create--help**.

In dit voorbeeld maakt een basic-toepassingsgateway met standaardinstellingen voor de listener, back-endpool, back-end-http-instellingen en regels. U kunt deze instellingen aan de behoeften van uw implementatie wanneer het inrichten voltooid is.
Als u al uw webtoepassing die is gedefinieerd met behulp van de back-endpool in de voorgaande stappen hebt eenmaal is gemaakt, begint taakverdeling.

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van aangepaste statustests recentst [een aangepaste statustest maken](application-gateway-create-probe-portal.md)

Meer informatie over het configureren van SSL-Offloading, en past de kostbare SSL-ontsleuteling uit uw webservers naar de pagina [SSL-Offload configureren](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
