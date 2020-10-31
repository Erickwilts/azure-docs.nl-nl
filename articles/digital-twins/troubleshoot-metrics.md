---
title: Metrische gegevens weer geven met Azure Monitor
titleSuffix: Azure Digital Twins
description: Zie de metrische gegevens van Azure Digital Apparaatdubbels weer geven in Azure Monitor.
author: baanders
ms.author: baanders
ms.date: 8/4/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 7fde67ab9b9160bb89493748d09e83bd9cfcff34
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2020
ms.locfileid: "93091698"
---
# <a name="troubleshooting-azure-digital-twins-metrics"></a>Problemen met Azure Digital Apparaatdubbels: metrische gegevens oplossen

De metrische gegevens die in dit artikel worden beschreven, bevatten informatie over de status van Azure Digital Apparaatdubbels-resources in uw Azure-abonnement. Met de metrische gegevens van Azure Digital Apparaatdubbels kunt u de algemene status van de Azure Digital Apparaatdubbels-service en de gekoppelde resources beoordelen. Met deze gebruikers gerichte statistieken kunt u zien wat er gebeurt met uw Azure Digital Apparaatdubbels en kunt u de hoofd oorzaak van problemen analyseren zonder dat u contact hoeft op te nemen met de ondersteuning van Azure.

Metrische gegevens zijn standaard ingeschakeld. U kunt de metrische gegevens van de Azure Digital Apparaatdubbels bekijken via de [Azure Portal](https://portal.azure.com).

## <a name="how-to-view-azure-digital-twins-metrics"></a>De metrische gegevens van Azure Digital Apparaatdubbels weer geven

1. Maak een Azure Digital Apparaatdubbels-exemplaar. U kunt instructies vinden over het instellen van een Azure Digital Apparaatdubbels-instantie in [*How-to: een exemplaar en verificatie instellen*](how-to-set-up-instance-portal.md).

2. Zoek uw Azure Digital Apparaatdubbels-exemplaar in de [Azure Portal](https://portal.azure.com) (u kunt de pagina hiervoor openen door de naam ervan te typen in de zoek balk van de portal). 

    Selecteer in het menu van de instantie **metrische gegevens** .
   
    :::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Scherm afbeelding van de pagina met metrische gegevens voor Azure Digital Apparaatdubbels":::

    Op deze pagina worden de metrische gegevens weer gegeven voor uw Azure Digital Apparaatdubbels-exemplaar. U kunt ook aangepaste weer gaven van uw metrische gegevens maken door de items te selecteren die u wilt weer geven in de lijst.
    
3. U kunt ervoor kiezen om uw metrische gegevens te verzenden naar een Event Hubs-eind punt of een Azure Storage-account door **Diagnostische instellingen** te selecteren in het menu en vervolgens **Diagnostische instelling toe te voegen** .

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Scherm afbeelding van de pagina met metrische gegevens voor Azure Digital Apparaatdubbels":::

    Zie voor meer informatie over dit proces [*probleem oplossing: diagnostische gegevens instellen*](troubleshoot-diagnostics.md).

4. U kunt ervoor kiezen om waarschuwingen in te stellen voor uw metrische gegevens door **waarschuwingen** te selecteren in het menu en vervolgens **+ nieuwe waarschuwings regel** .
    :::image type="content" source="media/troubleshoot-alerts/alerts-pre.png" alt-text="Scherm afbeelding van de pagina met metrische gegevens voor Azure Digital Apparaatdubbels":::

    Zie [*probleem oplossing: waarschuwingen instellen*](troubleshoot-alerts.md)voor meer informatie over dit proces.

## <a name="azure-digital-twins-metrics-and-how-to-use-them"></a>Azure Digital Apparaatdubbels-metrische gegevens en hoe u deze kunt gebruiken

Azure Digital Apparaatdubbels biedt diverse metrische gegevens om u een overzicht te geven van de status van uw exemplaar en de bijbehorende resources. U kunt ook informatie uit meerdere metrische gegevens combi neren om een grotere afbeelding van de status van uw exemplaar te tekenen. 

In de volgende tabellen worden de metrische gegevens beschreven die worden bijgehouden door elk Azure Digital Apparaatdubbels-exemplaar, en wordt uitgelegd hoe elke metriek in verhouding staat tot de algemene status van uw exemplaar.

#### <a name="api-request-metrics"></a>Metrische gegevens van API-aanvraag

Metrische gegevens met API-aanvragen:

| Metrisch gegeven | Weergave naam voor metrische gegevens | Eenheid | Aggregatietype| Beschrijving | Afmetingen |
| --- | --- | --- | --- | --- | --- |
| ApiRequests | API-aanvragen | Aantal | Totaal | Het aantal API-aanvragen voor digitale-Apparaatdubbels voor lezen, schrijven, verwijderen en query bewerkingen. |  Verificatie <br>Schijf <br>Protocolsubstatus <br>Status code, <br>Klasse status code, <br>Status tekst |
| ApiRequestsFailureRate | Aantal mislukte API-aanvragen | Percentage | Average | Het percentage van de API-aanvragen dat door de service wordt ontvangen voor uw exemplaar en die een interne fout (500) reactie code geven voor digitale Apparaatdubbels-Lees-, schrijf-, verwijder-en query bewerkingen. | Verificatie <br>Schijf <br>Protocolsubstatus <br>Status code, <br>Klasse status code, <br>Status tekst
| ApiRequestsLatency | Latentie van API-aanvragen | Milliseconden | Average | De reactie tijd voor API-aanvragen. Dit is de tijd tussen het moment waarop de aanvraag wordt ontvangen door Azure Digital Apparaatdubbels totdat de service een geslaagd/mislukt resultaat verzendt voor digitale Apparaatdubbels lezen, schrijven, verwijderen en query bewerkingen. | Verificatie <br>Schijf <br>Protocol |

#### <a name="billing-metrics"></a>Metrische facturerings gegevens

Metrische gegevens met facturering:

>[!NOTE]
>Deze metrische gegevens worden nog steeds weer gegeven in de lijst die kan worden geselecteerd en blijven op nul totdat de nieuwe prijzen voor de service beschikbaar worden. Zie [*prijzen voor Azure Digital apparaatdubbels*](https://azure.microsoft.com/pricing/details/digital-twins/)voor meer informatie.

| Metrisch gegeven | Weergave naam voor metrische gegevens | Eenheid | Aggregatietype| Beschrijving | Afmetingen |
| --- | --- | --- | --- | --- | --- |
| BillingApiOperations | Facturering-API-bewerkingen | Aantal | Totaal | Facturerings metriek voor het aantal API-aanvragen dat is gedaan voor de Azure Digital Apparaatdubbels-service. | Meter-id |
| BillingMessagesProcessed | Verwerkte facturerings berichten | Aantal | Totaal | Facturerings metriek voor het aantal berichten dat vanuit Azure Digital Apparaatdubbels naar externe eind punten wordt verzonden.<br><br>Een nettolading mag niet groter zijn dan 1 KB om te worden beschouwd als één bericht voor facturerings doeleinden. Nettoladingen die groter zijn dan deze worden geteld als extra berichten in stappen van 1 KB (dus een bericht tussen 1 en 2 KB wordt geteld als 2 berichten, tussen 2 en 3 KB is 3 berichten, enzovoort).<br>Deze beperking geldt ook voor antwoorden, dus een aanroep die 1,5 KB retourneert in de hoofd tekst van de reactie, wordt bijvoorbeeld gefactureerd als twee bewerkingen. | Meter-id |
| BillingQueryUnits | Facturerings query eenheden | Aantal | Totaal | Het aantal query-eenheden, een intern berekende meting van het gebruik van de service resource, die wordt gebruikt om query's uit te voeren. Er is ook een helper-API beschikbaar voor het meten van query-eenheden: [klasse QueryChargeHelper](/dotnet/api/azure.digitaltwins.core.querychargehelper?preserve-view=true&view=azure-dotnet-preview) | Meter-id |

#### <a name="ingress-metrics"></a>Metrische gegevens binnenkomend

Metrische gegevens die moeten worden uitgevoerd met inkomend verkeer:

| Metrisch gegeven | Weergave naam voor metrische gegevens | Eenheid | Aggregatietype| Beschrijving | Afmetingen |
| --- | --- | --- | --- | --- | --- |
| IngressEvents | Ingangs gebeurtenissen | Aantal | Totaal | Het aantal inkomende telemetrie-gebeurtenissen in azure Digital Apparaatdubbels. | Resultaat |
| IngressEventsFailureRate | Aantal mislukte ingangs gebeurtenissen | Percentage | Average | Het percentage inkomende telemetrie-gebeurtenissen waarvoor de service een interne fout (500) respons code retourneert. | Resultaat |
| IngressEventsLatency | Latentie van ingangs gebeurtenissen | Milliseconden | Average | Het tijdstip waarop een gebeurtenis arriveert wanneer deze klaar is om te worden egressed door Azure Digital Apparaatdubbels, waarbij de service een geslaagd/mislukt resultaat verzendt. | Resultaat |

#### <a name="routing-metrics"></a>Routerings metrieken

Metrische gegevens die moeten worden uitgevoerd met route ring:

| Metrisch gegeven | Weergave naam voor metrische gegevens | Eenheid | Aggregatietype| Beschrijving | Afmetingen |
| --- | --- | --- | --- | --- | --- |
| MessagesRouted | Berichten gerouteerd | Aantal | Totaal | Het aantal berichten dat wordt doorgestuurd naar een Azure-service voor eind punten, zoals Event hub, Service Bus of Event Grid. | Type eind punt, <br>Resultaat |
| RoutingFailureRate | Aantal mislukte routeringen | Percentage | Average | Het percentage gebeurtenissen dat resulteert in een fout wanneer ze worden doorgestuurd van Azure Digital Apparaatdubbels naar een Azure-service voor eind punten, zoals Event hub, Service Bus of Event Grid. | Type eind punt, <br>Resultaat |
| RoutingLatency | Routerings latentie | Milliseconden | Average | De tijd die is verstreken tussen een gebeurtenis die wordt gerouteerd van Azure Digital Apparaatdubbels naar het moment dat deze wordt geplaatst in de Azure-service voor eind punten, zoals Event hub, Service Bus of Event Grid. | Type eind punt, <br>Resultaat |

## <a name="dimensions"></a>Afmetingen

Met dimensies kunt u meer informatie over de metrische gegevens vaststellen. Sommige para meters voor route ring bieden informatie per eind punt. De volgende tabel bevat mogelijke waarden voor deze dimensies.

| Dimensie | Waarden |
| --- | --- |
| Verificatie | OAuth |
| Bewerking (voor API-aanvragen) | Micro soft. DigitalTwins/DigitalTwins/verwijderen, <br>Micro soft. DigitalTwins/DigitalTwins/schrijven, <br>Micro soft. DigitalTwins/DigitalTwins/lezen, <br>Micro soft. DigitalTwins/eventroutes/lezen, <br>Micro soft. DigitalTwins/eventroutes/schrijven, <br>Micro soft. DigitalTwins/eventroutes/verwijderen, <br>Micro soft. DigitalTwins/modellen/lezen, <br>Micro soft. DigitalTwins/modellen/schrijven, <br>Micro soft. DigitalTwins/modellen/verwijderen, <br>Micro soft. DigitalTwins/query/actie |
| Eindpunttype | Event Grid <br>Event hub, <br>Service Bus |
| Protocol | HTTPS |
| Resultaat | Geleverd <br>Fout |
| Statuscode | 200, 404, 500, enzovoort. |
| Klasse status code | 2xx, 4xx, 5xx, enzovoort. |
| Status tekst | Er is een interne server fout, niet gevonden, enzovoort. |

## <a name="next-steps"></a>Volgende stappen

Zie [*probleem oplossing: diagnostische gegevens instellen*](troubleshoot-diagnostics.md)voor meer informatie over het beheren van vastgelegde metrische gegevens voor Azure Digital apparaatdubbels.
