---
title: Update installeren op het apparaat Azure Data Box Gateway serie | Microsoft Docs
description: Hierin wordt beschreven hoe u updates toepast met de Azure Portal en de lokale web-UI voor het Azure Data Box Gateway Series-apparaat
services: databox
author: twooley
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: twooley
ms.openlocfilehash: 3972aecf70a664980e8279152eb001915ae1bb53
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2020
ms.locfileid: "88783872"
---
# <a name="update-your-azure-data-box-gateway"></a>Uw Azure Data Box Gateway bijwerken

In dit artikel worden de stappen beschreven die nodig zijn voor het installeren van de update op uw Azure Data Box Gateway via de lokale webgebruikersinterface en via de Azure Portal. U past de software-updates of hotfixes toe om uw Data Box Gateway-apparaat up-to-date te houden.

> [!IMPORTANT]
>
> - Update **1911** komt overeen met de **1.6.1049.786** -software versie op het apparaat. Ga naar [release opmerkingen](data-box-gateway-1911-release-notes.md)voor meer informatie over deze update.
>
> - Als u een update of hotfix installeert, wordt het apparaat opnieuw opgestart. Gezien de Data Box Gateway een apparaat met één knoop punt is, wordt elke I/O-bewerking onderbroken en wordt het apparaat een downtime van Maxi maal 30 minuten voor de software-update van het apparaat.

Deze stappen worden afzonderlijk beschreven in de volgende gedeelten.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

U wordt aangeraden updates te installeren via de Azure Portal. Op het apparaat wordt eenmaal per dag automatisch gescand op updates. Zodra de updates beschikbaar zijn, ziet u een melding in de portal. U kunt vervolgens de updates downloaden en installeren.

> [!NOTE]
> Zorg ervoor dat het apparaat in orde is en dat de status wordt weer gegeven als **online** voordat u doorgaat met de installatie van de updates.

1. Wanneer de updates beschikbaar zijn voor uw apparaat, ziet u een melding. Selecteer de melding of van de bovenste opdracht balk, **werk het apparaat**bij. Hiermee kunt u software-updates voor apparaten Toep assen.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. Controleer op de Blade **apparaat bijwerken** of u de licentie voorwaarden hebt gelezen die zijn gekoppeld aan nieuwe functies in de release opmerkingen.

    U kunt ervoor kiezen om de updates te **downloaden en te installeren** of alleen de updates te **downloaden** . Vervolgens kunt u ervoor kiezen om deze updates later te installeren.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Als u de updates wilt downloaden en installeren, controleert u de optie dat updates automatisch worden geïnstalleerd nadat het downloaden is voltooid.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Het downloaden van updates wordt gestart. U ziet een melding dat de down load wordt uitgevoerd.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    Er wordt ook een meldings banner weer gegeven in de Azure Portal. Dit geeft de voortgang van de down load aan.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    U kunt deze melding selecteren of het **apparaat bijwerken** selecteren om de gedetailleerde status van de update te bekijken.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. Nadat het downloaden is voltooid, wordt de meldings banner bijgewerkt om aan te geven dat de voltooiing is voltooid. Als u ervoor hebt gekozen om de updates te downloaden en te installeren, wordt de installatie automatisch gestart.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Als u ervoor hebt gekozen om alleen updates te downloaden, selecteert u de melding om de Blade updates van het **apparaat** te openen. Selecteer **Installeren**.
  
    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. U ziet een melding dat de installatie wordt uitgevoerd.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    In de portal wordt ook een informatieve waarschuwing weer gegeven om aan te geven dat de installatie wordt uitgevoerd. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Aangezien dit een apparaat met één knoop punt is, wordt het apparaat opnieuw opgestart nadat de updates zijn geïnstalleerd. De kritieke waarschuwing tijdens het opnieuw opstarten geeft aan dat de heartbeat van het apparaat verloren is gegaan.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    Selecteer de waarschuwing om de bijbehorende gebeurtenis te bekijken.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. De status van het apparaat wordt **online** bijgewerkt nadat de updates zijn geïnstalleerd.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    Selecteer op de bovenste opdracht balk de optie updates van het **apparaat**. Controleer of de update is geïnstalleerd en of de versie van de apparaatsoftware overeenkomt met die.

    ![Software versie na update](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>De lokale web-UI gebruiken

Er zijn twee stappen voor het gebruik van de lokale web-UI:

- De update of hotfix downloaden
- De update of hotfix installeren

Deze stappen worden afzonderlijk in detail beschreven in de volgende gedeelten.

### <a name="download-the-update-or-the-hotfix"></a>De update of hotfix downloaden

Voer de volgende stappen uit om de update te downloaden. U kunt de update downloaden van de door micro soft geleverde locatie of vanuit de Microsoft Update catalogus.

Voer de volgende stappen uit om de update te downloaden uit de Microsoft Update catalogus.

1. Start de browser en ga naar [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

   ![Catalogus doorzoeken](./media/data-box-gateway-apply-updates/download-update-1.png)

2. Voer in het zoekvak van de catalogus van Microsoft Update het Knowledge Base-nummer (KB) in van de hotfix of de voor waarden voor de update die u wilt downloaden. Voer bijvoorbeeld **Azure data Box gateway**in en klik vervolgens op **zoeken**.

   De update vermelding wordt weer gegeven als **Azure Data Box Gateway 1911**.

   ![Catalogus doorzoeken](./media/data-box-gateway-apply-updates/download-update-2.png)

3. Selecteer **Downloaden**. Er is één bestand dat kan worden gedownload *SoftwareUpdatePackage.exe* dat overeenkomt met de software-update van het apparaat. Down load het bestand naar een map op het lokale systeem. U kunt de map ook kopiëren naar een netwerk share die bereikbaar is vanaf het apparaat.

   ![Catalogus doorzoeken](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>De update of hotfix installeren

Voordat u de update of hotfix installeert, moet u het volgende doen:

- U beschikt over de update of de hotfix is lokaal op uw host gedownload of toegankelijk via een netwerk share.
- De apparaatstatus is in orde, zoals wordt weer gegeven op de pagina **overzicht** van de lokale webgebruikersinterface.

   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Het volt ooien van deze procedure duurt ongeveer 20 minuten. Voer de volgende stappen uit om de update of hotfix te installeren.

1. Ga in de lokale web-UI naar **onderhouds**  >  **Software-update**. Noteer de software versie die u uitvoert.

   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Geef het pad op naar het update bestand. U kunt ook naar het installatie bestand van de update bladeren als u deze op een netwerk share plaatst. Selecteer het software-update bestand met *SoftwareUpdatePackage.exe* achtervoegsel.

   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. Selecteer **Toepassen**.

   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. Als u om bevestiging wordt gevraagd, selecteert u **Ja** om door te gaan. Gezien het apparaat is een apparaat met één knoop punt, nadat de update is toegepast, wordt het apparaat opnieuw opgestart en is er uitval tijd.
   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. De update wordt gestart. Nadat het apparaat is bijgewerkt, wordt het opnieuw opgestart. De lokale gebruikers interface is niet toegankelijk in deze duur.

6. Wanneer het opnieuw opstarten is voltooid, wordt u naar de **aanmeldings** pagina geleid. Als u wilt controleren of de software van het apparaat is bijgewerkt, gaat u naar **onderhouds**  >  **Software-update**in de lokale web-UI. De weer gegeven software versie in dit voor beeld is **1.6.1049.786**.

   ![apparaat bijwerken](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [het beheren van uw Azure data Box gateway](data-box-gateway-manage-users.md).
