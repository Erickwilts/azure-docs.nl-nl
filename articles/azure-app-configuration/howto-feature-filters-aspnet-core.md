---
title: Functie filters gebruiken om een functie in te scha kelen voor een subset van gebruikers
titleSuffix: Azure App Configuration
description: Meer informatie over het gebruik van functie filters om een functie in te scha kelen voor een subset van gebruikers
ms.service: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: 181c97615985283011834dcf9145810b1563fb4a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2020
ms.locfileid: "80056999"
---
# <a name="use-feature-filters-to-enable-a-feature-for-a-subset-of-users"></a>Functie filters gebruiken om een functie in te scha kelen voor een subset van gebruikers

Met functie vlaggen kunt u de functionaliteit in uw toepassing activeren of deactiveren. Een eenvoudige functie vlag is in-of uitgeschakeld. De toepassing gedraagt zich altijd op dezelfde manier. U kunt bijvoorbeeld een nieuwe functie achter een functie vlag implementeren. Wanneer de functie vlag is ingeschakeld, zien alle gebruikers de nieuwe functie. Als u de functie vlag uitschakelt, wordt de nieuwe functie verborgen.

Als u daarentegen een _vlag voor voorwaardelijke functies_ gebruikt, kan de functie vlag dynamisch worden in-of uitgeschakeld. De toepassing kan zich anders gedragen, afhankelijk van de criteria van de functie vlag. Stel dat u de nieuwe functie wilt weer geven in een kleine subset van gebruikers op het eerst. Met een vlag voor voorwaardelijke functies kunt u de functie vlag voor sommige gebruikers inschakelen en uitschakelen voor anderen. _Functie filters_ bepalen de status van de functie vlag elke keer dat deze wordt geëvalueerd.

De `Microsoft.FeatureManagement` bibliotheek bevat twee functie filters:

- `PercentageFilter`Hiermee schakelt u de functie vlag in op basis van een percentage.
- `TimeWindowFilter`Hiermee schakelt u de functie vlag in gedurende een opgegeven periode.

U kunt ook uw eigen functie filter maken waarmee de [Interface micro soft. FeatureManagement. IFeatureFilter wordt](/dotnet/api/microsoft.featuremanagement.ifeaturefilter)geïmplementeerd.

## <a name="registering-a-feature-filter"></a>Een functie filter registreren

U registreert een functie filter door de `AddFeatureFilter` methode aan te roepen en de naam van het functie filter op te geven. De volgende code wordt bijvoorbeeld geregistreerd `PercentageFilter` :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddFeatureManagement().AddFeatureFilter<PercentageFilter>();
}
```

## <a name="configuring-a-feature-filter-in-azure-app-configuration"></a>Een functie filter configureren in Azure-app configuratie

Sommige functie filters hebben aanvullende instellingen. U kunt bijvoorbeeld `PercentageFilter` een functie activeren op basis van een percentage. Het heeft een instelling die het te gebruiken percentage definieert.

U kunt deze instellingen configureren voor functie vlaggen die zijn gedefinieerd in Azure-app configuratie. Voer de volgende stappen uit om `PercentageFilter` de functie vlag in te scha kelen voor 50% van aanvragen voor een web-app:

1. Volg de instructies in [Quick Start: Voeg functie vlaggen toe aan een ASP.net core-app](./quickstart-feature-flag-aspnet-core.md) om een web-app te maken met een functie vlag.

1. Ga in het Azure Portal naar uw configuratie archief en klik op **functie beheer**.

1. Klik op het context menu voor de functie vlag van de *bèta versie* die u in de Quick Start hebt gemaakt. Klik op **Bewerken**.

    > [!div class="mx-imgBorder"]
    > ![Bèta-functie vlag bewerken](./media/edit-beta-feature-flag.png)

1. Selecteer in het **bewerkings** scherm **het keuze** rondje als dit nog niet is geselecteerd. Klik vervolgens op de knop **filter toevoegen** . (Het label **op** de radio knop wordt gewijzigd in **voorwaardelijke**tekst lezen.)

1. Voer *micro soft. percentage*in het veld **sleutel** in.

    > [!div class="mx-imgBorder"]
    > ![Functie filter toevoegen](./media/feature-flag-add-filter.png)

1. Klik op het context menu naast de functie filter sleutel. Klik op **para meters bewerken**.

    > [!div class="mx-imgBorder"]
    > ![Para meters voor functie filter bewerken](./media/feature-flag-edit-filter-parameters.png)

1. Beweeg de muis aanwijzer onder de **naam** van de koptekst zodat er tekst vakken worden weer gegeven in het raster. Voer een **naam** in voor de *waarde* en de **waarde** 50. Het veld **waarde** geeft het percentage aanvragen aan waarvoor het functie filter moet worden ingeschakeld.

    > [!div class="mx-imgBorder"]
    > ![Para meters voor functie filter instellen](./media/feature-flag-set-filter-parameters.png)

1. Klik op **Toep assen** om terug te gaan naar het scherm **functie vlag bewerken** . Klik vervolgens nogmaals op **Toep assen** om de instellingen voor de functie vlag op te slaan.

1. De **status** van de functie vlag wordt nu weer gegeven als *voorwaardelijk*. Deze status geeft aan dat de functie vlag per aanvraag wordt in-of uitgeschakeld, op basis van de criteria die worden afgedwongen door het functie filter.

    > [!div class="mx-imgBorder"]
    > ![Vlag voor voorwaardelijke functie](./media/feature-flag-filter-enabled.png)

## <a name="feature-filters-in-action"></a>Functie filters in actie

Als u de effecten van deze functie vlag wilt zien, start u de toepassing en klikt u op de knop **vernieuwen** in uw browser meerdere keren. U ziet dat het *bèta* -item ongeveer 50% van de tijd op de werk balk wordt weer gegeven. Het is de rest van de tijd verborgen, omdat de `PercentageFilter` *bèta* functie wordt gedeactiveerd voor een subset van aanvragen. In de volgende video ziet u dit gedrag in actie.

> [!div class="mx-imgBorder"]
> ![PercentageFilter in actie](./media/feature-flags-percentagefilter.gif)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht van functie beheer](./concept-feature-management.md)
