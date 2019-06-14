---
title: Azure Key Vault-oplossing in Azure Monitor | Microsoft Docs
description: U kunt de oplossing Azure Key Vault in Azure Monitor gebruiken om te controleren van Azure Key Vault-Logboeken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: bwren
ms.openlocfilehash: 481b643f2f7201a2a1745c7aef9ddd81883da020
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60498345"
---
# <a name="azure-key-vault-analytics-solution-in-azure-monitor"></a>Azure Key Vault-analyse-oplossing in Azure Monitor

![Key Vault-symbool](media/azure-key-vault/key-vault-analytics-symbol.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt de oplossing Azure Key Vault in Azure Monitor gebruiken om te controleren van Azure Key Vault AuditEvent-Logboeken.

Voor het gebruik van de oplossing, moet u logboekregistratie van diagnostische gegevens van Azure Key Vault inschakelen en aangeven dat de diagnostische gegevens naar Log Analytics-werkruimte. Het is niet die nodig zijn voor het schrijven van de logboeken naar Azure Blob storage.

> [!NOTE]
> In januari 2017, de ondersteunde manier van het verzenden van Logboeken van Key Vault naar Log Analytics gewijzigd. Als de Key Vault-oplossing die u gebruikt ziet *(afgeschaft)* in de titel, verwijzen naar [migreren van de oude Key Vault-oplossing](#migrating-from-the-old-key-vault-solution) voor stappen die u moet volgen.
>
>

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende instructies om te installeren en configureren van de oplossing Azure Key Vault:

1. Gebruik de procedure beschreven in [toevoegen Azure Monitor-oplossingen uit de galerie van oplossingen](../../azure-monitor/insights/solutions.md) om toe te voegen van de oplossing Azure Key Vault aan uw Log Analytics-werkruimte.
2. Diagnostische logboekregistratie inschakelen voor de Key Vault-resources te bewaken, met behulp van de [portal](#enable-key-vault-diagnostics-in-the-portal) of [PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>Key Vault diagnostische gegevens in de portal inschakelen

1. In de Azure-portal, gaat u naar de Key Vault-resource om te controleren
2. Selecteer *diagnostische instellingen* om de volgende pagina te openen

   ![afbeelding van Azure Key Vault-tegel](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics01.png)
3. Klik op *diagnostische gegevens inschakelen* om de volgende pagina te openen

   ![afbeelding van Azure Key Vault-tegel](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics02.png)
4. Geef een naam voor de diagnostische instelling.
5. Klik op het selectievakje voor *verzenden naar Log Analytics*
6. Selecteer een bestaande Log Analytics-werkruimte of maak een werkruimte
7. Om in te schakelen *AuditEvent* Logboeken, klikt u op het selectievakje onder Log
8. Klik op *opslaan* om in te schakelen van de logboekregistratie van diagnostische gegevens naar Log Analytics-werkruimte.

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Key Vault diagnostische gegevens met behulp van PowerShell inschakelen
De volgende PowerShell-script geeft een voorbeeld van hoe u `Set-AzDiagnosticSetting` Diagnostische logboekregistratie inschakelen voor Key Vault:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'

Set-AzDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Bekijk de details voor verzameling van Azure Key Vault-gegevens
Azure Key Vault-oplossing verzamelt logboeken met diagnostische gegevens rechtstreeks van de Key Vault.
Het is niet nodig om te schrijven van de logboeken naar Azure Blob storage en geen agent is vereist voor het verzamelen van gegevens.

De volgende tabel bevat de methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Azure Key Vault.

| Platform | Agent toewijzen | Systems Center Operations Manager-agent | Azure | Operations Manager vereist? | Operations Manager-agent gegevens verzonden via de beheergroep | Verzamelingsfrequentie |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | bij ontvangst |

## <a name="use-azure-key-vault"></a>Azure Key Vault gebruiken
Nadat u [installeren van de oplossing](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), de Key Vault-gegevens weergeven door te klikken op de **Key Vault-analyse** tegel van de Azure Monitor **overzicht** pagina. Open deze pagina van de **Azure Monitor** menu door te klikken op **meer** onder de **Insights** sectie. 

![afbeelding van Azure Key Vault-tegel](media/azure-key-vault/log-analytics-keyvault-tile.png)

Nadat u op de **Key Vault-analyse** herhalen, kunt u samenvattingen van uw logboekbestanden weergeven en vervolgens inzoomen op details voor de volgende categorieën:

* Volume van de key vault-bewerkingen worden na verloop van tijd
* Kan de bewerking volumes niet na verloop van tijd
* Gemiddelde operationele latentie per bewerking
* Quality of service voor bewerkingen met het aantal bewerkingen die meer dan 1000 ms nemen en een lijst met bewerkingen die meer dan 1000 ms

![afbeelding van Azure Key Vault-dashboard](media/azure-key-vault/log-analytics-keyvault01.png)

![afbeelding van Azure Key Vault-dashboard](media/azure-key-vault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Details van elke bewerking wilt weergeven
1. Op de **overzicht** pagina, klikt u op de **Key Vault-analyse** tegel.
2. Op de **Azure Key Vault** dashboard, Controleer de samenvattingsinformatie in een van de blades, en klik op een om gedetailleerde informatie over het in de zoekpagina logboek weer te geven.

    Op een van de log search pagina's, kunt u resultaten weergeven door de tijd, gedetailleerde resultaten en uw Logboekgeschiedenis zoeken. U kunt ook filteren op facetten om de resultaten te beperken.

## <a name="azure-monitor-log-records"></a>Azure Monitor log-records
De oplossing Azure Key Vault analyseert records met het type van **KeyVaults** die worden verzameld van [AuditEvent-logboeken](../../key-vault/key-vault-logging.md) in Azure Diagnostics.  Eigenschappen voor deze records zijn in de volgende tabel:  

| Eigenschap | Description |
|:--- |:--- |
| Type |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |IP-adres van de client die de aanvraag heeft ingediend |
| Category | *AuditEvent* |
| CorrelationId |Een optionele GUID die de client kan doorgeven om de logboeken aan de clientzijde te relateren aan (Sleutelkluis-)logboeken aan de servicezijde. |
| DurationMs |De tijd die nodig was om de REST-API-aanvraag af te handelen in milliseconden. Deze tijd omvat niet de netwerklatentie, zodat de tijd die u aan de clientzijde meet mogelijk niet overeenkomt met de tijd. |
| httpStatusCode_d |HTTP-statuscode is geretourneerd door de aanvraag (bijvoorbeeld *200*) |
| id_s |De unieke ID van de aanvraag |
| identity_claim_appid_g | GUID voor de toepassings-ID |
| OperationName |Naam van de bewerking, zoals beschreven in [logboekregistratie van Azure Key Vault](../../key-vault/key-vault-logging.md) |
| OperationVersion |REST-API-versie door de client aangevraagde (bijvoorbeeld *2015-06-01*) |
| requestUri_s |URI van de aanvraag |
| Resource |Naam van de key vault |
| ResourceGroup |Resourcegroep van de key vault |
| ResourceId |Azure Resource Manager-resource-id. Voor Sleutelkluis-Logboeken is dit de Key Vault-resource-ID. |
| ResourceProvider |*MICROSOFT.KEYVAULT* |
| ResourceType | *VAULTS* |
| resultSignature |HTTP-status (bijvoorbeeld *OK*) |
| ResultType |Resultaat van de REST-API-aanvraag (bijvoorbeeld *succes*) |
| SubscriptionId |Azure-abonnement-ID van het abonnement met de Key Vault |

## <a name="migrating-from-the-old-key-vault-solution"></a>Migreren van de oude Key Vault-oplossing
In januari 2017, de ondersteunde manier van het verzenden van Logboeken van Key Vault naar Log Analytics gewijzigd. Deze wijzigingen bieden de volgende voordelen:
+ Logboeken worden geschreven rechtstreeks naar een Log Analytics-werkruimte zonder de noodzaak voor het gebruik van een storage-account
+ Minder latentie vanaf het moment waarop de logboeken worden gegenereerd voor hen beschikbaar worden gesteld in Log Analytics
+ Zijn minder configuratiestappen
+ Een algemene indeling voor alle typen Azure diagnostics

De bijgewerkte oplossing gebruiken:

1. [Diagnostische gegevens rechtstreeks naar een Log Analytics-werkruimte worden verzonden vanuit Key Vault configureren](#enable-key-vault-diagnostics-in-the-portal)  
2. De oplossing Azure Key Vault inschakelen met behulp van de procedure beschreven in [toevoegen Azure Monitor-oplossingen uit de galerie van oplossingen](../../azure-monitor/insights/solutions.md)
3. Bijwerken van een opgeslagen query's, dashboards of waarschuwingen gebruik van het nieuwe gegevenstype
   + Type is een wijziging ten opzichte van: KeyVaults naar AzureDiagnostics. Het ResourceType kunt u filteren op Logboeken van Key Vault.
   + In plaats van: `KeyVaults`, gebruiken `AzureDiagnostics | where ResourceType'=="VAULTS"`
   + Velden: (Veldnamen zijn hoofdlettergevoelig)
   + Voor elk veld dat een achtervoegsel van \_s, \_d, of \_g in de naam wijzigen van het eerste teken in kleine letters
   + Voor elk veld dat een achtervoegsel van \_o in naam van de gegevens is opgesplitst in afzonderlijke velden op basis van de geneste veldnamen. Bijvoorbeeld, is de UPN van de aanroeper opgeslagen in een veld `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   + Veld CallerIpAddress gewijzigd in CallerIPAddress
   + Veld RemoteIPCountry is niet meer aanwezig
4. Verwijder de *Key Vault Analytics (afgeschaft)* oplossing. Als u met behulp van PowerShell, gebruikt u `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Gegevens die worden verzameld voordat de wijziging niet zichtbaar in de nieuwe oplossing is. U kunt zoeken naar deze gegevens met behulp van de oude Type en de veldnamen.

## <a name="troubleshooting"></a>Problemen oplossen
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Volgende stappen
* Gebruik [query's bijgehouden in Azure Monitor](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde Azure Key Vault-gegevens weer te geven.
