---
title: Syslog-gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Informatie over het verbinden met Azure Sentinel Syslog-gegevens.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 673b1df6094703bebcbfd9d82c1268c01d46e814
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233578"
---
# <a name="connect-your-external-solution-using-syslog"></a>Verbinding maken met de externe oplossing met behulp van Syslog

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt een on-premises apparaat die ondersteuning biedt voor Syslog naar Azure Sentinel verbinden. Dit wordt gedaan met behulp van een agent die is gebaseerd op een Linux-machine tussen het apparaat en Azure Sentinel. Als uw Linux-machine in Azure, kunt u de logboeken van uw apparaat of toepassing naar een toegewezen werkruimte voor u maken in Azure en verbindt dit streamen. Als uw Linux-machine zich niet in Azure, kunt u de logboeken van uw apparaat streamen naar een toegewezen on-premises virtuele machine of computer waarop u de Agent voor Linux installeren. 

> [!NOTE]
> Als uw apparaat Syslog CEF ondersteunt, de verbinding volledig is en u moet deze optie kiest en volg de instructies in [verbinding te maken van gegevens van CEF](connect-common-event-format.md).

## <a name="how-it-works"></a>Hoe werkt het?

Syslog-verbinding wordt gerealiseerd met behulp van een agent voor Linux. Standaard ontvangt de agent voor Linux gebeurtenissen van de Syslog-daemon via UDP, maar in gevallen waarin een Linux-machine worden verwacht voor het verzamelen van een groot aantal Syslog-gebeurtenissen, zoals wanneer een Linux-agent er gebeurtenissen van andere apparaten ontvangen is, de configuratie is gewijzigd in gebruik van TCP-transport tussen de Syslog-daemon en de agent.

## <a name="connect-your-syslog-appliance"></a>Verbinding maken met uw Syslog-apparaat

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en kies de **Syslog** tegel.
2. Als uw Linux-machine niet binnen Azure valt, downloadt en installeert de Azure-Sentinel **-Agent voor Linux** op uw apparaat. 
1. Als u in Azure werkt, selecteert of maakt een virtuele machine die in de werkruimte van Azure Sentinel die toegewezen is aan het ontvangen van Syslog-berichten. Selecteer de virtuele machine in Azure Sentinel werkruimten en klikt u op **Connect** boven aan het linkerdeelvenster.
3. Klik op **configureren van de logboeken worden verbonden** terug in de Syslog-connector-instellingen. 
4. Klik op **Druk hier om de configuratieblade te openen**.
1. Selecteer **gegevens** en vervolgens **Syslog**.
   - Zorg ervoor dat elke faciliteit die u verzendt door Syslog is in de tabel. Voor elke functie wilt u controleren, stelt u een prioriteit. Klik op **Toepassen**.
1. Zorg dat u verzendt die faciliteiten in uw Syslog-machine. 

3. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Syslog-Logboeken, **Syslog**.




## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Syslog on-premises apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
