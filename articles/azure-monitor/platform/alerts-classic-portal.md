---
title: Maken, weergeven en beheren van klassieke metrische waarschuwingen met behulp van Azure Monitor
description: Informatie over het gebruik van Azure portal, CLI of Powershell maken, weergeven en beheren van klassieke waarschuwingsregels voor metrische gegevens.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: snmuvva
ms.openlocfilehash: 4a225dbc8e84d65a6ea25f63627599e5bb7d2ced
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129795"
---
# <a name="create-view-and-manage-classic-metric-alerts-using-azure-monitor"></a>Maken, weergeven en beheren van klassieke metrische waarschuwingen met behulp van Azure Monitor

Klassieke metrische waarschuwingen in Azure Monitor biedt een manier om te worden geïnformeerd wanneer een van de metrische gegevens een drempelwaarde overschrijden. Klassieke metrische waarschuwingen is een oudere functionaliteit waarmee waarschuwingen voor de niet-dimensionale metrische gegevens. Er is een bestaande nieuwere functionaliteit, metrische waarschuwingen die verbeterde functionaliteit voor klassieke metrische waarschuwingen heeft. U kunt meer informatie over de nieuwe functionaliteit metrische waarschuwingen in [metriek overzicht van waarschuwingen](../../azure-monitor/platform/alerts-metric-overview.md). In dit artikel wordt wordt beschreven hoe u maken, weergeven en beheren van klassieke waarschuwingsregels voor metrische gegevens via Azure portal, Azure CLI en Powershell.

## <a name="with-azure-portal"></a>Met Azure portal

1. In de [portal](https://portal.azure.com/), zoek de resource die u wilt bewaken, en selecteer dit.

2. In de **bewaking** sectie, selecteer **waarschuwingen (klassiek)** . De tekst en het pictogram kunnen enigszins verschillen voor verschillende resources. Als u niet kunt vinden **waarschuwingen (klassiek)** hier is het wellicht in **waarschuwingen** of **waarschuwingsregels**.

    ![Bewaking](media/alerts-classic-portal/AlertRulesButton.png)

3. Selecteer de **metrische waarschuwing toevoegen (klassiek)** opdracht in en vul de velden.

    ![Waarschuwing toevoegen](media/alerts-classic-portal/AddAlertOnlyParamsPage.png)

4. **Naam** de waarschuwingsregel. Kies vervolgens een **beschrijving**, die ook wordt weergegeven in e-mailmeldingen.

5. Selecteer de **Metric** die u wilt bewaken. Kies vervolgens een **voorwaarde** en **drempelwaarde** waarde voor de metrische gegevens. Ook voor kiezen de **periode** van de tijd waarop de regel voor metrische gegevens moet worden voldaan voordat de waarschuwing triggers. Bijvoorbeeld, als u de periode 'in de afgelopen 5 minuten' en de waarschuwing een CPU dan 80% zoekt, de waarschuwing wordt geactiveerd wanneer de CPU consistent bovenstaande 80% voor 5 minuten is. Na de eerste optreedt trigger, het opnieuw wordt geactiveerd wanneer de CPU voor 5 minuten blijft dan 80%. De meting van CPU-metrische gegevens gebeurt elke minuut.

6. Selecteer **e-eigenaren...**  als u wilt dat beheerders en medebeheerders e-mailmeldingen ontvangen wanneer de waarschuwing wordt geactiveerd.

7. Als u wilt om meldingen te verzenden naar extra e-mailadressen wanneer de waarschuwing wordt geactiveerd, voegt u ze op in de **beheerder email(s)** veld. Scheid meerdere e-mailberichten met puntkomma's, in de volgende indeling: *e\@contoso.com;email2\@contoso.com*

8. Plaats in een geldige URI in de **Webhook** veld als u wilt dat deze moet worden aangeroepen wanneer de waarschuwing wordt geactiveerd.

9. Als u Azure Automation gebruikt, kunt u een runbook om te worden uitgevoerd wanneer de waarschuwing wordt geactiveerd.

10. Selecteer **OK** om de waarschuwing te maken.

Binnen een paar minuten, wordt de waarschuwing is actief en wordt geactiveerd als eerder beschreven.

Nadat u een waarschuwing gemaakt, kunt u selecteert en voer een van de volgende taken:

* Een grafiek waarin de drempelwaarde voor metrische gegevens en de werkelijke waarden van de vorige dag weergeven.
* Bewerken of te verwijderen.
* **Uitschakelen** of **inschakelen** als u wilt tijdelijk stoppen of doorgaan met het ontvangen van meldingen voor deze waarschuwing.

## <a name="with-azure-cli"></a>Met Azure CLI

In de vorige secties beschreven hoe u kunt maken, weergeven en beheren van waarschuwingsregels voor metrische gegevens gebruik met Azure portal. In deze sectie wordt beschreven hoe u doet hetzelfde met platformonafhankelijke [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest). Snelste manier om te beginnen met behulp van Azure CLI is via [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).

### <a name="get-all-classic-metric-alert-rules-in-a-resource-group"></a>Alle klassieke waarschuwingsregels voor metrische gegevens in een resourcegroep ophalen

```azurecli
az monitor alert list --resource-group <group name>
```

### <a name="see-details-of-a-particular-classic-metric-alert-rule"></a>Zie de details van een bepaalde klassieke waarschuwingsregel voor metrische gegevens

```azurecli
az monitor alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-classic-metric-alert-rule"></a>Een klassieke waarschuwingsregel voor metrische gegevens maken

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-a-classic-metric-alert-rule"></a>Een klassieke waarschuwingsregel voor metrische gegevens verwijderen

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="with-powershell"></a>Met PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Deze secties ziet u hoe u PowerShell-opdrachten maken, weergeven en beheren van klassieke metrische waarschuwingen. De voorbeelden in het artikel ziet u hoe u Azure Monitor-cmdlets kunt gebruiken voor klassieke metrische waarschuwingen.

1. Als u niet hebt gedaan, kunt u instellen van PowerShell om uit te voeren op uw computer. Zie voor meer informatie, [installeren en configureren van PowerShell](/powershell/azure/overview). U kunt ook de volledige lijst met Azure Monitor PowerShell-cmdlets op bekijken [Cmdlets van Azure Monitor (inzichten)](https://docs.microsoft.com/powershell/module/az.applicationinsights).

2. Eerst, meld u aan bij uw Azure-abonnement.

    ```powershell
    Connect-AzAccount
    ```

3. Hier ziet u een aanmeldingsscherm. Nadat u uw Account, TenantID, zich aanmelden en standaard abonnements-ID worden weergegeven. Alle Azure-cmdlets werken in de context van uw standaardabonnement. U hebt toegang tot de volgende opdracht gebruiken om de lijst met abonnementen weer te geven:

    ```powershell
    Get-AzSubscription
    ```

4. Als u wilt uw context werken naar een ander abonnement wijzigen, gebruik de volgende opdracht:

    ```powershell
    Set-AzContext -SubscriptionId <subscriptionid>
    ```

5. U kunt alle klassieke waarschuwingsregels voor metrische gegevens op een resourcegroep ophalen:

    ```powershell
    Get-AzAlertRule -ResourceGroup montest
    ```

6. U kunt details van een klassieke waarschuwingsregel voor metrische gegevens weergeven

    ```powershell
    Get-AzAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
    ```

7. U kunt alle regels voor waarschuwingen instellen voor een doelresource ophalen. Bijvoorbeeld, instellen alle waarschuwingsregels op een virtuele machine.

    ```powershell
    Get-AzAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
    ```

8. Regels voor klassieke waarschuwingen kunnen niet meer worden gemaakt via PowerShell. Het maken van een waarschuwingsregel die u wilt gebruikmaken van de nieuwe ['Add-AzMetricAlertRule'](/powershell/module/az.monitor/add-azmetricalertrule) opdracht.

## <a name="next-steps"></a>Volgende stappen

- [Een klassieke metrische waarschuwing maken met een Resource Manager-sjabloon](../../azure-monitor/platform/alerts-enable-template.md).
- [Hebt u een klassieke waarschuwing voor metrische gegevens op de hoogte stellen een niet-Azure-systeem met behulp van een webhook](../../azure-monitor/platform/alerts-webhooks.md).
