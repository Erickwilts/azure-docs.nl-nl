---
title: Een IoT hub-gebeurtenisbron toevoegen aan Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een gebeurtenisbron die is verbonden met een IoT-hub aan uw Time Series Insights-omgeving toevoegen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: 955b0e36c63b181e2fe6d2f87e7b015196fceff9
ms.sourcegitcommit: ec7b0bf593645c0d1ef401a3350f162e02c7e9b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455601"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>Een IoT hub-gebeurtenisbron toevoegen aan uw Time Series Insights-omgeving

In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het toevoegen van een gebeurtenisbron die gegevens uit Azure IoT Hub voor uw Azure Time Series Insights-omgeving leest.

> [!NOTE]
> De instructies in dit artikel zijn van toepassing Azure Time Series Insights algemeen beschikbaar en omgevingen met Time Series Insights Preview.

## <a name="prerequisites"></a>Vereisten

* Maak een [Azure Time Series Insights-omgeving](time-series-insights-update-create-environment.md).
* Maak een [IoT-hub met behulp van de Azure-portal](../iot-hub/iot-hub-create-through-portal.md).
* De IoT-hub moet actieve berichtgebeurtenissen worden verzonden in hebben.
* Maak een speciale klantengroep in voor de Time Series Insights-omgeving te gebruiken vanuit de IoT-hub. Elke Time Series Insights-gebeurtenisbron beschikken over een eigen speciale klantengroep die niet worden gedeeld met alle andere consumenten. Als meerdere lezers gebruiken van gebeurtenissen uit de dezelfde consumergroep, worden alle lezers waarschijnlijk mislukte tests ziet. Zie voor meer informatie, de [Ontwikkelaarshandleiding voor Azure IoT Hub](../iot-hub/iot-hub-devguide.md).

### <a name="add-a-consumer-group-to-your-iot-hub"></a>Een consumentengroep toevoegen aan uw IoT-hub

Toepassingen gebruiken consumentengroepen gegevens wilt halen uit Azure IoT Hub. Om te lezen op betrouwbare wijze gegevens uit uw IoT-hub, bieden een speciale klantengroep die alleen wordt gebruikt door deze Time Series Insights-omgeving.

Een nieuwe consumergroep toevoegen aan uw IoT-hub:

1. In de Azure-portal, zoeken en openen van uw IoT-hub.

1. Onder **instellingen**, selecteer **ingebouwde eindpunten**, en selecteer vervolgens de **gebeurtenissen** eindpunt.

   [![Selecteer op de pagina eindpunten Build in de gebeurtenissen-knop](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_one.png#lightbox)

1. Onder **consumentengroepen**, voer een unieke naam voor de consumergroep. Wanneer u een nieuwe gebeurtenisbron maken, moet u deze dezelfde naam gebruiken in uw Time Series Insights-omgeving.

1. Selecteer **Opslaan**.

## <a name="add-a-new-event-source"></a>Een nieuwe gebeurtenisbron toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **Alle resources** in het menu aan de linkerkant. Selecteer uw Time Series Insights-omgeving.

1. Onder **omgeving topologie**, selecteer **gebeurtenisbronnen**, en selecteer vervolgens **toevoegen**.

   [![Selecteer gebeurtenisbronnen en selecteer vervolgens de knop toevoegen](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_two.png#lightbox)

1. In de **nieuwe gebeurtenisbron** in het deelvenster voor **naam van bron**, voer een naam die uniek is voor deze Time Series Insights-omgeving. Voer bijvoorbeeld **gebeurtenisstroom**.

1. Voor **bron**, selecteer **IoT-Hub**.

1. Selecteer een waarde voor **importoptie**:

   * Als u al een IoT-hub in een van uw abonnementen hebt, selecteert u **gebruikt IoT-Hub in de beschikbare abonnementen**. Deze optie is de eenvoudigste oplossing.
   
     [![Opties selecteren in het nieuwe deelvenster van de gebeurtenis-bron](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_three.png#lightbox)

    * De volgende tabel beschrijft de eigenschappen die vereist zijn voor de **gebruikt IoT-Hub in de beschikbare abonnementen** optie:

       [![Bron-deelvenster nieuwe gebeurtenis - eigenschappen instellen in de IoT-Hub gebruiken via de optie van de beschikbare abonnementen](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_four.png#lightbox)

       | Eigenschap | Description |
       | --- | --- |
       | Abonnements-id | Selecteer het abonnement waarin de IoT-hub is gemaakt.
       | De naam van de IoT-hub | Selecteer de naam van de IoT-hub.
       | Naam van IoT hub-beleid | Selecteer het beleid voor gedeelde toegang. Het beleid voor gedeelde toegang vindt u op het tabblad instellingen van IoT-hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **service verbinding maken met** machtigingen.
       | Beleidssleutel voor IoT hub | De sleutel wordt vooraf ingevuld.
       | IoT hub-consumentengroep | De consumentengroep die gebeurtenissen uit de IoT-hub kan lezen. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken.
       | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
       | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar de IoT hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

    * Als de IoT-hub bevindt zich buiten uw abonnementen, of als u geavanceerde opties kiest, selecteert u **bieden IoT Hub-instellingen handmatig**.

      De volgende tabel beschrijft de vereiste eigenschappen voor de **bieden IoT Hub-instellingen handmatig**:

       | Eigenschap | Description |
       | --- | --- |
       | Abonnements-id | Het abonnement waarin de IoT-hub is gemaakt.
       | Resourcegroep | Naam van de resourcegroep waarin de IoT-hub is gemaakt.
       | De naam van de IoT-hub | de naam van uw IoT-hub. Wanneer u uw IoT-hub gemaakt, kunt u een naam voor de IoT hub ingevoerd.
       | Naam van IoT hub-beleid | Het beleid voor gedeelde toegang. U kunt het beleid voor gedeelde toegang maken op het tabblad instellingen van IoT-hub. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor uw gebeurtenisbron *moet* hebben **service verbinding maken met** machtigingen.
       | Beleidssleutel voor IoT hub | De gedeelde toegangssleutel die wordt gebruikt voor het verifiëren van toegang tot de Azure Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
       | IoT hub-consumentengroep | De consumentengroep die gebeurtenissen uit de IoT-hub kan lezen. Het is raadzaam dat u een speciale klantengroep voor uw gebeurtenisbron gebruiken.
       | Serialisatie-indeling voor gebeurtenissen | JSON is momenteel de enige beschikbare serialisatie-indeling. De gebeurtenisberichten moeten zich in deze indeling of er zijn geen gegevens kunnen worden gelezen. |
       | Naam van de eigenschap timestamp | Om te bepalen deze waarde, moet u inzicht in de indeling van het bericht van de berichtgegevens die worden verzonden naar de IoT hub. Deze waarde is de **naam** van de eigenschap specifieke gebeurtenis in de berichtgegevens die u wilt gebruiken als de tijdstempel van de gebeurtenis. De waarde is hoofdlettergevoelig. Als dit veld leeg blijft, de **tijd van de gebeurtenis in de wachtrij plaatsen** in de gebeurtenis bron wordt gebruikt als de tijdstempel van de gebeurtenis. |

1. Voeg de toegewezen Time Series Insights consument groepsnaam die u hebt toegevoegd aan uw IoT-hub.

1. Selecteer **Maken**.

   [![De knop maken](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png)](media/time-series-insights-how-to-add-an-event-source-iothub/iothub_five.png#lightbox)

1. Nadat u de gebeurtenisbron hebt gemaakt, begint Time Series Insights automatisch streaming-gegevens aan uw omgeving.

## <a name="next-steps"></a>Volgende stappen

* [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) om de gegevens te beveiligen.

* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.

* Toegang tot uw omgeving in de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
