---
title: Rollover van een Azure Service Fabric-cluster-certificaat | Microsoft Docs
description: Meer informatie over hoe op rollover van een Service Fabric-clustercertificaat geïdentificeerd door de algemene naam van het certificaat.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: aljo
ms.openlocfilehash: dd4b6026772a20c522532e1ba65c6846addfa161
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159902"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Handmatig een Service Fabric-cluster-certificaat vernieuwen
Als een Service Fabric-clustercertificaat bijna verloopt, moet u het certificaat bijwerken.  Rollover van certificaten is eenvoudig als het cluster is [ingesteld voor gebruik certificaten op basis van de algemene naam](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (in plaats van vingerafdruk).  Haal een nieuw certificaat van een certificeringsinstantie met een nieuwe vervaldatum.  Zelfondertekende certificaten worden niet ondersteund voor productie Service Fabric-clusters, om op te nemen van certificaten die zijn gegenereerd tijdens de Azure portal Cluster maken-werkstroom. Het nieuwe certificaat moet dezelfde algemene naam als het certificaat voor oudere hebben. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Service Fabric-cluster wordt automatisch gebruikt voor het opgegeven certificaat met een verdere in de toekomstige vervaldatum; bij het valideren van meer dan één certificaat geïnstalleerd op de host. Een best practice is het gebruik van Resource Manager-sjabloon voor het inrichten van Azure-Resources. Voor niet-productieomgeving is het volgende script kan worden gebruikt om een nieuw certificaat uploaden naar een sleutelkluis, en installeert u het certificaat op de virtuele-machineschaalset: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Virtual Machine Scale ingesteld geheimen bieden geen ondersteuning voor dezelfde resource-id voor twee afzonderlijke geheimen, berekent zoals elke geheim een samengestelde unieke resource is. 

Voor meer informatie leest u het volgende:
* Meer informatie over [cluster security](service-fabric-cluster-security.md).
* [Bijwerken en clustercertificaten beheren](service-fabric-cluster-security-update-certs-azure.md)

