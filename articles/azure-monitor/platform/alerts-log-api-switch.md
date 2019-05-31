---
title: Overschakelen van de verouderde Log Analytics waarschuwingen API in de nieuwe API voor Azure-waarschuwingen
description: Overzicht van verouderde savedSearch op basis van Log Analytics-waarschuwing API en proces als u wilt overschakelen van regels voor waarschuwingen naar nieuwe ScheduledQueryRules API met details van algemene problemen van klanten-adressering.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 0e8cb18b3ea4b01db6b373ebbcb55c1e17614319
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399153"
---
# <a name="switch-api-preference-for-log-alerts"></a>API-voorkeur voor logboekwaarschuwingen wijzigen

> [!NOTE]
> Inhoud vermeld van toepassing op gebruikers alleen Azure openbare cloud en **niet** voor Azure Government or Azure China-cloud.  

Tot voor kort beheerde u waarschuwingsregels in de Microsoft Operations Management Suite-portal. De nieuwe ervaring voor waarschuwingen is geïntegreerd met verschillende services in Microsoft Azure, inclusief Log Analytics en we gevraagd [uw regels voor waarschuwingen van OMS-portal uitbreiden naar Azure](alerts-extend.md). Maar om ervoor te zorgen minimale verstoring voor klanten, het proces heeft geen invloed op de programma-interface voor het verbruik - [Log Analytics-waarschuwing API](api-alerts.md) op basis van SavedSearch.

Maar nu u een echte Azure programmatische alternatief aankondigen voor Log Analytics waarschuwingen gebruikers [Azure Monitor - ScheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), dit is ook bij reflectieve in uw [Azure billing - voor logboekwaarschuwingen](alerts-unified-log.md#pricing-and-billing-of-log-alerts). Zie voor meer informatie over het beheren van uw waarschuwingen met behulp van de API, [waarschuwingen beheren met behulp van Azure-Resourcesjabloon](alerts-log.md#managing-log-alerts-using-azure-resource-template) en [waarschuwingen beheren met behulp van PowerShell](alerts-log.md#managing-log-alerts-using-powershell).

## <a name="benefits-of-switching-to-new-azure-api"></a>Voordelen van het overschakelen naar de nieuwe Azure-API

Er zijn enkele voordelen van het maken en beheren van waarschuwingen via [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) via [verouderde Log Analytics-waarschuwing API](api-alerts.md); er is een lijst enkele van de volgende belangrijkste oplossingen:

- Mogelijkheid om te [cross-werkruimte zoeken in logboeken](../log-query/cross-workspace-query.md) in regels voor waarschuwingen en het bereik van externe bronnen, zoals Log Analytics-werkruimten of zelfs Application Insights-apps
- Met behulp van meerdere velden die aan de groep in de query, [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) gebruiker kunt opgeven welk veld moet een statistische functie op in Azure portal
- Meld u waarschuwingen die zijn gemaakt met behulp van [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kunt hebben periode gedefinieerd tot 48 uur en ophalen van gegevens voor een langere periode dan voorheen
- Waarschuwingsregels maken in één keer als één resource zonder de noodzaak om te maken van drie niveaus van resources als [verouderde Log Analytics-waarschuwing API](api-alerts.md)
- Enkele programma-interface voor alle varianten van query's gebaseerde logboekwaarschuwingen in Azure - nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kan worden gebruikt voor het beheren van regels voor Log Analytics, evenals de Application Insights
- Beheren van uw waarschuwingen met behulp van [Powershell-cmdlets](alerts-log.md#managing-log-alerts-using-powershell)
- Alle waarschuwingen functionaliteit voor het nieuwe logboekbestanden en toekomstige ontwikkeling is beschikbaar via de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

## <a name="process-of-switching-from-legacy-log-alerts-api"></a>Proces van het overschakelen van verouderde Log-API voor waarschuwingen

Gebruikers zijn gratis voor [verouderde Log Analytics-waarschuwing API](api-alerts.md) of de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules). Waarschuwingsregels die zijn gemaakt door een van beide API, zal *worden beheerd door de dezelfde API alleen* - en als u vanuit de Azure-portal. Standaard Azure Monitor worden echter ook doorgaan met [verouderde Log Analytics-waarschuwing API](api-alerts.md) voor het maken van een nieuwe waarschuwingsregel vanuit Azure portal voor bestaande werkruimten van Log Analytics. Als [nieuwe Log-werkruimte gemaakt op of na 1 juni 2019 aangekondigd](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/) -automatisch gebruikmaken van nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) door standaard te nemen in Azure portal.

De gevolgen van de switch van voorkeur aan scheduledQueryRules API worden gecompileerd hieronder:

- Alle interacties gedaan voor het beheren van waarschuwingen via programmatische interfaces moet nu worden uitgevoerd met [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) in plaats daarvan. Voor meer informatie ziet, [voorbeeld gebruiken via Azure Resource-sjabloon](alerts-log.md#managing-log-alerts-using-azure-resource-template) en [voorbeeld gebruik via PowerShell](alerts-log.md#managing-log-alerts-using-powershell)
- Een nieuwe waarschuwingsregel hebt gemaakt in Azure portal worden gemaakt met [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen en gebruikers gebruiken de [aanvullende functionaliteit van de nieuwe API](#benefits-of-switching-to-new-azure-api) via Azure portal ook
- Ernst voor waarschuwingsregels verplaatst uit: *Kritiek, waarschuwing en ter informatie*naar *ernstwaarden 0, 1 en 2 van*. Samen met de optie voor regels voor waarschuwingen met urgentie 4 ook maken/bijwerken.

Het proces van servermodi waarschuwingsregels van [verouderde Log Analytics-waarschuwing API](api-alerts.md) heeft geen betrekking op de meldingsdefinitie, query of configuratie in geen enkele manier wijzigen. De waarschuwingsregels en worden niet beïnvloed en de waarschuwingen bewaken, worden niet gestopt of vastgelopen, tijdens of na de switch. De enige wijziging is een wijziging in de API voorkeur en toegang tot uw regels via een nieuwe API.

> [!NOTE]
> Wanneer een gebruiker kan worden gebruikt als u wilt overschakelen van de voorkeur aan de nieuwe [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules), wordt het niet mogelijk terug opt of teruggaan naar het gebruik van de oudere [verouderde Log Analytics-waarschuwing API](api-alerts.md).

Elke klant die wil vrijwillig overschakelen naar de nieuwe [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) en blokkeren van gebruik van de [verouderde Log Analytics-waarschuwing API](api-alerts.md); kunt dit doen door het uitvoeren van een PUT-aanroep op de onderstaande API om over te schakelen van alle waarschuwing regels die zijn gekoppeld aan de specifieke Log Analytics-werkruimte.

```
PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Met de aanvraag hoofdtekst die de onderstaande JSON.

```json
{
    "scheduledQueryRulesEnabled" : true
}
```

De API kan ook worden geopend vanuit een PowerShell-opdrachtregel met [ARMClient](https://github.com/projectkudu/ARMClient), een open-source-opdrachtregelprogramma dat vereenvoudigt het aanroepen van de Azure Resource Manager-API. Zoals hieronder wordt weergegeven, in de PUT-aanroep voorbeeld ARMclient hulpprogramma gebruiken om over te schakelen van alle regels voor waarschuwingen die zijn gekoppeld aan de specifieke Log Analytics-werkruimte.

```powershell
$switchJSON = '{"scheduledQueryRulesEnabled": "true"}'
armclient PUT /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $switchJSON
```

Als switch van alle regels voor waarschuwingen in Log Analytics-werkruimte gebruik van nieuwe [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) is geslaagd, het volgende antwoord wordt geleverd.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```

Gebruikers kunnen ook de huidige status van uw Log Analytics-werkruimte controleren en zien of deze is of is niet ingesteld voor het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen. Als u wilt controleren, kunnen gebruikers een GET-aanroep uitvoeren op de onderstaande API.

```
GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Het uitvoeren van de bovenstaande in met behulp van PowerShell-opdrachtregel met behulp [ARMClient](https://github.com/projectkudu/ARMClient) hulpprogramma, raadpleegt u het onderstaande voorbeeld.

```powershell
armclient GET /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Als de opgegeven Log Analytics-werkruimte is overgeschakeld naar het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen; vervolgens het antwoord JSON worden zoals hieronder vermeld.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : true
}
```
Anders, als de opgegeven Log Analytics-werkruimte nog niet is overgeschakeld voor het gebruik van [scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) alleen; vervolgens het antwoord JSON worden zoals hieronder vermeld.

```json
{
    "version": 2,
    "scheduledQueryRulesEnabled" : false
}
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Azure Monitor - waarschuwingen](alerts-unified-log.md).
- Meer informatie over het maken van [waarschuwingen voor activiteitenlogboeken in Azure-waarschuwingen](alerts-log.md).
- Meer informatie over de [ervaren Azure-waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
