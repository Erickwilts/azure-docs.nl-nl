---
title: Beheerde identiteiten voor een Azure-resources - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat informatie over het gebruik van beheerde identiteiten voor Azure-resources met Azure Event Hubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 05/20/2019
ms.author: shvija
ms.openlocfilehash: 4e6f16a15547583baab63f452504d36eb2e43b85
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65978475"
---
# <a name="managed-identities-for-azure-resources-with-event-hubs"></a>Beheerde identiteiten voor Azure-resources met Event Hubs

[Identiteiten voor een Azure-resources beheerd](../active-directory/managed-identities-azure-resources/overview.md) is een cross-Azure-functie die u kunt maken van een veilige identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. Daarna kunt u die identiteit koppelen met access-control-rollen die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing nodig heeft. 

Met beheerde identiteiten beheert het Azure-platform deze runtime-identiteit. U hoeft niet voor het opslaan en toegang tot sleutels in uw toepassingscode of de configuratie voor de identiteit zelf, of voor de resources die u nodig hebt om toegang te beveiligen. Een Event Hubs-client-app die wordt uitgevoerd binnen een Azure App Service-toepassing of in een virtuele machine met beheerde identiteiten voor een Azure-resources-ondersteuning is ingeschakeld, hoeft niet voor het afhandelen van SAS-regels en sleutels of andere toegangstokens. De client-app hoeft alleen het adres van het eindpunt van de Event Hubs-naamruimte. Wanneer de app verbinding maakt, wordt de context van de beheerde identiteit in Event Hubs gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel.

Zodra deze gekoppeld aan een beheerde identiteit is, kunt alle geautoriseerde bewerkingen uitvoeren in een Event Hubs-client. Autorisatie is verleend door een beheerde identiteit koppelen met behulp van Event Hubs. 

## <a name="event-hubs-roles-and-permissions"></a>Event Hubs-rollen en machtigingen
U kunt een beheerde identiteit toevoegen de **Event Hubs-Gegevenseigenaar** rol van een Event Hubs-naamruimte. Deze rol hebben de identiteit, volledig beheer (voor beheer en bewerkingen van gegevens) op alle entiteiten in de naamruimte.

>[!IMPORTANT]
> We eerder toe te voegen beheerde identiteit ondersteund de **eigenaar** of **Inzender** rol. Echter bevoegdheden voor toegang tot de gegevens **eigenaar** en **Inzender** rol niet meer worden herkend. Als u de **eigenaar** of **Inzender** rol, switch voor het gebruik van de **Event Hubs-Gegevenseigenaar** rol.

Volg deze stappen voor het gebruik van de nieuwe ingebouwde rol: 

1. Ga naar [Azure Portal](https://portal.azure.com)
2. Navigeer naar de Event Hubs-naamruimte.
3. Op de **Event Hubs Namespace** weergeeft, schakelt **toegang Control(IAM)** in het menu links.
4. Op de **Access Control (IAM)** weergeeft, schakelt **toevoegen** in de **een roltoewijzing toevoegen** sectie. 

    ![Een knop van de toewijzing van rol toevoegen](./media/event-hubs-managed-service-identity/add-role-assignment-button.png)
5. Op de **roltoewijzing toevoegen** pagina, de volgende stappen uit: 
    1. Voor **rol**, selecteer **Azure Event Hubs-Gegevenseigenaar**. 
    2. Selecteer de **identiteit** worden toegevoegd aan de rol.
    3. Selecteer **Opslaan**. 

        ![Event Hubs eigenaarsrol voor de gegevens](./media/event-hubs-managed-service-identity/add-role-assignment-dialog.png)
6. Schakel over naar de **roltoewijzingen** pagina en Bevestig dat de gebruiker wordt toegevoegd aan de **Azure Event Hubs-Gegevenseigenaar** rol. 

    ![Controleer of de gebruiker is toegevoegd aan de rol](./media/event-hubs-managed-service-identity/role-assignments.png)
 
## <a name="use-event-hubs-with-managed-identities-for-azure-resources"></a>Event Hubs gebruiken met beheerde identiteiten voor Azure-Resources

Het volgende gedeelte bevat de volgende stappen uit:

1. Maken en implementeren van een voorbeeldtoepassing die wordt uitgevoerd onder een beheerde identiteit.
2. Die identiteit toegang verlenen tot een Event Hubs-naamruimte.
3. Hoe de toepassing communiceert met eventhubs met behulp van die identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u dit doet in Azure bent, voert u de [in deze gebruiksaanwijzing](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van het maken van een MVC-toepassing zoals wordt weergegeven in de zelfstudie, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-identity"></a>Instellen van de beheerde identiteit

Zodra u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in Azure portal (wordt ook weergegeven in de instructies) en navigeer naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![Beheerde Service-identiteit pagina](./media/event-hubs-managed-service-identity/msi1.png)
 
Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-event-hubs-namespace"></a>Een nieuwe Event Hubs-naamruimte maken

Volgende [maken van een Event Hubs-naamruimte](event-hubs-create.md). 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **roltoewijzing toevoegen** om toe te voegen van de beheerde identiteit op de **eigenaar** rol. Om dit te doen, zoekt u de naam van de web-App in de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**. De beheerde identiteit voor de webtoepassing nu heeft toegang tot de Event Hubs-naamruimte en naar de event hub die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Wijzig nu de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt ook de web-toepassingscode van [deze GitHub-opslagplaats](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/MSI/EventHubsMSIDemoWebApp). 

Als u de app gestart, wijst u EventHubsMSIDemo.aspx van uw browser. U kunt dit ook instellen als uw startpagina. De code kan worden gevonden in het bestand EventHubsMSIDemo.aspx.cs. Het resultaat is een minimale webtoepassing met een paar velden, en met **verzenden** en **ontvangen** knoppen die verbinding maken met Event Hubs verzenden of ontvangen van gebeurtenissen. 

Houd er rekening mee hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt u een token-provider voor de beheerde identiteit met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.EventHubAudience)` aanroepen. Er zijn daarom geen geheimen opslaan en gebruiken. De stroom van de context van de beheerde identiteit met Event Hubs en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider, dit is een eenvoudiger dan het gebruik van SAS-model.

Nadat u deze wijzigingen aanbrengt, publiceren en de toepassing wordt uitgevoerd. U kunt de juiste publiceren van gegevens krijgen door te downloaden en vervolgens een publicatieprofiel in Visual Studio te importeren:

![Profiel voor publiceren importeren](./media/event-hubs-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt, en klik vervolgens op een **verzenden** of **ontvangen**. 
 
De beheerde identiteit werkt alleen in de Azure-omgeving, en alleen in de App Service-implementatie waarin u deze hebt geconfigureerd. Beheerde identiteiten op dit moment niet werken met implementatiesites voor App Service.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Details van prijzen van Eventhubs](https://azure.microsoft.com/pricing/details/event-hubs/)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
