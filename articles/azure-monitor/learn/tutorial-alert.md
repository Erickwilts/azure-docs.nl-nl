---
title: Waarschuwingen verzenden vanuit Azure Application Insights | Microsoft Docs
description: Zelfstudie over het verzenden van waarschuwingen als reactie op fouten in uw toepassing met behulp van Azure Application Insights.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: tutorial
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/10/2019
ms.custom: mvc
ms.openlocfilehash: 370d86ae28e49bba9681c6bdc81cc05b4e12a97b
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2019
ms.locfileid: "72894859"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Toepassingsstatus bewaken en waarschuwingen erover verzenden met Azure Application Insights

Met Azure Application Insights kunt u uw toepassing bewaken en waarschuwingen ontvangen wanneer deze niet beschikbaar is, wanneer er fouten optreden, of wanneer er sprake is van prestatieproblemen.  In deze zelf studie doorloopt u het proces van het maken van tests om de beschik baarheid van uw toepassing continu te controleren.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Beschikbaarheidstest maken voor het continu controleren van de reacties van de toepassing
> * E-mail verzenden naar beheerders wanneer zich een probleem voordoet

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

Een [Application Insights-resource](https://docs.microsoft.com/azure/azure-monitor/learn/dotnetcore-quick-start#enable-application-insights)maken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-availability-test"></a>Beschikbaarheidstest maken

Met beschikbaarheidstests in Application Insights kunt u uw toepassing automatisch testen vanuit verschillende locaties over de hele wereld.   In deze zelf studie voert u een URL-test uit om ervoor te zorgen dat uw webtoepassing beschikbaar is.  U kunt ook een volledig overzicht maken om de werking van de toepassing gedetailleerd te testen. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  

2. Selecteer **Beschik baarheid** onder het menu **onderzoeken** en klik vervolgens op **test maken**.

    ![Beschikbaarheidstest toevoegen](media/tutorial-alert/add-test-001.png)

3. Typ een naam voor de test en laat de andere standaardinstellingen ongewijzigd.  Met deze selectie worden aanvragen voor de toepassings-URL elke 5 minuten van vijf verschillende geografische locaties geactiveerd.

4. Selecteer **waarschuwingen** om de vervolg keuzelijst **waarschuwingen** te openen. hier kunt u details opgeven voor de reactie op het mislukken van de test. Kies **bijna realtime** en stel de status in op **ingeschakeld.**

    Typ een e-mailadres waarnaar een melding moet worden verzonden, wanneer wordt voldaan aan de waarschuwingscriteria.  U kunt optioneel ook het adres van een webhook typen dat moet worden aangeroepen wanneer wordt voldaan aan de waarschuwingscriteria.

    ![Test maken](media/tutorial-alert/create-test-001.png)

5. Ga terug naar het test paneel, selecteer de weglatings tekens en de waarschuwing bewerken om de configuratie voor uw nabije realtime-waarschuwing in te voeren.

    ![Waarschuwing bewerken](media/tutorial-alert/edit-alert-001.png)

6. Stel mislukte locaties in op een waarde die groter is dan of gelijk is aan 3. Maak een [actie groep](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) om te configureren wie een melding krijgt wanneer uw waarschuwings drempel wordt geschonden.

    ![Waarschuwing in gebruikers interface opslaan](media/tutorial-alert/save-alert-001.png)

7. Wanneer u uw waarschuwing hebt geconfigureerd, klikt u op de naam van de test om de details van elke locatie weer te geven. Testen kunnen worden weer gegeven in zowel de lijn grafiek als de indeling van het spreidings diagram om de geslaagde/mislukte pogingen voor een bepaald tijds bereik te visualiseren.

    ![Testdetails](media/tutorial-alert/test-details-001.png)

8. U kunt inzoomen op de details van een test door te klikken op de punt in het spreidings diagram. Hiermee wordt de weer gave end-to-end-transactie Details geopend. In het onderstaande voorbeeld ziet u de details van een mislukte aanvraag.

    ![Testresultaat](media/tutorial-alert/test-result-001.png)
  
## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u waarschuwingen voor problemen kunt genereren, gaat u verder met de volgende zelfstudie waarin u leert hoe u hoe kunt analyseren hoe uw gebruikers de toepassing gebruiken.

> [!div class="nextstepaction"]
> [Gebruikers begrijpen](../../azure-monitor/learn/tutorial-users.md)