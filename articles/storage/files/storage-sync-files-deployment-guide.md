---
title: Azure Files Sync implementeren | Microsoft Docs
description: Informatie over het implementeren van Azure File Sync van begin tot eind.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 0913e1877c63ed1a8e960676be02a12b45a34a7d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240100"
---
# <a name="deploy-azure-file-sync"></a>Azure Files SYNC implementeren
Gebruik Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, NFS en FTPS gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

Wordt aangeraden dat u leest [Planning voor de implementatie van Azure Files](storage-files-planning.md) en [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) voordat u de stappen in dit artikel beschreven.

## <a name="prerequisites"></a>Vereisten
* Een Azure-bestandsshare in dezelfde regio die u wilt implementeren van Azure File Sync. Zie voor meer informatie:
    - [Beschikbaarheid in regio](storage-sync-files-planning.md#region-availability) voor Azure File Sync.
    - [Een bestandsshare maken](storage-how-to-create-file-share.md) voor een stapsgewijze beschrijving van het maken van een bestandsshare.
* Ten minste één exemplaar van Windows Server of Windows Server-cluster om te synchroniseren met Azure File Sync ondersteund. Zie voor meer informatie over ondersteunde versies van Windows Server [compatibiliteit met Windows Server](storage-sync-files-planning.md#azure-file-sync-system-requirements-and-interoperability).
* De Az-PowerShell-module kan worden gebruikt met PowerShell 5.1 of PowerShell 6 +. U kunt de Az-PowerShell-module voor Azure File Sync gebruiken voor elk ondersteund systeem, met inbegrip van niet-Windows-systemen, maar de cmdlet voor de server-registratie moet altijd worden uitgevoerd rechtstreeks op de Windows Server-instantie die u registreert. Op Windows Server 2012 R2, kunt u controleren of u ten minste uitgevoerd PowerShell 5.1. \* door te kijken naar de waarde van de **PSVersion** eigenschap van de **$PSVersionTable** object:

    ```powershell
    $PSVersionTable.PSVersion
    ```

    Als de waarde van de PSVersion minder dan 5.1 is. \*, worden als het geval is bij de meeste nieuwe installaties van Windows Server 2012 R2, u kunt vervolgens gemakkelijk upgraden door te downloaden en installeren van [Windows Management Framework (WMF) 5.1](https://www.microsoft.com/download/details.aspx?id=54616). Het juiste pakket downloaden en installeren voor Windows Server 2012 R2 is **Win8.1AndW2K12R2 KB\*\*\*\*\*\*\*-x64.msu**. 

    PowerShell 6 + kan worden gebruikt met een ondersteund systeem en kan worden gedownload de [GitHub-pagina](https://github.com/PowerShell/PowerShell#get-powershell). 

    > [!Important]  
    > Als u van plan bent te gebruiken in de gebruikersinterface van de registratie van Server, in plaats van rechtstreeks vanuit PowerShell registreren, moet u PowerShell 5.1.

* Als u ervoor hebt gekozen om te gebruiken PowerShell 5.1, zorg ervoor dat op zijn minst .NET 4.7.2 is geïnstalleerd. Meer informatie over [.NET Framework-versies en afhankelijkheden van](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies) op uw systeem.
* De Az-PowerShell-module, die kan worden geïnstalleerd door de instructies hier: [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-Az-ps). 
* De module Az.StorageSync onafhankelijk van de Az-module is geïnstalleerd:

    ```PowerShell
    Install-Module Az.StorageSync -AllowClobber
    ```

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Windows Server voorbereiden voor gebruik met Azure File Sync
Voor elke server die u gebruiken met Azure File Sync wilt, met inbegrip van elk serverknooppunt in een failovercluster uitschakelen **verbeterde beveiliging van Internet Explorer**. Dit is alleen vereist voor registratie van de oorspronkelijke server. U kunt de optie opnieuw inschakelen nadat de server is geregistreerd.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
1. Open Server Manager.
2. Klik op **lokale Server**:  
    !['Lokale Server' aan de linkerkant van de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
3. Selecteer in het deelvenster **Eigenschappen** de koppeling naar **Verbeterde beveiliging van Internet Explorer**.  
    ![Het deelvenster 'Internet Explorer Verbeterde beveiliging' in de Serverbeheer-UI](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
4. In de **verbeterde beveiliging van Internet Explorer** in het dialoogvenster, selecteer **uit** voor **beheerders** en **gebruikers**:  
    ![De verbeterde beveiliging van Internet Explorer-pop-upvenster met 'Uit' geselecteerd](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Als u wilt de verbeterde beveiliging van Internet Explorer uitschakelen, voer het volgende uit vanuit een PowerShell-sessie met verhoogde bevoegdheden:

```powershell
# Disable Internet Explorer Enhanced Security Configuration 
# for Administrators
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Disable Internet Explorer Enhanced Security Configuration 
# for Users
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A8-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0 -Force

# Force Internet Explorer closed, if open. This is required to fully apply the setting.
# Save any work you have open in the IE browser. This will not affect other browsers,
# including Microsoft Edge.
Stop-Process -Name iexplore -ErrorAction SilentlyContinue
``` 

---

## <a name="deploy-the-storage-sync-service"></a>Opslagsynchronisatieservice implementeren 
De implementatie van Azure File Sync begint met het plaatsen van een **Opslagsynchronisatieservice** resource in een resourcegroep van het geselecteerde abonnement. U wordt aangeraden inrichten als enkele van deze indien nodig. Maakt u een vertrouwensrelatie tot stand tussen uw servers en deze resource en een server kan alleen worden geregistreerd bij een Opslagsynchronisatieservice. Het verdient daarom zo veel opslagsynchronisatieservices als nodig is voor afzonderlijke groepen van servers implementeren. Houd er rekening mee dat servers van andere opslagsynchronisatieservices kunnen niet met elkaar synchroniseren.

> [!Note]
> De Opslagsynchronisatieservice overgenomen machtigingen van het abonnement en de resourcegroep die is geïmplementeerd in. Het is raadzaam dat u zorgvuldig controleren wie toegang heeft tot deze. Entiteiten met schrijftoegang kunnen beginnen met het synchroniseren van nieuwe sets met bestanden van servers die zijn geregistreerd bij deze opslag synchronisatieservice en ervoor zorgen dat gegevens naar de Azure-opslag die toegankelijk is voor deze stromen.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Voor het implementeren van een Opslagsynchronisatieservice, gaat u naar de [Azure-portal](https://portal.azure.com/), klikt u op *een resource maken* en zoek vervolgens naar Azure File Sync. Selecteer in de lijst met zoekresultaten **Azure File Sync**, en selecteer vervolgens **maken** openen de **Opslagsynchronisatieservice implementeren** tabblad.

Voer de volgende gegevens in in het deelvenster dat verschijnt:

- **Naam**: Een unieke naam (per abonnement) voor opslagsynchronisatieservice.
- **Abonnement**: Het abonnement waarin u wilt maken van de Opslagsynchronisatieservice. Afhankelijk van de strategie van de configuratie van uw organisatie wellicht u toegang tot een of meer abonnementen. Een Azure-abonnement is de meest eenvoudige container voor de facturering voor elke cloudservice (zoals Azure-bestanden).
- **Resourcegroep**: Een resourcegroep is een logische groep Azure-resources, zoals een storage-account of een Opslagsynchronisatieservice. U kunt een nieuwe resourcegroep maken of een bestaande resourcegroep gebruiken voor Azure File Sync. (Wordt u aangeraden resourcegroepen als containers voor het isoleren van bronnen logisch voor uw organisatie, zoals het groeperen van HR-resources of resources voor een specifiek project.)
- **Locatie**: De regio waarin u wilt implementeren van Azure File Sync. Alleen ondersteunde regio's zijn beschikbaar in deze lijst.

Wanneer u klaar bent, selecteert u **maken** de Opslagsynchronisatieservice implementeren.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Vervang **< Az_Region >** , **< RG_Name >** , en **< my_storage_sync_service >** door uw eigen waarden vervolgens gebruiken de volgende opdrachten om te maken en implementeren van een Opslagsynchronisatieservice:

```powershell
Connect-AzAccount

# this variable holds the Azure region you want to deploy 
# Azure File Sync into
$region = '<Az_Region>'

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = @()
Get-AzLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the selected Azure Region or the region is mistyped.")
}

# the resource group to deploy the Storage Sync Service into
$resourceGroup = '<RG_Name>'

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = @()
Get-AzResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    New-AzResourceGroup -Name $resourceGroup -Location $region
}

$storageSyncName = "<my_storage_sync_service>"
$storageSync = New-AzStorageSyncService -ResourceGroupName $resourceGroup -Name $storageSyncName -Location $region
```

---

## <a name="install-the-azure-file-sync-agent"></a>Azure File Sync-agent installeren
De Azure File Sync-agent is een downloadbaar pakket waardoor Windows Server met een Azure-bestandsshare kan worden gesynchroniseerd. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
U kunt de agent uit downloaden de [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=858257). Wanneer het downloaden is voltooid, dubbelklikt u op het MSI-pakket voor het starten van de installatie van de Azure File Sync-agent.

> [!Important]  
> Als u van plan bent te gebruiken van Azure File Sync met een failovercluster, moet de Azure File Sync-agent worden geïnstalleerd op elk knooppunt in het cluster. Elk knooppunt in het cluster moet worden geregistreerd voor het werken met Azure File Sync.

U wordt aangeraden dat u het volgende doen:
- Laat het standaardinstallatiepad (C:\Program Files\Azure\StorageSyncAgent), voor het oplossen van problemen en server onderhoud eenvoudiger.
- Schakel Microsoft Update om Azure File Sync up-to-date te houden. Alle updates optreden naar de Azure File Sync-agent, met inbegrip van functie-updates en hotfixes, vanaf Microsoft Update. Het is raadzaam om de meest recente update installeren op Azure File Sync. Zie voor meer informatie, [updatebeleid Azure File Sync](storage-sync-files-planning.md#azure-file-sync-agent-update-policy).

Wanneer de installatie van de Azure File Sync-agent is voltooid, wordt de gebruikersinterface van de registratie van Server automatisch wordt geopend. U moet een Opslagsynchronisatieservice hebben voordat u registreert; Zie de volgende sectie voor het maken van een Opslagsynchronisatieservice.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Voer de volgende PowerShell-code voor het downloaden van de juiste versie van de Azure File Sync-agent voor uw besturingssysteem en installeer deze op uw systeem.

> [!Important]  
> Als u van plan bent te gebruiken van Azure File Sync met een failovercluster, moet de Azure File Sync-agent worden geïnstalleerd op elk knooppunt in het cluster. Elk knooppunt in het cluster moet worden geregistreerd voor het werken met Azure File Sync.

```powershell
# Gather the OS version
$osver = [System.Environment]::OSVersion.Version

# Download the appropriate version of the Azure File Sync agent for your OS.
if ($osver.Equals([System.Version]::new(10, 0, 17763, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2019 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(10, 0, 14393, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2016 `
        -OutFile "StorageSyncAgent.msi" 
} elseif ($osver.Equals([System.Version]::new(6, 3, 9600, 0))) {
    Invoke-WebRequest `
        -Uri https://aka.ms/afs/agent/Server2012R2 `
        -OutFile "StorageSyncAgent.msi" 
} else {
    throw [System.PlatformNotSupportedException]::new("Azure File Sync is only supported on Windows Server 2012 R2, Windows Server 2016, and Windows Server 2019")
}

# Install the MSI. Start-Process is used to PowerShell blocks until the operation is complete.
# Note that the installer currently forces all PowerShell sessions closed - this is a known issue.
Start-Process -FilePath "StorageSyncAgent.msi" -ArgumentList "/quiet" -Wait

# Note that this cmdlet will need to be run in a new session based on the above comment.
# You may remove the temp folder containing the MSI and the EXE installer
Remove-Item -Path ".\StorageSyncAgent.msi" -Recurse -Force
```

---

## <a name="register-windows-server-with-storage-sync-service"></a>Windows Server registreren met opslagsynchronisatieservice
Als u Windows Server registreert voor een opslagsynchronisatieservice, wordt er een vertrouwensrelatie ingesteld tussen uw server (of cluster) en de opslagsynchronisatieservice. Een server kan slechts voor één opslagsynchronisatieservice worden geregistreerd en kan worden gesynchroniseerd met andere servers en Azure-bestandsshares die aan dezelfde opslagsynchronisatieservice zijn gekoppeld.

> [!Note]
> De registratie van de uw Azure-referenties gebruikt voor het maken van een vertrouwensrelatie tussen de Opslagsynchronisatieservice en uw Windows-Server, maar vervolgens de server maakt en gebruikt een eigen identiteit die is geldig als de server blijft ingeschreven en de huidige Shared Access Signature-token (SAS-opslag) is geldig. Een nieuwe SAS-token kan niet worden uitgegeven voor de server nadat de server geregistreerd is, dus verwijdert de mogelijkheid van de server voor toegang tot uw Azure-bestandsshares, elke synchronisatie stoppen.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
De gebruikersinterface van de registratie van Server wordt automatisch geopend na de installatie van de Azure File Sync-agent. Als dat niet zo is, kunt u de interface handmatig openen vanaf de bestandslocatie: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe. Wanneer de gebruikersinterface van de registratie van Server wordt geopend, selecteert u **aanmelden** om te beginnen.

Nadat u zich hebt aangemeld, wordt u gevraagd om de volgende informatie:

![Schermafbeelding van de gebruikersinterface van de serverregistratie](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure-abonnement**: Het abonnement met de Opslagsynchronisatieservice (Zie [implementeren de Opslagsynchronisatieservice](#deploy-the-storage-sync-service)). 
- **Resourcegroep**: De resourcegroep die de opslagsynchronisatieservice bevat.
- **Opslagsynchronisatieservice**: De naam van de Opslagsynchronisatieservice waarmee u wilt registreren.

Nadat u de juiste gegevens hebt geselecteerd, selecteert u **registreren** om de serverregistratie te voltooien. Als deel van het registratieproces wordt u gevraagd u nogmaals aan te melden.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
```powershell
$registeredServer = Register-AzStorageSyncServer -ParentObject $storageSync
```

---

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Synchronisatiegroep en cloudeindpunt maken
Een synchronisatiegroep definieert de synchronisatietopologie voor een verzameling bestanden. Eindpunten binnen een synchronisatiegroep worden onderling synchroon gehouden. Een synchronisatiegroep moet één cloudeindpunt bevatten, dat een Azure-bestandsshare en een of meer servereindpunten representeert. Een servereindpunt vertegenwoordigt een pad op de geregistreerde server. Een server kan servereindpunten in meerdere synchronisatiegroepen hebben. U kunt zoveel synchronisatiegroepen als nodig is voor de topologie van uw gewenste synchronisatie op de juiste wijze te beschrijven.

Een cloudeindpunt is een aanwijzer naar een Azure-bestandsshare. Alle servereindpunten worden gesynchroniseerd met een cloudeindpunt, waardoor het cloudeindpunt van de hub. Het opslagaccount voor de Azure-bestandsshare moet zich bevinden in dezelfde regio als de Opslagsynchronisatieservice. Het geheel van de Azure-bestandsshare wordt gesynchroniseerd, met één uitzondering: Een speciale map, vergelijkbaar met de verborgen "System Volume Information"-map op een NTFS-volume, worden ingericht. Deze map heet '. SystemShareInformation'. Het bevat metagegevens met belangrijke synchroniseren die niet worden gesynchroniseerd naar andere eindpunten. Gebruik of verwijder deze niet!

> [!Important]  
> U kunt wijzigingen aanbrengen in een cloud-eindpunt of een servereindpunt in de groep voor synchronisatie en de bestanden zijn gesynchroniseerd met de andere eindpunten in de groep voor synchronisatie. Als u een wijziging rechtstreeks met het cloudeindpunt (Azure-bestandsshare) aanbrengt, moeten wijzigingen eerst worden gedetecteerd door een Azure File Sync-taak wijzigen detectie. Een taak wijzigen-detectie wordt slechts één keer voor elke 24 uur voor een cloudeindpunt gestart. Zie voor meer informatie, [Veelgestelde vragen over Azure-bestanden](storage-files-faq.md#afs-change-detection).

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Maken van een synchronisatiegroep in de [Azure-portal](https://portal.azure.com/), gaat u naar de Opslagsynchronisatieservice en selecteer vervolgens **+ groep voor synchronisatie met**:

![Een nieuwe synchronisatiegroep in de Azure-portal maken](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

Voer in het deelvenster dat verschijnt de volgende gegevens in om een synchronisatiegroep met een cloudeindpunt te maken:

- **Naam van synchronisatiegroep**: De naam van de groep voor synchronisatie moet worden gemaakt. Deze naam moet uniek zijn binnen de opslagsynchronisatieservice, maar het mag een willekeurige naam zijn die u makkelijk kunt onthouden.
- **Abonnement**: Het abonnement waarin u de Opslagsynchronisatieservice in geïmplementeerd [implementeren de Opslagsynchronisatieservice](#deploy-the-storage-sync-service).
- **Opslagaccount**: Als u selecteert **storage-account selecteren**, een andere deelvenster wordt weergegeven waarin u het opslagaccount met de Azure-bestandsshare die u synchroniseren wilt met kunt selecteren.
- **Azure-bestandsshare**: De naam van de Azure-bestandsshare die u wilt synchroniseren.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Voer de volgende PowerShell voor het maken van de groep voor synchronisatie. Vervang `<my-sync-group>` door de gewenste naam van de groep voor synchronisatie.

```powershell
$syncGroupName = "<my-sync-group>"
$syncGroup = New-AzStorageSyncGroup -ParentObject $storageSync -Name $syncGroupName
```

Wanneer de synchronisatiegroep is gemaakt, kunt u uw cloudeindpunt maken. Vervang `<my-storage-account>` en `<my-file-share>` met de verwachte waarden.

```powershell
# Get or create a storage account with desired name
$storageAccountName = "<my-storage-account>"
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup | Where-Object {
    $_.StorageAccountName -eq $storageAccountName
}

if ($storageAccount -eq $null) {
    $storageAccount = New-AzStorageAccount `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup `
        -Location $region `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly:$true
}

# Get or create an Azure file share within the desired storage account
$fileShareName = "<my-file-share>"
$fileShare = Get-AzStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $fileShareName -and $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    $fileShare = New-AzStorageShare -Context $storageAccount.Context -Name $fileShareName
}

# Create the cloud endpoint
New-AzStorageSyncCloudEndpoint `
    -Name $fileShare.Name `
    -ParentObject $syncGroup `
    -StorageAccountResourceId $storageAccount.Id `
    -AzureFileShareName $fileShare.Name
```

---

## <a name="create-a-server-endpoint"></a>Servereindpunt maken
Een servereindpunt representeert een bepaalde locatie op een geregistreerde server, bijvoorbeeld een map op een servervolume. Een servereindpunt moet een pad op een geregistreerde server (in plaats van een gekoppelde share) en het pad moet voor het gebruik van cloud-opslaglagen op een niet-systeemvolume. NAS-opslag (NAS) wordt niet ondersteund.

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Een servereindpunt toevoegen, gaat u naar de zojuist gemaakte synchronisatiegroep en selecteer vervolgens **servereindpunt toevoegen**.

![Nieuw servereindpunt toevoegen in het deelvenster met de synchronisatiegroep](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

Voer in het deelvenster **Servereindpunt toevoegen** de volgende gegevens in om een servereindpunt te maken:

- **Geregistreerde server**: De naam van de server of cluster waar u wilt maken van het servereindpunt.
- **Pad**: Het pad van de Windows Server worden gesynchroniseerd als onderdeel van de groep voor synchronisatie.
- **Cloud-Opslaglagen**: Overschakelen naar de in- of uitschakelen van cloud tiering. Met cloud tiering, niet vaak worden gebruikt of toegang krijgen tot bestanden kan worden lagen naar Azure Files.
- **Beschikbare volumeruimte**: De hoeveelheid vrije ruimte te reserveren op het volume waarop het servereindpunt zich bevindt. Bijvoorbeeld, als de vrije ruimte op volume is ingesteld op 50% op een volume dat een single servereindpunt heeft, is ongeveer de helft van de hoeveelheid gegevens gelaagd naar Azure Files. Ongeacht of cloud tiering is ingeschakeld, wordt uw Azure-bestandsshare is altijd een volledige kopie van de gegevens in de groep voor synchronisatie.

Als u wilt het servereindpunt toevoegen, selecteert u **maken**. Uw bestanden worden nu gesynchroniseerd gehouden in uw Azure-bestandsshare en Windows Server. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Voer de volgende PowerShell-opdrachten om te maken van het servereindpunt en vervang `<your-server-endpoint-path>` en `<your-volume-free-space>` met de gewenste waarden.

```powershell
$serverEndpointPath = "<your-server-endpoint-path>"
$cloudTieringDesired = $true
$volumeFreeSpacePercentage = <your-volume-free-space>

if ($cloudTieringDesired) {
    # Ensure endpoint path is not the system volume
    $directoryRoot = [System.IO.Directory]::GetDirectoryRoot($serverEndpointPath)
    $osVolume = "$($env:SystemDrive)\"
    if ($directoryRoot -eq $osVolume) {
        throw [System.Exception]::new("Cloud tiering cannot be enabled on the system volume")
    }

    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath `
        -CloudTiering `
        -VolumeFreeSpacePercent $volumeFreeSpacePercentage
} else {
    # Create server endpoint
    New-AzStorageSyncServerEndpoint `
        -Name $registeredServer.FriendlyName `
        -SyncGroup $syncGroup `
        -ServerResourceId $registeredServer.ResourceId `
        -ServerLocalPath $serverEndpointPath 
}
```

---

## <a name="onboarding-with-azure-file-sync"></a>Onboarding met Azure File Sync
De aanbevolen stappen voor Onboarding van Azure File Sync voor de eerste met nul downtime behoud van volledige kwaliteit en toegangsbeheerlijst (ACL) zijn als volgt:
 
1. Implementeer een Opslagsynchronisatieservice.
2. Maak een synchronisatiegroep.
3. Azure File Sync-agent installeren op de server met de volledige gegevensset.
4. Die server te registreren en maken van een servereindpunt op de share. 
5. Laat synchroniseren moet het volledige uploaden naar de Azure-bestandsshare (cloudeindpunt).  
6. Nadat de initiële uploaden voltooid is, installeert u Azure File Sync-agent op elk van de resterende servers.
7. Nieuwe bestandsshares maken op elk van de resterende servers.
8. Maak servereindpunten op nieuwe bestandsshares met beleidsinstelling voor lagen cloud, indien gewenst. (Deze stap vereist extra opslag beschikbaar voor de eerste installatie.)
9. Laat Azure File Sync-agent hiervoor een snel herstel van de volledige naamruimte zonder de werkelijke gegevens worden overgebracht. Na de synchronisatie van de volledige naamruimte, wordt synchronisatie-engine vult u de lokale schijfruimte op basis van de beleidsinstelling voor lagen cloud voor het servereindpunt van de. 
10. Controleer of de synchronisatie is voltooid en testen van uw topologie naar wens. 
11. Gebruikers en toepassingen naar deze nieuwe share omleiden.
12. Eventueel kunt u eventuele dubbele shares op de servers verwijderen.
 
Als u geen extra opslag voor initiële onboarding hebt en wilt koppelen aan de bestaande shares, kunt u de gegevens in de Azure-bestandsshares vooraf is ingevuld. Deze aanpak wordt voorgesteld, en alleen als u downtime accepteren en helemaal geen gegevens wijzigen op de servershares te tijdens het initiële onboarding-proces garanderen. 
 
1. Zorg ervoor dat de gegevens op een van de server niet tijdens het onboarding-proces wijzigen.
2. Vooraf zaad Azure-bestandsshares met de gegevens van de server met behulp van een hulpprogramma voor gegevensoverdracht via het SMB bijvoorbeeld Robocopy, directe SMB-kopie. Aangezien AzCopy geen gegevens via het SMB uploaden heeft zodat deze kan niet worden gebruikt voor het vooraf seeding.
3. Azure File Sync-topologie met de gewenste servereindpunten die verwijst naar de bestaande shares maken.
4. Laat de synchronisatie klaar bent met het proces voor afstemming op alle eindpunten. 
5. Zodra afstemming voltooid is, kunt u shares voor wijzigingen kunt openen.
 
Houd er rekening mee dat, vooraf seeding benadering momenteel enkele beperkingen heeft: 
- Vol vertrouwen op bestanden blijven niet behouden. Bijvoorbeeld verliezen bestanden ACL's en -timestamps.
- Gegevens worden gewijzigd op de server voordat de synchronisatie-topologie is volledig en die wordt uitgevoerd kunnen leiden tot conflicten op de servereindpunten.  
- Nadat het cloudeindpunt is gemaakt, wordt een proces voor het detecteren van de bestanden in de cloud voordat u begint met de eerste synchronisatie uitgevoerd in Azure File Sync. De tijd om dit proces te voltooien, is afhankelijk van de diverse factoren, zoals snelheid van het netwerk, beschikbare bandbreedte en het aantal bestanden en mappen. Voor de ruwe schatting in de preview-versie, wordt detectie uitgevoerd ongeveer op 10 bestanden per seconde.  Dus zelfs als vooraf seeding snel wordt uitgevoerd, de totale tijd om op te halen van een volledig werkend systeem mogelijk aanzienlijk langer wanneer gegevens vooraf geseede zich in de cloud.

## <a name="migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync"></a>Migreren van een DFS-replicatie (DFS-R)-implementatie naar Azure File Sync
Voor het migreren van een DFS-R-implementatie naar Azure File Sync:

1. Maak een synchronisatiegroep om weer te geven van de DFS-R-topologie die u wilt vervangen.
2. Start op de server met de volledige set gegevens in de topologie van uw DFS-R om te migreren. Azure File Sync op die server installeren.
3. Deze server te registreren en maken van een servereindpunt voor de eerste server moeten worden gemigreerd. Schakel geen cloud tiering.
4. Laat alle van de gegevenssynchronisatie naar uw Azure-bestandsshare (cloudeindpunt).
5. Installeer en registreer de Azure File Sync-agent op elk van de resterende DFS-R-servers.
6. Uitschakelen van DFS-R. 
7. Maak een servereindpunt op elk van de DFS-R-servers. Schakel geen cloud tiering.
8. Controleer of de synchronisatie is voltooid en testen van uw topologie naar wens.
9. Buiten gebruik stellen van DFS-R.
10. Warmtemeting mei cloud nu op een servereindpunt naar wens worden ingeschakeld.

Zie voor meer informatie, [Azure File Sync interop met Distributed File System (DFS)](storage-sync-files-planning.md#distributed-file-system-dfs).

## <a name="next-steps"></a>Volgende stappen
- [Toevoegen of verwijderen van een Azure File Sync-servereindpunt](storage-sync-files-server-endpoint.md)
- [Registreren of de registratie van een server met Azure File Sync](storage-sync-files-server-registration.md)
- [Monitor voor Azure File Sync](storage-sync-files-monitoring.md)
