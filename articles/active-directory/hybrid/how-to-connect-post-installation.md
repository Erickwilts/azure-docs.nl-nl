---
title: 'Azure AD Connect: Volgende stappen en over het beheren van Azure AD Connect | Microsoft Docs'
description: Informatie over het uitbreiden van de standaardconfiguratie en operationele taken voor Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c204029557a73dc3f02015afb92c0fdbf0d4d50e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64571299"
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Volgende stappen en over het beheren van Azure AD Connect
Gebruik de operationele procedures in dit artikel om aan te passen van Azure Active Directory (Azure AD) Connect om te voldoen aan de behoeften en vereisten van uw organisatie.  

## <a name="add-additional-sync-admins"></a>Aanvullende synchronisatie beheerders toevoegen
Standaard worden alleen de gebruiker die heeft de installatie- en lokale beheerders kunnen voor het beheren van de geïnstalleerde synchronisatie-engine. Voor extra gebruikers kunnen toegang tot en beheer van de synchronisatie-engine, zoek de groep ADSyncAdmins met de naam op de lokale server en deze toevoegen aan deze groep.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Licenties toewijzen aan gebruikers van Azure AD Premium en Enterprise Mobility Suite
Nu dat uw gebruikers zijn gesynchroniseerd naar de cloud, moet u ze een licentie toewijzen, zodat ze kunnen aan de slag met cloud-apps zoals Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Een Azure AD Premium of Enterprise Mobility Suite-licentie toewijzen

1. Aanmelden bij de Azure-portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Op de **Active Directory** pagina, dubbelklik op de map met de gebruikers die u wilt instellen.
4. Selecteer boven aan de pagina met de adreslijst **Licenties**.
5. Op de **licenties** weergeeft, schakelt **Active Directory Premium** of **Enterprise Mobility Suite**, en klik vervolgens op **toewijzen**.
6. Selecteer in het dialoogvenster de gebruikers aan wie u een licentie wilt toewijzen en klik op het vinkje om de wijzigingen op te slaan.

## <a name="verify-the-scheduled-synchronization-task"></a>Controleer of de geplande synchronisatie-taak
De Azure portal gebruiken om de status van een synchronisatie te controleren.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Om te controleren of de geplande synchronisatie-taak
1. Aanmelden bij de Azure-portal als een beheerder.
2. Selecteer aan de linkerkant **Active Directory**.
3. Selecteer aan de linkerkant **Azure AD Connect**
4. Aan de bovenkant van de pagina, houd er rekening mee de laatste synchronisatie.

![Tijd van Directory-synchronisatie](./media/how-to-connect-post-installation/verify2.png)

## <a name="start-a-scheduled-synchronization-task"></a>Een taak geplande synchronisatie starten
Als u een synchronisatietaak uitvoeren wilt, kunt u dit doen door:

1. Dubbelklik op het bureaublad een snelkoppeling Azure AD Connect om de wizard te starten.
2. Klik op **Configureren**.
3. Selecteer op het scherm taken de **aanpassen Synchronisatieopties** en klikt u op **volgende**
4. Voer uw Azure AD-referenties in
5. Klik op **volgende**. Klik op **volgende**.  Klik op **volgende**.
5.  Op de **klaar om te configureren** scherm, zorg ervoor dat de **Start het synchronisatieproces wanneer de configuratie is voltooid** is ingeschakeld.
6.  Klik op **Configureren**.

Zie voor meer informatie over de Azure AD Connect sync Scheduler [Azure AD Connect Scheduler](how-to-connect-sync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Aanvullende taken die beschikbaar zijn in Azure AD Connect
Na de initiële installatie van Azure AD Connect, kunt u altijd de wizard opnieuw starten vanuit de Azure AD Connect-start-snelkoppeling pagina of het bureaublad.  U ziet dat de wizard opnieuw doorlopen aantal nieuwe opties in de vorm van aanvullende taken biedt.  

De volgende tabel bevat een overzicht van deze taken en een korte beschrijving van elke taak.

![Lijst met extra taken](./media/how-to-connect-post-installation/addtasks2.png)

| Extra taken | Description |
| --- | --- |
|**Privacy-instellingen**|Bekijken welke telemetriegegevens wordt gedeeld met Microsoft.|
|**De huidige configuratie weergeven**|Bekijk uw huidige Azure AD Connect-oplossing.  Dit omvat algemene instellingen, gesynchroniseerd mappen en synchronisatie-instellingen. |
| **Synchronisatieopties aanpassen** |De huidige configuratie, zoals aanvullende Active Directory-forests toevoegen aan de configuratie of het inschakelen van synchronisatie-opties, zoals gebruikers, groepen, apparaat of terugschrijven van wachtwoord wijzigen. |
|**Apparaatopties configureren**|Apparaatopties die beschikbaar zijn voor synchronisatie|
|**Directory-schema vernieuwen**|Hiermee kunt u nieuwe on-premises directory-objecten voor synchronisatie toevoegen|
|**De Faseringsmodus configureren** |Fase-informatie die niet direct is gesynchroniseerd en is niet geëxporteerd naar Azure AD of on-premises Active Directory.  Met deze functie kunt u de synchronisaties bekijken voordat ze optreden. |
|**Aanmelden van gebruikers wijzigen**|Wijzig de verificatiemethode die gebruikers gebruiken om aan te melden|
|**Federation beheren**|Beheren van uw AD FS-infrastructuur, het vernieuwen van certificaten en het toevoegen van AD FS-servers|
|**Problemen oplossen**|Help bij het oplossen van problemen met Azure AD Connect|

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
