---
title: Het verwijderen van een gebruiker toegang tot een toepassing | Microsoft Docs
description: Meer informatie over het verwijderen van een gebruiker toegang tot een toepassing
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b69502995eff88df53af3671a8e611809f83e59
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65826099"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Het verwijderen van een gebruiker toegang tot een toepassing

In dit artikel helpt u te begrijpen hoe u van een gebruiker toegang tot een toepassing te verwijderen.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Ik wil een specifieke gebruiker of groep van toewijzing aan een toepassing verwijderen

Als u wilt verwijderen van een gebruiker of groepstoewijzing aan een toepassing, volgt u de stappen in de [de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Ik wil alle toegang tot een toepassing voor elke gebruiker uitschakelen

Als u wilt alle gebruikers zich aangemeld bij een toepassing uitschakelen, volgt u de stappen in de [gebruikersaanmeldingen voor een enterprise-app in Azure Active Directory uitschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-to-delete-an-application-entirely"></a>Ik wil een toepassing volledig verwijderen

Naar **verwijderen van een toepassing**, volgt u deze instructies:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt verwijderen.

7. Nadat de toepassing wordt geladen, klikt u op **verwijderen** pictogram in de bovenste toepassing **overzicht** deelvenster.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Ik wil alle bewerkingen van toekomstige gebruikers toestemming voor elke toepassing uitschakelen

Toestemming van de gebruiker wordt uitgeschakeld voor uw hele directory te voorkomen dat eindgebruikers ermee akkoord dat voor elke toepassing. Beheerders kunnen nog steeds instemmen namens gebruiker. Voor meer informatie over de toepassing toestemming geven en waarom u mogelijk of niet nalevingsredenen om dit te doen, Lees [inzicht in gebruikers- en toestemming van een beheerder](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent). Zie ook [machtigingen en toestemming](../develop/v2-permissions-and-consent.md).

Naar **uitschakelen van alle toekomstige gebruikers toestemming bewerkingen in uw hele directory**, volgt u deze instructies:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** 

3.  Klik op **bedrijfstoepassingen** in het navigatiemenu.

5.  Klik op **gebruikersinstellingen**.

6.  Stelt de **gebruikers kunnen toestaan dat apps toegang hebben tot bedrijfsgegevens namens hen** in-of uitschakelen om te **Nee** en klik op de knop opslaan.


## <a name="next-steps"></a>Volgende stappen

[Toegang tot apps beheren](what-is-access-management.md)
