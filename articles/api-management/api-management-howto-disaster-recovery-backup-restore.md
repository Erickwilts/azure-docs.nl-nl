---
title: Herstel na nood geval met behulp van back-up en herstel implementeren in azure API Management | Microsoft Docs
description: Meer informatie over het gebruik van back-up en herstel om herstel na nood gevallen in azure API Management uit te voeren.
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
ms.date: 06/26/2019
ms.author: apimpm
ms.openlocfilehash: 619a4de993f052f143e4117f0100ed1e0aa77b03
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68498593"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Herstel na nood geval implementeren met behulp van back-up en herstel van de service in azure API Management

Door uw Api's via Azure API Management te publiceren en te beheren, profiteert u van de fout tolerantie en infrastructuur mogelijkheden die u op een andere manier hand matig ontwerpt, implementeert en beheert. Het Azure-platform vermindert een grote fractie van mogelijke fouten tegen een fractie van de kosten.

Als u de beschikbaarheids problemen wilt herstellen die van invloed zijn op de regio die als host fungeert voor uw API Management-service, kunt u op elk gewenst moment uw service in een andere regio maken. Afhankelijk van de beoogde herstel tijd wilt u mogelijk een stand-by-service in een of meer regio's blijven gebruiken. U kunt ook proberen de configuratie en inhoud te behouden in synchronisatie met de actieve service volgens uw Recovery Point Objective. De functies voor back-up en herstel van de service bieden de benodigde bouw stenen voor het implementeren van een strategie voor herstel na nood gevallen.

Back-up-en herstel bewerkingen kunnen ook worden gebruikt voor het repliceren van API Management service configuratie tussen de operationele omgevingen, zoals ontwikkeling en fase ring. Houd er rekening mee dat runtime gegevens, zoals gebruikers en abonnementen, ook worden gekopieerd, wat mogelijk niet altijd wenselijk is.

In deze hand leiding wordt beschreven hoe u back-up-en herstel bewerkingen automatiseert en hoe u een geslaagde verificatie van aanvragen voor back-up en herstel kunt garanderen door Azure Resource Manager.

> [!IMPORTANT]
> Met de herstel bewerking wordt de aangepaste hostname-configuratie van de doel service niet gewijzigd. We raden u aan om hetzelfde aangepaste hostname-en TLS-certificaat te gebruiken voor de services actief en stand-by, zodat het verkeer na het volt ooien van de herstel bewerking opnieuw kan worden omgeleid naar het stand-by-exemplaar door een eenvoudige DNS CNAME-wijziging.
>
> Met de back-upbewerking worden geen vooraf geaggregeerde logboek gegevens vastgelegd die worden gebruikt in rapporten die worden weer gegeven op de Blade Analytics in de Azure Portal.

> [!WARNING]
> Elke back-up verloopt na 30 dagen. Als u een back-up probeert te herstellen nadat de verval periode van 30 dagen is verstreken, mislukt de herstel bewerking `Cannot restore: backup expired` met een bericht.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Azure Resource Manager aanvragen verifiëren

> [!IMPORTANT]
> De REST API voor back-up en herstel maakt gebruik van Azure Resource Manager en heeft een ander verificatie mechanisme dan de REST Api's voor het beheren van uw API Management entiteiten. In de stappen in deze sectie wordt beschreven hoe u Azure Resource Manager-aanvragen verifieert. Zie voor meer informatie [verificatie van Azure Resource Manager aanvragen](/rest/api/index).

Alle taken die u op resources uitvoert met behulp van de Azure Resource Manager moeten worden geverifieerd met Azure Active Directory door de volgende stappen uit te voeren:

-   Een toepassing toevoegen aan de Azure Active Directory-Tenant.
-   Stel machtigingen in voor de toepassing die u hebt toegevoegd.
-   Het token ophalen voor het verifiëren van aanvragen voor het Azure Resource Manager.

### <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga met het abonnement dat uw API Management service-exemplaar bevat naar het tabblad **app-registraties** in **Azure Active Directory** (Azure Active Directory > beheer/app-registraties).

    > [!NOTE]
    > Als de standaard directory Azure Active Directory niet zichtbaar is voor uw account, neemt u contact op met de beheerder van het Azure-abonnement om de vereiste machtigingen voor uw account te verlenen.

3. Klik op **Nieuwe toepassing registreren**.

    Het venster **maken** wordt aan de rechter kant weer gegeven. Daar voert u de relevante gegevens van de AAD-app in.

4. Voer een naam in voor de toepassing.
5. Selecteer **systeem eigen**voor het toepassings type.
6. Voer een URL `http://resources` voor de tijdelijke aanduiding in, zoals voor de omleidings- **URI**, omdat het een vereist veld is, maar de waarde niet later wordt gebruikt. Klik op het selectie vakje om de toepassing op te slaan.
7. Klik op **Create**.

### <a name="add-an-application"></a>Een toepassing toevoegen

1. Zodra de toepassing is gemaakt, klikt u op **instellingen**.
2. Klik op **vereiste machtigingen**.
3. Klik op **+ toevoegen**.
4. Druk op **een API selecteren**.
5. Kies **Windows** **Azure Service Management-API**.
6. Druk op **selecteren**.

    ![Machtigingen toevoegen](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Klik op **gedelegeerde machtigingen** naast de zojuist toegevoegde toepassing, schakel het selectie vakje in voor **toegang tot Azure Service Management (preview)** .
8. Druk op **selecteren**.
9. Klik op **machtigingen verlenen**.

### <a name="configuring-your-app"></a>Uw app configureren

Voordat u de Api's aanroept die de back-up genereren en deze herstellen, moet u een Token ophalen. In het volgende voor beeld wordt het NuGet-pakket [micro soft. Identity model. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) gebruikt om het token op te halen.

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

Vervang `{tenant id}`, `{application id}` en`{redirect uri}` gebruik de volgende instructies:

1. Vervang `{tenant id}` door de Tenant-id van de Azure Active Directory toepassing die u hebt gemaakt. U kunt de id openen door te klikken op **app-registraties** -> -**eind punten**.

    ![Eindpunten][api-management-endpoint]

2. Vervang `{application id}` door de waarde die u krijgt door te navigeren naar de pagina **instellingen** .
3. Vervang de `{redirect uri}` door de waarde van het tabblad omleidings- **uri's** van uw Azure Active Directory-toepassing.

    Zodra de waarden zijn opgegeven, moet in het code voorbeeld een token worden geretourneerd dat lijkt op het volgende voor beeld:

    ![Token][api-management-arm-token]

    > [!NOTE]
    > Het token verloopt mogelijk na een bepaalde periode. Voer het code voorbeeld opnieuw uit om een nieuw token te genereren.

## <a name="calling-the-backup-and-restore-operations"></a>De back-up-en herstel bewerkingen aanroepen

De REST-Api's zijn [API Management-service-Backup](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) -en [API Management-service-herstellen](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore).

Voordat u de bewerkingen ' back-up maken en terugzetten ' aanroept die in de volgende secties worden beschreven, stelt u de header van de autorisatie aanvraag in voor uw REST-aanroep.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="step1"> </a>Een back-up maken van een API Management-service

Als u een back-up wilt maken van een API Management service, geeft u de volgende HTTP-aanvraag:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

positie

-   `subscriptionId`-ID van het abonnement dat de API Management-service bevat waarvan u een back-up wilt maken
-   `resourceGroupName`-de naam van de resource groep van uw Azure API Management-service
-   `serviceName`-de naam van de API Management-service waarvan u een back-up wilt maken die is opgegeven op het moment dat deze wordt gemaakt
-   `api-version`-vervangen door`2018-06-01-preview`

Geef in de hoofd tekst van de aanvraag de doel-Azure Storage-account naam, de toegangs sleutel, de naam van de BLOB-container en de naam van de back-up op:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Stel de waarde van de `Content-Type` aanvraag header in `application/json`op.

Het maken van een back-up is een langlopende bewerking die langer dan een minuut kan duren. Als de aanvraag is voltooid en het back-upproces is gestart `202 Accepted` , ontvangt u een antwoord `Location` status code met een header. Stel Get-aanvragen naar de URL in de `Location` header in om de status van de bewerking te bepalen. Terwijl de back-up wordt uitgevoerd, blijft de status code ' 202 geaccepteerd ' ontvangen. Met een antwoord code `200 OK` wordt aangegeven dat de back-upbewerking is voltooid.

Houd rekening met de volgende beperkingen bij het maken van een back-upaanvraag:

-   De in de hoofd tekst van de aanvraag opgegeven **container** **moet bestaan**.
-   Zorg dat er een back-up wordt gemaakt **van wijzigingen in Service beheer** , zoals SKU-upgrade of downgrade, wijziging in domein naam en meer.
-   Het terugzetten van een **back-up wordt alleen 30 dagen** na het moment van maken gegarandeerd.
-   **Gebruiks gegevens** die worden gebruikt voor het maken van analyse rapporten, worden **niet opgenomen** in de back-up. Gebruik [Azure API Management rest API][azure api management rest api] om periodiek analyse rapporten op te halen voor het bewaren van gegevens.
-   De frequentie waarmee u Service back-ups uitvoert, is van invloed op uw Recovery Point Objective. Om het te minimaliseren, wordt u aangeraden regel matige back-ups te implementeren en back-ups op aanvraag uit te voeren nadat u wijzigingen hebt aangebracht in uw API Management-service.
-   **Wijzigingen** in de service configuratie, (bijvoorbeeld api's, beleids regels en ontwikkelaars Portal) terwijl de back-upbewerking wordt uitgevoerd, wordt **mogelijk uitgesloten van de back-up en gaat verloren**.
-   Toegang vanaf het besturings element tot Azure Storage account **toestaan** . De klant moet de volgende set inkomende Ip's op hun opslag account openen voor back-up. 
    > 13.84.189.17/32, 13.85.22.63/32, 23.96.224.175/32, 23.101.166.38/32, 52.162.110.80/32, 104.214.19.224/32, 13.64.39.16/32, 40.81.47.216/32, 51.145.179.78/32, 52.142.95.35/32, 40.90.185.46/32, 20.40.125.155/32
### <a name="step2"> </a>Een API Management-service herstellen

Als u een API Management-service van een eerder gemaakte back-up wilt herstellen, moet u de volgende HTTP-aanvraag doen:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

positie

-   `subscriptionId`-ID van het abonnement dat de API Management-service bevat waarvan u een back-up wilt herstellen
-   `resourceGroupName`-de naam van de resource groep die de Azure API Management-service bevat waarvan u een back-up wilt herstellen
-   `serviceName`-de naam van de API Management-service die wordt hersteld op de aanmaak tijd die wordt gemaakt
-   `api-version`-vervangen door`2018-06-01-preview`

In de hoofd tekst van de aanvraag geeft u de locatie van het back-upbestand op. Dat wil zeggen, de Azure Storage-account naam, de toegangs sleutel, de naam van de BLOB-container en de naam van de back-up toevoegen:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

Stel de waarde van de `Content-Type` aanvraag header in `application/json`op.

Herstellen is een langlopende bewerking die Maxi maal 30 minuten kan duren. Als de aanvraag is voltooid en het herstel proces is gestart, ontvangt `202 Accepted` u een antwoord status code `Location` met een header. Stel Get-aanvragen naar de URL in de `Location` header in om de status van de bewerking te bepalen. Terwijl het herstel wordt uitgevoerd, blijft de status code ' 202 accepted ' ontvangen. Met een antwoord code `200 OK` wordt aangegeven dat de herstel bewerking is voltooid.

> [!IMPORTANT]
> **De SKU** van de service die wordt hersteld in **moet overeenkomen met** de SKU van de back-upservice die wordt hersteld.
>
> **Wijzigingen** die zijn aangebracht in de service configuratie (bijvoorbeeld api's, beleids regels, ontwikkelaars Portal vormgeving) terwijl de herstel bewerking wordt uitgevoerd, **kunnen worden overschreven**.

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Back-up-en herstel bewerkingen kunnen ook worden uitgevoerd met respectievelijk Power shell _Backup-AzApiManagement_ en Restore _-AzApiManagement-_ opdrachten.

## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende bronnen voor verschillende scenario's van het back-up-en herstel proces.

-   [Azure API Management-accounts repliceren](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
-   [Back-up maken en terugzetten in API Management automatiseren met Logic Apps](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps)
-   [Azure API Management: Het maken van een back-](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)up en het herstellen van_de configuratie de aanpak die wordt gedetailleerd door Stuart komt niet overeen met de officiële richt lijnen, maar is wel interessant._ 
    

[backup an api management service]: #step1
[restore an api management service]: #step2
[azure api management rest api]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest
[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png
[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
