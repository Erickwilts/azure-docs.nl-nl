---
title: Veelvoorkomende fouten oplossen
description: Meer informatie over het oplossen van problemen met het maken van beleids definities, de verschillende SDK en de invoeg toepassing voor Kubernetes.
ms.date: 10/05/2020
ms.topic: troubleshooting
ms.openlocfilehash: 6026dc75187c8a70203a2484380eed70d519599d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91743434"
---
# <a name="troubleshoot-errors-using-azure-policy"></a>Fouten oplossen met behulp van Azure Policy

Er kunnen fouten optreden bij het maken van beleids definities, het werken met SDK of het instellen van de [Azure Policy voor](../concepts/policy-for-kubernetes.md) de invoeg toepassing Kubernetes. In dit artikel worden verschillende fouten beschreven die zich kunnen voordoen en hoe u deze kunt oplossen.

## <a name="finding-error-details"></a>Fout details zoeken

De locatie van de fout Details is afhankelijk van de actie die de fout veroorzaakt.

- Wanneer u met een aangepast beleid werkt, probeert u het in de Azure Portal om meer informatie over het schema op te halen of om de resulterende [compatibiliteits gegevens](../how-to/get-compliance-data.md) te bekijken om te zien hoe de resources zijn geëvalueerd.
- Bij het werken met verschillende SDK biedt de SDK Details over waarom de functie is mislukt.
- Wanneer u werkt met de invoeg toepassing voor Kubernetes, begint u met de [logboek registratie](../concepts/policy-for-kubernetes.md#logging) in het cluster.

## <a name="general-errors"></a>Algemene fouten

### <a name="scenario-alias-not-found"></a>Scenario: alias niet gevonden

#### <a name="issue"></a>Probleem

Azure Policy maakt gebruik van [aliassen](../concepts/definition-structure.md#aliases) om toe te wijzen aan Azure Resource Manager eigenschappen.

#### <a name="cause"></a>Oorzaak

Er wordt een onjuiste of niet-bestaande alias gebruikt in een beleids definitie.

#### <a name="resolution"></a>Oplossing

Controleer eerst of de Resource Manager-eigenschap een alias heeft. Gebruik [Azure Policy extensie voor Visual Studio code](../how-to/extension-for-vscode.md), [Azure resource Graph](../../resource-graph/samples/starter.md#distinct-alias-values)of SDK om beschik bare aliassen op te zoeken. Als de alias voor een resource manager-eigenschap niet bestaat, maakt u een ondersteunings ticket.

### <a name="scenario-evaluation-details-not-up-to-date"></a>Scenario: evaluatie Details zijn niet up-to-date

#### <a name="issue"></a>Probleem

Een resource heeft de status niet gestart of de compatibiliteits Details zijn niet actueel.

#### <a name="cause"></a>Oorzaak

De toewijzing van een nieuw beleid of initiatief duurt ongeveer 30 minuten. Nieuwe of bijgewerkte resources binnen het bereik van een bestaande toewijzing worden later meer dan 15 minuten beschikbaar. Er wordt om de 24 uur een standaard compatibiliteits scan uitgevoerd. Zie [evaluatie triggers](../how-to/get-compliance-data.md#evaluation-triggers)voor meer informatie.

#### <a name="resolution"></a>Oplossing

Wacht eerst de juiste hoeveelheid tijd om een evaluatie uit te voeren en de resultaten van naleving beschikbaar te stellen in Azure Portal of SDK. Zie [evaluatie scan op aanvraag](../how-to/get-compliance-data.md#on-demand-evaluation-scan)om een nieuwe evaluatie scan te starten met Azure PowerShell of rest API.

### <a name="scenario-compliance-not-as-expected"></a>Scenario: naleving niet zoals verwacht

#### <a name="issue"></a>Probleem

Een resource bevindt zich niet in de evaluatie status, niet _compatibel_ of _niet-compatibel_, die voor die resource wordt verwacht.

#### <a name="cause"></a>Oorzaak

De resource bevindt zich niet in het juiste bereik voor de beleids toewijzing of de beleids definitie werkt niet zoals bedoeld.

#### <a name="resolution"></a>Oplossing

Volg deze stappen om de beleids definitie op te lossen:

1. Wacht eerst de juiste hoeveelheid tijd om een evaluatie uit te voeren en de resultaten van naleving beschikbaar te stellen in Azure Portal of SDK. Zie [evaluatie scan op aanvraag](../how-to/get-compliance-data.md#on-demand-evaluation-scan)om een nieuwe evaluatie scan te starten met Azure PowerShell of rest API.
1. Controleer of de toewijzings parameters en het toewijzings bereik correct zijn ingesteld.
1. Controleer de [beleids definitie modus](../concepts/definition-structure.md#mode):
   - Modus all voor alle resource typen.
   - Modus ' geïndexeerd ' als de beleids definitie controleert op Tags of locatie.
1. Controleer of het bereik van de resource niet wordt [uitgesloten](../concepts/assignment-structure.md#excluded-scopes) of [uitgezonderd](../concepts/exemption-structure.md).
1. Als naleving voor een beleids toewijzing `0/0` bronnen bevat, zijn er geen resources vastgesteld die van toepassing zijn binnen het toewijzings bereik. Controleer de beleids definitie en het toewijzings bereik.
1. Voor een niet-compatibele resource die wordt verwacht te voldoen, controleert u de [redenen voor niet-naleving](../how-to/determine-non-compliance.md). De vergelijking van de definitie voor de waarde van de geëvalueerde eigenschap geeft aan waarom een resource niet aan het beleid voldoet.
   - Als de **doel waarde** onjuist is, wijzigt u de beleids definitie.
   - Als de **huidige waarde** onjuist is, valideert u de resource-Payload tot en met `resources.azure.com` .
1. Controle [problemen oplossen: afdwingen niet zoals verwacht](#scenario-enforcement-not-as-expected) voor andere veelvoorkomende problemen en oplossingen.

Als u nog steeds een probleem met uw gedupliceerde en aangepaste ingebouwde beleids definitie of aangepaste definitie hebt, maakt u een ondersteunings ticket onder het **ontwerpen van een beleid** om het probleem correct te routeren.

### <a name="scenario-enforcement-not-as-expected"></a>Scenario: afdwinging is niet zoals verwacht

#### <a name="issue"></a>Probleem

Een resource waarvan wordt verwacht dat deze wordt uitgevoerd door Azure Policy is niet en er is geen vermelding in het [Azure-activiteiten logboek](../../../azure-monitor/platform/platform-logs-overview.md).

#### <a name="cause"></a>Oorzaak

De beleids toewijzing is geconfigureerd voor [enforcementMode](../concepts/assignment-structure.md#enforcement-mode) van _disabled_. Terwijl de afdwingings modus is uitgeschakeld, wordt het beleids effect niet afgedwongen en is er geen vermelding in het activiteiten logboek.

#### <a name="resolution"></a>Oplossing

Voer de volgende stappen uit om het afdwingen van uw beleids toewijzing op te lossen:

1. Wacht eerst de juiste hoeveelheid tijd om een evaluatie uit te voeren en de resultaten van naleving beschikbaar te stellen in Azure Portal of SDK. Zie [evaluatie scan op aanvraag](../how-to/get-compliance-data.md#on-demand-evaluation-scan)om een nieuwe evaluatie scan te starten met Azure PowerShell of rest API.
1. Controleer of de toewijzings parameters en het toewijzings bereik correct zijn ingesteld en of **enforcementMode** is _ingeschakeld_. 
1. Controleer de [beleids definitie modus](../concepts/definition-structure.md#mode):
   - Modus all voor alle resource typen.
   - Modus ' geïndexeerd ' als de beleids definitie controleert op Tags of locatie.
1. Controleer of het bereik van de resource niet wordt [uitgesloten](../concepts/assignment-structure.md#excluded-scopes) of [uitgezonderd](../concepts/exemption-structure.md).
1. Controleer of de resource lading overeenkomt met de beleids logica. U kunt dit doen door [een har-tracering](../../../azure-portal/capture-browser-trace.md) vast te leggen of door de eigenschappen van de arm-sjabloon te controleren.
1. Controle [problemen oplossen: naleving niet zoals verwacht](#scenario-compliance-not-as-expected) voor andere veelvoorkomende problemen en oplossingen.

Als u nog steeds een probleem met uw gedupliceerde en aangepaste ingebouwde beleids definitie of aangepaste definitie hebt, maakt u een ondersteunings ticket onder het **ontwerpen van een beleid** om het probleem correct te routeren.

### <a name="scenario-denied-by-azure-policy"></a>Scenario: geweigerd door Azure Policy

#### <a name="issue"></a>Probleem

Het maken of bijwerken van een resource wordt geweigerd.

#### <a name="cause"></a>Oorzaak

Een beleids toewijzing aan het bereik dat door de nieuwe of bijgewerkte resource wordt gebruikt, voldoet aan de criteria van een beleids definitie met een [weigerings](../concepts/effects.md#deny) effect. Resources die deze definities kunnen vergaderingen, kunnen niet worden gemaakt of bijgewerkt.

#### <a name="resolution"></a>Oplossing

Het fout bericht van een beleids toewijzing voor weigeren omvat de beleids definitie en beleids toewijzing-Id's. Als de fout informatie in het bericht ontbreekt, is het ook beschikbaar in het [activiteiten logboek](../../../azure-monitor/platform/activity-log.md#view-the-activity-log). Gebruik deze informatie om meer te weten te komen over de resource beperkingen en pas de bron eigenschappen in uw aanvraag aan om de toegestane waarden te vergelijken.

## <a name="template-errors"></a>Sjabloon fouten

### <a name="scenario-policy-supported-functions-processed-by-template"></a>Scenario: door beleid ondersteunde functies die worden verwerkt door de sjabloon

#### <a name="issue"></a>Probleem

Azure Policy ondersteunt een aantal Azure Resource Manager sjablonen (ARM-sjablonen) en functies die alleen beschikbaar zijn in een beleids definitie. Resource Manager verwerkt deze functies als onderdeel van een implementatie in plaats van als onderdeel van een beleids definitie.

#### <a name="cause"></a>Oorzaak

Het gebruik van ondersteunde functies, zoals `parameter()` of `resourceGroup()` , resulteert in het verwerkte resultaat van de functie tijdens de implementatie in plaats van de functie voor de beleids definitie en de Azure Policy engine te laten verwerken.

#### <a name="resolution"></a>Oplossing

Als u wilt door gaan met een functie die deel uitmaakt van een beleids definitie, moet u de volledige teken reeks met `[` de betreffende eigenschap uitzoeken `[[resourceGroup().tags.myTag]` . Het escape teken heeft tot gevolg dat Resource Manager de waarde als een teken reeks behandelt bij het verwerken van de sjabloon. Azure Policy wordt de functie vervolgens in de beleids definitie geplaatst, zodat deze dynamisch naar verwachting kan worden uitgevoerd. Zie [syntaxis en expressies in azure Resource Manager-sjablonen](../../../azure-resource-manager/templates/template-expressions.md)voor meer informatie.

## <a name="add-on-installation-errors"></a>Fouten bij de installatie van invoeg toepassingen

### <a name="scenario-install-using-helm-chart-fails-on-password"></a>Scenario: installeren met behulp van helm-grafiek mislukt bij wacht woord

#### <a name="issue"></a>Probleem

De `helm install azure-policy-addon` opdracht mislukt met een van de volgende berichten:

- `!: event not found`
- `Error: failed parsing --set data: key "<key>" has no value (cannot end with ,)`

#### <a name="cause"></a>Oorzaak

Het gegenereerde wacht woord bevat een komma ( `,` ) waarin helm-grafiek wordt gesplitst.

#### <a name="resolution"></a>Oplossing

Escape de komma ( `,` ) in de wachtwoord waarde wanneer deze wordt uitgevoerd `helm install azure-policy-addon` met een back slash ( `\` ).

### <a name="scenario-install-using-helm-chart-fails-as-name-already-exists"></a>Scenario: de installatie met behulp van de helm-grafiek mislukt, omdat er al een naam bestaat

#### <a name="issue"></a>Probleem

De `helm install azure-policy-addon` opdracht mislukt met het volgende bericht:

- `Error: cannot re-use a name that is still in use`

#### <a name="cause"></a>Oorzaak

Het helm-diagram met de naam `azure-policy-addon` is al geïnstalleerd of gedeeltelijk geïnstalleerd.

#### <a name="resolution"></a>Oplossing

Volg de instructies om [de Azure Policy voor de invoeg toepassing Kubernetes te verwijderen](../concepts/policy-for-kubernetes.md#remove-the-add-on)en voer de `helm install azure-policy-addon` opdracht opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

- Krijg antwoorden van experts via [micro soft Q&A](/answers/topics/azure-policy.html).
- Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport) – het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.
- Als u meer hulp nodig hebt, kunt u een ondersteunings incident voor Azure opslaan. Ga naar de [ondersteunings site van Azure](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning verkrijgen**.
