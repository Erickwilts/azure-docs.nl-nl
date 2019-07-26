---
title: 'Quickstart: Een Azure Search-index maken in Java'
description: Hierin wordt uitgelegd hoe u een index maakt, gegevens laadt en query's uitvoert met behulp van Java en de Azure Search REST-Api's.
services: search
author: jj09
manager: jlembicz
ms.service: search
ms.topic: conceptual
ms.date: 08/26/2018
ms.author: jjed
ms.custom: seodec2018, seo-java-july2019
ms.openlocfilehash: 7172cd01ca881ec3027854444107b0744b65feb3
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489793"
---
# <a name="quickstart-create-an-azure-search-index-in-java"></a>Quickstart: Een Azure Search-index maken in Java
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Informatie over het bouwen van een aangepaste Java-zoektoepassing die voor de zoekfunctie gebruikmaakt van Azure Search. In deze zelfstudie wordt de [Azure Search Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) gebruikt om de objecten en bewerkingen in deze oefening te bouwen.

Als u dit voorbeeld wilt uitvoeren, moet u over de Azure Search-service beschikken. U kunt zich hiervoor aanmelden in [Azure Portal](https://portal.azure.com). Zie [Een Azure Search-service in de portal maken](search-create-service-portal.md) voor stapsgewijze instructies.

De volgende software is gebruik om deze sample te maken en te testen:

* [Eclipse IDE voor Java EE-ontwikkelaars](https://www.eclipse.org/downloads/packages/release/photon/r/eclipse-ide-java-ee-developers). Zorg ervoor dat u de EE-versie downloadt. Voor een van de verificatiestappen is een functie vereist die alleen in deze versie beschikbaar is.
* [JDK 8u181](https://aka.ms/azure-jdks)
* [Apache Tomcat 8.5.33](https://tomcat.apache.org/download-80.cgi#8.5.33)

## <a name="about-the-data"></a>Over de gegevens
In deze voorbeeldtoepassing wordt gebruikgemaakt van gegevens van [United States Geological Services (USGS)](https://geonames.usgs.gov/domestic/download_data.htm), gefilterd op de staat Rhode Island om de grootte van de gegevensset te reduceren. We gebruiken deze gegevens om een zoektoepassing te bouwen die kenmerkende gebouwen, zoals ziekenhuizen of scholen, gebouwen zoals ziekenhuizen en scholen, maar ook geologische kenmerken, zoals stromen, meren en toppen, retourneert.

In deze toepassing bouwt en laadt het programma **SearchServlet. java** de index met behulp van een [Indexeer functie](https://msdn.microsoft.com/library/azure/dn798918.aspx) constructie, waarbij de gefilterde USGS-gegevensset wordt opgehaald uit een Azure SQL database. De programmacode bevat de vooraf gedefinieerde referenties en gegevens voor verbinding met de onlinegegevensbron. Voor de toegang tot de gegevens hoeft u verder niets te configureren.

> [!NOTE]
> Er is een filter op de gegevensset toegepast om onder de limiet van 10.000 documenten voor de gratis prijscategorie te blijven. Als u de standaardcategorie gebruikt, is de limiet niet van toepassing en kunt u deze code aanpassen om een grotere gegevensset te gebruiken. Zie [Limieten en beperkingen](search-limits-quotas-capacity.md) voor meer informatie over de capaciteit voor elke prijscategorie.
> 
> 

## <a name="about-the-program-files"></a>Over de programmabestanden
In de volgende lijst worden de bestanden beschreven die relevant zijn voor dit voorbeeld.

* Search.jsp: Biedt de gebruikers interface
* SearchServlet.java: Biedt methoden (vergelijkbaar met een controller in MVC)
* SearchServiceClient.java: Verwerkt HTTP-aanvragen
* SearchServiceHelper.java: Een helperklasse die statische methoden biedt
* Document.java: Biedt het gegevens model
* config. Properties: Hiermee stelt u de URL van de zoek service en`api-key`
* Pom. XML: Een Maven-afhankelijkheid

<a id="sub-2"></a>

## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>De service naam en `api-key` van uw Azure Search-service zoeken
Voor alle REST API-aanroepen in Azure Search is vereist dat u de service `api-key`-URL en een. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in de snelbalk op de **Search-service** om alle Azure Search-services weer te geven die zijn ingericht voor uw abonnement.
3. Selecteer de service die u wilt gebruiken.
4. Op het servicedashboard worden tegels weergegeven voor essentiële informatie. Daarnaast wordt het sleutelpictogram voor toegang tot de beheersleutels weergegeven.
   
      ![Scherm afbeelding die laat zien hoe u toegang krijgt tot de beheer sleutels vanuit het service dashboard][3]
5. Kopieer de service-URL en een beheersleutel. Deze hebt u later weer nodig wanneer u ze toevoegt aan het bestand **config.properties**.

## <a name="download-the-sample-files"></a>De voorbeeldbestanden downloaden
1. Ga naar [search-java-indexeerfunctie-demo](https://github.com/Azure-Samples/search-java-indexer-demo) op GitHub.
2. Klik op **Download ZIP** (ZIP downloaden), sla het zip-bestand op naar de schijf en pak vervolgens alle bestanden in het zip-bestand uit. U kunt de bestanden eventueel uitpakken naar uw Java-werkruimte, zodat u het project later eenvoudig kunt terugvinden.
3. De voorbeeldbestanden hebben het kenmerk alleen -lezen Klik met de rechtermuisknop op de mapeigenschappen en verwijder het kenmerk alleen-lezen.

Alle volgende bestandswijzigingen en uitvoerinstructies worden uitgevoerd voor de bestanden in deze map.  

## <a name="import-project"></a>Project importeren
1. Klik in Eclipse achtereenvolgens op **File** >  (Bestand)**Import** (Importeren) > **General** (Algemeen) > **Existing Projects into Workspace** (Bestaand project naar werkruimte).
   
    ![Scherm afbeelding die laat zien hoe u een bestaand project importeert][4]
2. Blader in **Select root directory** (Hoofddirectory selecteren) naar de map met de voorbeeldbestanden. Selecteer de map die de map .project bevat. Het project moet worden weergegeven in de lijst **Projects** (Projecten) als een geselecteerd item.
   
    ![Scherm opname van de lijst met projecten in het venster projecten importeren][12]
3. Klik op **Voltooien**.
4. Gebruik **Projectverkenner** om de bestanden weer te geven en te bewerken. Als Projectverkenner nog niet is geopend, klikt u op **Window** (Venster) > **Show View** (Weergaven tonen) > **Projectverkenner** of gebruik de snelkoppeling om Projectverkenner te openen.

## <a name="configure-the-service-url-and-api-key"></a>Configureer de service-URL en`api-key`
1. Dubbel klik in **project Verkenner**op **config. Properties** om de configuratie-instellingen te bewerken die de server naam `api-key`en bevatten.
2. Raadpleeg de stappen eerder in dit artikel, waar u de URL van de service en `api-key` in de [Azure Portal](https://portal.azure.com)hebt gevonden, om de waarden op te halen die u nu kunt invoeren in **config. Properties**.
3. Vervang "API-sleutel" in **config. Properties**door de `api-key` voor uw service. Vervolgens vervangt de service naam (het eerste onderdeel van de https://servicename.search.windows.net) URL ' service naam ' in hetzelfde bestand.
   
    ![Scherm afbeelding die laat zien hoe de API-sleutel moet worden vervangen][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Configureer de project-, build en runtime-omgeving
1. Ga in Eclipse naar de Projectverkenner en klik met de rechtermuisknop op het project > **Eigenschappen** > **Project-facetten**.
2. Selecteer **Dynamic Web Module** (Dynamische webmodule), **Java** en **JavaScript**.
   
    ![Scherm afbeelding die laat zien hoe u de project facetten voor uw project selecteert][6]
3. Klik op **Toepassen**.
4. Selecteer **Window** (Venster) > **Preferences** (Voorkeuren) > **Server** > **Runtime Environments** (Runtime-omgevingen) > **Add..** (Toevoegen).
5. Vouw Apache uit en selecteer de versie van de Apache Tomcat-server die u eerder hebt geïnstalleerd. Op ons systeem hebben we versie 8 geïnstalleerd.
   
    ![Scherm opname die laat zien waar u in het venster van de runtime omgeving uw versie van Apache Tomcat kunt selecteren][7]
6. Geef op de volgende pagina de Tomcat-installatiedirectory op. Op een Windows-computer is dit waarschijnlijk C:\Program Files\Apache Software Foundation\Tomcat *versie*.
7. Klik op **Voltooien**.
8. Selecteer **Window** (Venster) > **Preferences** (Voorkeuren) > **Java** > **Installed JREs**(Geïnstalleerde JRE's) > **Add** (Toevoegen).
9. Selecteer in het venster **Add JRE** (JRE toevoegen) de optie **Standard VM** (Standaard-VM).
10. Klik op **Volgende**.
11. Klik in JRE Definition (JRE-definitie), in JRE home (JRE-startpagina), op **Directory**.
12. Navigeer naar **Program Files** > **Java** en selecteer de JDK die u eerder hebt geïnstalleerd. Het is belangrijk dat JDK als de JRE wordt geselecteerd.
13. Kies bij Installed JREs (Geïnstalleerd JRE's) de optie **JDK**. Uw instellingen zijn vergelijkbaar met de volgende schermopname.
    
    ![Scherm afbeelding die laat zien hoe u JDK selecteert als geïnstalleerde JRE][9]
14. Selecteer eventueel **Window** (Venster) > **Web Browser** (Webbrowser) > **Internet Explorer** om de toepassing te openen in een extern browservenster. Het gebruik van de externe browser biedt een betere gebruikservaring voor de webtoepassing.
    
    ![Scherm afbeelding die laat zien hoe u Internet Explorer kunt selecteren als extern browser venster][8]

U hebt de configuratie nu voltooid. Vervolgens kunt u het project bouwen en uitvoeren.

## <a name="build-the-project"></a>Het project bouwen
1. Klik in Projectverkenner met de rechtermuisknop op de projectnaam en kies **Uitvoeren als** > **Maven build...** (Maven-build) om het project te configureren.
   
    ![Scherm afbeelding die laat zien hoe u maven build kiest in het venster Project Verkenner][10]
2. Ga naar Edit Configuration (Configuratie bewerken), typ bij Goals (Doelen) 'clean install' en klik vervolgens op **Run** (Uitvoeren).

Statusberichten worden uitgevoerd naar het consolevensters. Als het goed is wordt de tekst BUILD SUCCESS weergegeven. Hiermee wordt aangegeven dat het project zonder fouten is gebouwd.

## <a name="run-the-app"></a>De app uitvoeren
In deze laatste stap voert u de toepassing uit in de runtime-omgeving van een lokale server.

Als u nog geen serverruntime-omgeving hebt opgegeven in Eclipse, moet u dat eerst doen.

1. Vouw in Projectverkenner **WebContent** uit.
2. Klik met de rechtermuisknop op **Search.jsp** > **Run As** (Uitvoeren als) > **Run on Server** (Uitvoeren op server). Selecteer de Apache Tomcat-server en klik vervolgens op **Uitvoeren**.

> [!TIP]
> Als u geen standaardwerkruimte hebt gebruikt om uw project op te slaan, moet u **Run Configuration** (Configuratie uitvoeren) wijzigen om naar de locatie van het project te wijzen, zodat er geen opstartfout voor de server optreedt. Klik in Projectverkenner met de rechtermuisknop op **Search.jsp** > **Uitvoeren als** > **Configuratie uitvoeren**. Selecteer de Apache Tomcat-server. Klik op **Argumenten**. Klik op **Werkruimte** of **Bestandssysteem** om de map in te stellen die het project bevat.
> 
> 

Wanneer u de toepassing uitvoert, ziet u een browservenster met een zoekvak waarin u termen kunt opgeven.

Wacht ongeveer een minuut voordat u op **Zoeken** klikt om de service de tijd te geven om de index te maken en te laden. Als er een HTTP 404-fout optreedt, moet u alleen iets langer wachten voordat u het opnieuw probeert.

## <a name="search-on-usgs-data"></a>Zoeken in USGS-gegevens
De USGS-gegevensset bevat records die relevant zijn voor de staat Rhode Island. Als u op **Search** (Zoeken) klikt terwijl het zoekvak leeg is, worden de bovenste 50 vermeldingen weergegeven. Dit is de standaardinstelling.

Als u een zoekterm invoert, geeft u de zoekmachine iets om mee te werken. Voer een regionale naam in. 'Roger Williams' was de eerste gouverneur van Rhode Island. Er zijn verschillende parken, gebouwen en scholen naar hem vernoemd.

![Scherm afbeelding die laat zien hoe u naar USGS-gegevens zoekt][11]

U kunt ook de volgende termen proberen:

* Pawtucket
* Pembroke
* goose +cape

## <a name="next-steps"></a>Volgende stappen
Dit is de eerste Azure Search-zelfstudie op basis van Java en de USGS-gegevensset. In de loop van de tijd zal deze zelfstudie worden uitgebreid om aanvullende zoekfuncties te demonstreren die u mogelijk wilt gebruiken in uw aangepaste oplossingen.

Als u al enige ervaring met Azure Search hebt, kunt u dit voorbeeld gebruiken als springplank voor verdere experimenten, om de [zoekpagina](search-pagination-page-layout.md) uit te breiden of om [facetnavigatie](search-faceted-navigation.md) te implementeren. U kunt ook de pagina met zoekresultaten verbeteren door tellers toe te voegen document in batch te verwerken, zodat gebruikers door de resultaten kunnen bladeren.

Bent u niet bekend met Azure Search? Het is raadzaam andere zelfstudies te bekijken om inzicht te verwerven in wat u zoal kunt maken. Bezoek de [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) voor meer resources. 

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
