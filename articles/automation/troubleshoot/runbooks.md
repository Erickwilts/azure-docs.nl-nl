---
title: Fouten met Azure Automation Runbooks oplossen
description: Meer informatie over het oplossen van problemen met Azure Automation runbooks
services: automation
author: bobbytreed
ms.author: robreed
ms.date: 01/24/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 759422ea8c327ae67278354217dac4c60b32f7a9
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68850325"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Fouten met runbooks oplossen

In dit artikel vindt u informatie over veelvoorkomende problemen met runbooks en hoe u deze kunt oplossen.

## <a name="steps-to-troubleshoot-runbooks"></a>Stappen voor het oplossen van problemen met runbooks

Wanneer er fouten zijn opgetreden bij het uitvoeren van runbooks in Azure Automation, kunt u de volgende stappen gebruiken om het probleem te diagnosticeren.

1. **Zorg ervoor dat het runbook-script correct wordt uitgevoerd op uw lokale machine:**  Raadpleeg de [Power shell-docs](/powershell/scripting/overview) of [python-documenten](https://docs.python.org/3/) voor taal referentie-en leer modules.

   Als u uw script lokaal uitvoert, kunnen veelvoorkomende fouten worden gedetecteerd en opgelost, zoals:

   - **Ontbrekende modules**
   - **Syntaxis fouten**
   - **Logische fouten**

2. **Zorg ervoor dat uw knoop punten en Automation-werk ruimte de vereiste modules hebben:** Als uw runbook modules importeert, controleert u of deze beschikbaar zijn in uw Automation-account met behulp van de stappen die worden beschreven in [modules importeren](../shared-resources/modules.md#import-modules). Zie [problemen met modules oplossen](shared-resources.md#modules)voor meer informatie.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatie fouten bij het werken met Azure Automation runbooks

### <a name="login-azurerm"></a>Omstandigheden Aanmelding uitvoeren-AzureRMAccount om u aan te melden

#### <a name="issue"></a>Probleem

Bij het uitvoeren van een runbook wordt de volgende fout weer gegeven:

```error
Run Login-AzureRMAccount to login.
```

#### <a name="cause"></a>Oorzaak

Deze fout heeft twee primaire oorzaken:

* Verschillende versies van AzureRM-modules.
* U probeert toegang te krijgen tot resources in een afzonderlijk abonnement.

#### <a name="resolution"></a>Oplossing

Als deze fout wordt weer gegeven nadat u één AzureRM-module hebt bijgewerkt, moet u alle AzureRM-modules bijwerken naar dezelfde versie.

Als u probeert toegang te krijgen tot resources in een ander abonnement, kunt u de onderstaande stappen volgen om machtigingen te configureren.

1. Ga naar het run as-account van het Automation-account en kopieer de toepassings-ID en vinger afdruk.
  ![Toepassings-ID en vinger afdruk kopiëren](../media/troubleshoot-runbooks/collect-app-id.png)
1. Ga naar de Access Control van het abonnement waarin het Automation-account niet wordt gehost en voeg een nieuwe roltoewijzing toe.
  ![Toegangsbeheer](../media/troubleshoot-runbooks/access-control.png)
1. Voeg de toepassings-ID die u in de vorige stap hebt verzameld, toe. Selecteer Inzender machtigingen.
   ![Roltoewijzing toevoegen](../media/troubleshoot-runbooks/add-role-assignment.png)
1. Kopieer de naam van het abonnement voor de volgende stap.
1. U kunt nu de volgende runbook-code gebruiken om de machtigingen van uw Automation-account te testen voor het andere abonnement.

    Vervang '\<CertificateThumbprint\>' door de waarde die u in stap #1 hebt gekopieerd\>en de\<waarde ' subscriptionname ' die u in stap #4 hebt gekopieerd.

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint "<CertificateThumbprint>"
    #Select the subscription you want to work with
    Select-AzureRmSubscription -SubscriptionName '<YourSubscriptionNameGoesHere>'

    #Test and get outputs of the subscriptions you granted access.
    $subscriptions = Get-AzureRmSubscription
    foreach($subscription in $subscriptions)
    {
        Set-AzureRmContext $subscription
        Write-Output $subscription.Name
    }
    ```

### <a name="sign-in-failed"></a>Omstandigheden Aanmelden bij het Azure-account is mislukt

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met `Add-AzureAccount` de `Connect-AzureRmAccount` -of-cmdlets:

```error
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op als de naam van het referentie-element niet geldig is. Deze fout kan ook optreden als de gebruikers naam en het wacht woord die u hebt gebruikt voor het instellen van het Automation-referentie-element, niet geldig zijn.

#### <a name="resolution"></a>Oplossing

Voer de volgende stappen uit om te bepalen wat er mis is:

1. Zorg ervoor dat u geen speciale tekens hebt. Deze tekens bevatten het **\@** teken in de naam van het Automation-referentie-element dat u gebruikt om verbinding te maken met Azure.
2. Controleer of u de gebruikers naam en het wacht woord kunt gebruiken die zijn opgeslagen in de Azure Automation referentie in uw lokale Power shell ISE-editor. U kunt controleren of de gebruikers naam en het wacht woord juist zijn door de volgende cmdlets uit te voeren in Power shell ISE:

   ```powershell
   $Cred = Get-Credential
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred
   #Using Azure Resource Manager
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Als uw verificatie lokaal mislukt, betekent dit dat u uw Azure Active Directory referenties niet op de juiste wijze hebt ingesteld. Raadpleeg de [verificatie bij Azure met Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) blog bericht om de Azure Active Directory-account goed in te stellen.

4. Als het lijkt alsof er een tijdelijke fout optreedt, kunt u proberen de logica opnieuw proberen toe te voegen aan uw verificatie routine om te zorgen voor een betere verificatie.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Omstandigheden Kan het Azure-abonnement niet vinden

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het werken met `Select-AzureSubscription` de `Select-AzureRmSubscription` -of-cmdlets:

```error
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Fout

Deze fout kan optreden als:

* De naam van het abonnement is niet geldig

* De Azure Active Directory gebruiker die de abonnements gegevens probeert op te halen, is niet geconfigureerd als beheerder van het abonnement.

#### <a name="resolution"></a>Oplossing

Voer de volgende stappen uit om te bepalen of u hebt geverifieerd voor Azure en toegang hebt tot het abonnement dat u wilt selecteren:

1. Test uw script buiten Azure Automation om ervoor te zorgen dat het zelfstandig werkt.
2. Zorg ervoor dat u de `Add-AzureAccount` cmdlet uitvoert voordat u de `Select-AzureSubscription` cmdlet uitvoert.
3. Voeg `Disable-AzureRmContextAutosave –Scope Process` aan het begin van uw runbook toe. Deze cmdlet zorgt ervoor dat alle referenties alleen van toepassing zijn op de uitvoering van het huidige runbook.
4. Als dit fout bericht nog steeds wordt weer gegeven, wijzigt u de code door de para meter `Add-AzureAccount` AzureRmContext toe te voegen na de cmdlet en vervolgens de code uit te voeren.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Omstandigheden Verificatie naar Azure is mislukt omdat multi-factor Authentication is ingeschakeld

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout melding bij de verificatie bij Azure met uw Azure-gebruikers naam en-wacht woord:

```error
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Oorzaak

Als u multi-factor Authentication voor uw Azure-account hebt, kunt u geen Azure Active Directory gebruiker gebruiken om te verifiëren bij Azure. In plaats daarvan moet u een certificaat of een Service-Principal gebruiken om te verifiëren bij Azure.

#### <a name="resolution"></a>Oplossing

Als u een certificaat wilt gebruiken met de cmdlets van het klassieke Azure-implementatie model, raadpleegt u [een certificaat maken en toevoegen om Azure-Services te beheren.](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Als u een Service-Principal wilt gebruiken met Azure Resource Manager-cmdlets, raadpleegt u [Service-Principal maken met behulp van Azure Portal](../../active-directory/develop/howto-create-service-principal-portal.md) en [verificatie van een service-principal met Azure Resource Manager.](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="common-errors-when-working-with-runbooks"></a>Veelvoorkomende fouten bij het werken met runbooks

### <a name="child-runbook-object"></a>Onderliggend runbook retourneert een fout wanneer de uitvoer stroom objecten bevat in plaats van eenvoudige gegevens typen

#### <a name="issue"></a>Probleem

U ontvangt de volgende fout bij het aanroepen van een onderliggend runbook met de `-Wait` switch en de uitvoer stroom bevat een object:

```error
Object reference not set to an instance of an object
```

#### <a name="cause"></a>Oorzaak

Er is een bekend probleem waarbij de [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) de uitvoer stroom niet correct verwerkt als deze objecten bevat.

#### <a name="resolution"></a>Oplossing

U kunt dit probleem oplossen door in plaats daarvan een polling-logica te implementeren en de cmdlet [Get-AzureRmAutomationJobOutput](/powershell/module/azurerm.automation/get-azurermautomationjoboutput) te gebruiken om de uitvoer op te halen. In het volgende voor beeld wordt een voor beeld van deze logica gedefinieerd.

```powershell
$automationAccountName = "ContosoAutomationAccount"
$runbookName = "ChildRunbookExample"
$resourceGroupName = "ContosoRG"

function IsJobTerminalState([string] $status) {
    return $status -eq "Completed" -or $status -eq "Failed" -or $status -eq "Stopped" -or $status -eq "Suspended"
}

$job = Start-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName -Name $runbookName -ResourceGroupName $resourceGroupName
$pollingSeconds = 5
$maxTimeout = 10800
$waitTime = 0
while((IsJobTerminalState $job.Status) -eq $false -and $waitTime -lt $maxTimeout) {
   Start-Sleep -Seconds $pollingSeconds
   $waitTime += $pollingSeconds
   $job = $job | Get-AzureRmAutomationJob
}

$jobResults | Get-AzureRmAutomationJobOutput | Get-AzureRmAutomationJobOutputRecord | Select-Object -ExpandProperty Value
```

### <a name="get-serializationsettings"></a>Omstandigheden Er wordt een fout in uw taak stromen weer geven over de methode get_SerializationSettings

#### <a name="issue"></a>Probleem

U ziet in de fout in uw taak stromen voor een runbook met het volgende bericht:

```error
Connect-AzureRMAccount : Method 'get_SerializationSettings' in type
'Microsoft.Azure.Management.Internal.Resources.ResourceManagementClient' from assembly
'Microsoft.Azure.Commands.ResourceManager.Common, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'
does not have an implementation.
At line:16 char:1
+ Connect-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Connect-AzureRmAccount], TypeLoadException
    + FullyQualifiedErrorId : System.TypeLoadException,Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt door het gebruik van zowel AzureRM-als AZ-cmdlets in een runbook. Deze gebeurtenis treedt op wanneer `Az` u importeert `AzureRM`voordat u importeert.

#### <a name="resolution"></a>Oplossing

AZ en AzureRM-cmdlets kunnen niet worden geïmporteerd en gebruikt in hetzelfde runbook, Zie [AZ module support in azure Automation](../az-modules.md)voor meer informatie over AZ-ondersteuning in azure Automation.

### <a name="task-was-cancelled"></a>Omstandigheden Het runbook is mislukt met de volgende fout: Een taak is geannuleerd

#### <a name="issue"></a>Probleem

Het runbook is mislukt met een fout die vergelijkbaar is met het volgende voor beeld:

```error
Exception: A task was canceled.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door het gebruik van verouderde Azure-modules.

#### <a name="resolution"></a>Oplossing

Deze fout kan worden opgelost door uw Azure-modules bij te werken naar de meest recente versie.

Klik in uw Automation-account op **modules**en klik vervolgens op **Azure-modules bijwerken**. De update neemt ongeveer 15 minuten in beslag, zodra het runbook dat is mislukt opnieuw is uitgevoerd. Zie [Azure-modules bijwerken in azure Automation](../automation-update-azure-modules.md)voor meer informatie over het bijwerken van uw modules.

### <a name="runbook-auth-failure"></a>Omstandigheden Runbooks mislukken bij het omgaan met meerdere abonnementen

#### <a name="issue"></a>Probleem

Bij het uitvoeren van runbooks `Start-AzureRmAutomationRunbook`met kan het runbook geen Azure-resources beheren.

#### <a name="cause"></a>Oorzaak

Het runbook gebruikt niet de juiste context wanneer het wordt uitgevoerd.

#### <a name="resolution"></a>Oplossing

Wanneer u met meerdere abonnementen werkt, kan de context van het abonnement verloren gaan bij het aanroepen van runbooks. Om ervoor te zorgen dat de context van het abonnement wordt door gegeven aan `AzureRmContext` de runbooks, voegt u de para meter toe aan de cmdlet en geeft u de context door. Het is ook raadzaam om de `Disable-AzureRmContextAutosave` cmdlet met het **proces** bereik te gebruiken om ervoor te zorgen dat de referenties die u gebruikt alleen worden gebruikt voor het huidige runbook.

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Omstandigheden Het runbook is mislukt vanwege een ontbrekende cmdlet

#### <a name="issue"></a>Probleem

Het runbook is mislukt met een fout die vergelijkbaar is met het volgende voor beeld:

```error
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan zich voordoen op basis van een van de volgende redenen:

* De module met de cmdlet is niet geïmporteerd in het Automation-account
* De module met de cmdlet is geïmporteerd, maar is verouderd

#### <a name="resolution"></a>Oplossing

Deze fout kan worden opgelost door een van de volgende taken uit te voeren:

Als de module een Azure-module is, raadpleegt [u Azure PowerShell-modules in azure Automation bijwerken](../automation-update-azure-modules.md) voor meer informatie over het bijwerken van uw modules in uw Automation-account.

Als het een afzonderlijke module is, controleert u of de module in uw Automation-account is geïmporteerd.

### <a name="job-attempted-3-times"></a>Omstandigheden Het starten van de runbook-taak is drie keer geprobeerd, maar het starten is mislukt elke keer

#### <a name="issue"></a>Probleem

Het runbook is mislukt met de volgende fout:

```error
The job was tried three times but it failed
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op als gevolg van een van de volgende problemen:

* Geheugen limiet. De gedocumenteerde limieten voor de hoeveelheid geheugen die aan een sandbox wordt toegewezen, vindt u op de [automatiserings service limieten](../../azure-subscription-service-limits.md#automation-limits). Een taak kan mislukken als er meer dan 400 MB aan geheugen wordt gebruikt.

* Netwerk sockets. Azure-sandboxes zijn beperkt tot 1000 gelijktijdige netwerk sockets, zoals wordt beschreven in limieten voor Automation- [service](../../azure-subscription-service-limits.md#automation-limits).

* Module is niet compatibel. Deze fout kan optreden als module-afhankelijkheden niet juist zijn en als ze niet het geval zijn, retourneert uw runbook meestal het bericht ' opdracht niet gevonden ' of ' kan para meter niet binden '.

* Uw runbook heeft geprobeerd een uitvoerbaar of subproces aan te roepen in een runbook dat wordt uitgevoerd in een Azure sandbox. Dit scenario wordt niet ondersteund in azure-sandboxes.

* Uw runbook heeft geprobeerd te veel uitzonderings gegevens te schrijven naar de uitvoer stroom.

#### <a name="resolution"></a>Oplossing

Het probleem wordt opgelost met een van de volgende oplossingen:

* Voorgestelde methoden voor het werken binnen de geheugen limiet zijn het splitsen van de werk belasting tussen meerdere runbooks, het verwerken van niet zoveel gegevens in het geheugen, niet voor het schrijven van overbodige uitvoer van runbooks, of het bepalen van het aantal controle punten dat u in uw Power shell-werk stroom schrijft runbooks. U kunt de methode Clear gebruiken, bijvoorbeeld `$myVar.clear()` om de variabele uit te scha kelen en direct te gebruiken `[GC]::Collect()` om garbagecollection onmiddellijk uit te voeren. Deze acties verminderen het geheugen gebruik van uw runbook tijdens runtime.

* Werk uw Azure-modules bij door de stappen [te volgen om Azure PowerShell-modules in azure Automation bij te werken](../automation-update-azure-modules.md).

* Een andere oplossing is het runbook uit te voeren op een [Hybrid Runbook worker](../automation-hrw-run-runbooks.md). Hybrid Workers worden niet beperkt door de geheugen-en netwerk limieten die Azure-sandboxes zijn.

* Als u een proces (zoals. exe of subproces. Call) moet aanroepen in een runbook, moet u het runbook uitvoeren op een [Hybrid Runbook worker](../automation-hrw-run-runbooks.md).

* Er is een limiet van 1 MB voor de taak uitvoer stroom. Zorg ervoor dat u aanroepen naar een uitvoerbaar of subproces in een try/catch-blok plaatst. Als er een uitzonde ring optreedt, schrijft u het bericht van die uitzonde ring in een Automation-variabele. Hiermee voor komt u dat het naar de uitvoer stroom van de taak kan worden geschreven.

### <a name="fails-deserialized-object"></a>Omstandigheden Runbook is mislukt vanwege een gedeserialiseerd object

#### <a name="issue"></a>Probleem

Het runbook is mislukt met de volgende fout:

```error
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Oorzaak

Als uw runbook een Power shell-werk stroom is, worden complexe objecten in een gedeserialiseerd indeling opgeslagen om de status van het runbook te behouden als de werk stroom wordt onderbroken.

#### <a name="resolution"></a>Oplossing

Dit probleem wordt opgelost met een van de volgende drie oplossingen:

* Als u complexe objecten van de ene cmdlet naar de andere wilt door lopen, moet u deze cmdlets in een InlineScript plaatsen.
* Geef de naam of waarde die u nodig hebt uit het complexe object in plaats van het volledige object door te geven.
* Gebruik een Power shell-runbook in plaats van een Power shell workflow-runbook.

### <a name="runbook-fails"></a>Omstandigheden Mijn Runbook mislukt, maar werkt wanneer het lokaal wordt uitgevoerd

#### <a name="issue"></a>Probleem

Uw script mislukt als een runbook wordt uitgevoerd, maar dit werkt wanneer het lokaal wordt uitgevoerd.

#### <a name="cause"></a>Oorzaak

Het script kan mislukken wanneer het als een runbook wordt uitgevoerd om een van de volgende redenen:

* Verificatieproblemen
* De vereiste modules zijn niet geïmporteerd of verouderd.
* Uw script kan vragen om tussen komst van de gebruiker.
* Sommige modules maken veronderstellingen over bibliotheken die aanwezig zijn op Windows-computers. Deze bibliotheken zijn mogelijk niet aanwezig in een sandbox.
* Sommige modules zijn afhankelijk van een .NET-versie die afwijkt van die van de in de sandbox beschik bare.

#### <a name="resolution"></a>Oplossing

Dit probleem kan worden opgelost met een van de volgende oplossingen:

* Controleer of u goed bent [geverifieerd bij Azure](../manage-runas-account.md).
* Zorg ervoor dat uw [Azure-modules zijn geïmporteerd en bijgewerkt](../automation-update-azure-modules.md).
* Controleer of er geen-cmdlets worden gevraagd om informatie. Dit gedrag wordt niet ondersteund in runbooks.
* Controleer of alles dat deel uitmaakt van uw module, afhankelijk is van iets dat niet is opgenomen in de module.
* Azure-sandboxes gebruiken .NET Framework 4.7.2, als een module gebruikmaakt van een hogere versie die niet werkt. In dit geval moet u een [Hybrid Runbook worker](../automation-hybrid-runbook-worker.md) gebruiken

Als geen van deze oplossingen uw problemReview de [taak logboeken](../automation-runbook-execution.md#viewing-job-status-from-the-azure-portal) voor specifieke details in voor de oorzaak van het mislukken van uw runbook heeft opgelost.

### <a name="quota-exceeded"></a>Omstandigheden Runbook-taak is mislukt omdat het toegewezen quotum is overschreden

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de volgende fout:

```error
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer de taak uitvoering het quotum van 500 minuten voor uw account overschrijdt. Dit quotum is van toepassing op alle typen taken voor taak uitvoering. Sommige van deze taken kunnen een taak testen, een taak starten vanuit de portal, een taak uitvoeren met behulp van webhooks of een taak plannen om uit te voeren met behulp van de Azure Portal of in uw Data Center. Zie [prijzen](https://azure.microsoft.com/pricing/details/automation/)voor Automation voor meer informatie over prijzen voor Automation.

#### <a name="resolution"></a>Oplossing

Als u meer dan 500 minuten van verwerking per maand wilt gebruiken, moet u uw abonnement wijzigen van de laag gratis in de laag basis. U kunt een upgrade uitvoeren naar de Basic-laag door de volgende stappen uit te voeren:

1. Meld u aan bij uw Azure-abonnement
2. Selecteer het Automation-account dat u wilt upgraden
3. Klik op**prijzen**van **instellingen** > .
4. Klik op **inschakelen** op de onderkant van de pagina om uw account te upgraden naar de **Basic** -laag.

### <a name="cmdlet-not-recognized"></a>Omstandigheden De cmdlet wordt niet herkend bij het uitvoeren van een runbook

#### <a name="issue"></a>Probleem

De runbook-taak is mislukt met de volgende fout:

```error
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Oorzaak

Deze fout wordt veroorzaakt wanneer de Power shell-engine de cmdlet die u in uw runbook gebruikt, niet kan vinden. Deze fout kan optreden omdat de module met de cmdlet ontbreekt in het account. er is een naam conflict met een runbooknaam, of de cmdlet bestaat ook in een andere module en de naam kan niet worden omgezet met Automation.

#### <a name="resolution"></a>Oplossing

Het probleem wordt opgelost met een van de volgende oplossingen:

* Controleer of u de naam van de cmdlet juist hebt ingevoerd.
* Zorg ervoor dat de cmdlet bestaat in uw Automation-account en dat er geen conflicten zijn. Als u wilt controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkings modus en zoekt u naar de cmdlet die u `Get-Command <CommandName>`wilt zoeken in de bibliotheek of voert u uit. Nadat u hebt gecontroleerd of de cmdlet beschikbaar is voor het account en dat er geen naam conflicten met andere cmdlets of runbooks zijn, voegt u deze toe aan het canvas en zorgt u ervoor dat u een geldige para meter in uw runbook hebt ingesteld.
* Als er sprake is van een naam conflict en de cmdlet is beschikbaar in twee verschillende modules, kunt u dit probleem oplossen met behulp van de volledig gekwalificeerde naam voor de cmdlet. U kunt bijvoorbeeld **ModuleName\CmdletName**gebruiken.
* Als u het runbook on-premises uitvoert in een Hybrid worker-groep, moet u ervoor zorgen dat de module en de cmdlet zijn geïnstalleerd op de computer die als host fungeert voor de Hybrid Worker.

### <a name="long-running-runbook"></a>Omstandigheden Een langlopend runbook wordt niet voltooid

#### <a name="issue"></a>Probleem

Het runbook wordt na drie uur weer gegeven met de status **gestopt** . Mogelijk wordt ook de volgende fout weer gegeven:

```error
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Dit gedrag is inherent aan het ontwerp van Azure-sandboxes vanwege de bewaking van de ' fair share ' van processen binnen Azure Automation. Als de app langer dan drie uur wordt uitgevoerd, stopt de billijke share automatisch een runbook. De status van een runbook dat de duur van de billijke share overschrijdt, verschilt per runbook-type. Power shell-en python-runbooks zijn ingesteld op de status **gestopt** . De Power shell-werk stroom-runbooks zijn ingesteld op **mislukt**.

#### <a name="cause"></a>Oorzaak

Het runbook is groter dan de limiet van drie uur dat is toegestaan door een billijke share in een Azure sandbox.

#### <a name="resolution"></a>Oplossing

Een aanbevolen oplossing is om het runbook uit te voeren op een [Hybrid Runbook worker](../automation-hrw-run-runbooks.md).

Hybrid Workers worden niet beperkt door de limiet van het [billijke share](../automation-runbook-execution.md#fair-share) 3 uur-Runbook dat Azure-sandboxes zijn. Runbooks die worden uitgevoerd op Hybrid Runbook Workers moeten worden ontwikkeld ter ondersteuning van het opnieuw starten van gedrag als er onverwachte problemen zijn met de lokale infra structuur.

Een andere optie is om het runbook te optimaliseren door [onderliggende runbooks](../automation-child-runbooks.md)te maken. Als uw runbook met dezelfde functie wordt uitgevoerd op verschillende bronnen, zoals een database bewerking op verschillende data bases, kunt u die functie verplaatsen naar een onderliggend runbook. Elk van deze onderliggende runbooks wordt parallel uitgevoerd in afzonderlijke processen. Dit gedrag vermindert de totale tijd voor het volt ooien van het bovenliggende runbook.

De Power shell-cmdlets waarmee het onderliggende runbook-scenario wordt ingeschakeld, zijn:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) : met deze cmdlet kunt u een runbook starten en para meters door geven aan het runbook

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob) : als er bewerkingen zijn die moeten worden uitgevoerd nadat het onderliggende runbook is voltooid, kunt u met deze cmdlet de taak status van elk onderliggend item controleren.

### <a name="expired webhook"></a>Omstandigheden Status: 400 ongeldige aanvraag bij het aanroepen van een webhook

#### <a name="issue"></a>Probleem

Wanneer u probeert een webhook voor een Azure Automation runbook aan te roepen, wordt de volgende fout weer gegeven:

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Oorzaak

De webhook die u probeert aan te roepen, is uitgeschakeld of is verlopen.

#### <a name="resolution"></a>Oplossing

Als de webhook is uitgeschakeld, kunt u de webhook opnieuw inschakelen via de Azure Portal. Wanneer een webhook is verlopen, moet de webhook worden verwijderd en opnieuw worden gemaakt. U kunt [een webhook alleen vernieuwen](../automation-webhooks.md#renew-webhook) als deze nog niet is verlopen.

### <a name="429"></a>Omstandigheden 429: The request rate is currently too large. Probeer het opnieuw

#### <a name="issue"></a>Probleem

Wanneer u de `Get-AzureRmAutomationJobOutput` cmdlet uitvoert, wordt het volgende fout bericht weer gegeven:

```error
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Oorzaak

Deze fout kan optreden bij het ophalen van de taak uitvoer van een runbook met veel [uitgebreide stromen](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Oplossing

Er zijn twee manieren om deze fout op te lossen:

* Bewerk het runbook en verminder het aantal taak stromen dat wordt verzonden.
* Verminder het aantal streams dat moet worden opgehaald bij het uitvoeren van de cmdlet. Als u dit gedrag wilt volgen, kunt u `-Stream Output` de para meter `Get-AzureRmAutomationJobOutput` voor de cmdlet opgeven om alleen uitvoer stromen op te halen. 

### <a name="cannot-invoke-method"></a>Omstandigheden Power shell-taak mislukt met fout: Kan methode niet aanroepen

#### <a name="issue"></a>Probleem

Het volgende fout bericht wordt weer gegeven bij het starten van een Power shell-taak in een runbook dat wordt uitgevoerd in Azure:

```error
Exception was thrown - Cannot invoke method. Method invocation is supported only on core types in this language mode.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan optreden wanneer u een Power shell-taak start in een runbook dat is uitgevoerd in Azure. Dit probleem kan optreden omdat runbooks die in een Azure-sandbox werden uitgevoerd, mogelijk niet in de [volledige taal modus](/powershell/module/microsoft.powershell.core/about/about_language_modes)worden uitgevoerd.

#### <a name="resolution"></a>Oplossing

Er zijn twee manieren om deze fout op te lossen:

* `Start-Job` Gebruik`Start-AzureRmAutomationRunbook` in plaats van gebruiken om een runbook te starten
* Als uw runbook dit fout bericht bevat, voert u dit uit op een Hybrid Runbook Worker

Voor meer informatie over dit gedrag en ander gedrag van Azure Automation Runbooks raadpleegt u [Runbook Behavior](../automation-runbook-execution.md#runbook-behavior).

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een ondersteunings incident voor Azure opslaan. Ga naar de [ondersteunings site van Azure](https://azure.microsoft.com/support/options/) en selecteer **ondersteuning verkrijgen**.
