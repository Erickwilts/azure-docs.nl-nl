---
title: Logische apps beheren met behulp van Visual Studio - Azure Logic Apps
description: Logic apps en andere Azure-assets met Visual Studio Cloud Explorer beheren
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.custom: mvc
ms.date: 05/07/2019
ms.openlocfilehash: 694ff490d7623b2dff26a61ccae8106a276b84af
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447904"
---
# <a name="manage-logic-apps-with-visual-studio"></a>Logische apps beheren met Visual Studio

Hoewel u maken kunt, bewerken, beheren en implementeren van logische apps in de [Azure-portal](https://portal.azure.com), u kunt ook Visual Studio gebruiken als u toevoegen van uw logische apps wilt voor het besturingselement voor de gegevensbron, verschillende versies publiceren en maken [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen voor verschillende implementatieomgevingen. U kunt met Visual Studio Cloud Explorer, zoeken en beheren van uw logische apps, samen met andere Azure-resources. Bijvoorbeeld, kunt u openen, downloaden, bewerken, uitvoeren, uitvoeringsgeschiedenis, uitschakelen en inschakelen logische apps die al zijn geïmplementeerd in Azure portal weergeven. Als u geen ervaring met het werken met Azure Logic Apps in Visual Studio, ontdek [over het maken van logische apps met Visual Studio](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

> [!IMPORTANT]
> Implementeren of een logische app vanuit Visual Studio publiceren overschrijft de versie van de app in Azure portal. Dus als u wijzigingen in de Azure-portal die u wilt behouden aanbrengt, zorg ervoor dat u [vernieuwen van de logische app in Visual Studio](#refresh) vanuit de Azure-portal voordat u de volgende keer dat u implementeert of publiceren vanuit Visual Studio.

<a name="requirements"></a>

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Download en installeer deze hulpprogramma's als u ze nog niet hebt: 

  * [Met Visual Studio 2019, 2017 of 2015 - Community edition of hoger](https://aka.ms/download-visual-studio). 
  Deze snelstart maakt gebruik van Visual Studio Community 2017, dit is gratis.

    > [!IMPORTANT]
    > Wanneer u Visual Studio 2019 of 2017 installeert, zorg ervoor dat u selecteert de **Azure-ontwikkeling** werkbelasting.
    > Zie voor meer informatie, [beheren van resources die zijn gekoppeld aan uw Azure-accounts in Visual Studio Cloud Explorer](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view).

    Cloud Explorer voor Visual Studio 2015 installeren [Cloud Explorer van Visual Studio Marketplace downloaden](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015). 
    Zie voor meer informatie, [beheren van resources die zijn gekoppeld aan uw Azure-Accounts in de Cloud Explorer van Visual Studio (2015)](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-resources-managing-with-cloud-explorer?view=vs-2015).

  * [Azure SDK (2.9.1 of hoger)](https://azure.microsoft.com/downloads/) 

  * [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)

  * Azure Logic Apps-hulpprogramma's voor de versie van Visual Studio die u wilt:

    * [Visual Studio 2019](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2019)

    * [Visual Studio 2017](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2017)

    * [Visual Studio 2015](https://aka.ms/download-azure-logic-apps-tools-visual-studio-2015)

    U kunt hulpprogramma's van Azure Logic Apps ofwel rechtstreeks vanuit Visual Studio Marketplace downloaden en installeren, of leer [deze extensie te installeren vanuit Visual Studio](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions). 
    Zorg ervoor dat u Visual Studio opnieuw opstart na de installatie.

* Toegang tot het web tijdens het gebruik van de ingesloten ontwerper van logische Apps

  De ontwerpfunctie moet over een internetverbinding beschikken om resources te maken in Azure en de eigenschappen en gegevens van connectoren in uw logische app te lezen. 
  Als u bijvoorbeeld de Dynamics CRM Online-connector gebruikt, controleert de ontwerpfunctie uw CRM-exemplaar voor de beschikbare standaardregels en aangepaste eigenschappen.

<a name="find-logic-apps-vs"></a>

## <a name="find-your-logic-apps"></a>Uw logische apps zoeken

In Visual Studio vindt u alle logic-apps die zijn gekoppeld aan uw Azure-abonnement en worden geïmplementeerd in Azure portal met behulp van Cloud Explorer.

1. Open Visual Studio. Op de **weergave** in het menu **Cloud Explorer**.

1. Kies in de Cloud Explorer, **accountbeheer**. Selecteer het Azure-abonnement dat is gekoppeld aan uw logische apps, en kies vervolgens **toepassen**. Bijvoorbeeld:

   ![Kies "Accountbeheer"](./media/manage-logic-apps-with-visual-studio/account-management-select-Azure-subscription.png)

1. Op basis van of u nu zoekt door **resourcegroepen** of **resourcetypen**, als volgt te werk:

   * **Resourcegroepen**: Uw Azure-abonnement ziet u Cloud Explorer alle resourcegroepen die gekoppeld aan dat abonnement zijn. 
   Vouw de resourcegroep die uw logische app bevat, en selecteer vervolgens uw logische app.

   * **Resourcetypen**: Vouw onder uw Azure-abonnement, **Logic Apps**. Nadat u Cloud Explorer ziet alle geïmplementeerde logic-apps die gekoppeld aan uw abonnement zijn, selecteert u uw logische app.

<a name="open-designer"></a>

## <a name="open-in-visual-studio"></a>Openen in Visual Studio

U kunt logische apps eerder hebt gemaakt en geïmplementeerd via Azure portal of Azure Resource Manager-projecten met Visual Studio openen in Visual Studio.

1. Cloud Explorer openen en zoeken van uw logische app. 

1. Selecteer in het snelmenu van de logische app, **openen met Logic App-Editor**.

   > [!TIP]
   > Als u deze opdracht geen in Visual Studio 2019, Controleer of de meest recente updates voor Visual Studio.

   In dit voorbeeld ziet u logische apps per resourcetype, zodat uw logische apps worden weergegeven onder de **Logic Apps** sectie.

   ![Open geïmplementeerde logische app in Azure portal](./media/manage-logic-apps-with-visual-studio/open-logic-app-in-editor.png)

   Nadat de logische app wordt geopend in de ontwerper van logische Apps, aan de onderkant van het ontwerpprogramma, kunt u kiezen **codeweergave** zodat u de onderliggende definitie structuur van de logische app kunt controleren. 
   Als u maken van een sjabloon voor de implementatie voor de logische app wilt, krijgt u informatie [het downloaden van een Azure Resource Manager-sjabloon](#download-logic-app) voor deze logische app. Meer informatie over [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment).

<a name="download-logic-app"></a>

## <a name="download-from-azure"></a>Downloaden van Azure

U kunt logische apps downloaden de [Azure-portal](https://portal.azure.com) en slaat u ze als [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sjablonen. U kunt vervolgens lokaal bewerken de sjablonen met Visual Studio en aanpassen van logische apps voor verschillende implementatieomgevingen. Automatisch downloaden van logische apps *parameterizes* hun definities in [Resource Manager-sjablonen](../azure-resource-manager/resource-group-overview.md#template-deployment), die ook JavaScript Object Notation (JSON) gebruiken.

1. In Visual Studio Cloud Explorer openen en zoek en selecteer de logische app die u wilt downloaden van Azure.

2. Selecteer in het snelmenu van die app, **openen met Logic App-Editor**.

   > [!TIP]
   > Als u deze opdracht geen in Visual Studio 2019, Controleer of de meest recente updates voor Visual Studio.

   Logic App Designer wordt geopend en ziet u de logische app. 
   Als u wilt controleren van de onderliggende definitie van logische app en de structuur, aan de onderkant van het ontwerpprogramma, kies **codeweergave**. 

3. Kies op de werkbalk van de ontwerper **downloaden**.

   ![Kies 'Downloaden'](./media/manage-logic-apps-with-visual-studio/download-logic-app.png)

4. Wanneer u wordt gevraagd een locatie, blader naar die locatie en het Resource Manager-sjabloon voor de definitie van de logische app opslaan in JSON (.json)-bestandsindeling. 

De definitie van uw logische app wordt weergegeven in de `resources` subsectie in het Resource Manager-sjabloon. U kunt nu de definitie van de logische app en Resource Manager-sjabloon met Visual Studio bewerken. U kunt ook de sjabloon als een Azure Resource Manager-project toevoegen aan een Visual Studio-oplossing. Meer informatie over [Resource Manager voor logische apps in Visual Studio-projecten](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md). 

<a name="refresh"></a>

## <a name="refresh-from-azure"></a>Vernieuwen van Azure

Als u uw logische app in Azure portal bewerken en deze wijzigingen wilt behouden, zorg ervoor dat de versie van de app in Visual Studio met deze wijzigingen wordt vernieuwd. 

* In Visual Studio op de werkbalk Logic App Designer kiezen **vernieuwen**.

  -of-

* Open het snelmenu van uw logische app in Visual Studio Cloud Explorer en selecteer **vernieuwen**.

![Vernieuwen van de logische app met updates](./media/manage-logic-apps-with-visual-studio/refresh-logic-app.png)

## <a name="publish-logic-app-updates"></a>Logic app-updates publiceren

Wanneer u klaar bent voor uw logische app-updates implementeren vanuit Visual Studio naar Azure, op de werkbalk Logic App Designer Kies **publiceren**.

![Bijgewerkte logische app publiceren](./media/manage-logic-apps-with-visual-studio/publish-logic-app.png)

## <a name="manually-run-your-logic-app"></a>Uw logische app uitvoeren handmatig

U kunt een logische app geïmplementeerd in Azure vanuit Visual Studio handmatig activeren. Kies op de werkbalk Logic App Designer **Trigger uitvoeren**.

![Logische app handmatig uitvoeren](./media/manage-logic-apps-with-visual-studio/manually-run-logic-app.png)

## <a name="review-run-history"></a>Uitvoeringsgeschiedenis controleren

Als u wilt de status controleren en diagnosticeren van problemen met runs voor logic Apps, kunt u de details bekijken, zoals invoer en uitvoer voor degenen die wordt uitgevoerd in Visual Studio.

1. In Cloud Explorer het snelmenu van uw logische app openen en selecteer **Open uitvoeringsgeschiedenis**.

   ![Open de geschiedenis uitvoeren](./media/manage-logic-apps-with-visual-studio/view-run-history.png)

1. Als u wilt weergeven van de details voor een specifieke uitvoering, dubbelklikt u op een uitvoering. Bijvoorbeeld:

   ![Gedetailleerde uitvoeringsgeschiedenis](./media/manage-logic-apps-with-visual-studio/view-run-history-details.png)
  
   > [!TIP]
   > Als u wilt sorteren in de tabel door de eigenschap, kiest u de kolomkop voor die eigenschap. 

1. Vouw de stappen die invoer en uitvoer die u wilt bekijken. Bijvoorbeeld:

   ![Bekijk invoer en uitvoer voor elke stap](./media/manage-logic-apps-with-visual-studio/run-inputs-outputs.png)

## <a name="disable-or-enable-logic-app"></a>Logische app in- of uitschakelen

Zonder uw logische app te verwijderen, kunt u de trigger uit het activeren van de volgende keer wanneer de triggervoorwaarde wordt voldaan stoppen. Uw logische app uitschakelen wordt voorkomen dat de Logic Apps-engine van het maken en toekomstige werkstroomexemplaren voor uw logische app uitvoeren.
In Cloud Explorer het snelmenu van uw logische app openen en selecteer **uitschakelen**.

![Uw logische app uitschakelen](./media/manage-logic-apps-with-visual-studio/disable-logic-app.png)

> [!NOTE]
> Wanneer u een logische app uitschakelt, wordt geen nieuwe uitvoeringen worden geïnstantieerd. Alle wordt uitgevoerd en actieve uitvoeringen wordt voortgezet totdat ze zijn voltooid, wat tijd in beslag kan duren. 

Wanneer u gereed voor de logische app hervatten bent, kunt u uw logische app opnieuw activeren. In Cloud Explorer het snelmenu van uw logische app openen en selecteer **inschakelen**.

![Uw logische app inschakelen](./media/manage-logic-apps-with-visual-studio/enable-logic-app.png)

## <a name="delete-your-logic-app"></a>Uw logische app verwijderen

Als u wilt uw logische app verwijderen uit de Azure portal, in Cloud Explorer het snelmenu van uw logische app openen en selecteer **verwijderen**.

![Uw logische app verwijderen](./media/manage-logic-apps-with-visual-studio/delete-logic-app.png)

> [!NOTE]
> Wanneer u een logische app verwijdert, worden geen nieuwe uitvoeringen gemaakt. Alle uitvoeringen die bezig zijn en wachten op uitvoering worden geannuleerd. Als u duizenden uitvoeringen hebt, kan de annulering een aanzienlijke tijd in beslag nemen. 

## <a name="troubleshooting"></a>Problemen oplossen

Als u uw logische app-project in de ontwerper van logische Apps opent, krijgt u mogelijk niet de optie voor het selecteren van uw Azure-abonnement. In plaats daarvan uw logische app wordt geopend met een Azure-abonnement dat is niet de versie die u wilt gebruiken. Dit probleem treedt op omdat nadat u een logische app .json-bestand hebt geopend, Visual Studio de eerste geselecteerde abonnement voor toekomstig gebruik in de cache opgeslagen. U lost dit probleem, probeert u een van deze stappen:

* Wijzig de naam van de logische app .json-bestand. De abonnement-cache, is afhankelijk van de bestandsnaam.

* Verwijderen van de eerder geselecteerde abonnementen voor *alle* logische apps in uw oplossing, verwijdert u de verborgen Visual Studio-instellingenmap (.vs) in de directory van uw oplossing. Deze locatie opgeslagen gegevens van uw abonnement.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u voor het beheren van geïmplementeerde logische apps met Visual Studio. Vervolgens kunt u meer over het aanpassen van definities voor logische Apps voor implementatie:

> [!div class="nextstepaction"]
> [Definities voor logische Apps maken in JSON](../logic-apps/logic-apps-author-definitions.md)
