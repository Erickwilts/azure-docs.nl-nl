---
title: 'Service-naar-serviceverificatie: .NET-SDK met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met behulp van .NET SDK
services: data-lake-store
documentationcenter: ''
author: twooley
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 96c496ef67e26a3079577bf52e9d019d963467b8
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65915858"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>Service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met .NET SDK
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET-SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
>
>

In dit artikel leert u over het gebruik van de .NET SDK-service-naar-serviceverificatie met Azure Data Lake Storage Gen1 doen. Zie voor verificatie van eindgebruikers met Data Lake Storage Gen1 met .NET SDK, [eindgebruikersverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md).

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2013 of hoger**. De onderstaande instructies wordt Visual Studio 2019 gebruikt.

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Web'**. U moet zijn voltooid de stappen in [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Een .NET-toepassing maken
1. Selecteer in Visual Studio, de **bestand** in het menu **nieuw**, en vervolgens **Project**.
2. Kies **Console-App (.NET Framework)**, en selecteer vervolgens **volgende**.
3. In **projectnaam**, voer `CreateADLApplication`, en selecteer vervolgens **maken**.

4. Voeg de NuGet-pakketten toe aan het project.

   1. Klik in Solution Explorer met de rechtermuisknop op de projectnaam en klik op **Manage NuGet Packages**.
   2. Controleer op het tabblad **NuGet Package Manager** of **Package source** is ingesteld op **nuget.org** en of het selectievakje **Include prerelease** is ingeschakeld.
   3. Zoek en installeer de volgende NuGet-pakketten:

      * `Microsoft.Azure.Management.DataLake.Store`: in deze zelfstudie wordt gebruikgemaakt van v2.1.3-preview.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: in deze zelfstudie wordt gebruikgemaakt van v2.2.12.

        ![Een NuGet-bron toevoegen](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Een nieuw Azure Data Lake-account maken")
   4. Sluit de **NuGet Package Manager**.

5. Open **Program.cs**, verwijder de bestaande code en neem de volgende instructies op om verwijzingen naar naamruimten toe te voegen.

```csharp
using System;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading;
using System.Collections.Generic;
using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates

using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.DataLake.Store;
using Microsoft.Azure.Management.DataLake.Store.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

## <a name="service-to-service-authentication-with-client-secret"></a>Service-naar-serviceverificatie met clientgeheim
Dit codefragment in uw .NET-client-toepassing toevoegen. Vervang de tijdelijke aanduiding door de waarden die zijn opgehaald uit een Azure AD-webtoepassing (weergegeven als een vereiste). Dit codefragment kunt u verifiëren van uw toepassing **niet-interactief** met Data Lake Storage Gen1 met behulp van de client-geheim/sleutel voor Azure AD-webtoepassing.

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with client secret / key
    // Use the client ID of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
    var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
    var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
}
```

Het bovenstaande codefragment maakt gebruik van een Help-functie `GetCreds_SPI_SecretKey`. De code voor deze Help-functie is beschikbaar [hier op GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Service-naar-serviceverificatie met certificaat

Dit codefragment in uw .NET-client-toepassing toevoegen. Vervang de tijdelijke aanduiding door de waarden die zijn opgehaald uit een Azure AD-webtoepassing (weergegeven als een vereiste). Dit codefragment kunt u verifiëren van uw toepassing **niet-interactief** met Data Lake Storage Gen1 met behulp van het certificaat voor een Azure AD-webtoepassing. Zie voor instructies over het maken van een Azure AD-toepassing [service-principal maken met certificaten](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

```csharp
private static void Main(string[] args)
{
    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    string TENANT = "<AAD-directory-domain>";
    string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
    System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
    System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
    var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
    var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
    var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
}
```

Het bovenstaande codefragment maakt gebruik van een Help-functie `GetCreds_SPI_Cert`. De code voor deze Help-functie is beschikbaar [hier op GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u service-naar-serviceverificatie gebruiken om te verifiëren met Data Lake Storage Gen1 met .NET SDK. U kunt nu de volgende artikelen die bespreken hoe u de .NET SDK gebruiken om te werken met Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met .NET SDK](data-lake-store-data-operations-net-sdk.md)
