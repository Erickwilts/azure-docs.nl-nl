---
title: Azure Analysis Services scale-out | Microsoft Docs
description: Azure Analysis Services servers repliceren met scale-out
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8297a2b1e78da6685b3129071612dc4457990bc1
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68696391"
---
# <a name="azure-analysis-services-scale-out"></a>Uitschalen van Azure Analysis Services

Met scale-out kunnen client query's worden verdeeld over meerdere *query replica's* in een *query groep*, waardoor de reactie tijden tijdens veel query's worden verminderd. U kunt ook de verwerking van de query groep scheiden, zodat de client query's niet nadelig worden beïnvloed door de verwerking van bewerkingen. Scale-out kan worden geconfigureerd in Azure Portal of door gebruik te maken van de Analysis Services REST API.

Uitschalen is beschikbaar voor servers in de prijs categorie Standard. Elke query replica wordt gefactureerd op basis van hetzelfde aantal als uw server. Alle query replica's worden in dezelfde regio gemaakt als uw server. Het aantal query replica's dat u kunt configureren, is beperkt door de regio waarin uw server zich bevindt. Zie [Beschik baarheid per regio](analysis-services-overview.md#availability-by-region)voor meer informatie. Als u uitschalen uitbreidt, wordt de hoeveelheid beschikbaar geheugen voor uw server niet verg root. Als u geheugen wilt verg Roten, moet u een upgrade voor uw abonnement uitvoeren. 

## <a name="why-scale-out"></a>Waarom uitschalen?

In een typische Server implementatie fungeert één server als zowel de verwerkings server als de query server. Als het aantal client query's voor modellen op uw server groter is dan de QPU (query processing units) voor het abonnement van uw server, of als de verwerking van modellen op hetzelfde moment als hoge query werkbelastingen plaatsvindt, kan de prestaties afnemen. 

Met scale-out kunt u een query pool maken met Maxi maal zeven aanvullende bronnen voor query replica's (acht totaal, inclusief uw *primaire* server). U kunt het aantal replica's in de query pool schalen om te voldoen aan QPU-vereisten op kritieke tijden, en u kunt op elk gewenst moment een verwerkings server scheiden van de query pool. 

Ongeacht het aantal query replica's in een query groep, worden de verwerkings werkbelastingen niet verdeeld over query replica's. De primaire server fungeert als de verwerkings server. Query replica's bieden alleen query's voor de model databases die zijn gesynchroniseerd tussen de primaire server en elke replica in de query groep. 

Wanneer u uitschaalt, kan het tot vijf minuten duren voordat nieuwe query replica's incrementeel worden toegevoegd aan de query groep. Wanneer alle nieuwe query replica's actief zijn, worden nieuwe client verbindingen verdeeld over alle resources in de query groep. Bestaande client verbindingen worden niet gewijzigd ten opzichte van de resource waarmee ze momenteel zijn verbonden. Bij het schalen in worden alle bestaande client verbindingen met een bron van de query groep die uit de query groep wordt verwijderd, beëindigd. Clients kunnen opnieuw verbinding maken met een resterende resource in de query groep.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer u de eerste keer scale-out configureert, worden model databases op uw primaire server *automatisch* gesynchroniseerd met nieuwe replica's in een nieuwe query groep. Automatische synchronisatie wordt slechts één keer uitgevoerd. Tijdens automatische synchronisatie worden de gegevens bestanden van de primaire server (versleuteld op rest in Blob Storage) gekopieerd naar een tweede locatie, ook versleuteld op rest in Blob Storage. Replica's in de query groep worden vervolgens gehydrateerd met gegevens uit de tweede set bestanden. 

Hoewel een automatische synchronisatie alleen wordt uitgevoerd wanneer u een server voor de eerste keer uitbreidt, kunt u ook een hand matige synchronisatie uitvoeren. Bij het synchroniseren worden de gegevens van replica's in de query groep vergeleken met die van de primaire server. Wanneer modellen op de primaire server worden verwerkt (vernieuwd), moet er een synchronisatie worden uitgevoerd *nadat* de verwerkings bewerkingen zijn voltooid. Deze synchronisatie kopieert bijgewerkte gegevens van de bestanden van de primaire server in Blob Storage naar de tweede set bestanden. Replica's in de query groep worden vervolgens gehydrateerd met bijgewerkte gegevens uit de tweede set bestanden in Blob Storage. 

Wanneer u een volgende uitschaal bewerking uitvoert, bijvoorbeeld het aantal replica's in de query groep verhogen van twee naar vijf, worden de nieuwe replica's gehydrateerd met gegevens uit de tweede set bestanden in Blob Storage. Er is geen synchronisatie. Als u een synchronisatie hebt uitgevoerd nadat u hebt geschaald, worden de nieuwe replica's in de query groep twee keer gehydrateerd: een redundante Hydration. Wanneer u een volgende scale-out-bewerking uitvoert, is het belang rijk dat u rekening houdt met het volgende:

* Voer een synchronisatie uit *vóór de uitschaal bewerking* om redundante Hydration van de toegevoegde replica's te voor komen. Gelijktijdige synchronisatie-en scale-out bewerkingen die op hetzelfde moment worden uitgevoerd, zijn niet toegestaan.

* Wanneer u zowel verwerkings- *als* uitschaal bewerkingen automatiseert, is het belang rijk om eerst gegevens op de primaire server te verwerken, vervolgens een synchronisatie uit te voeren en vervolgens de uitschaal bewerking uit te voeren. Met deze reeks wordt de minimale impact op QPU en geheugen bronnen gegarandeerd.

* Synchronisatie is ook toegestaan wanneer er geen replica's in de query groep zijn. Als u opschalen van nul naar een of meer replica's met nieuwe gegevens van een verwerkings bewerking op de primaire server, voert u de synchronisatie eerst uit zonder replica's in de query groep en vervolgens scale-out. Bij het synchroniseren van wordt geen redundante Hydration van de zojuist toegevoegde replica's voor komen.

* Wanneer u een model database van de primaire server verwijdert, wordt deze niet automatisch verwijderd uit replica's in de query groep. U moet een synchronisatie bewerking uitvoeren met behulp van de Power shell [-opdracht Sync-AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) die het bestand/de s voor die data base verwijdert uit de gedeelde Blob-opslag locatie van de replica en vervolgens de model database op de replica's verwijdert. in de query groep. Als u wilt bepalen of een model database bestaat voor replica's in de query pool, maar niet op de primaire server, moet u ervoor zorgen dat de **afzonderlijke verwerkings server van de query groep** is ingesteld op **Ja**. Gebruik vervolgens SSMS om verbinding te maken met de primaire server `:rw` met behulp van de kwalificatie om te controleren of de data base bestaat. Maak vervolgens verbinding met replica's in de query groep door verbinding te maken `:rw` zonder de kwalificatie om te zien of dezelfde data base ook bestaat. Als de data base bestaat in replica's in de query groep, maar niet op de primaire server, voert u een synchronisatie bewerking uit.   

* Bij het wijzigen van de naam van een Data Base op de primaire server is er een extra stap die nodig is om ervoor te zorgen dat de data base correct is gesynchroniseerd met replica's. Nadat u de naam hebt gewijzigd, moet u een synchronisatie uitvoeren met behulp van `-Database` de opdracht [Sync-AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance) en de para meter met de oude database naam opgeven. Deze synchronisatie verwijdert de data base en bestanden met de oude naam uit een of meer replica's. Voer vervolgens een andere synchronisatie uit `-Database` met de para meter met de naam van de nieuwe data base. Bij de tweede synchronisatie wordt de data base met de nieuwe naam gekopieerd naar de tweede set bestanden en worden eventuele replica's gehydrateerd. Deze synchronisaties kunnen niet worden uitgevoerd met behulp van de opdracht model synchroniseren in de portal.

### <a name="separate-processing-from-query-pool"></a>Afzonderlijke verwerking van de query pool

U kunt de verwerkings server scheiden van de query groep voor een maximale prestaties voor zowel verwerkings-als query bewerkingen. Wanneer deze worden gescheiden, worden nieuwe client verbindingen alleen toegewezen aan query replica's in de query groep. Als verwerkings bewerkingen slechts korte tijd duren, kunt u ervoor kiezen om de verwerkings server alleen te scheiden van de query pool voor de hoeveelheid tijd die nodig is voor het uitvoeren van verwerkings-en synchronisatie bewerkingen en deze vervolgens weer op te nemen in de query groep. Wanneer u de verwerkings server van de query groep scheidt of deze opnieuw in de query groep toevoegt, kan het tot vijf minuten duren voordat de bewerking is voltooid.

## <a name="monitor-qpu-usage"></a>QPU-gebruik bewaken

Als u wilt bepalen of uitschalen voor uw server nodig is, controleert u de server in Azure Portal met behulp van metrische gegevens. Als uw QPU regel matig uitdergelijke, betekent dit dat het aantal query's voor uw modellen de limiet van QPU voor uw abonnement overschrijdt. De metrische gegevens wachtrij lengte van de query pool neemt ook toe wanneer het aantal query's in de wachtrij van de query thread de beschik bare QPU overschrijdt. 

Een andere goede metrische waarde om te kijken, is gemiddelde QPU door ServerResourceType. Deze metrische gegevens vergelijkt de gemiddelde QPU voor de primaire server met die van de query groep. 

![Metrische gegevens van query uitschalen](media/analysis-services-scale-out/aas-scale-out-monitor.png)

### <a name="to-configure-qpu-by-serverresourcetype"></a>QPU configureren met ServerResourceType
1. In een lijn diagram met metrische gegevens klikt u op **metrische gegevens toevoegen**. 
2. Selecteer uw server in **resource**, Selecteer in **metrische naam ruimte**de optie **Analysis Services standaard metrieken**en selecteer vervolgens in **metrische gegevens** **QPU**, en selecteer vervolgens bij **aggregatie**de optie **Gem**. 
3. Klik op **splitsing Toep assen**. 
4. In **waarden**selecteert u **ServerResourceType**.  

Zie [Monitor server metrics](analysis-services-monitor.md) (Metrische servergegevens bewaken) voor meer informatie.

## <a name="configure-scale-out"></a>Uitschalen configureren

### <a name="in-azure-portal"></a>In Azure Portal

1. Klik in de portal op **uitschalen**. Gebruik de schuif regelaar om het aantal query replica servers te selecteren. Het aantal replica's dat u kiest, is naast uw bestaande server.  

2. Selecteer op **de verwerkings server scheiden van de query groep**Ja om de verwerkings server van de query servers uit te sluiten. Client [verbindingen](#connections) die gebruikmaken van de standaard Connection String `:rw`(zonder) worden omgeleid naar replica's in de query groep. 

   ![Schuif regelaar voor uitschalen](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Klik op **Opslaan** om uw nieuwe query replica servers in te richten. 

Bij het configureren van scale-out voor een server wordt de eerste keer dat modellen op de primaire server automatisch worden gesynchroniseerd met replica's in de query groep. Automatische synchronisatie vindt slechts eenmaal plaats, wanneer u scale-out voor het eerst op een of meer replica's configureert. Volgende wijzigingen in het aantal replica's op dezelfde server *activeren geen andere automatische synchronisatie*. De automatische synchronisatie wordt niet opnieuw uitgevoerd, zelfs niet als u de server instelt op nul replica's en vervolgens weer uitschaalt naar een wille keurig aantal replica's. 

## <a name="synchronize"></a>Synchroniseren 

Synchronisatie bewerkingen moeten hand matig of via de REST API worden uitgevoerd.

### <a name="in-azure-portal"></a>In Azure Portal

In **overzicht** > model > **model synchroniseren**.

![Schuif regelaar voor uitschalen](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>REST-API

Gebruik de **synchronisatie** bewerking.

#### <a name="synchronize-a-model"></a>Een model synchroniseren   

`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Synchronisatie status ophalen  

`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

Retour status codes:


|Code  |Description  |
|---------|---------|
|-1     |  Ongeldig       |
|0     | Repliceren...        |
|1     |  Reactiveren       |
|2     |   Voltooid       |
|3     |   Mislukt      |
|4     |    Voltooien     |
|||


### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voordat u Power shell gebruikt, [moet u de nieuwste Azure PowerShell-module installeren of bijwerken](/powershell/azure/install-az-ps). 

Als u synchronisatie wilt uitvoeren, gebruikt u [Sync-AzAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/az.analysisservices/sync-AzAnalysisServicesinstance).

Als u het aantal query replica's wilt instellen, gebruikt u [set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). Geef de optionele `-ReadonlyReplicaCount` para meter op.

Als u de verwerkings server van de query groep wilt scheiden, gebruikt u [set-AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver). Geef de optionele `-DefaultConnectionMode` para meter op `Readonly`die moet worden gebruikt.

Zie [een Service-Principal gebruiken met de module AZ. AnalysisServices](analysis-services-service-principal.md#azmodule)voor meer informatie.

## <a name="connections"></a>Verbindingen

Op de overzichts pagina van de server bevinden zich twee server namen. Als u nog geen uitschalen hebt geconfigureerd voor een server, werken beide server namen op hetzelfde niveau. Zodra u scale-out voor een server hebt geconfigureerd, moet u de juiste server naam opgeven, afhankelijk van het verbindings type. 

Voor client verbindingen van eind gebruikers, zoals Power BI Desktop, Excel en aangepaste apps, gebruikt u **Server naam**. 

Voor SSMS-, SSDT-en verbindings reeksen in Power shell, Azure function-apps en AMO, gebruikt u de naam van de **beheer server**. De naam van de beheer server bevat `:rw` een speciale kwalificatie (lezen/schrijven). Alle verwerkings bewerkingen worden uitgevoerd op de beheer server (primair).

![Server namen](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="scale-up--down-vs-scale-out"></a>Omhoog schalen versus omhoog/omlaag Uitschalen

U kunt de prijs categorie van de server wijzigen op een server met meerdere replica's. Dezelfde prijs categorie is van toepassing op alle replica's. Met de bewerking omhoog en omlaag schalen worden eerst alle replica's in één keer omlaag gebracht. vervolgens worden alle replica's op de nieuwe prijs categorie weer geven.

## <a name="troubleshoot"></a>Problemen oplossen

**Probleem:** Fout bij **het ophalen van gebruikers kan\<de server naam van de server > exemplaar niet vinden in de verbindings modus readonly.**

**Oplossen** Wanneer u de **afzonderlijke verwerking van de verwerkings server van de optie query groep** selecteert, worden client verbindingen die `:rw`gebruikmaken van de standaard Connection String (zonder), omgeleid naar de replica's van de query groep. Als replica's in de query groep nog niet online zijn omdat de synchronisatie nog niet is voltooid, kunnen omgeleide client verbindingen mislukken. Als u mislukte verbindingen wilt voor komen, moeten er ten minste twee servers in de query groep aanwezig zijn bij het uitvoeren van een synchronisatie. Elke server wordt afzonderlijk gesynchroniseerd, terwijl anderen online blijven. Als u de verwerkings server niet in de query groep hebt tijdens de verwerking, kunt u deze verwijderen uit de groep voor verwerking en deze vervolgens opnieuw toevoegen aan de groep nadat de verwerking is voltooid, maar vóór de synchronisatie. Gebruik geheugen-en QPU-metrische gegevens om de synchronisatie status te bewaken.



## <a name="related-information"></a>Gerelateerde informatie

[Metrische Server gegevens bewaken](analysis-services-monitor.md)   
[Azure Analysis Services beheren](analysis-services-manage.md) 
