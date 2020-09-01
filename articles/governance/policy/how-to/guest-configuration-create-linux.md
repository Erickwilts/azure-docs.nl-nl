---
title: Beleidsregels voor gastconfiguratie voor Linux maken
description: Meer informatie over het maken van een Azure Policy gast configuratie beleid voor Linux.
ms.date: 08/17/2020
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 4f49732aa2be50b0d8be6f1f3af974121dc9f363
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2020
ms.locfileid: "89076358"
---
# <a name="how-to-create-guest-configuration-policies-for-linux"></a>Beleidsregels voor gastconfiguratie voor Linux maken

Lees de overzichts informatie op [Azure Policy-gast configuratie](../concepts/guest-configuration.md)voordat u een aangepast beleid maakt.
 
Ga voor meer informatie over het maken van gast configuratie beleidsregels voor Windows naar de pagina [beleid voor gast configuratie maken voor Windows](./guest-configuration-create.md)

Bij het controleren van Linux maakt gastconfiguratie gebruik van [Chef InSpec](https://www.inspec.io/). Het InSpec-profiel definieert de toestand waarin de machine zich moet bevinden. Als de evaluatie van de configuratie mislukt, wordt het beleids effect **auditIfNotExists** geactiveerd en wordt de computer als **niet-compatibel**beschouwd.

[Azure Policy-gast configuratie](../concepts/guest-configuration.md) kan alleen worden gebruikt voor het controleren van instellingen binnen machines. Herstel van instellingen binnen computers is nog niet beschikbaar.

Gebruik de volgende acties om uw eigen configuratie te maken voor het valideren van de status van een Azure-of niet-Azure-machine.

> [!IMPORTANT]
> Aangepaste beleids regels met gast configuratie is een preview-functie.
>
> De gastconfiguratie-extensie is vereist voor het uitvoeren van controles op virtuele machines van Azure. Als u de uitbrei ding op schaal wilt implementeren op alle Linux-machines, wijst u de volgende beleids definitie toe: `Deploy prerequisites to enable Guest Configuration Policy on Linux VMs`

## <a name="install-the-powershell-module"></a>De Power shell-module installeren

De gast configuratie module automatiseert het proces van het maken van aangepaste inhoud, waaronder:

- Een inhouds artefact voor een gast configuratie maken (. zip)
- Geautomatiseerd testen van het artefact
- Een beleids definitie maken
- Het beleid publiceren

De module kan worden geïnstalleerd op een computer met Windows, macOS of Linux met Power shell 6,2 of later lokaal of met [Azure Cloud shell](https://shell.azure.com), of met de [Azure PowerShell core docker-installatie kopie](https://hub.docker.com/r/azuresdk/azure-powershell-core).

> [!NOTE]
> Compilatie van configuraties wordt niet ondersteund in Linux.

### <a name="base-requirements"></a>Basisvereisten

Besturings systemen waarop de module kan worden geïnstalleerd:

- Linux
- macOS
- Windows

> [!NOTE]
> Voor de cmdlet test-GuestConfigurationPackage is OpenSSL-versie 1,0 vereist vanwege een afhankelijkheid van OMI. Dit veroorzaakt een fout op een wille keurige omgeving met OpenSSL 1,1 of hoger.

Voor de module gast configuratie resource is de volgende software vereist:

- Power shell 6,2 of hoger. Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/powershell/scripting/install/installing-powershell) op.
- Azure PowerShell 1.5.0 of hoger. Als deze nog niet is geïnstalleerd, volgt u [deze instructies](/powershell/azure/install-az-ps) op.
  - Alleen de AZ-modules AZ. accounts en AZ. resources zijn vereist.

### <a name="install-the-module"></a>Installeer de module

De **GuestConfiguration** -module installeren in Power shell:

1. Voer de volgende opdracht uit vanuit een Power shell-prompt:

   ```azurepowershell-interactive
   # Install the Guest Configuration DSC resource module from PowerShell Gallery
   Install-Module -Name GuestConfiguration
   ```

1. Controleer of de module is geïmporteerd:

   ```azurepowershell-interactive
   # Get a list of commands for the imported GuestConfiguration module
   Get-Command -Module 'GuestConfiguration'
   ```

## <a name="guest-configuration-artifacts-and-policy-for-linux"></a>Gast configuratie artefacten en beleid voor Linux

Zelfs in Linux-omgevingen gebruikt de gast configuratie de gewenste status configuratie als een taal abstractie. De implementatie is gebaseerd op systeem eigen code (C++) zodat Power shell niet hoeft te worden geladen. Er is echter wel een configuratie-MOF vereist waarin de details van de omgeving worden beschreven.
DSC fungeert als wrapper voor inspecers om te standaardiseren hoe het wordt uitgevoerd, hoe para meters worden opgegeven en hoe uitvoer wordt geretourneerd naar de service. Er is weinig kennis van DSC vereist bij het werken met aangepaste Inspec-inhoud.

#### <a name="configuration-requirements"></a>Configuratie vereisten

De naam van de aangepaste configuratie moet consistent zijn. De naam van het zip-bestand voor het inhouds pakket, de configuratie naam in het MOF-bestand en de naam van de gast toewijzing in de Azure Resource Manager sjabloon (ARM-sjabloon) moet hetzelfde zijn.

### <a name="custom-guest-configuration-configuration-on-linux"></a>Configuratie van aangepaste gast configuratie op Linux

Gast configuratie op Linux maakt gebruik `ChefInSpecResource` van de resource om de-engine de naam van het [Inspec-profiel](https://www.inspec.io/docs/reference/profiles/)te geven. **Name** is de enige vereiste bron eigenschap. Maak een YaML-bestand en een ruby-script bestand, zoals hieronder wordt beschreven.

Maak eerst het YaML-bestand dat wordt gebruikt door de specificatie. Het bestand bevat algemene informatie over de omgeving. Hieronder ziet u een voor beeld:

```YaML
name: linux-path
title: Linux path
maintainer: Test
summary: Test profile
license: MIT
version: 1.0.0
supports:
    - os-family: unix
```

Sla dit bestand op in `inspec.yml` een map met `linux-path` de naam in de projectmap.

Maak vervolgens het ruby-bestand met de specificatie van de Inspec-taal die wordt gebruikt om de machine te controleren.

```Ruby
describe file('/tmp') do
    it { should exist }
end
```

Sla dit bestand met `linux-path.rb` de naam op in een nieuwe map met de naam in `controls` de `linux-path` map.

Maak ten slotte een configuratie, importeer de resource module **PSDesiredStateConfiguration** en compileer de configuratie.

```powershell
# Define the configuration and import GuestConfiguration
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
        }
    }
}

# Compile the configuration to create the MOF files
import-module PSDesiredStateConfiguration
AuditFilePathExists -out ./Config
```

Sla het bestand op met `config.ps1` de naam in de projectmap. Voer deze uit in Power shell door `./config.ps1` uit te voeren in de Terminal. Er wordt een nieuw MOF-bestand gemaakt.

De `Node AuditFilePathExists` opdracht is niet technisch vereist, maar er wordt een bestand gemaakt met de naam `AuditFilePathExists.mof` in plaats van de standaard waarde `localhost.mof` . Als u de bestands naam. MOF volgt, is het eenvoudig om veel bestanden te organiseren wanneer deze op schaal worden uitgevoerd.

U hebt nu een project structuur als volgt:

```file
/ AuditFilePathExists
    / Config
        AuditFilePathExists.mof
    / linux-path
        linux-path.yml
        / controls
            linux-path.rb 
```

De ondersteunende bestanden moeten samen worden verpakt. Het voltooide pakket wordt gebruikt door de gast configuratie om de Azure Policy definities te maken.

Met de `New-GuestConfigurationPackage` cmdlet maakt u het pakket. Para meters van de `New-GuestConfigurationPackage` cmdlet bij het maken van Linux-inhoud:

- **Naam**: naam van het gast configuratie pakket.
- **Configuratie**: gecompileerd configuratie document volledig pad.
- **Pad**: pad naar map voor uitvoer. Deze parameter is optioneel. Als u niets opgeeft, wordt het pakket gemaakt in de huidige map.
- **ChefProfilePath**: volledig pad naar het INSPEC-profiel. Deze para meter wordt alleen ondersteund bij het maken van inhoud om Linux te controleren.

Voer de volgende opdracht uit om een pakket te maken met behulp van de configuratie die in de vorige stap is gegeven:

```azurepowershell-interactive
New-GuestConfigurationPackage `
  -Name 'AuditFilePathExists' `
  -Configuration './Config/AuditFilePathExists.mof' `
  -ChefInSpecProfilePath './'
```

Nadat u het configuratie pakket hebt gemaakt, maar voordat u het naar Azure publiceert, kunt u het pakket testen vanaf uw werk station of CI/CD-omgeving. De GuestConfiguration-cmdlet `Test-GuestConfigurationPackage` bevat dezelfde agent in uw ontwikkel omgeving als wordt gebruikt binnen Azure-machines. Met deze oplossing kunt u integratie testen lokaal uitvoeren voordat u de gefactureerde Cloud omgevingen uitbrengt.

Omdat de agent de lokale omgeving werkelijk evalueert, moet u in de meeste gevallen de test-cmdlet uitvoeren op hetzelfde OS-platform als u wilt controleren.

Para meters van de `Test-GuestConfigurationPackage` cmdlet:

- **Naam**: naam van het gast configuratie beleid.
- **Para meter**: beleids parameters die zijn opgegeven in de hashtabel-indeling.
- **Pad**: volledig pad van het gast configuratie pakket.

Voer de volgende opdracht uit om het pakket te testen dat door de vorige stap is gemaakt:

```azurepowershell-interactive
Test-GuestConfigurationPackage `
  -Path ./AuditFilePathExists/AuditFilePathExists.zip
```

De cmdlet ondersteunt ook invoer van de Power shell-pijp lijn. Pipet de uitvoer van de `New-GuestConfigurationPackage` cmdlet naar de `Test-GuestConfigurationPackage` cmdlet.

```azurepowershell-interactive
New-GuestConfigurationPackage -Name AuditFilePathExists -Configuration ./Config/AuditFilePathExists.mof -ChefProfilePath './' | Test-GuestConfigurationPackage
```

De volgende stap is het publiceren van het bestand naar de Blob-opslag. Het onderstaande script bevat een functie die u kunt gebruiken om deze taak te automatiseren. Voor de opdrachten die in de functie worden gebruikt, `publish` is de `Az.Storage` module vereist.

```azurepowershell-interactive
function publish {
    param(
    [Parameter(Mandatory=$true)]
    $resourceGroup,
    [Parameter(Mandatory=$true)]
    $storageAccountName,
    [Parameter(Mandatory=$true)]
    $storageContainerName,
    [Parameter(Mandatory=$true)]
    $filePath,
    [Parameter(Mandatory=$true)]
    $blobName
    )

    # Get Storage Context
    $Context = Get-AzStorageAccount -ResourceGroupName $resourceGroup `
        -Name $storageAccountName | `
        ForEach-Object { $_.Context }

    # Upload file
    $Blob = Set-AzStorageBlobContent -Context $Context `
        -Container $storageContainerName `
        -File $filePath `
        -Blob $blobName `
        -Force

    # Get url with SAS token
    $StartTime = (Get-Date)
    $ExpiryTime = $StartTime.AddYears('3')  # THREE YEAR EXPIRATION
    $SAS = New-AzStorageBlobSASToken -Context $Context `
        -Container $storageContainerName `
        -Blob $blobName `
        -StartTime $StartTime `
        -ExpiryTime $ExpiryTime `
        -Permission rl `
        -FullUri

    # Output
    return $SAS
}

# replace the $storageAccountName value below, it must be globally unique
$resourceGroup        = 'policyfiles'
$storageAccountName   = 'youraccountname'
$storageContainerName = 'artifacts'

$uri = publish `
  -resourceGroup $resourceGroup `
  -storageAccountName $storageAccountName `
  -storageContainerName $storageContainerName `
  -filePath ./AuditFilePathExists.zip `
  -blobName 'AuditFilePathExists'
```
Nadat een aangepast beleids pakket voor de gast configuratie is gemaakt en geüpload, maakt u de beleids definitie voor het gast configuratie beleid. De `New-GuestConfigurationPolicy` cmdlet gebruikt een aangepast beleids pakket en maakt een beleids definitie.

Para meters van de `New-GuestConfigurationPolicy` cmdlet:

- **ContentUri**: open bare http (s)-URI van het inhouds pakket voor de gast configuratie.
- **DisplayName**: weergave naam beleid.
- **Beschrijving**: beschrijving van het beleid.
- **Para meter**: beleids parameters die zijn opgegeven in de hashtabel-indeling.
- **Versie**: beleids versie.
- **Pad**: doelpad voor het maken van beleids definities.
- **Platform**: doel platform (Windows/Linux) voor gast configuratie beleid en inhouds pakket.
- **Tag** voegt een of meer label filters toe aan de beleids definitie
- **Categorie** stelt het veld meta gegevens van categorie in de beleids definitie in

In het volgende voor beeld worden de beleids definities in een opgegeven pad van een aangepast beleids pakket gemaakt:

```azurepowershell-interactive
New-GuestConfigurationPolicy `
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Platform 'Linux' `
    -Version 1.0.0 `
    -Verbose
```

De volgende bestanden worden gemaakt door `New-GuestConfigurationPolicy` :

- **auditIfNotExists.jsop**
- **deployIfNotExists.jsop**
- **Initiative.jsop**

De cmdlet-uitvoer retourneert een object dat de weergave naam en het pad van de beleids bestanden bevat.

Ten slotte publiceert u de beleids definities met de `Publish-GuestConfigurationPolicy` cmdlet. De cmdlet heeft alleen de para meter **Path** die verwijst naar de locatie van de json-bestanden die zijn gemaakt door `New-GuestConfigurationPolicy` .

Als u de opdracht publiceren wilt uitvoeren, moet u toegang hebben tot beleids regels maken in Azure. De specifieke autorisatie vereisten worden beschreven op de pagina [overzicht van Azure Policy](../overview.md) . De beste ingebouwde rol is Inzender voor **resource beleid**.

```azurepowershell-interactive
Publish-GuestConfigurationPolicy `
  -Path '.\policyDefinitions'
```

 De `Publish-GuestConfigurationPolicy` cmdlet accepteert het pad van de Power shell-pijp lijn. Deze functie houdt in dat u de beleids bestanden kunt maken en publiceren in één set opdrachten die zijn gesluisd.

 ```azurepowershell-interactive
 New-GuestConfigurationPolicy `
  -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
  -DisplayName 'Audit Linux file path.' `
  -Description 'Audit that a file path exists on a Linux machine.' `
  -Path './policies' `
 | Publish-GuestConfigurationPolicy
 ```

Met het beleid dat is gemaakt in azure, is de laatste stap het initiatief toe te wijzen. Zie hoe u het initiatief toewijst met [Portal](../assign-policy-portal.md), [Azure cli](../assign-policy-azurecli.md)en [Azure PowerShell](../assign-policy-powershell.md).

> [!IMPORTANT]
> Gast configuratie beleidsregels moeten **altijd** worden toegewezen met het initiatief waarbij het beleid voor _AuditIfNotExists_ en _DeployIfNotExists_ wordt gecombineerd. Als alleen het _AuditIfNotExists_ -beleid is toegewezen, worden de vereisten niet geïmplementeerd en wordt in het beleid altijd weer gegeven dat ' 0 '-servers compatibel zijn.

Voor het toewijzen van een beleids definitie met _DeployIfNotExists_ -effect is een extra toegangs niveau vereist. Als u de minimale bevoegdheid wilt verlenen, kunt u een aangepaste roldefinitie maken die de **Inzender voor resource beleid**uitbreidt. In het onderstaande voor beeld maakt u een rol met de naam **resource Policy CONTRIBUTOR Dine** met de extra machtiging _micro soft. Authorization/roleAssignments/write_.

```azurepowershell-interactive
$subscriptionid = '00000000-0000-0000-0000-000000000000'
$role = Get-AzRoleDefinition "Resource Policy Contributor"
$role.Id = $null
$role.Name = "Resource Policy Contributor DINE"
$role.Description = "Can assign Policies that require remediation."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Authorization/roleAssignments/write")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/$subscriptionid")
New-AzRoleDefinition -Role $role
```

### <a name="using-parameters-in-custom-guest-configuration-policies"></a>Para meters gebruiken in aangepaste gast configuratie beleidsregels

Gast configuratie ondersteunt het overschrijven van eigenschappen van een configuratie tijdens runtime. Deze functie betekent dat de waarden in het MOF-bestand in het pakket niet als statisch moeten worden beschouwd. De onderdrukkings waarden worden geleverd via Azure Policy en hebben geen invloed op de manier waarop de configuraties worden gemaakt of gecompileerd.

Met Inspec worden para meters doorgaans verwerkt als invoer tijdens runtime of als code met behulp van kenmerken. Gast configuratie maakt dit proces verborgen, zodat er een invoer kan worden opgegeven wanneer het beleid is toegewezen. Er wordt automatisch een kenmerk bestand gemaakt op de machine. U hoeft geen bestand te maken en toe te voegen aan uw project. Er zijn twee stappen om para meters toe te voegen aan uw Linux-controle project.

Definieer de invoer in het ruby-bestand waar u scripteert wat moet worden gecontroleerd op de computer. Hieronder vindt u een voor beeld.

```Ruby
attr_path = attribute('path', description: 'The file path to validate.')

describe file(attr_path) do
    it { should exist }
end
```

De cmdlets `New-GuestConfigurationPolicy` en `Test-GuestConfigurationPolicyPackage` bevatten een para meter met de naam **para meter**. Met deze para meter wordt een hashtabel met alle details van elke para meter en worden automatisch alle vereiste secties gemaakt van de bestanden die worden gebruikt voor het maken van elke Azure Policy definitie.

In het volgende voor beeld wordt een beleids definitie gemaakt om een bestandspad te controleren. de gebruiker geeft het pad op naar het tijdstip van de beleids toewijzing.

```azurepowershell-interactive
$PolicyParameterInfo = @(
    @{
        Name = 'FilePath'                             # Policy parameter name (mandatory)
        DisplayName = 'File path.'                    # Policy parameter display name (mandatory)
        Description = "File path to be audited."      # Policy parameter description (optional)
        ResourceType = "ChefInSpecResource"           # Configuration resource type (mandatory)
        ResourceId = 'Audit Linux path exists'        # Configuration resource property name (mandatory)
        ResourcePropertyName = "AttributesYmlContent" # Configuration resource property name (mandatory)
        DefaultValue = '/tmp'                         # Policy parameter default value (optional)
    }
)

# The hashtable also supports a property named 'AllowedValues' with an array of strings to limit input to a list

New-GuestConfigurationPolicy
    -ContentUri 'https://storageaccountname.blob.core.windows.net/packages/AuditFilePathExists.zip?st=2019-07-01T00%3A00%3A00Z&se=2024-07-01T00%3A00%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=JdUf4nOCo8fvuflOoX%2FnGo4sXqVfP5BYXHzTl3%2BovJo%3D' `
    -DisplayName 'Audit Linux file path.' `
    -Description 'Audit that a file path exists on a Linux machine.' `
    -Path './policies' `
    -Parameter $PolicyParameterInfo `
    -Version 1.0.0
```

Voor Linux-beleid voegt u de eigenschap **AttributesYmlContent** in uw configuratie toe en overschrijft u de waarden naar behoefte. De gast configuratie agent maakt automatisch het YAML-bestand dat wordt gebruikt door de specificatie voor het opslaan van kenmerken. Zie het voorbeeld hieronder.

```powershell
Configuration AuditFilePathExists
{
    Import-DscResource -ModuleName 'GuestConfiguration'

    Node AuditFilePathExists
    {
        ChefInSpecResource 'Audit Linux path exists'
        {
            Name = 'linux-path'
            AttributesYmlContent = "path: /tmp"
        }
    }
}
```

## <a name="policy-lifecycle"></a>Levens duur van beleid

Voor het vrijgeven van een update voor de beleids definitie zijn er twee velden waarvoor aandacht is vereist.

- **Versie**: wanneer u de `New-GuestConfigurationPolicy` cmdlet uitvoert, moet u een versie nummer opgeven dat groter is dan het aantal dat momenteel is gepubliceerd. De eigenschap werkt de versie van de toewijzing van de gast configuratie bij, zodat de agent het bijgewerkte pakket herkent.
- **contentHash**: deze eigenschap wordt automatisch bijgewerkt door de `New-GuestConfigurationPolicy` cmdlet. Het is een hash-waarde van het pakket dat is gemaakt door `New-GuestConfigurationPackage` . De eigenschap moet correct zijn voor het `.zip` bestand dat u publiceert. Als alleen de eigenschap **contentUri** is bijgewerkt, wordt het inhouds pakket niet geaccepteerd door de extensie.

De eenvoudigste manier om een bijgewerkt pakket vrij te geven, is het proces dat wordt beschreven in dit artikel herhalen en een bijgewerkt versie nummer opgeven. Dit proces garandeert dat alle eigenschappen correct zijn bijgewerkt.


### <a name="filtering-guest-configuration-policies-using-tags"></a>Beleid voor gast configuratie filteren met behulp van Tags

Het beleid dat wordt gemaakt door cmdlets in de module gast configuratie kan optioneel een filter voor labels bevatten. De para meter **-tag** van `New-GuestConfigurationPolicy` ondersteunt een matrix met hashtabellen die afzonderlijke tags bevatten. De tags worden toegevoegd aan de `If` sectie van de beleids definitie en kunnen niet worden gewijzigd door een beleids toewijzing.

Hieronder vindt u een voorbeeld fragment van een beleids definitie die wordt gefilterd op labels.

```json
"if": {
  "allOf" : [
    {
      "allOf": [
        {
          "field": "tags.Owner",
          "equals": "BusinessUnit"
        },
        {
          "field": "tags.Role",
          "equals": "Web"
        }
      ]
    },
    {
      // Original Guest Configuration content will follow
    }
  ]
}
```

## <a name="optional-signing-guest-configuration-packages"></a>Optioneel: gast configuratie pakketten ondertekenen

Het aangepaste beleid voor gast configuratie gebruikt SHA256-hash om te valideren dat het beleids pakket niet is gewijzigd.
Klanten kunnen eventueel ook een certificaat gebruiken om pakketten te ondertekenen en de gast configuratie-extensie dwingen alleen ondertekende inhoud toe te staan.

Om dit scenario in te scha kelen, zijn er twee stappen die u moet volt ooien. Voer de cmdlet uit om het inhouds pakket te ondertekenen en voeg een tag toe aan de machines waarvoor code moet worden ondertekend.

Als u de functie voor handtekening validatie wilt gebruiken, voert `Protect-GuestConfigurationPackage` u de cmdlet uit om het pakket te ondertekenen voordat het wordt gepubliceerd. Voor deze cmdlet is het certificaat voor ondertekening van programma code vereist.

Para meters van de `Protect-GuestConfigurationPackage` cmdlet:

- **Pad**: volledig pad van het gast configuratie pakket.
- **PublicGpgKeyPath**: pad naar de open bare GPG-sleutel. Deze para meter wordt alleen ondersteund bij het ondertekenen van inhoud voor Linux.

Een goede referentie voor het maken van GPG-sleutels voor gebruik met Linux-machines wordt verschaft door een artikel op GitHub, waardoor [een nieuwe GPG-sleutel wordt gegenereerd](https://help.github.com/en/articles/generating-a-new-gpg-key).

GuestConfiguration-agent verwacht dat de open bare sleutel van het certificaat aanwezig is in het pad `/usr/local/share/ca-certificates/extra` op Linux-machines. Installeer de open bare sleutel van het certificaat op de computer voordat u het aangepaste beleid toepast, zodat het knoop punt ondertekende inhoud verifieert. Dit proces kan worden uitgevoerd met behulp van een wille keurige techniek binnen de virtuele machine of met behulp van Azure Policy. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-push-certificate-windows)vindt u een voorbeeld sjabloon.
Het Key Vault toegangs beleid moet de compute-resource provider toegang geven tot certificaten tijdens implementaties. Zie [Key Vault instellen voor virtuele machines in azure Resource Manager](../../../virtual-machines/windows/key-vault-setup.md#use-templates-to-set-up-key-vault)voor gedetailleerde stappen.

Nadat de inhoud is gepubliceerd, voegt u een tag met de naam `GuestConfigPolicyCertificateValidation` en waarde `enabled` toe aan alle virtuele machines waarvoor ondertekening van programma code vereist is. Zie de voor [beelden](../samples/built-in-policies.md#tags) van tags voor de manier waarop Tags op schaal kunnen worden geleverd met behulp van Azure Policy. Zodra deze tag is geïmplementeerd, maakt de beleids definitie die is gegenereerd met de `New-GuestConfigurationPolicy` cmdlet, de vereiste via de gast configuratie-extensie.

## <a name="troubleshooting-guest-configuration-policy-assignments-preview"></a>Problemen met toewijzing van gast configuratie beleid oplossen (preview-versie)

Er is een hulp programma beschikbaar in de preview-versie om te helpen bij het oplossen van problemen Azure Policy toewijzing van gast configuraties. Het hulp programma is in Preview en gepubliceerd op de PowerShell Gallery als module naam [gast configuratie probleem Oplosser](https://www.powershellgallery.com/packages/GuestConfigurationTroubleshooter/).

Voor meer informatie over de cmdlets in dit hulp programma gebruikt u de opdracht Get-Help in Power shell om de ingebouwde richt lijnen weer te geven. Wanneer het hulp programma veelvuldige updates ontvangt, is dat de beste manier om de meest recente informatie te verkrijgen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het controleren van Vm's met [gast configuratie](../concepts/guest-configuration.md).
- Meer informatie over het [programmatisch maken van beleids regels](./programmatically-create.md).
- Meer informatie over het [ophalen van compatibiliteits gegevens](./get-compliance-data.md).
