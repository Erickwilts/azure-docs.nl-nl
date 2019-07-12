---
title: Gebruikspatronen van Azure CDN analyseren | Microsoft Docs
description: Dit artikel beschrijft de verschillende typen analyserapporten die beschikbaar zijn voor Azure CDN-producten.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: magattus
ms.openlocfilehash: 238dea3c136daf13d3db7be41bed103a0cbf7636
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593686"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Gebruikspatronen van Azure CDN analyseren

Nadat u CDN voor uw toepassing inschakelt, kunt u CDN-gebruik controleren, controleert u de status van uw levering en mogelijke problemen. Azure CDN biedt deze mogelijkheden in de volgende manieren: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Basisanalyse via diagnostische logboeken in Azure

Core analytics is beschikbaar voor CDN-eindpunten voor alle Prijscategorieën. Diagnostische logboeken van Azure core analytics worden geëxporteerd naar Azure storage, eventhubs, toestaan of Azure Monitor-Logboeken. Logboeken in Azure Monitor biedt een oplossing met grafieken die kunnen worden geconfigureerd met een gebruiker en kan worden aangepast. Zie voor meer informatie over diagnostische logboeken in Azure, [diagnostische logboeken in Azure](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Verizon core rapporten

Als een Azure CDN-gebruiker met een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel, kunt u Verizon core rapporten weergeven in de aanvullende portal Verizon. Verizon core rapporten zijn toegankelijk via de **beheren** optie in de Azure portal en biedt tal van grafieken en weergaven. Zie voor meer informatie, [Kernrapporten](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Aangepaste Verizon-rapporten

Als een Azure CDN-gebruiker met een **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** profiel, kunt u aangepaste Verizon-rapporten weergeven in de aanvullende portal Verizon. Aangepaste Verizon-rapporten zijn toegankelijk via de **beheren** optie in de Azure portal. De Verizon aangepaste rapporten pagina ziet u het aantal treffers of gegevens die zijn overgedragen voor elk van de rand CName die behoren tot een Azure CDN-profiel. De gegevens kunnen worden gegroepeerd op HTTP-antwoord code of cache status gedurende een bepaalde periode. Zie voor meer informatie, [aangepaste Verizon-rapporten](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Azure CDN Premium van Verizon-rapporten

Met **Azure CDN Premium van Verizon**, u kunt ook toegang tot de volgende rapporten:
   * [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
   * [Realtime statistieken](cdn-real-time-stats.md)
   * [Prestaties van edge-knooppunt](cdn-edge-performance.md)

