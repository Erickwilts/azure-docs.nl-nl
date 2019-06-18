---
title: Beveiligde TLS voor Azure Storage client in te schakelen | Microsoft Docs
description: Informatie over het inschakelen van TLS 1.2 op de client van Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/25/2018
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 218708ffc9a680150d7b6bf559a00ca87054bbe8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65152973"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Beveiligde TLS voor Azure Storage-client inschakelen

Transport Layer Security (TLS) en Secure Sockets Layer (SSL) zijn cryptografische protocollen die beveiliging van communicatie via een netwerk. 1\.0 SSL 2.0 en 3.0 zijn kwetsbaar gevonden. Ze hebben door RFC is verboden. TLS 1.0 is onveilig voor het gebruik van onbeveiligde blok-codering (DES CBC en RC2 CBC) en Stream-codering (RC4). PCI-Raad wordt ook aangeraden de migratie naar een hoger TLS-versie. Voor meer informatie kunt u zien [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0).

Azure Storage is gestopt met SSL 3.0 sinds 2015 en maakt gebruik van TLS 1.2 op openbare HTTPs-eindpunten, maar de TLS 1.0 en 1.1 TLS zijn nog steeds worden ondersteund voor achterwaartse compatibiliteit.

Om ervoor te zorgen beveiligd en compatibel verbinding met Azure Storage, moet u TLS 1.2 of een nieuwere versie in client-side inschakelen voordat u verzendt aanvragen naar Azure Storage-service werkt.

## <a name="enable-tls-12-in-net-client"></a>TLS 1.2 in .NET-client inschakelen

Voor de client om te onderhandelen over TLS 1.2, het besturingssysteem en de .NET Framework-versie moeten beide ondersteunen TLS 1.2. Zie voor meer informatie [ondersteuning voor TLS 1.2](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12).

Het volgende voorbeeld laat zien hoe voor het inschakelen van TLS 1.2 in uw .NET-client.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>TLS 1.2 in PowerShell-client inschakelen

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)] 

Het volgende voorbeeld laat zien hoe voor het inschakelen van TLS 1.2 in uw PowerShell-client.

```powershell
# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGroupName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers
```

## <a name="verify-tls-12-connection"></a>TLS 1.2-verbinding controleren

U kunt Fiddler gebruiken om te controleren als TLS 1.2 daadwerkelijk wordt gebruikt. Open Fiddler om te beginnen met het vastleggen van netwerkverkeer van de client vervolgens bovenstaande voorbeeld worden uitgevoerd. U kunt vervolgens de TLS-versie vinden in de verbinding die het voorbeeld maakt.

De volgende schermafbeelding is een voorbeeld van de verificatie.

![Schermafbeelding van de verificatie van TLS-versie in Fiddler](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Zie ook

* [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [PCI-naleving van TLS](https://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls)
* [Schakel TLS in Java-client](https://www.java.com/en/configure_crypto.html)
