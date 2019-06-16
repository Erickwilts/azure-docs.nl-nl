---
title: Met Azure Monitor-logs - Azure Logic Apps B2B-berichten bijhouden | Microsoft Docs
description: Bijhouden van de B2B-communicatie voor integratieaccounts en Azure Logic Apps met Azure Log Analytics
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 8cf5d9f3ee1503769a2ec199847175899bcd86bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "62120123"
---
# <a name="track-b2b-messages-with-azure-monitor-logs"></a>B2B-berichten bijhouden met Azure Monitor-logboeken

Na het instellen van B2B-communicatie tussen handelspartners in uw integratie-account, kunnen deze partners berichten met protocollen zoals AS2, X 12 en EDIFACT uitwisselen. Om te zien dat deze berichten correct worden verwerkt, kunt u deze berichten met bijhouden [logboeken van Azure Monitor](../log-analytics/log-analytics-overview.md). Bijvoorbeeld, kunt u deze bijhouden op basis van een web-mogelijkheden voor het bijhouden van berichten:

* Aantal berichten en status
* Status van bevestigingen
* Berichten correleren met bevestigingen
* Beschrijvingen van de gedetailleerde fouten voor mislukte aanmeldingen
* Zoekopties

> [!NOTE]
> Eerder beschreven stappen voor het uitvoeren van deze taken met de Microsoft Operations Management Suite (OMS), is deze pagina [buiten gebruik stellen in januari 2019](../azure-monitor/platform/oms-portal-transition.md), vervangt u deze stappen met Azure Log Analytics op in plaats daarvan. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Vereisten

* Een logische app die ingesteld met de logboekregistratie van diagnostische gegevens. Informatie over [over het maken van een logische app](quickstart-create-first-logic-app-workflow.md) en [over het instellen van logboekregistratie voor deze logische app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integratie-account ingesteld met bewaking en logboekregistratie. Informatie over [over het maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [over het instellen van controle en logboekregistratie voor dat account](../logic-apps/logic-apps-monitor-b2b-message.md).

* Als u dat nog niet gedaan hebt, [diagnostische gegevens publiceren naar Azure Monitor logboeken](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

* Nadat u de bovenstaande vereisten voldoen, moet u ook een Log Analytics-werkruimte die u gebruiken voor het bijhouden van B2B-communicatie via Log Analytics. Als u een Log Analytics-werkruimte hebt, krijgt u informatie [over het maken van een Log Analytics-werkruimte](../azure-monitor/learn/quick-create-workspace.md).

## <a name="install-logic-apps-b2b-solution"></a>Logische Apps B2B-oplossing installeren

Voordat u Azure Monitor-logboeken bijhouden van B2B-berichten voor uw logische app hebt kunt, voegt u toe de **Logic Apps B2B** oplossing naar Azure Monitor-Logboeken. Meer informatie over [oplossingen toevoegen aan Azure Monitor-logboeken](../azure-monitor/learn/quick-create-workspace.md).

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Alle services**. In het zoekvak zoeken "log analytics" en selecteer **Log Analytics**.

   ![Selecteer de Log Analytics](media/logic-apps-track-b2b-messages-omsportal/find-log-analytics.png)

1. Onder **Log Analytics**, zoek en selecteer uw Log Analytics-werkruimte. 

   ![Log Analytics-werkruimte selecteren](media/logic-apps-track-b2b-messages-omsportal/select-log-analytics-workspace.png)

1. Onder **aan de slag met Log Analytics** > **bewakingsoplossingen configureren**, kiest u **oplossingen weergeven**.

   ![Kies "Oplossingen weergeven"](media/logic-apps-track-b2b-messages-omsportal/log-analytics-workspace.png)

1. Kies op de pagina overzicht **toevoegen**, wordt geopend de **beheeroplossingen** lijst. Uit de lijst, selecteer **Logic Apps B2B**. 

   ![Selecteer logische Apps B2B-oplossing](media/logic-apps-track-b2b-messages-omsportal/add-b2b-solution.png)

   Als u de oplossing, klikt u onder aan de lijst niet vinden Kies **meer laden** totdat de oplossing wordt weergegeven.

1. Kies **maken**, Controleer of de Log Analytics-werkruimte waar u wilt installeren van de oplossing en kies vervolgens **maken** opnieuw.   

   ![Kies 'Maken' voor Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/create-b2b-solution.png)

   Als u niet wilt gebruiken van een bestaande werkruimte, kunt u ook een nieuwe werkruimte maken op dit moment.

1. Als u klaar bent, gaat u terug naar van uw werkruimte **overzicht** pagina. 

   De Logic Apps B2B-oplossing wordt nu weergegeven op de pagina overzicht. 
   Wanneer de B2B-berichten worden verwerkt, wordt het aantal berichten op deze pagina wordt bijgewerkt.

<a name="message-status-details"></a>

## <a name="view-b2b-message-information"></a>B2B-bericht weergeven

Nadat de B2B-berichten worden verwerkt, kunt u de status en details voor deze berichten bekijken op de **Logic Apps B2B** tegel.

1. Ga naar uw logische Analytics-werkruimte en open de pagina overzicht. Kies **Logic Apps B2B**.

   ![Aantal bijgewerkte berichten](media/logic-apps-track-b2b-messages-omsportal/b2b-overview-tile.png)

   > [!NOTE]
   > Standaard de **Logic Apps B2B** tegel ziet u gegevens op basis van een enkele dag. Als u wilt het bereik van gegevens naar een ander interval wijzigen, kies de bereik-besturingselement aan de bovenkant van de pagina:
   > 
   > ![Interval wijzigen](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)

1. Na het bericht wordt statusdashboard weergegeven, vindt u meer informatie over een specifieke berichttype met de gegevens op basis van een enkele dag. Kies de tegel voor **AS2**, **X12**, of **EDIFACT**.

   ![Bericht-status weergeven](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Er wordt een lijst met berichten weergegeven voor uw gekozen tegel. 
   Zie voor meer informatie over de eigenschappen voor elk berichttype, deze beschrijvingen van de eigenschap bericht:

   * [Eigenschappen van de AS2-berichten](#as2-message-properties)
   * [Berichteigenschappen die X12](#x12-message-properties)
   * [Eigenschappen van de EDIFACT-berichten](#EDIFACT-message-properties)

   Dit is wat een AS2-bericht-lijst eruit kan:

   ![AS2-berichten weergeven](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Als u wilt weergeven of exporteren van de invoer en uitvoer voor specifieke berichten, selecteert u deze berichten en kies **downloaden**. Wanneer u wordt gevraagd, sla het ZIP-bestand op uw lokale computer en pak vervolgens dat bestand. 

   De uitgepakte map bevat een map voor elk geselecteerde bericht. 
   Als u van bevestigingen ingesteld, wordt in de berichtenmap ook bestanden met de bevestiging details bevat. 
   De berichtenmap voor elk heeft ten minste deze bestanden: 
   
   * Leesbare bestanden met de nettolading van de invoer en uitvoer nettolading details
   * Gecodeerde bestanden met de invoer en uitvoer

   Voor elk berichttype, kunt u de map en de naam bestandsindelingen hier vinden:

   * [AS2-map en elk bestand naam indelingen](#as2-folder-file-names)
   * [X12 map en elk bestand naam indelingen](#x12-folder-file-names)
   * [EDIFACT-map en elk bestand naam indelingen](#edifact-folder-file-names)

   ![Bestanden downloaden](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Om weer te geven Voer alle acties die dezelfde ID, op de **zoeken in logboeken** pagina, kiest u een bericht uit de lijst met berichten.

   U kunt deze acties door de kolom, of zoeken naar specifieke resultaten sorteren.

   ![Bewerkingen met dezelfde ID worden uitgevoerd](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Zoekresultaten met vooraf gedefinieerde query's, kies **Favorieten**.

   * Informatie over [over het bouwen van query's door filters toe te voegen](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Of lees meer over [over het vinden van gegevens met zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md).

   * Als u wilt wijzigen van query's in het zoekvak, werk de query met de kolommen en waarden die u wilt gebruiken als filters.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>De eigenschapbeschrijvingen en Naamindelingen voor AS2, X 12 en EDIFACT-berichten

Voor elk berichttype hier worden de eigenschapbeschrijvingen en typen Naamindelingen voor gedownloade bestanden.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Eigenschapbeschrijvingen AS2-bericht

Hier volgen eigenschapbeschrijvingen voor elk AS2-bericht.

| Eigenschap | Description |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een AS2-overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een AS2-overeenkomst |
| Logische apps | De logische app waar de AS2-acties zijn ingesteld |
| Status | De status van de AS2-bericht <br>Geslaagd = ontvangen of een geldige AS2-bericht verzonden. Er is geen MDN is ingesteld. <br>Geslaagd = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en ontvangen, of MDN wordt verzonden. <br>Kan geen = een ongeldige AS2-bericht ontvangen. Er is geen MDN is ingesteld. <br>In behandeling = ontvangen of een geldige AS2-bericht verzonden. MDN is ingesteld en MDN wordt verwacht. |
| Ack | De status van het MDN-bericht <br>Geaccepteerd = ontvangen of een positief MDN verzonden. <br>In afwachting van = wachten ontvangen of een MDN te verzenden. <br>Afgewezen = ontvangen of een negatieve MDN verzonden. <br>Niet vereist = MDN is niet ingesteld in de overeenkomst. |
| Direction | De richting van de AS2-bericht |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Bericht-ID | De AS2-bericht-ID van de AS2-bericht-headers |
| Tijdstempel | De tijd wanneer het bericht worden verwerkt door de AS2-actie |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 typen Naamindelingen voor gedownloade bestanden

Hier volgen de typen Naamindelingen voor elke map voor gedownloade AS2-bericht en de bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_AS2\_[correlation-ID]\_[message-ID]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_AS2\_[correlatie-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 message eigenschapbeschrijvingen

Hier vindt u de eigenschapbeschrijvingen voor elke X12 bericht.

| Eigenschap | Description |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een X12 overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een X12 overeenkomst |
| Logische apps | De logische app waar de X12 acties zijn ingesteld |
| Status | De X12 status message <br>Geslaagd = ontvangen of een geldige X12 verzonden bericht. Er zijn geen functionele ack is ingesteld. <br>Geslaagd = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan geen = ontvangen of een ongeldige X12 verzonden bericht. <br>In behandeling = ontvangen of een geldige X12 verzonden bericht. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| Ack | Status van functionele Ack (een 997-bevestiging) <br>Geaccepteerd = ontvangen of een positief functionele ack. verzonden <br>Afgewezen = ontvangen of een negatieve functionele ack. verzonden <br>In afwachting van = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar niet verzenden naar partner. <br>Niet vereist = functionele ack is niet ingesteld. |
| Direction | De X12 bericht richting |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Berichttype | Het berichttype voor EDI, X 12 |
| ICN | De Uitwisselingscontrolenummer voor de X12 bericht |
| TSCN | De transactie ingesteld controlenummer voor de X12 bericht |
| Tijdstempel | De tijd wanneer de X12 actie verwerkt het bericht |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 naam indelingen voor gedownloade bestanden

Hier vindt u de van Naamindelingen voor elk X12 gedownload bericht map en bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_X12\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_X12\_[controle-uitwisseling-aantal]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT-bericht-eigenschapbeschrijvingen

Hier volgen eigenschapbeschrijvingen voor elk EDIFACT-bericht.

| Eigenschap | Description |
| --- | --- |
| Afzender | De gastpartner die is opgegeven in **instellingen ontvangen**, of de hostpartner die is opgegeven in **instellingen voor verzenden** voor een EDIFACT-overeenkomst |
| Ontvanger | De hostpartner die is opgegeven in **instellingen ontvangen**, of de gastpartner is opgegeven **instellingen voor verzenden** voor een EDIFACT-overeenkomst |
| Logische apps | De logische app waar de EDIFACT-acties zijn ingesteld |
| Status | De status van de EDIFACT-bericht <br>Geslaagd = ontvangen of een geldige EDIFACT-bericht verzonden. Er zijn geen functionele ack is ingesteld. <br>Geslaagd = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en ontvangen, of een functionele ack wordt verzonden. <br>Kan geen = ontvangen of heeft een ongeldige EDIFACT-bericht verzonden <br>In behandeling = ontvangen of een geldige EDIFACT-bericht verzonden. Functionele ack is ingesteld en een functionele ack wordt verwacht. |
| Ack | Status van functionele Ack (een 997-bevestiging) <br>Geaccepteerd = ontvangen of een positief functionele ack. verzonden <br>Afgewezen = ontvangen of een negatieve functionele ack. verzonden <br>In afwachting van = een functionele ack verwacht maar niet ontvangen. <br>In behandeling zijnde = gegenereerd een functionele ack maar niet verzenden naar partner. <br>Niet vereist = functionele Ack is niet ingesteld. |
| Direction | De richting van de EDIFACT-bericht |
| Correlatie-id | De ID die overeenkomt met alle triggers en acties in een logische app |
| Berichttype | Het type EDIFACT-bericht |
| ICN | De Uitwisselingscontrolenummer voor de EDIFACT-bericht |
| TSCN | De transactie ingesteld controlenummer voor de EDIFACT-bericht |
| Tijdstempel | De tijd waarop de EDIFACT-actie het bericht verwerkt |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT typen Naamindelingen voor gedownloade bestanden

Hier volgen de typen Naamindelingen voor elke map voor gedownloade EDIFACT-bericht en de bestanden.

| Bestand of map | Naamindeling |
| :------------- | :---------- |
| Berichtenmap | [sender]\_[receiver]\_EDIFACT\_[interchange-control-number]\_[global-control-number]\_[transaction-set-control-number]\_[timestamp] |
| Invoer en uitvoer als instellen, bevestiging bestanden | **De nettolading van de invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_input_payload.txt </p>**Uitvoerpayload**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_uitvoer\_payload.txt </p></p>**Invoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_inputs.txt </p></p>**Uitvoer**: [afzender]\_[ontvanger]\_EDIFACT\_[controle-uitwisseling-aantal]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Volgende stappen

* [B2B-berichten in Logboeken van Azure Monitor opvragen](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [Volgschema’s voor AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Volgschema’s voor X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Aangepaste volgschema 's](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
