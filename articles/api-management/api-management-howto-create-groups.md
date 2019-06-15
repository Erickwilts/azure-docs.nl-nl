---
title: Accounts voor ontwikkelaars met behulp van groepen in Azure API Management beheren | Microsoft Docs
description: Informatie over het beheren van accounts voor ontwikkelaars met behulp van groepen in Azure API Management
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 5392cf5463dd0b11d1ce53856c8e4e2e788892b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60658391"
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Hoe u groepen maken en gebruiken voor het beheren van accounts voor ontwikkelaars in Azure API Management

In API Management worden groepen gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten voor het eerst zichtbaar aan groepen zijn gemaakt, en vervolgens ontwikkelaars in deze groepen kunnen weergeven en zich abonneren op de producten die gekoppeld aan de groepen zijn. 

API Management heeft de volgende onveranderbare systeemgroepen:

* **Beheerders** - Azure-abonnementbeheerders zijn lid van deze groep. Beheerders beheren service-exemplaren van API Management, door het maken van de API's, bewerkingen en producten die door ontwikkelaars worden gebruikt.
* **Ontwikkelaars** - Geverifieerde gebruikers van de ontwikkelaarsportal vallen in deze groep. Ontwikkelaars zijn de klanten die toepassingen bouwen met uw API's. Ontwikkelaars krijgen toegang tot de ontwikkelaarsportal en bouwen toepassingen waarmee de bewerkingen van een API worden aangeroepen.
* **Gasten** - Niet-geverifieerde gebruikers van de ontwikkelaarsportal, zoals potentiële klanten die de ontwikkelaarsportal van een API Management-exemplaar bezoeken, vallen in deze groep. Ze kunnen bepaalde alleen-lezentoegang krijgen, zoals de mogelijkheid om API's te bekijken maar niet om ze aan te roepen.

Naast deze systeemgroepen kunnen beheerders aangepaste groepen maken of [gebruikmaken van externe groepen in gekoppelde Azure Active Directory-tenants][leverage external groups in associated Azure Active Directory tenants]. Aangepaste en externe groepen kunnen naast systeemgroepen worden gebruikt om ontwikkelaars zichtbaarheid van en toegang tot API-producten te geven. U kunt bijvoorbeeld één aangepaste groep maken voor ontwikkelaars die zijn gekoppeld aan een specifieke partnerorganisatie en hen toegang geven tot de API's vanuit een product dat alleen relevante API's bevat. Een gebruiker kan lid zijn van meerdere groepen.

Deze handleiding wordt beschreven hoe beheerders van een exemplaar van API Management kunnen nieuwe groepen toevoegen en deze koppelen aan producten en -ontwikkelaars.

Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met behulp van de API Management REST API [groep](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) entiteit.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Vereisten

Taken in dit artikel uitvoeren: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"> </a>Een groep maken

Deze sectie wordt beschreven hoe u een nieuwe groep toevoegen aan uw API Management-account.

1. Selecteer de **groepen** tabblad aan de linkerkant van het scherm.
2. Klik op **+ toevoegen**.
3. Voer een unieke naam voor de groep en een optionele beschrijving.
4. Kies **Maken**.

    ![Een nieuwe groep toevoegen](./media/api-management-howto-create-groups/groups001.png)

Nadat de groep is gemaakt, wordt deze toegevoegd aan de **groepen** lijst. <br/>Bewerken van de **naam** of **beschrijving** van de groep, klikt u op de naam van de groep en **instellingen**.<br/>Als u wilt verwijderen van de groep, klikt u op de naam van de groep en druk op **verwijderen**.

Nu dat de groep is gemaakt, kan deze worden gekoppeld met producten en -ontwikkelaars.

## <a name="associate-group-product"> </a>Een groep te koppelen aan een product

1. Selecteer de **producten** tabblad aan de linkerkant.
2. Klik op de naam van het gewenste product.
3. Druk op **toegangsbeheer**.
4. Klik op **+ groep toevoegen**.

    ![Een groep te koppelen aan een product](./media/api-management-howto-create-groups/groups002.png)
5. Selecteer de groep die u wilt toevoegen.

    ![Een groep te koppelen aan een product](./media/api-management-howto-create-groups/groups003.png)

    Als u wilt een groep verwijderen uit het product, klikt u op **verwijderen**.

    ![Een groep verwijderen](./media/api-management-howto-create-groups/groups004.png)

Zodra een product gekoppeld aan een groep is, kunnen ontwikkelaars in die groep weergeven en zich abonneren op het product.

> [!NOTE]
> Als u wilt toevoegen in Azure Active Directory-groepen, Zie [hoe ontwikkelaarsaccounts authoriseren met Azure Active Directory in Azure API Management](api-management-howto-aad.md).

## <a name="associate-group-developer"> </a>Groepen koppelen aan ontwikkelaars

Deze sectie wordt beschreven hoe u groepen koppelen aan leden.

1. Selecteer de **groepen** tabblad aan de linkerkant van het scherm.
2. Selecteer **leden**.

    ![Een lid toevoegen](./media/api-management-howto-create-groups/groups005.png)
3. Druk op **+ toevoegen** en selecteer een lid.

    ![Een lid toevoegen](./media/api-management-howto-create-groups/groups006.png)
4. Druk op **Selecteer**.

Als de koppeling tussen de ontwikkelaar en de groep wordt toegevoegd, kunt u bekijken in de **gebruikers** tabblad.

## <a name="next-steps"> </a>Volgende stappen

* Nadat een ontwikkelaar is toegevoegd aan een groep, kunnen ze bekijken en zich abonneren op de producten die zijn gekoppeld aan die groep. Zie voor meer informatie, [maken en publiceren van een product in Azure API Management][How create and publish a product in Azure API Management],
* Naast het maken en beheren van groepen in de publicatieportal bevindt, kunt u maken en beheren van uw groepen met behulp van de API Management REST API [groep](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) entiteit.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md
