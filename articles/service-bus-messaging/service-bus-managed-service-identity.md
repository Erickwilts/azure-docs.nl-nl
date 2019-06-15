---
title: Beheerde identiteiten voor een Azure-resources met Azure Service Bus-preview | Microsoft Docs
description: Gebruik van beheerde identiteiten voor Azure-resources met Azure Service Bus
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/01/2018
ms.author: aschhab
ms.openlocfilehash: 8477ff8c8ff0bc1629ff4cdc61f7c28c6eed778c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65978798"
---
# <a name="managed-identities-for-azure-resources-with-service-bus"></a>Beheerde identiteiten voor Azure Service Bus-resources 

[Identiteiten voor een Azure-resources beheerd](../active-directory/managed-identities-azure-resources/overview.md) is een cross-Azure-functie die u kunt maken van een veilige identiteit die is gekoppeld aan de implementatie waarmee uw toepassingscode wordt uitgevoerd. Daarna kunt u die identiteit koppelen met access-control-rollen die aangepaste machtigingen voor toegang tot specifieke Azure-resources die uw toepassing nodig heeft.

Met beheerde identiteiten beheert het Azure-platform deze runtime-identiteit. U hoeft niet voor het opslaan en toegang tot sleutels in uw toepassingscode of de configuratie voor de identiteit zelf, of voor de resources die u nodig hebt om toegang te beveiligen. Een Service Bus client-app uitgevoerd in een Azure App Service-toepassing of in een virtuele machine met ingeschakelde entiteiten voor Azure-resources ondersteuning hoeft niet beheerde voor het afhandelen van SAS-regels en sleutels of andere toegangstokens. De client-app hoeft alleen het adres van het eindpunt van de Service Bus Messaging-naamruimte. Wanneer de app verbinding maakt, wordt de context van de beheerde entiteit in Service Bus gebonden aan de client in een bewerking die wordt weergegeven in een voorbeeld verderop in dit artikel. Zodra deze gekoppeld aan een beheerde identiteit is, kan uw Service Bus-clienttoepassing alle geautoriseerde bewerkingen kan uitvoeren. Autorisatie is verleend door te koppelen van een beheerde entiteit met behulp van Service Bus. 

## <a name="service-bus-roles-and-permissions"></a>Service Bus-rollen en machtigingen

U kunt een beheerde identiteit toevoegen aan de rol 'Service Bus Gegevenseigenaar' van een Service Bus-naamruimte. Deze biedt de identiteit, volledig beheer (voor beheer en bewerkingen van gegevens) op alle entiteiten in de naamruimte.

>[!IMPORTANT]
> We eerder toe te voegen beheerde identiteit ondersteund de **'Eigenaar'** of **"Bijdrager"** rol.
>
> Echter bevoegdheden voor toegang tot de gegevens **'Eigenaar'** en **"Bijdrager"** rol wordt niet meer worden herkend. Als u de **'Eigenaar'** of **"Bijdrager"** rol en vervolgens die moet worden aangepast aan het gebruik van de **'Service Bus Gegevenseigenaar'** rol.

Voor het gebruik van de nieuwe ingebouwde rol, voer de onderstaande stappen -

1. gaat u verder met de [Azure-portal](https://portal.azure.com)
2. Navigeer naar de Service Bus-naamruimte waarin u momenteel setup de 'Eigenaar' of ' ' de rol Inzender hebt.
3. Klik op 'Control(IAM) toegang' in het linkerdeelvenster-menu.
4. Gaat u verder met het toevoegen van een nieuwe roltoewijzing als hieronder

    ![](./media/service-bus-role-based-access-control/ServiceBus_RBAC_SBDataOwner.png)

5. Klik op 'Opslaan' om op te slaan van de nieuwe roltoewijzing.

## <a name="use-service-bus-with-managed-identities-for-azure-resources"></a>Service Bus gebruiken met beheerde identiteiten voor Azure-resources

De volgende sectie wordt beschreven hoe u maken en implementeren van een voorbeeldtoepassing die wordt uitgevoerd onder een beheerde identiteit, hoe u die identiteit toegang verlenen tot een Service Bus Messaging-naamruimte en hoe de toepassing communiceert met behulp van Service Bus-entiteiten Deze identiteit.

Deze inleiding beschrijft een webtoepassing die wordt gehost [Azure App Service](https://azure.microsoft.com/services/app-service/). De stappen die nodig zijn voor een virtuele machine wordt gehost toepassing lijken.

### <a name="create-an-app-service-web-application"></a>Een App Service-webtoepassing maken

De eerste stap is het maken van een App Service-ASP.NET-toepassing. Als u niet bekend met hoe u om dit te doen in Azure bent, voert u de [in deze gebruiksaanwijzing](../app-service/app-service-web-get-started-dotnet-framework.md). Echter, in plaats van het maken van een MVC-toepassing zoals wordt weergegeven in de zelfstudie, een Web Forms-toepassing maken.

### <a name="set-up-the-managed-identity"></a>Instellen van de beheerde identiteit

Zodra u de toepassing hebt gemaakt, gaat u naar de nieuwe web-app in Azure portal (wordt ook weergegeven in de instructies) en navigeer naar de **beheerde Service-identiteit** pagina en de functie inschakelen: 

![](./media/service-bus-managed-service-identity/msi1.png)

Nadat u de functie hebt ingeschakeld, is een nieuwe service-identiteit in uw Azure Active Directory gemaakt en geconfigureerd in de App Service-host.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Een nieuwe Service Bus Messaging-naamruimte maken

Volgende [maken van een Service Bus Messaging-naamruimte](service-bus-create-namespace-portal.md). 

Navigeer naar de naamruimte **Access Control (IAM)** pagina in de portal en klik vervolgens op **roltoewijzing toevoegen** om toe te voegen van de beheerde identiteit op de **eigenaar** rol. Om dit te doen, zoekt u de naam van de web-App in de **machtigingen toevoegen** deelvenster **Selecteer** veld en klik vervolgens op de vermelding. Klik vervolgens op **Opslaan**.

Beheerde identiteit van de web-App heeft nu toegang tot de Service Bus-naamruimte en naar de wachtrij die u eerder hebt gemaakt. 

### <a name="run-the-app"></a>De app uitvoeren

Wijzig nu de standaardpagina van de ASP.NET-toepassing die u hebt gemaakt. U kunt de code van de web-endtoepassing van [deze GitHub-opslagplaats](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

De pagina Default.aspx is de landingspagina. De code kan worden gevonden in het bestand Default.aspx.cs. Het resultaat is een minimale webtoepassing met een paar velden, en met **verzenden** en **ontvangen** knoppen die verbinding met Service Bus maken te verzenden of ontvangen van berichten.

Houd er rekening mee hoe de [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) -object is geïnitialiseerd. In plaats van de tokenprovider Shared Access Token (SAS), de code maakt u een token-provider voor de beheerde identiteit met de `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)` aanroepen. Er zijn daarom geen geheimen behouden en te gebruiken. De stroom van de context van de beheerde identiteit tot Service Bus en de autorisatie-handshake worden automatisch verwerkt door de tokenprovider. Het is een eenvoudiger dan het gebruik van SAS-model.

Nadat u deze wijzigingen aanbrengt, publiceren en de toepassing wordt uitgevoerd. U kunt de juiste publiceren van gegevens eenvoudig verkrijgen door te downloaden en vervolgens een publicatieprofiel in Visual Studio te importeren:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Als u wilt verzenden of ontvangen van berichten, voer de naam van de naamruimte en de naam van de entiteit die u hebt gemaakt. Klik vervolgens op een **verzenden** of **ontvangen**.


> [!NOTE]
> - De beheerde identiteit werkt alleen in de Azure-omgeving van App-services, Azure-VM's, en schaal worden ingesteld. Voor .NET-toepassingen, de Microsoft.Azure.Services.AppAuthentication library, die wordt gebruikt door de Service Bus NuGet-pakket, biedt een abstractie via dit protocol en biedt ondersteuning voor een lokale ontwikkeling-ervaring. Deze bibliotheek kunt u uw code lokaal op uw ontwikkelcomputer, met behulp van uw gebruikersaccount vanuit Visual Studio, Azure CLI 2.0 of Active Directory Integrated Authentication testen. Zie voor meer informatie over opties voor lokale ontwikkeling met deze bibliotheek [Service-naar-serviceverificatie naar Azure Key Vault met behulp van .NET](../key-vault/service-to-service-authentication.md).  
> 
> - Beheerde identiteiten werken op dit moment niet met implementatiesites voor App Service.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
