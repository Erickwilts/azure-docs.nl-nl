---
title: Azure Event Grid-abonnementen via de portal
description: Beschrijft hoe u Event Grid-abonnementen via de portal maakt.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: spelluru
ms.openlocfilehash: b54bc52a2feaf4646d801265ddb273c2c86158ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60823446"
---
# <a name="subscribe-to-events-through-portal"></a>Abonneren op gebeurtenissen tot en met portal

In dit artikel wordt beschreven hoe u Event Grid-abonnementen via de portal maken.

## <a name="create-event-subscriptions"></a>Gebeurtenisabonnementen maken

Het maken van een Event Grid-abonnement voor het gebruik van de ondersteunde [gebeurtenisbronnen](event-sources.md), gebruik de volgende stappen. In dit artikel laat zien hoe een Event Grid-abonnement voor een Azure-abonnement maken.

1. Selecteer **Alle services**.

   ![Alle services selecteren](./media/subscribe-through-portal/select-all-services.png)

1. Zoeken naar **Event Grid-abonnementen** en selecteert u deze uit de beschikbare opties.

   ![Search](./media/subscribe-through-portal/search.png)

1. Selecteer **+ Gebeurtenisabonnement**.

   ![Abonnement toevoegen](./media/subscribe-through-portal/add-subscription.png)

1. Selecteer het type abonnement die u wilt maken. Selecteer bijvoorbeeld het volgende om u te abonneren op gebeurtenissen voor uw Azure-abonnement, **Azure-abonnementen** en het doelabonnement.

   ![Selecteer Azure-abonnement](./media/subscribe-through-portal/azure-subscription.png)

1. Om u te abonneren op alle gebeurtenistypen voor deze bron van gebeurtenis, behouden de **abonneren op alle gebeurtenistypen** optie is ingeschakeld. Anders selecteert u de typen gebeurtenissen voor dit abonnement.

   ![Selecteer de typen gebeurtenissen](./media/subscribe-through-portal/select-event-types.png)

1. Geef aanvullende informatie over het gebeurtenisabonnement, zoals het eindpunt voor het verwerken van gebeurtenissen en de naam van een abonnement.

   ![Geef details van abonnement](./media/subscribe-through-portal/provide-subscription-details.png)

1. Onbestelbare inschakelen en aanpassen van beleid voor opnieuw proberen, selecteert u **extra functies**.

   ![Extra functies selecteren](./media/subscribe-through-portal/select-additional-features.png)

1. Selecteer een container wilt gebruiken voor het opslaan van gebeurtenissen die niet worden bezorgd en instellen hoe nieuwe pogingen worden verzonden.

   ![Onbestelbare en probeer het opnieuw inschakelen](./media/subscribe-through-portal/set-deadletter-retry.png)

1. Wanneer u klaar bent, selecteert u **Maken**.

## <a name="create-subscription-on-resource"></a>Abonnement maken voor de resource

Sommige bronnen van gebeurtenissen ondersteuning voor het maken van een gebeurtenisabonnement via de interface van de portal voor die bron. Selecteer de gebeurtenisbron en zoek naar **gebeurtenissen** in het linkerdeelvenster.

![Geef details van abonnement](./media/subscribe-through-portal/resource-events.png)

De portal biedt u de opties voor het maken van een gebeurtenisabonnement die relevant is voor die bron.

## <a name="next-steps"></a>Volgende stappen

* Voor informatie over de bezorging van gebeurtenissen en nieuwe pogingen, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
