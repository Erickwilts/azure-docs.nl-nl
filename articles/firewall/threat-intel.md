---
title: Bedreigingsinformatie van Azure Firewall filteren op basis van
description: Meer informatie over Azure-Firewall threat intelligence filteren
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/11/2019
ms.author: victorh
ms.openlocfilehash: 4ef9089c94d9e806cc519c4f8243cdcb7e73953a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194028"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Azure Firewall threat intelligence gebaseerd filteren - openbare Preview

Filteren op basis van bedreigingsinformatie kan voor uw firewall worden ingeschakeld voor waarschuwingen over of het weigeren van verkeer van en naar bekende kwaadaardige IP-adressen en domeinen. De IP-adressen en domeinen zijn afkomstig uit de feed Bedreigingsinformatie van Microsoft. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) wordt gebruikt door bedreigingsinformatie van Microsoft en wordt gebruikt door meerdere services, met inbegrip van Azure Security Center.

![Firewall-bedreigingsinformatie](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> Threat intelligence filteren op basis van is momenteel in openbare preview en is voorzien van een Preview-versie service level agreement. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben.  Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als threat intelligence gebaseerde filtering is ingeschakeld, worden de gekoppelde regels worden verwerkt voordat een van de NAT-regels, netwerkregels of regels voor application. Tijdens de Preview-versie, zijn alleen de hoogste betrouwbaarheid records zijn opgenomen.

U kunt aan te melden een waarschuwing wanneer een regel wordt geactiveerd, of u kunt kiezen waarschuwing en weigeren modus.

Standaard is threat intelligence gebaseerde filtering ingeschakeld in de modus voor waarschuwingen. Deze functie uitschakelen kan of de modus wijzigen totdat de interface van de portal beschikbaar in uw regio.

![Informatie over bedreigingen op basis van filteren interface van de portal](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Logboeken

Het volgende fragment in de logboekbestanden ziet u een geactiveerde regel:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Testen

- **Uitgaande testen** -waarschuwingen voor uitgaand verkeer moet een zeldzame exemplaar, zoals betekent dit dat uw omgeving is aangetast. Om te werkt testen uitgaande waarschuwingen, test FQDN-naam is gemaakt die een waarschuwing wordt geactiveerd. Gebruik **testmaliciousdomain.eastus.cloudapp.azure.com** voor uw uitgaande tests.

- **Inkomende testen** -u kunt verwachten om waarschuwingen te zien op het binnenkomende verkeer als DNAT regels zijn geconfigureerd op de firewall. Dit geldt zelfs als alleen specifieke bronnen zijn toegestaan voor de regel DNAT en verkeer anders is geweigerd. Firewall van Azure niet wordt gewaarschuwd voor alle bekende poort scanners; alleen op scanners waarvan bekend is dat ook via een schadelijke activiteiten.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Firewall Log Analytics-voorbeelden](log-analytics-samples.md)
- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md)
- Controleer de [Microsoft Security intelligence report](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)