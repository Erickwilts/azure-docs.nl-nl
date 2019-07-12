---
title: Voorbereiden binnen een paar Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d5750b3e-bfbd-4fa0-b888-ebfab7d9c9ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2019
ms.author: rkarlin
ms.openlocfilehash: c9f2f011acb9d815202aa6c6a38ed364ffb0f9cd
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67619657"
---
# <a name="on-board-azure-sentinel-preview"></a>Voorbeeld van de trein Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In deze snelstartgids leert u hoe u aan boord Sentinel van Azure. 

Aan het ingebouwde Azure-Sentinel moet u eerst Azure Sentinel inschakelen en vervolgens verbinding maken met uw gegevensbronnen. Azure Sentinel wordt geleverd met een aantal connectors voor Microsoft-oplossingen, beschikbaar is buiten het vak en biedt realtime-integratie, inclusief Microsoft Threat Protection-oplossingen, Microsoft 365-bronnen, zoals Office 365, Azure AD, Azure ATP, en Microsoft Cloud App Security, en meer. Er zijn bovendien ingebouwde connectors voor het complete ecosysteem van de beveiliging voor niet-Microsoft-oplossingen. U kunt ook de algemene indeling voor gebeurtenissen, Syslog of REST-API om uw gegevensbronnen verbinding met Azure Sentinel te gebruiken.  

Nadat u verbinding maakt met uw gegevensbronnen, kunt u kiezen uit een galerie met vakkundig gemaakte dashboards die Maak inzichten op basis van uw gegevens duidelijk. Deze dashboards kunnen eenvoudig worden aangepast aan uw behoeften.


## <a name="global-prerequisites"></a>Algemene vereisten

- Actieve Azure-abonnement, als u nog geen hebt, maak een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- Log Analytics-werkruimte. Meer informatie over het [een Log Analytics-werkruimte maken](../log-analytics/log-analytics-quick-create-workspace.md)

-  Om in te schakelen Sentinel van Azure, moet u bijdrager machtigingen voor het abonnement waarin de Azure-Sentinel werkruimte zich bevindt. 
- Voor het gebruik van Azure Sentinel, moet u bijdrager of lezer machtigingen voor de resourcegroep die deel uitmaakt van de werkruimte op
- Aanvullende machtigingen nodig om verbinding maken met specifieke gegevensbronnen
 
## Azure Sentinel inschakelen <a name="enable"></a>

1. Ga in de Azure-portal.
2. Zorg ervoor dat het abonnement waarin Azure Sentinel is gemaakt, is geselecteerd. 
3. Zoeken naar Azure Sentinel. 
   ![search](./media/quickstart-onboard/search-product.png)

1. Klik op **+ toevoegen**.
1. Selecteer de werkruimte die u wilt gebruiken of een nieuwe maken. U kunt Azure Sentinel uitvoeren op meer dan één werkruimte, maar de gegevens is geïsoleerd voor één werkruimte.

   ![Zoeken](./media/quickstart-onboard/choose-workspace.png)

   >[!NOTE] 
   > - **De locatie van** is het belangrijk om te begrijpen dat alle gegevens die u naar Azure Sentinel streamen is opgeslagen in de geografische locatie van de werkruimte die u hebt geselecteerd.  
   > - Standaardwerkruimten die zijn gemaakt door Azure Security Center wordt niet weergegeven in de lijst. u kunt Azure Sentinel niet installeren op deze.
   > - Azure Sentinel kunt uitvoeren op werkruimten die zijn geïmplementeerd in een van de volgende regio's:  Australië-Zuidoost, Canada centraal, centraal-India, VS-Oost, VS-Oost 2 EUAP (Canarische), Japan-Oost, Zuidoost-Azië, VK Zuid, West-Europa, VS-West 2.

6. Klik op **toevoegen Azure Sentinel**.
  

## <a name="connect-data-sources"></a>Verbinding maken met gegevensbronnen

Azure Sentinel maakt de verbinding met services en -apps door te verbinden met de service en de gebeurtenissen en logboeken worden doorgestuurd naar Azure Sentinel. Voor machines en virtuele machines, kunt u de Azure-Sentinel agent die de logboeken worden verzameld en ze doorstuurt naar het Azure-Sentinel installeren. Voor Firewalls en proxy's, Azure Sentinel maakt gebruik van een Linux Syslog-server. De agent is geïnstalleerd en van de agent verzamelt het logboek voor bestanden en ze doorstuurt naar het Azure-Sentinel. 
 
1. Klik op **gegevensverzameling**.
2. Er is een tegel voor elke gegevensbron die u kunt verbinding maken.<br>
Bijvoorbeeld, klikt u op **Azure Active Directory**. Als u verbinding maakt met deze gegevensbron, moet u de logboeken van Azure AD in Azure Sentinel streamen. U kunt selecteren welk type u wan om op te halen - logboeken aanmelden logboeken en/of de logboeken voor controle. <br>
Aan de onderkant aanbevelingen Azure Sentinel voor welke dashboards die u moet installeren voor elke connector zodat u kunt dat u onmiddellijk inzichten krijgen interessante van uw gegevens. <br> Volg de installatie-instructies of [verwijzen naar de relevante verbinding guide](connect-data-sources.md) voor meer informatie. Zie voor meer informatie over gegevensconnectors [verbinding maken met Microsoft-services](connect-data-sources.md).

Nadat uw gegevensbronnen zijn verbonden, worden uw gegevens streamen naar Azure Sentinel begint en gereed is voor u aan de slag. Vindt u de logboeken in de [ingebouwde dashboards](quickstart-get-visibility.md) en beginnen met het samenstellen van query's in Log Analytics [onderzoeken van de gegevens](tutorial-investigate-cases.md).



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd over verbinding maken met gegevensbronnen Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
- Gegevens uit Stream [algemene fout indeling toestellen](connect-common-event-format.md) in Azure Sentinel.
