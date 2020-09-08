---
title: Identiteitsbeveiliging en voorwaardelijke toegang instellen in Azure AD B2C
description: Meer informatie over hoe u identiteitsbeveiliging en voorwaardelijke toegang voor u Azure AD B2C Tenant configureert om riskante aanmeldingen en andere risicogebeurtenissen weer te geven en beleidsregels te maken op basis van risicodetecties.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 09/01/2020
ms.author: mimart
author: msmimart
manager: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5554cfcde9aba1b0e5c9c8b60e2e6a7e9a8ba378
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/01/2020
ms.locfileid: "89270667"
---
# <a name="set-up-identity-protection-and-conditional-access-in-azure-ad-b2c"></a>Identiteitsbeveiliging en voorwaardelijke toegang instellen in Azure AD B2C

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

Identiteitsbeveiliging biedt voortdurende risicodetectie voor uw Azure AD B2C-Tenant. Als uw Azure AD B2C-prijscategorie voor tenants een Premium P2 is, kunt u gedetailleerde risicogebeurtenissen voor identiteitsbeveiliging in de Azure Portal weergeven. U kunt ook beleid voor [voorwaardelijke toegang](../active-directory/conditional-access/overview.md) gebruiken op basis van deze risicodetecties om acties te bepalen en organisatiebeleid af te dwingen.

## <a name="prerequisites"></a>Vereisten

- Uw Azure AD B2C-tenant moet zijn [gekoppeld aan een Azure AD-abonnement](billing.md#link-an-azure-ad-b2c-tenant-to-a-subscription).
- Azure AD B2C Premium P2 is vereist voor het gebruik van voorwaardelijke toegang op basis van aanmeldings- en gebruikersrisico's. Wijzig indien nodig [uw Azure AD B2C-prijscategorie in Premium P2](https://aka.ms/exid-pricing-tier). 
- Als u identiteitsbeveiliging en voorwaardelijke toegang in uw B2C-tenant wilt beheren, hebt u een account nodig waaraan de rol van globale beheerder of de rol beveiligingsbeheerder is toegewezen.
- Als u deze functies in uw tenant wilt gebruiken, moet u eerst overschakelen naar de prijscategorie Azure AD B2C Premium P2.

## <a name="set-up-identity-protection"></a>Identiteitsbeveiliging instellen

Identiteitsbeveiliging is standaard ingeschakeld. Als u risico gebeurtenissen voor identiteitsbeveiliging in uw Azure AD B2C Tenant wilt weergeven, koppelt u uw Azure AD B2C Tenant aan een Azure AD-abonnement en selecteert u de prijscategorie Azure AD B2C Premium P2. U kunt gedetailleerde rapporten van risicogebeurtenissen weergeven in de Azure Portal.

### <a name="supported-identity-protection-risk-detections"></a>Ondersteunde risicodetecties voor identiteits beveiliging

De volgende risicodetecties worden momenteel ondersteund voor Azure AD B2C:  

|Risicodetectietype  |Beschrijving  |
|---------|---------|
| Ongewoon traject     | Aanmelding vanaf een ongewone locatie op basis van recente aanmeldingen van de gebruiker.        |
|Anoniem IP-adres     | Aanmelding vanaf een anoniem IP-adres (bijvoorbeeld Tor-browser, anonymizer-VPN’s)        |
|Onbekende aanmeldingseigenschappen     | Aanmelding met eigenschappen die niet recent zijn waargenomen voor de gebruiker.        |
|Aan malware gekoppeld IP-adres     | Aanmelding via een aan malware gekoppeld IP-adres         |
|Azure AD-bedreigingsinformatie     | De interne en externe bedreigingsinformatiebronnen van Microsoft hebben een bekend aanvalspatroon geïdentificeerd        |

## <a name="view-risk-events-for-your-azure-ad-b2c-tenant"></a>Risicogebeurtenissen voor uw Azure AD B2C-tenant weergeven

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.

1. Zoek en selecteer **Azure AD B2C** in de Azure-portal.

1. Selecteer onder **Beveiliging** **Riskante gebruikers (preview)** .

   ![Riskante gebruikers](media/conditional-access-identity-protection-setup/risky-users.png)

1. Selecteer onder **Beveiliging** **Risicodetecties (preview)** .

   ![Risicodetectie](media/conditional-access-identity-protection-setup/risk-detections.png)

## <a name="add-a-conditional-access-policy"></a>Een beleid voor voorwaardelijke toegang toevoegen 

Als u een beleid voor voorwaardelijke toegang wilt toevoegen op basis van de risicodetecties voor identiteitsbeveiliging, moet u ervoor zorgen dat de standaardinstellingen voor beveiliging zijn uitgeschakeld voor uw Azure AD B2C Tenant en vervolgens het beleid voor voorwaardelijke toegang maken.

### <a name="to-disable-security-defaults"></a>Standaardinstellingen voor beveiliging uitschakelen

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

2. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.

3. Zoek en selecteer in de Azure-portal de optie **Azure Active Directory**.

4. Selecteer **Eigenschappen** en selecteer vervolgens **Standaardinstellingen voor beveiliging beheren**.

   ![Standaardinstellingen voor beveiliging uitschakelen](media/conditional-access-identity-protection-setup/disable-security-defaults.png)

5. Selecteer bij Standaardinstellingen voor beveiliging inschakelen de optie Nee. 

   ![Stel de Standaardinstellingen voor beveiliging inschakelen in op Nee](media/conditional-access-identity-protection-setup/enable-security-defaults-toggle.png)

### <a name="to-create-a-conditional-access-policy"></a>Een beleid voor voorwaardelijke toegang maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

1. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.

1. Zoek en selecteer **Azure AD B2C** in de Azure-portal.

1. Selecteer **Voorwaardelijke toegang (preview)** onder **Beveiliging**. De pagina **Beleid voor voorwaardelijke toegang** wordt geopend. 

1. Selecteer **Nieuw beleid** en volg de documentatie voor voorwaardelijke toegang van Azure AD om een nieuw beleid te maken. Hier volgt een voorbeeld:

   - [Aanmelden met voorwaardelijke toegang op basis van risico's: Inschakelen met beleid voor voorwaardelijke toegang](../active-directory/conditional-access/howto-conditional-access-policy-risk.md#enable-with-conditional-access-policy)

   > [!IMPORTANT]
   > Wanneer u de gebruikers selecteert waarop u het beleid wilt toepassen, selecteert u niet alleen **Alle gebruikers**, want u kunt uw eigen aanmelding dan blokkeren.

## <a name="test-the-conditional-access-policy"></a>Het beleid voor voorwaardelijke toegang testen

1. Maak met behulp van de volgende instellingen een beleid voor voorwaardelijke toegang:
   
   - Selecteer onder **Gebruikers en groepen** de testgebruiker. Selecteer niet **Alle gebruikers** of u blokkeert uzelf bij het aanmelden.
   - Kies voor **Cloud-apps of -acties** **Apps selecteren** en kies vervolgens uw Relying Party-toepassing.
   - Selecteer als voorwaarden **Aanmeldingsrisico** en **Hoge** **Medium** en **Lage** risiconiveaus.
   - Kies **Toegang blokkeren** voor **Verlenen**.

      ![Kies Toegang blokkeren](media/conditional-access-identity-protection-setup/test-conditional-access-policy.png)

1. Schakel uw testbeleid voor voorwaardelijke toegang in door **Maken** te selecteren.

1. Simuleer een riskante aanmelding met behulp van de [Tor-Browser](https://www.torproject.org/download/). 

1. In het gedecodeerde token van jwt.ms voor de aanmeldingspoging moet u zien dat de aanmelding is geblokkeerd:

   ![Een geblokkeerde aanmelding testen](media/conditional-access-identity-protection-setup/test-blocked-sign-in.png)

## <a name="review-conditional-access-outcomes-in-the-audit-report"></a>Bekijk de resultaten voor voorwaardelijke toegang in het controlerapport

Het resultaat van een voorwaardelijke toegangsgebeurtenis bekijken:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).

2. Selecteer het pictogram **Map + Abonnement** in de werkbalk van de portal en selecteer vervolgens de map die uw Azure AD B2C-tenant bevat.

3. Zoek en selecteer **Azure AD B2C** in de Azure-portal.

4. Onder **Activiteiten** selecteert u **Controlelogboeken**.

5. Filter het controlelogboek door **Categorie** in te stellen op **B2C** en **Activiteit-resourcetype** op **IdentityProtection**. Selecteer vervolgens **Toepassen**.

6. Controleer de controle-activiteit gedurende de afgelopen 7 dagen. De volgende typen activiteiten zijn opgenomen:

   - **Beleidsregels voor voorwaardelijke toegang evalueren**: Deze vermelding in het controlelogboek geeft aan dat er een evaluatie van voorwaardelijke toegang is uitgevoerd tijdens een verificatie.
   - **Gebruiker herstellen**: Deze vermelding geeft aan dat aan de subsidie of vereisten van een beleid voor voorwaardelijke toegang is voldaan door de eindgebruiker en dat deze activiteit is gerapporteerd aan de risico-engine om het risico van de gebruiker te beperken.

7. Selecteer een **Beleid voor voorwaardelijke toegang evalueren**-logboekvermelding in de lijst om de details van **-activiteit te openen: Controlelogboek**-pagina, waarin de audit logboek-id's worden weer gegeven, samen met deze informatie in de sectie **Aanvullende details**:

   - ConditionalAccessResult: De toekenning die is vereist voor de evaluatie van het voorwaardelijke beleid.
   - AppliedPolicies: Een lijst met alle beleidsregels voor voorwaardelijke toegang waarin aan de voorwaarden is voldaan en het beleid is ingeschakeld.
   - ReportingPolicies: Een lijst met beleidsregels voor voorwaardelijke toegang die zijn ingesteld op de modus Alleen rapport en waar aan de voorwaarden is voldaan.

## <a name="next-steps"></a>Volgende stappen

[Voorwaardelijke toegang toevoegen aan een gebruikersstroom](conditional-access-user-flow.md).
