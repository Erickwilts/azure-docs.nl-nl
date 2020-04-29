---
title: DNS-namen configureren met Traffic Manager
description: Meer informatie over het configureren van een aangepast domein voor een Azure App Service-app die kan worden geïntegreerd met Traffic Manager voor taak verdeling.
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.topic: article
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: f8322c12669e41fc7c9aa88e99f95cf1b26ea87d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78944138"
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-with-traffic-manager-integration"></a>Een aangepaste domein naam configureren in Azure App Service met Traffic Manager-integratie

[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

> [!NOTE]
> Zie voor Cloud Services [een aangepaste domein naam configureren voor een Azure-Cloud service](../cloud-services/cloud-services-custom-domain-name.md).

Wanneer u [Azure Traffic Manager](/azure/traffic-manager/) gebruikt om verkeer te verdelen naar [Azure app service](overview.md), is de app service-app toegankelijk via ** \<Traffic-Manager-endpoint>. trafficmanager.net**. U kunt een aangepaste domein naam, zoals www\.contoso.com, toewijzen met uw app service-app om een herken bare domein naam voor uw gebruikers te bieden.

In dit artikel wordt beschreven hoe u een aangepaste domein naam configureert met een App Service-app die is geïntegreerd met [Traffic Manager](../traffic-manager/traffic-manager-overview.md).

> [!NOTE]
> Alleen [CNAME](https://en.wikipedia.org/wiki/CNAME_record) -records worden ondersteund wanneer u een domein naam configureert met behulp van het Traffic Manager-eind punt. Omdat een record niet wordt ondersteund, wordt een toewijzing van een hoofd domein, zoals contoso.com, ook niet ondersteund.
> 

## <a name="prepare-the-app"></a>De app voorbereiden

Als u een aangepaste DNS-naam wilt toewijzen aan een app die is geïntegreerd met Azure Traffic Manager, moet het [app service plan](https://azure.microsoft.com/pricing/details/app-service/) van de web-app op de laag **Standard** of hoger zijn. In deze stap zorgt u ervoor dat de App Service-app zich in de ondersteunde prijscategorie bevindt.

### <a name="check-the-pricing-tier"></a>Controleer de prijscategorie

Zoek in het [Azure Portal](https://portal.azure.com)naar en selecteer **app Services**.

Selecteer op de pagina **app Services** de naam van uw Azure-app.

![Navigatie naar Azure-app in de portal](./media/app-service-web-tutorial-custom-domain/select-app.png)

Selecteer in de linkernavigatiebalk van de app-pagina **Omhoog schalen (app service plan)**.

![Menu Opschalen](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

De huidige laag van de app wordt gemarkeerd door een blauwe rand. Controleer of de app zich in de **Standard** -laag of hoger (een laag in de **productie** -of **geïsoleerde** categorie) bevindt. Zo ja, sluit u de pagina **opschalen** en gaat u naar [het maken van de CNAME-toewijzing](#create-the-cname-mapping).

![Controleer prijscategorie](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

### <a name="scale-up-the-app-service-plan"></a>Het App Service-plan opschalen

Als u uw app wilt schalen, selecteert u een van de prijs categorieën in de **productie** categorie. Klik op **Aanvullende opties bekijken** voor aanvullende opties.

Klik op **Toepassen**.

## <a name="create-traffic-manager-endpoint"></a>Traffic Manager-eind punt maken

Volg de stappen bij het [toevoegen of verwijderen van eind punten](../traffic-manager/traffic-manager-endpoints.md)en voeg uw app service-app toe als een eind punt in uw Traffic Manager-profiel.

Zodra uw App Service-app een ondersteunde prijs categorie heeft, wordt deze weer gegeven in de lijst met beschik bare App Service doelen wanneer u het eind punt toevoegt. Als uw app niet wordt weer gegeven, [controleert u de prijs categorie van uw app](#prepare-the-app).

## <a name="create-the-cname-mapping"></a>De CNAME-toewijzing maken
> [!NOTE]
> Als u een [app service domein wilt configureren dat u hebt aangeschaft](manage-custom-dns-buy-domain.md), slaat u deze sectie over en gaat u naar [aangepast domein inschakelen](#enable-custom-domain).
> 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records-no-h.md)]

Hoewel de specifieke kenmerken van elke domein provider verschillen, kunt u toewijzen *vanuit* de aangepaste domein naam (zoals **contoso.com**) *aan* de Traffic Manager domein naam (**contoso.trafficmanager.net**) die is geïntegreerd met uw app.

> [!NOTE]
> Als er al een record wordt gebruikt en u uw apps aan het preventief moet koppelen, kunt u een extra CNAME-record maken. Als u bijvoorbeeld preventief **www\.contoso.com** wilt binden aan uw app, maakt u een CNAME-record van **awverify. www** in **contoso.trafficmanager.net**. U kunt vervolgens ' www\.contoso.com ' aan uw app toevoegen zonder dat u de ' www ' CNAME-record hoeft te wijzigen. Zie [een actieve DNS-naam migreren naar Azure app service](manage-custom-dns-migrate-domain.md)voor meer informatie.

Zodra u klaar bent met het toevoegen of wijzigen van DNS-records bij uw domein provider, slaat u de wijzigingen op.

## <a name="enable-custom-domain"></a>Aangepast domein inschakelen
Nadat de records voor uw domein naam zijn door gegeven, gebruikt u de browser om te controleren of uw aangepaste domein naam wordt omgezet in uw App Service-app.

> [!NOTE]
> Het kan enige tijd duren voordat uw CNAME door het DNS-systeem wordt door gegeven. U kunt een service gebruiken, <a href="https://www.digwebinterface.com/">https://www.digwebinterface.com/</a> bijvoorbeeld om te controleren of de CNAME beschikbaar is.
> 
> 

1. Zodra de domein omzetting is gelukt, gaat u terug naar de app-pagina in [Azure Portal](https://portal.azure.com)
2. Selecteer in de linkernavigatiebalk **aangepaste domeinen** > **hostnaam toevoegen**.
4. Typ de aangepaste domein naam die u eerder hebt toegewezen en selecteer **valideren**.
5. Zorg ervoor dat het **hostnaam-record type** is ingesteld op **CNAME\.(www example.com of een ander subdomein)**.

6. Omdat de App App Service nu is geïntegreerd met een Traffic Manager-eind punt, ziet u de Traffic Manager domein naam onder **CNAME-configuratie**. Selecteer deze en klik op **aangepast domein toevoegen**.

    ![DNS-naam toevoegen aan de app](./media/configure-domain-traffic-manager/enable-traffic-manager-domain.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een aangepaste DNS-naam beveiligen met een SSL-binding in Azure App Service](configure-ssl-bindings.md)
