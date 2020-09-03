---
title: 'Zelfstudie: Azure App Configuration gebruiken om functievlaggen te beheren'
titleSuffix: Azure App Configuration
description: In deze zelfstudie leert u hoe u functievlaggen onafhankelijk van uw toepassing kunt beheren met behulp van Azure App Configuration.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: lcozzens
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: dd816ebcf2a40e6a0b7febcc3fe5c1006dac20bb
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2020
ms.locfileid: "88209938"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Zelfstudie: Functievlaggen beheren in Azure App Configuration

U kunt alle functievlaggen opslaan in Azure App Configuration en deze vanuit één locatie beheren. App Configuration heeft een portal-gebruikersinterface met de naam **Functiebeheer**. Deze is specifiek ontworpen voor functievlaggen. App Configuration biedt ook systeemeigen ondersteuning voor het gegevensschema van .NET Core-functievlaggen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Functievlaggen definiëren en beheren in Azure App Configuration.
> * Functievlaggen openen vanuit uw toepassing.

## <a name="create-feature-flags"></a>Functievlaggen maken

Functiebeheer in Azure Portal voor App Configuration biedt een gebruikersinterface voor het maken en beheren van de functievlaggen die u in uw toepassingen gebruikt.

Nieuwe functievlag toevoegen:

1. Selecteer **Functiebeheer** >  **+ Toevoegen** om een functievlag toe te voegen.

    ![Lijst met functievlaggen](./media/azure-app-configuration-feature-flags.png)

1. Voer een unieke sleutelnaam voor de functie vlag in. U hebt deze naam nodig om naar de vlag in de code te verwijzen.

1. U kunt de functie desgewenst een beschrijving geven.

1. Stel de beginstatus van de functievlag in. Deze status is doorgaans *Uit* of *Aan*. De status *Aan* wordt gewijzigd in *Voorwaardelijk* als u een filter aan de functievlag toevoegt.

    ![Functievlaggen maken](./media/azure-app-configuration-feature-flag-create.png)

1. Wanneer de status *Aan* is, selecteert u **+Filter toevoegen** om aanvullende voorwaarden op te geven om de status nader aan te duiden. Voer een ingebouwde of aangepaste filtersleutel in en selecteer **+Parameter toevoegen** om een of meer parameters aan het filter te koppelen. Ingebouwde filters omvatten:

    | Sleutel | JSON-parameters |
    |---|---|
    | Microsoft.Percentage | {"Value": 0-100 procent} |
    | Microsoft.TimeWindow | {"Start": UTC-tijd, "End": UTC-tijd} |

    ![Filter voor functievlag](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Status van functievlaggen bijwerken

De statuswaarde van een functievlag wijzigen:

1. Selecteer **Functiebeheer**.

1. Rechts van de functievlag die u wilt wijzigen, selecteert u het beletselteken ( **...** ) en selecteert u vervolgens **Bewerken**.

1. Stel een nieuwe status voor de functievlag in.

## <a name="access-feature-flags"></a>Functievlaggen openen

Functievlaggen die met Functiebeheer zijn gemaakt, worden als normale sleutelwaarden opgeslagen en opgehaald. Ze worden bewaard onder een speciaal naamruimtevoorvoegsel, `.appconfig.featureflag`. Met Configuratieverkenner kunnen de onderliggende sleutelwaarden worden weergegeven. Deze waarden kunnen door de toepassing worden opgehaald met behulp van de configuratieproviders van App Configuration, SDK's, opdrachtregelextensies en REST API's.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u functievlaggen en hun statussen kunt beheren met behulp van App Configuration. Raadpleeg het volgende artikel voor meer informatie over de ondersteuning van functiebeheer in ASP.NET Core en App Configuration:

* [Functievlaggen gebruiken in een ASP.NET Core-app](./use-feature-flags-dotnet-core.md)
