---
title: bestand opnemen
description: bestand opnemen
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: 9b86f2e05e2cb42470061bd6398b4200607f2418
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133077"
---
1. Voor het maken van een nieuwe app-configuratiearchief, moet u zich aanmelden bij de [Azure-portal](https://aka.ms/azconfig/portal). Selecteer in de linkerbovenhoek van de pagina **+ een resource maken**. In de **Marketplace doorzoeken** Voer **Appconfiguratie** en druk op Enter.

    ![Zoeken naar App Configuration](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

2. Selecteer **Appconfiguratie** uit de zoekresultaten, en selecteer vervolgens **maken**.

3. Op de **Appconfiguratie** > **maken** pagina, voer de volgende instellingen.

    | Instelling | Voorgestelde waarde | Description |
    |---|---|---|
    | **Resourcenaam** | Wereldwijd unieke naam | Voer een unieke resourcenaam in voor de resource van het app-configuratiearchief. De naam moet een tekenreeks zijn van 1 tot 63 tekens die alleen cijfers, letters en het teken `-` mag bevatten. De naam mag niet beginnen of eindigen met de `-` teken en opeenvolgende `-` tekens zijn niet geldig.  |
    | **Abonnement** | Uw abonnement | Selecteer het Azure-abonnement dat u wilt gebruiken om App Configuration te testen. Als uw account slechts één abonnement heeft, wordt deze automatisch geselecteerd en de **abonnement** vervolgkeuzelijst wordt niet weergegeven. |
    | **Resourcegroep** | *AppConfigTestResources* | Selecteer of maak een resourcegroep voor de resource van het app-configuratiearchief. Deze groep is handig voor het ordenen van meerdere resources die u mogelijk wilt verwijderen op hetzelfde moment door de resourcegroep te verwijderen. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](/azure/azure-resource-manager/resource-group-overview) voor meer informatie. |
    | **Locatie** | *US - centraal* | Gebruik **Locatie** om de geografische locatie op te geven waar het app-configuratiearchief wordt gehost. Voor de beste prestaties, de resource te maken in dezelfde regio als andere onderdelen van uw toepassing. |

    ![Resource voor app-configuratiearchief maken](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

4. Selecteer **Maken**. De implementatie kan een paar minuten langer duren.

5. Nadat de implementatie is voltooid, selecteert u **instellingen** > **toegangssleutels**. Maak een notitie van een van beide de alleen-lezen of een primaire Lees-/ verbindingsreeks primaire sleutel. Met deze verbindingsreeks later kunt u uw toepassing configureren voor het communiceren met de app-configuratie-store die u hebt gemaakt.
