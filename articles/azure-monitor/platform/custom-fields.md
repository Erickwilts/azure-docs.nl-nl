---
title: Aangepaste velden in Azure Monitor | Microsoft Docs
description: De functie aangepaste velden van Azure Monitor kunt u uw eigen doorzoekbare velden maken van records in een Log Analytics-werkruimte die aan de eigenschappen van een verzamelde record toevoegen.  In dit artikel beschrijft het proces voor het maken van een aangepast veld en bevat een gedetailleerd overzicht met een voorbeeld van de gebeurtenis.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: bwren
ms.openlocfilehash: 974a3391c592a1caf7bdcc6d9e01032f0c73aaa6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61461819"
---
# <a name="create-custom-fields-in-a-log-analytics-workspace-in-azure-monitor"></a>Maken van aangepaste velden in een Log Analytics-werkruimte in Azure Monitor

> [!NOTE]
> In dit artikel wordt beschreven hoe u tekstgegevens in een Log Analytics-werkruimte parseren als deze verzameld. Er zijn voordelen van het parseren van tekstgegevens in een query nadat deze verzameld, zoals beschreven in [parseren van tekstgegevens in Azure Monitor](../log-query/parse-text.md).

De **aangepaste velden** functie van Azure Monitor kunt u bestaande records in de werkruimte van Log Analytics uitbreiden door uw eigen doorzoekbare velden toe te voegen.  Aangepaste velden worden automatisch ingevuld op basis van gegevens die zijn geëxtraheerd uit andere eigenschappen in dezelfde record.

![Overzicht](media/custom-fields/overview.png)

Het onderstaande voorbeeldrecord heeft bijvoorbeeld nuttige gegevens ophalen uit de beschrijving van de gebeurtenis. Deze gegevens te pakken op een afzonderlijke eigenschap, maakt deze beschikbaar voor deze acties als sorteren en filteren.

![Voorbeeld ophalen](media/custom-fields/sample-extract.png)

> [!NOTE]
> In de Preview-versie bent u beperkt tot 100 aangepaste velden in uw werkruimte.  Deze limiet zal worden uitgebreid als deze functie algemeen beschikbaar wordt.

## <a name="creating-a-custom-field"></a>Het maken van een aangepast veld
Wanneer u een aangepast veld maakt, moet de Log Analytics inzicht in gegevens die u wilt gebruiken voor het vullen van de waarde ervan.  Een technologie van Microsoft Research FlashExtract met de naam wordt gebruikt deze gegevens snel te identificeren.  In plaats van dat u expliciete instructies te geven, wordt Azure Monitor leert over de gegevens die u wilt extraheren uit voorbeelden die u opgeeft.

De volgende secties vindt u de procedure voor het maken van een aangepast veld.  Aan de onderkant van dit artikel is een overzicht van de extractie van een voorbeeld.

> [!NOTE]
> Het aangepaste veld wordt ingevuld wanneer records die overeenkomen met de opgegeven criteria worden toegevoegd aan de Log Analytics-werkruimte, zodat deze alleen op de records die zijn verzameld weergegeven wordt nadat het aangepaste veld is gemaakt.  Het aangepaste veld wordt niet worden toegevoegd aan de records die zich al in het gegevensarchief wanneer deze wordt gemaakt.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Stap 1: records waarvan u het aangepaste veld identificeren
De eerste stap is het identificeren van de records die het aangepaste veld krijgt.  U kunt beginnen met een [standaardlogboek query](../log-query/log-query-overview.md) en selecteer vervolgens een record om te fungeren als het model dat Azure Monitor leert uit.  Wanneer u opgeeft dat u wilt ophalen van gegevens naar een aangepast veld, de **veld extractie Wizard** wordt geopend waarin u valideren en verfijnen van de criteria.

1. Ga naar **logboeken** en gebruik een [query voor het ophalen van de records](../log-query/log-query-overview.md) waarvoor het aangepaste veld.
2. Selecteer een record die Log Analytics wordt gebruikt om te fungeren als een model voor het extraheren van gegevens om in te vullen van het aangepaste veld.  U kunt de gegevens die u wilt extraheren uit deze record wordt geïdentificeerd en Log Analytics gebruikt deze informatie om te bepalen van de logica voor het vullen van het aangepaste veld voor alle vergelijkbare records.
3. Vouw de recordeigenschappen, klikt u op de ellips aan de linkerkant van de belangrijkste eigenschap van de record en selecteer **velden uit te pakken**.
4. De **veld extractie Wizard** wordt geopend, en de geselecteerde record wordt weergegeven in de **Hoofdvoorbeeld** kolom.  Het aangepaste veld worden gedefinieerd voor de records met dezelfde waarden in de eigenschappen die zijn geselecteerd.  
5. Als de selectie niet precies wat u wilt is, selecteert u extra velden om de criteria te beperken.  Als u wilt wijzigen de waarden voor de criteria, moet u annuleren en selecteer een andere record die voldoen aan de criteria die u wilt.

### <a name="step-2---perform-initial-extract"></a>Stap 2: initiële extract uitvoeren.
Nadat u de records die u het aangepaste veld hebt geïdentificeerd, kunt u de gegevens die u wilt uitpakken identificeren.  Log Analytics wordt deze informatie gebruiken om vergelijkbare patronen in vergelijkbare records te identificeren.  In de stap hierna zich kunt u kunt valideren van de resultaten en geef meer details op voor Log Analytics gebruiken in de analyse.

1. Markeer de tekst in de voorbeeldrecord die u wilt vullen van het aangepaste veld.  U krijgt vervolgens met een dialoogvenster voor een naam en het gegevenstype van het veld en uit te voeren van de eerste extraheren.  De tekens  **\_CF** zullen automatisch worden toegevoegd.
2. Klik op **extraheren** om uit te voeren van een analyse van verzamelde records.  
3. De **samenvatting** en **zoekresultaten** secties geven de resultaten van het extract, zodat u kunt de nauwkeurigheid inspecteren.  **Samenvatting** de criteria die wordt gebruikt voor het identificeren van records en een telling voor elk van de gegevenswaarden geïdentificeerd worden weergegeven.  **Zoekresultaten** biedt een gedetailleerde lijst met records die voldoen aan de criteria.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Stap 3: Controleer of de nauwkeurigheid van het uitpakken en aangepast veld maken
Nadat u het eerste extract hebt uitgevoerd, worden de resultaten op basis van gegevens die al zijn verzameld door Log Analytics weergegeven.  Als de resultaten er nauwkeurige kunt u het aangepaste veld maken met niet meer werken.  Als dat niet het geval is, klikt u vervolgens kunt u de resultaten verfijnen zodat de logica door Log Analytics verbeteren kunt.

1. Als alle waarden in het eerste extract niet juist zijn, klikt u op de **bewerken** pictogram naast een onjuiste opnemen en selecteer **deze markering wijzigen** om te wijzigen van de selectie.
2. De vermelding wordt gekopieerd naar de **extra voorbeelden** sectie onder de **Hoofdvoorbeeld**.  U kunt hier de markering aanpassen zodat Log Analytics begrijpen van de selectie die moet hebt opgegeven.
3. Klik op **extraheren** naar deze nieuwe gegevens gebruiken voor het evalueren van de bestaande records.  De resultaten kunnen worden gewijzigd voor records dan de regio die u zojuist hebt gewijzigd op basis van deze nieuwe bedrijfsinformatie.
4. Doorgaan met het toevoegen van correcties totdat alle records in het extract correct de gegevens om in te vullen van het nieuwe aangepaste veld kan worden geïdentificeerd.
5. Klik op **opslaan extraheren** wanneer u tevreden bent met de resultaten.  Het aangepaste veld is nu gedefinieerd, maar dit zal niet worden toegevoegd aan een record nog.
6. Wachten op nieuwe records die overeenkomen met de opgegeven criteria om te worden verzameld en voer de zoeken in Logboeken opnieuw uit. Nieuwe records moeten het aangepaste veld hebben.
7. Gebruik het aangepaste veld, zoals een andere record eigenschap.  U kunt gebruiken om te aggregeren en groep-gegevens en zelfs gebruiken voor het produceren van nieuwe inzichten.

## <a name="viewing-custom-fields"></a>Aangepaste velden weergeven
U kunt een lijst van alle aangepaste velden weergeven in uw beheergroep vanuit de **geavanceerde instellingen** menu van uw Log Analytics-werkruimte in de Azure-portal.  Selecteer **gegevens** en vervolgens **aangepaste velden** voor een lijst van alle aangepaste velden in uw werkruimte.  

![Aangepaste velden](media/custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Een aangepast veld verwijderen
Er zijn twee manieren om te verwijderen van een aangepast veld.  De eerste is de **verwijderen** optie voor elk veld bij het weergeven van de volledige lijst, zoals hierboven beschreven.  De andere methode is het ophalen van een record en klik op de knop aan de linkerkant van het veld.  Het menu heeft een optie voor het verwijderen van het aangepaste veld.

## <a name="sample-walkthrough"></a>Walkthrough voor voorbeeld
De volgende sectie leert u hoe via een compleet voorbeeld van het maken van een aangepast veld.  In dit voorbeeld haalt de naam van de service in Windows-gebeurtenissen die duiden op een service status wordt gewijzigd.  Dit is gebaseerd op gebeurtenissen die door Service Control Manager zijn gemaakt tijdens het opstarten van het systeem op Windows-computers.  Als u dit voorbeeld volgen wilt, moet u [verzamelen van gebeurtenissen voor het systeemlogboek](data-sources-windows-events.md).

Vullen we de volgende query uit om het ophalen van alle gebeurtenissen van Service Control Manager waarvoor een gebeurtenis-ID van 7036 die de gebeurtenis die aangeeft van een service starten of stoppen.

![Query’s uitvoeren](media/custom-fields/query.png)

We selecteren en een record met gebeurtenis-ID 7036 uitbreiden.

![Bronrecord](media/custom-fields/source-record.png)

Aangepaste velden definiëren we door te klikken op de ellips naast de belangrijkste eigenschap.

![Velden ophalen](media/custom-fields/extract-fields.png)

De **veld extractie Wizard** wordt geopend, en de **EventLog** en **EventID** velden zijn geselecteerd in de **Hoofdvoorbeeld** kolom.  Hiermee wordt aangegeven dat het aangepaste veld voor gebeurtenissen van het logboek met een gebeurtenis-ID van 7036 worden gedefinieerd.  Dit is voldoende hoeven dus niet om alle andere velden te selecteren.

![Belangrijkste voorbeeld](media/custom-fields/main-example.png)

Selecteren we de naam van de service in de **RenderedDescription** eigenschap en gebruik **Service** om de servicenaam te identificeren.  Het aangepaste veld wordt aangeroepen **Service_CF**. Het veldtype is in dit geval een tekenreeks, dus we die niet worden gewijzigd laten.

![Veld Title](media/custom-fields/field-title.png)

We zien dat de servicenaam correct wordt geïdentificeerd voor sommige records, maar niet voor andere gebruikers.   De **zoekresultaten** weergeven die deel uitmaken van de naam van de **WMI-prestatieadapter** niet is geselecteerd.  De **samenvatting** ziet u dat één record geïdentificeerd **Modules installatieprogramma** in plaats van **installatieprogramma voor Windows-Modules**.  

![Zoekresultaten](media/custom-fields/search-results-01.png)

We beginnen met de **WMI-prestatieadapter** record.  We klikt u op het bewerkingspictogram en vervolgens **deze markering wijzigen**.  

![Markering wijzigen](media/custom-fields/modify-highlight.png)

We verbeteren de markering om op te nemen het woord **WMI** en voer de extraheren.  

![Nog een voorbeeld](media/custom-fields/additional-example-01.png)

Zien we dat de vermeldingen voor **WMI-prestatieadapter** hebt gecorrigeerd, en die informatie in Log Analytics ook worden gebruikt om op te lossen de records voor **installatieprogramma voor Windows-Module**.

![Zoekresultaten](media/custom-fields/search-results-02.png)

We kunnen nu een query uitvoeren die controleert of **Service_CF** is gemaakt, maar nog niet is toegevoegd aan alle records. Dat komt doordat het aangepaste veld niet werkt op basis van bestaande records, dus moeten we wachten op nieuwe records moeten worden verzameld.

![Eerste aantal](media/custom-fields/initial-count.png)

Na enige tijd is verstreken, zodat nieuwe gebeurtenissen worden verzameld, zien we dat de **Service_CF** veld nu wordt toegevoegd aan de records die voldoen aan onze criteria.

![De laatste resultaten](media/custom-fields/final-results.png)

We kunnen nu het aangepaste veld, zoals een andere record eigenschap gebruiken.  Ter illustratie, maken we een query die door de nieuwe groepen **Service_CF** veld om te controleren welke services het actiefst zijn.

![Groeperen op query](media/custom-fields/query-group.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) query's met behulp van aangepaste velden voor criteria bouwen.
* Monitor [aangepaste logboekbestanden](data-sources-custom-logs.md) die u met behulp van aangepaste velden parseren.

