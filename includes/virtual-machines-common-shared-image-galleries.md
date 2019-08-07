---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: e43b8c951d8c7d5d60904fe49d8639efaba5c89f
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775863"
---
Galerie met gedeelde afbeeldingen is een service die u helpt bij het bouwen van structuur en organisatie rond uw beheerde installatie kopieën. Galerieën met gedeelde afbeeldingen bieden:

- Beheerde algemene replicatie van installatie kopieën.
- Versie beheer en groepering van installatie kopieën voor eenvoudiger beheer.
- Maxi maal beschik bare installatie kopieën met ZRS-accounts (zone redundant Storage) in regio's die Beschikbaarheidszones ondersteunen. ZRS biedt een betere veerkracht tegen zonegebonden fouten.
- Het delen van meerdere abonnementen en zelfs tussen Active Directory (AD)-tenants met behulp van RBAC.
- Het schalen van uw implementaties met afbeeldings replica's in elke regio.

Met behulp van een galerie met gedeelde afbeeldingen kunt u uw installatie kopieën delen met verschillende gebruikers, service-principals of AD-groepen binnen uw organisatie. Gedeelde installatie kopieën kunnen worden gerepliceerd naar meerdere regio's, zodat u uw implementaties sneller kunt schalen.

Een beheerde installatie kopie is een kopie van een volledige virtuele machine (inclusief eventuele gekoppelde gegevens schijven) of alleen de besturingssysteem schijf, afhankelijk van hoe u de installatie kopie maakt. Wanneer u een virtuele machine maakt op basis van de installatie kopie, wordt een kopie van de Vhd's in de installatie kopie gebruikt voor het maken van de schijven voor de nieuwe virtuele machine. De beheerde installatie kopie blijft in de opslag en kan opnieuw worden gebruikt voor het maken van nieuwe Vm's.

Als u een groot aantal beheerde installatie kopieën hebt dat u wilt behouden en deze beschikbaar wilt maken in het hele bedrijf, kunt u een galerie met gedeelde afbeeldingen gebruiken als opslag plaats waarmee u gemakkelijk uw installatie kopieën kan delen. 

De functie gedeelde installatie kopie galerie heeft meerdere bron typen:

| Resource | Description|
|----------|------------|
| **Beheerde installatie kopie** | Een basis installatie kopie die alleen kan worden gebruikt of gebruikt om een **installatie kopie versie** in een galerie met installatie kopieën te maken. Beheerde installatie kopieën worden gemaakt op basis van gegeneraliseerde Vm's. Een beheerde installatie kopie is een speciaal type VHD dat kan worden gebruikt om meerdere virtuele machines te maken en kan nu worden gebruikt om versies van gedeelde installatie kopieën te maken. |
| **Galerie met installatie kopieën** | Net als de Azure Marketplace is een **afbeeldings galerie** een opslag plaats voor het beheren en delen van installatie kopieën, maar u bepaalt wie toegang heeft. |
| **Definitie van installatie kopie** | Installatie kopieën worden in een galerie gedefinieerd en bevatten informatie over de installatie kopie en vereisten voor het gebruik ervan binnen uw organisatie. U kunt bijvoorbeeld informatie over de installatie kopie toevoegen aan Windows of Linux, minimale en maximale geheugen vereisten en release opmerkingen. Het is een definitie van een type installatie kopie. |
| **Versie van installatie kopie** | Een **installatie kopie versie** is wat u gebruikt om een virtuele machine te maken wanneer u een galerie gebruikt. U kunt meerdere versies van een installatie kopie naar behoefte hebben voor uw omgeving. Net als bij een beheerde installatie kopie wordt de versie van de installatie kopie gebruikt voor het maken van nieuwe schijven voor de virtuele machine wanneer u een **installatie kopie** gebruikt voor het maken van een virtuele machine. Installatie kopie versies kunnen meerdere keren worden gebruikt. |

<br>


![Afbeelding die laat zien hoe u meerdere versies van een afbeelding in uw galerie kunt hebben](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>installatiekopiedefinities

Afbeeldings definities zijn een logische groepering voor versies van een installatie kopie. De definitie van de installatie kopie bevat informatie over waarom de installatie kopie is gemaakt, waarvoor het besturings systeem is en informatie over het gebruik van de installatie kopie. Een definitie van een installatie kopie is een plan voor alle details over het maken van een specifieke installatie kopie. U implementeert geen VM op basis van een definitie van een installatie kopie, maar vanuit de installatie kopie versie die is gemaakt op basis van de definitie.


Er zijn drie para meters voor elke definitie van de installatie kopie die worden gebruikt in combi natie: **Publisher**, **aanbieding** en **SKU**. Deze worden gebruikt om een specifieke definitie van een installatie kopie te vinden. U kunt installatie kopie versies die een of twee delen, maar niet alle drie waarden bevatten.  Dit zijn bijvoorbeeld drie afbeeldings definities en hun waarden:

|Definitie van installatiekopie|Uitgever|Aanbieding|Sku|
|---|---|---|---|
|myImage1|Contoso|Financiën|Back-end|
|myImage2|Contoso|Financiën|Front-end|
|myImage3|Testen|Financiën|Front-end|

Alle drie deze hebben unieke sets waarden. De indeling is vergelijkbaar met de manier waarop u op dit moment Publisher, aanbieding en SKU kunt opgeven voor [installatie kopieën van Azure Marketplace](../articles/virtual-machines/windows/cli-ps-findimage.md) in azure PowerShell om de nieuwste versie van een Marketplace-installatie kopie te verkrijgen. Elke afbeeldings definitie moet een unieke set van deze waarden hebben.

Hieronder ziet u andere para meters die kunnen worden ingesteld voor de definitie van uw installatie kopie, zodat u uw resources gemakkelijker kunt bijhouden:

* Status van het besturings systeem: u kunt de besturingssysteem status instellen op gegeneraliseerd of gespecialiseerd, maar alleen gegeneraliseerd wordt momenteel ondersteund. Installatie kopieën moeten worden gemaakt op basis van vm's die zijn gegeneraliseerd met Sysprep voor `waagent -deprovision` Windows of voor Linux.
* Besturings systeem: dit kan Windows of Linux zijn.
* Beschrijving: gebruik beschrijving om meer gedetailleerde informatie te geven over waarom de definitie van de installatie kopie bestaat. U kunt bijvoorbeeld een definitie van een installatie kopie hebben voor de front-end-server waarop de toepassing vooraf is geïnstalleerd.
* EULA: kan worden gebruikt om te verwijzen naar een gebruiksrecht overeenkomst die specifiek is voor de definitie van de installatie kopie.
* Privacyverklaring en opmerkingen bij de release: Sla release opmerkingen en privacyverklaringen op in azure Storage en geef een URI op om ze te openen als onderdeel van de definitie van de installatie kopie.
* Datum van einde van de levens duur: koppel een einddatum datum aan de definitie van uw installatie kopie om de oude afbeeldings definities te kunnen verwijderen met automatisering.
* Tag: u kunt labels toevoegen wanneer u de definitie van de installatie kopie maakt. Zie [Tags gebruiken om uw resources te organiseren](../articles/azure-resource-manager/resource-group-using-tags.md) voor meer informatie over tags.
* Minimale en maximale vCPU-en geheugen aanbevelingen: als uw installatie kopie vCPU en geheugen aanbevelingen heeft, kunt u die gegevens koppelen aan de definitie van uw installatie kopie.
* Niet-toegestane schijf typen: u kunt informatie geven over de opslag behoeften voor uw VM. Als de installatie kopie bijvoorbeeld niet geschikt is voor standaard HDD-schijven, voegt u deze toe aan de lijst niet toegestaan.


## <a name="regional-support"></a>Regionale ondersteuning

De bron regio's worden weer gegeven in de volgende tabel. Alle open bare regio's kunnen doel regio's zijn, maar om naar Australië-centraal te repliceren en Australië-centraal 2 moet u uw abonnement white list. Als u White List wilt aanvragen, gaat u naar: https://azure.microsoft.com/en-au/global-infrastructure/australia/contact/


| Bron regio's |
|---------------------|-----------------|------------------|-----------------|
| Australië - centraal   | US - centraal EUAP | Korea - centraal    | US - west-centraal |
| Australië - centraal 2 | Azië - oost       | Korea - zuid      | Europa -west     |
| Australië - oost      | East US         | US - noord-centraal | India - west      |
| Australië - zuidoost | US - oost 2       | Europa - noord     | US - west         |
| Brazilië - zuid        | US - oost 2 EUAP  | US - zuid-centraal | US - west 2       |
| Canada - midden      | Frankrijk - centraal  | India - zuid      |                 |
| Canada - oost         | Frankrijk - zuid    | Azië - zuidoost   |                 |
| India - centraal       | Japan - oost      | Verenigd Koninkrijk Zuid         |                 |
| US - centraal          | Japan - west      | Verenigd Koninkrijk West          |                 |



## <a name="limits"></a>Limieten 

Er zijn limieten, per abonnement, voor het implementeren van resources met behulp van de galerie met gedeelde afbeeldingen:
- 100 gedeelde afbeeldings galerieën, per abonnement, per regio
- 1\.000 installatie kopie definities, per abonnement, per regio
- 10.000 installatie kopie versies, per abonnement, per regio

Zie voor meer informatie [resource gebruik controleren](https://docs.microsoft.com/azure/networking/check-usage-against-limits) op limieten voor voor beelden van het controleren van uw huidige gebruik.
 

## <a name="scaling"></a>Schalen
Met de galerie voor gedeelde afbeeldingen kunt u het aantal replica's opgeven dat door Azure moet worden bewaard voor de installatie kopieën. Dit helpt bij implementatie scenario's met meerdere VM'S als de VM-implementaties kunnen worden gespreid naar verschillende replica's, waardoor de kans dat verwerking van het proces voor het maken van exemplaren wordt beperkt door overbelasting van één replica wordt verkleind.


Met de galerie voor gedeelde afbeeldingen kunt u nu tot 1.000 VM-exemplaren implementeren in een schaalset voor virtuele machines (tot 600 met beheerde installatie kopieën). Image replica's bieden betere implementatie prestaties, betrouw baarheid en consistentie.  U kunt in elke doel regio een ander aantal replica's instellen, op basis van de schaal behoeften voor de regio. Omdat elke replica een diepe kopie van uw installatie kopie is, kunt u hiermee uw implementaties lineair schalen met elke extra replica. We begrijpen dat er niet twee installatie kopieën of regio's hetzelfde zijn. Hier volgt onze algemene richt lijnen voor het gebruik van replica's in een regio:

- Voor elke 20 Vm's die u gelijktijdig maakt, raden we u aan één replica te blijven gebruiken. Als u 120 Vm's gelijktijdig maakt met behulp van dezelfde installatie kopie in een regio, wordt u aangeraden minstens 6 replica's van uw installatie kopie te gebruiken. 
- Voor elke implementatie van schaal sets met Maxi maal 600 exemplaren, raden we u aan ten minste één replica te gebruiken. Als u bijvoorbeeld vijf schaal sets gelijktijdig maakt, waarbij elk 600 VM-exemplaren dezelfde installatie kopie in één regio gebruikt, wordt u aangeraden ten minste vijf replica's van uw installatie kopie te gebruiken. 

We raden u altijd aan het aantal replica's te overtreffen door factoren zoals afbeeldings grootte, inhoud en type besturings systeem.


![Afbeelding die laat zien hoe u afbeeldingen kunt schalen](./media/shared-image-galleries/scaling.png)



## <a name="make-your-images-highly-available"></a>Uw afbeeldingen Maxi maal beschikbaar maken

[Azure zone redundant Storage (ZRS)](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) biedt een tolerantie voor een fout in de beschikbaarheids zone in de regio. Met de algemene Beschik baarheid van de galerie gedeelde afbeeldingen kunt u ervoor kiezen om uw installatie kopieën op te slaan in ZRS-accounts in regio's met Beschikbaarheidszones. 

U kunt ook het account type kiezen voor elk van de doel regio's. Het standaard type opslag account is Standard_LRS, maar u kunt Standard_ZRS voor regio's met Beschikbaarheidszones kiezen. Controleer [hier](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs)de regionale beschik BAARHEID van ZRS.

![Afbeelding met ZRS](./media/shared-image-galleries/zrs.png)


## <a name="replication"></a>Replicatie
Met de galerie voor gedeelde afbeeldingen kunt u ook automatisch uw installatie kopieën naar andere Azure-regio's repliceren. Elke versie van een gedeelde installatie kopie kan worden gerepliceerd naar verschillende regio's, afhankelijk van wat voor uw organisatie zinvol is. Een voor beeld is om altijd de nieuwste afbeelding in meerdere regio's te repliceren terwijl alle oudere versies alleen beschikbaar zijn in één regio. Dit kan helpen besparen op opslag kosten voor gedeelde installatie kopie versies. 

De regio's waarvan een versie van de gedeelde installatie kopie wordt gerepliceerd naar, kunnen worden bijgewerkt na de aanmaak tijd. De tijd die nodig is om naar verschillende regio's te repliceren, is afhankelijk van de hoeveelheid gegevens die wordt gekopieerd en het aantal regio's waarnaar de versie wordt gerepliceerd. Dit kan in sommige gevallen enkele uren duren. Terwijl de replicatie plaatsvindt, kunt u de status van de replicatie per regio weer geven. Zodra de replicatie van de installatie kopie in een regio is voltooid, kunt u een virtuele machine of schaalset implementeren met behulp van de installatie kopie versie in de regio.

![Afbeelding die laat zien hoe u installatie kopieën kunt repliceren](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Toegang

Aangezien de galerie met gedeelde afbeeldingen, de afbeeldings definitie en de versie van de installatie kopie alle resources zijn, kunnen ze worden gedeeld met behulp van de ingebouwde systeem eigen Azure RBAC-besturings elementen. Met RBAC kunt u deze resources delen met andere gebruikers, service-principals en groepen. U kunt zelfs toegang delen voor personen buiten de Tenant waarin deze zijn gemaakt. Zodra een gebruiker toegang heeft tot de versie van de gedeelde installatie kopie, kunnen ze een virtuele machine of een VM-Schaalset implementeren.  Dit is de matrix voor delen waarmee u begrijpt waarover de gebruiker toegang krijgt:

| Gedeeld met gebruiker     | Gedeelde installatiekopiegalerie | Definitie van installatiekopie | Beeldversie |
|----------------------|----------------------|--------------|----------------------|
| Gedeelde installatiekopiegalerie | Ja                  | Ja          | Ja                  |
| Definitie van installatiekopie     | Nee                   | Ja          | Ja                  |

U wordt aangeraden op Galerie niveau te delen voor de beste ervaring. Het is niet raadzaam om afzonderlijke versies van een installatie kopie te delen. Zie [toegang tot Azure-resources beheren met RBAC](../articles/role-based-access-control/role-assignments-portal.md)voor meer informatie over RBAC.

Installatie kopieën kunnen ook worden gedeeld, op schaal, zelfs via tenants met behulp van een multi tenant-app-registratie. Zie [Galerie-VM-installatie kopieën delen in azure](../articles/virtual-machines/linux/share-images-across-tenants.md)-tenants voor meer informatie over het delen van installatie kopieën tussen tenants.

## <a name="billing"></a>Billing
Er zijn geen extra kosten verbonden aan het gebruik van de Shared Image Gallery-service. Er worden kosten in rekening gebracht voor de volgende resources:
- Opslag kosten voor het opslaan van de versies van de gedeelde installatie kopieën. De kosten zijn afhankelijk van het aantal replica's van de versie van de installatie kopie en het aantal regio's waarnaar de versie wordt gerepliceerd. Als u bijvoorbeeld twee installatie kopieën hebt en beide worden gerepliceerd naar drie regio's, wordt u gewijzigd voor 6 Managed disks op basis van de grootte. Zie [Managed disks prijzen](https://azure.microsoft.com/pricing/details/managed-disks/)voor meer informatie.
- Uitgaande netwerk kosten voor replicatie van de eerste versie van de installatie kopie van de bron regio naar de gerepliceerde regio's. Volgende replica's worden binnen de regio verwerkt, zodat er geen extra kosten in rekening worden gebracht. 

## <a name="updating-resources"></a>De resources worden bijgewerkt

Nadat u deze hebt gemaakt, kunt u enkele wijzigingen aanbrengen in de afbeeldingen galerie-resources. Deze zijn beperkt tot:
 
Galerie met gedeelde afbeeldingen:
- Description

Definitie van installatie kopie:
- Aanbevolen Vcpu's
- Aanbevolen geheugen
- Description
- Einde van de levens duur

Versie van installatie kopie:
- Aantal regionale replica's
- Doelregio's
- Van laatste uitsluiten
- Einde van de levens duur


## <a name="sdk-support"></a>SDK-ondersteuning

Met de volgende Sdk's wordt ondersteuning geboden voor het maken van gedeelde afbeeldings galerieën:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/azure-arm-compute/?view=azure-node-latest)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Go](https://docs.microsoft.com/go/azure/)

## <a name="templates"></a>Sjablonen

U kunt een resource voor de galerie met gedeelde afbeeldingen maken met behulp van sjablonen. Er zijn verschillende Azure Quick Start-sjablonen beschikbaar: 

- [Een galerie met gedeelde afbeeldingen maken](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Een definitie van een installatie kopie maken in een galerie met gedeelde afbeeldingen](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Een installatie kopie versie maken in een galerie met gedeelde afbeeldingen](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Een VM maken op basis van de installatie kopie versie](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

**V:** Hoe kan ik alle resources van de galerie met gedeelde afbeeldingen in abonnementen weer geven? 
 
 A. Volg de onderstaande stappen om alle resources van de gedeelde installatie kopie galerie weer te geven in abonnementen waartoe u toegang hebt op de Azure Portal:

1. Open de [Azure Portal](https://portal.azure.com).
1. Ga naar **alle resources**.
1. Selecteer alle abonnementen waaronder u alle resources wilt weer geven.
1. Zoek naar bronnen van het type **persoonlijke galerie**.
 
   Als u de afbeeldings definities en afbeeldings versies wilt zien, moet u ook **verborgen typen weer geven**selecteren.
 
   Als u alle resources van de galerie met gedeelde afbeeldingen wilt weer geven voor alle abonnementen waarvoor u machtigingen hebt, gebruikt u de volgende opdracht in de Azure CLI:

   ```bash
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```


**V:** Kan ik mijn bestaande afbeelding verplaatsen naar de galerie met gedeelde afbeeldingen?
 
 A. Ja. Er zijn drie scenario's die zijn gebaseerd op de typen installatie kopieën die u mogelijk hebt.

 Scenario 1: Als u een beheerde installatie kopie hebt, kunt u er een installatie kopie-en afbeeldings versie van maken.

 Scenario 2: Als u een niet-beheerde gegeneraliseerde installatie kopie hebt, kunt u er een beheerde installatie kopie van maken en vervolgens een installatie kopie en afbeeldings versie van de afbeelding maken. 

 Scenario 3: Als u een VHD hebt in uw lokale bestands systeem, moet u de VHD uploaden, een beheerde installatie kopie maken en vervolgens de versie van de installatie kopie en de installatie kopie maken.
- Als de VHD van een Windows-VM is, raadpleegt u [een gegeneraliseerde VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Zie [een VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd) als de VHD voor een virtuele Linux-machine is.


**V:** Kan ik een installatie kopie versie van een gespecialiseerde schijf maken?

 A. Nee, we ondersteunen momenteel geen gespecialiseerde schijven als installatie kopieën. Als u een gespecialiseerde schijf hebt, moet u [een virtuele machine maken op basis van de VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) door de speciale schijf aan een nieuwe virtuele machine te koppelen. Zodra u een virtuele machine uitvoert, moet u de instructies volgen voor het maken van een beheerde installatie kopie van de virtuele [Windows-machine](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) of [Linux-machine](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Zodra u een gegeneraliseerde beheerde installatie kopie hebt, kunt u het proces starten om een beschrijving van de gedeelde installatie kopie en de versie van de installatie kopie te maken.

 
**V:** Nadat de resource voor de gedeelde afbeeldings galerie is gemaakt, kan ik deze verplaatsen naar een ander abonnement?

 A. Nee, u kunt de resource van de gedeelde afbeeldings galerie niet naar een ander abonnement verplaatsen. U kunt de installatie kopieën in de galerie echter naar andere regio's repliceren.

**V:** Kan ik mijn afbeeldings versies in Clouds repliceren: Azure China 21Vianet, Azure Duitsland en Azure Government Cloud? 

 A. Nee, u kunt geen afbeeldings versies van Clouds repliceren.

**V:** Kan ik mijn installatie kopie versies in abonnementen repliceren? 

 A. Nee, u kunt de installatie kopie versies van de verschillende regio's in een abonnement repliceren en deze gebruiken in andere abonnementen via RBAC.

**V:** Kan ik installatie kopie versies delen in azure AD-tenants? 

 A. Ja, u kunt RBAC gebruiken om te delen met personen in een Tenant. Als u echter op schaal wilt delen, raadpleegt u ' Galerie-installatie kopieën delen in azure-tenants ' met [Power shell](../articles/virtual-machines/windows/share-images-across-tenants.md) of [cli](../articles/virtual-machines/linux/share-images-across-tenants.md).


**V:** Hoe lang duurt het om installatie kopieën in de doel regio's te repliceren?

 A. De replicatie tijd van de installatie kopie is geheel afhankelijk van de grootte van de installatie kopie en het aantal regio's waarnaar deze wordt gerepliceerd. Als best practice, kunt u echter het beste de installatie kopie klein en de bron-en doel regio's sluiten voor optimale resultaten. U kunt de status van de replicatie controleren met de vlag-ReplicationStatus.


**V:** Wat is het verschil tussen bron gebied en doel regio?

 A. Bron regio is de regio waarin de versie van de installatie kopie wordt gemaakt en de doel regio's zijn de regio's waarin een kopie van de afbeeldings versie wordt opgeslagen. Voor elke installatie kopie versie kunt u slechts één bron regio hebben. Zorg er ook voor dat u de locatie van het bron gebied als een van de doel regio's doorgeeft wanneer u een installatie kopie versie maakt.  


**V:** Hoe kan ik de bron regio opgeven tijdens het maken van de installatie kopie versie?

 A. Tijdens het maken van een installatie kopie, kunt u de **-locatie-** tag in CLI en de tag **-Location** in Power shell gebruiken om de bron regio op te geven. Zorg ervoor dat de beheerde installatie kopie die u als basis installatie kopie gebruikt voor het maken van de installatie kopie versie, zich op dezelfde locatie bevindt als de locatie waar u de installatie kopie versie wilt maken. Zorg er ook voor dat u de locatie van het bron gebied als een van de doel regio's doorgeeft wanneer u een installatie kopie versie maakt.  


**V:** Hoe kan ik geeft u het aantal afbeeldings versie replica's op dat in elke regio moet worden gemaakt?

 A. Er zijn twee manieren waarop u kunt opgeven hoeveel afbeeldings versie replica's er moeten worden gemaakt in elke regio:
 
1. Het regionale aantal replica's waarmee het aantal replica's wordt opgegeven dat u per regio wilt maken. 
2. Het aantal algemene replica's dat de standaard waarde per regio is, in het geval dat het aantal regionale replica's is opgegeven. 

Als u het aantal regionale replica's wilt opgeven, geeft u de locatie door, samen met het aantal replica's dat u in die regio wilt maken: "Zuid-Centraal VS = 2". 

Als er voor elke locatie geen regionaal aantal replica's is opgegeven, is het standaard aantal replica's het aantal gemeen schappelijke replica's dat u hebt opgegeven. 

Als u het aantal algemene replica's in cli wilt opgeven, gebruikt u het argument **--replica-Count** in de `az sig image-version create` opdracht.


**V:** Kan ik de galerie met gedeelde afbeeldingen maken op een andere locatie dan die waar ik de definitie van de installatie kopie en de installatie kopie versie wil maken?

 A. Ja, dat is mogelijk. Maar als best practice, raden we u aan de resource groep, de galerie met gedeelde installatie kopieën, de definitie van de installatie kopie en de installatie kopie versie op dezelfde locatie te laten staan.


**V:** Wat zijn de kosten voor het gebruik van de galerie met gedeelde afbeeldingen?

 A. Er worden geen kosten in rekening gebracht voor het gebruik van de service voor de gedeelde installatie kopie galerie, met uitzonde ring van de opslag kosten voor het opslaan van de installatie kopie versies en de kosten van het netwerk uitgaand voor het repliceren van de installatie kopieën van de bron regio

**V:** Welke API-versie moet ik gebruiken om een gedeelde installatie kopie galerie, afbeeldings definitie, afbeeldings versie en VM-VMSS uit de installatie kopie versie te maken?

 A. Voor VM-en virtuele-machine schaal sets die gebruikmaken van een installatie kopie versie raden we u aan om API-versie 2018-04-01 of hoger te gebruiken. Als u wilt werken met de gemeen schappelijke afbeeldings galerieën, afbeeldings definities en afbeeldings versies, raden we u aan API-versie 2018-06-01 te gebruiken. Voor zone redundant Storage (ZRS) is versie 2019-03-01 of hoger vereist.
