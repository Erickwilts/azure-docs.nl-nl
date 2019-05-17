---
title: Web-app-analyse voor ASP.NET instellen met Azure Application Insights | Microsoft Docs
description: Configureer prestaties, beschikbaarheid en gebruiker gedrag analysehulpprogramma's voor uw ASP.NET-website, on-premises gehost of in Azure.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbullwin
ms.openlocfilehash: c69e925913d4dcb3a8bec34cbf25dac184ea65c5
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595551"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Application Insights instellen voor uw ASP.NET-website

Met deze procedure wordt uw ASP.NET web-app zo geconfigureerd dat telemetrie wordt verzonden naar de [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)-service. Dit werkt voor ASP.NET-apps die worden gehost op uw eigen on-premises IIS-server of in de cloud. U beschikt over grafieken en een krachtige querytaal waarmee u meer inzicht krijgt in de prestaties van uw app en hoe mensen deze gebruiken. Daarnaast ontvangt u meldingen over fouten of prestatieproblemen. Veel ontwikkelaars gebruiken deze functies graag, maar u kunt de telemetrie desgewenst uitbreiden en aanpassen.

Voor de Setup zijn maar een paar klikken nodig in Visual Studio. U kunt kosten besparen door de hoeveelheid telemetrie te beperken. Deze functie kunt u experimenteren en fouten opsporen of een site met weinig gebruikers bewaken. Wanneer u besluit dat u de productiesite wilt bewaken, kunt u de limiet later eenvoudig verhogen.

## <a name="prerequisites"></a>Vereisten
Als u Application Insights wilt toevoegen aan uw ASP.NET-website, moet u het volgende doen:

- Installeer [2019 van Visual Studio voor Windows](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET- en webontwikkelworkload (de optionele componenten niet uitschakelen)
    - Azure-ontwikkeling

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="ide"></a> Stap 1: De Application Insights SDK toevoegen

> [!IMPORTANT]
> De schermafbeeldingen in dit voorbeeld worden op basis van Visual Studio 2017 versie 15.9.9 en hoger. De ervaring om toe te voegen van Application Insights is afhankelijk van andere versies van Visual Studio en per type ASP.NET-sjabloon. Oudere versies mogelijk alternatieve tekst, zoals 'Configureren van Application Insights'.

Met de rechtermuisknop op de naam van uw web-app in Solution Explorer en kies **toevoegen** > **Application Insights Telemetry**

![Schermopname van Solution Explorer waarin Application Insights is gemarkeerd](./media/asp-net/add-telemetry-new.png)

(Afhankelijk van de Application Insights-SDK-versie wordt u mogelijk gevraagd om een upgrade uit te voeren naar de meest recente SDK-versie. Als u hierom wordt gevraagd, selecteert u **SDK bijwerken**.)

![Schermafbeelding: Er is een nieuwe versie van de Microsoft Application Insights-SDK beschikbaar. SDK bijwerken is gemarkeerd](./media/asp-net/0002-update-sdk.png)

Application Insights-configuratiescherm:

Selecteer **aan de slag**.

![Schermopname van de pagina Uw app registreren bij Application Insights](./media/asp-net/00004-start-free.png)

Als u een resourcegroep of locatie wilt instellen voor het opslaan van uw gegevens, klikt u op **Instellingen configureren**. Resourcegroepen worden gebruikt om toegang tot gegevens te beheren. Als u verschillende apps hebt die deel uitmaken van hetzelfde systeem, kunt u hun Application Insights-gegevens in dezelfde resourcegroep plaatsen.

 Selecteer **Registreren**.

![Schermopname van de pagina Uw app registreren bij Application Insights](./media/asp-net/00005-register-ed.png)

 Er wordt telemetrie verzonden naar [Azure Portal](https://portal.azure.com), zowel tijdens de foutopsporing als na het publiceren van de app.
> [!NOTE]
> Als u geen telemetrie naar de portal wilt verzenden tijdens de foutopsporing, voegt u de Application Insights SDK toe aan de app, maar configureert u geen resource in de portal. U kunt de telemetrie in Visual Studio bekijken tijdens de foutopsporing. U kunt later terugkeren naar deze configuratiepagina, of u kunt wachten tot de app is geïmplementeerd en [telemetrie inschakelen tijdens runtime](../../azure-monitor/app/monitor-performance-live-website-now.md).

## <a name="run"></a> Stap 2: Uw app uitvoeren
Voer uw app uit met F5. Open verschillende pagina's om telemetrie te genereren.

In Visual Studio ziet u het aantal gebeurtenissen dat is vastgelegd in een logboek.

![Schermopname van Visual Studio. Tijdens het opsporen van fouten wordt de knop Application Insights weergegeven.](./media/asp-net/00006-Events.png)

## <a name="step-3-see-your-telemetry"></a>Stap 3: Uw telemetrie weergeven
U ziet uw telemetrie in Visual Studio of in de Application Insights-webportal. Zoek telemetrie in Visual Studio om fouten in de app op te sporen. Bewaak de prestaties en het gebruik in de webportal wanneer het systeem live is. 

### <a name="see-your-telemetry-in-visual-studio"></a>De telemetrie bekijken in Visual Studio

In Visual Studio om de Application Insights-gegevens te bekijken.  Selecteer **Solution Explorer** > **Verbonden services** > klik met de rechtermuisknop op **Application Insights**, en klik vervolgens op **Zoeken in live telemetrie**.

In het zoekvenster van Visual Studio Application Insights ziet u de telemetriegegevens van de toepassing die zijn gegenereerd aan de serverzijde van de app. Experimenteer met de filters en klik op een gebeurtenis voor meer details.

![Schermopname van de weergave Gegevens van foutopsporingssessie in het venster Application Insights.](./media/asp-net/55.png)

> [!Tip]
> Als u geen gegevens ziet, controleert u of het tijdsbereik correct is en klikt u op het pictogram Zoeken.

[Meer informatie over Application Insights-hulpprogramma's in Visual Studio](../../azure-monitor/app/visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Telemetrie bekijken in de webportal

U kunt de telemetrie ook in de Application Insights-webportal bekijken (tenzij u ervoor hebt gekozen alleen de SDK te installeren). De portal bevat meer grafieken, analysefuncties en weergaven met meerdere onderdelen dan Visual Studio. De portal bevat ook waarschuwingen.

Open uw Application Insights-resource. Meld u aan bij [Azure Portal](https://portal.azure.com/) om de telemetrie te bekijken, of selecteer hiervoor **Solution Explorer** > **Verbonden services** > klik met de rechtermuisknop op **Application Insights** > **Application Insights-portal openen**.

De portal wordt geopend met een weergave van de telemetrie van uw app.

![Schermopname van Application Insights-overzichtspagina](./media/asp-net/007.png)

Klik in de portal op een tegel of grafiek om meer details te bekijken.

[Meer informatie over het gebruik van Application Insights in Azure Portal](../../azure-monitor/app/app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Stap 4: Uw app publiceren
Publiceer uw app op de IIS-server of op Azure. Bekijk de livestream met metrische gegevens in [Live Metrics Stream](../../azure-monitor/app/metrics-explorer.md#live-metrics-stream) om te controleren of alles goed werkt.

Uw telemetrie wordt opgebouwd in de Application Insights-portal, waar u metrische gegevens kunt controleren, uw telemetrie kunt doorzoeken en [dashboards](../../azure-monitor/app/app-insights-dashboards.md) kunt instellen. U kunt ook de krachtige [Kusto-querytaal](/azure/kusto/query/) om gebruik en prestaties te analyseren of om specifieke gebeurtenissen te zoeken.

U kunt uw telemetrie ook blijven analyseren in [Visual Studio](../../azure-monitor/app/visual-studio.md) met hulpprogramma's voor diagnostisch zoeken en [Trends](../../azure-monitor/app/visual-studio-trends.md).

> [!NOTE]
> Als uw app zoveel telemetrie verzendt dat de[beperkingslimieten](../../azure-monitor/app/pricing.md#limits-summary) worden benaderd, worden automatisch [steekproeven](../../azure-monitor/app/sampling.md) ingeschakeld. Met steekproeven vermindert u de hoeveelheid telemetrie die vanuit uw app wordt verzonden, maar behoudt u gecorreleerde gegevens voor diagnostische doeleinden.
>
>

## <a name="land"></a> U bent nu klaar

Gefeliciteerd! U hebt het pakket Application Insights geïnstalleerd in uw app en dit zo geconfigureerd dat telemetrie wordt verzonden naar de Application Insights-service in Azure.

De Azure-resource die de telemetrie van uw app ontvangt, wordt aangeduid met een *instrumentatiesleutel*. U vindt deze sleutel in het bestand ApplicationInsights.config.


## <a name="upgrade-to-future-sdk-versions"></a>Upgraden naar toekomstige SDK-versies
Als u wilt upgraden naar een [nieuwe release van de SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), opent u **NuGet-pakketbeheer** opnieuw en filtert u op geïnstalleerde pakketten. Selecteer **Microsoft.ApplicationInsights.Web** en kies **Upgraden**.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg de wijzigingen vervolgens samen in de nieuwe versie.

## <a name="video"></a>Video

* Externe stapsgewijze video over [Application Insights configureren met een .NET-toepassing helemaal](https://www.youtube.com/watch?v=blnGAVgMAfA).

## <a name="next-steps"></a>Volgende stappen

Er zijn ook andere onderwerpen die u kunt bekijken als u geïnteresseerd bent in:

* [Een web-app instrumenteren tijdens runtime](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md)

### <a name="more-telemetry"></a>Meer telemetrie

* **[Laadgegevens voor browser en pagina](../../azure-monitor/app/javascript.md)**: voeg een codefragment in uw webpagina's in.
* **[Gedetailleerde bewaking van afhankelijkheid en uitzonderingen](../../azure-monitor/app/monitor-performance-live-website-now.md)**: installeer Status Monitor op uw server.
* **[Codeer aangepaste gebeurtenissen](../../azure-monitor/app/api-custom-events-metrics.md)** om gebruikersacties te tellen, te meten of de tijdsduur hiervan te bepalen.
* **[Haal logboekgegevens op](../../azure-monitor/app/asp-net-trace-logs.md)**: koppel logboekgegevens aan uw telemetrie.

### <a name="analysis"></a>Analyse

* **[Met Application Insights werken in Visual Studio](../../azure-monitor/app/visual-studio.md)**<br/>Bevat informatie over foutopsporing met telemetrie, het doorzoeken van diagnostische gegevens en het in detail analyseren van code.
* **[Werken met de Application Insights-portal](../../azure-monitor/app/app-insights-dashboards.md)**<br/> Bevat informatie over dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie.
* **[Analyse](../../azure-monitor/log-query/get-started-portal.md)**: de krachtige querytaal.

### <a name="alerts"></a>Waarschuwingen

* [Beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md): Maak tests om ervoor te zorgen dat uw site zichtbaar is op het web.
* [Slimme diagnostische gegevens](../../azure-monitor/app/proactive-diagnostics.md): Deze tests worden automatisch uitgevoerd, zodat u hoeft niets te stellen. Deze geeft aan of een app een ongebruikelijk aantal mislukte aanvragen heeft.
* [Metrische waarschuwingen](../../azure-monitor/app/alerts.md): Stel waarschuwingen om u te waarschuwen als een metriek een drempelwaarde overschrijdt. U kunt deze instellen op aangepaste metrische gegevens die u in uw app codeert.

### <a name="automation"></a>Automation

* [Het maken van een Application Insights-resource automatiseren](../../azure-monitor/app/powershell.md)
