---
title: Gebruikersprofiel en -ID voor gebruik met Azure-laptops
description: Over het maken en beheren van uw gebruikersprofiel en gebruikers-ID met Azure-Notebooks.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 7d069d86-660f-4c94-b6e3-0c0f38c52d0e
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: b8c21b908ca9162a7e44c7af1e222babc6ee1eb7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60631952"
---
# <a name="your-profile-and-user-id-for-azure-notebooks"></a>Uw profiel en gebruikers-ID voor Azure-laptops

In de ruimte die krachtige, samenwerking van Azure-notitieblokken wordt uw gebruikersprofiel anderen uw openbare installatiekopie:

[![De profielpagina van een Azure-laptops](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Uw gebruikers-ID is onderdeel van de URL's die u gebruiken voor het delen van projecten en notitieblokken. De volgende lijst beschrijft de verschillende URL-patronen:

- `https://notebooks.azure.com/<user_id>`: Uw profielpagina.
- `https://notebooks.azure.com/<user_id>/projects`: Uw projecten. U ziet alle projecten. andere gebruikers zien alleen uw openbare projecten.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Project-bestanden.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Klonen van een specifieke projecten.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: De HTML-Preview-versie van een specifieke laptop of bestand.

## <a name="your-user-id"></a>Uw gebruikers-ID

Wanneer voor de eerste keer aanmeldt bij Azure-laptops, wordt een tijdelijke gebruikers-ID, zoals "anon-idr3ca" automatisch toegewezen door uw account. Als u een gebruikers-ID die begint hebt met "anon-', Azure-notitieblokken vraagt u om dit te wijzigen wanneer u zich aanmeldt:

![Prompt voor het maken van een gebruikers-ID bij het aanmelden bij Azure-laptops](media/accounts/create-user-id.png)

Een **gebruikers-ID configureren** opdracht wordt ook weergegeven naast de naam van de tijdelijke gebruiker:

![Gebruikers-ID-opdracht die wordt weergegeven wanneer u een tijdelijke-ID configureren](media/accounts/configure-user-id-command.png)

U kunt ook uw gebruikers-ID op elk gewenst moment wijzigen op uw profielpagina.

Een gebruikers-ID moet bestaan uit tussen de vier en 16 letters, cijfers en afbreekstreepjes bevatten. Er zijn geen andere tekens zijn toegestaan en de gebruikers-ID mag niet beginnen of eindigen met een afbreekstreepje of gebruikmaken van meerdere opeenvolgende afbreekstreepjes in een rij. Omdat gebruikers-id's uniek zijn voor alle Azure-notitieblokken-accounts, ziet u het bericht, "Gebruikers-ID wordt al gebruikt." (Het bericht ook wordt weergegeven als u probeert te gebruiken een handelsmerken van Microsoft als een gebruikers-ID.) In dergelijke gevallen, kies een andere gebruiker-ID.

> [!Important]
> Bijgewerkt met de ID wordt ongeldig alle URL's die u mogelijk hebt gedeeld met uw vorige ID. Terug naar de vorige-ID voor het valideren van de koppelingen kunt u uw ID. Het is echter mogelijk dat een andere gebruiker voor een niet-gebruikte claim in de tussentijd de ID.

## <a name="your-profile"></a>Uw profiel

Uw profiel bestaat uit het publiek zichtbare gegevens in de URL, `https://notebooks.azure.com/<user_id>`. Uw profielpagina ziet ook uw recent gebruikte projecten en projecten met een ster.

Als u wilt uw profiel bewerkt, gebruikt u de **profielgegevens bewerken** opdracht op uw profielpagina. De secties van het profiel zijn als volgt:

| Sectie | Inhoud |
| --- | --- |
| Profielfoto | Een afbeelding die wordt weergegeven op uw profielpagina. |
| Accountgegevens | Uw weergavenaam, gebruikers-ID en openbare e-mailaccount. Het e-mailaccount biedt andere gebruikers in een gemiddelde contact met u opnemen en kan afwijken van de [account](azure-notebooks-user-account.md) kunt u zich aanmelden bij Azure-notitieblokken zelf. |
| Profielgegevens | Uw locatie, bedrijf, functietitel, website en een korte beschrijving van uzelf. |
| Sociale-profielen | Uw GItHub, Twitter en Facebook-ID's, als u wilt delen. |
| Privacyinstellingen | Biedt twee opdrachten uit:<ul><li>**Mijn profiel exporteren**: gemaakt en gedownload een *.zip* -bestand met de informatie die Azure-notitieblokken in uw profiel opslaat, zoals uw foto's, profielgegevens en -Logboeken.</li><li>**Verwijderen van Mijn Account**: Hiermee wordt uw persoonlijke gegevens die zijn opgeslagen in Azure-notitieblokken permanent verwijderd.</li></ul> |
| Sitefuncties inschakelen | Kunt u aspecten van het gedrag van Azure-notitieblokken te bepalen:<ul><li>**Unified Frontend voor laptops**: kunt u sneller opstarten van de notebook en betere persistentie.</li><li>**Standaard worden uitgevoerd in Jjupyterlab**: Azure-notitieblokken biedt standaard een eenvoudige gebruikersinterface die geschikt is voor de meeste gebruikers. Jjupyterlab biedt een uitgebreidere maar meer gecompliceerde interface voor ervaren gebruikers.</li><li>**VNext Website**: Hiermee kunt u de gemoderniseerd web-indeling die wordt weergegeven in deze documentatie.</li></ul> |

## <a name="next-steps"></a>Volgende stappen  

> [!div class="nextstepaction"]
> [Zelfstudie: een run maken een Jupyter-notebook te doen, lineaire regressie](tutorial-create-run-jupyter-notebook.md)
