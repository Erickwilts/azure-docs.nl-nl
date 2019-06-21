---
title: Barracuda gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van Barracuda Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 350d2c6253a417637c7ec8f2e38919dc4b969340
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190778"
---
# <a name="connect-your-barracuda-appliance"></a>Verbinding maken met uw Barracuda-apparaat 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Barracuda Web Application Firewall (WAF)-connector kunt u eenvoudig verbinding maken met uw Barracuda-logboeken met uw Azure Sentinel, dashboards weergeven, aangepaste waarschuwingen maken en onderzoek te verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking. Azure Sentinel maakt gebruik van de ingebouwde integratie tussen **Barracuda** en Microsoft Monitoring Agent voor naadloze integratie. 


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="configure-and-connect-barracuda-waf"></a>Configureren en verbinden van Barracuda WAF
Barracuda Web Application Firewall kunt integreren en logboeken exporteren rechtstreeks naar Azure Sentinel via Microsoft Monitoring Agent.
1. Ga naar [Barracuda WAF-configuratie stroom](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/), en volg de instructies voor het instellen van de verbinding, met behulp van deze parameters:
    - **Werkruimte-ID**: Kopieer de waarde van uw werkruimte-ID van de pagina Azure Sentinel Barracuda-connector.
    - **Primaire sleutel**: Kopieer de waarde van uw primaire sleutel vanaf de pagina Azure Sentinel Barracuda-connector.
2. In de portal voor Azure Sentinel, gaat u naar de werkruimte waarop u geïmplementeerd Sentinel van Azure en selecteer het weglatingsteken (...) aan het einde van de rij en selecteer **geavanceerde instellingen**. 
1. Selecteer **gegevens** en vervolgens **Syslog**.
1. Zorg ervoor dat de opslagruimte die u in Barracuda instelt bestaat en de ernst instellen en klik op **opslaan**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Barracuda-gebeurtenissen, **CommonSecurityLog** en **barracuda_CL**.


## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Barracuda-apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

