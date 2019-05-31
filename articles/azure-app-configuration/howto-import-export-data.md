---
title: Gegevens met Azure App-configuratie importeren of exporteren | Microsoft Docs
description: Meer informatie over het importeren of exporteren van gegevens naar of van Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 377c5088d39821e87412c517540b3190b0a14a00
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393274"
---
# <a name="import-or-export-configuration-data"></a>Configuratiegegevens importeren of exporteren

Azure App ondersteunt configuratiegegevens importeren en exporteren van bewerkingen. Deze bewerkingen gebruiken om te werken met de configuratiegegevens in bulk en exchange-gegevens tussen uw app opslaan van de configuratie en de code project. U kunt bijvoorbeeld één app-configuratie-winkel voor testen en één voor productie instellen. U kunt vervolgens toepassingsinstellingen tussen deze via een bestand kopiëren zodat u geen gegevens tweemaal op enter.

Dit artikel bevat een handleiding voor het importeren en exporteren van gegevens met App-configuratie.

## <a name="import-data"></a>Gegevens importeren

Import worden configuratie opslaan van gegevens in een App-configuratie van een bestaande gegevensbron, in plaats van handmatig in te voeren. De importfunctie gebruiken om gegevens te migreren naar een app opslaan van de configuratie- of statistische gegevens uit meerdere bronnen. App-configuratie biedt ondersteuning voor importeren uit een bestand met JSON, YAML of eigenschappen.

Gegevens importeren met behulp van de [Azure-portal](https://portal.azure.com) of de [Azure CLI](./scripts/cli-import.md). Voer de volgende stappen uit vanuit de Azure-portal:

1. Blader naar uw app-configuratiearchief, en selecteer **Import/Export**.

2. Op de **importeren** tabblad **bron service** > **configuratiebestand**.

3. Selecteer **voor taal** > **bestandstype**.

4. Selecteer de **map** pictogram en blader naar het bestand te importeren.

    ![Bestand importeren](./media/import-file.png)

5. Selecteer een **scheidingsteken**, en (optioneel) Voer een **voorvoegsel** moet worden gebruikt voor geïmporteerde sleutelnamen.

6. Selecteer desgewenst een **Label**.

7. Selecteer **toepassen** importeren voltooid.

    ![Bestand importeren is voltooid](./media/import-file-complete.png)

## <a name="export-data"></a>Gegevens exporteren

Uitvoer schrijft configuratiegegevens zijn opgeslagen in de App-configuratie naar een andere bestemming. Gebruik de functie exporteren, bijvoorbeeld bij het opslaan van gegevens in een configuratie van apps naar een bestand dat ingesloten met de code van uw toepassing tijdens de implementatie.

Gegevens exporteren met behulp van de [Azure-portal](https://portal.azure.com) of de [Azure CLI](./scripts/cli-export.md). Voer de volgende stappen uit vanuit de Azure-portal:

1. Blader naar uw app-configuratiearchief, en selecteer **Import/Export**.

2. Op de **exporteren** tabblad **doel service** > **configuratiebestand**.

3. (Optioneel) Voer een **voorvoegsel** en selecteer een **Label** en een point-in-time voor sleutels moet worden geëxporteerd.

4. Selecteer een **bestandstype** > **scheidingsteken**.

5. Selecteer **toepassen** naar de export voltooien.

    ![Voltooide-bestand exporteren](./media/export-file-complete.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een ASP.NET Core web-app maken](./quickstart-aspnet-core-app.md)  
