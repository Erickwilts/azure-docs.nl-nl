---
title: De Azure AD-Graph API gebruiken
description: De Azure Active Directory (Azure AD) Graph API biedt programmatische toegang tot Azure AD via OData REST API-eind punten. Toepassingen kunnen Azure AD Graph API gebruiken om bewerkingen voor maken, lezen, bijwerken en verwijderen uit te voeren op Directory gegevens en-objecten.
services: active-directory
documentationcenter: n/a
author: rwike77
manager: CelesteDG
editor: ''
tags: ''
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: sureshja
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85c3a1953ce34ab6bf60111715d9d8972a4682ba
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853390"
---
# <a name="how-to-use-the-azure-ad-graph-api"></a>Procedure: De Azure AD Graph API gebruiken

De Azure Active Directory (Azure AD) Graph API biedt programmatische toegang tot Azure AD via OData REST API-eind punten. Toepassingen kunnen Azure AD Graph API gebruiken om bewerkingen voor maken, lezen, bijwerken en verwijderen uit te voeren op Directory gegevens en-objecten. U kunt bijvoorbeeld Azure AD Graph API gebruiken om een nieuwe gebruiker te maken, de eigenschappen van de gebruiker weer te geven of bij te werken, het wacht woord van de gebruiker te wijzigen, het groepslid maatschap te controleren op op rollen gebaseerde toegang, de gebruiker uit te scha kelen of te verwijderen. Zie [Azure AD-Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) en [azure AD-Graph API vereisten](https://msdn.microsoft.com/library/hh974476.aspx)voor meer informatie over Azure AD Graph API-functies en-toepassings scenario's.

Dit artikel is van toepassing op Azure AD-Graph API. Zie [de Microsoft Graph-API gebruiken](https://developer.microsoft.com/graph/docs/concepts/use_the_api)voor vergelijk bare informatie met betrekking tot Microsoft Graph-API.

> [!IMPORTANT]
> Wij raden u ten zeerste aan om [Microsoft Graph](https://developer.microsoft.com/graph) te gebruiken in plaats van Azure AD Graph API om toegang te krijgen tot Azure Active Directory-resources. We richten ons momenteel op ontwikkelingen in Microsoft Graph. Voor Azure AD Graph API zijn geen verdere verbeteringen gepland. Er is maar een beperkt aantal scenario's waarvoor Azure AD Graph API nog steeds geschikt is. Zie het blogbericht [Mogelijk Graph of Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in het Office-ontwikkelaarscentrum voor meer informatie.

## <a name="how-to-construct-a-graph-api-url"></a>Een Graph API URL maken

In Graph API kunt u gebruikmaken van Url's op basis van het open data-protocol (OData) om toegang te krijgen tot Directory gegevens en-objecten (met andere woorden, resources of entiteiten) waarop u ruwe bewerkingen wilt uitvoeren. De Url's die worden gebruikt in Graph API bestaan uit vier hoofd onderdelen: Service basis, Tenant-id, bronpad en query teken reeks `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`opties:. Bekijk het voor beeld van de volgende URL `https://graph.windows.net/contoso.com/groups?api-version=1.6`:.

* **Hoofdmap**van de service: In azure AD Graph API is de hoofdmap van de service https://graph.windows.net altijd.
* **Tenant-id**: Deze sectie kan een geverifieerde (geregistreerde) domein naam zijn, in het vorige voor beeld contoso.com. Het kan ook een Tenant object-ID of de alias ' myorganization ' of ' me ' zijn. Zie voor meer informatie het [adresseren van entiteiten en bewerkingen in azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview).
* **Bronpad**: In deze sectie van een URL wordt de bron geïdentificeerd waarmee wordt gecommuniceerd (gebruikers, groepen, een bepaalde gebruiker of een bepaalde groep, enz.) In het bovenstaande voor beeld is dit het hoogste niveau ' groepen ' om de resourceset op te lossen. U kunt ook een specifieke entiteit verpakken, bijvoorbeeld ' gebruikers/{objectId} ' of ' gebruikers/userPrincipalName '.
* **Query parameters**: Een vraag teken (?) scheidt de sectie bronpad van de sectie query parameters. De query parameter API-Version is vereist voor alle aanvragen in azure AD Graph API. Azure AD Graph API biedt ook ondersteuning voor de volgende OData-query opties: **$filter**, **$OrderBy**, **$expand**, **$Top**en **$Format**. De volgende query opties worden momenteel niet ondersteund: **$Count**, **$inlinecount**en **$Skip**. Zie [ondersteunde query's, filters en paginerings opties in azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)voor meer informatie.

## <a name="graph-api-versions"></a>Graph API versies

U geeft de versie voor een Graph API aanvraag op in de query parameter API-Version. Voor versie 1,5 en hoger gebruikt u een numerieke versie waarde. API-Version = 1.6. Voor eerdere versies gebruikt u een datum teken reeks die voldoet aan de notatie JJJJ-MM-DD; bijvoorbeeld: API-Version = 2013-11-08. Gebruik voor preview-functies de teken reeks ' bèta '; bijvoorbeeld: API-Version = Beta. Zie [versie beheer van Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning)voor meer informatie over de verschillen tussen Graph API versies.

## <a name="graph-api-metadata"></a>Meta gegevens Graph API

Als u het meta gegevensbestand van Azure AD Graph API wilt retour neren, voegt u het segment ' $metadata ' toe na de Tenant-id in de URL. de volgende URL retourneert bijvoorbeeld Meta `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`gegevens voor een demo bedrijf:. U kunt deze URL opgeven in de adres balk van een webbrowser om de meta gegevens te zien. In het meta gegevens document voor CSDL worden de entiteiten en complexe typen, hun eigenschappen en de functies en acties beschreven die worden weer gegeven door de door u aangevraagde versie van Graph API. Als u de para meter API-Version weglaat, worden de meta gegevens voor de meest recente versie geretourneerd.

## <a name="common-queries"></a>Algemene query's

In de [algemene query's van Azure ad Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) worden algemene query's vermeld die kunnen worden gebruikt met de Azure AD-grafiek, met inbegrip van query's die kunnen worden gebruikt voor toegang tot resources op het hoogste niveau in uw directory en query's voor het uitvoeren van bewerkingen in uw Directory.

`https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` Retourneert bijvoorbeeld bedrijfs gegevens voor Directory contoso.com.

Of `https://graph.windows.net/contoso.com/users?api-version=1.6` een lijst van alle gebruikers objecten in de map contoso.com.

## <a name="using-the-azure-ad-graph-explorer"></a>De Azure AD Graph Explorer gebruiken
U kunt de Azure AD Graph Explorer gebruiken voor de Azure AD-Graph API om de Directory gegevens op te vragen tijdens het bouwen van uw toepassing.

De volgende scherm afbeelding is de uitvoer die u kunt zien als u naar de Azure AD Graph Explorer wilt gaan, zich hebt aangemeld en `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` op ENTER om alle gebruikers in de map van de aangemelde gebruiker weer te geven:

![Voorbeeld uitvoer in azure AD Graph API Explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**De Azure AD Graph Explorer laden**: Als u het hulp programma wilt laden [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/), gaat u naar. Klik op **Aanmelden** en meld u aan met de referenties van uw Azure ad-account om de Azure AD Graph Explorer uit te voeren op uw Tenant. Als u Azure AD Graph Explorer uitvoert voor uw eigen Tenant, moet u of uw beheerder toestemming geven tijdens het aanmelden. Als u een Office 365-abonnement hebt, hebt u automatisch een Azure AD-Tenant. De referenties die u gebruikt om u aan te melden bij Office 365 zijn, in feite Azure AD-accounts, en u kunt deze referenties gebruiken met de Graph Explorer van Azure AD.

**Een query uitvoeren**: Als u een query wilt uitvoeren, typt u uw query in het tekstvak aanvraag en klikt u op **Get** of klikt u op de **Enter** -toets. De resultaten worden weer gegeven in het vak reactie. Een voor beeld `https://graph.windows.net/myorganization/groups?api-version=1.6` : een lijst met alle groeps objecten in de map van de aangemelde gebruiker.

Houd rekening met de volgende functies en beperkingen van de Azure AD Graph Explorer:

* De functie automatisch aanvullen voor resource sets. Als u deze functionaliteit wilt zien, klikt u op het tekstvak aanvraag (waarbij de bedrijfs-URL wordt weer gegeven). U kunt een resourceset selecteren in de vervolg keuzelijst.
* Aanvraag geschiedenis.
* Ondersteunt de aliassen ' me ' en ' myorganization '. U kunt bijvoorbeeld gebruiken `https://graph.windows.net/me?api-version=1.6` om het gebruikers object van de aangemelde gebruiker te retour neren of `https://graph.windows.net/myorganization/users?api-version=1.6` om alle gebruikers te retour neren in de map van de aangemelde gebruiker.
* Ondersteunt de volledige ruwe bewerkingen voor uw eigen directory `POST`met `GET` `PATCH` , en `DELETE`.
* Een sectie antwoord headers. Deze sectie kan worden gebruikt om problemen op te lossen die zich voordoen bij het uitvoeren van query's.
* Een JSON-viewer voor het antwoord met de mogelijkheden voor uitvouwen en samen vouwen.
* Geen ondersteuning voor het weer geven of uploaden van een miniatuur foto.

## <a name="using-fiddler-to-write-to-the-directory"></a>Fiddler gebruiken om naar de map te schrijven

Voor de doel einden van deze Snelstartgids kunt u de Fiddler-webdebugger gebruiken om het uitvoeren van schrijf bewerkingen voor uw Azure AD-Directory te oefenen. U kunt bijvoorbeeld de profiel foto van een gebruiker ophalen en uploaden (dit is niet mogelijk met Azure AD Graph Explorer). Zie [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)voor meer informatie en voor het installeren van Fiddler.

In het onderstaande voor beeld gebruikt u Fiddler Web Debugger om een nieuwe beveiligings groep ' MyTestGroup ' te maken in uw Azure AD-adres lijst.

**Een toegangs token verkrijgen**: Om toegang te krijgen tot Azure AD Graph, moeten clients eerst worden geverifieerd bij Azure AD. Zie [verificatie scenario's voor Azure AD](authentication-scenarios.md)voor meer informatie.

**Een query opstellen en uitvoeren**: Voltooi de volgende stappen:

1. Open Fiddler Web debugger en schakel over naar het tabblad Composer.
2. Omdat u een nieuwe beveiligings groep wilt maken, selecteert u in de vervolg keuzelijst de optie **bericht** als HTTP-methode. Zie voor meer informatie over bewerkingen en machtigingen voor een groeps object [groeperen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#group-entity) in de [Naslag informatie over Azure AD Graph rest API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Typ in het veld naast **post**de volgende aanvraag-URL: `https://graph.windows.net/{mytenantdomain}/groups?api-version=1.6`.
   
   > [!NOTE]
   > U moet {mytenantdomain} vervangen door de domein naam van uw eigen Azure AD-adres lijst.

4. Typ in het veld direct onder bericht betrekken de volgende HTTP-header:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Vervang uw toegangs token&gt; door het toegangs token voor uw Azure AD-adres lijst. &lt;

5. Typ in het veld **hoofd tekst van aanvraag** de volgende JSON:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Zie [groep maken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup)voor meer informatie over het maken van groepen.

Zie overzicht van [Azure AD Graph rest API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)voor meer informatie over Azure AD-entiteiten en-typen die beschikbaar worden gesteld door Graph en informatie over de bewerkingen die kunnen worden uitgevoerd met Graph.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [Azure AD-Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Meer informatie over het [Graph API-machtigings bereik van Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
