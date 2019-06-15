---
title: 'Licenties toewijzen aan een groep: Azure Active Directory | Microsoft Docs'
description: Over het toewijzen van licenties aan gebruikers met behulp van Azure Active Directory-groep licentieverlening
services: active-directory
keywords: Azure AD-licenties
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a54d1ad3ab809f2a2f8df6ae0e30b1b061c2be1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60471270"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Licenties toewijzen aan gebruikers door groepslidmaatschappen in Azure Active Directory

Dit artikel helpt u bij het toewijzen van productlicenties aan een groep gebruikers in Azure Active Directory (Azure AD) en vervolgens verifiëren dat ze correct zijn gelicentieerd.

In dit voorbeeld bevat de tenant een beveiligingsgroep met de naam **HR-afdeling**. Deze groep bevat alle leden van de afdeling human resources (ongeveer 1000 gebruikers). U Office 365 Enterprise E3-licenties toewijzen aan de gehele afdeling. De Enterprise Yammer-service die opgenomen in het product moet tijdelijk worden uitgeschakeld totdat de afdeling gereed is om deze te gebruiken. U ook wilt implementeren van Enterprise Mobility + Security-licenties in dezelfde groep gebruikers.

> [!NOTE]
> Sommige services van Microsoft zijn niet op alle locaties beschikbaar. Voordat een licentie kan worden toegewezen aan een gebruiker, wordt de beheerder heeft om op te geven van de locatie-eigenschap van het gebruik van de gebruiker.
> 
> Alle gebruikers geen gebruikslocatie opgegeven overnemen voor licentietoewijzing groep, de locatie van de map. Als u gebruikers op meerdere locaties hebt, raden wij aan dat u altijd gebruikslocatie als onderdeel van de gebruikersstroom maken in Azure AD (bijvoorbeeld via AAD Connect-configuratie) - die ervoor zorgt het resultaat van licentietoewijzing dat altijd juist is en gebruikers geen ontvangen Services op locaties die niet zijn toegestaan.

## <a name="step-1-assign-the-required-licenses"></a>Stap 1: De vereiste licenties toewijzen

1. Aanmelden bij de [ **Azure AD-beheercentrum** ](https://aad.portal.azure.com) met een licentie voor administrator-account. Voor het beheren van licenties, moet het account een licentiebeheerder, beheerder of globale beheerder.

2. Selecteer **licenties** om een deelvenster waar u kunt zien en beheren van alle licentieproducten in de tenant te openen.

4. Onder **alle producten**, Office 365 Enterprise E5 en Enterprise Mobility + Security E3 selecteren door de productnamen te selecteren. Selecteer eerst de toewijzing **toewijzen** aan de bovenkant van het deelvenster.

   ![Producten selecteren voor het toewijzen van licenties](./media/licensing-groups-assign/all-products-assign.png)
  
5. Op de **licentie toewijzen** venster **gebruikers en groepen** om een lijst van gebruikers en groepen te openen.

6. Selecteer een gebruiker of groep, en gebruik vervolgens de **Selecteer** knop aan de onderkant van het deelvenster uw selectie te bevestigen.

7. Op de **licentie toewijzen** deelvenster, klikt u op **toewijzingsopties**, weergeven met alle service-abonnementen opgenomen in de twee producten die we eerder hebt geselecteerd. Zoek **Yammer Enterprise** en schakel het **uit** om uit te schakelen die service van de productlicentie. Controleer of door te klikken op **OK** aan de onderkant van **licentieopties**.

   ![serviceplannen voor licenties selecteren](./media/licensing-groups-assign/assignment-options.png)
  
8. Voltooi de toewijzing door onder aan het deelvenster **Licentie toewijzen** op **Toewijzen** te klikken.

9. Een melding wordt weergegeven in de rechterbovenhoek waarin de status en het resultaat van het proces. Als de toewijzing aan de groep kan niet worden voltooid (bijvoorbeeld vanwege bestaande licenties in de groep), klikt u op de melding om details van de fout te bekijken.

Wanneer de licenties toewijzen aan een groep, Azure AD verwerkt alle bestaande leden van die groep. Dit proces kan even, variëren met de grootte van de groep. De volgende stap wordt beschreven hoe om te verifiëren dat het proces is voltooid en bepalen als meer aandacht is vereist voor het oplossen van problemen.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Stap 2: Controleer of de eerste toewijzing is voltooid

1. Ga naar **Azure Active Directory** > **groepen**. Selecteer de groep die licenties zijn toegewezen aan.

2. Selecteer in de groep **licenties**. Hiermee kunt u snel bevestigen als licenties volledig zijn toegewezen aan gebruikers en als er fouten optreden die u nodig hebt om uit te zoeken. De volgende informatie is beschikbaar:

   - Lijst van productlicenties die momenteel zijn toegewezen aan de groep. Selecteer een item om weer te geven van de specifieke services die zijn ingeschakeld en wijzigingen aanbrengen.

   - De status van de meest recente licentiewijzigingen die zijn aangebracht aan de groep (als de wijzigingen worden verwerkt of als de verwerking is voltooid voor alle gebruikersleden van de).

   - Informatie over gebruikers die zich in een foutstatus omdat licenties kunnen niet worden toegewezen aan.

   ![licentieverlening fouten en licentiestatus](./media/licensing-groups-assign/assignment-errors.png)

3. Meer gedetailleerde informatie over het verwerken van onder licentie **Azure Active Directory** > **gebruikers en groepen** > *groepsnaam*  >  **Auditlogboeken**. Houd rekening met de volgende activiteiten:

   - Activiteit: **Start de toepassing zijn op basis van groepslicentie op gebruikers**. Dit wordt geregistreerd wanneer het systeem het wijzigen van het toewijzen van licenties op de groep wordt opgehaald en wordt gestart toe te passen op alle gebruikersleden van de. Bevat informatie over de wijziging die is gemaakt.

   - Activiteit: **Toepassen op basis van groepslicentie op gebruikers voltooien**. Dit wordt geregistreerd wanneer het systeem is verwerkt alle gebruikers in de groep. Het bevat een overzicht van hoeveel gebruikers zijn verwerkt en hoeveel gebruikers Groepslicenties kunnen niet worden toegewezen.

   [Lees deze sectie](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) voor meer informatie over hoe de logboeken voor controle kunnen worden gebruikt voor het analyseren van wijzigingen van Groepslicenties.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Stap 3: Controleren op problemen en op te lossen

1. Ga naar **Azure Active Directory** > **groepen**, en de groep die licenties zijn toegewezen aan niet vinden.
2. Selecteer in de groep **licenties**. De melding boven op het deelvenster ziet u dat er 10 gebruikers die licenties kunnen niet worden toegewezen aan zijn. Open om te zien of een lijst met alle gebruikers in een licentie foutstatus van deze groep.
3. De **mislukte toewijzingen** kolom kan worden achterhaald dat beide productlicenties kunnen niet worden toegewezen aan de gebruikers. De **belangrijkste reden voor mislukken** kolom bevat de oorzaak van het probleem. In dit geval heeft **conflicterende serviceabonnementen**.

   ![kunnen niet worden toegewezen licenties](./media/licensing-groups-assign/failed-assignments.png)

4. Selecteer een gebruiker te openen de **licenties** deelvenster. Dit deelvenster toont alle licenties die momenteel zijn toegewezen aan de gebruiker. In dit voorbeeld wordt de gebruiker heeft de Office 365 Enterprise E1-licentie die is overgenomen van de **Kiosk gebruikers** groep. Dit veroorzaakt een conflict met de E3-licentie die het systeem heeft geprobeerd om toe te passen van de **HR-afdeling** groep. Als gevolg hiervan is geen van de licenties van die groep toegewezen aan de gebruiker.

   ![Alle licentie conflicten voor een gebruiker weergeven](./media/licensing-groups-assign/user-license-view.png)

5. Als u wilt dit conflict is opgelost, verwijdert u de gebruiker van de **Kiosk gebruikers** groep. Nadat Azure AD de wijziging, verwerkt de **HR-afdeling** licenties correct worden toegewezen.

   ![Correct zijn hier licenties toegewezen](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functieset voor Licentiebeheer via groepen, de volgende artikelen:

* [Wat is licentieverlening in Azure Active Directory op basis van groep?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Licentieproblemen voor een groep vaststellen en oplossen in Azure Active Directory](licensing-groups-resolve-problems.md)
* [Gebruikers met een afzonderlijke licentie migreren naar licenties op basis van groepen in Azure Active Directory](licensing-groups-migrate-users.md)
* [Het migreren van gebruikers tussen productlicenties groepsgebaseerde licentieverlening in Azure Active Directory gebruiken](licensing-groups-change-licenses.md)
* [Aanvullende scenario’s voor Azure Active Directory-licenties op basis van groepen](../active-directory-licensing-group-advanced.md)
* [PowerShell-voorbeelden voor Groepslicenties in Azure Active Directory](licensing-ps-examples.md)
