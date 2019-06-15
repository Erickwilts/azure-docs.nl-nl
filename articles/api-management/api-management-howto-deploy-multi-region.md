---
title: Azure API Management-services implementeren op meerdere Azure-regio's | Microsoft Docs
description: Informatie over het implementeren van een Azure API Management service-exemplaar naar meerdere Azure-regio's.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: d22da92355616c208c7616b4b0e8c26b7f9e7006
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60658154"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Over het implementeren van een Azure API Management service-exemplaar naar meerdere Azure-regio 's

Met Azure API Management biedt ondersteuning voor implementatie voor meerdere regio's, waarmee de API-uitgevers voor het distribueren van een enkele Azure API management-service naar een willekeurig aantal gewenste Azure-regio's. Dit vermindert de latentie waargenomen door consumenten API geografisch wordt gedistribueerd en verbetert tevens de servicebeschikbaarheid als één regio offline gaat.

Een nieuwe Azure API Management-service bevat in eerste instantie slechts één [eenheid] [ unit] in één Azure-regio, de primaire regio. Extra regio's kunnen eenvoudig worden toegevoegd via de Azure-portal. Een API Management gateway-server is geïmplementeerd in elke regio en aanroep verkeer wordt doorgestuurd naar de dichtstbijzijnde gateway in termen van latentie. Als een regio offline gaat, wordt het verkeer automatisch omgeleid naar de volgende dichtstbijzijnde gateway.

> [!NOTE]
> Met Azure API Management wordt alleen het onderdeel in de API-gateway gerepliceerd tussen regio's. Het onderdeel van de management-service wordt alleen in de primaire regio gehost. In het geval van een storing in de primaire regio is wijzigingen in de configuratie toepassen op een exemplaar van de Azure API Management-service niet mogelijk - met inbegrip van instellingen of beleid voor updates.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="add-region"> </a>Exemplaar van API Management-service implementeren naar een nieuwe regio

> [!NOTE]
> Als u een exemplaar van API Management-service nog niet hebt gemaakt, raadpleegt u [API Management service-exemplaar maken][Create an API Management service instance].

In de Azure-portal, gaat u naar de **schalen en prijzen** pagina voor uw API Management service-exemplaar. 

![Het tabblad schalen][api-management-scale-service]

Als u wilt implementeren naar een nieuwe regio, klikt u op **+ toevoegen regio** via de werkbalk.

![Regio toevoegen][api-management-add-region]

Selecteer de locatie in de vervolgkeuzelijst en stel het aantal eenheden voor met de schuifregelaar.

![Eenheden opgeven][api-management-select-location-units]

Klik op **toevoegen** om uw selectie in de tabel locaties. 

Herhaal dit proces totdat u alle geconfigureerde locaties en klikt u op **opslaan** via de werkbalk om het implementatieproces te starten.

## <a name="remove-region"> </a>Verwijderen van exemplaar van API Management-service vanaf een locatie

In de Azure-portal, gaat u naar de **schalen en prijzen** pagina voor uw API Management service-exemplaar. 

![Het tabblad schalen][api-management-scale-service]

Voor de locatie die u wilt verwijderen, opent u de context menu met behulp van de **...**  knop aan de rechterkant van de tabel. Selecteer de **verwijderen** optie.

Bevestig de verwijdering en klikt u op **opslaan** de wijzigingen moeten worden toegepast.

## <a name="route-backend"> </a>Route API-aanroepen naar regionale back-endservices

Met Azure API Management biedt slechts één back-end-service-URL. Hoewel er zijn exemplaren van Azure API Management in verschillende regio's, wordt de API-gateway aanvragen nog steeds doorsturen naar de dezelfde back endservice, die wordt geïmplementeerd in slechts één regio. In dit geval de prestatieverbetering afkomstig is alleen van antwoorden in de cache opgeslagen in Azure API Management in een regio die specifiek zijn voor de aanvraag, maar contact opnemen met de back-end over de hele wereld kan nog steeds leiden tot hoge latentie.

Als u wilt maken volledig gebruik van geografische verdeling van uw systeem, moet u back endservices die zijn geïmplementeerd in dezelfde regio's als Azure API Management-instanties hebben. Klik vervolgens met behulp van beleid en `@(context.Deployment.Region)` eigenschap, kunt u het verkeer routeren naar lokale exemplaren van uw back-end.

1. Navigeer naar uw Azure API Management-exemplaar en klik op **API's** in het menu links.
2. Selecteer de gewenste API.
3. Klik op **Code-editor** in de vervolgkeuzelijst pijl in de **binnenkomende verwerking**.

    ![API-code-editor](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Gebruik de `set-backend` in combinatie met voorwaardelijke `choose` beleid voor het maken van een juiste routeringsbeleid in de `<inbound> </inbound>` sectie van het bestand.

    Bijvoorbeeld, de onderstaande XML-bestand voor regio's VS-West en Oost-Azië zou moeten werken:

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

> [!TIP]
> U kunt ook uw back-endservices met front [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), de API-aanroepen naar de Traffic Manager sturen en laat het kunt oplossen door automatisch de routering.

## <a name="custom-routing"> </a>Gebruik aangepaste routering naar regionale gateways van API Management

API Management stuurt aanvragen naar een regionaal *gateway* op basis van [de laagste latentie](../traffic-manager/traffic-manager-routing-methods.md#performance). Hoewel het niet mogelijk om op te heffen met deze instelling in API Management is, kunt u uw eigen Traffic Manager kunt gebruiken met aangepaste regels voor doorsturen.

1. Maak uw eigen [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/).
1. Als u een aangepast domein [gebruiken met Traffic Manager](../traffic-manager/traffic-manager-point-internet-domain.md) in plaats van de API Management-service.
1. [De regionale API Management-eindpunten configureren in Traffic Manager](../traffic-manager/traffic-manager-manage-endpoints.md). De regionale eindpunten volgen het patroon van de URL van `https://<service-name>-<region>-01.regional.azure-api.net`, bijvoorbeeld `https://contoso-westus2-01.regional.azure-api.net`.
1. [De API Management-status van regionale-eindpunten configureren in Traffic Manager](../traffic-manager/traffic-manager-monitoring.md). De status van regionale eindpunten volgen het patroon van de URL van `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef`, bijvoorbeeld `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef`.
1. Geef [de routeringsmethode](../traffic-manager/traffic-manager-routing-methods.md) van Traffic Manager.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: https://azure.microsoft.com/pricing/details/api-management/
[Premium]: https://azure.microsoft.com/pricing/details/api-management/
