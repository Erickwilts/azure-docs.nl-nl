---
title: Cloud App Security gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Informatie over het verbinden met Cloud App Security gegevens Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: a418bb318654752eaf48ffbdd05b80cabb487ece
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65207554"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Verbinding maken met gegevens uit Microsoft Cloud App Security 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken van streamen [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) in Azure Sentinel met één klik. Deze verbinding kunt u de waarschuwingen van Cloud App Security in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Vereisten

- Gebruiker met de globale beheerder of beheerder beveiligingsmachtigingen

## <a name="connect-to-cloud-app-security"></a>Verbinding maken met Cloud App Security

Als u Cloud App Security al hebt, zorgt ervoor dat deze [ingeschakeld op uw netwerk](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Als u Cloud App Security is geïmplementeerd en ophalen van uw gegevens, gegevens van de waarschuwing kan eenvoudig worden gestreamd naar Azure Sentinel.


1. Selecteer in Azure Sentinel, **gegevensconnectors** en klik vervolgens op de **Cloud App Security** tegel.

2. Klik op **Verbinden**.

3. Zoek voor het gebruik van de relevante schema in Log Analytics voor de Cloud App Security-waarschuwingen, **SecurityAlert**.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Microsoft Cloud App Security Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
