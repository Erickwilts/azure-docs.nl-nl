---
title: Eind punten en routes beheren (Portal)
titleSuffix: Azure Digital Twins
description: Zie voor meer informatie over het instellen en beheren van eind punten en gebeurtenis routes voor Azure Digital Apparaatdubbels-gegevens met behulp van de Azure Portal.
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 21188f473cbd5a6fd2a1ee549f47ad9b0e5b8af3
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2020
ms.locfileid: "93279486"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-portal"></a>Eind punten en routes beheren in azure Digital Apparaatdubbels (Portal)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

In azure Digital Apparaatdubbels kunt u [gebeurtenis meldingen](how-to-interpret-event-data.md) naar downstream-Services of verbonden reken bronnen sturen. Dit wordt gedaan door eerst **eind punten** in te stellen die de gebeurtenissen kunnen ontvangen. U kunt vervolgens [**gebeurtenis routes**](concepts-route-events.md) maken om op te geven welke gebeurtenissen worden gegenereerd door Azure Digital apparaatdubbels naar welke eind punten worden verzonden.

Dit artikel begeleidt u bij het maken van eind punten en routes met behulp van de [Azure Portal](https://portal.azure.com).

U kunt ook eind punten en routes beheren met de [gebeurtenis routes api's](/rest/api/digital-twins/dataplane/eventroutes), de [.NET-SDK (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true)of de [Azure Digital apparaatdubbels cli](how-to-use-cli.md). Voor een versie van dit artikel die gebruikmaakt van deze mechanismen in plaats van de portal, Zie [*How to: Manage endpoints and routes (api's en CLI)*](how-to-manage-routes-apis-cli.md).

## <a name="prerequisites"></a>Vereisten

* U hebt een **Azure-account** nodig (u kunt deze [hier](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)gratis instellen)
* U hebt een **Azure Digital apparaatdubbels-exemplaar** in uw Azure-abonnement nodig. Als u nog geen exemplaar hebt, kunt u er een maken met behulp van de stappen in [*How-to: een instantie en verificatie instellen*](how-to-set-up-instance-portal.md). Laat de volgende waarden van Setup handig zijn om later in dit artikel te gebruiken:
    - Exemplaarnaam
    - Resourcegroep

U vindt deze informatie in de [Azure Portal](https://portal.azure.com) na het instellen van uw exemplaar. Meld u aan bij de portal en zoek naar de naam van uw instantie in de zoek balk van de portal.
 
:::image type="content" source="media/how-to-manage-routes-portal/search-field-portal.png" alt-text="Scherm opname van Azure Portal zoek balk.":::

Selecteer uw exemplaar in de resultaten om de detail pagina voor uw exemplaar te bekijken:

:::image type="content" source="media/how-to-manage-routes-portal/instance-details.png" alt-text="Scherm opname van Details van ADT-exemplaar." border="false":::

## <a name="create-an-endpoint-for-azure-digital-twins"></a>Een eind punt maken voor Azure Digital Apparaatdubbels

Dit zijn de ondersteunde typen eind punten die u voor uw exemplaar kunt maken:
* [Event Grid](../event-grid/overview.md) 
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Zie [*kiezen tussen Azure Messa ging Services*](../event-grid/compare-messaging-services.md)voor meer informatie over de verschillende typen eind punten.

Als u een eind punt wilt koppelen aan Azure Digital Apparaatdubbels, moet u het gebeurtenis grid-onderwerp, Event Hub of Service Bus die u voor het eind punt gebruikt, al bestaan. 

### <a name="create-an-event-grid-endpoint"></a>Een Event Grid-eind punt maken

**Vereiste** : Maak een event grid-onderwerp door de stappen in [de sectie *een aangepast onderwerp maken*](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) van de Snelstartgids Event grid *aangepaste gebeurtenissen* te volgen.

Zodra u het onderwerp hebt gemaakt, kunt u het koppelen aan Azure Digital Apparaatdubbels vanaf de pagina van uw Azure Digital Apparaatdubbels-exemplaar in de [Azure Portal](https://portal.azure.com) (u kunt het exemplaar vinden door de naam ervan in te voeren in de zoek balk van de portal).

Selecteer _eind punten_ in het menu exemplaar. Selecteer vervolgens op de pagina met *eind punten* de optie *+ een eind punt maken*. 

Op de pagina *een eind punt maken* die wordt geopend, kunt u een eind punt van het type _Event grid_ maken door het bijbehorende keuze rondje te selecteren. De overige details volt ooien: Voer een naam in voor het eind punt in het veld _naam_ , kies uw _abonnement_ in de vervolg keuzelijst en kies het vooraf gemaakte  _Event grid onderwerp_ in de derde vervolg keuzelijst.

Maak vervolgens uw eind punt op _Opslaan_.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-grid.png" alt-text="Scherm opname van het maken van een eind punt van het type Event Grid.":::

U kunt controleren of het eind punt is gemaakt door het meldings pictogram in de bovenste Azure Portal balk te controleren: 

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-notifications.png" alt-text="Scherm afbeelding van de melding om te controleren of het eind punt is gemaakt." border="false":::

U kunt ook het eind punt weer geven dat weer is gemaakt op de pagina *eind punten* voor uw Azure Digital apparaatdubbels-exemplaar.

Als het maken van het eind punt mislukt, Bekijk dan het fout bericht en probeer het over enkele minuten opnieuw.

Het onderwerp Event grid is nu beschikbaar als een eind punt in azure Digital Apparaatdubbels, onder de naam die is opgegeven in het veld _naam_ . Normaal gesp roken gebruikt u die naam als het doel van een **gebeurtenis route** , die u [later in dit artikel](#create-an-event-route)gaat maken.

### <a name="create-an-event-hubs-endpoint"></a>Een Event Hubs-eind punt maken

**Vereisten** : 
* U hebt een _Event hubs naam ruimte_ en een _Event hub_ nodig. U maakt beide door de stappen in de Event Hubs [*een event hub*](../event-hubs/event-hubs-create.md) Quick start maken te volgen.
* U hebt een _autorisatie regel_ nodig. Als u dit wilt maken, raadpleegt u de Event Hubs [*toegang verlenen tot Event hubs resources met behulp van het artikel voor hand tekeningen voor gedeelde toegang*](../event-hubs/authorize-access-shared-access-signature.md) .

Ga naar de detail pagina voor uw Azure Digital Apparaatdubbels-exemplaar in de [Azure Portal](https://portal.azure.com) (u kunt deze vinden door de naam ervan in te voeren in de zoek balk van de portal).

Selecteer _eind punten_ in het menu exemplaar. Selecteer vervolgens op de pagina met *eind punten* de optie *+ een eind punt maken*. 

Op de pagina *een eind punt maken* die wordt geopend, kunt u een eind punt van het type _Event hub_ maken door het bijbehorende keuze rondje te selecteren. Voer in het veld _naam_ een naam in voor het eind punt. Selecteer vervolgens uw _abonnement_ en uw vooraf gemaakte _Event hub-naam ruimte_ , _Event hub_ en _autorisatie regel_ uit de respectieve vervolg keuzelijsten.

Maak vervolgens uw eind punt op _Opslaan_.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-event-hub.png" alt-text="Scherm opname van het maken van een eind punt van het type Event Hubs.":::

U kunt controleren of het eind punt is gemaakt door het meldings pictogram in de bovenste Azure Portal balk te controleren. 

Als het maken van het eind punt mislukt, Bekijk dan het fout bericht en probeer het over enkele minuten opnieuw.

De Event hub is nu beschikbaar als een eind punt in azure Digital Apparaatdubbels, onder de naam die is opgegeven in het veld _naam_ . Normaal gesp roken gebruikt u die naam als het doel van een **gebeurtenis route** , die u [later in dit artikel](#create-an-event-route)gaat maken.

### <a name="create-a-service-bus-endpoint"></a>Een Service Bus-eind punt maken

**Vereisten** : 
* U hebt een _Service Bus naam ruimte_ en een _Service Bus onderwerp_ nodig. U maakt beide door de stappen te volgen in het Service Bus Snelstartgids [*en abonnementen maken*](../service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md) . U hoeft de sectie [*abonnementen maken op onderwerp*](../service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal.md#create-subscriptions-to-the-topic) niet uit te voeren.
* U hebt een _autorisatie regel_ nodig. Als u dit wilt maken, raadpleegt u het artikel Service Bus [*verificatie en autorisatie*](../service-bus-messaging/service-bus-authentication-and-authorization.md#shared-access-signature) .

Ga naar de detail pagina voor uw Azure Digital Apparaatdubbels-exemplaar in de [Azure Portal](https://portal.azure.com) (u kunt deze vinden door de naam ervan in te voeren in de zoek balk van de portal).

Selecteer _eind punten_ in het menu exemplaar. Selecteer vervolgens op de pagina met *eind punten* de optie *+ een eind punt maken*. 

Op de pagina *een eind punt maken* die wordt geopend, kunt u een eind punt van het type _Service Bus_ maken door het bijbehorende keuze rondje te selecteren. Voer in het veld _naam_ een naam in voor het eind punt. Selecteer vervolgens uw _abonnement_ en uw vooraf gemaakte _Service Bus naam ruimte_ , _Service Bus onderwerp_ en _autorisatie regel_ uit de respectieve vervolg keuzelijsten.

Maak vervolgens uw eind punt op _Opslaan_.

:::image type="content" source="media/how-to-manage-routes-portal/create-endpoint-service-bus.png" alt-text="Scherm opname van het maken van een eind punt van het type Service Bus.":::

U kunt controleren of het eind punt is gemaakt door het meldings pictogram in de bovenste Azure Portal balk te controleren. 

Als het maken van het eind punt mislukt, Bekijk dan het fout bericht en probeer het over enkele minuten opnieuw.

Het Service Bus onderwerp is nu beschikbaar als een eind punt in azure Digital Apparaatdubbels, onder de naam die is opgegeven in het veld _naam_ . Normaal gesp roken gebruikt u die naam als het doel van een **gebeurtenis route** , die u [later in dit artikel](#create-an-event-route)gaat maken.

### <a name="create-an-endpoint-with-dead-lettering"></a>Een eind punt maken met onbestelbare berichten

Wanneer een eind punt een gebeurtenis binnen een bepaalde tijds periode niet kan leveren of nadat de gebeurtenis een bepaald aantal keren is geprobeerd, kan de gebeurtenis worden verzonden naar een opslag account. Dit proces wordt **onbestelbare berichten** genoemd.

Als u een eind punt wilt maken waarvoor onbestelbare berichten zijn ingeschakeld, moet u de [arm-api's](/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) gebruiken om uw eind punt te maken, in plaats van de Azure Portal.

Zie de [*api's en CLI*](how-to-manage-routes-apis-cli.md#create-an-endpoint-with-dead-lettering) -versie van dit artikel voor instructies over hoe u dit doet met de api's.

## <a name="create-an-event-route"></a>Een gebeurtenis route maken

Als u gegevens daad werkelijk van Azure Digital Apparaatdubbels naar een eind punt wilt verzenden, moet u een **gebeurtenis route** definiëren. Met deze routes kunnen ontwikkel aars de gebeurtenis stroom, in het systeem en op downstream-Services, interactiviteit. Lees meer over gebeurtenis routes in [*concepten: route ring van Azure Digital apparaatdubbels-gebeurtenissen*](concepts-route-events.md).

Voor **waarde: u** moet eind punten maken zoals eerder in dit artikel wordt beschreven voordat u kunt verdergaan om een route te maken. U kunt door gaan met het maken van een gebeurtenis route wanneer de eind punten zijn ingesteld.

>[!NOTE]
>Als u onlangs uw eind punten hebt geïmplementeerd, controleert u of de implementatie is voltooid **voordat** u deze voor een nieuwe gebeurtenis route probeert te gebruiken. Als u de route niet kunt instellen omdat de eind punten niet gereed zijn, wacht u een paar minuten en probeert u het opnieuw.

### <a name="creation-steps-with-the-azure-portal"></a>Stappen voor het maken van de Azure Portal

Een definitie van een gebeurtenis route bevat deze elementen:
* De naam van de route die u wilt gebruiken
* De naam van het eind punt dat u wilt gebruiken
* Een filter dat definieert welke gebeurtenissen naar het eind punt worden verzonden
    - Als u de route wilt uitschakelen zodat er geen gebeurtenissen worden verzonden, gebruikt u een filter waarde van `false`
    - Als u een route wilt inschakelen die geen specifieke filters heeft, gebruikt u een filter waarde van `true`
    - Zie de sectie [*filter gebeurtenissen*](#filter-events) hieronder voor meer informatie over elk ander type filter.

U kunt meerdere meldingen en gebeurtenis typen toestaan om één route te selecteren.

Als u een gebeurtenis route wilt maken, gaat u naar de pagina Details van uw Azure Digital Apparaatdubbels-exemplaar in de [Azure Portal](https://portal.azure.com) (u kunt het exemplaar vinden door de naam ervan in te voeren in de zoek balk van de portal).

Selecteer in het menu exemplaar _gebeurtenis routes_. Selecteer vervolgens op de pagina *gebeurtenis routes* die volgt op *+ een gebeurtenis route maken*. 

Kies op de pagina *een route voor een gebeurtenis maken* die wordt geopend mini maal:
* Een naam voor uw route in het veld _naam_
* Het _eind punt_ dat u wilt gebruiken om de route te maken 

Als u de route wilt inschakelen, moet u ook **een gebeurtenis route filter** van ten minste toevoegen `true` . (Als u de standaard waarde van weglaat `false` , wordt de route gemaakt, maar er worden geen gebeurtenissen naar verzonden.) U doet dit door de schakel optie voor de _Geavanceerde editor_ in te scha kelen en te schrijven `true` in het vak *filter* .

:::image type="content" source="media/how-to-manage-routes-portal/create-event-route-no-filter.png" alt-text="Scherm opname van het maken van gebeurtenis routes voor uw exemplaar." lightbox="media/how-to-manage-routes-portal/create-event-route-no-filter.png":::

Wanneer u klaar bent, klikt u op de knop _Opslaan_ om uw gebeurtenis route te maken.

## <a name="filter-events"></a>Gebeurtenissen filteren

Zoals hierboven beschreven, hebben routes een **filter** veld. Als de filter waarde voor uw route is `false` , worden er geen gebeurtenissen naar uw eind punt verzonden. 

Nadat het minimale filter van `true` is ingeschakeld, ontvangen eind punten diverse gebeurtenissen van Azure Digital apparaatdubbels:
* Telemetrie die wordt geactiveerd door [Digital apparaatdubbels](concepts-twins-graph.md) met de Azure Digital APPARAATDUBBELS Service API
* Dubbele eigenschaps wijzigings meldingen, die worden geactiveerd bij wijzigingen in de eigenschappen voor elke dubbele in het Azure Digital Apparaatdubbels-exemplaar
* Levenscyclus gebeurtenissen, die worden geactiveerd wanneer apparaatdubbels of relaties worden gemaakt of verwijderd

U kunt de typen gebeurtenissen die worden verzonden beperken door een specifiek filter te definiëren.

Als u een gebeurtenis filter wilt toevoegen tijdens het maken van een gebeurtenis route, gebruikt u de sectie _een filter voor gebeurtenis routes toevoegen_ van de pagina *een gebeurtenis route maken* . 

U kunt een of meer algemene filter opties selecteren of de geavanceerde filter opties gebruiken om uw eigen aangepaste filters te schrijven.

#### <a name="use-the-basic-filters"></a>De basis filters gebruiken

Als u de basis filters wilt gebruiken, vouwt u de optie _gebeurtenis typen_ uit en selecteert u de selectie vakjes die overeenkomen met de gebeurtenissen die u naar het eind punt wilt verzenden. 

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-basic-1.png" alt-text="Scherm afbeelding van het maken van een gebeurtenis route met een basis filter. De selectie vakjes van de gebeurtenissen te selecteren.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Hiermee wordt het filter tekstvak automatisch gevuld met de tekst van het filter dat u hebt geselecteerd:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-basic-2.png" alt-text="Scherm afbeelding van het maken van een gebeurtenis route met een basis filter. De automatisch ingevulde filter tekst wordt weer gegeven nadat u de gebeurtenissen hebt geselecteerd.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

#### <a name="use-the-advanced-filters"></a>De geavanceerde filters gebruiken

U kunt ook de geavanceerde filter optie gebruiken om uw eigen aangepaste filters te schrijven.

Als u een gebeurtenis route met geavanceerde filter opties wilt maken, wisselt u de switch voor de _Geavanceerde editor_ in om deze in te scha kelen. U kunt vervolgens uw eigen gebeurtenis filters schrijven in het vak *filter* :

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-manage-routes-portal/create-event-route-filter-advanced.png" alt-text="Scherm afbeelding van het maken van een gebeurtenis route met een geavanceerd filter.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Dit zijn de ondersteunde route filters. Het detail in de kolom *filter tekst schema* is de tekst die kan worden ingevoerd in het vak Filter.

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de verschillende typen gebeurtenis berichten die u kunt ontvangen:
* [*Instructies: gebeurtenis gegevens interpreteren*](how-to-interpret-event-data.md)
