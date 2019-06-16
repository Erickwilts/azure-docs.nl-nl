---
title: Limieten en configuratie - Azure Logic Apps | Microsoft Docs
description: Service limieten en configuratiewaarden voor Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/23/2019
ms.openlocfilehash: e824ac81f1336644fa70cc24539284feacee3199
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244539"
---
# <a name="limits-and-configuration-information-for-azure-logic-apps"></a>Limieten en configuratie-informatie voor Azure Logic Apps

Dit artikel beschrijft de limieten en configuratiegegevens voor het maken en uitvoeren van geautomatiseerde werkstromen met Azure Logic Apps. Zie voor Microsoft Flow, [limieten en configuratie in Microsoft Flow](https://docs.microsoft.com/flow/limits-and-config).

<a name="definition-limits"></a>

## <a name="definition-limits"></a>Definitielimieten

Hier zijn de limieten voor een definitie van één logische app:

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Acties per werkstroom | 500 | U kunt om uit te breiden deze limiet, geneste werkstromen toevoegen als nodig is. |
| Toegestane nestdiepte voor acties voor acties | 8 | U kunt om uit te breiden deze limiet, geneste werkstromen toevoegen als nodig is. |
| Werkstromen per regio per abonnement | 1000 | |
| Triggers per werkstroom | 10 | Als u werkt in de weergave van code, niet de ontwerper |
| Switch bereik gevallen beperken | 25 | |
| Variabelen per werkstroom | 250 | |
| Tekens per expressie | 8\.192 | |
| Maximale grootte voor `trackedProperties` | 16\.000 tekens |
| Geef de naam voor `action` of `trigger` | 80 tekens | |
| Lengte van `description` | 256 tekens | |
| Maximaal `parameters` | 50 | |
| Maximaal `outputs` | 10 | |
||||

<a name="run-duration-retention-limits"></a>

## <a name="run-duration-and-retention-limits"></a>Limieten voor de duur en retentie uitvoeren

Hier zijn de limieten voor een enkele logische app:

| Name | Limiet voor meerdere tenants | Integratie van service-omgeving beperken | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Uitvoeringsduur | 90 dagen | 365 dagen | De standaardlimiet Zie [wijziging uitvoeringsduur](#change-duration). |
| Bewaarperiode | Begintijd van 90 dagen na het uitvoeren van | 365 dagen | De standaardlimiet Zie [opslag retentie wijzigen](#change-retention). |
| Minimale vernieuwingsfrequentie | 1 seconde | 1 seconde ||
| Maximale terugkeerpatroon | 500 dagen | 500 dagen ||
|||||

<a name="change-duration"></a>
<a name="change-retention"></a>

### <a name="change-run-duration-and-storage-retention"></a>Voer de duur en opslag retentie wijzigen

Als u wilt de standaardlimiet voor uitvoeringsduur en bewaarperiode wijzigen, de volgende stappen uit. Als u nodig hebt om te gaan dan de maximale limiet [contact op met de Logic Apps-team](mailto://logicappsemail@microsoft.com) voor hulp bij uw vereisten.

1. Kies in de Azure-portal, in het menu van uw logische app **Werkstroominstellingen**.

2. Onder **Runtime-opties**, uit de **behoud van uitvoeringsgeschiedenis in dagen uitgevoerd** Kies **aangepaste**.

3. Opgeven of de schuifregelaar voor het aantal dagen dat u wilt.

<a name="looping-debatching-limits"></a>

## <a name="concurrency-looping-and-debatching-limits"></a>Gelijktijdigheid van taken, lussen en debatching limieten

Hier zijn de limieten voor een enkele logische app:

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Trigger gelijktijdigheid | * Onbeperkt wanneer gelijktijdigheidsbeheer is uitgeschakeld <p><p>* 25 is de standaardlimiet wanneer gelijktijdigheidsbeheer is ingeschakeld, die kan niet ongedaan worden gemaakt nadat u op het besturingselement. U kunt de standaardinstelling wijzigen op een waarde tussen 1 en 50 liggen. | Deze limiet beschrijving van het hoogste aantal logic app-exemplaren die op hetzelfde moment of parallel kunnen worden uitgevoerd. <p><p>De standaardlimiet op een waarde tussen 1 en 50 liggen, Zie [limiet voor gelijktijdigheid van wijziging trigger](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency) of [exemplaren sequentieel activeren](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-trigger). |
| Maximale wachttijd wordt uitgevoerd | Wanneer de gelijktijdigheidsbeheer is ingeschakeld, wordt het minimum aantal uitvoeringen van de wachtrij is 10 plus het aantal gelijktijdige uitvoeringen (trigger gelijktijdigheid). Kunt u het maximum aantal maximaal 100 liggen. | Deze limiet beschrijving van het hoogste aantal logic app-exemplaren die worden uitgevoerd wanneer u uw logische app wordt al uitgevoerd voor het maximum aantal gelijktijdige exemplaren kan wachten. <p><p>De standaardlimiet Zie [wijziging wachten uitvoeringen beperken](../logic-apps/logic-apps-workflow-actions-triggers.md#change-waiting-runs). |
| Foreach-matrix-items | 100\.000 | Deze limiet beschrijving van het hoogste aantal matrixitems waarmee een lus 'voor elke' kan worden verwerkt. <p><p>Voor grotere matrices filteren, kunt u de [queryactie](../connectors/connectors-native-query.md). |
| Foreach-gelijktijdigheid | 20 is de standaardlimiet wanneer gelijktijdigheidsbeheer is uitgeschakeld. U kunt de standaardinstelling wijzigen op een waarde tussen 1 en 50 liggen. | Deze limiet is hoogste aantal 'voor elke' iteraties die kunnen worden uitgevoerd op hetzelfde moment of parallel in een lus. <p><p>De standaardlimiet op een waarde tussen 1 en 50 liggen, Zie [wijzigen 'voor elke' gelijktijdigheid limiet](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency) of [uitvoeren 'voor elke' wordt uitgevoerd na elkaar](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each). |
| SplitOn-items | 100\.000 | Voor triggers die een matrix retourneert, kunt u een expressie die gebruikmaakt van een 'SplitOn'-eigenschap die [splitst of matrixitems in meerdere werkstroomexemplaren debatches](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) voor verwerking, in plaats van gebruik in een 'Foreach"een lus. Deze expressie verwijst naar de matrix moet worden gebruikt voor het maken en uitvoeren van een werkstroomexemplaar dat voor elk matrixitem. |
| UNTIL-iteraties | 5,000 | |
||||

<a name="throughput-limits"></a>

## <a name="throughput-limits"></a>Doorvoerlimieten

Hier zijn de limieten voor een enkele logische app:

### <a name="multi-tenant-logic-apps-service"></a>Service met meerdere tenants Logic Apps

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Actie: Uitvoeringen per vijf minuten | 100\.000 de standaardlimiet is, maar 300.000 is de maximale limiet. | De standaardlimiet Zie [uw logische app uitvoeren in de modus "hoge doorvoer"](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode), deze bevindt zich in preview. Of u kunt de workload gedistribueerd over meer dan één logische app zo nodig. |
| Actie: Gelijktijdige uitgaande oproepen | ~2,500 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. |
| Runtime-eindpunt: Gelijktijdige binnenkomende oproepen | ~1,000 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. |
| Runtime-eindpunt: Aanroepen per vijf minuten lezen  | 60,000 | U kunt werklast verdelen over meer dan één app zo nodig. |
| Runtime-eindpunt: Aanroepen per vijf minuten aanroepen | 45,000 | U kunt werklast verdelen over meer dan één app zo nodig. |
| Inhoud doorvoer per vijf minuten | 600 MB | U kunt werklast verdelen over meer dan één app zo nodig. |
||||

### <a name="integration-service-environment-ise"></a>Integratie van service-omgeving (ISE)

| Name | Limiet | Opmerkingen |
|------|-------|-------|
| Limiet voor basiseenheid voor uitvoering | Systeem beperkt wanneer infrastructuurcapaciteit 80% bereikt | Biedt ~ 4.000 actie-uitvoeringen per minuut, dit is van ~ 160 miljoen actie-uitvoeringen per maand | |
| Schaallimiet eenheid worden uitgevoerd | Systeem beperkt wanneer infrastructuurcapaciteit 80% bereikt | Elke schaaleenheid kan ~ 2.000 aanvullende actie-uitvoeringen per minuut, die ongeveer 80 miljoen bieden meer actie-uitvoeringen per maand | |
| Maximale schaaleenheden die u kunt toevoegen | 10 | |
||||

Hoger dan deze limieten in de normale verwerking of voer belastingtests uitvoeren die mogelijk verder gaan dan deze limieten [contact op met de Logic Apps-team](mailto://logicappsemail@microsoft.com) voor hulp bij uw vereisten.

<a name="request-limits"></a>

## <a name="http-limits"></a>HTTP-limieten

Hier zijn de limieten voor een afzonderlijke HTTP-aanvraag of een aanroep van synchrone connector:

#### <a name="timeout"></a>Time-out

Bepaalde bewerkingen connector asynchrone aanroepen of luisteren naar aanvragen van de webhook, zodat de time-out voor deze bewerkingen mogelijk niet langer zijn dan deze limieten. Zie voor meer informatie, de technische details voor de specifieke connector en ook [werkstroomtriggers en acties](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action).

| Name | Limiet voor meerdere tenants | Integratie van service-omgeving beperken | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Uitgaande aanvraag | 120 seconden | 240 seconden | Voor meer actieve bewerkingen, gebruikt u een [asynchrone polling patroon](../logic-apps/logic-apps-create-api-app.md#async-pattern) of een [totdat-lus](../logic-apps/logic-apps-workflow-actions-triggers.md#until-action). |
| Synchrone reactie | 120 seconden | 240 seconden | Alle stappen in het antwoord moeten voltooien binnen de limiet voor de oorspronkelijke aanvraag voor het ophalen van het antwoord, tenzij u een andere logische app als een geneste werkstroom aanroepen. Zie voor meer informatie, [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md). |
|||||

#### <a name="message-size"></a>Berichtgrootte

| Name | Limiet voor meerdere tenants | Integratie van service-omgeving beperken | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Berichtgrootte | 100 MB | 200 MB | Als tijdelijke oplossing voor deze limiet, Zie [grote berichten met logische groepen te verdelen verwerken](../logic-apps/logic-apps-handle-large-messages.md). Echter sommige connectors en API's mogelijk geen ondersteuning voor logische groepen te verdelen of zelfs de standaardlimiet. |
| Grootte van het bericht met logische groepen te verdelen | 1 GB | 5 GB | Deze limiet geldt voor acties die systeemeigen ondersteuning voor logische groepen te verdelen of kunnen u inschakelen opdelen in hun runtime-configuratie. <p>Voor de integratie van service-omgeving, de Logic Apps-engine ondersteunt deze limiet, maar connectors hebben hun eigen chunking beperkt tot de engine is bereikt, bijvoorbeeld, Zie [Azure Blob Storage-connector](/connectors/azureblob/). Zie voor meer informatie logische groepen te verdelen, [grote berichten met logische groepen te verdelen verwerken](../logic-apps/logic-apps-handle-large-messages.md). |
| Limiet voor evaluatie van expressie | 131\.072 tekens | 131\.072 tekens | De `@concat()`, `@base64()`, `@string()` expressies mag niet langer zijn dan deze limiet. |
|||||

#### <a name="retry-policy"></a>Beleid voor opnieuw proberen

| Name | Limiet | Opmerkingen |
| ---- | ----- | ----- |
| Nieuwe pogingen | 90 | De standaardwaarde is 4. U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Maximale vertraging voor opnieuw proberen | 1 dag | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
| Min vertraging voor opnieuw proberen | 5 seconden | U kunt de standaardinstelling wijzigen met de [parameter van het beleid opnieuw](../logic-apps/logic-apps-workflow-actions-triggers.md). |
||||

<a name="custom-connector-limits"></a>

## <a name="custom-connector-limits"></a>Limieten voor de aangepaste connector

Hier zijn de limieten voor aangepaste connectors die u van web-API's maken kunt.

| Name | Limiet voor meerdere tenants | Integratie van service-omgeving beperken | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| Aantal aangepaste connectors | 1000 per Azure-abonnement | 1000 per Azure-abonnement ||
| Aantal aanvragen per minuut voor een aangepaste connector | 500 aanvragen per minuut per verbinding | 2000 aanvragen per minuut per *aangepaste connector* ||
|||

<a name="managed-identity"></a>

## <a name="managed-identities"></a>Beheerde identiteiten

| Name | Limiet |
| ---- | ----- |
| Aantal logische apps met het systeem toegewezen identiteit per Azure-abonnement beheerd | 100 |
|||

<a name="integration-account-limits"></a>

## <a name="integration-account-limits"></a>Limieten voor integratie-account

<a name="artifact-number-limits"></a>

### <a name="artifact-limits-per-integration-account"></a>Artefact limieten per integratie-account

Hier zijn de limieten voor het aantal artefacten voor elke integratie-account. Zie voor meer informatie, [prijzen voor Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!NOTE] 
> Gebruik de gratis laag alleen voor experimentele scenario's, niet-productie scenario's. Deze laag beperkt doorvoer en het gebruik, en heeft geen service level agreement (SLA).

| Artefact | Gratis | Basic | Standard |
|----------|------|-------|----------|
| EDI-handelspartners overeenkomsten | 10 | 1 | 1000 |
| EDI-handelspartners | 25 | 2 | 1000 |
| Kaarten | 25 | 500 | 1000 |
| Schema 's | 25 | 500 | 1000 |
| Assembly's | 10 | 25 | 1000 |
| Certificaten | 25 | 2 | 500 |
| Batchconfiguraties | 5 | 1 | 50 |
||||

<a name="artifact-capacity-limits"></a>

### <a name="artifact-capacity-limits"></a>Capaciteitslimieten artefact

| Artefact | Limiet | Opmerkingen |
| -------- | ----- | ----- |
| Assembly | 8 MB | Als u wilt uploaden van bestanden die groter zijn dan 2 MB, gebruikt u een [Azure storage-account en blob-container](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
| Kaart (XSLT-bestand) | 8 MB | Als u wilt uploaden van bestanden die groter zijn dan 2 MB, gebruikt u de [REST API van Azure Logic Apps - kaarten](https://docs.microsoft.com/rest/api/logic/maps/createorupdate). |
| Schema | 8 MB | Als u wilt uploaden van bestanden die groter zijn dan 2 MB, gebruikt u een [Azure storage-account en blob-container](../logic-apps/logic-apps-enterprise-integration-schemas.md). |
||||

| Runtime-eindpunt | Limiet | Opmerkingen |
|------------------|-------|-------|
| Aanroepen per vijf minuten lezen | 60,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. |
| Aanroepen per vijf minuten aanroepen | 45,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. |
| Aanroepen per vijf minuten voor het bijhouden | 45,000 | U kunt de workload gedistribueerd over meer dan één account indien nodig. |
| Gelijktijdige aanroepen blokkeren | ~1,000 | U kunt Verminder het aantal gelijktijdige aanvragen of Beperk de duur indien nodig. |
||||

<a name="b2b-protocol-limits"></a>

### <a name="b2b-protocol-as2-x12-edifact-message-size"></a>Berichtgrootte voor B2B-protocol (AS2, X12, EDIFACT)

Dit zijn de limieten die voor B2B-protocollen gelden:

| Name | Limiet voor meerdere tenants | Integratie van service-omgeving beperken | Opmerkingen |
|------|--------------------|---------------------------------------|-------|
| AS2 | v2 - 100 MB<br>v1 - 50 MB | v2 - 200 MB <br>v1 - 50 MB | Is van toepassing op decoderen en coderen |
| X12 | 50 MB | 50 MB | Is van toepassing op decoderen en coderen |
| EDIFACT | 50 MB | 50 MB | Is van toepassing op decoderen en coderen |
||||

<a name="disable-delete"></a>

## <a name="disabling-or-deleting-logic-apps"></a>Het uitschakelen of verwijderen van logische apps

Wanneer u een logische app uitschakelt, wordt geen nieuwe uitvoeringen worden geïnstantieerd. Alle uitgevoerd en actieve uitvoeringen blijven totdat ze zijn voltooid, wat tijd in beslag kan duren.

Wanneer u een logische app verwijdert, worden geen nieuwe uitvoeringen gemaakt. Alle uitvoeringen die bezig zijn en wachten op uitvoering worden geannuleerd. Als u duizenden uitvoeringen hebt, kan de annulering een aanzienlijke tijd in beslag nemen.

<a name="configuration"></a>

## <a name="firewall-configuration-ip-addresses"></a>Firewallconfiguratie: IP-adressen

Alle logische apps in dezelfde regio gebruiken de dezelfde IP-adresbereiken. Ter ondersteuning van de aanroepen die uw logische apps rechtstreeks met [HTTP](../connectors/connectors-native-http.md), [HTTP + Swagger](../connectors/connectors-native-http-swagger.md), en andere HTTP-aanvragen, instellen van uw firewalls met *alle* de [ inkomende](#inbound) *en* [uitgaande](#outbound) IP-adressen die worden gebruikt door de service Logic Apps, op basis van de regio's waar uw logische apps bestaan. Deze adressen worden weergegeven onder de **inkomend** en **uitgaand** koppen in deze sectie en per regio worden gesorteerd.

Ter ondersteuning van de aanroepen die [door Microsoft beheerde connectors](../connectors/apis-list.md) maken, instellen van uw firewall met *alle* de [uitgaande](#outbound) IP-adressen die worden gebruikt door deze connectors, op basis van de regio's waar uw logische apps bestaan. Deze adressen worden weergegeven onder de **uitgaand** koptekst in deze sectie en per regio worden gesorteerd.

Voor [Azure Government](../azure-government/documentation-government-overview.md) en [Azure China 21Vianet](https://docs.microsoft.com/azure/china/), gereserveerde IP-adressen voor connectors niet op dit moment beschikbaar zijn.

> [!IMPORTANT]
>
> Als u bestaande configuraties hebt, werkt u deze **zo snel mogelijk vóór 1 September 2018** zodat ze zijn en overeenkomen met de IP-adressen in deze lijsten voor de regio's waar uw logische apps bestaan.

Logic Apps biedt geen ondersteuning voor rechtstreeks verbinding te maken naar Azure storage-accounts via firewalls. Voor toegang tot deze opslagaccounts, hier een van beide opties gebruiken:

* Maak een [integratieserviceomgeving](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), waarmee verbinding kan maken met bronnen in een Azure-netwerk.

* Als u al API Management gebruikt, kunt u deze service voor dit scenario. Zie voor meer informatie, [integratie van eenvoudige ondernemingsstructuur](https://aka.ms/aisarch).

<a name="inbound"></a>

### <a name="inbound-ip-addresses---logic-apps-service-only"></a>Inkomende IP-adressen - Logic Apps-service

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
| US - oost | 40.117.99.79, 40.117.100.228, 137.116.126.165, 137.135.106.54 |
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

### <a name="outbound-ip-addresses---logic-apps-service--managed-connectors"></a>Uitgaande IP-adressen - Logic Apps-service & beheerde connectors

| Regio | Logic Apps-IP | Beheerde connectors IP |
|--------|---------------|-----------------------|
| Australië - oost | 13.75.149.4, 52.187.226.96, 52.187.226.139, 52.187.227.245, 52.187.229.130, 52.187.231.184, 104.210.90.241, 104.210.91.55 | 13.70.72.192 - 13.70.72.207, 13.72.243.10, 40.126.251.213 |
| Australië - zuidoost | 13.70.159.205, 13.73.114.207, 13.77.3.139, 13.77.56.167, 13.77.58.136, 52.189.214.42, 52.189.220.75, 52.189.222.77 | 13.70.136.174, 13.77.50.240 - 13.77.50.255, 40.127.80.34 |
| Brazilië - zuid | 191.234.161.28, 191.234.161.168, 191.234.162.131, 191.234.162.178, 191.234.182.26, 191.235.82.221, 191.235.91.7, 191.237.255.116 | 104.41.59.51, 191.232.38.129, 191.233.203.192 - 191.233.203.207 | 
| Canada - midden | 13.71.184.150, 13.71.186.1, 40.85.250.135, 40.85.250.212, 40.85.252.47, 52.233.29.92, 52.228.39.241, 52.228.39.244 | 13.71.170.208 - 13.71.170.223, 13.71.170.224 - 13.71.170.239, 52.228.33.76, 52.228.34.13, 52.228.42.205, 52.233.26.83, 52.233.31.197, 52.237.24.126 |
| Canada - oost | 40.86.203.228, 40.86.216.241, 40.86.217.241, 40.86.226.149, 40.86.228.93, 52.229.120.45, 52.229.126.25, 52.232.128.155 | 40.69.106.240 - 40.69.106.255, 52.229.120.52, 52.229.120.131, 52.229.120.178, 52.229.123.98, 52.229.126.202, 52.242.35.152 |
| India - centraal | 52.172.154.168, 52.172.185.79, 52.172.186.159, 104.211.74.145, 104.211.90.162, 104.211.90.169, 104.211.101.108, 104.211.102.62 | 52.172.211.12, 104.211.81.192 - 104.211.81.207, 104.211.98.164 |
| US - centraal | 13.67.236.125, 23.100.82.16, 23.100.86.139, 23.100.87.24, 23.100.87.56, 40.113.218.230, 40.122.170.198, 104.208.25.27 | 13.89.171.80 - 13.89.171.95, 40.122.49.51, 52.173.245.164 |
| Azië - oost | 13.75.94.173, 40.83.73.39, 40.83.75.165, 40.83.77.208, 40.83.100.69, 40.83.127.19, 52.175.33.254, 65.52.175.34 | 13.75.36.64 - 13.75.36.79, 23.99.116.181, 52.175.23.169 |
| US - oost | 13.92.98.111, 23.100.29.190, 23.101.132.208, 23.101.136.201, 23.101.139.153, 40.114.82.191, 40.121.91.41, 104.45.153.81 | 40.71.11.80 - 40.71.11.95, 40.71.249.205, 191.237.41.52 |
| US - oost 2 | 40.70.26.154, 40.70.27.236, 40.70.29.214, 40.70.131.151, 40.84.30.147, 104.208.140.40, 104.208.155.200, 104.208.158.174 | 40.70.146.208 - 40.70.146.223, 52.232.188.154, 104.208.233.100 |
| Japan - oost | 13.71.158.3, 13.71.158.120, 13.73.4.207, 13.78.18.168, 13.78.20.232, 13.78.21.155, 13.78.35.229, 13.78.42.223 | 13.71.153.19, 13.78.108.0 - 13.78.108.15, 40.115.186.96 |
| Japan - west | 40.74.64.207, 40.74.68.85, 40.74.74.21, 40.74.76.213, 40.74.77.205, 40.74.140.4, 104.214.137.243, 138.91.26.45 | 40.74.100.224 - 40.74.100.239, 40.74.130.77, 104.215.61.248 |
| US - noord-centraal | 52.162.208.216, 52.162.213.231, 65.52.8.225, 65.52.9.96, 65.52.10.183, 157.55.210.61, 157.55.212.238, 168.62.248.37 | 52.162.107.160 - 52.162.107.175, 52.162.242.161, 65.52.218.230 |
| Europa - noord | 40.112.92.104, 40.112.95.216, 40.113.1.181, 40.113.3.202, 40.113.4.18, 40.113.12.95, 52.178.165.215, 52.178.166.21 | 13.69.227.208 - 13.69.227.223, 52.178.150.68, 104.45.93.9 |
| US - zuid-centraal | 13.65.82.17, 13.66.52.232, 23.100.124.84, 23.100.127.172, 23.101.183.225, 70.37.54.122, 70.37.50.6, 104.210.144.48 | 13.65.86.57, 104.214.19.48 - 104.214.19.63, 104.214.70.191 |
| India - zuid | 52.172.50.24, 52.172.52.0, 52.172.55.231, 104.211.227.229, 104.211.229.115, 104.211.230.126, 104.211.230.129, 104.211.231.39 | 13.71.125.22, 40.78.194.240 - 40.78.194.255, 104.211.227.225 |
| Azië - zuidoost | 13.67.91.135, 13.67.107.128, 13.67.110.109, 13.76.4.194, 13.76.5.96, 13.76.133.155, 52.163.228.93, 52.163.230.166 | 13.67.8.240 - 13.67.8.255, 13.76.231.68, 52.187.68.19 |
| US - west-centraal | 13.78.129.20, 13.78.137.179, 13.78.141.75, 13.78.148.140, 13.78.151.161, 52.161.18.218, 52.161.9.108, 52.161.27.190 | 13.71.195.32 - 13.71.195.47, 52.161.24.128, 52.161.26.212, 52.161.27.108, 52.161.29.35, 52.161.30.5, 52.161.102.22 |
| Europa -west | 13.95.147.65, 23.97.210.126, 23.97.211.179, 23.97.218.130, 40.68.209.23, 40.68.222.65, 51.144.182.201, 104.45.9.52 | 13.69.64.208 - 13.69.64.223, 40.115.50.13, 52.174.88.118 |
| India - west | 104.211.154.7, 104.211.154.59, 104.211.156.153, 104.211.158.123, 104.211.158.127, 104.211.162.205, 104.211.164.80, 104.211.164.136 | 104.211.146.224 - 104.211.146.239, 104.211.161.203, 104.211.189.218 |
| US - west | 40.83.164.80, 40.118.244.241, 40.118.241.243, 52.160.92.112, 104.42.38.32, 104.42.49.145, 157.56.162.53, 157.56.167.147 |40.112.243.160 - 40.112.243.175, 104.40.51.248, 104.42.122.49 |
| US - west 2 | 13.66.201.169, 13.66.210.167, 13.66.246.219, 13.77.149.159, 52.175.198.132, 52.183.29.132, 52.183.30.169 | 13.66.140.128 - 13.66.140.143, 13.66.218.78, 13.66.219.14, 13.66.220.135, 13.66.221.19, 13.66.225.219, 52.183.78.157 |
| Verenigd Koninkrijk Zuid | 51.140.28.225, 51.140.73.85, 51.140.74.14, 51.140.78.44, 51.140.137.190, 51.140.142.28, 51.140.153.135, 51.140.158.24 | 51.140.80.51, 51.140.148.0 - 51.140.148.15 |
| Verenigd Koninkrijk West | 51.141.45.238, 51.141.47.136, 51.141.54.185, 51.141.112.112, 51.141.113.36, 51.141.114.77, 51.141.118.119, 51.141.119.63 | 51.140.211.0 - 51.140.211.15, 51.141.47.105 |
||||

## <a name="next-steps"></a>Volgende stappen  

* Meer informatie over het [uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)  
* Meer informatie over [algemene voorbeelden en scenario's](../logic-apps/logic-apps-examples-and-scenarios.md)
