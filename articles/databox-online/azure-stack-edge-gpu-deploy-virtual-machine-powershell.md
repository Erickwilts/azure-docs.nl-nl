---
title: Implementeer Vm's op uw Azure Stack Edge Pro GPU-apparaat via Azure PowerShell
description: Hierin wordt beschreven hoe u virtuele machines (Vm's) maakt en beheert op een Azure Stack Edge Pro GPU-apparaat met behulp van Azure PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 12/23/2020
ms.author: alkohli
ms.openlocfilehash: 32685207f8d6e81d03c90d01b186337ce79f843a
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/24/2020
ms.locfileid: "97763823"
---
# <a name="deploy-vms-on-your-azure-stack-edge-pro-gpu-device-via-azure-powershell"></a>Implementeer Vm's op uw Azure Stack Edge Pro GPU-apparaat via Azure PowerShell

In dit artikel wordt beschreven hoe u een virtuele machine op uw Azure Stack Edge Pro-apparaat maakt en beheert met behulp van Azure PowerShell. Dit artikel is van toepassing op Azure Stack Edge Pro GPU, Azure Stack Edge Pro R-en Azure Stack Edge mini-R-apparaten.

## <a name="vm-deployment-workflow"></a>VM-implementatiewerkstroom

De implementatiewerkstroom wordt afgebeeld in het volgende diagram.

![VM-implementatiewerkstroom](media/azure-stack-edge-gpu-deploy-virtual-machine-powershell/vm-workflow-r.svg)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [azure-stack-edge-gateway-deploy-vm-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]


## <a name="query-for-built-in-subscription-on-the-device"></a>Query voor ingebouwd abonnement op het apparaat

Voor Azure Resource Manager wordt slechts één door de gebruiker zichtbaar vast abonnement ondersteund. Dit abonnement is uniek per apparaat en de abonnements naam of abonnements-ID kan niet worden gewijzigd.

Dit abonnement bevat alle resources die zijn gemaakt voor het maken van VM'S. 

> [!IMPORTANT]
> Dit abonnement wordt gemaakt wanneer u virtuele machines inschakelt op het Azure Portal en lokaal op uw apparaat is.

Dit abonnement wordt gebruikt voor het implementeren van de Vm's.

1.  Als u dit abonnement wilt vermelden, typt u:

    ```powershell
    Get-AzureRmSubscription
    ```
    
    Hieronder ziet u een voorbeeld van de uitvoer.

    ```powershell
    PS C:\windows\system32> Get-AzureRmSubscription
    
    Name                 Id                 TenantId          State
    ----                 --                --------           -----
    Default Provider Subscription A4257FDE-B946-4E01-ADE7-674760B8D1A3 c0257de7-538f-415c-993a-1b87a031879d Enabled
    
    PS C:\windows\system32>
    ```
        
3.  Bekijk de lijst met de geregistreerde resource providers die op het apparaat worden uitgevoerd. Deze lijst omvat doorgaans compute, netwerk en opslag.

    ```powershell
    Get-AzureRMResourceProvider
    ```

    > [!NOTE]
    > De resource providers zijn vooraf geregistreerd en kunnen niet worden gewijzigd of gewijzigd.
    
    Hieronder ziet u een voorbeeld van de uitvoer:

    ```powershell
    Get-AzureRmResourceProvider
    ProviderNamespace : Microsoft.Compute
    RegistrationState : Registered
    ResourceTypes     : {virtualMachines, virtualMachines/extensions, locations, operations...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Network
    RegistrationState : Registered
    ResourceTypes     : {operations, locations, locations/operations, locations/usages...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Resources
    RegistrationState : Registered
    ResourceTypes     : {tenants, locations, providers, checkresourcename...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Storage
    RegistrationState : Registered
    ResourceTypes     : {storageaccounts, storageAccounts/blobServices, storageAccounts/tableServices,
                        storageAccounts/queueServices...}
    Locations         : {DBELocal}
    ZoneMappings      :
    ```
    
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Een resource groep is een logische container waarin de Azure-resources, zoals opslag account, schijf, beheerde schijf, worden geïmplementeerd en beheerd.

> [!IMPORTANT]
> Alle resources worden gemaakt op dezelfde locatie als die van het apparaat en de locatie is ingesteld op **DBELocal**.

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

Hieronder ziet u een voorbeeld van de uitvoer.

```powershell
New-AzureRmResourceGroup -Name rg191113014333 -Location DBELocal 
Successfully created Resource Group:rg191113014333
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Maak een nieuw opslag account met behulp van de resource groep die u in de vorige stap hebt gemaakt. Dit is een **lokaal opslag account** dat wordt gebruikt voor het uploaden van de installatie kopie van de virtuele schijf voor de VM.

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> Alleen de lokale opslag accounts, zoals lokaal redundante opslag (Standard_LRS of Premium_LRS), kunnen worden gemaakt via Azure Resource Manager. Als u gelaagde opslag accounts wilt maken, raadpleegt u de stappen in [toevoegen, verbinding maken met opslag accounts op uw Azure stack Edge Pro](azure-stack-edge-j-series-deploy-add-storage-accounts.md).

Hieronder ziet u een voorbeeld van de uitvoer.

```powershell
New-AzureRmStorageAccount -Name sa191113014333  -ResourceGroupName rg191113014333 -SkuName Standard_LRS -Location DBELocal

ResourceGroupName      : rg191113014333
StorageAccountName     : sa191113014333
Id                     : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Microsoft.Storage/storageaccounts/sa191113014333
Location               : DBELocal
Sku                    : Microsoft.Azure.Management.Storage.Models.Sku
Kind                   : Storage
Encryption             : Microsoft.Azure.Management.Storage.Models.Encryption
AccessTier             :
CreationTime           : 11/13/2019 9:43:49 PM
CustomDomain           :
Identity               :
LastGeoFailoverTime    :
PrimaryEndpoints       : Microsoft.Azure.Management.Storage.Models.Endpoints
PrimaryLocation        : DBELocal
ProvisioningState      : Succeeded
SecondaryEndpoints     :
SecondaryLocation      :
StatusOfPrimary        : Available
StatusOfSecondary      :
Tags                   :
EnableHttpsTrafficOnly : False
NetworkRuleSet         :
Context                : Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext
ExtendedProperties     : {}
```

Voer de opdracht uit om de sleutel van het opslag account op te halen `Get-AzureRmStorageAccountKey` . Hier wordt een voor beeld van een uitvoer van deze opdracht weer gegeven.

```powershell
PS C:\Users\Administrator> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: my-resource-ase
Name:myasestoracct

KeyName Value
------- -----
key1 /IjVJN+sSf7FMKiiPLlDm8mc9P4wtcmhhbnCa7...
key2 gd34TcaDzDgsY9JtDNMUgLDOItUU0Qur3CBo6Q...
```

## <a name="add-blob-uri-to-hosts-file"></a>Blob-URI toevoegen aan het hosts-bestand

U hebt de BLOB-URI in het hosts-bestand al toegevoegd voor de client die u gebruikt om verbinding te maken met de Blob-opslag in de sectie [hostbestand wijzigen voor naam omzetting van het eind punt](azure-stack-edge-j-series-connect-resource-manager.md#step-5-modify-host-file-for-endpoint-name-resolution). Dit was de vermelding voor de BLOB-URI:

\<Azure consistent network services VIP \>\<storage name\>. blob. \<appliance name\> .\<dnsdomain\>


## <a name="install-certificates"></a>Certificaten installeren

Als u *https* gebruikt, moet u de juiste certificaten op uw apparaat installeren. In dit geval installeert u het BLOB-eindpunt certificaat. Zie certificaten maken en uploaden in [certificaten beheren](azure-stack-edge-j-series-manage-certificates.md)voor meer informatie.

## <a name="upload-a-vhd"></a>Een VHD uploaden

Kopieer de schijf installatie kopieën die moeten worden gebruikt in pagina-blobs in het lokale opslag account dat u in de vorige stappen hebt gemaakt. U kunt een hulp programma zoals [AzCopy](../storage/common/storage-use-azcopy-v10.md) gebruiken om de VHD te uploaden naar het opslag account dat u in de vorige stappen hebt gemaakt. 

<!--Before you use AzCopy, make sure that the [AzCopy is configured correctly](#configure-azcopy) for use with the blob storage REST API version that you are using with your Azure Stack Edge Pro device.

```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> [!NOTE]
> Set `BlobType` to page for creating a managed disk out of VHD. Set `BlobType` to block when writing to tiered storage accounts using AzCopy.

You can download the disk images from the marketplace. For detailed steps, go to [Get the virtual disk image from Azure marketplace](azure-stack-edge-j-series-create-virtual-machine-image.md).

A sample output using AzCopy 7.3 is shown below. For more information on this command, go to [Upload VHD file to storage account using AzCopy](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md).


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```-->
Gebruik de volgende opdrachten met AzCopy 10:  

```powershell
$StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName <ResourceGroupName> -Name <StorageAccountName>)[0].Value

$endPoint = (Get-AzureRmStorageAccount -name <StorageAccountName> -ResourceGroupName <ResourceGroupName>).PrimaryEndpoints.Blob

$StorageAccountContext = New-AzureStorageContext -StorageAccountName <StorageAccountName> -StorageAccountKey <StorageAccountKey> -Endpoint <Endpoint>

$StorageAccountSAS = New-AzureStorageAccountSASToken -Service Blob,File,Queue,Table -ResourceType Container,Service,Object -Permission "acdlrw" -Context <StorageAccountContext> -Protocol HttpsOnly

<AzCopy exe path> cp "Full VHD path" "<BlobEndPoint>/<ContainerName><StorageAccountSAS>"
```

Hier volgt een voorbeeld van uitvoer: 

```powershell
$ContainerName = <ContainerName>
$ResourceGroupName = <ResourceGroupName>
$StorageAccountName = <StorageAccountName>
$VHDPath = "Full VHD Path"
$VHDFile = <VHDFileName>

$StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $ResourceGroupName -Name $StorageAccountName)[0].Value

$endPoint = (Get-AzureRmStorageAccount -name $StorageAccountName -resourcegroupname $ResourceGroupName).PrimaryEndpoints.Blob

$StorageAccountContext = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey -Endpoint $endPoint

$StorageAccountSAS = New-AzureStorageAccountSASToken -Service Blob,File,Queue,Table -ResourceType Container,Service,Object -Permission "acdlrw" -Context $StorageAccountContext -Protocol HttpsOnly

C:\AzCopy.exe  cp "$VHDPath\$VHDFile" "$endPoint$ContainerName$StorageAccountSAS"
```

## <a name="create-managed-disks-from-the-vhd"></a>Beheerde schijven maken op basis van de VHD

Een beheerde schijf maken op basis van de geüploade VHD.

```powershell
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri "Source URL for your VHD"
```
Hieronder ziet u een voorbeeld van de uitvoer: 
<code>
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import –SourceUri http://</code><code>sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages/ubuntu13.vhd</code> 

```powershell
New-AzureRMDisk -ResourceGroupName <Resource group name> -DiskName <Disk name> -Disk $DiskConfig
```

Hieronder ziet u een voorbeeld van de uitvoer. Ga naar [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk?view=azurermps-6.13.0)voor meer informatie over deze cmdlet.

```powershell
Tags               :
New-AzureRmDisk -ResourceGroupName rg191113014333 -DiskName ld191113014333 -Disk $DiskConfig

ResourceGroupName  : rg191113014333
ManagedBy          :
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              :
TimeCreated        : 11/13/2019 1:49:07 PM
OsType             :
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 30
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micros
                     oft.Compute/disks/ld191113014333
Name               : ld191113014333
Type               : Microsoft.Compute/disks
Location           : DBELocal
Tags               : {}
```

## <a name="create-a-vm-image-from-the-image-managed-disk"></a>Een VM-installatiekopie maken op basis van de installatiekopie van de beheerde schijf

Gebruik de volgende opdracht om een VM-installatie kopie te maken op basis van de beheerde schijf. Vervang de waarden binnen \< \> door de namen die u kiest.

```powershell
$imageConfig = New-AzureRmImageConfig -Location DBELocal
$ManagedDiskId = (Get-AzureRmDisk -Name <Disk name> -ResourceGroupName <Resource group name>).Id
Set-AzureRmImageOsDisk -Image $imageConfig -OsType '<OS type>' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId 

The supported OS types are Linux and Windows.

For OS Type=Linux, for example:
Set-AzureRmImageOsDisk -Image $imageConfig -OsType 'Linux' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId
New-AzureRmImage -Image $imageConfig -ImageName <Image name>  -ResourceGroupName <Resource group name>
```

Hieronder ziet u een voorbeeld van de uitvoer. Ga naar [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage?view=azurermps-6.13.0)voor meer informatie over deze cmdlet.

```powershell
New-AzureRmImage -Image Microsoft.Azure.Commands.Compute.Automation.Models.PSImage -ImageName ig191113014333  -ResourceGroupName rg191113014333
ResourceGroupName    : rg191113014333
SourceVirtualMachine :
StorageProfile       : Microsoft.Azure.Management.Compute.Models.ImageStorageProfile
ProvisioningState    : Succeeded
Id                   : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micr
                       osoft.Compute/images/ig191113014333
Name                 : ig191113014333
Type                 : Microsoft.Compute/images
Location             : dbelocal
Tags                 : {}
```

## <a name="create-vm-with-previously-created-resources"></a>Een VM maken met eerder gemaakte resources

U moet één virtueel netwerk maken en een virtuele netwerk interface koppelen voordat u de virtuele machine maakt en implementeert.

> [!IMPORTANT]
> Bij het maken van het virtuele netwerk en de virtuele netwerk interface gelden de volgende regels:
> - Er kan slechts één Vnet worden gemaakt (zelfs over resource groepen) en het moet exact overeenkomen met het logische netwerk in termen van de adres ruimte.
> -   Er is slechts één subnet toegestaan in het Vnet. Het subnet moet exact dezelfde adres ruimte zijn als het Vnet.
> -   Alleen een statische toewijzings methode is toegestaan tijdens het maken van Vnic en de gebruiker moet een privé-IP-adres opgeven.

 
**Een query uitvoeren op de automatisch gemaakte Vnet**

Wanneer u berekenen van de lokale gebruikers interface van uw apparaat inschakelt, wordt er automatisch een Vnet `ASEVNET` gemaakt onder de `ASERG` resource groep. Gebruik de volgende opdracht om een query uit te zoeken op het bestaande Vnet:

```powershell
$aRmVN = Get-AzureRMVirtualNetwork -Name ASEVNET -ResourceGroupName ASERG 
```

<!--```powershell
$subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name <Subnet name> -AddressPrefix <Address Prefix>
$aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName <Resource group name> -Name <Vnet name> -Location DBELocal -AddressPrefix <Address prefix> -Subnet $subNetId
```-->

**Een Vnic maken met de Vnet-subnet-ID**

```powershell
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <IP config Name> -SubnetId $aRmVN.Subnets[0].Id -PrivateIpAddress <Private IP>
$Nic = New-AzureRmNetworkInterface -Name <Nic name> -ResourceGroupName <Resource group name> -Location DBELocal -IpConfiguration $ipConfig
```

De voorbeeld uitvoer van deze opdrachten wordt hieronder weer gegeven:

```powershell
PS C:\Users\Administrator> $subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name my-ase-subnet -AddressPrefix "5.5.0.0/16"

PS C:\Users\Administrator> $aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName Resource-my-ase -Name my-ase-virtualnetwork -Location DBELocal -AddressPrefix "5.5.0.0/16" -Subnet $subNetId
WARNING: The output object type of this cmdlet will be modified in a future release.
PS C:\Users\Administrator> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name my-ase-ip -SubnetId $aRmVN.Subnets[0].Id
PS C:\Users\Administrator> $Nic = New-AzureRmNetworkInterface -Name my-ase-nic -ResourceGroupName Resource-my-ase -Location DBELocal -IpConfiguration $ipConfig
WARNING: The output object type of this cmdlet will be modified in a future release.

PS C:\Users\Administrator> $Nic

PS C:\Users\Administrator> (Get-AzureRmNetworkInterface)[0]

Name                        : nic200108020444
ResourceGroupName           : rg200108020444
Location                    : dbelocal
Id                          : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Network/networ
                              kInterfaces/nic200108020444
Etag                        : W/"f9d1759d-4d49-42fa-8826-e218e0b1d355"
ResourceGuid                : 3851ae62-c13e-4416-9386-e21d9a2fef0f
ProvisioningState           : Succeeded
Tags                        :
VirtualMachine              : {
                                "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Compu
                              te/virtualMachines/VM200108020444"
                              }
IpConfigurations            : [
                                {
                                  "Name": "ip200108020444",
                                  "Etag": "W/\"f9d1759d-4d49-42fa-8826-e218e0b1d355\"",
                                  "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Net
                              work/networkInterfaces/nic200108020444/ipConfigurations/ip200108020444",
                                  "PrivateIpAddress": "5.5.166.65",
                                  "PrivateIpAllocationMethod": "Static",
                                  "Subnet": {
                                    "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/DbeSystemRG/providers/Microsoft.Netw
                              ork/virtualNetworks/vSwitch1/subnets/subnet123",
                                    "ResourceNavigationLinks": [],
                                    "ServiceEndpoints": []
                                  },
                                  "ProvisioningState": "Succeeded",
                                  "PrivateIpAddressVersion": "IPv4",
                                  "LoadBalancerBackendAddressPools": [],
                                  "LoadBalancerInboundNatRules": [],
                                  "Primary": true,
                                  "ApplicationGatewayBackendAddressPools": [],
                                  "ApplicationSecurityGroups": []
                                }
                              ]
DnsSettings                 : {
                                "DnsServers": [],
                                "AppliedDnsServers": []
                              }
EnableIPForwarding          : False
EnableAcceleratedNetworking : False
NetworkSecurityGroup        : null
Primary                     : True
MacAddress                  : 00155D18E432                :
```

U kunt eventueel tijdens het maken van een Vnic voor een virtuele machine het open bare IP-adres door geven. In dit geval wordt het privé-IP-adres door het open bare IP-adres geretourneerd. 

```powershell
New-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <ResourceGroupName> -AllocationMethod Static -Location DBELocal
$publicIP = (Get-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <Resource group name>).Id
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <ConfigName> -PublicIpAddressId $publicIP -SubnetId $subNetId
```


**Een VM maken**:

U kunt nu de VM-installatie kopie gebruiken om een virtuele machine te maken en deze te koppelen aan het virtuele netwerk dat u eerder hebt gemaakt.

```powershell
$pass = ConvertTo-SecureString "<Password>" -AsPlainText -Force;
$cred = New-Object System.Management.Automation.PSCredential("<Enter username>", $pass)

You will use this username, password to login to the VM, once it is created and powered up.

$VirtualMachine = New-AzureRmVMConfig -VMName <VM name> -VMSize "Standard_D1_v2"

$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -<OS type> -ComputerName <Your computer Name> -Credential $cred

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name <OS Disk Name> -Caching "ReadWrite" -CreateOption "FromImage" -Linux -StorageAccountType StandardLRS

$nicID = (Get-AzureRmNetworkInterface -Name <nic name> -ResourceGroupName <Resource Group Name>).Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nicID

$image = (Get-AzureRmImage -ResourceGroupName <Resource Group Name> -ImageName $ImageName).Id

$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -Id $image

New-AzureRmVM -ResourceGroupName <Resource Group Name> -Location DBELocal -VM $VirtualMachine -Verbose
```

## <a name="connect-to-a-vm"></a>Verbinding maken met een virtuele machine

Afhankelijk van of u een Windows-of een Linux-VM hebt gemaakt, kunnen de stappen om verbinding te maken verschillend zijn.

### <a name="connect-to-linux-vm"></a>Verbinding maken met een virtuele Linux-machine

Volg deze stappen om verbinding te maken met een virtuele Linux-machine.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-windows-vm"></a>Verbinding maken met Windows VM

Volg deze stappen om verbinding te maken met een virtuele Windows-machine.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]


<!--Connect to the VM using the private IP that you passed during the VM creation.

Open an SSH session to connect with the IP address.

`ssh -l <username> <ip address>`

When prompted, provide the password that you used when creating the VM.

If you need to provide the SSH key, use this command.

ssh -i c:/users/Administrator/.ssh/id_rsa Administrator@5.5.41.236

If you used a public IP address during VM creation, you can use that IP to connect to the VM. To get the public IP: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
The public IP in this case will be the same as the private IP that you passed during virtual network interface creation.-->


## <a name="manage-vm"></a>Virtuele machine beheren

In de volgende sectie worden enkele veelvoorkomende bewerkingen voor de virtuele machine beschreven die u op uw Azure Stack Edge Pro-apparaat gaat maken.

### <a name="list-vms-running-on-the-device"></a>Virtuele machines weer geven die worden uitgevoerd op het apparaat

Voer de volgende opdracht uit om een lijst te retour neren van alle virtuele machines die worden uitgevoerd op uw Azure Stack Edge Pro-apparaat.


`Get-AzureRmVM -ResourceGroupName <String> -Name <String>`
 

### <a name="turn-on-the-vm"></a>De virtuele machine inschakelen

Voer de volgende cmdlet uit om een virtuele machine die wordt uitgevoerd op uw apparaat in te scha kelen:


`Start-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>`


Ga naar [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0)voor meer informatie over deze cmdlet.

### <a name="suspend-or-shut-down-the-vm"></a>De virtuele machine onderbreken of afsluiten

Voer de volgende cmdlet uit om een virtuele machine die wordt uitgevoerd op uw apparaat te stoppen of af te sluiten:


```powershell
Stop-AzureRmVM [-Name] <String> [-StayProvisioned] [-ResourceGroupName] <String>
```


Ga naar de [cmdlet stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm?view=azurermps-6.13.0)voor meer informatie over deze cmdlet.

### <a name="add-a-data-disk"></a>Een gegevens schijf toevoegen

Als de vereisten voor de werk belasting van uw VM toenemen, moet u mogelijk een gegevens schijf toevoegen.

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" -VhdUri "https://contoso.blob.core.windows.net/vhds/diskstandard03.vhd" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty 
 
Update-AzureRmVM -ResourceGroupName "<Resource Group Name string>" -VM $VirtualMachine
```

### <a name="delete-the-vm"></a>De VM verwijderen

Voer de volgende cmdlet uit om een virtuele machine van uw apparaat te verwijderen:

```powershell
Remove-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>
```

Ga naar de [cmdlet Remove-AzureRmVm](/powershell/module/azurerm.compute/remove-azurermvm?view=azurermps-6.13.0)voor meer informatie over deze cmdlet.



## <a name="next-steps"></a>Volgende stappen

[Azure Resource Manager-cmdlets](/powershell/module/azurerm.resources/?view=azurermps-6.13.0)