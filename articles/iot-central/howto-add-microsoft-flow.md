---
title: Werkstromen bouwen met de Azure IoT Central-connector in Microsoft Flow | Microsoft Docs
description: Gebruik van de IoT Central-connector in Microsoft Flow-trigger werkstromen en maken, ophalen, bijwerken, verwijderen van apparaten en opdrachten uitvoeren in werkstromen.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
manager: hegate
ms.openlocfilehash: e8bc8b8d4e3585ea4c0505f2e36abc6d1da7f8eb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797707"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Werkstromen bouwen met de IoT Central-connector in Microsoft Flow

*In dit onderwerp is van toepassing op builders en beheerders.*

Microsoft Flow gebruiken voor het automatiseren van werkstromen in de vele toepassingen en services die afhankelijk zijn van zakelijke gebruikers. Met behulp van de IoT Central-connector in Microsoft Flow, kunt u werkstromen activeren wanneer een regel wordt geactiveerd in IoT Central. In een werkstroom geactiveerd door IoT Central of een andere toepassing, kunt u de acties in de IoT Central-connector:
- Een apparaat maken
- Apparaatgegevens ophalen
- De eigenschappen en instellingen van een apparaat bijwerken
- Een opdracht uitvoeren op een apparaat
- Een apparaat verwijderen

Bekijk [deze Microsoft Flow-sjablonen](https://aka.ms/iotcentralflowtemplates) die IoT Central verbinden met andere services, zoals mobiele meldingen en Microsoft Teams.

## <a name="prerequisites"></a>Vereisten

- Een betalen per gebruik-toepassing
- Een Microsoft persoonlijke of werk- of schoolaccount gebruiken van Microsoft Flow ([meer informatie over Microsoft Flow plannen](https://aka.ms/microsoftflowplans))
- Een account voor werk of school te gebruiken van de Azure IoT Central-connector

## <a name="trigger-a-workflow"></a>Een werkstroom wordt geactiveerd

Deze sectie leest u hoe u voor het activeren van een mobiele melding ontvangen in de mobiele Flow-app wanneer een regel wordt geactiveerd in IoT Central. U kunt deze volledige werkstroom bouwen in uw IoT Central-app met behulp van de ontwerpfunctie van Microsoft Flow.

1. Begin met [maken van een regel in IoT Central](howto-create-telemetry-rules.md). Nadat u de voorwaarden van de regel hebt opgeslagen, selecteert u de **Microsoft Flow-actie** als een nieuwe actie. Een dialoogvenster geopend voor u het configureren van uw werkstroom. Het IoT Central-gebruikersaccount dat u bent aangemeld bij wordt gebruikt voor aanmelding bij Microsoft Flow.

    ![Maak een nieuwe Microsoft Flow-actie](media/howto-add-microsoft-flow/createflowaction.png)

1. Hier ziet u een lijst met werkstromen die u hebt toegang tot en zijn gekoppeld aan deze regel IoT Central. Klik op **sjablonen verkennen** of **New > maken op basis van sjabloon** en u kunt kiezen uit een van de beschikbare sjablonen. 

    ![Beschikbare sjablonen voor Microsoft Flow](media/howto-add-microsoft-flow/flowtemplates1.png)

1. U wordt gevraagd om aan te melden bij de connectors in de sjabloon die u hebt gekozen. 

    > [!NOTE]
    > Voor het gebruik van de Azure IoT Central-connector, moet u zich aanmelden met een Azure Active Directory-account (werk of school-account). Een persoonlijk account, zoals abc@outlook.com of abc@live.com worden niet ondersteund door de Azure IoT Central-connector.

    Wanneer u bent aangemeld bij de connectors, komt u terecht in de ontwerper om aan te maken van uw werkstroom. De werkstroom heeft een IoT Central activeringsopdracht met uw toepassing en de regel al ingevuld.

1. U kunt de werkstroom aanpassen door aan te passen de gegevens doorgegeven aan de actie en de nieuwe acties toe te voegen. In dit voorbeeld wordt de actie is **meldingen - ik wil een mobiele melding ontvangen**. U kunt opnemen *dynamische inhoud* van uw IoT Central-regel, geven belangrijke informatie zoals de naam van apparaat- en tijdstempel op de melding.

    > [!NOTE]
    > Selecteer de **meer** tekst in het venster met dynamische inhoud om op te halen van de meting en eigenschap waarden die de regel geactiveerd.

    ![Flow actie met deelvenster met dynamische open bewerken](./media/howto-add-microsoft-flow/flowdynamicpane1.png)

1. Wanneer u klaar bent voor het bewerken van de actie selecteren **opslaan**. U wordt omgeleid naar de pagina overzicht van uw werkstroom. Hier ziet u de uitvoeringsgeschiedenis en delen met andere collega's.

    > [!NOTE]
    > Als u wilt dat andere gebruikers in uw IoT Central-app met deze regel bewerken, moet u deze met hen delen in Microsoft Flow. De Microsoft Flow-accounts toevoegen als eigenaren in uw werkstroom.

1. Als u terug naar uw IoT Central-app gaat, ziet u dat deze regel bevat een Microsoft Flow-actie in het gebied van acties.

U kunt ook met behulp van de connector IoT Central rechtstreeks vanuit Microsoft Flow-werkstromen bouwen. Vervolgens kunt u kiezen welke IoT Central-app verbinding kan maken.

## <a name="create-a-device-in-a-workflow"></a>Een apparaat in een werkstroom maken

Deze sectie leest u hoe u een nieuw apparaat in IoT Central maken op de push-bewerking van een knop op een mobiel apparaat via de mobiele Microsoft Flow-app. U kunt deze actie in stroom gebruiken, zoals Dynamics ERP-systemen integreren met IoT Central door het maken van een nieuw apparaat wanneer een apparaat ergens anders wordt toegevoegd.

1. Beginnen met het maken van een lege workflow in Microsoft Flow.

1. Zoeken naar **knop stroom voor mobiel** als een trigger.

1. In deze trigger toevoegen met de naam tekstinvoer **apparaatnaam**. Wijzigen van de beschrijvende tekst om te worden **Voer de naam van het apparaat van het nieuwe apparaat**.

1. Voeg een nieuwe actie toe. Zoek de **Azure IoT Central - maken van een apparaat** actie.

1. Kies uw toepassing en kies een apparaat-sjabloon voor het maken van een apparaat uit in de vervolgkeuzelijsten. Hier ziet u de actie uitvouwen om alle eigenschappen en instellingen van het apparaat weer te geven.

1. Selecteer het veld met de naam van apparaat. Kies in het deelvenster met dynamische inhoud **apparaatnaam**. Deze waarde wordt doorgegeven vanuit de invoer van de gebruiker krijgt via de mobiele app en de naam van het nieuwe apparaat in IoT Central. In dit voorbeeld is de enige vereiste veld de naam van het apparaat, aangegeven door de rode asterisk. Sjabloon voor een ander apparaat mogelijk meerdere vereiste velden die moeten worden ingevuld om een nieuwe apparaat te maken.

    ![Stroom dynamische actiedeelvenster apparaat maken](./media/howto-add-microsoft-flow/flowcreatedevice1.png)

1. (Optioneel) Vul in de andere velden naar wens voor het maken van het nieuwe apparaten.

1. Sla ten slotte uw werkstroom.

1. Probeer uw werkstroom in de mobiele Microsoft Flow-app. Ga naar de **knoppen** tabblad in de app. U ziet de knop maken een nieuwe werkstroom voor apparaten ->. Voer de naam van het nieuwe apparaat en bekijk wordt weergeven in IoT Central.

    ![Stroom maken mobiele apparaat-schermafbeelding](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Een apparaat in een werkstroom bijwerken

Deze sectie leest u hoe u instellingen voor apparaten en de eigenschappen in IoT Central voor de push-bewerking van een knop op een mobiel apparaat via de mobiele Microsoft Flow-app bij te werken.

1. Beginnen met het maken van een lege workflow in Microsoft Flow.

1. Zoeken naar **knop stroom voor mobiel** als een trigger.

1. In deze trigger toevoegen invoer, zoals een **locatie** tekstinvoer die overeenkomt met een instelling of de eigenschap u wilt wijzigen. De beschrijvende tekst wijzigen.

1. Voeg een nieuwe actie toe. Zoek de **Azure IoT Central - een-apparaat bijwerken** actie.

1. Kies uw toepassing in de vervolgkeuzelijst. U moet nu een ID van het bestaande apparaat dat u wilt bijwerken. 

    > [!NOTE] 
    > **Moet u de ID is gevonden in de URL** op het apparaat de detailpagina van het apparaat dat u wilt bijwerken. De apparaat-ID gevonden in de apparatenverkenner lijst met apparaten is niet de juiste is voor gebruik in Microsoft Flow.

    ![IoT Central-ID van de URL](./media/howto-add-microsoft-flow/iotcdeviceidurl.png)

1. U kunt de naam van het apparaat bijwerken. Voor het bijwerken van een van de eigenschappen en instellingen van het apparaat, moet u de sjabloon van het apparaat van het apparaat dat u bijwerken wilt de **apparaat sjabloon** vervolgkeuzelijst. De actie-tegel wordt uitgebreid om weer te geven van alle eigenschappen en instellingen die u kunt bijwerken.

    ![Werkstroom voor apparaat-stroom updates](./media/howto-add-microsoft-flow/flowupdatedevice1.png)

1. Selecteer elk van de eigenschappen en instellingen die u wilt bijwerken. Kies in het deelvenster met dynamische inhoud, de bijbehorende invoer door de trigger. In dit voorbeeld wordt de locatiewaarde omlaag doorgegeven aan het bijwerken van de locatie-eigenschap van het apparaat.

1. Sla ten slotte uw werkstroom.

1. Probeer uw werkstroom in de mobiele Microsoft Flow-app. Ga naar de **knoppen** tabblad in de app. Hier ziet u de Update een werkstroom voor apparaten -> knop. Voer de invoer en zien van het apparaat in IoT Central bijgewerkt!

## <a name="get-device-information-in-a-workflow"></a>Apparaatgegevens in een werkstroom ophalen

Krijgt u informatie over de apparaten met behulp van de ID de **Azure IoT Central - een apparaat laten** actie. 
> [!NOTE] 
> **Moet u de ID is gevonden in de URL** op het apparaat de detailpagina van het apparaat dat u wilt bijwerken. De apparaat-ID gevonden in de apparatenverkenner lijst met apparaten is niet de juiste is voor gebruik in Microsoft Flow.

U krijgt informatie zoals de naam van apparaat, sjabloon de naam van apparaat, eigenschapswaarden en waarden voor de instellingen moeten worden doorgegeven aan latere acties in uw werkstroom. Hier volgt een voorbeeldwerkstroom die de waarde van de naam van de klant-eigenschap van een apparaat doorgeeft aan Microsoft Teams.

   ![Werkstroom voor flow get-apparaten](./media/howto-add-microsoft-flow/flowgetdevice1.png)


## <a name="run-a-command-on-a-device-in-a-workflow"></a>Een opdracht uitvoeren op een apparaat in een werkstroom
U kunt een opdracht uitvoeren op een apparaat dat is opgegeven door de ID met behulp van de **Azure IoT Central - voert u een opdracht** actie. 

> [!NOTE] 
> **Moet u de ID is gevonden in de URL** op het apparaat de detailpagina van het apparaat dat u wilt bijwerken. De apparaat-ID gevonden in de apparatenverkenner lijst met apparaten is niet de juiste is voor gebruik in Microsoft Flow.
    
U kunt de opdracht uit te voeren en de parameters van de opdracht doorgeven door deze actie kunt kiezen. Hier volgt een voorbeeldwerkstroom die de opdracht van een apparaat opnieuw opstarten op een knop in de mobiele Microsoft Flow-app wordt uitgevoerd.

   ![Werkstroom voor flow get-apparaten](./media/howto-add-microsoft-flow/flowrunacommand1.png)

## <a name="delete-a-device-in-a-workflow"></a>Een apparaat in een werkstroom verwijderen

U kunt een apparaat met behulp van de ID verwijderen de **Azure IoT Central - een apparaat verwijderen** actie. 
> [!NOTE] 
> **Moet u de ID is gevonden in de URL** op het apparaat de detailpagina van het apparaat dat u wilt bijwerken. De apparaat-ID gevonden in de apparatenverkenner lijst met apparaten is niet de juiste is voor gebruik in Microsoft Flow.

Hier volgt een voorbeeldwerkstroom die Hiermee verwijdert u een apparaat op de push-bewerking van een knop in de mobiele Microsoft Flow-app.

   ![Werkstroom voor de apparaten van stroom verwijderen](./media/howto-add-microsoft-flow/flowdeletedevice.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen bij het maken van een verbinding met de Azure IoT Central-connector ondervindt, volgen hier enkele tips om u te helpen.

1. Persoonlijke Microsoft-accounts (zoals @hotmail.com, @live.com, @outlook.com domeinen) worden niet ondersteund op dit moment. U moet een Azure Active Directory (AD)-werk- of schoolaccount.

2. Voor het gebruik van de IoT Central-connector in Microsoft Flow, moet u zijn aangemeld bij de IoT Central-toepassing ten minste één keer. Anders wordt wordt niet de toepassing weergegeven in de vervolgkeuzelijsten van toepassing.

3. Als u een fout opgetreden tijdens het gebruik van een Azure AD-account ontvangt, probeert te openen van Windows PowerShell en voer de volgende commandlets als beheerder.

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u kunt Microsoft Flow gebruiken om werkstromen te bouwen, de voorgestelde volgende stap is het [apparaten beheren](howto-manage-devices.md).

