---
title: Referenties beheren in Azure Automation
description: In dit artikel leest u hoe u referentie-assets maakt en hoe u deze kunt gebruiken in een runbook of DSC-configuratie.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 01/31/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 798da0261dbb692d411f1bb568a272a6bb7dbb03
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715661"
---
# <a name="manage-credentials-in-azure-automation"></a>Referenties beheren in Azure Automation

Een Automation-referentie-Asset bevat een object dat beveiligings referenties bevat, zoals een gebruikers naam en een wacht woord. Runbooks en DSC-configuraties gebruiken cmdlets waarmee een [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) -object voor verificatie wordt geaccepteerd. Het is ook mogelijk om de gebruikers naam en het wacht woord van het `PSCredential` object op te halen om aan te geven aan bepaalde toepassingen of services waarvoor verificatie is vereist. 

>[!NOTE]
>Beveilig assets in Azure Automation referenties, certificaten, verbindingen en versleutelde variabelen bevatten. Deze assets worden versleuteld en opgeslagen in Azure Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk Automation-account. Azure Automation slaat de sleutel op in de door het systeem beheerde Key Vault. Voordat u een beveiligde Asset opslaat, laadt Automation de sleutel van Key Vault en gebruikt deze om de Asset te versleutelen. 

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="powershell-cmdlets-used-to-access-credentials"></a>Power shell-cmdlets die worden gebruikt voor toegang tot referenties

Met de cmdlets in de volgende tabel worden Automation-referenties met Power shell gemaakt en beheerd. Ze worden geleverd als onderdeel van de [AZ-modules](modules.md#az-modules).

| Cmdlet | Beschrijving |
|:--- |:--- |
| [Get-AzAutomationCredential](/powershell/module/az.automation/get-azautomationcredential?view=azps-3.3.0) |Hiermee wordt een [CredentialInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.commands.automation.model.credentialinfo?view=azurerm-ps) -object opgehaald dat meta gegevens bevat over de referentie. De cmdlet haalt het `PSCredential` object zelf niet op.  |
| [New-AzAutomationCredential](/powershell/module/az.automation/new-azautomationcredential?view=azps-3.3.0) |Hiermee maakt u een nieuwe Automation-referentie. |
| [Remove-AzAutomationCredential](/powershell/module/az.automation/remove-azautomationcredential?view=azps-3.3.0) |Hiermee verwijdert u een Automation-referentie. |
| [Set-AzAutomationCredential](/powershell/module/az.automation/set-azautomationcredential?view=azps-3.3.0) |Hiermee stelt u de eigenschappen voor een bestaande Automation-referentie. |

## <a name="other-cmdlets-used-to-access-credentials"></a>Andere cmdlets die worden gebruikt voor toegang tot referenties

De cmdlets in de volgende tabel worden gebruikt voor toegang tot referenties in uw runbooks en DSC-configuraties. 

| Cmdlet | Beschrijving |
|:--- |:--- |
| `Get-AutomationPSCredential` |Hiermee wordt een `PSCredential` object opgehaald dat in een runbook-of DSC-configuratie kan worden gebruikt. Meestal moet u deze [interne cmdlet](modules.md#internal-cmdlets) gebruiken in plaats van de `Get-AzAutomationCredential` cmdlet, aangezien de laatste alleen referentie gegevens ophaalt. Deze informatie is doorgaans niet nuttig voor het door geven van een andere cmdlet. |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |Hiermee wordt een referentie opgehaald met de vraag naar een gebruikers naam en wacht woord. Deze cmdlet maakt deel uit van de standaard module micro soft. Power shell. Security. Zie [standaard modules](modules.md#default-modules).|
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | Hiermee maakt u een referentie-element. Deze cmdlet maakt deel uit van de standaard Azure-module. Zie [standaard modules](modules.md#default-modules).|

Als `PSCredential` u objecten in uw code wilt ophalen, moet u de `Orchestrator.AssetManagement.Cmdlets` module importeren. Zie [modules beheren in azure Automation](modules.md)voor meer informatie.

```azurepowershell
Import-Module Orchestrator.AssetManagement.Cmdlets -ErrorAction SilentlyContinue
```

> [!NOTE]
> Vermijd het gebruik van variabelen in de `Name` para meter van `Get-AutomationPSCredential` . Hun gebruik kan de detectie van afhankelijkheden tussen runbooks of DSC-configuraties en referentie-assets tijdens het ontwerp bemoeilijken.

## <a name="python-2-functions-that-access-credentials"></a>Python 2-functies die toegang hebben tot referenties

De functie in de volgende tabel wordt gebruikt voor toegang tot referenties in een python 2-runbook.

| Functie | Beschrijving |
|:---|:---|
| `automationassets.get_automation_credential` | Hiermee wordt informatie over een referentie-element opgehaald. |

> [!NOTE]
> Importeer de `automationassets` module boven aan het python-runbook om toegang te krijgen tot de Asset-functies.

## <a name="create-a-new-credential-asset"></a>Een nieuwe referentie-Asset maken

U kunt een nieuw referentie-element maken met behulp van de Azure Portal of met behulp van Windows Power shell.

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>Maak een nieuw referentie-element met de Azure Portal

1. Selecteer in uw Automation-account **referenties** onder **gedeelde resources**.
1. Selecteer **een referentie toevoegen**.
2. Voer in het deel venster nieuwe referentie de juiste referentie naam in volgens uw naamgevings standaarden. 
3. Typ uw toegangs-ID in het veld **gebruikers naam** . 
4. Voor beide wachtwoord velden voert u uw geheime toegangs sleutel in.

    ![Nieuwe referentie maken](../media/credentials/credential-create.png)

5. Als het selectie vakje multi-factor Authentication is ingeschakeld, schakelt u dit uit. 
6. Klik op **maken** om het nieuwe referentie-element op te slaan.

> [!NOTE]
> Azure Automation biedt geen ondersteuning voor gebruikers accounts die gebruikmaken van multi-factor Authentication.

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>Een nieuw referentie-element maken met Windows Power shell

In het volgende voor beeld ziet u hoe u een nieuw Automation-referentie-element maakt. Er `PSCredential` wordt eerst een object gemaakt met de naam en het wacht woord en vervolgens gebruikt voor het maken van de referentie-Asset. In plaats daarvan kunt u de- `Get-Credential` cmdlet gebruiken om de gebruiker te vragen een naam en wacht woord in te voeren.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="get-a-credential-asset"></a>Een referentie-element ophalen

Een runbook-of DSC-configuratie haalt een referentie-element op met de interne `Get-AutomationPSCredential` cmdlet. Met deze cmdlet wordt een `PSCredential` object opgehaald dat u kunt gebruiken met een cmdlet die een referentie vereist. U kunt ook de eigenschappen van het referentie object ophalen om het afzonderlijk te gebruiken. Het object heeft eigenschappen voor de gebruikers naam en het beveiligde wacht woord. 

> [!NOTE]
> De `Get-AzAutomationCredential` cmdlet haalt geen `PSCredential` object op dat voor verificatie kan worden gebruikt. Het bevat alleen informatie over de referentie. Als u een referentie in een runbook moet gebruiken, moet u deze als een object ophalen `PSCredential` met `Get-AutomationPSCredential` .

U kunt ook de methode [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) gebruiken om een [NetworkCredential](/dotnet/api/system.net.networkcredential) -object op te halen dat een niet-beveiligde versie vertegenwoordigt van het wacht woord.

### <a name="textual-runbook-example"></a>Voor beeld van een tekst-runbook

In het volgende voor beeld ziet u hoe u een Power shell-referentie gebruikt in een runbook. De referentie wordt opgehaald en de gebruikers naam en het wacht woord worden toegewezen aan variabelen.


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

U kunt ook een referentie gebruiken om te verifiëren bij Azure met [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.3.0). In de meeste gevallen moet u een [uitvoeren als-account](../manage-runas-account.md) gebruiken en de verbinding ophalen met [Get-AzAutomationConnection](../automation-connections.md).


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>Voor beeld van grafisch runbook

U kunt een activiteit voor de interne `Get-AutomationPSCredential` cmdlet toevoegen aan een grafisch runbook door met de rechter muisknop op de referentie in het deel venster Bibliotheek van de grafische editor te klikken en **toevoegen aan canvas**te selecteren.

![Referentie toevoegen aan canvas](../media/credentials/credential-add-canvas.png)

In de volgende afbeelding ziet u een voor beeld van het gebruik van een referentie in een grafisch runbook. In dit geval biedt de referentie verificatie voor een runbook aan Azure-resources, zoals beschreven in [Azure AD gebruiken in azure Automation om te verifiëren bij Azure](../automation-use-azure-ad.md). Met de eerste activiteit wordt de referentie opgehaald die toegang heeft tot het Azure-abonnement. De activiteit account verbinding gebruikt vervolgens deze referentie om verificatie uit te voeren voor alle activiteiten die zich daarna voordoen. Er wordt hier een [pijplijn koppeling](../automation-graphical-authoring-intro.md#use-links-for-workflow) gebruikt omdat deze `Get-AutomationPSCredential` een enkel object verwacht.  

![Referentie toevoegen aan canvas](../media/credentials/get-credential.png)

## <a name="use-credentials-in-a-dsc-configuration"></a>Referenties gebruiken in een DSC-configuratie

DSC-configuraties in Azure Automation kunnen werken met referentie-assets met `Get-AutomationPSCredential` , maar ze kunnen ook referentie-assets door geven via para meters. Zie [configuraties compileren in azure Automation DSC](../automation-dsc-compile.md#credential-assets)voor meer informatie.

## <a name="use-credentials-in-a-python-2-runbook"></a>Referenties gebruiken in een python 2-runbook

In het volgende voor beeld ziet u een voor beeld van toegang tot referenties in Python 2-runbooks.


```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Volgende stappen

* [Modules beheren in Azure Automation](modules.md)
* [Uitvoering van runbooks in Azure Automation](../automation-runbook-execution.md)
* [Overzicht van Azure Automation status configuratie](../automation-dsc-overview.md) 