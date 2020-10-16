---
title: Micro soft Defender voor eindpunt gegevens verbinden met Azure Sentinel | Microsoft Docs
description: Informatie over het verbinden van gegevens van micro soft Defender voor eind punten (voorheen micro soft Defender ATP) aan Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: d1de9270534a3550d8db043fdd6bcfe56ec31bda
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "90895599"
---
# <a name="connect-alerts-from-microsoft-defender-for-endpoint-formerly-microsoft-defender-atp"></a>Verbinding maken met waarschuwingen van micro soft Defender voor eind punt (voorheen micro soft Defender ATP)

> [!IMPORTANT]
>
> - **Micro soft Defender voor eind punt** was voorheen bekend als **micro soft Defender Advanced Threat Protection** of **MDATP**.
>
>     Mogelijk ziet u de oude naam die nog steeds wordt gebruikt in het product (inclusief de gegevens connector in azure Sentinel) gedurende een bepaalde periode.
>
> - Opname van micro soft Defender voor eindpunt waarschuwingen is momenteel beschikbaar als open bare preview.
> Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Met de [micro soft Defender for Endpoint](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) -connector kunt u waarschuwingen van micro soft Defender naar het eind punt streamen naar Azure Sentinel. Op die manier kunt u beveiligings gebeurtenissen in uw organisatie uitgebreid analyseren en playbooks bouwen voor een effectief en direct antwoord.

> [!NOTE]
>
> Als u de nieuwe onbewerkte gegevens logboeken wilt opnemen in de [geavanceerde jacht](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview)van micro soft Defender voor het eind punt, gebruikt u de nieuwe connector voor Microsoft 365 Defender (voorheen micro soft Threat Protection, [raadpleegt](./connect-microsoft-365-defender.md)u de documentatie).

## <a name="prerequisites"></a>Vereisten

- U moet een geldige licentie voor micro soft Defender voor eind punt hebben, zoals beschreven in [micro soft Defender voor endpoint-implementatie instellen](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing). 

- U moet een globale beheerder of een beveiligings beheerder zijn in de Azure Sentinel-Tenant.

## <a name="connect-to-microsoft-defender-for-endpoint"></a>Verbinding maken met micro soft Defender voor eind punt

Als micro soft Defender voor het eind punt is geïmplementeerd en uw gegevens worden opgenomen, kunnen de waarschuwingen eenvoudig worden gestreamd naar Azure Sentinel.

1. In azure Sentinel selecteert u **gegevens connectors**, selecteert u **micro soft Defender voor eind punt** (kan nog steeds *micro soft Defender Advanced Threat Protection*worden genoemd) in de galerie en selecteert u de **pagina connector openen**.

1. Klik op **Verbinding maken**. 

1. Als u een query wilt uitvoeren voor micro soft Defender for Endpoint-waarschuwingen in **Logboeken**, voert u **SecurityAlert** in het query venster in en voegt u een filter toe waarbij de **provider naam** **MDATP**is.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u micro soft Defender voor eind punt verbindt met Azure Sentinel. Zie de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over het [verkrijgen van inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag met [het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
