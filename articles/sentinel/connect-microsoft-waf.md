---
title: Verbinding maken met Microsoft web application firewall-gegevens naar Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met Microsoft web application firewall gegevens Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 0a308394c3cfa77f80db1361b5a49a485ee5ca0e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611355"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Verbinding maken met gegevens van Microsoft web application firewall

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken van de Azure-toepassingsgateway Microsoft web application firewall (WAF) streamen. Deze WAF uw toepassingen beschermt tegen veelvoorkomende beveiligingsproblemen op het web zoals SQL-injectie en cross-site scripting, en kunt u het pas regels aan om fout-positieven. Volg deze instructies voor het streamen van uw Microsoft Web application firewall-Logboeken in Azure Sentinel.


## <a name="prerequisites"></a>Vereisten

- Een bestaande resource van application gateway

## <a name="connect-to-microsoft-web-application-firewall"></a>Verbinding maken met Microsoft web application firewall

Als u Microsoft WAF al hebt, zorg ervoor dat u hebt een bestaande gateway-resource.
Wanneer Microsoft web application firewall is geïmplementeerd en ophalen van gegevens, gegevens van de waarschuwing kan eenvoudig worden gestreamd naar Azure Sentinel.
    
1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors**.
1. Selecteer op de pagina Data connectors de **WAF** tegel.
1. Ga naar [resource Application Gateway](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) en kies uw WAF.
    1. Selecteer **diagnostische instellingen**.
    1. Selecteer **+ diagnostische instelling toevoegen** onder de tabel.
    1. In de **diagnostische instellingen** pagina, typt u een **naam** en selecteer **verzenden naar Log Analytics**.
    1. Onder **Log Analytics-werkruimte** selecteert u de werkruimte Sentinel van Azure.
    1. Selecteer de typen logboeken die u wilt analyseren. Het wordt aangeraden: ApplicationGatewayAccessLog en ApplicationGatewayFirewallLog.
1. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Microsoft web application firewall waarschuwingen, **AzureDiagnostics**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Microsoft web application firewall Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
