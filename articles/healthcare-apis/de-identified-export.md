---
title: Het exporteren van de niet-geïdentificeerde gegevens (preview) voor de Azure API voor FHIR
description: In dit artikel wordt beschreven hoe u de niet-geïdentificeerde export kunt instellen en gebruiken
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 9/28/2020
ms.author: matjazl
ms.openlocfilehash: bdbab0e032764d07119402686051d391376cb913
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843867"
---
# <a name="exporting-de-identified-data-preview"></a>Het exporteren van de niet-geïdentificeerde gegevens (preview-versie)

> [!Note] 
> De resultaten van het gebruik van de niet-geïdentificeerde export variëren afhankelijk van factoren zoals gegevens die zijn ontdubbeld en de functies die door de klant worden geselecteerd. Micro soft kan de niet-geïdentificeerde export uitvoer niet evalueren of de aanvaard baarheid voor de gebruiks cases en nalevings vereisten van de klant vaststellen. De niet-geïdentificeerde export is niet gegarandeerd om te voldoen aan specifieke juridische, wettelijke of nalevings vereisten.

De opdracht $export kan ook worden gebruikt voor het exporteren van de niet-geïdentificeerde gegevens van de FHIR-server. Er wordt gebruikgemaakt van de anoniem maken-engine van [FHIR-hulpprogram ma's voor anoniem maken](https://github.com/microsoft/FHIR-Tools-for-Anonymization)en er worden anoniem maken configuratie details gebruikt in query parameters. U kunt uw eigen anoniem maken-configuratie bestand maken of het [voorbeeld configuratie bestand](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) voor de HIPAA-methode Harbor gebruiken als uitgangs punt. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

|Query parameter            | Voorbeeld |Optionele| Beschrijving|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |DemoConfig.jsop|Vereist voor de niet-geïdentificeerde export |De naam van het configuratie bestand. Bekijk [hier](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format)de indeling van het configuratie bestand. Dit bestand moet worden bewaard in een container met de naam **anoniem maken** binnen hetzelfde Azure-opslag account dat is geconfigureerd als de export locatie. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Optioneel voor de niet-geïdentificeerde export|Dit is de ETAG van het configuratie bestand. U kunt de ETAG ophalen met behulp van Azure Storage Explorer vanuit de BLOB-eigenschap|

> [!IMPORTANT]
> Zowel onbewerkte als niet-geïdentificeerde export schrijf bewerkingen naar hetzelfde Azure Storage-account dat is opgegeven als onderdeel van de configuratie van de export. Het is raadzaam om verschillende containers te gebruiken die overeenkomen met de configuratie van de niet-geïdentificeerde configuraties en de gebruikers toegang te beheren op het niveau van de container.