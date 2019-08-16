---
title: Azure-resources verplaatsen naar een nieuw abonnement of een nieuwe resource groep | Microsoft Docs
description: Azure Resource Manager gebruiken voor resources verplaatsen naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 53482fdd760517967c9a4a976b43b64ba745c637
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69542952"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Resources verplaatsen naar een nieuwe resource groep of een nieuw abonnement

In dit artikel wordt beschreven hoe u Azure-resources verplaatst naar een ander Azure-abonnement of een andere resource groep onder hetzelfde abonnement. U kunt de Azure Portal, Azure PowerShell, Azure CLI of de REST API gebruiken om resources te verplaatsen.

Zowel de bron groep als de doel groep worden tijdens de verplaatsings bewerking vergrendeld. Schrijf en verwijderbewerkingen op de brongroepen zijn geblokkeerd totdat de verplaatsing is voltooid. Deze vergrendeling betekent dat u kunt geen toevoegen, bijwerken of verwijderen van resources in de resourcegroepen, maar dit betekent niet dat de resources zijn geblokkeerd. Als u een SQL-Server en de bijbehorende database naar een nieuwe resourcegroep verplaatsen, er een toepassing die gebruikmaakt van de database zonder uitvaltijd. Dit kan nog steeds lezen en schrijven naar de database.

Als u een resource verplaatst, wordt deze alleen verplaatst naar een nieuwe resource groep of een nieuw abonnement. De locatie van de resource wordt niet gewijzigd.

## <a name="checklist-before-moving-resources"></a>Controlelijst voor het verplaatsen van resources

Voordat u een resource verplaatst, moeten er enkele belangrijke stappen worden uitgevoerd. U kunt fouten voorkomen door te controleren of aan de volgende voorwaarden is voldaan.

1. De resources die u wilt verplaatsen, moeten de verplaatsings bewerking ondersteunen. Zie [ondersteuning voor het verplaatsen van resources voor bronnen](move-support-resources.md)voor een lijst met resources die kunnen worden verplaatst.

1. Sommige services hebben specifieke beperkingen of vereisten bij het verplaatsen van resources. Als u een van de volgende services hebt verplaatst, controleert u of er richt lijnen zijn voordat u verdergaat.

   * [Hulp App Services verplaatsen](./move-limitations/app-service-move-limitations.md)
   * [Richt lijnen voor het verplaatsen van Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Klassiek implementatie model richt lijnen](./move-limitations/classic-model-move-limitations.md) voor het verplaatsen van klassieke compute, klassieke opslag, klassieke virtuele netwerken en Cloud Services
   * [Hulp Recovery Services verplaatsen](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Hulp Virtual Machines verplaatsen](./move-limitations/virtual-machines-move-limitations.md)
   * [Richt lijnen voor het verplaatsen van virtuele netwerken](./move-limitations/virtual-network-move-limitations.md)

1. De bron-en doel abonnementen moeten actief zijn. Als u problemen ondervindt bij het inschakelen van een uitgeschakeld account, [maakt u een ondersteunings aanvraag voor Azure](../azure-supportability/how-to-create-azure-support-request.md). Selecteer **Abonnementsbeheer** voor het probleemtype.

1. De bron- en doelabonnementen moeten aanwezig zijn in dezelfde [Azure Active Directory-tenant](../active-directory/develop/quickstart-create-new-tenant.md). Om te controleren dat beide abonnementen dezelfde tenant-ID hebben, gebruikt u Azure PowerShell of Azure CLI.

   Voor Azure PowerShell gebruiken:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Gebruik voor Azure CLI:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Als de tenant-id's voor de bron- en doelabonnementen niet hetzelfde zijn, moet u de volgende methoden gebruiken om af te stemmen van de tenant-id's:

   * [Eigendom van een Azure-abonnement naar een ander account overdragen](../billing/billing-subscription-transfer.md)
   * [Een Azure-abonnement koppelen of toevoegen aan Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Het doelabonnement moet zijn geregistreerd voor de resourceprovider van de resource die wordt verplaatst. Als u niet het geval is, ontvangt u een foutmelding waarin staat dat de **abonnement is niet geregistreerd voor een resourcetype**. Mogelijk ziet u deze fout wanneer u een resource verplaatst naar een nieuw abonnement, maar dat abonnement nooit is gebruikt voor dat resource type.

   Gebruik de volgende opdrachten om op te halen van de registratiestatus voor PowerShell:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Gebruik het volgende voor het registreren van een resourceprovider:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Gebruik de volgende opdrachten om op te halen van de status van de registratie voor Azure CLI:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Gebruik het volgende voor het registreren van een resourceprovider:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Het verplaatsen van de resources account moet ten minste de volgende machtigingen hebben:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** op de resourcegroep.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** op de doel-resourcegroep.

1. Controleer voordat u de resources verplaatst, de abonnementquota voor het abonnement dat u de resources wilt verplaatsen. Als het verplaatsen van de resources betekent dat het abonnement overschrijdt de grenzen, moet u om te controleren of u een verhoging van de quota kan opvragen. Zie voor een lijst van beperkingen en hoe u een verhoging [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).

1. **Voor een verplaatsing tussen abonnementen moeten de resource en de afhankelijke resources zich in dezelfde resource groep bevinden en moeten ze samen worden verplaatst.** Een virtuele machine met Managed disks vereist bijvoorbeeld dat de virtuele machine en de beheerde schijven samen worden verplaatst, samen met andere afhankelijke bronnen.

   Als u een resource naar een nieuw abonnement verplaatst, controleert u of de resource afhankelijke resources heeft en of ze zich in dezelfde resource groep bevinden. Als de resources zich niet in dezelfde resource groep bevinden, controleert u of de resources kunnen worden geconsolideerd in dezelfde resource groep. Als dit het geval is, kunt u al deze resources in dezelfde resource groep plaatsen met behulp van een Verplaats bewerking tussen resource groepen.
    
Zie [scenario voor verplaatsen tussen abonnementen](#scenario-for-move-across-subscriptions)voor meer informatie.

## <a name="scenario-for-move-across-subscriptions"></a>Scenario voor het verplaatsen van meerdere abonnementen
Het verplaatsen van resources van het ene naar het andere abonnement is een proces dat uit drie stappen bestaat:

![scenario voor het verplaatsen van meerdere abonnementen](./media/resource-group-move-resources/cross-subscription-move-scenario.png)

Voor illustratie doeleinden hebben we slechts één afhankelijke resource.

* Stap 1: Als afhankelijke resources over verschillende resource groepen worden gedistribueerd, moet u deze eerst verplaatsen naar één resource groep.
* Stap 2: Verplaats de resource en afhankelijke resources samen van het bron abonnement naar het doel abonnement.
* Stap 3: U kunt de afhankelijke resources eventueel opnieuw distribueren naar verschillende resource groepen binnen het doel abonnement. 

## <a name="validate-move"></a>Verplaatsen valideren

De [verplaatsingsbewerking valideren](/rest/api/resources/resources/validatemoveresources) kunt u het testen van uw scenario verplaatsen zonder daadwerkelijk de resources te verplaatsen. Gebruik deze bewerking om te controleren of de verplaatsing slaagt. Validatie wordt automatisch aangeroepen wanneer u een verplaatsings aanvraag verzendt. Gebruik deze bewerking alleen als u de resultaten vooraf moet bepalen. Als u wilt deze bewerking uitvoeren, moet u de:

* naam van de resourcegroep
* Resource-ID van de doelresourcegroep
* Resource-ID van elke resource verplaatsen
* de [toegangstoken](/rest/api/azure/#acquire-an-access-token) voor uw account

De volgende aanvraag verzenden:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

Met een aanvraagtekst:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Als de aanvraag is ingedeeld, retourneert de bewerking:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

De statuscode 202 geeft aan dat de validatieaanvraag is geaccepteerd, maar deze nog niet nog bepaald als de bewerking voor verplaatsen slaagt. De `location` waarde bevat een URL die u gebruikt om de status van de langdurige bewerking te controleren.  

Om te controleren of de status, de volgende aanvraag te verzenden:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

Terwijl de bewerking wordt nog steeds uitgevoerd, wordt u voor het ontvangen van de statuscode 202 blijven. Wacht het aantal seconden aangegeven in de `retry-after` waarde voordat u doorgaat. Als de verplaatsing is gevalideerd, ontvangt u de statuscode 204. Als de validatie voor het verplaatsen is mislukt, ontvangt u een foutbericht weergegeven, zoals:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Gebruik de portal

Als u resources wilt verplaatsen, selecteert u de resource groep met deze resources en selecteert u vervolgens de knop **verplaatsen** .

![resources verplaatsen](./media/resource-group-move-resources/select-move.png)

Selecteer of u de resources naar een nieuwe resourcegroep of een nieuw abonnement verplaatsen wilt.

Selecteer de resources wilt verplaatsen en de resourcegroep van de bestemming. Bevestig dat u wilt bijwerken scripts voor deze resources, en selecteer **OK**. Als u het pictogram bewerken abonnement in de vorige stap hebt geselecteerd, moet u ook het doelabonnement.

![doel selecteren](./media/resource-group-move-resources/select-destination.png)

In **meldingen**, ziet u dat de verplaatsing wordt uitgevoerd.

![move-status weergeven](./media/resource-group-move-resources/show-status.png)

Wanneer deze is voltooid, krijgt u een melding van het resultaat.

![Zet resultaten weergeven](./media/resource-group-move-resources/show-result.png)

Als er een fout optreedt, raadpleegt u [problemen met het verplaatsen van Azure-resources naar een nieuwe resource groep of een nieuw abonnement](troubleshoot-move.md).

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

Als u bestaande resources wilt verplaatsen naar een andere resource groep of een ander abonnement, gebruikt u de opdracht [Move-AzResource](/powershell/module/az.resources/move-azresource) . In het volgende voor beeld ziet u hoe u verschillende resources kunt verplaatsen naar een nieuwe resource groep.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Als u wilt verplaatsen naar een nieuw abonnement, een waarde bevatten voor de `DestinationSubscriptionId` parameter.

Als er een fout optreedt, raadpleegt u [problemen met het verplaatsen van Azure-resources naar een nieuwe resource groep of een nieuw abonnement](troubleshoot-move.md).

## <a name="use-azure-cli"></a>Azure CLI gebruiken

Als u bestaande resources naar een andere resourcegroep of abonnement, gebruikt u de [verplaatsen van de az resource](/cli/azure/resource?view=azure-cli-latest#az-resource-move) opdracht. Geef de resource-id's van de resources te verplaatsen. In het volgende voor beeld ziet u hoe u verschillende resources kunt verplaatsen naar een nieuwe resource groep. In de `--ids` parameter, Geef een door spaties gescheiden lijst van de resource-id's te verplaatsen.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Als u wilt verplaatsen naar een nieuw abonnement, bieden de `--destination-subscription-id` parameter.

Als er een fout optreedt, raadpleegt u [problemen met het verplaatsen van Azure-resources naar een nieuwe resource groep of een nieuw abonnement](troubleshoot-move.md).

## <a name="use-rest-api"></a>REST API gebruiken

Als u bestaande resources wilt verplaatsen naar een andere resource groep of een ander abonnement, gebruikt u de bewerking [resources verplaatsen](/rest/api/resources/Resources/MoveResources) .

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

In de hoofdtekst van de aanvraag geeft u de doelresourcegroep die is en de resources te verplaatsen.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Als er een fout optreedt, raadpleegt u [problemen met het verplaatsen van Azure-resources naar een nieuwe resource groep of een nieuw abonnement](troubleshoot-move.md).

## <a name="next-steps"></a>Volgende stappen

Zie [ondersteuning voor het verplaatsen van resources voor bronnen](move-support-resources.md)voor een lijst met resources die kunnen worden verplaatst.
