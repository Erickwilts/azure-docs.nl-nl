---
title: Service-eind punten Virtual Network-Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat informatie over het toevoegen van een service-eind punt van micro soft. EventHub aan een virtueel netwerk.
ms.topic: article
ms.date: 07/29/2020
ms.openlocfilehash: cb0d9a9c4d5e2503e68620ec4e6386d8e05d471c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88185061"
---
# <a name="allow-access-to-azure-event-hubs-namespaces-from-specific-virtual-networks"></a>Toegang tot Azure Event Hubs-naam ruimten van specifieke virtuele netwerken toestaan 

Dankzij de integratie van Event Hubs met de [service-eind punten van Virtual Network (VNet)][vnet-sep] is beveiligde toegang mogelijk tot berichten mogelijkheden van werk belastingen, zoals virtuele machines die zijn gebonden aan virtuele netwerken, waarbij het netwerkpad van het netwerk verkeer aan beide uiteinden wordt beveiligd.

Eenmaal geconfigureerd om te zijn gebonden aan ten minste één subnet-service-eind punt van een virtueel netwerk, accepteert de respectieve Event Hubs naam ruimte geen verkeer meer vanaf een wille keurige locatie, maar gemachtigd subnetten in virtuele netwerken. Vanuit het perspectief van het virtuele netwerk moet u een Event Hubs naam ruimte binden aan een service-eind punt een geïsoleerde netwerk tunnel van het subnet van het virtuele netwerk naar de berichten service configureren. 

Het resultaat is een privé-en geïsoleerde relatie tussen de werk belastingen die zijn gebonden aan het subnet en de betreffende Event Hubs naam ruimte, ondanks het waarneem bare netwerk adres van het berichten service-eind punt in een openbaar IP-bereik. Er is een uitzonde ring op dit gedrag. Als u een service-eind punt inschakelt, wordt de `denyall` regel in de [IP-firewall](event-hubs-ip-filtering.md) die aan het virtuele netwerk is gekoppeld, standaard ingeschakeld. U kunt specifieke IP-adressen toevoegen aan de IP-firewall om toegang tot het open bare eind punt van Event hub mogelijk te maken. 

>[!IMPORTANT]
> Virtuele netwerken worden ondersteund in de lagen **Standard** en **Dedicated** van Event Hubs. Het wordt niet ondersteund in de laag **basis** .
>
> Door firewall regels in te scha kelen voor uw Event Hubs-naam ruimte worden binnenkomende aanvragen standaard geblokkeerd, tenzij aanvragen afkomstig zijn van een service die vanuit toegestane virtuele netwerken wordt uitgevoerd. Aanvragen die zijn geblokkeerd, zijn onder andere die van andere Azure-Services, van de Azure Portal, van de services logboek registratie en metrische gegevens, enzovoort. 
>
> Hier volgen enkele van de services die geen toegang hebben tot Event Hubs bronnen wanneer virtuele netwerken zijn ingeschakeld. Houd er rekening mee dat de lijst **niet** limitatief is.
>
> - Azure Stream Analytics
> - Azure-IoT Hub routes
> - Azure IoT-Device Explorer
> - Azure Event Grid
> - Azure Monitor (Diagnostische instellingen)
>
> Als uitzonde ring kunt u toegang tot Event Hubs resources van bepaalde vertrouwde services toestaan, zelfs wanneer virtuele netwerken zijn ingeschakeld. Zie [Trusted Services](#trusted-microsoft-services)(Engelstalig) voor een lijst met vertrouwde services.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Geavanceerde beveiligings scenario's ingeschakeld door VNet-integratie 

Oplossingen die een strakke en compartmentalized beveiliging vereisen en waarbij virtuele netwerk-subnetten de segmentatie tussen de compartmentalized-services bieden, is nog steeds communicatie paden nodig tussen services die zich in deze compartimenten bevinden.

Elke onmiddellijke IP-route tussen de compartimenten, waaronder die van HTTPS via TCP/IP, vormt het risico van misbruik van beveiligings problemen vanuit de netwerklaag. Berichten services bieden geïsoleerde communicatie paden, waar berichten zelfs naar de schijf worden geschreven wanneer ze tussen partijen worden overgezet. Werk belastingen in twee verschillende virtuele netwerken die beide zijn gebonden aan hetzelfde Event Hubs-exemplaar kunnen efficiënt en betrouwbaar communiceren via berichten, terwijl de respectieve integriteit van de isolatie grens van het netwerk behouden blijft.
 
Dit betekent dat uw beveiligings gevoelige cloud oplossingen niet alleen toegang krijgen tot de toonaangevende betrouw bare en schaal bare asynchrone berichten mogelijkheden van Azure, maar ze kunnen nu gebruikmaken van berichten om communicatie paden te maken tussen veilige oplossingen compartimenten die inherent veiliger zijn dan wat kan worden behaald met een peer-to-peer communicatie modus, inclusief HTTPS en andere met TLS beveiligde socket protocollen.

## <a name="bind-event-hubs-to-virtual-networks"></a>Event hubs binden aan virtuele netwerken

**Regels voor virtuele netwerken** zijn de firewall beveiligings functie waarmee wordt bepaald of de naam ruimte van uw Azure-Event hubs verbindingen accepteert van een bepaald subnet van een virtueel netwerk.

Het binden van een Event Hubs naam ruimte aan een virtueel netwerk is een proces dat uit twee stappen bestaat. U moet eerst een service- **eind punt voor een virtueel netwerk** maken in het subnet van een virtueel netwerk en deze inschakelen voor **micro soft. EventHub** , zoals wordt uitgelegd in het artikel [overzicht van service-eind punten][vnet-sep] . Wanneer u het service-eind punt hebt toegevoegd, bindt u de naam ruimte van de Event Hubs met een regel voor het **virtuele netwerk**.

De regel van het virtuele netwerk is een koppeling van de Event Hubs naam ruimte met een subnet van een virtueel netwerk. Terwijl de regel bestaat, krijgen alle werk belastingen die aan het subnet zijn gebonden toegang tot de Event Hubs naam ruimte. Event Hubs zichzelf geen uitgaande verbindingen tot stand brengt, hoeft geen toegang te krijgen en wordt daarom nooit toegang tot uw subnet verleend door deze regel in te scha kelen.

## <a name="use-azure-portal"></a>Azure Portal gebruiken
In deze sectie wordt beschreven hoe u Azure Portal kunt gebruiken om een service-eind punt voor een virtueel netwerk toe te voegen. Als u de toegang wilt beperken, moet u het eind punt van de virtuele netwerk service voor deze Event Hubs naam ruimte integreren.

1. Navigeer naar uw **Event hubs-naam ruimte** in de [Azure Portal](https://portal.azure.com).
4. Selecteer **netwerken** onder **instellingen** in het menu links. U ziet het tabblad **netwerken** alleen voor **standaard** of **toegewezen** naam ruimten. 

    > [!NOTE]
    > Standaard is de optie **geselecteerde netwerken** geselecteerd, zoals wordt weer gegeven in de volgende afbeelding. Als u geen IP-firewall regel opgeeft of een virtueel netwerk toevoegt op deze pagina, kunt u toegang krijgen tot de naam ruimte via het **open bare Internet** (met behulp van de toegangs sleutel). 

    :::image type="content" source="./media/event-hubs-firewall/selected-networks.png" alt-text="Tabblad netwerken-opties voor geselecteerde netwerken" lightbox="./media/event-hubs-firewall/selected-networks.png":::    

    Als u de optie **alle netwerken** selecteert, accepteert de Event hub verbindingen van elk IP-adres (met behulp van de toegangs sleutel). Deze instelling komt overeen met een regel die het IP-adres bereik 0.0.0.0/0 accepteert. 

    ![Optie Firewall: alle netwerken geselecteerd](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Als u de toegang tot specifieke netwerken wilt beperken, selecteert u de optie **geselecteerde netwerken** boven aan de pagina als deze nog niet is geselecteerd.
2. Selecteer in de sectie **Virtual Network** van de pagina * * + bestaand virtueel netwerk toevoegen * * *. Selecteer **+ nieuw virtueel netwerk maken** als u een nieuw VNet wilt maken. 

    ![bestaand virtueel netwerk toevoegen](./media/event-hubs-tutorial-vnet-and-firewalls/add-vnet-menu.png)
3. Selecteer het virtuele netwerk in de lijst met virtuele netwerken en kies vervolgens het **subnet**. U moet het service-eind punt inschakelen voordat u het virtuele netwerk aan de lijst toevoegt. Als het service-eind punt niet is ingeschakeld, wordt u door de portal gevraagd om dit in te scha kelen.
   
   ![subnet selecteren](./media/event-hubs-tutorial-vnet-and-firewalls/select-subnet.png)

4. U ziet het volgende geslaagde bericht nadat het service-eind punt voor het subnet is ingeschakeld voor **micro soft. EventHub**. Selecteer **toevoegen** onder aan de pagina om het netwerk toe te voegen. 

    ![subnet selecteren en eindpunt inschakelen](./media/event-hubs-tutorial-vnet-and-firewalls/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Als u het service-eind punt niet kunt inschakelen, kunt u het ontbrekende service-eind punt van het virtuele netwerk negeren met behulp van de Resource Manager-sjabloon. Deze functionaliteit is niet beschikbaar in de portal.
5. Geef op of u wilt **toestaan dat vertrouwde micro soft-services deze firewall overs Laan**. Zie [vertrouwde micro soft-Services](#trusted-microsoft-services) voor meer informatie. 
6. Selecteer **Opslaan** op de werk balk om de instellingen op te slaan. Wacht een paar minuten totdat de bevestiging op de portal meldingen wordt weer gegeven.

    ![Netwerk opslaan](./media/event-hubs-tutorial-vnet-and-firewalls/save-vnet.png)

    > [!NOTE]
    > Zie toegang tot specifieke [IP-adressen of bereiken toestaan](event-hubs-ip-filtering.md)om de toegang tot specifieke IP-adressen of bereiken te beperken.

[!INCLUDE [event-hubs-trusted-services](../../includes/event-hubs-trusted-services.md)]

## <a name="use-resource-manager-template"></a>Resource Manager-sjabloon gebruiken

Met de volgende Resource Manager-sjabloon kan een regel voor een virtueel netwerk worden toegevoegd aan een bestaande Event Hubs naam ruimte.

Sjabloon parameters:

* `namespaceName`: Event Hubs naam ruimte.
* `vnetRuleName`: Naam voor de Virtual Network regel die moet worden gemaakt.
* `virtualNetworkingSubnetId`: Volledig gekwalificeerd pad van Resource Manager voor het subnet van het virtuele netwerk; bijvoorbeeld `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` voor het standaard subnet van een virtueel netwerk.

> [!NOTE]
> Hoewel er geen regels kunnen worden geweigerd, is voor de Azure Resource Manager sjabloon de standaard actie ingesteld op **' toestaan '** , waardoor verbindingen niet worden beperkt.
> Wanneer u Virtual Network of firewall regels maakt, moeten we de ***' defaultAction '*** wijzigen
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
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
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
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.EventHub"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
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

Zie de volgende koppelingen voor meer informatie over virtuele netwerken:

- [Service-eindpunten voor een virtueel Azure-netwerk][vnet-sep]
- [IP-filtering van Azure Event Hubs][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: event-hubs-ip-filtering.md
