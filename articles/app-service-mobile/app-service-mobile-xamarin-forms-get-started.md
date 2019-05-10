---
title: Aan de slag met Mobile Apps met behulp van Xamarin.Forms
description: Volg deze zelfstudie om aan de slag te gaan met Mobile Apps voor Xamarin.Forms-ontwikkeling
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: b99513cad34bba1b050a24795ecb21d0357d19c1
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416079"
---
# <a name="create-a-xamarinforms-app-with-azure"></a>Een Xamarin.Forms-app maken met Azure

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Xamarin.Forms-app door de functie Mobile Apps van Azure App Service als de back-end te gebruiken. U maakt zowel een nieuwe back-end voor Mobile Apps als een eenvoudige Xamarin.Forms-app voor takenlijsten die app-gegevens opslaat in Azure.

Het voltooien van deze zelfstudie is een vereiste voor alle andere zelfstudies over Mobile Apps voor Xamarin.Forms.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u zich aanmelden voor een proefversie van Azure en maximaal tien gratis mobiele apps krijgen die u ook na de proefperiode kunt blijven gebruiken. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

* Visual Studio Tools voor Xamarin, in Visual Studio 2017 of hoger, of Visual Studio voor Mac. Zie de [Xamarin-installatiepagina][Install Xamarin] voor instructies.

* (optioneel) Als u een iOS-app wilt maken, is een Mac met Xcode 9.0 of hoger vereist. Visual Studio voor Mac kan worden gebruikt voor het ontwikkelen van iOS-apps, of Visual Studio 2017 of later kan worden gebruikt (zolang de Mac beschikbaar op het netwerk is).

## <a name="create-a-new-mobile-apps-back-end"></a>Een nieuwe back-end voor Mobile Apps maken

Doe het volgende om een nieuwe back-end voor Mobile Apps te maken:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een back-end voor Mobile Apps ingesteld die kan worden gebruikt door uw mobiele toepassingen. Nu gaat u een serverproject downloaden voor een eenvoudige back-end voor takenlijsten en deze vervolgens publiceren naar Azure.

## <a name="configure-the-server-project"></a>Het serverproject configureren

Doe het volgende om het serverproject te configureren voor het gebruik van de Node.js- of .NET-back-end:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>De Xamarin.Forms-oplossing downloaden en uitvoeren

De Visual Studio-hulpprogramma's voor Xamarin zijn vereist voor het openen van de oplossing. Zie de [Xamarin-installatie-instructies][Install Xamarin]. Als de hulpprogramma's al zijn geïnstalleerd, voert u de volgende stappen uit om de oplossing te downloaden en te openen:

### <a name="visual-studio"></a>Visual Studio

1. Ga naar de [Azure Portal].

2. In de blade Instellingen voor uw mobiele app klikt u op **Quickstart** (onder Implementatie) > **Xamarin.Forms**. Klik in stap 3 op  **Een nieuwe app maken** als deze optie nog niet is geselecteerd.  Klik vervolgens op de knop **Downloaden**.

   Er wordt nu een project gedownload dat een clienttoepassing bevat die is verbonden met uw mobiele app. Sla het gecomprimeerde projectbestand op uw lokale computer op en noteer de opslaglocatie.

3. Pak het project dat u hebt gedownload uit en opent u het in Visual Studio.

   ![Uitgepakt project in Visual Studio][8]

4. Volg de onderstaande instructies om de Android- of Windows-projecten uit te voeren en het iOS-project als er een Mac-netwerkcomputer beschikbaar is.

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Ga naar de [Azure Portal].

2. In de blade Instellingen voor uw mobiele app klikt u op **Quickstart** (onder Implementatie) > **Xamarin.Forms**. Klik in stap 3 op  **Een nieuwe app maken** als deze optie nog niet is geselecteerd.  Klik vervolgens op de knop **Downloaden**.

   Er wordt nu een project gedownload dat een clienttoepassing bevat die is verbonden met uw mobiele app. Sla het gecomprimeerde projectbestand op uw lokale computer op en noteer de opslaglocatie.

3. Pak het project uit dat u hebt gedownload en open het in Visual Studio voor Mac.

   ![Uitgepakt project in Visual Studio voor Mac][9]

4. Volg de onderstaande instructies om de Android- of iOS-projecten uit te voeren.



## <a name="optional-run-the-android-project"></a>(Optioneel) Het Android-project uitvoeren

In deze sectie voert u het Xamarin.Android-project uit. Als u niet met Android-apparaten werkt, kunt u deze sectie overslaan.

### <a name="visual-studio"></a>Visual Studio

1. Klik met de rechtermuisknop op het Android-project (Droid) en selecteer vervolgens **Instellen als opstartproject**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in naast het Android-project en zorg ervoor dat het vak **Bouwen** is ingeschakeld voor het gedeelde codeproject.

4. Druk op **F5** of klik op de knop **Start** om het project te maken en de app te starten in een Android-emulator.

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Klik met de rechtermuisknop op het Android-project en selecteer vervolgens **Set As Startup Project**.

2. Selecteer **Start Debugging** in het menu **Run** om het project te maken en de app te starten in een Android-emulator.



Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

![Taak-app van Android][11]

Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.

> [!NOTE]
> U vindt de code die toegang geeft tot de back-end voor Mobile Apps, in het C#-bestand **TodoItemManager.cs** van het gedeelde codeproject in de oplossing.
>

## <a name="optional-run-the-ios-project"></a>(Optioneel) Het iOS-project uitvoeren

In deze sectie voert u het Xamarin.iOS-project uit voor iOS-apparaten. Als u niet met iOS-apparaten werkt, kunt u deze sectie overslaan.

### <a name="visual-studio"></a>Visual Studio

1. Klik met de rechtermuisknop op het iOS-project en selecteer vervolgens **Set as StartUp Project**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in naast het iOS-project en zorg ervoor dat het vak **Bouwen** is ingeschakeld voor het gedeelde codeproject.

4. Selecteer de toets **F5** om het project te bouwen en de app te starten in de iPhone-emulator.

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Klik met de rechtermuisknop op het iOS-project en selecteer vervolgens **Set As Startup Project**.

2. Selecteer **Start Debugging** in het menu **Run** om het project te bouwen en de app te starten in de iPhone-emulator.



Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

![Taak-app voor iOS][10]

Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.

> [!NOTE]
> U vindt de code die toegang geeft tot de back-end voor Mobile Apps, in het C#-bestand **TodoItemManager.cs** van het gedeelde codeproject in de oplossing.
>

## <a name="optional-run-the-windows-project"></a>(Optioneel) Het Windows-project uitvoeren

In deze sectie voert u het UWP-project Xamarin.Forms (Universal Windows Platform) uit voor Windows-apparaten. Als u niet met Windows-apparaten werkt, kunt u deze sectie overslaan.

### <a name="visual-studio"></a>Visual Studio

1. Klik met de rechtermuisknop op het UWP-project en selecteer vervolgens **Set as StartUp Project**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in naast het gekozen Windows-project en zorg ervoor dat het vak **Bouwen** is ingeschakeld voor het gedeelde codeproject.

4. Druk op **F5** of klik op de knop **Start** om het project te maken en de app te starten in een Windows-emulator (waar **lokale machine** moet worden weergegeven).

> [!NOTE]
> Het Windows-project kan niet worden uitgevoerd op macOS.



Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.

![Taak-app voor UWP][12]

> [!NOTE]
> U vindt de code die toegang geeft tot de back-end voor Mobile Apps, in het C#-bestand **TodoItemManager.cs** van het portable class library-project van uw oplossing.
>

## <a name="troubleshooting"></a>Probleemoplossing

Als u er problemen zijn met het bouwen van de oplossing, voert u NuGet Package Manager uit om bij te werken naar de nieuwste versie van `Xamarin.Forms` en werkt u in het Android-project de `Xamarin.Android`-ondersteuningspakketten bij. Quick Start-projecten bevatten mogelijk niet altijd de nieuwste versies.

Alle ondersteuningspakketten waarnaar wordt verwezen in uw Android-project, moeten dezelfde versie hebben. Het [Azure Mobile Apps NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) heeft `Xamarin.Android.Support.CustomTabs`-afhankelijkheid voor het Android-platform, dus als uw project gebruikmaakt van nieuwere ondersteuningspakketten, moet u dit pakket met de vereiste versie rechtstreeks installeren om conflicten te voorkomen.

## <a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md) Lees hoe u gebruikers van uw app verifieert met een id-provider.

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md) Lees hoe u ondersteuning voor pushmeldingen toevoegt aan uw app en de back-end voor Mobile Apps configureert voor het gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.

* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md) Lees hoe u offlineondersteuning voor uw app toevoegt met behulp van een back-end voor Mobile Apps. Offline synchroniseren zorgt ervoor dat u gegevens van een mobiele app kunt weergeven, toevoegen of wijzigen, zelfs als er geen netwerkverbinding is.

* [De beheerde client gebruiken voor Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md) Lees hoe u werkt met de SDK voor beheerde clients in uw Xamarin-app.

* [Andere Azure-services gebruiken met Xamarin.Forms](https://docs.microsoft.com/xamarin/xamarin-forms/data-cloud/) Lees hoe u extra Azure-functies zoals zoeken, opslag en cognitieve services toevoegt aan Xamarin.Forms-apps.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png

<!-- URLs. -->
[Install Xamarin]: https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/
[Mobile app SDK]: https://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/
