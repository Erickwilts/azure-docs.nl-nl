---
title: Firewall regels voor Azure Event Hubs | Microsoft Docs
description: Gebruik firewall regels om verbindingen van specifieke IP-adressen toe te staan aan Azure Event Hubs.
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: e07f863bf8b7d5f64ec0ba04bf16fba12f4a785d
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2020
ms.locfileid: "94427442"
---
# <a name="allow-access-to-azure-event-hubs-namespaces-from-specific-ip-addresses-or-ranges"></a>Toegang tot Azure Event Hubs-naam ruimten van bepaalde IP-adressen of bereiken toestaan
Event Hubs naam ruimten zijn standaard toegankelijk vanuit Internet zolang de aanvraag een geldige verificatie en autorisatie heeft. Met IP-firewall kunt u dit nog verder beperken tot een aantal IPv4-adressen of IPv4-adresbereiken in CIDR-notatie [(klasseloze Inter-Domain route ring)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) .

Deze functie is handig in scenario's waarin Azure Event Hubs alleen toegankelijk moet zijn vanaf bepaalde bekende sites. Met firewall regels kunt u regels configureren voor het accepteren van verkeer dat afkomstig is van specifieke IPv4-adressen. Als u bijvoorbeeld Event Hubs met [Azure Express route][express-route]gebruikt, kunt u een **firewall regel** maken om alleen verkeer toe te staan van uw on-premises IP-adressen van de infra structuur. 

>[!WARNING]
> Door firewall regels in te scha kelen voor uw Event Hubs-naam ruimte worden binnenkomende aanvragen standaard geblokkeerd, tenzij aanvragen afkomstig zijn van een service die vanuit de toegestane open bare IP-adressen wordt uitgevoerd. Aanvragen die zijn geblokkeerd, zijn onder andere die van andere Azure-Services, van de Azure Portal, van de services logboek registratie en metrische gegevens, enzovoort. Als uitzonde ring kunt u toegang verlenen tot Event Hubs resources van bepaalde vertrouwde services, zelfs wanneer de IP-filtering is ingeschakeld. Zie voor een lijst met vertrouwde services [vertrouwde micro soft-Services](#trusted-microsoft-services).

## <a name="ip-firewall-rules"></a>IP-firewallregels
De IP-firewall regels worden toegepast op het niveau van de Event Hubs naam ruimte. De regels zijn dus van toepassing op alle verbindingen van clients die gebruikmaken van elk ondersteund protocol. Een verbindings poging van een IP-adres dat niet overeenkomt met een toegestane IP-regel op de Event Hubs naam ruimte, wordt geweigerd als niet-geautoriseerd. In het antwoord wordt de IP-regel niet vermeld. IP-filter regels worden in volg orde toegepast en de eerste regel die overeenkomt met het IP-adres, bepaalt de accepteren of afwijzen.

## <a name="use-azure-portal"></a>Azure Portal gebruiken
In deze sectie wordt beschreven hoe u de Azure Portal gebruikt om IP-firewall regels voor een Event Hubs naam ruimte te maken. 

1. Navigeer naar uw **Event hubs-naam ruimte** in de [Azure Portal](https://portal.azure.com).
4. Selecteer **netwerken** onder **instellingen** in het menu links. U ziet het tabblad **netwerken** alleen voor **standaard** of **toegewezen** naam ruimten. 
    > [!NOTE]
    > Standaard is de optie **geselecteerde netwerken** geselecteerd, zoals wordt weer gegeven in de volgende afbeelding. Als u geen IP-firewall regel opgeeft of een virtueel netwerk toevoegt op deze pagina, kunt u toegang krijgen tot de naam ruimte via het **open bare Internet** (met behulp van de toegangs sleutel).  

    :::image type="content" source="./media/event-hubs-firewall/selected-networks.png" alt-text="Tabblad netwerken-opties voor geselecteerde netwerken" lightbox="./media/event-hubs-firewall/selected-networks.png":::    

    Als u de optie **alle netwerken** selecteert, accepteert de Event hub verbindingen van elk IP-adres (met behulp van de toegangs sleutel). Deze instelling komt overeen met een regel die het IP-adres bereik 0.0.0.0/0 accepteert. 

    ![Scherm opname waarin de pagina Firewall en virtuele netwerken wordt weer gegeven met de optie alle netwerken geselecteerd.](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Als u de toegang tot specifieke IP-adressen wilt beperken, controleert u of de optie **geselecteerde netwerken** is geselecteerd. Voer de volgende stappen uit in de sectie **firewall** :
    1. Selecteer **de optie uw IP-adres voor client toevoegen** om uw huidige client-IP de toegang tot de naam ruimte te geven. 
    2. Voer bij **adres bereik** een specifiek IPv4-adres of een bereik van IPv4-adres in CIDR-notatie in. 
3. Geef op of u wilt **toestaan dat vertrouwde micro soft-services deze firewall overs Laan**. Zie [vertrouwde micro soft-Services](#trusted-microsoft-services) voor meer informatie. 

      ![Optie Firewall: alle netwerken geselecteerd](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Selecteer **Opslaan** op de werk balk om de instellingen op te slaan. Wacht een paar minuten totdat de bevestiging op de portal meldingen wordt weer gegeven.

    > [!NOTE]
    > Zie toegang tot specifieke [netwerken toestaan](event-hubs-service-endpoints.md)om de toegang tot specifieke virtuele netwerken te beperken.

[!INCLUDE [event-hubs-trusted-services](../../includes/event-hubs-trusted-services.md)]


## <a name="use-resource-manager-template"></a>Resource Manager-sjabloon gebruiken

> [!IMPORTANT]
> Firewall regels worden ondersteund in de **standaard** -en **toegewezen** lagen van Event hubs. Deze worden niet ondersteund in de Basic-laag.

Met de volgende Resource Manager-sjabloon kunt u een IP-filter regel toevoegen aan een bestaande Event Hubs naam ruimte.

Sjabloon parameters:

- **ipMask** is een enkel IPv4-adres of een blok met IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR-notatie 70.37.104.0/24 staat voor de IPv4-adressen 256 van 70.37.104.0 naar 70.37.104.255, met 24 waarmee het aantal belang rijke voorvoegsel bits voor het bereik wordt aangegeven.

> [!NOTE]
> Hoewel er geen regels kunnen worden geweigerd, is voor de Azure Resource Manager sjabloon de standaard actie ingesteld op **' toestaan '** , waardoor verbindingen niet worden beperkt.
> Wanneer u Virtual Network of firewall regels maakt, moeten we de **_' defaultAction '_ wijzigen**
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> in op
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkrulesets",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Als u de sjabloon wilt implementeren, volgt u de instructies voor [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppeling voor meer informatie over het beperken van toegang tot Event Hubs voor virtuele netwerken van Azure:

- [Service-eind punten Virtual Network voor Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  ../expressroute/expressroute-faqs.md#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
