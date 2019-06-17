---
title: Veelgestelde vragen over Azure traffic analytics | Microsoft Docs
description: Krijg antwoorden op enkele veelgestelde vragen over traffic analytics.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: kumud
ms.openlocfilehash: e4e9ef4f3a50aeac4db4d2cc2f2b6cbafcc47268
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051638"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Veelgestelde vragen over Traffic Analytics

In dit artikel verzamelt op één plek veel van de meest gestelde vragen over traffic analytics in Azure Network Watcher.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Wat zijn de vereisten voor het gebruik van traffic analytics?

Traffic Analytics is het volgende vereist:

- Een abonnement Network Watcher is ingeschakeld.
- Stroomlogboeken van Netwerkbeveiligingsgroep (NSG) ingeschakeld voor de nsg's die u wilt bewaken.
- Een Azure Storage-account voor het opslaan van Logboeken van de onbewerkte stroom.
- Een Azure Log Analytics-werkruimte met lees- en schrijftoegang.

Uw account moet voldoen aan een van de volgende verkeersanalyse inschakelen:

- Uw account moet beschikken over een van de volgende rollen voor toegang op rollen gebaseerd beheer (RBAC) op het abonnementsbereik: eigenaar, bijdrager, lezer of Inzender voor netwerken.
- Als uw account niet aan een van de eerder vermelde rollen toegewezen is, moet deze worden toegewezen aan een aangepaste rol die de volgende acties uit, op het abonnementsniveau van het is toegewezen.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Om te controleren of de rol is toegewezen aan een gebruiker voor een abonnement:

1. Aanmelden bij Azure met behulp van **aanmelding AzAccount**. 

2. Selecteer het abonnement dat vereist met behulp van **Selecteer AzSubscription**. 

3. U kunt alle functies die zijn toegewezen aan een opgegeven gebruiker gebruiken **Get-AzRoleAssignment - SignInName [e-mailadres gebruiker] - IncludeClassicAdministrators**. 

Als u geen uitvoer niet ziet, moet u contact op met de beheerder van het betreffende abonnement voor toegang tot de opdrachten worden uitgevoerd. Zie voor meer informatie, [op rollen gebaseerd toegangsbeheer met Azure PowerShell beheren](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-is-traffic-analytics-available"></a>In welke Azure-regio's is de Verkeersanalyse beschikbaar?

U kunt traffic analytics gebruiken voor nsg's in een van de volgende ondersteunde regio's:
- Canada - midden
- US - west-centraal
- US - oost
- US - oost 2
- US - noord-centraal
- US - zuid-centraal
- US - centraal
- US - west
- US - west 2
- Frankrijk - centraal
- Europa -west
- Europa - noord
- Brazilië - zuid
- Verenigd Koninkrijk West
- Verenigd Koninkrijk Zuid
- Australië - oost
- Australië - zuidoost 
- Azië - oost
- Azië - zuidoost
- Korea - centraal
- India - centraal
- India - zuid
- Japan - oost
- Japan - west
- VS (overheid) - Virginia

De Log Analytics-werkruimte moet bestaan in de volgende regio's:
- Canada - midden
- US - west-centraal
- US - west
- US - west 2
- US - zuid-centraal
- US - centraal
- US - oost
- US - oost 2
- Frankrijk - centraal
- Europa -west
- Europa - noord
- Verenigd Koninkrijk Zuid
- Australië - oost
- Australië - zuidoost
- Azië - oost
- Azië - zuidoost 
- Korea - centraal
- India - centraal
- Japan - oost
- VS (overheid) - Virginia

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Kan de nsg's ik stroom inschakelen Logboeken voor zich in verschillende regio's dan mijn werkruimte?

Ja, deze nsg's kunnen zich in verschillende regio's dan uw Log Analytics-werkruimte.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Kunnen meerdere nsg's worden geconfigureerd in één werkruimte?

Ja.

## <a name="can-i-use-an-existing-workspace"></a>Kan ik een bestaande werkruimte gebruiken?

Ja. Als u een bestaande werkruimte selecteert, zorg ervoor dat deze is gemigreerd naar de nieuwe querytaal. Als u niet bijwerken van de werkruimte wilt, moet u een nieuwe maken. Zie voor meer informatie over de nieuwe querytaal [Azure Monitor upgrade naar nieuwe zoekopdrachten in logboeken](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-log-analytics-workspace-be-in-a-different-subscription"></a>Kan mijn Azure-Opslagaccount zich in één abonnement en de Log Analytics-werkruimte zich in een ander abonnement?

Ja, uw Azure Storage-account kan worden in één abonnement en uw Log Analytics-werkruimte kan zich in een ander abonnement.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Kan ik onbewerkte Logboeken opslaan in een ander abonnement?

Nee. U kunt de onbewerkte Logboeken opslaan in een storage-account waarop een NSG-stroomlogboeken is ingeschakeld. Zowel de storage-account en de onbewerkte logboekbestanden moeten worden in hetzelfde abonnement en dezelfde regio.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>Wat gebeurt er als ik kan een NSG voor traffic analytics vanwege een fout 'Niet gevonden' niet configureren?

Selecteer een ondersteunde regio. Als u een niet-ondersteunde regio selecteert, ontvangt u een foutbericht 'Niet gevonden'. De ondersteunde regio's worden eerder in dit artikel vermeld.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>Wat gebeurt er als ik krijg de status 'kan niet laden,' onder de NSG stroom logboeken pagina?

De Microsoft.Insights-provider moet worden geregistreerd voor flow logboekregistratie voor een goede werking. Als u niet zeker weet of de Microsoft.Insights-provider is geregistreerd voor uw abonnement, Vervang *xxxxx-xxxxx-xxxxxx-xxxx* in de volgende opdracht uit en voer de volgende opdrachten vanuit PowerShell:

```powershell-interactive
**Select-AzSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Heb ik hebt de oplossing geconfigureerd. Waarom zie ik iets op het dashboard?

Het dashboard kan tot 30 minuten worden weergegeven voor het eerst duren. De oplossing moet voldoende gegevens voor het afleiden van betekenisvolle inzichten eerst samenvoegen. Vervolgens worden rapporten gegenereerd. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Wat gebeurt er als ik dit bericht ontvangt: "We kan niet geen gegevens vinden in deze werkruimte voor het geselecteerde tijdsinterval. Wijzig het tijdsinterval of Selecteer een andere werkruimte. "?

Probeer de volgende opties:
- Het tijdsinterval in de bovenste balk wijzigen.
- Selecteer een andere Log Analytics-werkruimte in de bovenste balk.
- Probeer toegang tot verkeersanalyse na 30 minuten als deze onlangs is ingeschakeld.
    
Als de problemen zich blijven voordoen, verhoogt u opmerkingen kunnen in de [-gebruikersforum stem](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Wat gebeurt er als ik dit bericht ontvangt: "Het analyseren van uw NSG-stroomlogboeken voor de eerste keer. Dit proces kan 20-30 minuten duren. Controleer later opnieuw. 2) als de bovenstaande stap niet werkt en uw werkruimte valt onder de gratis SKU, controleert u uw werkruimte Gebruik hier als u wilt valideren overschreden, Raadpleeg anders de clusterdocumentatie Veelgestelde vragen voor meer informatie. '?

U ziet dit bericht omdat:
- Traffic Analytics onlangs is ingeschakeld en kan niet nog samengevoegde hebben voldoende gegevens voor het afleiden van betekenisvolle inzichten.
- Gebruik de gratis versie van de Log Analytics-werkruimte en de quotumlimiet is overschreden. Mogelijk moet u een werkruimte gebruiken met een grotere capaciteit.
    
Als de problemen zich blijven voordoen, verhoogt u opmerkingen kunnen in de [-gebruikersforum stem](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Wat gebeurt er als ik dit bericht ontvangt: "Het lijkt er zijn gegevens van resources (topologie) beschikbaar en er is geen informatie stromen. In de tussentijd zorgen, klik hier om te zien van de gegevens van resources en Raadpleeg Veelgestelde vragen voor meer informatie. '?

U ziet de gegevens van resources op het dashboard; Er zijn echter geen statistieken met betrekking tot flow aanwezig zijn. Vanwege geen stromen communicatie tussen de bronnen kan geen gegevens aanwezig zijn. Wacht gedurende 60 minuten en de status controleren. Als het probleem zich blijft voordoen, en u zeker weet dat er communicatie stromen tussen bronnen zijn, verhoogt u opmerkingen kunnen in de [-gebruikersforum stem](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>Kan ik traffic analytics met behulp van PowerShell configureren of een Azure Resource Manager-sjabloon of de client?

U kunt traffic analytics configureren met behulp van Windows PowerShell versie 6.2.1 en hoger. Zie configureren van stroomlogboeken en verkeersanalyse voor een specifieke NSG met behulp van de cmdlet Set [Set AzNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkwatcherconfigflowlog). Als u de stroomlogboeken en de status van traffic analytics voor een specifieke NSG, Zie [Get-AzNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkwatcherflowlogstatus).

Op dit moment niet u een Azure Resource Manager-sjabloon gebruiken voor het configureren van traffic analytics.

Zie de volgende voorbeelden verkeersanalyse configureren met behulp van een Azure Resource Manager-client.

**Cmdlet Set-voorbeeld:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Voorbeeld van de cmdlet ophalen:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```


## <a name="how-is-traffic-analytics-priced"></a>Hoe is de Verkeersanalyse geprijsd?

Traffic Analytics wordt gemeten. De meting is gebaseerd op de verwerking van logboekgegevens van de stroom door de service en opslaan van de resulterende uitgebreide Logboeken in Log Analytics-werkruimte. 

Bijvoorbeeld, volgens de [prijsplan](https://azure.microsoft.com/pricing/details/network-watcher/), de regio West-Centraal VS, overwegen als gegevens die zijn opgeslagen in een storage-account dat is verwerkt door Traffic Analytics stroomlogboeken is 10 GB en uitgebreide logboeken die zijn opgenomen in Log Analytics-werkruimte is 1 GB en vervolgens de kosten van toepassing zijn: 10 x 2.3$ + 1 x 2.76$ 25.76 = $

## <a name="how-frequently-does-traffic-analytics-process-data"></a>Hoe vaak worden gegevens verwerkt met Traffic Analytics?

Raadpleeg de [aggregatie gegevenssectie](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-schema#data-aggregation) in Traffic Analytics Schema en gegevens aggregatie Document

## <a name="how-does-traffic-analytics-decide-that-an-ip-is-malicious"></a>Hoe Traffic Analytics bepaalt dat een IP-adres schadelijk is? 

Traffic Analytics is afhankelijk van Microsoft interne threat intelligence systemen om een IP-adres als schadelijke achten. Deze systemen-veel intelligence boven op het bouwen en gebruiken van diverse telemetrie-bronnen, zoals Microsoft-producten en services, de Microsoft Digital Crimes Unit (DCU), de Microsoft Security Response Center (MSRC) en externe feeds. Sommige van deze gegevens is Microsoft intern. Als een bekende IP-adres is ophalen gemarkeerd als malicios, Geef een ondersteuningsticket om te weten te verhogen.

## <a name="how-can-i-set-alerts-on-traffic-analytics-data"></a>Hoe kan ik waarschuwingen instellen voor Traffic Analytics-gegevens?

Traffic Analytics heeft geen ingebouwde ondersteuning voor waarschuwingen. Echter, omdat Traffic Analytics-gegevens worden opgeslagen in Log Analytics u kunt aangepaste query's schrijven en waarschuwingen instellen voor deze. Stappen:
- Voor Log Analytics in Traffic Analytics kunt u de korte URL. 
- Gebruik de [schema hier beschreven](traffic-analytics-schema.md) om uw query's schrijven 
- Klik op 'Nieuwe waarschuwingsregel' om de waarschuwing te maken
- Raadpleeg [log waarschuwingen documentatie](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) om de waarschuwing te maken

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Hoe kan ik Navigeer met behulp van het toetsenbord in de geografische kaartweergave?

De pagina van de kaart geo bevat twee hoofdsecties:
    
- **Banner**: De banner aan de bovenkant van de geografische kaart voorziet in knoppen voor het verkeer Distributiefilters (bijvoorbeeld, implementatie, verkeer uit landen/regio's en kwaadaardig) selecteren. Wanneer u een knop selecteert, wordt het betreffende filter wordt toegepast op de kaart. Als u de actieve knop selecteert, wordt de kaart de actieve datacenters in uw implementatie.
- **Kaart**: Onder de banner bevat de sectie map distributie van verkeer tussen Azure-datacenters en landen/regio's.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- De selectie op de pagina van de kaart geo voor de koptekst is standaard het filter 'Azure DC's '.
- Als u wilt verplaatsen naar een ander filter, gebruiken de `Tab` of de `Right arrow` sleutel. Als u wilt verplaatsen naar achteren, gebruikt u een de `Shift+Tab` of de `Left arrow` sleutel. Doorsturen navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` of de `Down` pijl aan het geselecteerde filter toepassen. Op basis van filterselectie en implementatie, worden een of meer knooppunten onder de sectie map gemarkeerd.
- Als u wilt overschakelen tussen koptekst en een kaart, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Toetsenbordnavigatie op de kaart
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de geselecteerde knooppunten (**Azure-datacenter** of **land/regio**) in de kaartweergave.
- Als u wilt verplaatsen naar andere knooppunten gemarkeerd op de kaart, gebruikt u een `Tab` of de `Right arrow` clientsleutel voor het doorsturen van verkeer. Gebruik `Shift+Tab` of de `Left arrow` voor achterwaartse verplaatsen.
- Als u wilt een gemarkeerde knooppunt in de kaart selecteert, gebruikt u de `Enter` of `Down arrow` sleutel.
- De selectie van deze knooppunten in focus naar de **hulpprogramma informatievenster** voor het knooppunt. Standaard focus verplaatsen naar de gesloten knop aan de **hulpprogramma informatievenster**. Verder verplaatsen binnen de **vak** weergeven, gebruikt u `Right arrow` en `Left arrow` verplaatsen vooruit en achteruit, respectievelijk. Drukken `Enter` heeft hetzelfde effect als wanneer de gerichte knop in de **hulpprogramma informatievenster**.
- Wanneer u drukt u op `Tab` terwijl de focus zich op de **hulpprogramma informatievenster**, de focus naar de eindpunten in het hetzelfde continent als het geselecteerde knooppunt. Gebruik de `Right arrow` en `Left arrow` verplaatsen via deze eindpunten.
- Als u wilt verplaatsen naar andere flow-eindpunten of continent clusters, gebruikt u `Tab` voor doorsturen van verkeer en `Shift+Tab` voor achterwaartse verplaatsen.
- Wanneer de focus is ingesteld op **Continent clusters**, gebruikt u de `Enter` of `Down` pijltoetsen om te markeren van de eindpunten in het cluster continent. Als u wilt verplaatsen door middel van eindpunten en de knop sluiten op het informatievak van het cluster continent, gebruiken de `Right arrow` of `Left arrow` sleutel voor verkeer, alleen vooruit en achteruit, respectievelijk. Op elk willekeurig eindpunt, kunt u `Shift+L` overschakelen naar de regel voor verbinding van het geselecteerde knooppunt naar het eindpunt. U kunt ook op `Shift+L` opnieuw te verplaatsen naar het eindpunt van de geselecteerde.
        
### <a name="keyboard-navigation-at-any-stage"></a>Toetsenbordnavigatie tijdens elke fase
    
- `Esc` de uitgebreide selectie samengevouwen.
- De `Up arrow` sleutel voert dezelfde actie als `Esc`. De `Down arrow` sleutel voert dezelfde actie als `Enter`.
- Gebruik `Shift+Plus` om in te zoomen, en `Shift+Minus` om uit te zoomen.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Hoe kan ik Navigeer met behulp van het toetsenbord in de weergave van de topologie virtueel netwerk?

De pagina met virtuele netwerken topologie bevat twee hoofdsecties:
    
- **Banner**: De banner aan de bovenkant van de virtuele netwerken-topologie voorziet in knoppen voor het verkeer Distributiefilters (bijvoorbeeld, verbonden virtuele netwerken, niet-verbonden virtuele netwerken en openbare IP-adressen) selecteren. Wanneer u een knop selecteert, wordt het betreffende filter wordt toegepast op de topologie. Als u de actieve knop selecteert, wordt de topologie van de actieve virtuele netwerken in uw implementatie.
- **Topologie**: Onder de banner bevat de sectie topologie distributie van verkeer tussen virtuele netwerken.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- De selectie op de pagina van de topologie virtuele netwerken voor de koptekst is standaard het filter 'Verbonden VNets'.
- Als u wilt verplaatsen naar een ander filter, gebruikt u de `Tab` toets om door te gaan. Om naar achteren verplaatsen, gebruikt u de `Shift+Tab` sleutel. Doorsturen navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` naar het geselecteerde filter toepassen. Op basis van de filterselectie en implementatie, zijn een of meer knooppunten (virtueel netwerk) onder de sectie topologie gemarkeerd.
- Als u wilt schakelen tussen de banner en de topologie, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Toetsenbordnavigatie op de topologie
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de geselecteerde knooppunten (**VNet**) in de Topologieweergave.
- Als u wilt verplaatsen naar andere gemarkeerde knooppunten in de Topologieweergave, gebruikt u de `Shift+Right arrow` clientsleutel voor het doorsturen van verkeer. 
- Op de gemarkeerde knooppunten focus naar de **hulpprogramma informatievenster** voor het knooppunt. Standaard focus naar de **meer details** knop op de **hulpprogramma informatievenster**. Verder verplaatsen binnen de **vak** weergeven, gebruikt u de `Right arrow` en `Left arrow` verplaatsen vooruit en achteruit, respectievelijk. Drukken `Enter` heeft hetzelfde effect als wanneer de gerichte knop in de **hulpprogramma informatievenster**.
- Op de selectie van deze knooppunten, vindt u alle bijbehorende verbindingen, één voor één, door te drukken de `Shift+Left arrow` sleutel. Focus naar de **hulpprogramma informatievenster** van die verbinding. Op elk gewenst moment de focus kan worden verplaatst naar het knooppunt door te drukken `Shift+Right arrow` opnieuw.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Hoe kan ik Navigeer met behulp van het toetsenbord in de weergave van de topologie subnet?

De pagina van de topologie virtuele subnetwerken bevat twee hoofdsecties:
    
- **Banner**: De banner aan de bovenkant van de topologie virtuele subnetwerken voorziet in knoppen voor het verkeer distributie-filters (bijvoorbeeld Active, gemiddeld en Gateway-subnetten) selecteren. Wanneer u een knop selecteert, wordt het betreffende filter wordt toegepast op de topologie. Als u de actieve knop selecteert, wordt de topologie van de actieve virtuele subnet in uw implementatie.
- **Topologie**: Onder de banner wordt in de sectie topologie distributie van verkeer tussen virtuele subnetwerken bevat.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- De selectie op de pagina van de topologie virtuele subnetwerken voor de koptekst is standaard het filter 'Subnetten'.
- Als u wilt verplaatsen naar een ander filter, gebruikt u de `Tab` toets om door te gaan. Om naar achteren verplaatsen, gebruikt u de `Shift+Tab` sleutel. Doorsturen navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` naar het geselecteerde filter toepassen. Op basis van filterselectie en implementatie, zijn een of meer knooppunten (Subnet) onder de sectie topologie gemarkeerd.
- Als u wilt schakelen tussen de banner en de topologie, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Toetsenbordnavigatie op de topologie
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de geselecteerde knooppunten (**Subnet**) in de Topologieweergave.
- Als u wilt verplaatsen naar andere gemarkeerde knooppunten in de Topologieweergave, gebruikt u de `Shift+Right arrow` clientsleutel voor het doorsturen van verkeer. 
- Op de gemarkeerde knooppunten focus naar de **hulpprogramma informatievenster** voor het knooppunt. Standaard focus naar de **meer details** knop op de **hulpprogramma informatievenster**. Verder verplaatsen binnen de **vak** weergeven, gebruikt u `Right arrow` en `Left arrow` verplaatsen vooruit en achteruit, respectievelijk. Drukken `Enter` heeft hetzelfde effect als wanneer de gerichte knop in de **hulpprogramma informatievenster**.
- Op de selectie van deze knooppunten, vindt u alle bijbehorende verbindingen, één voor één, door te drukken `Shift+Left arrow` sleutel. Focus naar de **hulpprogramma informatievenster** van die verbinding. Op elk gewenst moment de focus kan worden verplaatst naar het knooppunt door te drukken `Shift+Right arrow` opnieuw.    

