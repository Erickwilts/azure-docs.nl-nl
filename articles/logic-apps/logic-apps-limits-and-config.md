---
title: Limieten en configuratie-Azure Logic Apps | Microsoft Docs
description: Service limieten en configuratie waarden voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 07/19/2019
ms.openlocfilehash: b649ea1c69f82840a663cf32391e4c74ac6cc33e
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68319513"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limieten en configuratie-informatie voor Azure Logic Apps

In dit artikel worden de limieten en configuratie gegevens voor het maken en uitvoeren van geautomatiseerde werk stromen met Azure Logic Apps beschreven. Zie [limieten en configuratie in Microsoft flow](https://docs.microsoft.com/flow/limits-and-config)voor Microsoft flow.

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Definitie limieten

Dit zijn de limieten voor een definitie van een enkele logische app:

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Acties per werk stroom | 500 | Als u deze limiet wilt uitbreiden, kunt u indien nodig geneste werk stromen toevoegen. |
| Toegestane nest diepte voor acties | 8 | Als u deze limiet wilt uitbreiden, kunt u indien nodig geneste werk stromen toevoegen. |
| Werk stromen per regio per abonnement | 1000 | |
| Triggers per werk stroom | 10 | Wanneer u werkt in de code weergave, niet de ontwerper |
| Limiet voor switch Scope cases | 25 | |
| Variabelen per werk stroom | 250 | |
| Tekens per expressie | 8\.192 | |
| Maximale grootte voor`trackedProperties` | 16.000 tekens |
| Naam voor `action` of`trigger` | 80 tekens | |
| Lengte van`description` | 256 tekens | |
| Gehalte`parameters` | 50 | |
| Gehalte`outputs` | 10 | |
||||

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Uitvoerings limieten voor de duur en de Bewaar periode

Dit zijn de limieten voor het uitvoeren van een enkele logische app:

| Name | Limiet voor meerdere tenants | Limiet voor de integratie service omgeving | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Uitvoerings duur | 90 dagen | 365 dagen | Zie de duur van de [uitvoering wijzigen](#change-duration)als u de standaard limiet wilt wijzigen. |
| Opslag bewaren | 90 dagen vanaf de begin tijd van de uitvoering | 365 dagen | Zie [opslag bewaaring wijzigen](#change-retention)als u de standaard limiet wilt wijzigen. |
| Mini maal terugkeer patroon | 1 seconde | 1 seconde ||
| Maximum interval van terugkeer patroon | 500 dagen | 500 dagen ||
|||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Uitvoerings duur en opslag bewaaring wijzigen

Volg deze stappen als u de standaard limiet voor de uitvoerings duur en opslag bewaaring wilt wijzigen. Als u meer dan de maximum limiet wilt, [neemt u contact op met het Logic apps team](mailto://logicappsemail@microsoft.com) om aan uw vereisten te voldoen.

1. Kies **werk stroom instellingen**in het Azure portal van het menu van de logische app.

2. Klik onder **runtime-opties**in de lijst **uitvoerings geschiedenis in dagen** op **aangepast**.

3. Typ of sleep de schuif regelaar voor het gewenste aantal dagen.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Gelijktijdigheid, herhalingen en limieten voor het debatchren

Dit zijn de limieten voor het uitvoeren van een enkele logische app:

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Gelijktijdigheid van triggers | * Onbeperkt wanneer het gelijktijdigheids beheer is uitgeschakeld <p><p>* 25 is de standaard limiet wanneer het gelijktijdigheids beheer is ingeschakeld. Dit kan niet ongedaan worden gemaakt nadat u het besturings element inschakelt. U kunt de standaard waarde van 1 tot en met 50 wijzigen. | Deze limiet beschrijft het hoogste aantal logische app-exemplaren dat tegelijkertijd kan worden uitgevoerd of parallel. <p><p>Als u de standaard limiet wilt wijzigen in een waarde tussen 1 en 50, raadpleegt u de [gelijktijdige overschrijding](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) van de trigger of [trigger instanties opeenvolgend](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). |
| Maximum aantal wachtende uitvoeringen | Wanneer het gelijktijdigheids beheer is ingeschakeld, is het minimum aantal wachtende uitvoeringen 10 plus het aantal gelijktijdige uitvoeringen (gelijktijdigheid van triggers). U kunt het maximum aantal tot 100 wijzigen, inclusief. | Deze limiet beschrijft het hoogste aantal logische app-exemplaren dat kan worden uitgevoerd als het maximum aantal gelijktijdige exemplaren van de logische app al wordt uitgevoerd. <p><p>Zie de limiet voor het uitvoeren van een [wacht](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs)tijd wijzigen om de standaard limiet te wijzigen. |
| Elementen van foreach-matrix | 100,000 | Deze limiet beschrijft het hoogste aantal matrix items dat voor elke lus kan worden verwerkt. <p><p>Als u grotere matrices wilt filteren, kunt u de [query actie](../connectors/connectors-native-query.md)gebruiken. |
| Gelijktijdigheid van foreach | 20 is de standaard limiet wanneer het gelijktijdigheids beheer is uitgeschakeld. U kunt de standaard waarde van 1 tot en met 50 wijzigen. | Deze limiet is het hoogste aantal ' for each '-herhalingen die tegelijkertijd kunnen worden uitgevoerd, of parallel. <p><p>Als u de standaard limiet wilt wijzigen in een waarde tussen 1 en 50, raadpleegt u [wijzigen voor elke "gelijktijdige overschrijding](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) " of [voert u "voor elke" sequentieel uit](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). |
| SplitOn items | 100,000 | Voor triggers die een matrix retour neren, kunt u een expressie opgeven die gebruikmaakt van een eigenschap SplitOn die de [matrix items in meerdere workflowexemplaren voor verwerking splitst of](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) opsplitst, in plaats van een foreach-lus te gebruiken. Deze expressie verwijst naar de matrix die moet worden gebruikt voor het maken en uitvoeren van een workflowexemplaar voor elk matrix item. |
| Tot iteraties | 5,000 | |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Doorvoer limieten

Dit zijn de limieten voor een definitie van een enkele logische app:

### <a name="multi-tenant-logic-apps-service"></a>Multi tenant-Logic Apps service

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Actie: Uitvoeringen per vijf minuten | 100.000 is de standaard limiet, maar 300.000 is de maximum limiet. | Als u de standaard limiet wilt wijzigen, raadpleegt u [uw logische app uitvoeren in de modus voor hoge door Voer](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode), die in preview is. Of u kunt de werk belasting indien nodig verdelen over meerdere logische apps. |
| Actie: Gelijktijdige uitgaande oproepen | ~2,500 | U kunt het aantal gelijktijdige aanvragen verminderen of de duur beperken als dat nodig is. |
| Runtime-eind punt: Gelijktijdige binnenkomende oproepen | ~1,000 | U kunt het aantal gelijktijdige aanvragen verminderen of de duur beperken als dat nodig is. |
| Runtime-eind punt: Lees aanroepen per 5 minuten  | 60,000 | U kunt de werk belasting naar meerdere apps distribueren als dat nodig is. |
| Runtime-eind punt: Aanroepen starten per 5 minuten | 45,000 | U kunt de werk belasting naar meerdere apps distribueren als dat nodig is. |
| Inhouds doorvoer per 5 minuten | 600 MB | U kunt de werk belasting naar meerdere apps distribueren als dat nodig is. |
||||

### <a name="integration-service-environment-ise"></a>Integration service Environment (ISE)

| Name | Limiet | Opmerkingen |
|------|-------|-------|
| Uitvoerings limiet basis eenheid | Systeem-beperkt wanneer de capaciteit van de infra structuur 80% bereikt | Biedt ~ 4.000 actie-uitvoeringen per minuut, wat ~ 160.000.000 actie uitvoeringen per maand is | |
| Limiet voor het uitvoeren van schaal eenheden | Systeem-beperkt wanneer de capaciteit van de infra structuur 80% bereikt | Elke schaal eenheid kan ~ 2.000 extra actie-uitvoeringen per minuut bieden, wat ~ 80.000.000 meer actie-uitvoeringen per maand | |
| Maximale schaal eenheden die u kunt toevoegen | 10 | |
||||

[Neem contact op met het Logic apps team](mailto://logicappsemail@microsoft.com) om aan uw vereisten te voldoen, om aan de slag te gaan met de normale verwerking, of om belasting tests uit te voeren.

<a name="request-limits"></a>

## <a name="http-limits"></a>HTTP-limieten

Dit zijn de limieten voor een enkele HTTP-aanvraag of een synchrone connector aanroep:

#### <a name="timeout"></a>Time-out

Sommige connector bewerkingen maken asynchrone aanroepen of Luis teren naar webhook-aanvragen, zodat de time-out voor deze bewerkingen mogelijk langer is dan deze limieten. Zie de technische Details voor de specifieke connector en ook [werk stroom triggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action)voor meer informatie.

| Name | Limiet voor meerdere tenants | Limiet voor de integratie service omgeving | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Uitgaande aanvraag | 120 seconden | 240 seconden | Gebruik een [asynchroon polling-patroon](../logic-apps/logic-apps-create-api-app.md#async-pattern) of een [lus until](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action)voor het uitvoeren van meer bewerkingen. |
| Synchrone reactie | 120 seconden | 240 seconden | Voor de oorspronkelijke aanvraag om het antwoord op te halen, moeten alle stappen in het antwoord binnen de limiet worden voltooid, tenzij u een andere logische app als geneste werk stroom aanroept. Zie [Logic apps aanroepen, activeren of nesten](../logic-apps/logic-apps-http-endpoint.md)voor meer informatie. |
|||||

#### <a name="message-size"></a>Berichtgrootte

| Name | Limiet voor meerdere tenants | Limiet voor de integratie service omgeving | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Berichtgrootte | 100 MB | 200 MB | Zie [grote berichten verwerken met Chunking](../logic-apps/logic-apps-handle-large-messages.md)om deze limiet te omzeilen. Sommige connectors en Api's ondersteunen echter mogelijk geen Chunking of zelfs de standaard limiet. |
| Bericht grootte met Chunking | 1 GB | 5 GB | Deze limiet geldt voor acties die systeem eigen ondersteuning bieden voor Chunking of waarmee u Chunking in de runtime configuratie kunt inschakelen. <p>Voor de integratie service omgeving ondersteunt de Logic Apps-Engine deze limiet, maar connectors hebben hun eigen segment limieten tot de limiet van de engine. Zie bijvoorbeeld [Azure Blob Storage-connector](/connectors/azureblob/). Zie [grote berichten verwerken met Chunking](../logic-apps/logic-apps-handle-large-messages.md)voor meer informatie over chunks. |
| Limiet voor expressie-evaluatie | 131.072 tekens | 131.072 tekens | De `@concat()`, `@base64()` ,`@string()` expressies mogen niet langer zijn dan deze limiet. |
|||||

#### <a name="retry-policy"></a>Beleid voor opnieuw proberen

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Nieuwe pogingen | 90 | De standaard waarde is 4. Als u de standaard waarde wilt wijzigen, gebruikt u de [para meter beleid opnieuw proberen](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Maximale vertraging voor opnieuw proberen | 1 dag | Als u de standaard waarde wilt wijzigen, gebruikt u de [para meter beleid opnieuw proberen](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Minimale vertraging voor opnieuw proberen | 5 seconden | Als u de standaard waarde wilt wijzigen, gebruikt u de [para meter beleid opnieuw proberen](../logic-apps/logic-apps-workflow-actions-triggers.md). |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Limieten voor aangepaste connectors

Dit zijn de limieten voor aangepaste connectors die u kunt maken op basis van web-Api's.

| Name | Limiet voor meerdere tenants | Limiet voor de integratie service omgeving | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Aantal aangepaste connectors | 1000 per Azure-abonnement | 1000 per Azure-abonnement ||
| Aantal aanvragen per minuut voor een aangepaste connector | 500 aanvragen per minuut per verbinding | 2\.000 aanvragen per minuut per *aangepaste connector* ||
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>Beheerde identiteiten

| Name | Limiet |
| ---- | ----- |
| Aantal logische apps met door het systeem toegewezen beheerde identiteiten per Azure-abonnement | 100 |
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Limieten van integratie account

Elk Azure-abonnement heeft deze limieten voor het integratie account:

* Eén integratie account voor de [gratis laag](../logic-apps/logic-apps-pricing.md#integration-accounts)

* 1\.000 totaal aantal integratie accounts, met inbegrip van integratie accounts in een [ISE (Integration service Environment)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) voor zowel [ontwikkel aars als Premium-sku's](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level).

* Elke ISE, of [Developer of Premium](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level), is beperkt tot 5 totale integratie accounts:

  | ISE SKU | Limiet |
  |---------|-------|
  | **Premium** | 5 totaal- [standaard](../logic-apps/logic-apps-pricing.md#integration-accounts) alleen, geen gratis of basis |
  | **Developer** | 5 totaal-gratis (beperkt tot 1), standaard of beide, maar geen basis |
  |||

Aanvullende kosten zijn van toepassing op integratie accounts die u toevoegt buiten de integratie accounts die zijn opgenomen in een ISE. Zie het [Logic apps-prijs model](../logic-apps/logic-apps-pricing.md#fixed-pricing)voor meer informatie over de prijzen en facturerings werkzaamheden voor ISEs. Zie [Logic apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps/)voor prijs tarieven.

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Limieten voor artefacten per integratie account

Dit zijn de limieten voor het aantal artefacten voor elke laag van de integratie-account. Zie [Logic apps prijzen](https://azure.microsoft.com/pricing/details/logic-apps/)voor prijs tarieven. Zie het [Logic apps-prijs model](../logic-apps/logic-apps-pricing.md#integration-accounts)voor meer informatie over de prijzen en facturering voor integratie accounts.

> [!NOTE]
> Gebruik de laag gratis alleen voor experimentele scenario's, niet voor productie scenario's. Deze laag beperkt de door Voer en het gebruik en heeft geen SLA (Service Level Agreement).

| Werp | Free | Basic | Standard |
|----------|------|-------|----------|
| EDI-handels overeenkomsten | 10 | 1 | 1000 |
| EDI-handels partners | 25 | 2 | 1000 |
| Kaarten | 25 | 500 | 1000 |
| Schema 's | 25 | 500 | 1000 |
| Assembly's | 10 | 25 | 1000 |
| Certificaten | 25 | 2 | 500 |
| Batch configuraties | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Capaciteits limieten artefacten

| Werp | Limiet | Opmerkingen |
| -------- | ----- | ----- |
| Assembly | 8 MB | Als u bestanden wilt uploaden die groter zijn dan 2 MB, gebruikt u een [Azure-opslag account en een BLOB-container](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
| Kaart (XSLT-bestand) | 8 MB | Als u bestanden wilt uploaden die groter zijn dan 2 MB, gebruikt u de [Azure Logic apps-rest API-kaarten](https://docs.microsoft.com/rest/api/logic/maps/createorupdate). |
| Schema | 8 MB | Als u bestanden wilt uploaden die groter zijn dan 2 MB, gebruikt u een [Azure-opslag account en een BLOB-container](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
||||

| Runtime-eind punt | Limiet | Opmerkingen |
|------------------|-------|-------|
| Lees aanroepen per 5 minuten | 60,000 | U kunt de werk belasting naar meerdere accounts distribueren als dat nodig is. |
| Aanroepen starten per 5 minuten | 45,000 | U kunt de werk belasting naar meerdere accounts distribueren als dat nodig is. |
| Tracerings aanroepen per 5 minuten | 45,000 | U kunt de werk belasting naar meerdere accounts distribueren als dat nodig is. |
| Gelijktijdige aanroepen blok keren | ~1,000 | U kunt het aantal gelijktijdige aanvragen verminderen of de duur beperken als dat nodig is. |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>B2B-Protocol (AS2, X12, EDIFACT) bericht grootte

Dit zijn de limieten voor de bericht grootte die van toepassing zijn op B2B-protocollen:

| Name | Limiet voor meerdere tenants | Limiet voor de integratie service omgeving | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| AS2 | v2-100 MB<br>v1 - 50 MB | v2-200 MB <br>v1 - 50 MB | Van toepassing op decoderen en coderen |
| X12 | 50 MB | 50 MB | Van toepassing op decoderen en coderen |
| EDIFACT | 50 MB | 50 MB | Van toepassing op decoderen en coderen |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>Logic apps uitschakelen of verwijderen

Wanneer u een logische app uitschakelt, worden er geen nieuwe uitvoeringen geïnstantieerd. Alle lopende uitvoeringen en in behandeling zijnde uitvoeringen gaan door totdat ze zijn voltooid. Dit kan enige tijd in beslag nemen.

Wanneer u een logische app verwijdert, worden geen nieuwe uitvoeringen gemaakt. Alle uitvoeringen die bezig zijn en wachten op uitvoering worden geannuleerd. Als u duizenden uitvoeringen hebt, kan de annulering een aanzienlijke tijd in beslag nemen.

<a name="configuration"></a>

## <a name="firewall-configuration-ip-addresses"></a>Firewallconfiguratie: IP-adressen

Alle Logic apps in dezelfde regio gebruiken dezelfde IP-adresbereiken. Ter ondersteuning van de aanroepen die uw Logic apps rechtstreeks aanbrengt met [http](../connectors/connectors-native-http.md), [http + Swagger](../connectors/connectors-native-http-swagger.md)en andere HTTP-aanvragen, stelt u uw firewalls in met *alle* [inkomende](#inbound) *en* [uitgaande](#outbound) IP-adressen die worden gebruikt door de Logic apps-service op basis van de regio's waar uw Logic apps bestaan. Deze adressen worden weer gegeven  onder de kopteksten binnenkomend en uitgaand in deze sectie en worden per regio gesorteerd.

Om de aanroepen te ondersteunen die door [micro soft beheerde connectors](../connectors/apis-list.md) worden gemaakt, stelt u uw firewall in met *alle* [uitgaande](#outbound) IP-adressen die door deze connectors worden gebruikt, op basis van de regio's waar uw Logic apps bestaan. Deze adressen worden weer gegeven  onder de kop uitgaand in deze sectie en worden per regio gesorteerd.

Voor [Azure Government](../azure-government/documentation-government-overview.md) en [Azure China 21vianet](https://docs.microsoft.com/azure/china/)zijn gereserveerde IP-adressen voor connectors momenteel niet beschikbaar.

> [!IMPORTANT]
>
> Als u bestaande configuraties hebt, moet u deze **zo snel mogelijk vóór 1 September 2018** bijwerken, zodat deze overeenkomen met de IP-adressen in deze lijsten voor de regio's waar uw Logic apps zijn opgenomen.

Logic Apps biedt geen ondersteuning voor rechtstreekse verbinding met Azure Storage-accounts via firewalls. Voor toegang tot deze opslag accounts gebruikt u een van de volgende opties:

* Een [integratie service omgeving](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)maken waarmee verbinding kan worden gemaakt met bronnen in een virtueel Azure-netwerk.

* Als u API Management al gebruikt, kunt u deze service gebruiken voor dit scenario. Zie [eenvoudige architectuur voor ondernemings integratie](https://aka.ms/aisarch)voor meer informatie.

<a name="inbound"></a>

### <a name="inbound-ip-addresses---logic-apps-service-only"></a>Inkomende IP-adressen-alleen Logic Apps-service

| Regio | IP |
|--------|----|
| Australië - oost | 13.75.153.66, 52.187.231.161, 104.210.89.222, 104.210.89.244 |
| Australië - zuidoost | 13.73.115.153, 40.115.78.70, 40.115.78.237, 52.189.216.28 |
| Brazilië - zuid | 191.234.166.198, 191.235.86.199, 191.235.94.220, 191.235.95.229 |
| Canada - midden | 13.88.249.209, 40.85.241.105, 52.233.29.79, 52.233.30.218 |
| Canada - oost | 40.86.202.42, 52.229.125.57, 52.232.129.143, 52.232.133.109 |
| India - centraal | 52.172.157.194, 52.172.184.192, 52.172.191.194, 104.211.73.195 |
| US - centraal | 13.67.236.76, 40.77.31.87, 40.77.111.254, 104.43.243.39 |
| Azië - oost | 13.75.89.159, 23.97.68.172, 40.83.98.194, 168.63.200.173 |
| East US | 40.117.99.79, 40.117.100.228, 137.116.126.165, 137.135.106.54 |
| US - oost 2 | 40.70.27.253, 40.79.44.7, 40.84.25.234, 40.84.59.136 |
| Japan - oost | 13.71.146.140, 13.78.43.164, 13.78.62.130, 13.78.84.187 |
| Japan - west | 40.74.68.85, 40.74.81.13, 40.74.85.215, 40.74.140.173 |
| US - noord-centraal | 65.52.9.64, 65.52.211.164, 168.62.249.81, 157.56.12.202 |
| Europa - noord | 13.79.173.49, 40.112.90.39, 52.169.218.253, 52.169.220.174 |
| US - zuid-centraal | 13.65.98.39, 13.84.41.46, 13.84.43.45, 40.84.138.132 |
| India - zuid | 52.172.9.47, 52.172.49.43, 52.172.51.140, 104.211.225.152 |
| Azië - zuidoost | 52.163.93.214, 52.187.65.81, 52.187.65.155, 104.215.181.6 |
| US - west-centraal | 13.78.137.247, 52.161.8.128, 52.161.19.82, 52.161.26.172 |
| Europa -west | 13.95.155.53, 51.144.176.185, 52.174.49.6, 52.174.54.218 |
| India - west | 104.211.157.237, 104.211.164.25, 104.211.164.112, 104.211.165.81 |
| US - west | 13.91.252.184, 52.160.90.237, 138.91.188.137, 157.56.160.212 |
| US - west 2 | 13.66.128.68, 13.66.224.169, 52.183.30.10, 52.183.39.67 |
| Verenigd Koninkrijk Zuid | 51.140.78.71, 51.140.79.109, 51.140.84.39, 51.140.155.81 |
| Verenigd Koninkrijk West | 51.141.48.98, 51.141.51.145, 51.141.53.164, 51.141.119.150 |
| | |

<a name="outbound"></a>

### <a name="outbound-ip-addresses---logic-apps-service--managed-connectors"></a>Uitgaande IP-adressen-Logic Apps service & beheerde connectors

| Regio | Logic Apps IP-adres | IP van beheerde connectors |
|--------|---------------|-----------------------|
| Australië - oost | 13.75.149.4, 52.187.226.96, 52.187.226.139, 52.187.227.245, 52.187.229.130, 52.187.231.184, 104.210.90.241, 104.210.91.55 | 13.70.72.192 - 13.70.72.207, 13.72.243.10 |
| Australië - zuidoost | 13.70.159.205, 13.73.114.207, 13.77.3.139, 13.77.56.167, 13.77.58.136, 52.189.214.42, 52.189.220.75, 52.189.222.77 | 13.77.50.240 - 13.77.50.255, 13.70.136.174 |
| Brazilië - zuid | 191.234.161.28, 191.234.161.168, 191.234.162.131, 191.234.162.178, 191.234.182.26, 191.235.82.221, 191.235.91.7, 191.237.255.116 | 191.233.203.192 - 191.233.203.207, 104.41.59.51 | 
| Canada - midden | 13.71.184.150, 13.71.186.1, 40.85.250.135, 40.85.250.212, 40.85.252.47, 52.233.29.92, 52.228.39.241, 52.228.39.244 | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.237.24.126 |
| Canada - oost | 40.86.203.228, 40.86.216.241, 40.86.217.241, 40.86.226.149, 40.86.228.93, 52.229.120.45, 52.229.126.25, 52.232.128.155 | 40.69.106.240 - 40.69.106.255, 52.242.35.152 |
| India - centraal | 52.172.154.168, 52.172.185.79, 52.172.186.159, 104.211.74.145, 104.211.90.162, 104.211.90.169, 104.211.101.108, 104.211.102.62 | 104.211.81.192 - 104.211.81.207, 52.172.211.12 |
| US - centraal | 13.67.236.125, 23.100.82.16, 23.100.86.139, 23.100.87.24, 23.100.87.56, 40.113.218.230, 40.122.170.198, 104.208.25.27 | 13.89.171.80 - 13.89.171.95, 52.173.245.164 |
| Azië - oost | 13.75.94.173, 40.83.73.39, 40.83.75.165, 40.83.77.208, 40.83.100.69, 40.83.127.19, 52.175.33.254, 65.52.175.34 | 13.75.36.64 - 13.75.36.79, 52.175.23.169 |
| East US | 13.92.98.111, 23.100.29.190, 23.101.132.208, 23.101.136.201, 23.101.139.153, 40.114.82.191, 40.121.91.41, 104.45.153.81 | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 |
| US - oost 2 | 40.70.26.154, 40.70.27.236, 40.70.29.214, 40.70.131.151, 40.84.30.147, 104.208.140.40, 104.208.155.200, 104.208.158.174 | 40.70.146.208 - 40.70.146.223, 52.232.188.154 |
| Japan - oost | 13.71.158.3, 13.71.158.120, 13.73.4.207, 13.78.18.168, 13.78.20.232, 13.78.21.155, 13.78.35.229, 13.78.42.223 | 13.78.108.0 - 13.78.108.15, 13.71.153.19 |
| Japan - west | 40.74.64.207, 40.74.68.85, 40.74.74.21, 40.74.76.213, 40.74.77.205, 40.74.140.4, 104.214.137.243, 138.91.26.45 | 40.74.100.224 - 40.74.100.239, 104.215.61.248 |
| US - noord-centraal | 52.162.208.216, 52.162.213.231, 65.52.8.225, 65.52.9.96, 65.52.10.183, 157.55.210.61, 157.55.212.238, 168.62.248.37 | 52.162.107.160 - 52.162.107.175, 52.162.242.161 |
| Europa - noord | 40.112.92.104, 40.112.95.216, 40.113.1.181, 40.113.3.202, 40.113.4.18, 40.113.12.95, 52.178.165.215, 52.178.166.21 | 13.69.227.208 - 13.69.227.223, 52.178.150.68 |
| US - zuid-centraal | 13.65.82.17, 13.66.52.232, 23.100.124.84, 23.100.127.172, 23.101.183.225, 70.37.54.122, 70.37.50.6, 104.210.144.48 | 104.214.19.48 - 104.214.19.63, 13.65.86.57 |
| India - zuid | 52.172.50.24, 52.172.52.0, 52.172.55.231, 104.211.227.229, 104.211.229.115, 104.211.230.126, 104.211.230.129, 104.211.231.39 | 40.78.194.240 - 40.78.194.255, 13.71.125.22 |
| Azië - zuidoost | 13.67.91.135, 13.67.107.128, 13.67.110.109, 13.76.4.194, 13.76.5.96, 13.76.133.155, 52.163.228.93, 52.163.230.166 | 13.67.8.240 - 13.67.8.255, 52.187.68.19 |
| US - west-centraal | 13.78.129.20, 13.78.137.179, 13.78.141.75, 13.78.148.140, 13.78.151.161, 52.161.18.218, 52.161.9.108, 52.161.27.190 | 13.71.195.32 - 13.71.195.47, 52.161.102.22 |
| Europa -west | 13.95.147.65, 23.97.210.126, 23.97.211.179, 23.97.218.130, 40.68.209.23, 40.68.222.65, 51.144.182.201, 104.45.9.52 | 13.69.64.208 - 13.69.64.223, 52.174.88.118 |
| India - west | 104.211.154.7, 104.211.154.59, 104.211.156.153, 104.211.158.123, 104.211.158.127, 104.211.162.205, 104.211.164.80, 104.211.164.136 | 104.211.146.224 - 104.211.146.239, 104.211.189.218 |
| US - west | 40.83.164.80, 40.118.244.241, 40.118.241.243, 52.160.92.112, 104.42.38.32, 104.42.49.145, 157.56.162.53, 157.56.167.147 | 40.112.243.160 - 40.112.243.175, 104.42.122.49 |
| US - west 2 | 13.66.201.169, 13.66.210.167, 13.66.246.219, 13.77.149.159, 52.175.198.132, 52.183.29.132, 52.183.30.169 | 13.66.140.128 - 13.66.140.143, 52.183.78.157 |
| Verenigd Koninkrijk Zuid | 51.140.28.225, 51.140.73.85, 51.140.74.14, 51.140.78.44, 51.140.137.190, 51.140.142.28, 51.140.153.135, 51.140.158.24 | 51.140.148.0 - 51.140.148.15, 51.140.80.51 |
| Verenigd Koninkrijk West | 51.141.45.238, 51.141.47.136, 51.141.54.185, 51.141.112.112, 51.141.113.36, 51.141.114.77, 51.141.118.119, 51.141.119.63 | 51.140.211.0 - 51.140.211.15, 51.141.47.105 |
||||

## <a name="next-steps"></a>Volgende stappen  

* Meer informatie over [het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Meer informatie over [algemene voor beelden en scenario's](../logic-apps/logic-apps-examples-and-scenarios.md)
