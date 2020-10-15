---
title: bestand opnemen
description: bestand opnemen
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 04/14/2020
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: 5c36a541c9380596884252efcba4a9c0ca2be33f
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92082182"
---
| Resource | Limiet |
| ---------------------------------------------------------------------- | -------------------------- |
| Maximum aantal schaaleenheden | 10 per regio<sup>1</sup> |
| Cachegrootte | 5 GiB per eenheid<sup>2</sup> |
| Gelijktijdige back-end verbindingen<sup>3</sup> per HTTP-instantie | 2\.048 per eenheid<sup>4</sup> |
| Maximumgrootte van antwoord in cache | 2 MiB |
| Maximale grootte van beleidsdocument | 256 KiB<sup>5</sup> |
| Maximum aantal aangepaste gateway-domeinen per service-exemplaar<sup>6</sup> | 20 |
| Maximum aantal CA-certificaten per service-exemplaar<sup>7</sup> | 10 |
| Maximum aantal service-exemplaren per abonnement<sup>8</sup> | 20 |
| Maximum aantal abonnementen per service-exemplaar<sup>8</sup> | 500 |
| Maximum aantal clientcertificaten per service-exemplaar<sup>8</sup> | 50 |
| Maximum aantal API's per service-exemplaar<sup>8</sup> | 50 |
| Maximum aantal API-bewerkingen per service-exemplaar<sup>8</sup> | 1000 |
| Maximale totale aanvraagduur<sup>8</sup> | 30 seconden |
| Maximumgrootte van de gebufferde nettolading<sup>8</sup> | 2 MiB |
| Maximumgrootte aanvraag-URL<sup>9</sup> | 4096 bytes |
| Maximum aantal zelf-hostende gateways<sup>10</sup> | 25 |

<sup>1</sup>schaallimieten zijn afhankelijk van de prijscategorie. Zie [Prijzen voor API Management](https://azure.microsoft.com/pricing/details/api-management/)voor meer informatie over de prijscategorieën en hun schaallimieten.<br/>
<sup>2</sup>Cachegrootte per eenheid is afhankelijk van de prijscategorie. Zie [Prijzen voor API Management](https://azure.microsoft.com/pricing/details/api-management/)voor de prijscategorieën en hun schaallimieten.<br/>
<sup>3</sup>Verbindingen worden gegroepeerd en opnieuw gebruikt, tenzij expliciet gesloten door de back-end.<br/>
<sup>4</sup>Deze limiet geldt per eenheid van de lagen Basic, Standard en Premium. De Developer-laag is beperkt tot 1.024. Deze limiet is niet van toepassing op de Consumption-laag.<br/>
<sup>5</sup>Deze limiet geldt voor de lagen Basic, Standard en Premium. In de Consumption-laag is de grootte van beleidsdocumenten beperkt tot 16 KiB.<br/>
<sup>6</sup>Meerdere aangepaste domeinen worden alleen ondersteund in de Developer- en Premium-laag.<br/>
<sup>7</sup>CA-certificaten worden niet ondersteund in de Consumption-laag.<br/>
<sup>8</sup>Deze limiet is alleen van toepassing op de Consumption-laag. Voor andere lagen zijn er geen limieten in deze categorieën.<br/>
<sup>9</sup>Alleen van toepassing op de Consumption-laag. Inclusief een zoekstring van maximaal 2048 lang.<br/>
<sup>10</sup>Zelf-hostende gateways worden alleen ondersteund in de Developer- en Premium-laag. De limiet geldt voor het aantal [zelf-hostende gatewayresources](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/gateway). Neem contact op met de [ondersteuning](https://azure.microsoft.com/support/options/)als u deze limiet wilt verhogen. Merk op dat het aantal knooppunten (of replica's) dat is gekoppeld aan een zelf-hostende gatewayresource onbeperkt is in de Premium-laag, en is beperkt is tot één knooppunt in de Developer-laag.
