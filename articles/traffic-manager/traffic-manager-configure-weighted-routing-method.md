---
title: Routeringsmethode voor gewogen round robin-verkeer met behulp van Azure Traffic Manager configureren | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u verkeer verdelen met behulp van een round robin-methode in Traffic Manager
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: allensu
ms.openlocfilehash: 4ca43bf958606a71911bf5d35f31e4fe0b342601
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071287"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>De gewogen verkeersrouteringsmethode configureren in Traffic Manager

Verkeer routeren methode gebruikelijk is het bieden van een set identieke eindpunten, waaronder cloudservices en websites, en verkeer gelijkmatig naar elke verzenden. De volgende stappen beschrijven het configureren van dit type verkeersrouteringsmethode.

> [!NOTE]
> Azure-Web-App biedt al round robin-taakverdeling functionaliteit voor websites in een Azure-regio (die kunnen bestaan uit meerdere datacenters). Traffic Manager kunt u verkeer verdelen over websites in verschillende datacenters.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Het configureren van de gewogen verkeersrouteringsmethode

1. Meld u vanuit een browser aan bij [Azure Portal](https://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. Zoek in de zoekbalk van de portal, de **Traffic Manager-profielen** en klik vervolgens op de naam van het profiel dat u wilt de routeringsmethode voor configureren.
3. In de **Traffic Manager-profiel** blade controleren of de cloudservices en de websites die u wilt opnemen in uw configuratie weergegeven worden.
4. In de **instellingen** sectie, klikt u op **configuratie**, en klik in de **configuratie** blade voltooid zijn als volgt te werk:
    1. Voor **traffic routing-methode-instellingen**, Controleer of de verkeersrouteringsmethode **gewogen**. Als dit niet het geval is, klikt u op **gewogen** in de vervolgkeuzelijst.
    2. Stel de **monitor eindpuntinstellingen** identiek voor alle elk eindpunt binnen dit profiel als volgt te werk:
        1. Selecteer de juiste **Protocol**, en geef de **poort** getal. 
        2. Voor **pad** typt u een slash */* . Voor het controleren van eindpunten, moet u een pad en bestandsnaam opgeven. Een schuine streep naar voren '/' is een geldige vermelding voor het relatieve pad en geeft aan dat het bestand is in de hoofdmap (standaard).
        3. Aan de bovenkant van de pagina, klikt u op **opslaan**.
5. De wijzigingen in uw configuratie als volgt testen:
    1.  In de zoekbalk van de portal, zoek de naam van het Traffic Manager-profiel en klik op het Traffic Manager-profiel in de resultaten die de weergegeven.
    2.  In de **Traffic Manager** blade profiel, klikt u op **overzicht**.
    3.  De **Traffic Manager-profiel** blade wordt weergegeven voor de DNS-naam van uw zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het via een webbrowser) ophalen gerouteerd naar het juiste eindpunt als bepaald door het routeringstype. In dit geval alle aanvragen worden doorgestuurd elk eindpunt in een round robin besturingsaanvraag.
6. Zodra uw Traffic Manager-profiel werkt, bewerkt u de DNS-record op de gezaghebbende DNS-server de naam van uw bedrijf domein verwijzen naar de naam van het Traffic Manager-domein.

![Gewogen verkeersrouteringsmethode met Traffic Manager configureren][1]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [prioriteit routeringsmethode voor verkeer](traffic-manager-configure-priority-routing-method.md).
- Meer informatie over [prestaties routeringsmethode voor verkeer](traffic-manager-configure-performance-routing-method.md).
- Meer informatie over [geografische verkeersrouteringsmethode](traffic-manager-configure-geographic-routing-method.md).
- Meer informatie over het [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
