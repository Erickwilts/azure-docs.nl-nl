---
title: Activiteitenrapporten van Azure Active Directory-gebruiker niet vinden in Azure portal | Microsoft Docs
description: Meer informatie over waar de activiteitenrapporten van de Azure Active Directory-gebruiker zich bevinden in de Azure-portal.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: d47072713c57576abe780134792c3a5cbc27127c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60438276"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Activiteitenrapporten vinden in Azure portal

In dit artikel leert u hoe u Azure Active Directory (Azure AD) gebruiker-activiteitenrapporten vinden in Azure portal.

## <a name="audit-logs-report"></a>Rapport voor audittrails

Het Audittrailrapport zijn gerapporteerd combineert meerdere rapporten met betrekking tot de activiteiten van de toepassing in één weergave voor het melden van op basis van een context. Voor toegang tot het Audittrailrapport zijn gerapporteerd:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer uw directory in de rechterbovenhoek en selecteer vervolgens de **Azure Active Directory** blade van het navigatiedeelvenster links.
3. Selecteer **auditlogboeken** uit de **activiteit** sectie van de Azure Active Directory-blade. 

    ![Controlelogboeken](./media/howto-find-activity-reports/482.png "Controlelogboeken")

Het Audittrailrapport zijn gerapporteerd consolideert de volgende rapporten:

* Auditrapport
* Activiteit voor wachtwoord opnieuw instellen
* Registratie-activiteit wachtwoord opnieuw instellen
* Activiteit voor selfservicegroepen
* Naamswijzigingen voor Office 365-groep
* Inrichten van de activiteit van een account
* Status van het wachtwoord rollover
* Fouten bij het inrichten van een account

### <a name="filtering-on-audit-logs"></a>Filteren op auditlogboeken

U kunt geavanceerde filters in het controlerapport gebruiken voor toegang tot een specifieke categorie audit-gegevens door op te geven in de **categorie** filter. Bijvoorbeeld, als u wilt weergeven van alle activiteiten met betrekking tot gebruikers, selecteert u de **UserManagement** categorie. 

Categorieën zijn onder andere:

- Alle
- AdministrativeUnit
- Programmeert
- Authentication
- Autorisatie
- Contactpersoon
- Apparaat
- Apparaatconfiguratie
- DirectoryManagement
- EntitlementManagement
- GroupManagement
- Overige
- Beleid
- ResourceManagement
- RoleManagement
- UserManagement

U kunt ook filteren op een specifieke service met de **Service** vervolgkeuzelijst filteren. Bijvoorbeeld, als u alle gebeurtenissen voor beveiligingscontrole met betrekking tot selfservice wachtwoordbeheer, selecteer de **Self-service wachtwoordbeheer** filter.

Services omvatten:

- Alle
- Toegangsbeoordelingen
- Account inrichten 
- Toepassing eenmalige aanmelding
- Verificatiemethoden
- B2C
- Voorwaardelijke toegang
- Hoofddirectory
- Rechtenbeheer
- Identiteitsbeveiliging
- Uitgenodigde gebruikers
- PIM
- Self-service voor groepsbeheer
- Selfservice voor wachtwoordbeheer
- Gebruiksvoorwaarden

## <a name="sign-ins-report"></a>Aanmeldingenrapport 

De **aanmeldingen** weergave bevat alle gebruikersaanmeldingen, evenals de **toepassingsgebruik** rapport. Ook vindt u informatie over het gebruik van toepassing in de **beheren** sectie van de **bedrijfstoepassingen** overzicht.

Voor toegang tot het rapport aanmeldingen:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer uw directory in de rechterbovenhoek en selecteer vervolgens de **Azure Active Directory** blade van het navigatiedeelvenster links.
3. Selecteer **Signins** uit de **activiteit** sectie van de Azure Active Directory-blade. 

    ![Aanmeldingsweergave](./media/howto-find-activity-reports/483.png "aanmeldingen weergeven")


### <a name="filtering-on-application-name"></a>Filteren op toepassingsnaam

U kunt het rapport aanmeldingen gebruiken voor meer informatie over het toepassingsgebruik van, door te filteren op gebruikersnaam of toepassing.

![Filter aanmeldingsgebeurtenissen pagina](./media/howto-find-activity-reports/07.png "pagina aanmelden gebeurtenissen filteren")

## <a name="security-reports"></a>Beveiligingsrapporten

### <a name="anomalous-activity-reports"></a>Rapporten van afwijkende activiteiten

Afwijkende activiteitenrapporten bevatten informatie over beveiliging gerelateerde risicogebeurtenissen die Azure AD kunt detecteren en rapporteren van.

De volgende tabel afwijkende activiteit een lijst met de Azure AD beveiligingsrapporten en bijbehorende typen risicogebeurtenissen in Azure portal. Zie [Risicogebeurtenissen in Azure Active Directory](concept-risk-events.md) voor meer informatie.  


| Azure AD afwijkende activiteit-rapport |  Type risicogebeurtenis Identity protection|
| :--- | :--- |
| Gebruikers van wie de referenties zijn gelekt | Gelekte referenties |
| Onregelmatige aanmeldingsactiviteiten | Onmogelijke reis naar ongewone locaties |
| Aanmeldingen vanaf mogelijk geïnfecteerde apparaten | Aanmeldingen vanaf geïnfecteerde apparaten|
| Aanmeldingen van onbekende bronnen | Aanmeldingen vanaf anonieme IP-adressen |
| Aanmeldingen van IP-adressen met verdachte activiteit | Aanmeldingen van IP-adressen met verdachte activiteit |
| - | Aanmeldingen vanaf onbekende locaties |

De volgende afwijkende activiteitsbeveiliging voor Azure AD-rapporten worden niet opgenomen als risicogebeurtenissen in Azure portal:

* Aanmeldingen na meerdere mislukte pogingen
* Aanmeldingen vanuit meerdere locaties


### <a name="detected-risk-events"></a>Gedetecteerde risico

U hebt toegang tot rapporten over gedetecteerde risico in de **Security** sectie van de **Azure Active Directory** -blade in de [Azure-portal](https://portal.azure.com). Gedetecteerde risico worden bijgehouden in de volgende rapporten:   

- [Gebruikers die risico lopen](concept-user-at-risk.md)
- [Riskante aanmeldingen](concept-risky-sign-ins.md)

    ![Beveiligingsrapporten](./media/howto-find-activity-reports/04.png "beveiligingsrapporten")

## <a name="troubleshoot-issues-with-activity-reports"></a>Problemen oplossen met activiteitenrapporten

### <a name="missing-data-in-the-downloaded-activity-logs"></a>Ontbrekende gegevens in de gedownloade activiteitenlogboeken

#### <a name="symptoms"></a>Symptomen 

Ik heb de activiteitenlogboeken (audit of aanmeldingen) gedownload en ik zie niet alle records voor de tijd die ik heb geselecteerd. Hoe komt dat? 

 ![Rapportage](./media/troubleshoot-missing-data-download/01.png)
 
#### <a name="cause"></a>Oorzaak

Wanneer u activiteitenlogboeken in Azure portal downloadt, beperkt de schaal aan 250000 records, gesorteerd op meest recente eerst. 

#### <a name="resolution"></a>Oplossing

U kunt gebruikmaken van [API's van Azure AD Reporting](concept-reporting-api.md) om maximaal een miljoen records op te halen.

### <a name="missing-audit-data-for-recent-actions-in-the-azure-portal"></a>Ontbrekende controlegegevens voor recente acties in de Azure-portal

#### <a name="symptoms"></a>Symptomen

Ik heb enkele acties uitgevoerd in de Azure-portal en had verwacht de auditlogboeken voor deze acties te zien op de blade `Activity logs > Audit Logs`, maar ik kan ze niet vinden.

 ![Rapportage](./media/troubleshoot-missing-audit-data/01.png)
 
#### <a name="cause"></a>Oorzaak

Acties worden niet direct weergegeven in de activiteitenlogboeken. In de onderstaande tabel ziet u de latentiewaarden voor activiteitenlogboeken. 

| Rapport | &nbsp; | Latentie (P95) | Latentie (P99) |
|--------|--------|---------------|---------------|
| Directorycontrole | &nbsp; | 2 minuten | 5 minuten |
| Aanmeldingsactiviteit | &nbsp; | 2 minuten | 5 minuten | 

#### <a name="resolution"></a>Oplossing

Wacht 15 minuten tot twee uur en kijk of de acties nu wel worden vermeld in het logboek. Als u de vermeldingen na twee uur nog steeds niet ziet, [maakt u een ondersteuningsticket aan](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en dan gaan we aan de slag met het probleem.

### <a name="missing-logs-for-recent-user-sign-ins-in-the-azure-ad-sign-ins-activity-log"></a>Meld u ontbrekende logboeken voor recente gebruikersaanmeldingen in de activiteit van Azure AD-aanmeldingen

#### <a name="symptoms"></a>Symptomen

Ik heb me onlangs aangemeld bij de Azure-portal en dan zou ik hier eigenlijk vermeldingen voor moeten zien in de logboeken op de blade `Activity logs > Sign-ins`, maar ik kan ze niet vinden.

 ![Rapportage](./media/troubleshoot-missing-audit-data/02.png)
 
#### <a name="cause"></a>Oorzaak

Acties worden niet direct weergegeven in de activiteitenlogboeken. In de onderstaande tabel ziet u de latentiewaarden voor activiteitenlogboeken. 

| Rapport | &nbsp; | Latentie (P95) | Latentie (P99) |
|--------|--------|---------------|---------------|
| Directorycontrole | &nbsp; | 2 minuten | 5 minuten |
| Aanmeldingsactiviteit | &nbsp; | 2 minuten | 5 minuten | 

#### <a name="resolution"></a>Oplossing

Wacht 15 minuten tot twee uur en kijk of de acties nu wel worden vermeld in het logboek. Als u de vermeldingen na twee uur nog steeds niet ziet, [maakt u een ondersteuningsticket aan](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en dan gaan we aan de slag met het probleem.

### <a name="i-cant-view-more-than-30-days-of-report-data-in-the-azure-portal"></a>Ik kan niet meer dan 30 dagen aan rapportgegevens bekijken in de Azure-portal

#### <a name="symptoms"></a>Symptomen

Ik kan niet meer dan 30 dagen aan aanmeldings- en controlegegevens bekijken in de Azure-portal. Hoe komt dat? 

 ![Rapportage](./media/troubleshoot-missing-audit-data/03.png)

#### <a name="cause"></a>Oorzaak

Afhankelijk van uw licentie bewaart Azure Active Directory Actions de activiteitenrapporten gedurende de volgende perioden:

| Rapport           | &nbsp; |  Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| ---              | ----   |  ---           | ---                 | ---                 |
| Directorycontrole  | &nbsp; |   7 dagen     | 30 dagen             | 30 dagen             |
| Aanmeldingsactiviteit | &nbsp; | Niet beschikbaar. U kunt gedurende zeven dagen uw eigen aanmeldingsactiviteit bekijken op de blade met uw individuele gebruikersprofiel. | 30 dagen | 30 dagen             |

Zie [Bewaarbeleid Azure Active Directory-rapporten](reference-reports-data-retention.md) voor meer informatie.  

#### <a name="resolution"></a>Oplossing

U hebt twee opties om de gegevens langer dan 30 dagen te bewaren. U kunt de [API's van de rapportagefunctie van Azure AD](concept-reporting-api.md) gebruiken om de gegevens via programmacode op te halen en op te slaan in een database. Een alternatief is om auditlogboeken te integreren in een SIEM-systeem van derden, zoals Splunk of SumoLogic.

## <a name="next-steps"></a>Volgende stappen

* [Audit-Logboeken, overzicht](concept-audit-logs.md)
* [Overzicht van aanmeldingen](concept-sign-ins.md)
* [Overzicht van riskante gebeurtenissen](concept-risk-events.md)
