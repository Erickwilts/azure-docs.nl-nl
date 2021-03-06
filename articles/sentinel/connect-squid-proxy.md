---
title: Inktvis-proxy gegevens verbinden met Azure Sentinel | Microsoft Docs
description: Meer informatie over het gebruik van de pijl inktvis proxy-gegevens connector voor het ophalen van inktvis-proxy Logboeken in azure Sentinel. Bekijk de inktvis-proxy gegevens in werkmappen, maak waarschuwingen en verbeter het onderzoek.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/17/2021
ms.author: yelevin
ms.openlocfilehash: b183abf8d42e6f4b1c43db2d87b2650721e0c2a9
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567900"
---
# <a name="connect-your-squid-proxy-to-azure-sentinel"></a>Uw inktvis-proxy verbinden met Azure Sentinel

> [!IMPORTANT]
> De pijl inktvis proxy connector is momenteel beschikbaar als **Preview-versie**. Zie de [aanvullende gebruiks voorwaarden voor Microsoft Azure previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor aanvullende juridische voor waarden die van toepassing zijn op Azure-functies die in bèta, preview of op andere wijze nog niet beschikbaar zijn in algemene Beschik baarheid.

In dit artikel wordt uitgelegd hoe u uw inktvis-proxy apparaat verbindt met Azure Sentinel. Met de pijl inktvis proxy data connector kunt u eenvoudig uw inktvis-logboeken verbinden met Azure Sentinel, zodat u de gegevens in werkmappen kunt bekijken, gebruiken om aangepaste waarschuwingen te maken en deze op te nemen om het onderzoek te verbeteren. Integratie tussen pijl inktvis-proxy en Azure Sentinel maakt gebruik van syslog.

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="prerequisites"></a>Vereisten

- U moet lees-en schrijf machtigingen hebben voor de Azure Sentinel-werk ruimte.

## <a name="forward-squid-proxy-logs-to-the-syslog-agent"></a>Pijl inktvis-proxy logboeken naar de syslog-agent sturen  

Configureer de proxy inktvis voor het door sturen van syslog-berichten naar uw Azure-werk ruimte via de syslog-agent.

1. Selecteer in het navigatie menu van de Azure-Sentinel de optie **Data connectors**.

1. Selecteer in de galerie **Data connectors** de **pijl inktvis-proxy (preview-versie)** en open vervolgens de **pagina connector**.

1. Volg de instructies op de pagina **inktvis proxy** connector:

    1. De agent voor Linux installeren en vrijgeven

        - Kies een Azure Linux-VM of een niet-Azure Linux-machine (fysiek of virtueel).

    1. De logboeken configureren die moeten worden verzameld

        - In de geavanceerde instellingen van de werk ruimte voegt u een aangepast logboek type toe, uploadt u een voorbeeld bestand en configureert u als gericht.

## <a name="find-your-data"></a>Uw gegevens zoeken

Nadat de verbinding tot stand is gebracht, worden de gegevens weer gegeven in **Logboeken**, onder **aangepaste logboeken**, in de `SquidProxy_CL` tabel.

Zie het tabblad **volgende stappen** op de pagina connector voor een aantal nuttige voorbeeld query's.

## <a name="validate-connectivity"></a>Connectiviteit valideren

Het kan Maxi maal 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven. 

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u verbinding kunt maken met de inktvis-proxy met Azure Sentinel. Zie de volgende artikelen voor meer informatie over Azure Sentinel:

- Meer informatie over het [verkrijgen van inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag met [het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.
