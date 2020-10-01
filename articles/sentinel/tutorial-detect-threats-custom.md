---
title: Aangepaste analyse regels maken voor het detecteren van bedreigingen met Azure Sentinel | Microsoft Docs
description: Gebruik deze zelf studie voor meer informatie over het maken van aangepaste analyse regels voor het detecteren van beveiligings Risico's met Azure Sentinel. Profiteer van de groepering van gebeurtenissen en het groeperen van waarschuwingen en inzicht in automatisch uitgeschakeld.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2020
ms.author: yelevin
ms.openlocfilehash: 55853cc6a3dc27df4c63e0a28ab079813040e45d
ms.sourcegitcommit: 4bebbf664e69361f13cfe83020b2e87ed4dc8fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2020
ms.locfileid: "91617176"
---
# <a name="tutorial-create-custom-analytics-rules-to-detect-threats"></a>Zelf studie: aangepaste analyse regels maken voor het detecteren van bedreigingen

Zodra u [uw gegevens bronnen](quickstart-onboard.md)   aan Azure Sentinel hebt gekoppeld, kunt u aangepaste regels maken waarmee kan worden gezocht naar specifieke criteria in uw omgeving en incidenten worden gegenereerd wanneer de criteria worden vergeleken, zodat u ze kunt onderzoeken. Deze zelf studie helpt u bij het maken van aangepaste regels voor het detecteren van bedreigingen met Azure Sentinel.

Deze zelf studie helpt u bij het detecteren van bedreigingen met Azure Sentinel.
> [!div class="checklist"]
> * Analytics-regels maken
> * Bedreigingsreacties automatiseren

## <a name="create-custom-analytics-rules"></a>Aangepaste analyse regels maken

U kunt aangepaste analyse regels maken om u te helpen bij het zoeken naar de typen bedreigingen en afwijkingen die in uw omgeving worden verdacht. De regel zorgt ervoor dat u meteen op de hoogte wordt gebracht, zodat u de bedreigingen kunt sorteren, onderzoeken en oplossen.

1. Selecteer in de Azure Portal onder Azure Sentinel **Analytics**.

1. Selecteer in de bovenste menu balk **+ maken** en selecteer **geplande query regel**. Hiermee opent u de **wizard Analytics-regel**.

    :::image type="content" source="media/tutorial-detect-threats-custom/create-scheduled-query.png" alt-text="Geplande query maken":::

1. Geef op het tabblad **Algemeen** een unieke **naam** en **Beschrijving**op. In het veld **tactiek** kunt u kiezen uit verschillende categorieën aanvallen waarmee de regel wordt geclassificeerd. Stel de **Ernst** van de waarschuwing in als dat nodig is. Wanneer u de regel maakt, wordt de **status** standaard **ingeschakeld** , wat betekent dat deze onmiddellijk wordt uitgevoerd nadat u deze hebt gemaakt. Als u niet wilt dat deze onmiddellijk wordt uitgevoerd, selecteert u **uitgeschakeld**. de regel wordt toegevoegd aan het tabblad **actieve regels** en u kunt het gebruiken wanneer u het nodig hebt.

    ![Beginnen met het maken van een aangepaste analyse regel](media/tutorial-detect-threats-custom/general-tab.png)

1. Op het tabblad **regel logica instellen** kunt u een query rechtstreeks in het veld **regel query** schrijven, of de query in log Analytics maken, en deze vervolgens kopiëren en plakken.
 
   ![Query maken in de Azure-Sentinel](media/tutorial-detect-threats-custom/settings-tab.png)

   - Bekijk de **Preview-versie** van de resultaten aan de rechter kant, waarbij Azure Sentinel het aantal resultaten (logboek gebeurtenissen) weergeeft dat door de query wordt gegenereerd, waardoor de vlucht wordt gewijzigd wanneer u uw query schrijft en configureert. De grafiek toont het aantal resultaten gedurende de gedefinieerde tijds periode, die wordt bepaald door de instellingen in de sectie **query planning** .
    - Als u ziet dat uw query te veel of te frequente waarschuwingen zou activeren, kunt u een basis lijn instellen in de sectie **waarschuwings drempel** .

      Hier volgt een voorbeeld query waarmee u wordt gewaarschuwd wanneer een afwijkend aantal resources wordt gemaakt in azure activity.

      ```kusto
      AzureActivity
      | where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
      | where ActivityStatus == "Succeeded"
      | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller
      ```

        > [!NOTE]
        > De lengte van de query moet tussen 1 en 10.000 tekens lang zijn en mag niet ' Search \* ' of ' Union \* ' bevatten.

    1. Gebruik de sectie **entiteiten toewijzen** om de para meters van uw query resultaten te koppelen aan door Sentinel herkende entiteiten van Azure. Deze entiteiten vormen de basis voor verdere analyse, met inbegrip van het groeperen van waarschuwingen in incidenten op het tabblad **incident instellingen** .
  
    1. Stel in de sectie **query planning** de volgende para meters in:

       1. Stel **query uitvoeren** in om te bepalen hoe vaak de query wordt uitgevoerd, net zo vaak als elke 5 minuten of als een keer per dag.

       1. Stel **opzoek gegevens van de laatste** in om de tijds periode te bepalen van de gegevens die worden gedekt door de query. Dit kan bijvoorbeeld de afgelopen 10 minuten aan gegevens of de afgelopen 6 uur aan gegevens opvragen.

          > [!NOTE]
          > **Query-intervallen en lookback periode**
          > - Deze twee instellingen zijn onafhankelijk van elkaar, tot een bepaald punt. U kunt een query uitvoeren met een kort interval voor een tijds periode die langer is dan het interval (als gevolg van overlappende query's), maar u kunt geen query uitvoeren met een interval dat de dekkings periode overschrijdt, anders worden er hiaten in de algehele query dekking.
          >
          > **Opname vertraging**
          > - Voor een **latentie** die kan optreden tussen de generatie van een gebeurtenis bij de bron en de opname in azure Sentinel, en om ervoor te zorgen dat het volledige dekking zonder gegevens duplicatie plaatsvindt, voert Azure Sentinel geplande analyse regels uit op een **vertraging van vijf minuten** van de geplande tijd.

    1. Gebruik de sectie **waarschuwings drempel** om een basis lijn te definiëren. Stel bijvoorbeeld een **waarschuwing genereren wanneer het aantal query resultaten** **groter is dan** en voer het getal 1000 in als u wilt dat de regel alleen een waarschuwing genereert als de query meer dan 1000 resultaten retourneert telkens wanneer deze wordt uitgevoerd. Dit is een verplicht veld. Als u geen basis lijn wilt instellen, dus als u wilt dat uw waarschuwing elke gebeurtenis registreert, voert u 0 in het veld getal in.
    
    1. Kies onder **gebeurtenis groepering**een van de twee manieren om de groepering van **gebeurtenissen** in **waarschuwingen**te verwerken: 

       - **Alle gebeurtenissen groeperen in één waarschuwing** (de standaard instelling). De regel genereert één waarschuwing elke keer dat deze wordt uitgevoerd, mits de query meer resultaten oplevert dan de opgegeven **drempel waarde** voor de waarschuwing. De waarschuwing bevat een samen vatting van alle gebeurtenissen die in de resultaten worden geretourneerd. 

       - **Activeer een waarschuwing voor elke gebeurtenis**. De regel genereert een unieke waarschuwing voor elke gebeurtenis die door de query wordt geretourneerd. Dit is handig als u wilt dat gebeurtenissen afzonderlijk worden weer gegeven of dat u ze wilt groeperen op bepaalde para meters, per gebruiker, hostnaam of iets anders. U kunt deze para meters definiëren in de query.
    
       Momenteel is het aantal waarschuwingen dat een regel kan genereren, op 20. Als in een bepaalde regel **gebeurtenis groepering** is ingesteld om **een waarschuwing voor elke gebeurtenis te activeren**en de query van de regel meer dan 20 gebeurtenissen retourneert, wordt door elk van de eerste 19 gebeurtenissen een unieke waarschuwing gegenereerd. in de twintigste waarschuwing wordt de hele set geretourneerde gebeurtenissen samenvatten. Met andere woorden, de twintigste waarschuwing is wat zou zijn gegenereerd onder de **groep alle gebeurtenissen in één waarschuwings** optie.

       > [!NOTE]
       > Wat is het verschil tussen **gebeurtenissen** en **waarschuwingen**?
       >
       > - Een **gebeurtenis** is een beschrijving van één voorval. Zo kan één vermelding in een logboek bestand tellen als een gebeurtenis. In deze context verwijst een gebeurtenis naar een enkel resultaat dat wordt geretourneerd door een query in een Analytics-regel.
       >
       > - Een **waarschuwing** is een verzameling gebeurtenissen die samen worden genomen en belang rijk zijn in het oogpunt van beveiliging. Een waarschuwing kan één gebeurtenis bevatten als de gebeurtenis aanzienlijke beveiligings implicaties heeft: een administratieve aanmelding vanuit een buitenlands land buiten kantoor uren, bijvoorbeeld.
       >
       > - Op die manier zijn er **incidenten**? De interne logica van Azure Sentinel maakt **incidenten** van **waarschuwingen** of groepen waarschuwingen. De wachtrij incidenten is het brandpuntsafstand van het sorteren, onderzoek en herstel van analisten.
       > 
       > Azure Sentinel neemt onbewerkte gebeurtenissen van sommige gegevens bronnen en de al verwerkte waarschuwingen van anderen. Het is belang rijk te weten welk abonnement u op elk moment gebruikt.

       > [!IMPORTANT]
       > Gebeurtenis groepering is momenteel beschikbaar als open bare preview. Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
    
    1. In het gedeelte **onderdrukking** kunt u de instelling voor het **uitvoeren van de query stoppen nadat de waarschuwing is gegenereerd** inschakelen **op** als u een waarschuwing hebt ontvangen, wilt u de bewerking van deze regel voor een bepaalde periode voor het overschrijden van het query-interval onderbreken. Als u dit inschakelt, moet u de uitvoering van de **query stoppen** instellen op de tijd die nodig is voor het stoppen van de query, tot 24 uur.

1. Op het tabblad **instellingen voor incidenten** kunt u kiezen of en hoe Azure Sentinel waarschuwingen in incidenten kan omzetten. Als dit tabblad alleen wordt weer gegeven, maakt Azure Sentinel een afzonderlijke, afzonderlijk incident van elke waarschuwing. U kunt ervoor kiezen om geen incidenten te maken of om verschillende waarschuwingen in één incident te groeperen, door de instellingen op dit tabblad te wijzigen.

   > [!IMPORTANT]
   > Het tabblad incident instellingen bevindt zich momenteel in de open bare preview. Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
    
    1. In de sectie **incident instellingen** worden **incidenten gemaakt op basis van waarschuwingen die door deze analyse regel worden geactiveerd** , standaard ingesteld op **ingeschakeld**, wat betekent dat Azure Sentinel één afzonderlijk incident maakt van elke waarschuwing die wordt geactiveerd door de regel.
       - Als u niet wilt dat deze regel resulteert in het maken van incidenten (als deze regel bijvoorbeeld alleen gegevens voor de volgende analyse verzamelt), stelt u deze in op **uitgeschakeld**.

    1. Als u **Alert grouping** een enkel incident wilt genereren op basis van een groep van maxi maal 150 vergelijk bare of terugkerende waarschuwingen (zie opmerking), stelt u **gerelateerde waarschuwingen in, die door deze analyse regel worden geactiveerd, naar incidenten** op **ingeschakeld**en stelt u de volgende para meters in.

    - **De groep beperken tot waarschuwingen die zijn gemaakt in het geselecteerde tijds bestek**: Bepaal het tijds bestek waarbinnen de vergelijk bare of terugkerende waarschuwingen samen worden gegroepeerd. Alle bijbehorende waarschuwingen binnen deze tijds periode genereren gezamenlijk een incident of een reeks incidenten (afhankelijk van de groeperings instellingen hieronder). Waarschuwingen buiten deze tijds periode genereren een afzonderlijk incident of een reeks incidenten.

    - **Groeps waarschuwingen die door deze analyse regel worden geactiveerd in één incident per**: Kies de basis waarop waarschuwingen worden gegroepeerd:

        - **Groepeer waarschuwingen in één incident als alle entiteiten overeenkomen**: waarschuwingen worden gegroepeerd als ze identieke waarden delen voor elk van de toegewezen entiteiten (gedefinieerd op het tabblad set-regel logica hierboven). Dit is de aanbevolen instelling.

        - **Alle waarschuwingen groeperen die door deze regel in één incident worden geactiveerd**: alle waarschuwingen die door deze regel worden gegenereerd, worden gegroepeerd, zelfs als ze geen identieke waarden delen.

        - **Groepeer waarschuwingen in één incident als de geselecteerde entiteiten overeenkomen**: waarschuwingen worden gegroepeerd als ze identieke waarden delen voor een aantal toegewezen entiteiten (die u kunt selecteren in de vervolg keuzelijst). U kunt deze instelling gebruiken als u bijvoorbeeld afzonderlijke incidenten wilt maken op basis van de bron-of doel-IP-adressen.

    - **Gesloten afsluitende incidenten opnieuw openen**: als een incident is opgelost en gesloten, en later een andere waarschuwing wordt gegenereerd die tot dat incident moet behoren, stelt u deze instelling in op **ingeschakeld** als u wilt dat het gesloten incident opnieuw wordt geopend en schakelt u deze optie **uit als u** wilt dat de waarschuwing een nieuw incident maakt.
    
        > [!NOTE]
        > Maxi maal 150 waarschuwingen kunnen in één incident worden gegroepeerd. Als er meer dan 150 waarschuwingen worden gegenereerd door een regel die deze in één incident groepeert, wordt er een nieuw incident gegenereerd met dezelfde incident Details als het origineel en worden de overmatige waarschuwingen gegroepeerd in het nieuwe incident.

1. Op het tabblad **geautomatiseerde antwoorden** selecteert u de playbooks die u automatisch wilt uitvoeren wanneer een waarschuwing wordt gegenereerd door de aangepaste regel. Zie [reageren op bedreigingen](tutorial-respond-threats-playbook.md)voor meer informatie over het maken en automatiseren van playbooks.

1. Selecteer **controleren en maken** om alle instellingen voor de nieuwe waarschuwings regel te controleren en selecteer vervolgens **maken om uw waarschuwings regel te initialiseren**.
  
1. Nadat de waarschuwing is gemaakt, wordt er een aangepaste regel toegevoegd aan de tabel onder **actieve regels**. In deze lijst kunt u elke regel inschakelen, uitschakelen of verwijderen.

1. Als u de resultaten van de waarschuwings regels die u maakt, wilt weer geven, gaat u naar de pagina **incidenten** , waar u de [incidenten](tutorial-investigate-cases.md)kunt sorteren, onderzoeken en de bedreigingen herstelt.


> [!NOTE]
> Waarschuwingen die zijn gegenereerd in azure Sentinel zijn beschikbaar via [Microsoft Graph beveiliging](https://aka.ms/securitygraphdocs). Zie de [documentatie over Microsoft Graph Security Alerts](https://aka.ms/graphsecurityreferencebetadocs)(Engelstalig) voor meer informatie.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="a-scheduled-rule-failed-to-execute-or-appears-with-auto-disabled-added-to-the-name"></a>Een geplande regel kan niet worden uitgevoerd, of wordt weer gegeven wanneer automatisch uitgeschakeld is toegevoegd aan de naam

Het is een zeldzame voorval dat een geplande query regel niet kan worden uitgevoerd, maar dit kan gebeuren. Azure Sentinel classificeert fouten vooraf als tijdelijk of permanent, op basis van het specifieke type fout en de omstandigheden die ernaar hebben geleid.

#### <a name="transient-failure"></a>Tijdelijke fout

Een tijdelijke fout treedt op als gevolg van een tijdelijke situatie en gaat binnenkort terug naar normaal, waarbij de uitvoering van de regel slaagt. Enkele voor beelden van fouten die door Azure Sentinel worden geclassificeerd als tijdelijk:

- Het duurt te lang voordat een regel query wordt uitgevoerd en er een time-out optreedt.
- Verbindings problemen tussen gegevens bronnen en Log Analytics, of tussen Log Analytics en Azure Sentinel.
- Andere nieuwe en onbekende fouten worden beschouwd als tijdelijk.

In het geval van een tijdelijke storing wordt de regel door Azure Sentinel continu opnieuw proberen uit te voeren na vooraf vastgestelde en steeds toenemende intervallen, tot een bepaald punt. Daarna wordt de regel opnieuw uitgevoerd op het volgende geplande tijdstip. Een regel wordt nooit automatisch uitgeschakeld vanwege een tijdelijke fout.

#### <a name="permanent-failure---rule-auto-disabled"></a>Permanente fout-automatische uitgeschakelde regel

Een permanente fout treedt op als gevolg van een wijziging in de voor waarden waardoor de regel kan worden uitgevoerd, zonder dat er sprake is van een menselijke interventie. Hier volgen enkele voor beelden van fouten die zijn geclassificeerd als permanent:

- De doel werkruimte (waarop de regel query wordt uitgevoerd) is verwijderd.
- De doel tabel (waarop de regel query wordt uitgevoerd) is verwijderd.
- De Azure-Sentinel is verwijderd uit de doel werkruimte.
- Een functie die door de regel query wordt gebruikt, is niet meer geldig. de service is gewijzigd of verwijderd.
- De machtigingen voor een van de gegevens bronnen van de regel query zijn gewijzigd.
- Een van de gegevens bronnen van de regel query is verwijderd of de verbinding is verbroken.

**In het geval van een vooraf bepaald aantal opeenvolgende permanente fouten, van hetzelfde type en van dezelfde regel,** Azure Sentinel stopt met het uitvoeren van de regel en neemt ook de volgende stappen:

- Hiermee schakelt u de regel uit.
- Voegt de woorden **' automatisch uitgeschakeld '** toe aan het begin van de naam van de regel.
- Hiermee wordt de reden voor de fout (en het uitschakelen) toegevoegd aan de beschrijving van de regel.

U kunt eenvoudig de aanwezigheid van automatische uitgeschakelde regels bepalen door de regel lijst te sorteren op naam. De regels voor automatisch uitschakelen bevinden zich boven aan de lijst.

SOC managers moeten de regel lijst regel matig controleren op de aanwezigheid van automatische uitgeschakelde regels.

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u geleerd hoe u aan de slag gaat met het detecteren van bedreigingen met behulp van Azure Sentinel.

Als u wilt weten hoe u uw reacties op bedreigingen kunt automatiseren, [stelt u automatische reacties op dreigingen in azure Sentinel in](tutorial-respond-threats-playbook.md).

