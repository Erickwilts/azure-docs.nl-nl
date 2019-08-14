---
title: Apparaten beheren met de Azure Portal | Microsoft Docs
description: Meer informatie over het gebruik van de Azure Portal voor het beheren van apparaten.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03c9f0908d8b5290dc4585a330a7ea78a6577ab9
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68942949"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Apparaat-id's beheren met de Azure Portal

Met apparaat-id-beheer in Azure Active Directory (Azure AD) kunt u ervoor zorgen dat uw gebruikers toegang krijgen tot uw resources vanaf apparaten die voldoen aan uw normen voor beveiliging en naleving.

Dit artikel:

- Er wordt van uitgegaan dat u bekend bent met de [Inleiding tot apparaat-id-beheer in azure Active Directory](overview.md)
- Biedt informatie over het beheren van uw apparaat-id's met behulp van de Azure AD-Portal

## <a name="manage-device-identities"></a>Apparaatidentiteiten beheren

De Azure AD-Portal biedt een centrale plek voor het beheren van uw apparaat-id's. U kunt deze locatie bereiken met behulp van een [directe koppeling](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) of door de volgende hand matige stappen te volgen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als beheerder.
2. Klik op **Active Directory**in de linkernavigatiebalk.

   ![Apparaatinstellingen configureren](./media/device-management-azure-portal/01.png)

3. Klik in de sectie **beheren** op **apparaten**.

   ![Apparaatinstellingen configureren](./media/device-management-azure-portal/74.png)

Op de pagina **apparaten** kunt u het volgende doen:

- Apparaatinstellingen configureren
- Apparaten zoeken
- Identiteits beheer taken voor apparaten uitvoeren
- Audit logboeken met betrekking tot het apparaat controleren  
  
## <a name="configure-device-settings"></a>Apparaatinstellingen configureren

Als u uw apparaat-id's wilt beheren met de Azure AD-Portal, moeten uw apparaten zijn [geregistreerd of zijn gekoppeld](overview.md) aan Azure AD. Als beheerder kunt u het proces voor het registreren en toevoegen van apparaten nauw keurig afstemmen door de apparaatinstellingen te configureren.

![Apparaatinstellingen configureren](./media/device-management-azure-portal/22.png)

Op de pagina Apparaatinstellingen kunt u het volgende configureren:

![Een intune-apparaat beheren](./media/device-management-azure-portal/21.png)

- **Gebruikers kunnen apparaten toevoegen aan Azure AD** : met deze instelling kunt u de gebruikers selecteren die hun apparaten als aan Azure AD gekoppelde apparaten kunnen registreren. De standaard waarde is **alle**.

> [!NOTE]
> **Gebruikers mogen apparaten toevoegen aan de Azure AD** -instelling is alleen van toepassing op Azure AD-deelname op Windows 10.

- **Aanvullende lokale beheerders op aan Azure AD gekoppelde apparaten** : u kunt de gebruikers selecteren die lokale beheerders rechten op een apparaat krijgen. Gebruikers die hier worden toegevoegd, worden toegevoegd aan de rol van het *apparaat Administrators* in azure AD. Globale beheerders in azure AD en eigen aren van apparaten krijgen standaard lokale beheerders rechten. Deze optie is een Premium Edition-mogelijkheid die beschikbaar is via producten als Azure AD Premium of de Enter prise Mobility Suite (EMS).
- **Gebruikers kunnen hun apparaten registreren bij Azure AD** . u moet deze instelling configureren zodat Windows 10 Personal-, Ios-, Android-en macOs-apparaten kunnen worden geregistreerd bij Azure AD. Als u **geen**selecteert, mogen apparaten zich niet registreren bij Azure AD. Inschrijving met Microsoft Intune of Mobile Device Management (MDM) voor Office 365 vereist registratie. Als u een van deze services hebt geconfigureerd **alle** is geselecteerd en **is** niet beschikbaar.
- **Multi-factor Authentication vereisen voor het toevoegen van apparaten** : u kunt kiezen of gebruikers een extra verificatie factor moeten opgeven om hun apparaat toe te voegen aan Azure AD. De standaard waarde is **Nee**. U wordt aangeraden multi-factor Authentication te vereisen bij het registreren van een apparaat. Voordat u multi-factor Authentication inschakelt voor deze service, moet u ervoor zorgen dat multi-factor Authentication is geconfigureerd voor de gebruikers die hun apparaten registreren. Zie aan de slag [met Azure multi-factor Authentication](../authentication/concept-mfa-whichversion.md)voor meer informatie over de verschillende Azure multi-factor Authentication-Services. 

> [!NOTE]
> **Vereisen dat multi-factor Authentication wordt toegevoegd** aan de instelling apparaten is niet van toepassing op hybride apparaten die zijn toegevoegd aan Azure AD.

- **Maximum aantal apparaten** : met deze instelling kunt u het maximum aantal apparaten selecteren dat een gebruiker in azure AD kan hebben. Als een gebruiker dit quotum bereikt, kunnen ze geen extra apparaten toevoegen totdat een of meer van de bestaande apparaten worden verwijderd. Het quotum van het apparaat wordt geteld voor alle apparaten die zijn toegevoegd aan Azure AD of Azure AD die vandaag zijn geregistreerd. De standaard waarde is **20**.

> [!NOTE]
> De instelling **maximum aantal apparaten** is niet van toepassing op hybride apparaten die zijn toegevoegd aan Azure AD.

- **Gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten** -deze instelling is standaard ingesteld op **NONE**. Als u specifieke gebruikers of groepen selecteert, kunnen de instellingen en app-gegevens van de gebruiker worden gesynchroniseerd op hun Windows 10-apparaten. Meer informatie over hoe synchronisatie werkt in Windows 10.
Deze optie is een Premium-functie die beschikbaar is via producten als Azure AD Premium of de Enter prise Mobility Suite (EMS).

## <a name="locate-devices"></a>Apparaten zoeken

U hebt twee opties voor het zoeken van geregistreerde en gekoppelde apparaten:

- **Alle apparaten** in het gedeelte **beheren** van de pagina **apparaten**  

   ![Alle apparaten](./media/device-management-azure-portal/41.png)

- **Apparaten** in het gedeelte **beheren** van een **gebruikers** pagina

   ![Alle apparaten](./media/device-management-azure-portal/43.png)

Met beide opties kunt u een weer gave openen die:

- Met kunt u zoeken naar apparaten met behulp van de weergave naam of apparaat-ID als filter.
- Biedt een gedetailleerd overzicht van geregistreerde en gekoppelde apparaten
- Hiermee kunt u algemene beheer taken voor apparaten uitvoeren

![Alle apparaten](./media/device-management-azure-portal/51.png)

>[!TIP]
>
>* Als u een apparaat ziet dat ' hybride Azure AD join ' met de status ' in behandeling ' heeft in de kolom geregistreerd, geeft dit aan dat het apparaat is gesynchroniseerd vanuit Azure AD Connect en wacht op het volt ooien van de registratie van de client. Meer informatie over het [plannen van uw hybride Azure AD-koppelings implementatie](hybrid-azuread-join-plan.md). Meer informatie vindt u in het artikel, [Veelgestelde vragen over apparaten](faq.md).
>
>   ![Apparaten in behandeling](./media/device-management-azure-portal/75.png)
>
>* Voor sommige iOS-apparaten kunnen de apparaatnamen die apostrofs bevatten mogelijk andere tekens gebruiken die eruitzien als apostrofs. Het is dus lastig om dergelijke apparaten te doorzoeken: als u de zoek resultaten niet op de juiste manier ziet, moet u ervoor zorgen dat de zoek reeks overeenkomt met het apostrof-teken.

## <a name="device-identity-management-tasks"></a>Taken voor het beheer van apparaat-id's

Als globale beheerder of beheerder van een Cloud apparaat kunt u de geregistreerde of gekoppelde apparaten beheren. InTune-service beheerders kunnen:

- Apparaten bijwerken: voor beelden zijn dagelijkse bewerkingen, zoals het inschakelen/uitschakelen van apparaten
- Apparaten verwijderen: wanneer een apparaat buiten gebruik wordt gesteld en in azure AD moet worden verwijderd

In deze sectie vindt u informatie over algemene beheer taken voor identiteiten van apparaten.

### <a name="manage-an-intune-device"></a>Een intune-apparaat beheren

Als u intune-beheerder bent, kunt u apparaten beheren die zijn gemarkeerd als **Microsoft intune**. Als het apparaat niet is inge schreven bij Microsoft Intune wordt de optie ' beheren ' grijs weer gegeven.

![Een intune-apparaat beheren](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Een Azure AD-apparaat in-of uitschakelen

Als u een apparaat wilt in-of uitschakelen, hebt u twee opties:

- Het menu taken (...) op de pagina **alle apparaten**

   ![Een intune-apparaat beheren](./media/device-management-azure-portal/71.png)

- De werk balk op de pagina **apparaten**

   ![Een intune-apparaat beheren](./media/device-management-azure-portal/32.png)

**Opmerkingen**

- U moet een globale beheerder of een beheerder van een Cloud apparaat in azure AD zijn om een apparaat in-of uit te scha kelen. 
- Als u een apparaat uitschakelt, kan een apparaat niet worden geverifieerd met Azure AD, waardoor het apparaat geen toegang heeft tot uw Azure AD-resources die worden beschermd door de CA van het apparaat of het gebruik van uw WH4B-referenties.

### <a name="delete-an-azure-ad-device"></a>Een Azure AD-apparaat verwijderen

Als u een apparaat wilt verwijderen, hebt u twee opties:

- Het menu taken (...) op de pagina **alle apparaten**

   ![Een intune-apparaat beheren](./media/device-management-azure-portal/72.png)

- De werk balk op de pagina **apparaten**

   ![Een apparaat verwijderen](./media/device-management-azure-portal/34.png)

**Opmerkingen**

- Als u een apparaat wilt verwijderen, moet u een globale beheerder of intune-beheerder zijn in azure AD.
- Een apparaat verwijderen:
   - Hiermee voor komt u dat een apparaat toegang heeft tot uw Azure AD-resources.
   - Hiermee verwijdert u alle details die zijn gekoppeld aan het apparaat, bijvoorbeeld BitLocker-sleutels voor Windows-apparaten.  
   - Vertegenwoordigt een niet-herstel bare activiteit en wordt niet aanbevolen, tenzij dit vereist is.

Als een apparaat wordt beheerd door een andere beheer instantie (bijvoorbeeld Microsoft Intune), moet u ervoor zorgen dat het apparaat is gewist/buiten gebruik is gesteld voordat het apparaat in azure AD wordt verwijderd. Lees hoe u [verouderde apparaten beheert](device-management-azure-portal.md) voordat u apparaten verwijdert.

### <a name="view-or-copy-device-id"></a>Apparaat-ID weer geven of kopiëren

U kunt een apparaat-ID gebruiken om de details van de apparaat-ID op het apparaat te controleren of door Power shell te gebruiken tijdens het oplossen van problemen. Als u de Kopieer optie wilt openen, klikt u op het apparaat.

![Een apparaat-ID weer geven](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>BitLocker-sleutels weer geven of kopiëren

U kunt de BitLocker-sleutels weer geven en kopiëren om gebruikers te helpen hun versleutelde station te herstellen. Deze sleutels zijn alleen beschikbaar voor Windows-apparaten die zijn versleuteld en waarvan de sleutels zijn opgeslagen in azure AD. U kunt deze sleutels kopiëren bij het openen van Details van het apparaat.

![BitLocker-sleutels weer geven](./media/device-management-azure-portal/36.png)

Als u de BitLocker-sleutels wilt bekijken of kopiëren, moet u eigenaar zijn van het apparaat of een gebruiker zijn die ten minste een van de volgende rollen heeft:

- Cloud-apparaatbeheerder
- Globale beheerder
- Helpdeskbeheerder
- Intune-servicebeheerder
- Beveiligingsbeheerder
- Beveiligingslezer

> [!NOTE]
> Hybride Azure AD-gekoppelde Windows 10-apparaten hebben geen eigenaar. Als u dus op zoek bent naar een apparaat op basis van de eigenaar en niet hebt gevonden, zoekt u op de apparaat-ID.

## <a name="audit-logs"></a>Controlelogboeken

Apparaat-activiteiten zijn beschikbaar via de activiteiten Logboeken. Deze logboeken bevatten activiteiten die worden geactiveerd door de Device Registration service en door gebruikers:

- Apparaten maken en toevoegen van eigen aren/gebruikers op het apparaat
- Wijzigingen in Apparaatinstellingen
- Apparaat-bewerkingen, zoals het verwijderen of bijwerken van een apparaat

Uw ingangs punt voor de controle gegevens is **controle logboeken** in het gedeelte **activiteit** van de pagina **apparaten** .

![Controlelogboeken](./media/device-management-azure-portal/61.png)

Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- De datum en tijd waarop de gebeurtenis is opgetreden
- De doelen
- De initiator/Actor (wie) van een activiteit
- De activiteit (wat)

![Controlelogboeken](./media/device-management-azure-portal/63.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Controlelogboeken](./media/device-management-azure-portal/64.png)

Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de controlegegevens filteren met de volgende velden:

- Categorie
- Resourcetype van activiteit
- Activiteit
- Datumbereik
- Doel
- Gestart door (Actor)

Naast de filters kunt u zoeken naar specifieke vermeldingen.

![Controlelogboeken](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Volgende stappen

[Verouderde apparaten beheren in azure AD](manage-stale-devices.md)
