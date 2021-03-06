---
title: Rapporten genereren
description: Krijg inzicht in netwerk activiteiten, Risico's, aanvallen en trends door gebruik te maken van Defender voor IoT-rapportage hulpprogramma's.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/17/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: d0c3f531ede0a590a6ba7bb21c6edbd768c08069
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2020
ms.locfileid: "97840346"
---
# <a name="generate-reports"></a>Rapporten genereren

Krijg inzicht in netwerk activiteiten, Risico's, aanvallen en trends met behulp van Azure Defender voor IoT-rapportage hulpprogramma's. Er zijn hulpprogram ma's beschikbaar om rapporten te genereren:

- Op basis van de activiteit die door afzonderlijke Sens oren is gedetecteerd.
- Op basis van de activiteit die door alle Sens oren is gedetecteerd. 

Deze rapporten worden gegenereerd vanuit de on-premises beheer console.

## <a name="reports-for-sensor-risk-assessment"></a>Rapporten voor sensor risico analyse

Met rapportage voor risico analyse kunt u een beveiligings Score genereren voor elk netwerk apparaat, evenals een algemene netwerk beveiligings Score. De totale score geeft het percentage van 100 procent beveiliging.

Deze score is gebaseerd op de resultaten van pakket inspectie, gedrags modellerings engines en een SCADA voor de status van een bepaalde computer. Er is een uitgebreid scala aan andere informatie beschikbaar, zoals:

- Configuratie problemen

- Beveiligingslek met betrekking tot het apparaat door beveiligings niveau

- Problemen met netwerk beveiliging

- Operationele netwerk problemen

- Aanvals vectoren

- Verbindingen met ICS-netwerken

- Internet verbindingen

- Industriële malware-indica toren

- Protocolproblemen

  - Beveiligde apparaten: apparaten met een beveiligings Score van meer dan 90%.

- **Beveiligde apparaten**: apparaten met een beveiligings Score van meer dan 90 procent.

- **Kwets bare apparaten**: apparaten met een beveiligings Score van minder dan 70 procent.

- **Apparaten die moeten worden verbeterd**: apparaten met een beveiligings score tussen 70 procent en 89 procent.

Een rapport maken:

1. Selecteer **risico analyse** in het menu aan de zijkant.
2. Selecteer een sensor in de vervolg keuzelijst **sensor selecteren** .
3. Selecteer **rapport genereren**.
4. Selecteer **downloaden** in het gedeelte gearchiveerde rapporten.

:::image type="content" source="media/how-to-generate-reports/risk-assessment.png" alt-text="Een weer gave van de risico analyse.":::

Een bedrijfs logo importeren:

Een bedrijfs logo importeren:

- Selecteer **logo importeren**.

## <a name="reports-for-sensor-attack-vector"></a>Rapporten voor sensor aanvals vector

Aanvals vector rapporten bieden een grafische weer gave van de beveiligings keten van apparaten die kunnen worden misbruikt. Deze beveiligings problemen kunnen een aanvaller toegang geven tot belang rijke netwerk apparaten. De aanvals vector Simulator berekent aanvals vectoren in realtime en analyseert alle aanvals vectoren voor een specifiek doel.

Als u met de aanvals vector werkt, kunt u het effect van de oplossings activiteiten in de aanvals volgorde evalueren. U kunt vervolgens bepalen wanneer een systeem upgrade het pad van de aanvaller verstoort door de aanvals keten te verbreken of als een alternatief pad naar een aanval blijft. Deze informatie helpt u bij het bepalen van de prioriteit van herstel-en oplossings activiteiten.

:::image type="content" source="media/how-to-generate-reports/control-center.png" alt-text="Bekijk uw waarschuwingen in het beheer centrum.":::

> [!NOTE]
> Beheerders en beveiligings analisten kunnen de procedures uitvoeren die in deze sectie worden beschreven.

Een aanvals vector simulatie maken:

1. Selecteer :::image type="content" source="media/how-to-generate-reports/plus.png" alt-text="plus teken":::in het menu aan de linkerkant om een simulatie toe te voegen.

 :::image type="content" source="media/how-to-generate-reports/vector.png" alt-text="De simulatie van de aanvals vector.":::

2. Simulatie-eigenschappen invoeren:

   - **Naam**: naam van simulatie.

   - **Maximale vectoren**: het maximum aantal vectoren in één simulatie.

   - **Weer geven in apparaattoewijzing**: de aanvals vector weer geven als een filter op de apparaattoewijzing.

   - **Alle bron apparaten**: met de aanvals vector worden alle apparaten als een aanvals bron beschouwd.

   - **Aanvals bron**: bij de aanvals vector worden alleen de opgegeven apparaten als een aanvals bron beschouwd.

   - **Alle doel apparaten**: met de aanvals vector worden alle apparaten als een aanvals doel beschouwd.

   - **Aanvals doel**: met de aanvals vector worden alleen de opgegeven apparaten als een aanvals doel beschouwd.

   - **Apparaten uitsluiten**: opgegeven apparaten worden uitgesloten van de simulatie van de aanvals vector.

   - **Subnetten uitsluiten**: opgegeven subnetten worden uitgesloten van de simulatie van de aanvals vector.

3. Selecteer **simulatie toevoegen**. De simulatie wordt toegevoegd aan de lijst simulaties.

   :::image type="content" source="media/how-to-generate-reports/new-simulation.png" alt-text="Voeg een nieuwe simulatie toe.":::

4. Selecteer :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: deze optie als u de simulatie wilt bewerken.

   Selecteer :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: deze optie als u de simulatie wilt verwijderen.

   Selecteer :::image type="icon" source="media/how-to-generate-reports/make-a-favorite-icon.png" border="false"::: deze optie als u de simulatie wilt markeren als een favoriet.

5. Er wordt een lijst met aanvals vectoren weer gegeven en deze bevat een vector Score (van 100), een aanvals bron apparaat en een aanvals doel apparaat. Selecteer een specifieke aanval voor grafische weer gave van aanvals vectoren.

   :::image type="content" source="media/how-to-generate-reports/sample-attack-vectors.png" alt-text="Aanvals vectoren.":::

## <a name="sensor-data-mining-queries"></a>Sens data-analyse query's

Met hulpprogram ma's voor gegevens analyse kunt u op verschillende lagen uitgebreide en gedetailleerde informatie over uw netwerk apparaten genereren. U kunt bijvoorbeeld een query maken op basis van:

- Tijds perioden

- Verbindingen met Internet

- Poorten

- Protocollen

- Firmware versies

- Programmeer opdrachten

- Inactiviteit van het apparaat

U kunt het rapport nauw keurig aanpassen op basis van filters. U kunt bijvoorbeeld een query uitvoeren op een specifiek subnet waarin de firmware is bijgewerkt.

:::image type="content" source="media/how-to-generate-reports/active-device-list-v2.png" alt-text="Lijst met actieve apparaten.":::

Er zijn verschillende hulpprogram ma's beschikbaar voor het beheren van query's. U kunt bijvoorbeeld exporteren en opslaan.

> [!NOTE]
> Beheerders en beveiligings analisten hebben toegang tot opties voor gegevens analyse.

### <a name="dynamic-updates"></a>Dynamische updates

Query's voor gegevens analyse die u maakt, worden dynamisch bijgewerkt telkens wanneer u ze opent. Bijvoorbeeld:

- Als u een rapport voor firmware versies op apparaten op 1 juni maakt en het rapport opnieuw op 10 juni opent, wordt dit rapport bijgewerkt met informatie die nauw keurig is voor 10 juni.

- Als u een rapport maakt om de nieuwe apparaten te detecteren die in de afgelopen 30 dagen zijn gedetecteerd op 1 juni en het rapport op 30 juni opent, worden de resultaten weer gegeven voor de afgelopen 30 dagen.

### <a name="data-mining-use-cases"></a>Gebruiks voorbeelden van gegevens analyse

U kunt query's gebruiken om een uitgebreide reeks beveiligings behoeften voor verschillende beveiligings teams te verwerken:

- **Soc-incident respons**: een rapport in realtime genereren om te helpen bij het direct reageren op incidenten. Genereer bijvoorbeeld een rapport voor een lijst met apparaten waarvoor patches nodig zijn.

- **Forensische**: Genereer een rapport op basis van historische gegevens voor onderzoek rapporten.

- **IT-netwerk integriteit**: Genereer een rapport waarmee u de algehele netwerk beveiliging kunt verbeteren. Genereer bijvoorbeeld een rapport met een lijst met apparaten met zwakke verificatie referenties.

- **Zicht baarheid**: een rapport genereren met alle query-items om alle basislijn parameters van uw netwerk weer te geven.

### <a name="data-mining-storage"></a>Opslag voor gegevens analyse

Gegevens analyse gegevens worden doorlopend opgeslagen en opgeslagen, behalve wanneer een apparaat wordt verwijderd. Resultaten van gegevens analyse kunnen extern worden geëxporteerd en opgeslagen op een beveiligde server. Daarnaast voert de sensor automatische dagelijkse back-ups uit om te zorgen voor systeem continuïteit en behoud van gegevens.

### <a name="data-mining-and-reports"></a>Gegevens analyse en-rapporten 

Reguliere rapporten, die toegankelijk zijn via de optie **rapporten** , zijn vooraf gedefinieerde rapporten voor gegevens analyse. Ze zijn geen dynamische query's die beschikbaar zijn in gegevens analyse, maar een statische representatie van de query resultaten van de gegevens analyse.

Er zijn geen gegevens analyse query resultaten beschikbaar voor RO-gebruikers. Beheerders en beveiligings analisten die toegang willen hebben tot de gegevens die worden gegenereerd door gegevens analyse query's, moeten de gegevens opslaan als rapport.

### <a name="predefined-queries"></a>Vooraf gedefinieerde query's

De volgende vooraf gedefinieerde query's zijn beschikbaar. Deze query's worden in realtime gegenereerd.

- **CVEs**: een lijst met apparaten die in de afgelopen 24 uur zijn gedetecteerd met bekende beveiligings problemen.

- **Uitgesloten CVEs**: een lijst met alle CVEs die hand matig zijn uitgesloten. Om nauw keurige resultaten te krijgen in VA-rapporten en aanvals vectoren, kunt u de CVE-lijst hand matig aanpassen door CVEs op te nemen en uit te sluiten.

- **Internet activiteit**: apparaten die zijn verbonden met internet.

- Niet- **actieve apparaten**: apparaten die gedurende de afgelopen zeven dagen niet zijn gecommuniceerd.

- **Actieve apparaten**: actieve netwerk apparaten in de afgelopen 24 uur.

- **Externe toegang**: apparaten die communiceren via externe sessie protocollen.

- **Programmeer opdrachten**: apparaten die industriële programmering verzenden.

Deze rapporten zijn automatisch toegankelijk vanuit het scherm **rapporten** , waarin ro-gebruikers en andere gebruikers ze kunnen bekijken. RO-gebruikers hebben geen toegang tot rapporten van gegevens analyse.

:::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Het scherm voor gegevens analyse.":::

### <a name="create-a-data-mining-report"></a>Een gegevens analyse rapport maken

Een rapport voor gegevens analyse maken:

1. Selecteer **gegevens analyse** in het menu aan de zijkant. Vooraf gedefinieerde voorgestelde rapporten worden automatisch weer gegeven.

 :::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Selecteer gegevens analyse vanuit zijvenster.":::

2. Selecteer :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false":::.

3. Selecteer **Nieuw rapport** en definieer het rapport.

   :::image type="content" source="media/how-to-generate-reports/create-new-report-screen.png" alt-text="Maak een nieuw rapport door dit scherm in te vullen.":::

   De volgende para meters zijn beschikbaar:

   - Geef een naam en beschrijving voor het rapport op.

   - Selecteer een van de volgende categorieën:

      - **Categorieën (alle)** voor het weer geven van alle rapport resultaten over alle apparaten in uw netwerk.

      - **Algemeen** om standaard categorieën te kiezen.

   - Selecteer specifieke rapport parameters die voor u van belang zijn.

   - Kies een sorteer volgorde (sorteren **op**). Resultaten sorteren op basis van activiteit of categorie.

   - Selecteer **opslaan naar rapport pagina's** om de rapport resultaten op te slaan als een rapport dat toegankelijk is vanaf de **rapport** pagina. Hiermee kunnen RO-gebruikers het door u gemaakte rapport uitvoeren.

   - Selecteer **filters (toevoegen)** om meer filters toe te voegen. Joker aanvragen worden ondersteund.

   - Geef een apparaatgroep op (gedefinieerd in de apparaattoewijzing).

   - Geef een IP-adres op.

   - Geef een poort op.

   - Geef een MAC-adres op.

4. Selecteer **Opslaan**. Rapport resultaten open op de pagina voor **gegevens analyse** .

:::image type="content" source="media/how-to-generate-reports/data-mining-page.png" alt-text="Rapport resultaten zoals weer gegeven op de pagina voor gegevens analyse.":::

### <a name="manage-data-mining-reports"></a>Rapporten over gegevens analyse beheren

In de volgende tabel worden de beheer opties voor gegevens analyse beschreven:

| Pictogram afbeelding | Beschrijving |
|--|--|
| :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: | Bewerk de rapport parameters. |
| :::image type="icon" source="media/how-to-generate-reports/export-as-pdf-icon.png" border="false"::: | Exporteren als PDF. |
| :::image type="icon" source="media/how-to-generate-reports/csv-export-icon.png" border="false"::: |Exporteren als CSV-bestand. |
| :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: | Aanvullende informatie weer geven, zoals de datum die het laatst is gewijzigd. Gebruik deze functie om een moment opname van een query resultaat te maken. Mogelijk moet u dit doen voor verder onderzoek met team leiders of SOC analisten. |
| :::image type="icon" source="media/how-to-generate-reports/pin-icon.png" border="false"::: | Weer geven op de pagina **rapporten** of op de pagina **rapporten** verbergen. :::image type="content" source="media/how-to-generate-reports/hide-reports-page.png" alt-text="Uw rapporten verbergen of weer geven."::: |
| :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: | U kunt het rapport dan verwijderen. |

#### <a name="create-customized-directories"></a>Aangepaste mappen maken 

U kunt de uitgebreide informatie voor gegevens analyse query's ordenen door mappen voor categorieën te maken. U kunt bijvoorbeeld mappen maken voor protocollen of locaties.

Een nieuwe map maken:

1. Selecteer deze optie :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: om een nieuwe map toe te voegen.

2. Selecteer **nieuwe map** om het nieuwe Active Directory-formulier weer te geven.

3. Geef de nieuwe map een naam.

4. Sleep de vereiste rapporten naar de nieuwe map. U kunt het rapport op elk gewenst moment naar de hoofd weergave verplaatsen.

5. Klik met de rechter muisknop op de nieuwe map om deze te openen, te bewerken of te verwijderen.

#### <a name="create-snapshots-of-report-results"></a>Moment opnamen van rapport resultaten maken

Mogelijk moet u bepaalde query resultaten opslaan voor verder onderzoek. U moet bijvoorbeeld mogelijk resultaten delen met verschillende beveiligings teams.

Moment opnamen worden opgeslagen in de rapport resultaten en genereren geen dynamische query's.

:::image type="content" source="media/how-to-generate-reports/report-results-report.png" alt-text="Moment opnamen.":::

Een moment opname maken:

1. Open het vereiste rapport.

2. Selecteer het informatie pictogram :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: .

3. Selecteer **nieuwe maken**.

4. Voer een naam in voor de moment opname en selecteer **Opslaan**.

:::image type="content" source="media/how-to-generate-reports/take-a-snapshot.png" alt-text="Maak een moment opname.":::

#### <a name="customize-the-cve-list"></a>De CVE-lijst aanpassen

U kunt de CVE-lijst als volgt hand matig aanpassen:

  - CVEs opnemen/uitsluiten

  - De CVE-Score wijzigen

Hand matige wijzigingen uitvoeren in het CVE-rapport:

1.  Selecteer in het menu aan de zijkant **gegevens analyse**.

2. Selecteer :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: in de linkerbovenhoek van het venster voor **gegevens analyse** . Selecteer vervolgens **Nieuw rapport**.

   :::image type="content" source="media/how-to-generate-reports/create-a-new-report-screen.png" alt-text="Een nieuw rapport maken.":::

3. Selecteer een van de volgende opties in het linkerdeel venster:

   - **Bekende beveiligings problemen**: selecteert beide opties en geeft de resultaten weer in de twee tabellen van het rapport, een met CVEs en de andere met uitgesloten CVEs.

   - **CVEs**: Selecteer deze optie om een lijst met alle CVEs te presen teren.

   - **Uitgesloten CVEs**: Selecteer deze optie om een lijst weer te geven met alle uitgesloten CVEs.

4. Vul de gegevens **naam** en **Beschrijving** in en selecteer **Opslaan**. Het nieuwe rapport wordt weer gegeven in het venster voor **gegevens analyse** .

5. Als u CVEs wilt uitsluiten, opent u het rapport met gegevens analyse voor CVEs. De lijst met alle CVEs wordt weer gegeven.

   :::image type="content" source="media/how-to-generate-reports/cves.png" alt-text="C V-rapport.":::

6. Als u de selectie van items in de lijst wilt inschakelen, selecteert :::image type="icon" source="media/how-to-generate-reports/enable-selecting-icon.png" border="false"::: u de CVEs die u wilt aanpassen en selecteert u deze. De **werk** balk aan de onderkant wordt weer gegeven.

   :::image type="content" source="media/how-to-generate-reports/operations-bar-appears.png" alt-text="Scherm afbeelding van de werk balk van de gegevens analyse.":::

7. Selecteer de CVEs die u wilt uitsluiten en selecteer vervolgens **records verwijderen**. De CVEs die u hebt geselecteerd, worden niet weer gegeven in de lijst met CVEs en worden weer gegeven in de lijst met uitgesloten CVEs wanneer u er een genereert.

8. Als u de uitgesloten CVEs wilt opnemen in de lijst met CVEs, genereert u het rapport voor uitgesloten CVEs en verwijdert u de items die u wilt opnemen in de lijst met CVEs.

9. Selecteer de CVEs waarin u de score wilt wijzigen en selecteer vervolgens **CVE-score bijwerken**.

   :::image type="content" source="media/how-to-generate-reports/set-new-score-screen.png" alt-text="Werk de CVE-Score bij.":::

10. Voer de nieuwe score in en selecteer **OK**. De bijgewerkte score wordt weer gegeven in de CVEs die u hebt geselecteerd.

### <a name="reports-based-on-data-mining"></a>Rapporten op basis van gegevens analyse

Rapporten geven informatie weer die is gegenereerd door query resultaten van gegevens analyse. Dit omvat standaard rapporten voor gegevens analyse, die beschikbaar zijn in de weer gave rapporten. Beheerders-en beveiligings analisten kunnen ook aangepaste query's voor gegevens analyse genereren en deze opslaan als rapporten. Deze rapporten zijn ook beschikbaar voor RO-gebruikers.

Een rapport genereren:

1. Selecteer **rapporten** in het menu aan de zijkant.

2. Kies het vereiste rapport om weer te geven. De keuze kan **aangepaste** of **automatisch gegenereerde** rapporten zijn, zoals het **Program meren van opdrachten** en **externe toegang**.

3. U kunt het rapport exporteren door een van de pictogrammen in de rechter bovenhoek van het scherm te selecteren:

   :::image type="icon" source="media/how-to-generate-reports/export-to-pdf-icon.png" border="false"::: Exporteren naar een PDF-bestand.

   :::image type="icon" source="media/how-to-generate-reports/export-to-csv-icon.png" border="false"::: Exporteren naar een CSV-bestand.

> [!NOTE] 
> De RO-gebruiker kan alleen rapporten zien die voor hen zijn gemaakt.

:::image type="content" source="media/how-to-generate-reports/select-a-report-screen.png" alt-text="Selecteer het rapport dat u wilt genereren.":::

:::image type="content" source="media/how-to-generate-reports/remote-access-report.png" alt-text="Het rapport voor externe toegang is gegenereerd.":::

## <a name="reports-for-sensor-trends-and-statistics"></a>Rapporten voor sensor trends en statistieken

U kunt widget grafieken en cirkel diagrammen maken om inzicht te krijgen in netwerk trends en-statistieken. Widgets kunnen worden gegroepeerd onder door de gebruiker gedefinieerde Dash boards.

> [!NOTE]
> Alleen beheerders en beveiligings analisten kunnen de procedures in deze sectie uitvoeren.

Als u Dash boards en widgets wilt weer geven, selecteert u **Trends & statistieken** in het menu aan de zijkant.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Scherm opname van een onderzoek.":::

Het dash board bestaat uit widgets die de volgende typen informatie grafisch beschrijven:

- Verkeer per poort
- Kanaal bandbreedte
- Totale band breedte
- Actieve TCP-verbinding
- Apparaten:
  - Nieuwe apparaten
  - Apparaten bezet
  - Apparaten op leverancier
  - Apparaten op besturings systeem
  - Niet-verbonden apparaten
- Connectiviteits mogelijkheden per uur
- Waarschuwingen voor incidenten per type
- Database tabel toegang
- Widgets van protocol sectie
- Ethernet-en IP-adres:
  - Ethernet-en IP-adres verkeer door overschrijvings service
  - Ethernet-en IP-adres verkeer door overschrijvings klasse
  - Ethernet-en IP-adres verkeer via opdracht
- OPC
  - OPC best beheer bewerkingen
  - OPC, bovenste I/O-bewerkingen
- Siemens S7:
  - S7 verkeer per besturings functie
  - S7 verkeer per subfunctie
- SRTP:
  - SRTP verkeer per service code
  - SRTP-fouten per dag
- SuiteLink:
  - SuiteLink populairste labels
  - Gedrag van SuiteLink numerieke code
- IEC-60870-verkeer door ASDU
- DNP3-verkeer per functie
- MMS-verkeer per service
- Modbus-verkeer per functie
- OPC-UA-verkeer per service

> [!NOTE]
>  De tijd in de widgets wordt ingesteld op basis van de sensor tijd.

## <a name="reports-for-the-on-premises-management-console"></a>Rapporten voor de on-premises beheer console

De on-premises beheer console biedt u de mogelijkheid om rapporten te genereren voor elke sensor die er verbinding mee maakt. Rapporten zijn gebaseerd op Sens data-analyse query's die worden uitgevoerd.

U kunt de volgende rapporten genereren:

- **Actieve apparaten (afgelopen 24 uur)**: geeft een lijst met apparaten die binnen een periode van 24 uur netwerk activiteiten weer geven.

- **Niet-actieve apparaten (afgelopen 7 dagen)**: geeft een lijst van apparaten weer die de afgelopen zeven dagen geen netwerk activiteit tonen.

- **Programmeer opdrachten**: geeft een lijst met apparaten weer die in de afgelopen 24 uur programmeer opdrachten hebben verzonden.

- **Externe toegang**: geeft een lijst met apparaten weer die in de afgelopen 24 uur toegang hebben tot externe bronnen.

:::image type="content" source="media/how-to-generate-reports/reports-view.png" alt-text="Scherm opname van de weer gave rapporten.":::

Wanneer u de sensor kiest in de on-premises beheer console, worden alle aangepaste rapporten die op die sensor zijn geconfigureerd, weer gegeven in de lijst met rapporten. Voor elke sensor kunt u een standaard rapport of een aangepast rapport genereren dat op die sensor is geconfigureerd.

Een rapport genereren:

1. Selecteer **rapporten** in het linkerdeel venster. Het venster **rapporten** wordt weer gegeven.

2. Selecteer in de vervolg keuzelijst **Sens oren** de sensor waarvoor u het rapport wilt genereren.

   :::image type="content" source="media/how-to-generate-reports/sensor-drop-down-list.png" alt-text="Scherm opname van de weer gave Sens oren.":::

3. Selecteer in de vervolg keuzelijst met de juiste lijst het rapport dat u wilt genereren.

4. Als u een PDF-bestand van de rapport resultaten wilt maken, selecteert u :::image type="icon" source="media/how-to-generate-reports/pdf-report-icon.png" border="false"::: .

## <a name="risk-assessment-reports-for-the-on-premises-management-console"></a>Rapporten over risico analyse voor de on-premises beheer console

Genereer een rapport voor risico analyse voor elke sensor die is verbonden met uw on-premises beheer console. Dit rapport geeft inzicht in elk van de netwerken die uw Sens oren bewaken.

Met rapporten voor risico analyse kunt u een beveiligings Score genereren voor elk netwerk apparaat, evenals een algemene netwerk beveiligings Score. De totale score is gebaseerd op uitgebreide pakket inspecties, gedrags modellerings engines en een SCADA voor de status van een bepaalde machine. Er is een uitgebreid scala aan andere informatie beschikbaar. Bijvoorbeeld:

- Configuratie problemen

- Beveiligingslek met betrekking tot het apparaat door beveiligings niveau

- Problemen met netwerk beveiliging

- Operationele netwerk problemen

- Aanvals vectoren

- Verbindingen met ICS-netwerken

- Internet verbindingen

- Industriële malware-indica toren

- Protocolproblemen

Het rapport bevat aanbevelingen voor het beperken van problemen waarmee u uw beveiligings Score kunt verbeteren.

- Beveiligde apparaten: apparaten met een beveiligings Score van meer dan 90%.

- **Kwets bare apparaten**: apparaten met een beveiligings Score van minder dan 70 procent.

- **Apparaten die moeten worden verbeterd**: apparaten met een beveiligings score tussen 70 procent en 89 procent.

Een rapport maken:

1. Selecteer **risico analyse** in het menu aan de zijkant.

2. Selecteer een sensor in de vervolg keuzelijst **sensor selecteren** .

3. Selecteer **rapport genereren**.

4. Selecteer **downloaden** in het gedeelte **Gearchiveerde rapporten** .

Een bedrijfs logo importeren:

- Selecteer **logo importeren**.

:::image type="content" source="media/how-to-generate-reports/import-logo-screenshot.png" alt-text="Importeer uw logo via de weer gave risico analyse.":::

## <a name="see-also"></a>Zie tevens
[Werken met site kaart weergaven](how-to-gain-insight-into-global-regional-and-local-threats.md#work-with-site-map-views)
