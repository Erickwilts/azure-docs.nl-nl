---
title: Toevoegen of bijwerken van een gebruiker profielgegevens - Azure Active Directory | Microsoft Docs
description: Instructies over hoe u gegevens toevoegt aan het profiel van een gebruiker in Azure Active Directory, met inbegrip van de details van een afbeelding en de taak.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4002c14ed6340a0e1a89ee295d47c98436515775
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561774"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Toevoegen of bijwerken van de profielgegevens van een gebruiker met behulp van Azure Active Directory
Profielgegevens, zoals een profielfoto, taak-specifieke informatie en enkele instellingen die met Azure Active Directory (Azure AD) toevoegen. Zie voor meer informatie over het toevoegen van nieuwe gebruikers [toevoegen of verwijderen van gebruikers in Azure Active Directory](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Toevoegen of wijzigen van de profielgegevens
Zoals u ziet, is er meer informatie beschikbaar in het profiel van een gebruiker dan wat kunt u om toe te voegen tijdens het maken van de gebruiker. Deze extra informatie is optioneel en, indien nodig door uw organisatie kan worden toegevoegd.

## <a name="to-add-or-change-profile-information"></a>Toevoegen of wijzigen van de profielgegevens
1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) als een gebruikers beheerder voor de organisatie.

2. Selecteer **Azure Active Directory**, selecteer **gebruikers**, en selecteer vervolgens een gebruiker. Bijvoorbeeld, _Alain Charon_.

    De **Alain Charon - profiel** pagina wordt weergegeven.

    ![De profielpagina van de gebruiker, met inbegrip van bewerkbare informatie](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Selecteer **bewerken** (optioneel) toevoegen of bijwerken van de informatie die is opgenomen in elk van de beschikbare secties.

    ![De profielpagina van de gebruiker, met de bewerkbare gebieden](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **De profielfoto.** Selecteer een miniatuur voor het gebruikersaccount. Deze afbeelding wordt weergegeven in Azure Active Directory en op van de gebruiker persoonlijke's, zoals de pagina myapps.microsoft.com.

    - **De identiteit.** Een extra identiteits waarde voor de gebruiker toevoegen of bijwerken, zoals een getrouwde achternaam. U kunt deze naam onafhankelijk van de waarden voor voor naam en achternaam instellen. U kunt dit bijvoorbeeld gebruiken om initialen, een bedrijfs naam op te geven of om de weer gegeven volg orde van de namen te wijzigen. Een ander voor beeld: voor twee gebruikers met de naam Chris Green kunt u de teken reeks voor identiteiten gebruiken om de namen in te stellen op Chris B. Green ' Chris R. Green (Contoso). '

    - **Taakgegevens.** Toevoegen van een taak-gerelateerde informatie, zoals de functie, afdeling of beheer van de gebruiker.

    - **Instellingen.** Bepaal of de gebruiker zich bij Azure Active Directory-tenant aanmelden kan. U kunt ook de globale locatie van de gebruiker opgeven.

    - **Contactgegevens.** Relevante contactgegevens voor de gebruiker toevoegen. Bijvoorbeeld, een adres of een mobiel telefoonnummer.

    - **Contactgegevens voor verificatie.** Controleer of deze informatie om te controleren of dat er is een actieve telefoonnummer en e-mailadres voor de gebruiker. Deze informatie wordt gebruikt door Azure Active Directory om te controleren of dat de gebruiker is echt de gebruiker tijdens het aanmelden. Contactgegevens voor verificatie kan alleen door een globale beheerder worden bijgewerkt.

4. Selecteer **Opslaan**.

    Al uw wijzigingen worden opgeslagen voor de gebruiker.

    >[!Note]
    >U moet Windows Server Active Directory gebruiken om bij te werken van de identiteit, contactgegevens of taakgegevens voor waarvan u de bron van de instantie Windows Server Active Directory is-gebruikers. Nadat u de update hebt voltooid, moet u de volgende synchronisatiecyclus uitvoeren voordat u de wijzigingen ziet wachten.

## <a name="next-steps"></a>Volgende stappen
Nadat u de profielen van uw gebruikers hebt bijgewerkt, kunt u de volgende basis-processen uitvoeren:

- [Toevoegen of verwijderen van gebruikers](add-users-azure-active-directory.md)

- [Rollen toewijzen aan gebruikers](active-directory-users-assign-role-azure-portal.md)

- [Een basisgroep maken en leden toevoegen](active-directory-groups-create-azure-portal.md)

Of u andere gebruiker-beheertaken, zoals het toewijzen van gemachtigden, met behulp van beleid en het delen van gebruikersaccounts kan uitvoeren. Zie voor meer informatie over andere beschikbare acties [Azure Active Directory management gebruikersdocumentatie](../users-groups-roles/index.yml).
