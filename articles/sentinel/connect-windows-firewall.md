---
title: Windows firewall gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van Windows firewall Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 9388e267c52ef53b59aacad844e964d3cfeb13d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233815"
---
# <a name="connect-windows-firewall"></a>Verbinding maken met Windows-firewall

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De Windows firewall-connector kunt u eenvoudig verbinding maken met uw Windows Firewall-logboeken, als ze zijn verbonden met uw Azure-Sentinel-werkruimte. Deze verbinding kunt u dashboards weergeven, aangepaste waarschuwingen maken en onderzoek verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking.  


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="enable-the-connector"></a>De connector inschakelen 

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en klik vervolgens op de **Windows firewall** tegel. 
1. Selecteer welke gegevenstypen die u wilt streamen.
1. Klik op **Install**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor Windows firewall, **SecurityEvent**.

## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Windows firewall Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

