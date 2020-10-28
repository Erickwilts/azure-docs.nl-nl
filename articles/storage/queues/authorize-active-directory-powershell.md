---
title: Power shell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot wachtrij gegevens
titleSuffix: Azure Storage
description: Power shell ondersteunt aanmelden met Azure AD-referenties om opdrachten uit te voeren op Azure Storage wachtrij gegevens. Er wordt een toegangs token voor de sessie gegeven en gebruikt om aanroepende bewerkingen te autoriseren. De machtigingen zijn afhankelijk van de Azure-rol die is toegewezen aan de Azure AD-beveiligings-principal.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/14/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: queues
ms.openlocfilehash: 6937f3aa136ee4d5b906492c3f8cffc86acbc2e5
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/28/2020
ms.locfileid: "92785660"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-queue-data"></a>Power shell-opdrachten uitvoeren met Azure AD-referenties voor toegang tot wachtrij gegevens

Azure Storage biedt uitbrei dingen voor Power shell waarmee u zich kunt aanmelden en script opdrachten kunt uitvoeren met de referenties voor Azure Active Directory (Azure AD). Wanneer u zich aanmeldt bij Power shell met Azure AD-referenties, wordt een OAuth 2,0-toegangs token geretourneerd. Dit token wordt automatisch door Power shell gebruikt voor het autoriseren van volgende gegevens bewerkingen voor de wachtrij opslag. Voor ondersteunde bewerkingen hoeft u geen account sleutel of SAS-token meer door te geven met de opdracht.

U kunt machtigingen voor de wachtrij gegevens toewijzen aan een Azure AD-beveiligings-principal via Azure op rollen gebaseerd toegangs beheer (Azure RBAC). Zie [Manage access rights to Azure Storage Data with Azure RBAC](../common/storage-auth-aad-rbac-portal.md)(Engelstalig) voor meer informatie over Azure-rollen in azure Storage.

## <a name="supported-operations"></a>Ondersteunde bewerkingen

De Azure Storage-extensies worden ondersteund voor bewerkingen op wachtrij gegevens. Welke bewerkingen u kunt aanroepen, is afhankelijk van de machtigingen die zijn verleend aan de Azure AD-beveiligings-principal waarmee u zich aanmeldt bij Power shell. Machtigingen voor Azure Storage wachtrijen worden toegewezen via Azure RBAC. Als u bijvoorbeeld de rol **gegevens lezer** van de wachtrij hebt toegewezen, kunt u script opdrachten uitvoeren die gegevens uit een wachtrij lezen. Als u de rol Inzender voor **wachtrij gegevens** hebt toegewezen, kunt u script opdrachten uitvoeren om een wachtrij of de gegevens die ze bevatten, te lezen, schrijven of verwijderen.

Zie [Storage-bewerkingen aanroepen met OAuth-tokens](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)voor meer informatie over de vereiste machtigingen voor elke Azure Storage bewerking in een wachtrij.  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Power shell-opdrachten aanroepen met Azure AD-referenties

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Als u Azure PowerShell wilt gebruiken om u aan te melden en volgende bewerkingen uit te voeren op Azure Storage met behulp van Azure AD-referenties, maakt u een opslag context om te verwijzen naar het opslag account en voegt u de `-UseConnectedAccount` para meter toe.

In het volgende voor beeld ziet u hoe u een wachtrij maakt in een nieuw opslag account op basis van Azure PowerShell met uw Azure AD-referenties. Vergeet niet om de waarden van de tijdelijke aanduidingen tussen de punthaken te vervangen door uw eigen waarden:

1. Meld u aan bij uw Azure-account met de opdracht [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) :

    ```powershell
    Connect-AzAccount
    ```

    Zie [Aanmelden met Azure PowerShell](/powershell/azure/authenticate-azureps)voor meer informatie over het aanmelden bij Azure met Power shell.

1. Maak een Azure-resource groep door [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)aan te roepen. 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Maak een opslag account door [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)aan te roepen.

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Haal de context van het opslag account op waarmee het nieuwe opslag account wordt opgegeven door [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext)aan te roepen. Wanneer u op een opslag account werkt, kunt u naar de context verwijzen in plaats van herhaaldelijk de referenties door te geven. Neem de `-UseConnectedAccount` para meter op voor het aanroepen van volgende gegevens bewerkingen met uw Azure AD-referenties:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Voordat u de wachtrij maakt, wijst u de rol [gegevens Inzender voor opslag wachtrij](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor) toe aan uzelf. Hoewel u de eigenaar van het account bent, hebt u expliciete machtigingen nodig om gegevens bewerkingen uit te voeren op het opslag account. Zie voor meer informatie over het toewijzen van Azure-functies [de Azure Portal gebruiken om een Azure-rol toe te wijzen voor toegang tot Blob-en wachtrij gegevens](../common/storage-auth-aad-rbac-portal.md).

    > [!IMPORTANT]
    > Het kan enkele minuten duren voordat Azure-roltoewijzingen worden doorgegeven.

1. Maak een wachtrij door [New-AzStorageQueue](/powershell/module/az.storage/new-azstoragequeue)aan te roepen. Omdat deze aanroep de context gebruikt die in de vorige stappen is gemaakt, wordt de wachtrij gemaakt met uw Azure AD-referenties.

    ```powershell
    $queueName = "sample-queue"
    New-AzStorageQueue -Name $queueName -Context $ctx
    ```

## <a name="next-steps"></a>Volgende stappen

- [Power shell gebruiken om een Azure-rol toe te wijzen voor toegang tot Blob-en wachtrij gegevens](../common/storage-auth-aad-rbac-powershell.md)
- [Toegang tot Blob-en wachtrij gegevens toestaan met beheerde identiteiten voor Azure-resources](../common/storage-auth-aad-msi.md)