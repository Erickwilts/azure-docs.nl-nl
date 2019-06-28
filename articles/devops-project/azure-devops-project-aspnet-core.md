---
title: 'Quickstart: Een CI/CD-pijplijn voor .NET maken met Azure DevOps Projects'
description: Azure DevOps Projects zorgt ervoor dat u eenvoudig aan de slag kunt met Azure. Hiermee kunt u een .NET-app voor een Azure-service van uw keuze starten in slechts enkele stappen.
ms.prod: devops
ms.technology: devops-cicd
services: azure-devops-project
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 387801f2ecb2f5fa1639005726218efb54d75dc8
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331423"
---
# <a name="create-a-cicd-pipeline-for-net-with-azure-devops-projects"></a>Een CI/CD-pijplijn voor .NET maken met Azure DevOps Projects

Configureer continue integratie (CI) en continue levering (CD) voor uw .NET Core- of ASP.NET-toepassing met Azure DevOps Projects. Azure DevOps Projects vereenvoudigt de eerste configuratie van een build- en release-pijplijn in Azure Pipelines.

Als u geen Azure-abonnement hebt, kunt u er gratis een krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

DevOps Projects maakt een CI/CD-pijplijn in Azure DevOps. U kunt een nieuwe Azure DevOps-organisatie maken of een bestaande organisatie gebruiken. Met DevOps Projects worden ook Azure-resources gemaakt in het Azure-abonnement van uw keuze.

1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).

1. Kies in het linkerdeelvenster het pictogram **Een resource maken** in de linkernavigatiebalk en zoek naar **DevOps Projects**.  

3.  Selecteer **Maken**.

    ![Continue levering starten](_img/azure-devops-project-aspnet-core/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Een voorbeeldtoepassing en Azure-service selecteren

1. Selecteer de **.NET**-voorbeeldtoepassing. Bij de .NET-voorbeelden kunt u kiezen uit het open source-framework ASP.NET of het platformoverschrijdende .NET Core-framework.

   ![.NET Framework](_img/azure-devops-project-aspnet-core/chooselanguagedotnet.png)

2. Dit voorbeeld gebruikt een ASP.NET Core MVC-toepassing. Selecteer de **.NET Core** toepassingsframework, selecteer vervolgens **volgende**.    
    
3. Selecteer **Web-App voor Windows** als het implementatiedoel van een, selecteert u vervolgens **volgende**. U kunt eventueel andere Azure-services voor uw implementatie. Het toepassingsframework, die u eerder hebt gekozen, bepaalt het type van de Azure-service-implementatiedoel beschikbaar hier.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps en een Azure-abonnement configureren 

1. Voer een **projectnaam**.

2. Maak een nieuwe gratis **Azure DevOps-organisatie** of kies een bestaande organisatie in de vervolgkeuzelijst.

3. Selecteer uw **Azure-abonnement**, voer een naam in voor uw **Web-app** of de standaard nemen en selecteer vervolgens **gedaan**. Na een paar minuten, wordt de implementatie-overzicht van DevOps-projecten in Azure portal weergegeven. 

4. Selecteer **naar de resource gaan** om het DevOps-Project-dashboard weer te geven. In de rechterbovenhoek vastmaken de **Project** aan uw dashboard voor snelle toegang. Een voorbeeld-app is ingesteld in een opslagplaats in uw **Azure DevOps-organisatie**. Een build wordt uitgevoerd en uw app is geïmplementeerd in Azure.

5. Het dashboard biedt meer inzicht in uw codeopslagplaats, CI/CD-pijplijn en app in Azure. Selecteer aan de rechterkant onder Azure-resources, **Bladeren** om weer te geven van uw app die wordt uitgevoerd.

   ![Dashboardweergave](_img/azure-devops-project-aspnet-core/dashboardnopreview.png) 

## <a name="commit-code-changes-and-execute-cicd"></a>Codewijzigingen doorvoeren en CI/CD uitvoeren

Met DevOps Projects is een Git-opslagplaats gemaakt in Azure Repos of in GitHub. Ga als volgt te werk om de opslagplaats weer te geven en codewijzigingen aan de brengen in de toepassing:

1. Selecteer aan de linkerkant van het DevOps Projects-dashboard de koppeling voor uw **master** branch. Met deze koppeling opent u een weergave in de zojuist gemaakte Git-opslagplaats.

2. In de volgende stappen kunt u de webbrowser gebruiken om te maken en het doorvoeren van wijzigingen in de code rechtstreeks naar de **master** vertakking. U kunt ook de Git-opslagplaats klonen in uw favoriete IDE hiervoor **kloon** vanaf de bovenkant van de opslagplaats-pagina. 

3. Ga aan de linkerkant, de structuur van toepassing op **Application/aspnet-core-dotnet-core/Pages/Index.cshtml**.

4. Selecteer **Bewerken** en breng vervolgens een wijziging aan in de h2-kop. Typ bijvoorbeeld **Meteen aan de slag met Azure DevOps Projects** of maak een andere wijziging.

      ![Code-bewerkingen](_img/azure-devops-project-aspnet-core/codechange.png)

5. Selecteer **doorvoeren**, laat dan een reactie en selecteert u **doorvoeren** opnieuw.

6. Ga in uw browser naar het Azure DevOps-projectdashboard.  Als het goed is, ziet u nu dat er een build wordt gemaakt. De aangebrachte wijzigingen worden automatisch gebouwd en geïmplementeerd via een CI/CD-pijplijn.

## <a name="examine-the-cicd-pipeline"></a>De CI/CD-pijplijn onderzoeken

In de vorige stap werd in Azure DevOps Projects automatisch een volledige CI/CD-pijplijn geconfigureerd. U kunt de pijplijn verkennen en zo nodig aanpassen. Ga als volgt te werk om vertrouwd te raken met de build- en release-pijplijnen van Azure DevOps.

1. Selecteer boven in het DevOps Projects-dashboard de optie **Build-pijplijnen**.  
Met deze koppeling worden een browsertabblad en de Azure DevOps build-pijplijn van Azure DevOps voor het nieuwe project geopend.

1. Selecteer het beletselteken (...).  Met deze actie wordt een menu geopend waarin u diverse acties kunt starten, zoals een nieuwe build in de wachtrij plaatsen, een build onderbreken en de build-pijplijn bewerken.

1. Selecteer **Bewerken**.

    ![Build-pipeline](_img/azure-devops-project-aspnet-core/builddef.png)

1. In dit deelvenster kunt u de verschillende taken voor uw build-pijplijn onderzoeken.  
 De build voert verschillende taken uit, zoals het ophalen van bronnen uit de Git-opslagplaats, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer bovenaan de build-pijplijn de naam van de build-pijplijn.

1. Wijzig de naam van de build-pijplijn in een gebruiksvriendelijkere naam. Selecteer **Opslaan en wachtrij** en selecteer **Opslaan**.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**.   
In het deelvenster **Geschiedenis** ziet u een audittrail van recente wijzigingen voor de build.  Azure Pipelines houdt alle wijzigingen in de build-pijplijn bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  
In DevOps Projects is automatisch een CI-trigger gemaakt en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  
Afhankelijk van het scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

1. Selecteer **Build en release** en selecteer vervolgens **Releases**.  
In DevOps Projects wordt een release-pijplijn gemaakt om implementaties in Azure te beheren.

1.  Selecteer aan de linkerkant het beletselteken (...) naast de release-pijplijn en selecteer **Bewerken**.  
De release-pijplijn bevat een pijplijn die het releaseproces definieert.  

1. Onder **Artefacten** selecteert u **Neerzetten**.  De build-pijplijn die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer naast het pictogram **Neerzetten** de optie **Continue implementatietrigger**.  
Deze release-pijplijn heeft een ingeschakelde CD-trigger op basis waarvan een implementatie wordt uitgevoerd telkens wanneer een nieuw build-artefact beschikbaar is. U kunt de trigger eventueel uitschakelen zodat de implementaties handmatig moeten worden uitgevoerd.  

1. Selecteer aan de linkerkant **Taken**.   
De taken zijn de acties die tijdens het implementatieproces worden uitgevoerd. In dit voorbeeld is een taak gemaakt om te implementeren in Azure App Service.

1. Selecteer aan de rechterkant **Releases weergeven**. In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het beletselteken (...) naast een van de releases en selecteer vervolgens **Openen**.  
U kunt verschillende menu's verkennen, zoals een releaseoverzicht, gekoppelde werkitems en tests.


1. Selecteer **Doorvoeringen**.   
In deze weergave worden de codedoorvoeringen weergegeven die zijn gekoppeld aan deze implementatie. 

1. Selecteer **Logboeken**.  
De logboeken bevatten nuttige informatie over het implementatieproces. Ze kunnen worden weergegeven tijdens en na de implementaties.


## <a name="clean-up-resources"></a>Resources opschonen

U kunt Azure App Service en gerelateerde resources die u hebt gemaakt, verwijderen wanneer u ze niet meer nodig hebt. Gebruik de functionaliteit **Verwijderen** op het DevOps Projects-dashboard.

## <a name="next-steps"></a>Volgende stappen

Zie deze zelfstudie voor meer informatie over het wijzigen van de build- en release-pipelines in overeenstemming met de behoeften van uw team:

> [!div class="nextstepaction"]
> [CD-proces aanpassen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Video's

> [!VIDEO https://www.youtube.com/embed/itwqMf9aR0w]
