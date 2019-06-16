---
title: Algemene verificatiefouten oplossen | Azure Marketplace
description: Biedt hulp bij veelvoorkomende verificatiefouten bij het gebruik van de Cloud Partner Portal-API's.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: ddf3c9ce26a1538d91f1e6d6bcc04fd0d18e7936
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935804"
---
# <a name="troubleshooting-common-authentication-errors"></a>Algemene verificatiefouten oplossen

In dit artikel biedt hulp bij veelvoorkomende verificatiefouten bij het gebruik van de Cloud Partner Portal-API's.

## <a name="unauthorized-error"></a>Niet gemachtigd

Als u consistent `401 unauthorized` fouten, of u hebt een geldig toegangstoken.  Als u hebt nog niet gedaan, maakt u een eenvoudige toepassing voor Azure Active Directory (Azure AD) en een service-principal zoals beschreven in [portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Vervolgens gebruikt u de toepassing of een eenvoudige HTTP POST-aanvraag om te controleren of de toegang.  U kunt de Tenant-ID, toepassings-ID, Object-ID en de geheime sleutel voor het verkrijgen van het toegangstoken zoals weergegeven in de volgende afbeelding wordt opnemen:

![De 401-fout probleemoplossing](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Niet-toegestane fout

Als er een `403 forbidden` fout, zorg ervoor dat de juiste service-principal is toegevoegd aan uw uitgeversaccount in de Cloud Partner-Portal.
Volg de stappen in de [vereisten](./cloud-partner-portal-api-prerequisites.md) pagina aan uw service-principal toevoegen aan de portal.

Als de juiste service-principal is toegevoegd, controleert u de gegevens. Let u sluiten op de Object-ID opgegeven in de portal. Er zijn twee Object-id's in de registratiepagina van de Azure Active Directory-app en moet u de lokale Object-ID. U kunt de juiste waarde vinden door te gaan naar de **App-registraties** -pagina voor uw app en te klikken op de naam van de app onder **beheerde toepassing in lokale map**. Hiermee gaat u naar de lokale eigenschappen voor de app, waar u vind de juiste Object-ID in de **eigenschappen** pagina, zoals wordt weergegeven in de volgende afbeelding. Zorg er ook voor dat u de juiste uitgevers-ID gebruiken wanneer u de service-principal toevoegen en de API-aanroep.

![De 403-fout probleemoplossing](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
