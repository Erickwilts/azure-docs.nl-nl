---
title: IT Service Management-connector-beveiligd exporteren in Azure Monitor
description: In dit artikel wordt beschreven hoe u uw ITSM-producten of-Services verbindt met beveiligde export in Azure Monitor om ITSM-werk items centraal te controleren en te beheren.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 39f277fffbb9a76a4be4bfa8aaedeaf3479a989f
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2020
ms.locfileid: "90068880"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Verbinding maken tussen Azure en ITSM-hulpprogram ma's met behulp van beveiligde export

In dit artikel wordt beschreven hoe u de verbinding tussen het ITSM-product of de service van uw IT-Service beheer (met behulp van beveiligde export) kunt configureren.

Secure export is een bijgewerkte versie van [IT Service Management-connector (ITSMC)](./itsmc-overview.md). Met beide versies kunt u werk items maken in een ITSM-hulp programma wanneer Azure Monitor waarschuwingen verzendt. De functionaliteit omvat metrische, logboek-en activiteiten logboek waarschuwingen.

ITSMC maakt gebruik van gebruikers naam en wacht woord. Beveiligd exporteren heeft sterkere verificatie omdat deze gebruikmaakt van Azure Active Directory (Azure AD). Azure AD is de cloud-gebaseerde service voor identiteits-en toegangs beheer van micro soft. Het helpt gebruikers zich aan te melden en toegang te krijgen tot interne of externe resources. Met Azure AD met ITSM kunt u Azure-waarschuwingen identificeren (via de Azure AD-toepassings-ID) die naar het externe systeem zijn verzonden.

> [!NOTE]
> De mogelijkheid om Azure te verbinden met ITSM-hulpprogram ma's met behulp van beveiligde export is in preview.

## <a name="secure-export-architecture"></a>Beveiligde export architectuur

De Secure export-architectuur introduceert de volgende nieuwe mogelijkheden:

* **Nieuwe actie groep**: waarschuwingen worden verzonden naar het ITSM-hulp programma via de actie groep van de beveiligde webhook, in plaats van de ITSM actie groep die ITSMC gebruikt.
* **Azure AD-verificatie**: de verificatie vindt plaats via Azure AD in plaats van de referenties van de gebruikers naam en het wacht woord.

## <a name="secure-export-data-flow"></a>Gegevens stroom voor beveiligde export

De stappen voor de beveiligde export gegevens stroom zijn:

1. Azure Monitor verzendt een waarschuwing die is geconfigureerd voor het gebruik van beveiligde export.
1. De nettolading van de waarschuwing wordt door een beveiligde webhook-actie verzonden naar het ITSM-hulp programma.
1. De ITSM-toepassing controleert met Azure AD als de waarschuwing is gemachtigd om het ITSM-hulp programma in te voeren.
1. Als de waarschuwing is geautoriseerd, wordt de toepassing:
   
   1. Hiermee maakt u een werk item (bijvoorbeeld een incident) in het ITSM-hulp programma.
   1. Koppelt de ID van het configuratie-item (CI) aan de klant beheer database (CMDB).

![Diagram dat laat zien hoe het ITSM-hulp programma communiceert met Azure A D, Azure-waarschuwingen en een actie groep.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="connection-with-bmc-helix"></a>Verbinding met BMC Helix

Secure export ondersteunt BMC helix. Enkele voor delen van de integratie zijn:

* **Betere verificatie**: Azure AD biedt een veiligere verificatie zonder de time-outs die zich vaak voordoen in ITSMC.
* **Waarschuwingen die zijn opgelost in het ITSM-hulp programma**: metrische waarschuwingen implementeren ' fireed ' en ' opgeloste ' statussen. Wanneer aan de voor waarde wordt voldaan, wordt de status van de waarschuwing geactiveerd. Als niet meer aan de voor waarde wordt voldaan, is de waarschuwings status opgelost. In ITSMC kunnen waarschuwingen niet automatisch worden opgelost. Met beveiligde export stromen de omgezette status naar het ITSM-hulp programma en worden deze automatisch bijgewerkt.
* **[Gebruikelijk waarschuwings schema](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema)**: in ITSMC verschilt het schema van de nettolading van de waarschuwing op basis van het waarschuwings type. Bij beveiligde export is er een gemeen schappelijk schema voor alle waarschuwings typen. Dit algemene schema bevat de CI voor alle waarschuwings typen. Alle waarschuwings typen kunnen hun CI binden met de CMDB.

Ga met de volgende stappen aan de slag met het ITSM-connector-hulp programma:

1. Uw app bij Azure AD registreren.
2. Maak een beveiligde webhook-actie groep.
3. Configureer uw partner omgeving.

## <a name="register-with-azure-active-directory"></a>Registreren bij Azure Active Directory

Volg deze stappen om de toepassing te registreren bij Azure AD:

1. Volg de stappen in [een toepassing registreren bij het micro soft Identity-platform](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).
1. Selecteer **toepassing zichtbaar**maken in azure AD.
1. Selecteer **instellen** voor de URI van de **toepassings-id**.

   [![Scherm afbeelding van de optie voor het instellen van de U R I van de toepassing die ik D.](media/it-service-management-connector-secure-webhook-connections/azure-ad.png)](media/it-service-management-connector-secure-webhook-connections/azure-ad-expand.png#lightbox)
1. Selecteer **Opslaan**.

## <a name="create-a-secure-webhook-action-group"></a>Een actie groep voor een beveiligde webhook maken

Nadat uw toepassing is geregistreerd bij Azure AD, kunt u werk items maken in uw ITSM-hulp programma op basis van Azure-waarschuwingen met behulp van de actie beveiligde webhook in actie groepen.

Actie groepen bieden een modulaire en herbruikbare manier om acties voor Azure-waarschuwingen te activeren. U kunt actie groepen met metrische waarschuwingen, waarschuwingen voor activiteiten logboeken en waarschuwingen voor Azure Log Analytics gebruiken in de Azure Portal.
Zie voor meer informatie over actie groepen [actie groepen maken en beheren in de Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

Gebruik de volgende procedure in de BMC Helix-omgeving:

1. Meld u aan bij Integration Studio.
1. Zoek naar de stroom **incident maken vanuit Azure Alerts** .
1. Kopieer de webhook-URL.
   
   ![Scherm afbeelding van de webhook U R L in Integration Studio.](media/it-service-management-connector-secure-webhook-connections/bmc-url.png)

Volg deze instructies voor beveiligde webhook om een webhook aan een actie toe te voegen:

1. Zoek in het [Azure Portal](https://portal.azure.com/)naar en selecteer **monitor**. In het deel venster **monitor** worden al uw bewakings instellingen en-gegevens in één weer gave geconsolideerd.
1. Selecteer **waarschuwingen**  >  **acties beheren**.
1. Selecteer [actie groep toevoegen](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal)en vul de velden in.
1. Voer een naam in het vak Naam van de **actie groep** in en voer een naam in het vak **korte naam** in. De korte naam wordt gebruikt in plaats van een volledige naam van de actiegroep als er meldingen via deze groep worden verzonden.
1. Selecteer **beveiligde webhook**.
1. Selecteer deze details:
   1. Selecteer de object-ID van de Azure Active Directory instantie die u hebt geregistreerd.
   1. Plak voor de URI in de webhook-URL die u hebt gekopieerd uit de BMC Helix-omgeving.
   1. Stel **het algemene waarschuwings schema** in op **Ja**. 

   In de volgende afbeelding ziet u de configuratie van een voor beeld van een beveiligde webhook-actie:

   ![Scherm afbeelding met een beveiligde webhook-actie.](media/it-service-management-connector-secure-webhook-connections/secure-webhook.png)

## <a name="configure-the-partner-environment"></a>De partner omgeving configureren

### <a name="connect-bmc-helix-to-azure-monitor"></a>BMC Helix verbinden met Azure Monitor

De volgende secties bevatten informatie over hoe u verbinding kunt maken met uw BMC Helix-product en hoe u de export in azure kunt beveiligen.

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet:

* Azure AD is geregistreerd.
* U hebt de ondersteunde versie van BMC Helix multi-Cloud Service Management (versie 20,02 of hoger).

### <a name="configure-the-bmc-helix-connection"></a>De BMC Helix-verbinding configureren

1. Volg de instructies in het [inschakelen van vooraf gebouwde integratie met Azure monitor voor versie 20,02](https://docs.bmc.com/docs/multicloud/enabling-prebuilt-integration-with-azure-monitor-879728195.html).

1. Als onderdeel van de configuratie van de verbinding in BMC Helix gaat u naar uw integratie-BMC-exemplaar en volgt u deze instructies:

   1. Selecteer **Catalog**.
   1. Selecteer **Azure-waarschuwingen**.
   1. Selecteer **connectors**.
   1. Selecteer **configuratie**.
   1. Selecteer de configuratie **nieuwe verbinding toevoegen** .
   1. Vul de informatie in voor de configuratie sectie:
      - **Name**: Maak uw eigen naam.
      - **Autorisatie type**: **geen**
      - **Beschrijving**: Maak uw eigen naam.
      - **Site**: **Cloud**
      - **Aantal instanties**: **2**, de standaard waarde.
      - **Controle**: standaard ingeschakeld om gebruik in te scha kelen.
      - De Azure-Tenant-ID en de Azure-toepassings-ID zijn afkomstig uit de toepassing die u eerder hebt gedefinieerd.

![Scherm opname van de BMC-configuratie.](media/it-service-management-connector-secure-webhook-connections/bmc-configuration.png)

## <a name="next-steps"></a>Volgende stappen

* [ITSM-werk items maken op basis van Azure-waarschuwingen](./itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
