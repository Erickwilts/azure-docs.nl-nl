---
title: Azure CDN-inhoud beperken per land/regio | Microsoft Docs
description: Leer hoe u toegang beperken per land/regio naar uw Azure CDN-inhoud met behulp van de functie voor geo-filteren.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: magattus
ms.openlocfilehash: 083d8f66a73471548c812e27325e1ec69ad5c45c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869589"
---
# <a name="restrict-azure-cdn-content-by-countryregion"></a>Azure CDN-inhoud beperken per land/regio

## <a name="overview"></a>Overzicht
Wanneer een gebruiker vraagt om de inhoud, standaard, wordt de content wordt geleverd, ongeacht de locatie van de gebruiker die de aanvraag. In sommige gevallen kunt u echter toegang tot uw inhoud beperken per land/regio. Met de *filterfunctie voor geografische* functie, kunt u regels maken op bepaalde paden op uw CDN-eindpunt wilt toestaan of blokkeren van inhoud in de geselecteerde landen/regio's.

> [!IMPORTANT]
> **Azure CDN Standard van Microsoft** profielen bieden geen ondersteuning voor geo-filteren op basis van een pad.
> 

## <a name="standard-profiles"></a>Standard-profielen
De procedures in deze sectie zijn bedoeld voor **Azure CDN Standard van Akamai** en **Azure CDN Standard van Verizon** alleen profielen. 

Voor **Azure CDN Premium van Verizon** profielen, moet u de **beheren** portal te activeren geografisch filteren. Zie voor meer informatie, [Azure CDN Premium van Verizon profielen](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Pad naar de map definiëren
Voor toegang tot de functie voor geo-filteren, selecteert u uw CDN-eindpunt in de portal en vervolgens **filterfunctie voor geografische** via instellingen in het menu links. 

![Standaard geo-filteren](./media/cdn-filtering/cdn-geo-filtering-standard.png)

Uit de **pad** , geeft u het relatieve pad naar de locatie waarop gebruikers wordt toegestaan of toegang geweigerd. 

U geografisch filteren kunt toepassen voor alle bestanden met een zone voor forward slash (/) of door op te geven mappaden de specifieke mappen te selecteren (bijvoorbeeld */afbeeldingen/* ). U kunt ook geografisch filteren in één bestand (bijvoorbeeld */pictures/city.png*). Meerdere regels zijn toegestaan; Nadat u een regel hebt ingevoerd, wordt de volgende regel geeft u op een lege rij weergegeven.

Bijvoorbeeld, zijn alle van de volgende directory pad filters geldig:   
*/*                                 
*/Photos/*      
*/Photos/Strasbourg/*      
*/Photos/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Het type actie definiëren

Uit de **actie** in de lijst met **toestaan** of **blok**: 

- **Toestaan dat**: Alleen gebruikers van de opgegeven landen/regio mogen toegang tot activa die worden aangevraagd bij de recursieve-pad.

- **Blok**: Gebruikers van de opgegeven landen/regio's aan de activa aangevraagd bij het pad recursieve toegang geweigerd. Als er geen andere land/regio-filteropties voor die locatie zijn geconfigureerd, klikt u vervolgens alle andere gebruikers mogen toegang.

Bijvoorbeeld, een filterfunctie voor geografische regel voor het blokkeren van het pad */foto's /Straatsburg/* filters van de volgende bestanden:     
*http:\//\<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http:\//\<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countriesregions"></a>De landen/regio's definiëren
Uit de **LANDCODES** , selecteert u de landen/regio's die u wilt blokkeren of toestaan voor het pad. 

Nadat u klaar bent met de landen/regio selecteren, selecteert u **opslaan** activeren van de nieuwe filterfunctie voor geografische regel. 

![Regels voor geografische filteren](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Resources opschonen
Als u wilt verwijderen van een regel, selecteert u deze in de lijst op de **filterfunctie voor geografische** pagina, en kies vervolgens **verwijderen**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Azure CDN Premium van Verizon-profielen
Voor **voor Azure CDN Premium van Verizon** profielen, de gebruikersinterface voor het maken van een geo-filterregel verschilt:

1. Selecteer in het menu bovenaan in uw Azure CDN-profiel **beheren**.

2. Selecteer in de portal Verizon **HTTP grote**en selecteer vervolgens **landen filteren**.

    ![Standaard geo-filteren](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Selecteer **land Filter toevoegen**.

    De **één stap:** pagina wordt weergegeven.

4. Het pad opgeven, selecteert u **blok** of **toevoegen**en selecteer vervolgens **volgende**.

    De **stap twee:** pagina wordt weergegeven. 

5. Selecteer een of meer landen/regio's in de lijst en selecteer vervolgens **voltooien** activeren van de regel. 
    
    De nieuwe regel wordt weergegeven in de tabel op de **landen filteren** pagina.

    ![Regels voor geografische filteren](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Resources opschonen
Selecteer in de regels van land/regio filteren tabel, het verwijderingspictogram naast een regel om deze te verwijderen of het pictogram voor bewerken om deze te wijzigen.

## <a name="considerations"></a>Overwegingen
* Wijzigingen in de configuratie van de filterfunctie voor geografische doen niet onmiddellijk van kracht:
   * Voor profielen van **Azure CDN Standard van Microsoft** is het doorgeven gewoonlijk binnen 10 minuten voltooid. 
   * Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
   * Profielen van **Azure CDN Standard van Verizon** en **Azure CDN Premium van Verizon** worden normaal gesproken binnen 10 minuten doorgegeven. 
 
* Deze functie biedt geen ondersteuning voor jokertekens bevatten (bijvoorbeeld: *).

* De configuratie van de filterfunctie voor geografische die zijn gekoppeld aan het relatieve pad is toegepast recursief naar het opgegeven pad.

* Slechts één regel kan worden toegepast op hetzelfde relatieve pad. U kunt meerdere filters voor land/de regio die naar hetzelfde relatieve pad verwijzen dat wil zeggen, niet maken. Omdat het land/regio filters zijn recursieve, kan een map echter meerdere filters van het land/regio hebben. Met andere woorden, kan dat een submap van een eerder geconfigureerde map een ander land/regio-filter worden toegewezen.

* De functie voor geo-filteren gebruikt landcodes voor het definiëren van de landen/regio waaruit een aanvraag wordt toegestaan of geblokkeerd voor een beveiligde map. Hoewel Akamai en Verizon profielen de meeste van de dezelfde landcodes ondersteunen, zijn er enkele verschillen. Zie voor meer informatie, [Azure CDN landcodes](/previous-versions/azure/mt761717(v=azure.100)). 

