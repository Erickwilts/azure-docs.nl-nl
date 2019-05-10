---
title: API-referentie - Content Moderator
titlesuffix: Azure Cognitive Services
description: Meer informatie over de verschillende inhoudstoezicht en bekijkt u API's voor Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
ms.date: 04/30/2019
ms.author: sajagtap
ms.openlocfilehash: 19144ae40e67127b656cedd61199b732b1c05e86
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236606"
---
# <a name="content-moderator-api-reference"></a>Referentie voor de Content Moderator-API

U aan de slag met Azure Content Moderator API's in de volgende manieren:

- In de Azure-portal [abonneren op de Content Moderator-API](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator).
- Zie [Content Moderator proberen op het web](quick-start.md) aan te melden met de [Content Moderator-controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/).

## <a name="moderation-apis"></a>Beheer-API's

U kunt de volgende Content Moderator API's gebruiken voor het instellen van uw werkstromen na toezicht.

| Description | Verwijzing |
| -------------------- |-------------|
| **Afbeeldingstoezicht-API**<br /><br />Scan afbeeldingen en detecteren van mogelijke erotische en ongepaste inhoud met behulp van tags, vertrouwen scores en andere opgehaalde gegevens. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud die u in uw werkstroom na toezicht. <br /><br />| [Afbeelding van toezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "installatiekopie Afbeeldingstoezicht-API-verwijzing")   |
| **Teksttoezicht-API**<br /><br />Tekstinhoud scannen. Grof taalgebruik voorwaarden en persoonlijke gegevens worden geretourneerd. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud die u in uw werkstroom na toezicht.<br /><br /> | [Tekst toezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "tekst Afbeeldingstoezicht-API-verwijzing")   |
| **Videotoezicht-API**<br /><br />Video's scannen en mogelijke erotische en ongepaste inhoud detecteren. <br /><br />Gebruik deze informatie te publiceren, weigeren of Controleer de inhoud die u in uw werkstroom na toezicht.<br /><br /> | [Video toezicht-API-overzicht](video-moderation-api.md "toezicht-API voor Video-overzicht")   |
| **Lijst met Management-API**<br /><br />Maken en beheren van aangepaste uitsluitings- of insluitingsfilters lijsten met tekst en afbeeldingen. Indien ingeschakeld, de **afbeelding: overeenkomen met** en **tekst - scherm** bewerkingen uitvoeren zoeken bij benadering die overeenkomt met de ingediende inhoud op basis van uw aangepaste lijsten. <br /><br />Voor efficiëntie, kunt u de machine learning gebaseerde beheer stap overslaan.<br /><br /> | [Lijst van Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "lijst Management API-verwijzing")   |

## <a name="review-apis"></a>Beoordelings-API's

De beoordeling-API heeft de volgende onderdelen:

| Description | Verwijzing |
| -------------------- |-------------|
| **Taken**<br /><br /> Start beheer van de scan en revisie werkstromen voor zowel afbeeldingen en tekst inhoud. Een beheer-taak scant uw inhoud met behulp van de afbeeldingen-API voor beheer en de tekst toezicht-API. Toezicht op taken met de gedefinieerde en werkstromen voor het genereren van beoordelingen standaard. <br /><br />Nadat een menselijke moderator heeft de labels automatisch toegewezen en voorspellingsgegevens beoordeeld en een beslissing inhoudstoezicht verzonden, verzendt de API controleren alle gegevens naar uw API-eindpunt.<br /><br /> | [Naslaginformatie over de taak](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "taak-verwijzing")   |
| **Beoordelingen**<br /><br />Gebruik het beoordelingsprogramma rechtstreeks afbeelding of tekst beoordelingen voor menselijke moderators maken.<br /><br /> Nadat een menselijke moderator heeft de labels automatisch toegewezen en voorspellingsgegevens beoordeeld en een beslissing inhoudstoezicht verzonden, verzendt de API controleren alle gegevens naar uw API-eindpunt.<br /><br /> | [Verwijzing bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "verwijzing bekijken")   |
| **Werkstromen**<br /><br />Maken, bijwerken en meer informatie over de aangepaste werkstromen die uw team maakt. U kunt werkstromen definiëren met behulp van het beoordelingsprogramma. <br /> <br />Werkstromen doorgaans Content Moderator gebruiken, maar kunnen ook bepaalde andere API's die beschikbaar als connectors in het beoordelingsprogramma zijn gebruiken.<br /><br /> | [Werkstroom verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "werkstroom-verwijzing")   |