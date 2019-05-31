---
title: Met behulp implementeren disaster recovery back-up en herstellen in Azure API Management | Microsoft Docs
description: Informatie over het gebruik van back-up en herstellen als u wilt uitvoeren van herstel na noodgevallen in Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: e0c02829a2fef6e281794fdba6c9fb5d9b8a736b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241705"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Herstel na noodgeval met behulp van back-up van de service implementeren en te herstellen in Azure API Management

Door te publiceren en beheren van uw API's via Azure API Management, bent u profiteert van fouttolerantie en mogelijkheden in de infrastructuur die u anders ontwerpen, implementeren en beheren van handmatig had. Het Azure-platform vermindert een groot deel van mogelijke problemen met de tegen een fractie van de kosten.

Als u wilt herstellen van problemen met beschikbaarheid die invloed hebben op de regio die als host fungeert voor uw API Management-service, zijn gereed voor uw service in een andere regio op elk gewenst moment reconstrueren. Afhankelijk van de beschikbaarheid en de hersteldoelen voor de tijd, is het raadzaam om te reserveren van een Backup-service in een of meer regio's. U kunt ook proberen te houden met de configuratie en de inhoud synchroon met de actieve service. De service 'back-up en herstellen'-functie biedt de benodigde bouwstenen voor het implementeren van uw strategie voor noodherstel.

Deze handleiding wordt beschreven hoe u om Azure Resource Manager-aanvragen te verifiëren. U ziet ook hoe u back-up en herstellen van uw API Management service-exemplaren.

> [!NOTE]
> Het proces voor het back-up en herstellen van een exemplaar van API Management-service voor herstel na noodgevallen kan ook worden gebruikt voor het repliceren van API Management service-exemplaren voor scenario's zoals fasering.
>
> Elke back-up verloopt na 30 dagen. Als u probeert te herstellen van een back-up nadat de verloopperiode van 30 dagen is verlopen, wordt het herstellen mislukken met een `Cannot restore: backup expired` bericht.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Verificatie van Azure Resource Manager-aanvragen

> [!IMPORTANT]
> De REST-API voor back-up en herstel maakt gebruik van Azure Resource Manager en heeft een ander verificatiemechanisme dan de REST API's voor het beheren van uw API Management-entiteiten. De stappen in deze sectie wordt beschreven hoe u Azure Resource Manager-aanvragen verifiëren. Zie voor meer informatie, [verificatie van Azure Resource Manager-aanvragen](/rest/api/index).

Alle taken die u doen op resources met behulp van Azure Resource Manager moeten worden geverifieerd bij Azure Active Directory met de volgende stappen uit:

* Een toepassing toevoegen aan de Azure Active Directory-tenant.
* Machtigingen instellen voor de toepassing die u hebt toegevoegd.
* Ophalen van het token voor het verifiëren van aanvragen naar Azure Resource Manager.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Met behulp van het abonnement met uw API Management-service-exemplaar, gaat u naar de **App-registraties** tabblad **Azure Active Directory** (Azure Active Directory > beheren/App-registraties).

    > [!NOTE]
    > Als de standaarddirectory van Azure Active Directory niet zichtbaar is voor uw account, contact op met de beheerder van het Azure-abonnement om de vereiste machtigingen toewijzen aan uw account.
3. Klik op **Nieuwe toepassing registreren**.

    De **maken** venster wordt weergegeven aan de rechterkant. Dat is waar u de relevante informatie voor AAD-app.
4. Voer een naam in voor de toepassing.
5. Selecteer voor het toepassingstype **systeemeigen**.
6. Voer de URL van een tijdelijke aanduiding zoals `http://resources` voor de **omleidings-URI**, zoals het is een verplicht veld, maar de waarde later wordt niet gebruikt. Klik op het selectievakje in om op te slaan van de toepassing.
7. Klik op **Create**.

### <a name="add-an-application"></a>Een toepassing toevoegen

1. Nadat de toepassing is gemaakt, klikt u op **instellingen**.
2. Klik op **vereiste machtigingen**.
3. Klik op **+ toevoegen**.
4. Druk op **Select an API**.
5. Kies **Windows** **Azure Service Management API**.
6. Druk op **Selecteer**.

    ![Machtigingen toevoegen](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Klik op **gedelegeerde machtigingen** naast de zojuist toegevoegde toepassing, schakel het selectievakje voor **toegang tot Azure Service Management (preview)** .
8. Druk op **Selecteer**.
9. Klik op **machtigingen verlenen**.

### <a name="configuring-your-app"></a>Configureren van uw app

Voordat u de API's die de back-up genereren en deze herstellen, moet u een token verkrijgen. Het volgende voorbeeld wordt de [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) NuGet-pakket aan het token opgehaald.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Vervang `{tenant id}`, `{application id}`, en `{redirect uri}` met behulp van de volgende instructies:

1. Vervang `{tenant id}` met de tenant-ID van de Azure Active Directory-toepassing die u hebt gemaakt. U kunt de ID openen door te klikken **App-registraties** -> **eindpunten**.

    ![Eindpunten][api-management-endpoint]
2. Vervang `{application id}` met de waarde die u door te navigeren naar de **instellingen** pagina.
3. Vervang de `{redirect uri}` met de waarde van de **omleidings-URI's** tab van uw Azure Active Directory-toepassing.

    Zodra de waarden zijn opgegeven, moet het codevoorbeeld retourneren een token die vergelijkbaar is met het volgende voorbeeld:

    ![Token][api-management-arm-token]

    > [!NOTE]
    > Het token kan verlopen na een bepaalde periode. Het codevoorbeeld om een nieuw token opnieuw te worden uitgevoerd.

## <a name="calling-the-backup-and-restore-operations"></a>Aanroepen van de bewerkingen voor back-up en herstel

De REST-API's zijn [Api Management-Service - back-up](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) en [Api Management-Service - terugzetten](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore).

Voordat het aanroepen van de 'back-up en herstellen' bewerkingen die worden beschreven in de volgende secties stelt u de autorisatie-header voor aanvraag voor de REST-aanroep.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"> </a>Back-up van een API Management-service

Back-up een probleem van API Management-service de volgende HTTP-aanvraag:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

Waar:

* `subscriptionId` -ID van het abonnement waarin de API Management-service waarmee u een back-up wilt maken
* `resourceGroupName` -naam van de resourcegroep van uw Azure API Management-service
* `serviceName` -de naam van de API Management-service dat een back-up van de opgegeven op het moment van aanmaak
* `api-version` -vervangen `2018-06-01-preview`

Geef in de hoofdtekst van de aanvraag, de doelnaam van het Azure storage-account, toegangssleutel, de naam van de blob-container en de naam van de back-up:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Stel de waarde van de `Content-Type` aanvraagheader naar `application/json`.

Back-up is een langdurige bewerking die meer dan een minuut in beslag kan nemen.  Als de aanvraag is voltooid en het back-upproces is begonnen, ontvangt u een `202 Accepted` antwoordstatuscode met een `Location` header.  Controleer 'GET-aanvragen naar de URL in de `Location` header om de status van de bewerking erachter te komen. Terwijl de back-up uitgevoerd wordt, moet u blijven ontvangen van een statuscode '202 geaccepteerd'. Een Antwoordcode van `200 OK` geeft aan dat de back-upbewerking is voltooid.

Houd rekening met de volgende beperkingen bij het maken van een back-upaanvraag:

* **Container** opgegeven in de aanvraagtekst **moet bestaan**.
* Wanneer back-up uitgevoerd wordt, **te voorkomen dat wijzigingen in service management** zoals SKU upgrade of downgrade, wijzigt u in de domeinnaam van het, en meer.
* Herstellen van een **back-up kan worden gegarandeerd alleen voor 30 dagen** sinds het moment van aanmaak.
* **Gebruiksgegevens** gebruikt voor het maken van analyserapporten **niet is opgenomen** in de back-up. Gebruik [Azure API Management REST API] [ Azure API Management REST API] periodiek ophalen analyserapporten veilig wordt gesteld.
* De frequentie waarmee u service back-ups uitvoeren van invloed zijn op uw beoogd herstelpunt. Als u wilt minimaliseren, raden wij uitvoering van regelmatige back-ups en back-ups op aanvraag uitvoeren nadat u wijzigingen in uw API Management-service aanbrengt.
* **Wijzigingen** aangebracht in de serviceconfiguratie (bijvoorbeeld API's, beleidsregels en ontwikkelaarsportal) tijdens het back-up wordt proces **kunnen worden uitgesloten van de back-up en verloren**.

### <a name="step2"> </a>Herstellen van een API Management-service

Als u een API Management-service van een eerder gemaakte back-up herstellen, moet u de volgende HTTP-aanvraag:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

Waar:

* `subscriptionId` -ID van het abonnement waarin u bij het herstellen van een back-up in de API Management-service
* `resourceGroupName` -naam van de resourcegroep waarin u bij het herstellen van een back-up in Azure API Management-service
* `serviceName` -de naam van de API Management-service wordt hersteld in die is opgegeven bij de aanmaaktijd
* `api-version` -vervangen `2018-06-01-preview`

Geef de locatie van de back-upbestand in de hoofdtekst van de aanvraag. Dat wil zeggen, voeg de Azure storage-accountnaam, toegangssleutel, de naam van de blob-container en de naam van de back-up:

```json
{
  "storageAccount": "{storage account name for the backup}",
  "accessKey": "{access key for the account}",
  "containerName": "{backup container name}",
  "backupName": "{backup blob name}"
}
```

Stel de waarde van de `Content-Type` aanvraagheader naar `application/json`.

Terugzetten is een langdurige bewerking die op meer dan 30 minuten duren kan om uit te voeren. Als de aanvraag is voltooid en het herstelproces is begonnen, ontvangt u een `202 Accepted` antwoordstatuscode met een `Location` header. Controleer 'GET-aanvragen naar de URL in de `Location` header om de status van de bewerking erachter te komen. Terwijl het herstel uitgevoerd wordt, blijft u '202 geaccepteerd' statuscode. Een Antwoordcode van `200 OK` geeft aan dat de herstelbewerking is voltooid.

> [!IMPORTANT]
> **De SKU** van de service wordt hersteld in **moet overeenkomen met** de SKU van de back-up-service wordt hersteld.
>
> **Wijzigingen** aangebracht aan de serviceconfiguratie (bijvoorbeeld API's, beleid, ontwikkelaarsportal) tijdens het herstel wordt uitgevoerd **kan worden overschreven**.

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Back-up en herstelbewerkingen kunnen ook worden uitgevoerd met PowerShell *back-up-AzApiManagement* en *terugzetten AzApiManagement* respectievelijk-opdrachten.

## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende bronnen voor verschillende scenario's van het proces van het back-up/herstellen.

* [Azure API Management-Accounts repliceren](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
* [Back-up maken en terugzetten in API Management automatiseren met Logic Apps](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps)
* [Azure API Management: Back-Up en herstellen van configuratie](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  *de aanpak van gedetailleerde door Stuart komt niet overeen met de officiële richtlijnen, maar het is interessant.*

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2

[Azure API Management REST API]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
